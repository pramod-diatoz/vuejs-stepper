<template>
    <div class="stepper-p">
        <div class="top-p">
            <div v-for="(item, index) in options.headers" :class="{
            'step-header-p': true, 'active': index <= currentPosition, 'error': item.error,
          'start': index === 0, 'end': index === options.headers.length}"  @click="getIndex(index)">
                <div class="header-indicator-p">
                    <div class="step-header-line-p" v-if="index > 0">

                    </div>
                    <div :class="['step-header-content-p', {'one': index == 0}, {'two': index == 1}, {'three': index == 2}, {'four': index == 3}, {'current': index == currentPosition}, {'disabled': (index == 0 || index == 1) && (currentPosition == 2 || currentPosition == 3)}]" style="position:relative" @click="slideTo(index)">
                        <!-- {{index+1}} -->
                    </div>
                </div>
                <div class="title-p" :class="{'title-p': true, 'current': index == currentPosition, 'disabled': (index == 0 || index == 1) && (currentPosition == 2 || currentPosition == 3)}">
                    {{item.title}}
                </div>
            </div>
        </div>

        <div style="width:100%">
            <transition-group :name="transitionType" class="body" mode="out-in" >
                <div v-for="(item, index) in options.headers" :key="'step' + index" v-show="currentPosition === index"
                     :class="{'steps-item-p':true}">
                    <slot :name="item.stepName ? item.stepName : 'step-' + (index+1)"></slot>
                </div>
            </transition-group>
        </div>
        <div class="foot">
            <button v-if="currentPosition > 0" class="prev-button" type="button" @click="prev()">{{options.prevText ? options.prevText : 'Prev' }}</button>
            <button v-if="currentPosition < options.headers.length - 1" class="next-button" type="button" @click="next()">{{options.nextText ? options.nextText : 'Next' }}</button>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'Stepper',
    props: ['options'],
    data () {
      return {
        currentPosition: 0,
        transitionType: 'slide'
      }
    },
    methods: {
      next () {
        if (this.currentPosition < this.options.headers.length - 1) {
          this.transitionType = 'stepper-slide-1'
          this.currentPosition++
        }
      },
      prev () {
        if (this.currentPosition > 0) {
          this.transitionType = 'stepper-slide-2'
          this.currentPosition--
        }
      },
      slideTo (index) {
        // if(this.currentPosition == index) return
        // if(this.currentPosition > index){
        //   this.transitionType = 'stepper-slide-2'
        // } else {
        //   this.transitionType = 'stepper-slide-1'
        // }
        // this.currentPosition = index;

        // Custom Code for the required stepper logic
        switch (this.currentPosition) {
          case 0:
            return;
            break;
          case 1:
            if(index == 0) {
              this.transitionType = 'stepper-slide-2';
              this.currentPosition = index;
            } else {
              return;
            }
            break;
          case 2:
            return;
            break;
          case 3:
            if(index == 2) {
              this.transitionType = 'stepper-slide-2';
              this.currentPosition = index;
            } else {
              return;
            }
            break;
          default:
            break;
        }
      },
      getIndex(index){
        this.$emit("getStepperCurrentIndex",index)
      }
    }
  }
</script>
<style src="./Stepper.css" scoped></style>
<style scoped>

</style>
