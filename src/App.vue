<template>
  <div id="app">
    <RankSlider
      :propValue="bindProp"
      minMeaning="Min"
      maxMeaning="Max"
      @onTouchMove="handleSetSliderValue"
      @onTouchEnd="handleSetSliderValue"
    />
  </div>
</template>

<script>
import RankSlider from "./RankSlider";

export default {
  name: "App",
  components: {
    RankSlider
  },
  data() {
    return {
      bindProp: 0
    };
  },
  methods: {
    handleSetSliderValue(value) {
      this.bindProp = value;
    }
  }
};
</script>
