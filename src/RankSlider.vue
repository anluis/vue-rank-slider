<template>
  <div
    class="sld"
    @touchstart="handleTouchStart"
    @touchmove="handleTouchMove"
    @touchend="handleTouchEnd"
    @touchcancel="handleTouchEnd"
    @click="handleClick"
  >
    <div class="line">
      <div
        class="circle-o"
        :style="{
          backgroundColor: value > index + 1 ? color : defaultColor,
          flex: index === max - 1 ? 0 : 1
        }"
        v-for="(item, index) in max"
        :key="index"
      >
        <div
          :class="{
            'circle-i colored': value > index + 1,
            'circle-i': value <= index + 1
          }"
          class="circle-i"
          :style="{ backgroundColor: value > index + 1 ? color : defaultColor }"
        />
        <div
          class="circle-big"
          :style="{ backgroundColor: color }"
          v-show="value > index * step && value <= (index + 1) * step"
        >
          <span>{{ index + 1 }}</span>
        </div>
      </div>
    </div>
    <div class="number">
      <div
        :style="{
          backgroundColor: index !== max ? '' : color,
          flex: index !== max - 1 ? 1 : 0
        }"
        v-for="(item, index) in max"
        :key="index"
      >
        <div class="number-i">
          <span
            v-show="index % 2 === 0"
            :style="{ opacity: index / max + 0.1 }"
          >
            {{ index + 1 }}
          </span>
        </div>
      </div>
    </div>
    <div class="words" v-if="minMeaning !== '' && maxMeaning !== ''">
      <div class="left">
        {{ minMeaning }}
      </div>
      <div class="right">
        {{ maxMeaning }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "RankSlider",
  props: {
    disabled: {
      type: Boolean,
      required: false,
      default: false
    },
    max: {
      type: Number,
      required: false,
      default: 10
    },
    min: {
      type: Number,
      required: false,
      default: 1
    },
    step: {
      type: Number,
      required: false,
      default: 1
    },
    propValue: {
      type: Number,
      required: true,
      default: 1
    },
    minMeaning: {
      type: String,
      required: false,
      default: ""
    },
    maxMeaning: {
      type: String,
      required: false,
      default: ""
    },
    defaultColor: {
      type: String,
      required: false,
      default: "rgba(240, 240, 240, 1)"
    },
    color: {
      type: String,
      required: false,
      default: "rgba(253, 205, 0, 1)"
    }
  },
  data() {
    return {
      startX: 0,
      startY: 0,
      startValue: 1,
      value: 1,
      newValue: 0
    };
  },
  watch: {
    propValue: function() {
      this.value = this.propValue ? this.propValue : 1;
    }
  },
  mounted() {
    this.value = this.propValue ? this.propValue : 1;
  },
  computed: {
    range: function() {
      return this.max - this.min;
    }
  },
  methods: {
    handleTouchStart(event) {
      if (this.disabled) {
        return;
      }
      this.startX = event.touches[0].clientX;
      this.startValue = this.format(this.value);
    },
    handleTouchMove(event) {
      if (this.disabled) {
        return;
      }
      const rect = this.$el.getBoundingClientRect();
      const touch = event.touches[0];
      this.deltaX = touch.clientX - this.startX;
      this.offsetX = Math.abs(this.deltaX);
      const delta = this.deltaX;
      const total = rect.width;
      const diff = (delta / total) * this.range;
      this.newValue = this.startValue + diff;
      this.updateValue(this.newValue);
      this.$emit("onTouchMove", this.value);
    },
    handleTouchEnd() {
      if (this.disabled) {
        return;
      }
      this.updateValue(this.newValue);
      this.$emit("onTouchEnd", this.value);
    },
    format(value) {
      return (
        Math.round(Math.max(this.min, Math.min(value, this.max)) / this.step) *
        this.step
      );
    },
    updateValue(value) {
      this.value = this.format(value);
    },
    handleClick(event) {
      event.stopPropagation();
      if (this.disabled) {
        return;
      }
      const rect = this.$el.getBoundingClientRect();
      const delta = event.clientX - rect.left;
      const total = rect.width;
      const value = (delta / total) * this.range + this.min;
      this.startValue = this.value;
      this.updateValue(value);
      this.$emit("onTouchEnd", this.value);
    }
  }
};
</script>

<style lang="less" scoped>
.sld {
  position: relative;
  box-sizing: border-box;
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding-top: 20px;
  padding-left: 18px;
  padding-right: 18px;
  .line {
    height: 4px;
    width: 100%;
    display: flex;
    .circle-o {
      position: relative;
      .circle-i {
        width: 10px;
        height: 10px;
        border-radius: 50%;
        position: absolute;
        left: 0;
        transform: translate(-50%, -25%);
      }
      .circle-big {
        width: 36px;
        height: 36px;
        position: absolute;
        left: 0;
        border-radius: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 18px;
        font-weight: 600;
        transform: translate(-50%, -50%);
        > span {
          padding-top: 2px;
        }
      }
    }
  }
  .number {
    width: 100%;
    display: flex;
    height: 40px;
    align-items: center;
    margin: 5px 0;
    display: flex;
    align-items: center;
    .number-o {
      position: relative;
      font-size: 14px;
      .number-i {
        position: absolute;
        left: 0;
        transform: translateX(-50%);
      }
    }
  }
  .words {
    display: flex;
    justify-content: space-between;
    font-size: 18px;
    font-weight: 400;
    line-height: 25px;
    width: 100%;
    margin-top: 20px;
    .left {
      opacity: 0.5;
      color: rgba(206, 206, 206, 1);
    }
    .right {
      color: rgba(0, 0, 0, 1);
    }
  }
}
</style>
