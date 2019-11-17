<template>
	<div :id="divID" class="block" v-on:mousemove="posMove" :style="{'z-Index': z}">
		<img :src="picSrc" />
		<div :id="scaleID" class="scale"></div>
	</div>
</template>

<script>
  import elephant from '../assets/pic/object/elephant.jpg'
  import person from '../assets/pic/object/person.jpg'
  import horse from '../assets/pic/object/horse.jpg'
  import beach from '../assets/pic/object/beach.jpg'
	export default{
		props:{
			divID: String,
			picName: String,
			scaleID: String,
			z:Number,
			ID:Number
		},

    data(){
      var picSrc;
      console.log(this.$props.picName);
      if(this.$props.picName == 'elephant'){
        picSrc = elephant;
      }
      else if(this.$props.picName == 'person'){
        picSrc = person;
      }
      else if(this.$props.picName == 'horse'){
        picSrc = horse;
      }
      else if(this.$props.picName == 'beach'){
        picSrc = beach;
      }
      return{
        picSrc:picSrc,
      }
    },

    updated: function(){
      this.$nextTick(function(){
        console.log(this.$props.picName);
        if(this.$props.picName == 'elephant'){
          this.picSrc = elephant;
        }
        else if(this.$props.picName == 'person'){
          this.picSrc = person;
        }
        else if(this.$props.picName == 'horse'){
          this.picSrc = horse;
        }
        else if(this.$props.picName == 'beach'){
          this.picSrc = beach;
        }
      })
    },

		methods:{
			posMove:function(){
				var _getdivid = this.$props.divID;
				var _ID = this.$props.ID;
				var _scaleID = this.$props.scaleID;
				this.$emit("w",_getdivid,_ID, _scaleID);
			}
		}
	}
</script>

<style>
.block{
	width: 70px; height: 70px;
	position: absolute; cursor: pointer;
	-webkit-box-shadow: 0px 0px 0px 0px #C7C7C7;
	box-shadow: 0px 0px 0px 0px #C7C7C7;
}
.scale {
	width: 10px;
	height: 10px;
	overflow: hidden;
	cursor: se-resize;
	position: absolute;
	right: 0;
	bottom: 0;
	background-color: rgb(122, 191, 238);
}
</style>
