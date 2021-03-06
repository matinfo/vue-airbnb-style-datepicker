# Installation
With NPM:

```
npm install vue-airbnb-style-datepicker --save
```

With Yarn:

```
yarn add vue-airbnb-style-datepicker
```

NB: This plugin is dependant on VueJS 2.x and [date-fns](https://date-fns.org/) (for date manipulation). Make sure you have these dependencies installed.

## [Enable plugin in your app](#enable-plugin)

First off, tell Vue to use the plugin in your main.js:

```javascript
import Vue from 'vue'
import App from './App.vue'

// import component and stylesheet
import AirbnbStyleDatepicker from 'vue-airbnb-style-datepicker'
import 'vue-airbnb-style-datepicker/dist/styles.css'

// see docs for available options
const datepickerOptions = {}

// make sure we can use it in our components
Vue.use(AirbnbStyleDatepicker, datepickerOptions)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

The `options` is optional. It is only needed if you want to overwrite default colors, texts etc. For example if your site uses another language than english.
**Note that `days` and `daysShort` always should start with Monday.** So if you want Sunday as the first day in the week, use `sundayFirst: true` (but days and daysShort should still start with Monday)

## [Use plugin](#use-plugin)
Add datepicker in your component like this:

```html
<template>
  <div>
    <div class="datepicker-trigger">
      <input
        type="text"
        id="datepicker-trigger"
        placeholder="Select dates"
        :value="formatDates(dateOne, dateTwo)"
      >

      <AirbnbStyleDatepicker
        :trigger-element-id="'datepicker-trigger'"
        :mode="'range'"
        :fullscreen-mobile="true"
        :date-one="dateOne"
        :date-two="dateTwo"
        @date-one-selected="val => { dateOne = val }"
        @date-two-selected="val => { dateTwo = val }"
      />
    </div>
  </div>
</template>

<script>
import format from 'date-fns/format'

export default {
  data() {
    return {
      dateFormat: 'D MMM',
      dateOne: '',
      dateTwo: ''
    }
  },
  methods: {
    formatDates(dateOne, dateTwo) {
      let formattedDates = ''
      if (dateOne) {
        formattedDates = format(dateOne, this.dateFormat)
      }
      if (dateTwo) {
        formattedDates += ' - ' + format(dateTwo, this.dateFormat)
      }
      return formattedDates
    }
  }
}
</script>
```

NB: Note that you need to wrap the datepicker in a `<div class="datepicker-trigger">`. This is used as the base for the positioning of the datepicker.
Also note that the id of element that triggers the datepicker needs to be the same as prop `:trigger-element`.
<br><br>
This plugin does not dictate how you show the dates. This allows for more flexibility since you can use whatever trigger element you want. The value is being emitted from the component when a date is selected, and handled in the `@date-one-selected` and `@date-two-selected` methods. Then you just assign the value to your data properties. And it is up to you to decide how you want to display the dates.<br>
The `formatDates()` methods is just an example of how it can be solved.

## [Options for `Vue.use(AirbnbStyleDatepicker, datepickerOptions)`](#plugin-options)
| Prop name | Value |
| ------------- | ------------- |
| sundayFirst | Do you want the week to start on sunday.<br>Type: Boolean, Default: false  |
| days | Name of days in your language. **Must start with monday**<br>Type: Array<string>  |
| daysShort | Short name of days in your language (what's shown in the days legend). **Must start with monday**<br>Type: Array<string>  |
| monthNames | Names of months in your language.<br>Type: Array<string>  |
| colors | Override default colors. Use hex values (#efefef)<br>Type: Object  |
| texts | Override default texts (currently only "Cancel" and "Apply")<br>Type: Object  |

*Example with all available options*:
```javascript
Vue.use(AirBnbStyleDatepicker, {
  sundayFirst: false,
  days: ['Måndag', 'Tisdag', 'Onsdag', 'Torsdag', 'Fredag', 'Lördag', 'Söndag'],
  daysShort: ['Mån', 'Tis', 'Ons', 'Tors', 'Fre', 'Lör', 'Sön'],
  monthNames: [
    'Januari',
    'Februari',
    'Mars',
    'April',
    'Maj',
    'Juni',
    'Juli',
    'Augusti',
    'September',
    'Oktober',
    'November',
    'December'
  ],
  colors: {
    selected: '#00a699',
    inRange: '#66e2da',
    selectedText: '#fff',
    text: '#565a5c',
    inRangeBorder: '#33dacd',
    disabled: '#fff'
  },
  texts: {
    apply: 'Tillämpa',
    cancel: 'Avbryt'
  }
})
```

## [Properties & events for `<AirbnbStyleDatepicker />`](#props-and-events)
| Prop name | Value |
| ------------- | ------------- |
| triggerElementId | The id of the element that user clicks on (without #).<br>Type: String, Required  |
| mode | If datepicker should select a range or just a single date.<br>Type: String, Required, Values: `'single'` or `'range'`, Default: `'range'`  |
| dateOne | Model for first date.<br>Type: String, Required |
| dateTwo | Model for second date.<br>Type: String, Required if using `mode="range"` |
| minDate | Disable dates before this.<br>Type: String |
| endDate | Disable dates after this.<br>Type: String |
| offsetY | Offset vertical position of datepicker (in pixels from `triggerElementId` bottom).<br>Type: Number, Default: 0 |
| offsetX | Offset horisontal position of datepicker (in pixels from `triggerElementId` left or right depending on alignment).<br>Type: Number, Default: 0 |
| monthsToShow | How many months to show. For mobile it's always 1.<br>Type: Number, Default: 2 |
| startOpen | If you want the datepicker start open<br>Type: Boolean, Default: false |
| fullscreenMobile | Show fullscreen view on mobile.<br>Type: Boolean, Default: false |
| mobileHeader | Text to show on mobile header<br>Type: String, Default: 'Select dates' |
| inline | Use inline mode (datepicker always showing)<br>Type: Boolean, Default: false |
| disabledDates | Disable specific dates.<br>Type: Array<string> |
| showActionButtons | Show/hide action buttons ("Apply", "Cancel")<br>Type: Boolean, Default: false |
| trigger | To programmatically show datepicker. For example if you want to open the datepicker by clicking some other HTML element. You manually need to reset this variable though in the @closed method.<br>Type: Boolean, Default: false |
| @date-one-selected | Event emitted when second date is selected.<br>Required |
| @date-two-selected | Event emitted when second date is selected.<br>Required if using `mode="range"` |
| @opened | Event emitted when datepicker is opened. |
| @closed | Event emitted when datepicker is closed. |
| @apply | Event emitted when user clicks "Apply" |
| @previous-month | Event emitted when user changes to previous month. Returns array with first date in visible months. `['2019-09-01', '2019-10-01']` |
| @next-month | Event emitted when user changes to next month. Returns array with first date in visible months. `['2019-09-01', '2019-10-01']` |

<br><br>
*Example with all properties (not recommended, only to show values)*:
```html
<AirbnbStyleDatepicker
  :trigger-element-id="'datepicker-trigger'"
  :mode="'range'"
  :date-one="dateOne"
  :date-two="dateTwo"
  :min-date="'2018-10-12'"
  :end-date="'2021-01-01'"
  :offset-x="10"
  :offset-y="30"
  :months-to-show="2"
  :start-open="true"
  :fullscreen-mobile="true"
  :mobile-header="'Mobile header text'"
  :inline="true"
  :disabled-dates="['2018-10-20', '2018-10-22']"
  :show-action-buttons="true"
  :trigger="someBooleanDataProp"
  @date-one-selected="val => { dateOne = val }"
  @date-two-selected="val => { dateTwo = val }"
  @opened="onOpenedMethod"
  @closed="onClosedMethod"
  @apply="onApplyMethod"
  @previous-month="onChangeMonthMethod"
  @next-month="onChangeMonthMethod"
/>
```
