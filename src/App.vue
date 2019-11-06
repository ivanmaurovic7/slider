<template>
  <div id="app">
    <header>
        <h2>Slider</h2>
    </header>
    <div class="gridContainer">
        <div class="firstSliderRow">
            <img class="sliderItem" v-for="(slider, index) in sliderOne" :src="slider" :key="index" :style="{order: index+1}"/>
        </div>
        <div class="secondSliderRow">
            <img class="sliderItem" v-for="(slider, index) in sliderTwo" :src="slider" :key="index" :style="{order: index+1}"/>
        </div>
        <div class="navigation">
            <img src="./assets/arrow-blue-left.png" id="previous" @click="previous()">
            <img src="./assets/arrow-blue-right.png" id="next" @click="next()">
        </div>
        <div class="lorem">
            <h2>Lorem ipsum dolor sit amet consectetur adipisicing elit.</h2>
            <p>Lorem ipsum, dolor sit amet consectetur adipisicing elit. Corrupti, optio magnam. Aliquid repellendus sed est perferendis, in hic unde architecto fuga. A ad sint repellat vero qui, iure tempora. Delectus ratione quod impedit nulla, suscipit beatae nihil facilis temporibus dolorem?</p>
        </div>
    </div>
    <footer>
        2019
    </footer>
  </div>
</template>

<script>
import $ from "jquery";

export default {
  data() {
    return {
      clickedPrevious: false,
      clickedNext: false,
      sliderOne: [require("./assets/slider-image-1.jpg"), require("./assets/slider-image-2.jpg"), require("./assets/slider-image-3.jpg"), require("./assets/slider-image-4.jpg"), require("./assets/slider-image-5.jpg")],
      sliderTwo: [require("./assets/slider-image-6.jpg"), require("./assets/slider-image-7.jpg"), require("./assets/slider-image-8.jpg"), require("./assets/slider-image-9.jpg")],
    }
  },

  methods: {
    previous() {
      if(!this.clickedPrevious && !this.clickedNext) {
        // Prevent spam clicking
        this.clickedPrevious = true
        this.sliderInProgress(true)

        $('.firstSliderRow > .sliderItem[style="order: 1;"]').hide(1200)
        $('.secondSliderRow > .sliderItem[style="order: 1;"]').hide(1200)
        
        setTimeout(() => {

            // Make first slider row slide to previous
            this.sliderAction('.firstSliderRow', 'previous')

            // Make second slider row slide to previous
            this.sliderAction('.secondSliderRow', 'previous')

            this.clickedPrevious = false
            this.sliderInProgress(false)
        }, 1200)
      }
    },

    next() {
      if(!this.clickedPrevious && !this.clickedNext) {
        // Prevent spam clicking
        this.clickedNext = true
        this.sliderInProgress(true)

        $(`.firstSliderRow > .sliderItem[style="order: ${$('.firstSliderRow').children().length};"]`).hide(1200)
        $(`.secondSliderRow > .sliderItem[style="order: ${$('.secondSliderRow').children().length};"]`).hide(1200)

        setTimeout(() => {
          // Make first slider row slide to next
          this.sliderAction('.firstSliderRow', 'next')

          // Make second slider row slide to next
          this.sliderAction('.secondSliderRow', 'next')

          this.clickedNext = false
          this.sliderInProgress(false)
        }, 1200)
      }
    },

    sliderAction(sliderRow, action) {
      $(sliderRow + ' > .sliderItem').each((index, el) => {
        if($(el).css('order') == $(sliderRow).children().length && action == 'next') {
            $(el).css('order', '1').show(500)
        }
        else if($(el).css('order') == 1 && action == 'previous') {
            $(el).css('order', $(sliderRow).children().length).show(500)
        } else {
            $(el).css('order', action == 'next' ? '+=1' : '-=1')
        }
      })
    },

    sliderInProgress(state) {
      if(state === true) {
        $('#next').attr('src', require('./assets/arrow-gray-right.png'))
        $('#next').css('border', '1px solid #dddddd')

        $('#previous').attr('src', require('./assets/arrow-gray-left.png'))
        $('#previous').css('border', '1px solid #dddddd')

        $('#previous').addClass('disabled')
        $('#next').addClass('disabled')
      } else {
        $('#next').attr('src', require('./assets/arrow-blue-right.png'))
        $('#next').css('border', '1px solid #134880')

        $('#previous').attr('src', require('./assets/arrow-blue-left.png'))
        $('#previous').css('border', '1px solid #134880')

        $('#previous').removeClass('disabled')
        $('#next').removeClass('disabled')
      }
    },
  }
}
</script>

<style lang="sass">

/* General styling */
*
  margin: 0
  padding: 0
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif
  color: #134880
  overflow-x: hidden

html
  background: #f2f2f2

header
  position: fixed
  width: 100vw
  height: 70px
  background: #fff
  box-shadow: 0 0 50px rgb(51, 51, 51, 0.25)
  h2
    padding: 18px 45px

footer
  background: #fff
  height: 70px
  width: 100vw
  font-weight: bold
  text-align: center
  padding: 25px
  box-sizing: border-box
  box-shadow: 0 0 50px rgb(51, 51, 51, 0.25)

.gridContainer
  margin: 100px 0 0 -10vw
  display: grid
  padding: 100px 0
  grid-template-columns: 1fr 1fr 1fr 1fr
  grid-template-areas: "s1 s1 s1 lorem" "s2 s2 nav lorem"

/* Slider navigation buttons */

.navigation
  grid-area: nav
  overflow: hidden
  img
    border-radius: 10px
    border: 1px solid #134880
    padding: 20px 25px
    display: block
    margin: 20px 60px 20px auto
    transition: transform .5s ease
    &:hover
      cursor: pointer
      transform: scale(1.1)

.disabled
  cursor: wait !important
  transform: scale(1) !important

/* Slider */

.firstSliderRow, .secondSliderRow
  display: flex

.firstSliderRow
  grid-area: s1
  width: 80vw

.secondSliderRow
  grid-area: s2
  width: 55vw

.sliderItem
  margin: 7px
  border-radius: 10px
  height: 30vh

/* Lorem area */

.lorem
  grid-area: lorem
  display: flex
  flex-direction: column
  justify-content: center
  padding: 50px

/* Media queries */

@media only screen and (max-width: 1400px) and (min-width: 600px)
  .lorem
    display: none

  .gridContainer
    grid-template-areas: "s1 s1 s1 s1" "s2 s2 s2 nav"

  .sliderItem
    height: 20vh

  .firstSliderRow, .secondSliderRow
    width: 100vw

@media only screen and (max-width: 600px)
  .lorem
    display: none

  .gridContainer
    grid-template-areas: "s1 s1 s1 s1" "s2 s2 s2 s2" "nav nav nav nav"

  .sliderItem
    height: 100px

  .firstSliderRow, .secondSliderRow
    width: auto
</style>
