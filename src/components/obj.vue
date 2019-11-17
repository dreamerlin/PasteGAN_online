<template>
  <div :id="'div'+id">
     <div class="layout my-3 px-4 mx-4 justify-center">
      <div class="flex xs5 | sm5 md5| lg5">
        <h1>{{id}}</h1>

      </div>

      <div class="flex xs5 | sm5 md5| lg5">
        <select v-model="selected" style="font-size: 20px; text-align: center; position: relative; left: 0%;" v-on:change="toggle()">
          <Option value="0" hidden style="color: white;">--Choose--</option>
          <option v-for="item in ObjList" :value="item.index"  style="color: red; background: black; text-align: center; position: relative;">{{ item.label }}</option>
        </select>
      </div>

      <div class="flex xs5 | sm5 md5| lg5">
        <canvas :id="'cavs' + id" width="50px" height="50px" class="canvas"></canvas>
      </div>
    </div>

    <button :id="'btn'+id"  :style="{'width': '30px', 'height': '30px', 'position': 'absolute'}" v-show="selected!=0 && show"  v-on:click="Add_A()">
      <div class="v-btn__content" style="font-size: 20px;">+Add</div>
    </button>
  </div>
</template>

<script>
  import elephant from '../assets/pic/object/elephant.jpg'
  import person from '../assets/pic/object/person.jpg'
  import horse from '../assets/pic/object/horse.jpg'
  import beach from '../assets/pic/object/beach.jpg'
  export default{
    props:{
      id:{
        type:Number
      },
    },

    data() {
      var selected=0;
      var show=true;
      var ObjList=[
        {
        	index: 1,
            value: 'elephant',
            label: 'elephant'
        },
        {
        	index: 2,
            value: 'person',
            label: 'person'
        },
        {
        	index: 3,
            value: 'horse',
            label: 'horse'
        },
        {
        	index: 4,
        	value: 'beach',
        	label: 'beach'
        }
      ];
      return{
        ObjList:ObjList,
        show:true,
        selected:selected
      }
    },

    methods:{
      toggle:function(){
        this.show = true;
        var selected = 0;
        var pic_name;
        var val;
        selected = this.selected
        val = this.ObjList[selected-1]['value']
        if(val=='elephant')
        {
          pic_name = elephant
        }
        else if(val=='person')
        {
          pic_name = person
        }
        else if(val=='horse')
        {
          pic_name = horse
        }
        else if(val=='beach')
        {
          pic_name = beach
        }

        var cvs_id = 'cavs' + this.$props.id
        var cvs = document.getElementById(cvs_id)
        var imgObj = new Image()
        imgObj.src = pic_name
        imgObj.onload = function(){
          var ctx = cvs.getContext('2d');
          ctx.drawImage(this,0,0,50,50);
        }

        var Ind = this.$props.id;
        // var btn_id = 'btn' + this.$props.id;
        // document.getElementById(btn_id).style.display = "";
        this.$emit("p", selected, val, Ind)
      },

      Add_A:function(){
        this.show = false;
        // var btn_id = 'btn' + this.$props.id;
        // document.getElementById(btn_id).style.display = "none";
        this.$emit("add_obj_row");
      }
    }
  }
</script>

<style>
@import url("../assets/css/1.css");
</style>
