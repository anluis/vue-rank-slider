# vue-rank-slider

# Install
```
yarn add vue-rank-slider
```

# Usage

```javascript

import RankSlider from 'vue-rank-slider'
<RankSlider
  :propValue="bindProp" //Number, value binded
  minMeaning="Min" //String, text for min, will hide if set empty
  maxMeaning="Max" //String, text for max, will hide if set empty
  @onTouchMove="handleSetSliderValue" //Event, return the value, you need to set it to your bind value
  @onTouchEnd="handleSetSliderValue" //Same as @onTouchMove
  :color="" //String, selected color
  :defaultColor="" //String, default color
  :max="" //Number, max value
  :min="" //Number, min value
  :step="" //Number
  :disabled="" //Boolean, disable for touch event
/>
```