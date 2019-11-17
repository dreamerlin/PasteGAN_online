<template>
  <div>
    <div class="layout my-3 px-4 mx-4 justify-center">
      <div class="layout my-3 px-4 mx-4 justify-center">
        <select v-model="selected1" style="font-size: 20px; text-align: center; position: relative; left: -10%" v-on:change="toggle(1)">
          <Option value = "0" hidden style="color: white;">--Choose--</Option>
          <Option v-for="item in List1" :value="item.index" :key="item"
          style="color: red; background: black; text-align: center; position: relative;">
            {{ item.index + '-' + item.label }}
          </Option>
        </select>
      </div>

      <div class="layout my-3 px-4 mx-4 justify-center">
        <select v-model="selected2" style="font-size: 20px; text-align: center; position: relative; left: 0%" v-on:change="toggle(2)">
          <Option value = "0" hidden style="color: white;">--Choose--</Option>
          <Option v-for="item in List2" :value="item.index" :key="item"
          style="color: red; background: black; text-align: center; position: relative;">
            {{ item.label }}
          </Option>
        </select>
      </div>

      <div class="layout my-3 px-4 mx-4 justify-center">
        <select v-model="selected3" style="font-size: 20px; text-align: center; position: relative; left: 12%" v-on:change="toggle(3)">
          <Option value = "0" hidden style="color: white;">--Choose--</Option>
          <Option v-for="item in List3" :value="item.index" :key="item"
          style="color: red; background: black; text-align: center; position: relative;">
            {{ item.index + '-' + item.label }}
          </Option>
        </select>
      </div>

    </div>
    <button :style="{'width': '30px', 'height': '30px', 'position': 'absolute', 'left': '50%'}" v-show="selected1!=0 && selected2!=0 && selected3 !=0 && show" v-on:click="Add()">
      <div class="v-btn__content" style="font-size: 20px;"> +Add</div>
    </button>
  </div>
</template>

<script>
    export default {

		props:{
			Ind:{
				type: String
			},
      ObjList:{
        type: Object
      }
		},

        data: function() {
			var List;
			var selected1=0;
			var selected2=0;
			var selected3=0;
			var show = true;

      var subjectList = this.$parent.ObjList;
      var objectList = this.$parent.ObjList;
			var relationList = [
				{
					index: 1,
				    value: 'left of',
				    label: 'left of'
				},
				{
					index: 2,
				    value: 'right of',
				    label: 'right of'
				},
				{
					index: 3,
				    value: 'above',
				    label: 'above'
				},
				{
					index: 4,
					value: 'below',
					label: 'below'
				}
			];

      return {
        List1:subjectList,
				List2:relationList,
				List3:objectList,
				selected1:0,
				selected2:0,
				selected3:0,
				subject:"",
				relation:"",
				object:"",
				show: true,
				relationList:[]
            }
        },

    updated(){
      this.List1 = this.$parent.ObjList;
      this.List3 = this.$parent.ObjList;
    },

		methods:{
			toggle:function(index){
        this.show = true;
				var selected=0;
				var name;
				var val;

				if(index==1){
					selected = this.selected1;
					name = "subject";
					val = this.List1[selected-1]['index'];
					this.subject = val;
				}
				else if(index==2){
					selected = this.selected2;
					name = "relation";
					val = this.List2[selected-1]['value'];
					this.relation = val;
				}
				else if(index==3){
					selected = this.selected3;
					name = "object";
					val = this.List3[selected-1]['index'];
					this.object = val;
				}

				if((this.selected1!=0) && (this.selected2!=0) && (this.selected3!=0)){
					this.relationList = [this.subject, this.relation, this.object]
				}
				var relationList = this.relationList;
				var Ind = this.$props.Ind;
				this.$emit("p", selected, name, Ind, relationList)
			},
			Add:function(){
				this.show = false;
				this.$emit("add_row");
			},
		}
   }
</script>
<style>
select{
	width: auto;
	padding: 0 2%;
	margin: 0;
}

option{
	text-align:center;
}
</style>
