# Calendar 日历

### 介绍

日历组件用于选择日期或日期区间，2.4 版本开始支持此组件

### 引入

```js
import Vue from 'vue';
import { Calendar } from 'vant';

Vue.use(Calendar);
```

## 代码演示

### 选择单个日期

下面演示了结合单元格来使用日历组件的用法，日期选择完成后会触发`confirm`事件

```html
<van-cell title="选择单个日期" :value="date" @click="show = true" />
<van-calendar v-model="show" @confirm="onConfirm" />
```

```js
export default {
  data() {
    return {
      date: '',
      show: false
    };
  },
  methods: {
    formatDate(date) {
      return `${date.getMonth() + 1}/${date.getDate()}`;
    },
    onConfirm(date) {
      this.show = false;
      this.date = this.formatDate(date);
    }
  }
};
```

### 选择多个日期

设置`type`为`multiple`后可以选择多个日期，此时`confirm`事件返回的 date 为数组结构，数组包含若干个选中的日期。

```html
<van-cell title="选择多个日期" :value="text" @click="show = true" />
<van-calendar v-model="show" type="multiple" @confirm="onConfirm" />
```

```js
export default {
  data() {
    return {
      text: '',
      show: false
    };
  },
  methods: {
    onConfirm(date) {
      this.show = false;
      this.text = `选择了 ${date.length} 个日期`;
    }
  }
};
```

### 选择日期区间

设置`type`为`range`后可以选择日期区间，此时`confirm`事件返回的 date 为数组结构，数组第一项为开始时间，第二项为结束时间。

```html
<van-cell title="选择日期区间" :value="date" @click="show = true" />
<van-calendar v-model="show" type="range" @confirm="onConfirm" />
```

```js
export default {
  data() {
    return {
      date: '',
      show: false
    };
  },
  methods: {
    formatDate(date) {
      return `${date.getMonth() + 1}/${date.getDate()}`;
    },
    onConfirm(date) {
      const [start, end] = date;
      this.show = false;
      this.date = `${this.formatDate(start)} - ${this.formatDate(end)}`;
    }
  }
};
```

### 快捷选择

将`show-confirm`设置为`false`可以隐藏确认按钮，这种情况下选择完成后会立即触发`confirm`事件

```html
<van-calendar v-model="show" :show-confirm="false" />
```

### 自定义颜色

通过`color`属性可以自定义日历的颜色，对选中日期和底部按钮生效

```html
<van-calendar v-model="show" color="#07c160" />
```

### 自定义日期范围

通过`min-date`和`max-date`定义日历的范围

```html
<van-calendar
  v-model="show"
  :min-date="minDate"
  :max-date="maxDate"
/>
```

```js
export default {
  data() {
    return {
      show: false,
      minDate: new Date(2010, 0, 1),
      maxDate: new Date(2010, 0, 31)
    };
  }
};
```

### 自定义按钮文字

通过`confirm-text`设置按钮文字，通过`confirm-disabled-text`设置按钮禁用时的文字

```html
<van-calendar
  v-model="show"
  type="range"
  confirm-text="完成"
  confirm-disabled-text="请选择结束时间"
/>
```

### 自定义日期文案

通过传入`formatter`函数来对日历上每一格的内容进行格式化

```html
<van-calendar
  v-model="show"
  type="range"
  :formatter="formatter"
/>
```

```js
export default {
  methods: {
    formatter(day) {
      const month = day.date.getMonth() + 1;
      const date = day.date.getDate();

      if (month === 5) {
        if (date === 1) {
          day.topInfo = '劳动节';
        } else if (date === 4) {
          day.topInfo = '五四青年节';
        } else if (date === 11) {
          day.text = '今天';
        }
      }

      if (day.type === 'start') {
        day.bottomInfo = '入住';
      } else if (day.type === 'end') {
        day.bottomInfo = '离店';
      }

      return day;
    }
  }
}
```

### 自定义弹出位置

通过`position`属性自定义弹出层的弹出位置，可选值为`top`、`left`、`right`

```html
<van-calendar
  v-model="show"
  :round="false"
  position="right"
/>
```

### 日期区间最大范围

选择日期区间时，可以通过`max-range`属性来指定最多可选天数，选择的范围超过最多可选天数时，会弹出相应的提示文案

```html
<van-calendar
  type="range"
  :max-range="3"
  :style="{ height: '500px' }"
/>
```

### 平铺展示

将`poppable`设置为`false`，日历会直接展示在页面内，而不是以弹层的形式出现

```html
<van-calendar
  title="日历"
  :poppable="false"
  :show-confirm="false"
  :style="{ height: '500px' }"
/>
```

## API

### Props

| 参数 | 说明 | 类型 | 默认值 |
|------|------|------|------|
| v-model | 是否显示日历弹窗 | *boolean* | `false` |
| type `v2.5.4` | 选择类型:<br>`single`表示选择单个日期，<br>`multiple`表示选择多个日期，<br>`range`表示选择日期区间 | *string* | `single` |
| title | 日历标题 | *string* | `日期选择` |
| color | 主题色，对底部按钮和选中日期生效 | *string* | `#ee0a24` |
| min-date | 可选择的最小日期 | *Date*  | 当前日期 |
| max-date | 可选择的最大日期 | *Date*  | 当前日期的六个月后 |
| default-date | 默认选中的日期，`type`为`multiple`或`range`时须传入数组 | *Date \| Date[]* | 今天 |
| row-height | 日期行高 | *number \| string* | `64` |
| formatter | 日期格式化函数 | *(day: Day) => Day* | - |
| position | 弹出位置，可选值为 `top` `right` `left` | *string* | `bottom` |
| poppable | 是否以弹层的形式展示日历 | *boolean* | `true` |
| round | 是否显示圆角弹窗 | *boolean* | `true` |
| show-mark | 是否显示月份背景水印 | *boolean* | `true` |
| show-confirm | 是否展示确认按钮 | *boolean* | `true` |
| close-on-popstate `v2.4.4` | 是否在页面回退时自动关闭 | *boolean* | `false` |
| close-on-click-overlay | 是否在点击遮罩层后关闭 | *boolean* | `true` |
| safe-area-inset-bottom | 是否开启[底部安全区适配](#/zh-CN/quickstart#di-bu-an-quan-qu-gua-pei) | *boolean* | `true` |
| confirm-text | 确认按钮的文字 | *string* | `确定` |
| confirm-disabled-text | 确认按钮处于禁用状态时的文字 | *string* | `确定` |
| max-range `v2.4.3` | 日期区间最多可选天数，默认无限制 | *number \| string* | - |
| range-prompt `v2.4.3` | 选择超过区间范围时的提示文案 | *string* | `选择天数不能超过 xx 天` |
| get-container `v2.4.4` | 指定挂载的节点，[用法示例](#/zh-CN/popup#zhi-ding-gua-zai-wei-zhi) | *string \| () => Element* | - |

### Day 数据结构

日历中的每个日期都对应一个 Day 对象，通过`formatter`属性可以自定义 Day 对象的内容

| 键名 | 说明 | 类型 |
|------|------|------|
| date | 日期对应的 Date 对象 | *Date* |
| type | 日期类型，可选值为`selected`、`start`、`middle`、`end`、`disabled` | *string* |
| text | 中间显示的文字 | *string* |
| topInfo | 上方的提示信息 | *string* |
| bottomInfo | 下方的提示信息 | *string* |
| className | 额外类名 | *string* |

### Events

| 事件名 | 说明 | 回调参数 |
|------|------|------|
| select | 点击任意日期时触发 | *value: Date \| Date[]* |
| confirm | 日期选择完成后触发，若`show-confirm`为`true`，则点击确认按钮后触发 | *value: Date \| Date[]* |
| open `v2.5.2` | 打开弹出层时触发 | - |
| close `v2.5.2` | 关闭弹出层时触发 | - |
| opened `v2.5.2` | 打开弹出层且动画结束后触发 | - |
| closed `v2.5.2` | 关闭弹出层且动画结束后触发 | - |

### Slots

| 名称 | 说明 |
|------|------|
| title | 自定义标题 |
| footer | 自定义底部区域内容 |

### 方法

通过 ref 可以获取到 Calendar 实例并调用实例方法，详见[组件实例方法](#/zh-CN/quickstart#zu-jian-shi-li-fang-fa)

| 方法名 | 说明 | 参数 | 返回值 |
|------|------|------|------|
| reset | 重置选中的日期到默认值 | - | - |

## 常见问题

### 在 iOS 系统上初始化组件失败？

如果你遇到了在 iOS 上无法渲染组件的问题，请确认在创建 Date 对象时没有使用`new Date('2020-01-01')`这样的写法，iOS 不支持以中划线分隔的日期格式，正确写法是`new Date('2020/01/01')`。

对此问题的详细解释：[stackoverflow](https://stackoverflow.com/questions/13363673/javascript-date-is-invalid-on-ios)。
