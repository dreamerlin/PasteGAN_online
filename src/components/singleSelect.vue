<template>
<div>
  <select v-model="selected" style="font-size: 20px; text-align: center; position: relative;">
    <Option value = "0" hidden style="color: white;">--Choose--</Option>
    <Option v-for="item in List" :value="item.index" :key="item" style="color: red; background: black; text-align: center; position: relative;">
      {{ item.label }}
    </option>
  </select>
  <button :style="{'width': '30px', 'height': '30px', 'position': 'absolute', 'left': '50%'}" v-show="selected1!=0 && selected2!=0 && selected3 !=0 && show">
    <div class="v-btn__content" style="font-size: 20px;"> +Add</div>
  </button>
</div>
</template>

<script>
  export default{
    data:function() {
      var selected = 0;
      var show = true;
      var objectList = [
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
      return {
        objectList: objectList,
        show: true,
        selected: 0,
      }
    }
  }
</script>

<style>
</style>
