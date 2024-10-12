<template>
  <view>
    <view class="custom-select" @click.stop="toggleSelector">
      <view v-if="selected.length" class="selected-content">
        <view class="selected-item" v-for="item in selected" :key="item.value">
          {{ item.label }}
        </view>
      </view>
      <input v-if="searchable" class="select-input" v-model="keyword" :placeholder="placeholder" :clearable="false" style="border: none" @input="getOptions" />
      <image class="select-arrow" mode="widthFix" :src="showSelector ? arrowUpIcon : arrowDownIcon"></image>
      <view class="pop-arrow"></view>
      <scroll-view v-if="showSelector" scroll-y="true" class="select-options" style="top: calc(100% + 12px);">
        <view
          class="select-item"
          v-for="item in selectOptions"
          :key="item.value"
          :style="{opacity: item.disabled ? 0.6 : 1}"
          @click.stop="!item.disabled && change(item)"
        >
          <text class="item-name">{{ item.label }}</text>
          <image v-if="isSelected(item)" class="item-checked" mode="widthFix" :src="selectedIcon"></image>
        </view>
      </scroll-view>
    </view>
    <view v-if="showSelector" class="custom-select_mask" @click="toggleSelector" />
  </view>
</template>

<script>
const arrowDownIcon = require('../static/arrow-down.svg')
const arrowUpIcon = require('../static/arrow-up.svg')
const selectedIcon = require('../static/selected.svg')

export default {
  props: {
    options: {
      type: Array,
      default () {
        return []
      }
    },
    placeholder: {
      type: String,
      default: '请选择'
    },
    searchable: {
      type: Boolean,
      default: false
    }
  },
	data() {
		return {
      arrowDownIcon,
      arrowUpIcon,
      selectedIcon,
      showSelector: false, // 显示/隐藏 下拉框
      selected: [], // 选中值
      keyword: '',
      selectOptions: []
		}
	},
  watch: {
    options: {
      handler(newVal) {
        if (this.keyword) {
          this.selectOptions = newVal.filter(item => item.label.indexOf(this.keyword) > -1)
        } else {
          this.selectOptions = JSON.parse(JSON.stringify(newVal))
        }
      },
      immediate: true,
      deep: true
    }
  },
	methods: {
    // 显示/隐藏 下拉框
    toggleSelector() {
      this.showSelector = !this.showSelector
    },
    // 是否已被选中
    isSelected(option) {
      return this.selected.find(item => item.value === option.value) ? true : false
    },
    // 选中/取消选中事件
    change(option) {
      if (this.isSelected(option)) {
        // 已选中，取消选中
        this.selected = this.selected.filter(item => item.value !== option.value)
				this.$emit('change', this.selected)
      } else {
        // 未选中，选中
        this.selected.push(option)
        this.$emit('change', this.selected)
      }
    },
    // 下拉列表搜索
    getOptions() {
      this.$nextTick(() => {
        if (this.keyword) {
          this.selectOptions = this.options.filter(item => item.label.indexOf(this.keyword) > -1)
        } else {
          this.selectOptions = JSON.parse(JSON.stringify(this.options))
        }
      })
    }
  }
}
</script>

<style lang="scss">
.custom-select_mask {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  z-index: 2;
}
.custom-select {
  background-color: #FFFFFF;
  font-size: 14px;
  border: 1px solid #e5e5e5;
  box-sizing: border-box;
  border-radius: 4px;
  position: relative;
  display: flex;
  -webkit-user-select: none;
  user-select: none;
  flex-direction: row;
  align-items: center;
  border-bottom: solid 1px #e5e5e5;
  width: 100%;
  flex: 1;
  min-height: 35px;
  padding-right: 6px;
  display: flex;
  justify-content: space-between;
  .selected-content {
    padding: 0 5px;
    padding: 4px;
    max-width: 50%;
    display: flex;
    flex-wrap: wrap;
    .selected-item {
      margin-right: 4px;
      margin-bottom: 2px;
      line-height: 14px;
      font-size: 12px;
      padding: 4px 7px;
      color: #2979ff;
      border-radius: 3px;
      background-color: #fff;
      border-width: 1px;
      border-style: solid;
      border-color: #2979ff;
    }
  }
  .select-input  {
    border-width: 0px;
    border-color: transparent;
    height: 32px;
    font-size: 12px;
    padding-left: 8px;
  }
  .select-arrow {
    width: 14px;
  }
  .pop-arrow {
    width: 0;
    height: 0;
    border-color: transparent;
    border-style: solid;
    border-width: 6px;
    position: absolute;
    top: calc(100% + 7px);
    left: 10%;
    z-index: 4;
    border-top-width: 0;
    border-bottom-color: #FFFFFF;
  }
  .select-options {
    box-sizing: border-box;
    position: absolute;
    left: 0;
    width: 100%;
    background-color: #FFFFFF;
    border: 1px solid #EBEEF5;
    border-radius: 6px;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    z-index: 3;
    padding: 4px 0;
    height: 180px;
    .select-item {
      padding: 0 10px;
      height: 35px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      .item-checked {
        width: 14px;
      }
    }
  }
}
</style>
