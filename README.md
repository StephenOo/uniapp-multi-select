#### 平台兼容性


| Vue2 | Vue3 |
| --- | --- |
| √ | × |

#### 介绍

基于uni-app开发的一个下拉多选组件，可以支持输入选择和关键字筛选。
整体的ui风格是参考的uni-ui，所以如果你要是使用了uni-ui，则使用这个组件会保证项目的ui风格一致。

#### props

| 参数 | 说明 | 类型 | 默认值 | 是否必须 |
| --- | --- | --- | --- | --- |
| options | 下拉框列表 | Array | [] | 是 |
| searchable | 是否支持搜索 | Boolean | false | 否 |
| placeholder | 输入框占位 | String | 请选择 | 否 |

#### options属性
| 参数 | 说明 | 类型 | 是否必须 |
| --- | --- | --- | --- |
| value | key值 | String/Number | 是 |
| label | 显示名换 | String | 是 |

#### events
| 参数 | 说明 | 类型 | 可选值 | 是否必须 |
| --- | --- | --- | --- | --- |
| change | 下拉选择事件 | Function | (val)=>{} | 是 |

#### 示例

```
<multi-select
  :options="options"
  :searchable="true"
  placeholder="请输入关键字"
  @change="selectChange"
/>
```
#### 数据格式

```
options: [
  { label: '语文', value: 0 },
  { label: '数学', value: 1 },
  { label: '英语', value: 2 },
  { label: '体育', value: 3 }
]
```






