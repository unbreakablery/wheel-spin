<template>
  <div id="app">
    <WheelSpin
      :segments="options"
      :wheelSize="wheelSize"
      :pageTitle="pageTitle"
      :lineWidth="lineWidth"
      :btnColor="btnColor"
      :btnText="btnText"
      :spin="spin"
      :duration="duration"
      :textOrientation="textOrientation"
			:textAlignment="textAlignment"
			:textMargin="textMargin"
    />    
  </div>
</template>

<script>
import WheelSpin from './components/wheel-spin/WheelSpin.vue'

export default {
  name: 'App',
  components: {
    WheelSpin
  },
  data(){
    return{
      pageTitle       : "Wheel Spinner",
      wheelSize       : 640,
      btnColor        : "",
      btnText         : "SPIN!",
      lineWidth       : 8,
      duration        : 15,
      spin       	    : 15,
      textOrientation : 'vertical',
			textAlignment   : 'outer',
			textMargin      : 20,
      options         :[
					{'fillStyle' : '#ee1c24', 'text' : '0x', 'size': 10},
					{'fillStyle' : '#3cb878', 'text' : '0.3x', 'size': 11},
					{'fillStyle' : '#000000', 'text' : '5x', 'size': 1},
          {'fillStyle' : '#c1f234', 'text' : '0.5x', 'size': 13},
					{'fillStyle' : '#f6989d', 'text' : '3x', 'size': 3},
					{'fillStyle' : '#f26522', 'text' : '1x', 'size': 15},
					{'fillStyle' : '#e70697', 'text' : '1.5x', 'size': 10},
					{'fillStyle' : '#f9e200', 'text' : '2x', 'size': 5},
					{'fillStyle' : '#a186be', 'text' : '1.2x', 'textFontSize' : 16, 'textFillStyle' : '#ffffff', 'size': 15},
					{'fillStyle' : '#08ff03', 'text' : '4x', 'size': 2},
					{'fillStyle' : '#00aef0', 'text' : '0.7x', 'size': 15},
					// {'fillStyle' : '#f26522', 'text' : '800', 'size': 3},
					// {'fillStyle' : '#a186be', 'text' : '300', 'size': 10},
					// {'fillStyle' : '#fff200', 'text' : '400', 'size': 5},
					// {'fillStyle' : '#00aef0', 'text' : '650', 'size': 4},
					// {'fillStyle' : '#ee1c24', 'text' : '1000', 'size': 1},
					// {'fillStyle' : '#f6989d', 'text' : '500', 'size': 2},
					// {'fillStyle' : '#f26522', 'text' : '400', 'size': 3},
					// {'fillStyle' : '#3cb878', 'text' : '900', 'size': 2},
					// {'fillStyle' : '#000000', 'text' : 'BANKRUPT', 'textFontSize' : 16, 'textFillStyle' : '#ffffff', 'size': 3},
					// {'fillStyle' : '#a186be', 'text' : '600', 'size': 4},
					// {'fillStyle' : '#fff200', 'text' : '700', 'size': 3},
					// {'fillStyle' : '#00aef0', 'text' : '800', 'size': 2},
					// {'fillStyle' : '#ffffff', 'text' : 'LOOSE TURN', 'textFontSize' : 24, 'size': 3}
				]
    }
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
