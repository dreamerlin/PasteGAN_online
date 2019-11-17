<template>
<div data-app="true" class="application theme--dark" id="app">
	<div class="application--wrap">
		<div>
			<div class="layout mt-4 ml-5 justify-start">
				<h1 style="margin-bottom: 8px; font-size: 32px; font-weight: bold; -webkit-text-fill-color: antiquewhite;">
				PasteGAN
				</h1>
			</div>
			<div class="layout mb-4 justify-center">
				<h2 style="font-size: 30px; font-weight: 400; margin-top: 8px; margin-bottom: -5px;">
					Drag to draw bounding boxes and assign labels or simply load a pre-defined layout.
				</h2>
			</div>
			<div class="layout">
				<div>
				</div>
			</div>

			<div class="layout my-3 px-4 mx-4 justify-center">

				<div class="flex xs5 | sm5 md5| lg5">
					<h3 style="font-size: 30px; margin-bottom: 18px; text-align: center; font-weight: 400;">
						Objects
					</h3>

          <div class="layout my-3 px-4 mx-4 justify-center">
            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	ID
              </h3>
            </div>

            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	Crop
              </h3>
            </div>


            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	Image
              </h3>
            </div>

           </div>

            <vObj ref="obj" :id="id" v-for="id in ID_item" v-on:p="add_obj" v-on:add_obj_row="Create_obj_row"></vObj>

				</div>

				<div class="flex xs5 | sm5 md5| lg5">
					<h3 style="font-size: 30px; margin-bottom: 18px; text-align: center; font-weight: 400;">
						Relation
					</h3>

          <div class="layout my-3 px-4 mx-4 justify-center">
            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	Subject
              </h3>
            </div>

            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	Relation
              </h3>
            </div>


            <div class="flex xs5 | sm5 md5| lg5">
              <h3 style="font-size: 25px; margin-bottom: 18px; text-align: center; font-weight: 400;">
              	Object
              </h3>
            </div>
           </div>

           <v-select ref="selc"  style="font-size: 20px; position: relative; color: white;" :id="item.selcID" :Ind="item.index" :objList="ObjList" v-on:p="Keep_relation" v-on:add_row="Create_relation_row" v-for="item in items"></v-select>
				</div>

				<div class="flex xs5 | sm5 md5 lg5">
					<h3 style="font-size: 30px ; margin-bottom: 18px; text-align: center; font-weight: 400;">
						Image
					</h3>

          <div id="parent">
            <v-crop v-for="(box_item, key) in box_items" :divID="box_item.boxID" :picName="box_item.picName" :ID="box_item.ID" :scaleID="box_item.scaleID" :z="box_item.z" v-on:w="posMove"></v-crop>
          </div>

					<div class="postText">
						&nbsp Move Diastance &nbsp &nbsp &nbsp Top：&nbsp<span id="posTop"></span>&nbsp Left: &nbsp<span id="posLeft"></span> &nbsp
					</div>
				</div>

				<div class="flex xs5 | sm5 md5 lg5">
					<h3 style="font-size: 30px ; margin-bottom: 18px; text-align: center; font-weight: 400;">
						Output
					</h3>

          <div id="output">

          </div>
          <h4> Picture </h4>
          
          <div id="sg">

          </div>
          <h4> Scene Graph</h4>
          
				<canvas id="canvas2" width="270px" height="270px"></canvas>
				</div>

			</div>

      <span>{{ serverResponse }}</span>
      <div class="layout pb-2 row justify-center" style="position: relative ;">

      	<button type="button" class="v-btn theme--dark info" style="width: 30px; height: 30px; position: relative; left: -3%;" @click="getData">
      		<div class="v-btn__content">
      			Generate
      		</div>
      	</button>
      	<button type="button" class="v-btn theme--light" style="width: 30px; height: 30px; position: relative; left: 3%;" @click="resetData">
      		<div class="v-btn__content">
      			Reset
      		</div>
      	</button>
      </div>

			<div class="layout row justify-center mt-4 wrap">
				<flex id="footnotes" style="font-size: 10px;">
					<a href="https://arxiv.org/abs/1905.01608" style="color: rgb(255, 255, 255);position: relative;">
						PasteGAN: A Semi-Parametric Method to Generate Image from Scene Graph<br>
					Yikang Li, Tao Ma, Yeqi Bai, Nan Duan, Sining Wei, Xiaogang Wang, NIPS 2019</a>
				</flex>
			</div>
			<div class="layout row justify-center wrap">
				<flex id="footnotes1" class="mt-4" style="font-size: 10px; position: relative ;">
					Web Application Developed by Tao Ma
				</flex>
			</div>
		</div>
	</div>
</div>
</template>

<script>
  import sSelect from './components/singleSelect.vue'
  import vId from './components/id.vue'
  import vSelect from './components/select.vue'
  import vCavs from './components/cavs.vue'
  import vCrop from './components/crop.vue'
  import vObj from './components/obj.vue'
  import axios from 'axios'

  export default{
    components:{
      vSelect,
      vCavs,
      vCrop,
      vId,
      axios,
      sSelect,
      vObj
    },
    data(){
      return {
        cur_idx:0,
        zValue:0,
        selcnt:1,
        objcnt:1,
        serverResponse:'',
        ID_item:[1],
        box_items:[],
        json_data: {
          "objects": [],
          "relationships": [],
          "crops": [],
          "boxes": []
        },
        idx_to_pic:{
        	1:"elephant.jpg",
        	2:"person.jpg",
        	3:"horse.jpg",
        	4:"beach.jpg"
        },
        idx_to_box:{},
        idx_to_obj:{},
        obj_to_idx:{},
        idx_to_relationship:{},
        items: [
          {
            "selcID": "selc1",
            'objID1': "c1",
            "objID2": "c2",
            "index": 1
          }
        ],
        ObjList:[],
      }
    },
    methods:{

      Keep_relation: function(data, name, ind, relationList){
        if(!(relationList.length === 0)){
          this.idx_to_relationship[ind] = relationList;
        }
      },

      posMove: function(getdivid, ID, scaleID){
        var _ID = ID;
        var oDiv = document.getElementById(getdivid);
        var oScale = document.getElementById(scaleID);
        var oParent = document.getElementById('parent');
        oDiv.style.zIndex = this.zValue;

        this.box_items[_ID-1]['z'] = this.zValue;
        this.zValue = this.zValue+1;
        var sent = {
        	l: 0,  //设置div在父元素的活动范围，10相当于给父div设置padding-left：10;
        	r: oParent.offsetWidth - oDiv.offsetWidth - 6,  // offsetWidth:当前对象的宽度， offsetWidth = width+padding+border
        	t: 0,
        	b: oParent.offsetHeight - oDiv.offsetHeight - 6,
        	n: 10
        }
        this.drag(oDiv, sent, _ID, oScale, oParent);
      },

      drag: function(obj, sent, ID, oScale, oParent){
      	var ID = ID;
      	var dmW = document.documentElement.clientWidth || document.body.clientWidth;
      	var dmH = document.documentElement.clientHeight || document.body.clientHeight;
      	var fW = document.getElementById("parent").offsetWidth
      	var fH = document.getElementById("parent").offsetHeight
      	// var w = document.getElementById("")
      	// var W = obj.offsetWidth
      	// var H = obj.offsetHeight

      	// console.log(dmW,obj.offsetWidth)
      	var sent = sent || {};
      	// console.log(sent);
      	var l = sent.l || 0;
      	var r = sent.r || dmW - obj.offsetWidth;
      	var t = sent.t || 0;
      	var b = sent.b || dmH - obj.offsetHeight;
      	var n = sent.n || 10;



      	obj.onmousedown = function (ev){
      		var oEvent = ev || event;
      		var sentX = oEvent.clientX - obj.offsetLeft;
      		var sentY = oEvent.clientY - obj.offsetTop;
      		var left = 0;
      		var top = 0;
      		var right = left + obj.offsetWidth;
      		var bottom = top + obj.offsetHeight;

      		document.onmousemove = function (ev){
      			var oEvent = ev || event;
      			var slideLeft = oEvent.clientX - sentX;
      			var slideTop = oEvent.clientY - sentY;


      			if(slideLeft <= l){
      				slideLeft = l;
      			}
      			if(slideLeft >= r){
      				slideLeft = r;
      			}
      			if(slideTop <= t){
      				slideTop = t;
      			}
      			if(slideTop >= b){
      				slideTop = b;
      			}

      			obj.style.left = slideLeft + 'px';
      			obj.style.top = slideTop + 'px';

      			left = slideLeft;
      			top = slideTop;
      			right = left + obj.offsetWidth;
      			bottom = top + obj.offsetHeight;

      			document.getElementById('posTop').innerHTML = slideTop;
      			document.getElementById('posLeft').innerHTML = slideLeft;
      		};
      		document.onmouseup = function (){
      			document.onmousemove = null;
      			document.onmouseup = null;
      		}
      		return false;
      	}

      	oScale.onmousedown = function(e){
      		e.stopPropagation();
      		e.preventDefault();
      		var pos = {
      			'w': obj.offsetWidth,
      			'h': obj.offsetHeight,
      			'x': e.clientX,
      			'y': e.clientY
      		};
      		document.onmousemove = function(ev){
      			ev.preventDefault();
      			var w = Math.max(30, ev.clientX - pos.x + pos.w)
      			var h = Math.max(30,ev.clientY - pos.y + pos.h)

      			w = w >= oParent.offsetWidth-obj.offsetLeft ? oParent.offsetWidth-obj.offsetLeft : w
      			h = h >= oParent.offsetHeight-obj.offsetTop ? oParent.offsetHeight-obj.offsetTop : h
      			obj.style.width = w + 'px';
      			obj.style.height = h + 'px';
      			obj.childNodes[0].style.width = w + 'px';
      			obj.childNodes[0].style.height = h + 'px';

      		}
      		document.onmouseleave = function(){
      			document.onmousemove = null;
      			document.onmouseup = null;
      		}
      		document.onmouseup = function(){
      			document.onmousemove = null;
      			document.onmouseup = null;
      		}
      	}

      	var left = 	parseInt(document.getElementById('posLeft').innerHTML);
      	var top = parseInt(document.getElementById('posTop').innerHTML);
        console.log(left);
        console.log(top);
      	if(left == null || top == null)
      	{
      		left=0;
      		top=0;
      	}
      	var w = obj.offsetWidth;
      	var h = obj.offsetHeight;
        console.log(w);
        console.log(h);
      	var right = left + w;
      	var bottom = top + h;
      	console.log(left, top, right, bottom)
      	left = left/fW;
      	top = top/fH;
      	right = right/fW;
      	bottom = bottom/fH;
      	var box = [left,top,right,bottom];

      	console.log(box)

      	for(var i=0; i<this.box_items.length; i++){
      		if(ID == this.box_items[i]["ID"])
      		{
      			this.box_items[i]["box"] = [left,top,right,bottom]
      			break
      		}
      	}
      	this.idx_to_box[ID] = box;
      },

      add_obj:function(selected_id, obj, canvas_id){
        var _selected_id = selected_id;
        var _obj = obj;
        var _canvas_id = canvas_id;
        console.log(this.ObjList.length);
        if(!(this.idx_to_obj).hasOwnProperty(_canvas_id))
        {
          this.idx_to_obj[_canvas_id] = _obj;
          this.obj_to_idx[_obj] = _canvas_id;
        }
        else
        {
          this.idx_to_obj[_canvas_id] = _obj;
          this.obj_to_idx[_obj] = _canvas_id;
        }

        if((this.ObjList.length)==0)
        {
          var Obj = {};
          Obj['index'] = _canvas_id;
          Obj['value'] = _obj;
          Obj['label'] = _obj;
          this.ObjList.push(Obj);
        }
        var flag = false;
        for(var i=0; i<(this.ObjList).length; i++)
        {
          if(_canvas_id == this.ObjList[i]['index'])
          {
            this.ObjList[i]['value'] = _obj;
            this.ObjList[i]['label'] = _obj;
            flag =true
            break;
          }
        }

        if(!flag)
        {
          var Obj = {};
          Obj['index'] = _canvas_id;
          Obj['value'] = _obj;
          Obj['label'] = _obj;
          this.ObjList.push(Obj);
        }


        var flag = false;
        for(var i=0; i<this.box_items.length; i++)
        {
          if(_canvas_id == this.box_items[i]['ID'])
          {
            this.box_items[i]['picName'] = _obj;
            this.box_items[i]['z'] = this.zValue;
            this.zValue = this.zValue + 1;
            flag = true;
            break;
          }
        }
        if(!flag)
        {
          this.box_items.push({
            'ID': _canvas_id,
            'boxID': 'box' + _canvas_id,
            'scaleID': 'scale' + _canvas_id,
            'picName': _obj,
            'box': [0,0,0.14,0.14],
            'z': this.zValue
          })
          this.zValue = this.zValue + 1;
          this.idx_to_box[_canvas_id] = [0,0,0.14,0.14]
        }
      },

      Create_obj_row:function(){
        this.objcnt = this.objcnt + 1;
        (this.ID_item).push(this.objcnt);
      },

      Create_relation_row:function(){
        var objID1 = "c" + this.selcnt*2+1;
        var objID2 = "c" + this.selcnt*2+2;
        this.selcnt = this.selcnt + 1;
        this.items.push({
          'selcID': "selc" + this.selcnt,
          'objID1': objID1,
          'objID2': objID2,
          "index": this.selcnt
        })
      },

      getjson(){
        var relationshipsList = []

        for(var relationItem in this.idx_to_relationship)
        {
          var relation = JSON.parse(JSON.stringify(this.idx_to_relationship[relationItem]));
          var sub = relation[0];
          console.log(JSON.stringify(this.obj_to_idx))
          console.log(JSON.stringify(sub))

          relationshipsList.push(relation)
        }
        this.json_data['relationships'] = relationshipsList;

        var objList = [];
        var cropList = [];
        var boxList = [];
        for(var objIdx in this.idx_to_obj)
        {
          var obj = this.idx_to_obj[objIdx];
          objList.push(obj);
          var objCrop = obj + ".png";
          cropList.push(objCrop);
        }
        for(var boxIdx in this.idx_to_box)
        {
          var box = this.idx_to_box[boxIdx];
          boxList.push(box);
        }
        this.json_data['objects'] = objList;
        this.json_data['crops'] = cropList;
        this.json_data['boxes'] = boxList;
      },

      getData:function(){
        var that = this;
        const path = 'http://127.0.0.1:5000/getMsg';
        var send_data = [this.json_data];
        this.getjson();
        console.log(JSON.stringify(this.json_data));
        axios.post(path,
                send_data
        )
        .then(function(response){
                var msg = response["data"];
                that.serverResponse = msg;
        }).catch(function(error){
                alert('Error ' + error);
        })
        // this.json_data = {};
        // this.obj_to_idx = {};
        // this.idx_to_obj = {};
        // this.cur_idx = 0;
      },

      resetData:function(){
        this.ID_item = [1],
        this.objcnt = 1,

        this.idx_to_obj = {},
        this.obj_to_idx = {},

        (this.$refs['obj'])[0].selected = 0;
        var cvs = document.getElementById('cavs1').getContext("2d");
        cvs.clearRect(0,0,50,50);

        this.ObjList = [];
        this.items = [
          {
            "selcID": "selc1",
            'objID1': "c1",
            "objID2": "c2",
            "index": 1
          }
        ];
        this.box_items = [];
        this.cnt=1;
        this.json_data = {
        	"objects": [],
        	"relationships": [],
        	"crops": [],
        	"boxes": []
        };
        this.obj_to_idx = {};
        this.idx_to_obj = {};
        this.idx_to_box = {};
        this.idx_to_relationship = {};
        this.cur_idx = 0;

        // this.$refs.selc1.selected1 = 0;
        // this.$refs.selc1.selected2 = 0;
        // this.$refs.selc1.selected3 = 0;
        // this.$refs.selc1.show = true;
        (this.$refs['selc'])[0].selected1 = 0;
        (this.$refs['selc'])[0].selected2 = 0;
        (this.$refs['selc'])[0].selected3 = 0;
        (this.$refs['selc'])[0].List1 = [];
        (this.$refs['selc'])[0].List3 = [];
      }
    },

    updated: function(){

    }
  }
</script>

<style>
@import url("assets/css/1.css");
@import url("assets/css/2.css");
@import url("assets/css/3.css");
@import url("assets/css/4.css");
@import url("assets/css/5.css");
@import url("assets/css/6.css");
</style>
