# Enhance

## Array

ES6中Array原型和Object对象上都新增了一个keys方法.作用于**键**/**属性**名上

API参考[@Array.prototype.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)
[@Object.keys](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)

结合两者的特(优)点，个人扩展/封装了一个循环迭代器loopkeys,作用于**值**上
```javascript
function loopkeys(){
    var arr = [];
    switch(Object.prototype.toString.call(arguments[0])){
        case '[object Number]':
            while(arguments[0]--){
                arr.unshift(arguments[0]);
            }
            break;
        case '[object Array]':
            arr = arguments[0];
            break;
        default:
            throw 'argument is not Array or Number or Object';
    }
    var i = 0;
    return {
        next : function(){
            i++;
            if(typeof arr[i] == 'undefined'){
                i = 0;
                return {value : arr[i], done : true};
            }
            return {value : arr[i], done : false};
        },
        prev : function(){
            i--;
            if(i == -1){
                i = arr.length - 1;
                return {value : arr[i], done : true};
            }
            return {value : arr[i], done : false};
        }
    };
}
```

### 示例

```javascript
// example 1
var loop1 = loopkeys(['a','b','c']);
console.log(loop1.target) // ['a','b','c']
console.log(loop.next().value) // 'b'
console.log(loop.next().value) // 'c'
console.log(loop.next().value) // 'a'
console.log(loop.prev().value) // 'c'

// example 2
var loop2 = loopkeys(3);
console.log(loop2.target) // [0,1,2]
```
## 临时代码

```javascript
var checkTime = function(i) { //将0-9的数字前面加上0，例1变为01 
   if (i < 10) {
       i = "0" + i;
   }
   return i;
};
function getLagtime(lagtime) {
     var lagtime = lagtime || 0; //剩余的毫秒数
     var days = parseInt(lagtime / 60 / 60 / 24, 10); //计算剩余的天数
     var hours = parseInt(lagtime / 60 / 60 % 24, 10); //计算剩余的小时
     var minutes = parseInt(lagtime / 60 % 60, 10); //计算剩余的分钟
     var seconds = parseInt(lagtime % 60, 10); //计算剩余的秒数

     days = days ? checkTime(days) + '天' : '';  
     hours = checkTime(hours) + '小时';
     minutes = checkTime(minutes) + '分';  
     seconds = checkTime(seconds) + '秒';
     return days + hours + minutes + seconds;
}
```
```javascript
function omitTel($tel){
    return preg_replace('/(\d{3})\d{4}(\d{4})/', '$1****$2', $tel);
}
```
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>vue.js练习示例</title>
	<script type="text/javascript" src="./assets/js/vue.js"></script>
	<style type="text/css">
	.tree{
        display: -webkit-box;
        display: -ms-flexbox;
        display: flex;
        overflow-wrap: unset;
        overflow-x: auto;
    }
    .tree:before,
    .tree:after
    {
        content: '';
        width: 20px;
    }
    .tree>.nodebox{
        flex: 1;
    }
    .node{
        position: relative;
    }
    .node .add-icon{
        position: absolute;
        bottom: -22px;
        left: 50%;
        margin-left: -10px;
        width: 20px;
        height: 20px;
        border: 1px solid #ccc;
        border-radius: 50%;
        line-height: 20px;
        text-align: center;
        cursor: pointer;
    }
    .node .text{
        display: inline-block;
        border: 1px solid #ccc;
        padding: 10px;
        font-size: 12px;
        cursor: pointer;
        outline: none;
    }
    .node .text.active{
        box-shadow: 0px 0px 6px 0 #f76f6f;
    }
    .node .to-line{
        position: absolute;
        width: 0;
        border-left: 1px solid #ccc;
        left: 50%;
        transform-origin: left bottom;
    }
    .nodebox{
        display: inline-block;
        vertical-align: top;
        margin: 30px 10px;
        text-align: center;
        white-space: nowrap;
    }
	</style>
</head>
<body>
	<div class="tree" id="tree">
		<tree-dom v-bind:tree="tree" v-bind:key="tree.id"></tree-dom>
	</div>
	<script type="text/javascript">
	var data = {
    	'id': 1,
    	'name': '节点1',
        'root': true,
    	'son': [
    		{
    			'id': 2,
    			'name': '节点1.1',
    			'son': []
    		},
    		{
    			'id': 3,
    			'name': '节点1.2',
    			'son': [
    				{
    					'id': 5,
    					'name': '节点1.2.1',
    					'son': []
    				},
    				{
    					'id': 6,
    					'name': '节点1.2.2',
    					'son': []
    				}
    			]
    		},
    		{
    			'id': 4,
    			'name': '节点1.3',
    			'son': []
    		}
    	]
    }
    Vue.component('tree-dom',{
        props: ['tree','parent','index','pid'],
        template: `
            <div class="nodebox">
                <div class="node">
                    <span class="text" ref='node' v-bind:class="{active: isSelect}" v-bind:data-parent="pid" v-bind:id="'node'+_uid" v-on:focus="isSelect=!isSelect" v-on:blur="isSelect=!isSelect" v-on:keyup.delete="deleteSelect(parent,index)" tabindex="-1">{{tree.name}}</span>
                    <span class="to-line" v-if="!tree.root" v-bind:style="lineStyle"></span>
                    <span class="add-icon" v-on:click="addChild(tree.son)">+</span>
                </div>
                <tree-dom v-for="(node,index) in tree.son" v-bind:index="index" v-bind:parent="tree.son" v-bind:tree="node" v-bind:key="node.id" v-bind:pid="'node'+_uid"></tree-dom>
            </div>
        `,
        data: function(){
            return{
                isSelect: false,
                lineStyle: {
                    transform: 'rotate(0deg)',
                    height: '0px',
                    top:'0px',
                },
            }
        },
        methods: {
            addChild: function(son){
                son.push({
                    'id': Math.random(),
                    'name': '节点添加',
                    'son': []
                });
            },
            deleteSelect: function(parent,index){
                console.log(parent);
                // this.isSelect = !this.isSelect;
                if (parent&&this.isSelect) {
                    parent.splice(index,1);
                }
                
            },
        },
    });
	var app = new Vue({
		el: '#tree',
		data: {
            tree: data
        }
	});
	// function getAngle(px,py,mx,my){//获得当前点和目标点坐标连线，与y轴正半轴之间的夹角
 //        var x = Math.abs(px-mx);
 //        var y = Math.abs(py-my);
 //        var z = Math.sqrt(Math.pow(x,2)+Math.pow(y,2));
 //        var cos = y/z;
 //        var radina = Math.acos(cos);//用反三角函数求弧度
 //        var angle = Math.floor(180/(Math.PI/radina));//将弧度转换成角度

 //        if(mx>px&&my>py){//目标点在第四象限
 //            angle = 180 - angle;
 //        }

 //        if(mx==px&&my>py){//目标点在y轴负方向上
 //            angle = 180;
 //        }

 //        if(mx>px&&my==py){//目标点在x轴正方向上
 //            angle = 90;
 //        }

 //        if(mx<px&&my>py){//目标点在第三象限
 //            angle = 180+angle;
 //        }

 //        if(mx<px&&my==py){//目标点在x轴负方向
 //            angle = 270;
 //        }

 //        if(mx<px&&my<py){//目标点在第二象限
 //            angle = 360 - angle;
 //        }
 //        return angle;
 //    }
	// window.onload = function(){
	// 	var test1 = document.getElementById('test1').getBoundingClientRect();
	// 	var test2 = document.getElementById('test2').getBoundingClientRect();
	// 	var y = Math.abs(test2.top - test1.top - test1.height);//临边
	// 	var x = Math.abs((test1.left + test1.width/2) - (test2.left + test2.width/2));//对边
	// 	var len = Math.sqrt(Math.pow(x,2)+Math.pow(y,2));//斜边
	// 	var line = document.querySelector('.to-line');
	// 	line.style.height = len+'px';
	// 	line.style.top = -len+'px';
	// 	line.style.transform = 'rotate('+getAngle(test2.left+test2.width/2, test2.top, test1.left+test1.width/2, test1.top+test1.height)+'deg)';
	// 	console.log(y,x,len);
	// 	console.log(getAngle(test2.top, test2.left+test2.width/2, test1.left+test1.width/2, test1.top+test1.height));
	// }
	</script>
</body>
</html>
```
```javascript
/**
   * 生成二维码
   */
  createCode:function(){
      var that = this;
      wx.request({
          url: 'https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=13_D-u9JpOK1pJYLVILfgx7SCsWNeyeav8ja0kxhuPy39HdxP77RgbNjll4jeZFjAWcVWvfoJUj9Ussr5xCj-Ou1MJ6wrcnFnBIOS4ixy7c5BEOX9YQEvH_Bht0d0_WaVa8CU9lD8wyensCv47LZCUcAIACFX',
          data: {
              scene: 'company=likjs',
              width: 360,
          },
          responseType: 'arraybuffer',
          method: 'POST',
          success: function(res){
              console.log(res);
              that.setData({
                  codeurl: wx.arrayBufferToBase64(res.data),
              });
          },
          error: function(res){
              console.log(res);
          }
      })
  }
```
```php
/*post请求外部接口--示例*/
public function apipostAction($url, $post_data = '') { //curl
	$url = 'https://api.weixin.qq.com/wxa/getwxacodeunlimit?access_token=13_D-u9JpOK1pJYLVILfgx7SCsWNeyeav8ja0kxhuPy39HdxP77RgbNjll4jeZFjAWcVWvfoJUj9Ussr5xCj-Ou1MJ6wrcnFnBIOS4ixy7c5BEOX9YQEvH_Bht0d0_WaVa8CU9lD8wyensCv47LZCUcAIACFX';
	$post_data = json_encode([
	    "scene"=>'company=likjs',
	    "width"=>360,
	],true);

	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	curl_setopt($ch, CURLOPT_POST, 1);
	if ($post_data != '') {
	    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
	}
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 1);
	curl_setopt($ch, CURLOPT_HEADER, false);
	$file_contents = curl_exec($ch);
	curl_close($ch);
	// var_dump($file_contents);
	echo 'data:image/png;base64,'.base64_encode($file_contents);
}
```
```html
<template>
    <table class="table">
        <thead>
            <th>用户名称</th>
            <th>账号</th>
            <th>绑定手机</th>
            <th>客户类型</th>
            <th>账户数</th>
            <th>优惠券数</th>
            <th>关联电站数</th>
        </thead>
        <tbody>
            <tr>
                <td>恒充管理-杨文狄</td>
                <td>1000127998</td>
                <td>13744809948</td>
                <td>3</td>
                <td>2</td>
                <td>1</td>
                <td>1</td>
            </tr>
        </tbody>
    </table>
</template>
<style type="text/css" lang="stylus">
.table
    width: 100%;
    text-align: left;
    font-size: 16px;
    color: #000;
    tr,thead
        height: 48px;
        line-height: 48px;
    thead
        background-color: #ededed;
        border-bottom: 2px solid #dbddde;
    tr:hover
        background-color: #eef4f7;
    th,td
        padding-left: 10px;
    th:not(:last-child)
        border-right: 1px solid #dbddde;
</style>
<script>
export default{
    name: 'table',
    data(){
        return{}
    }
}
</script>
```
```
function getchild(menu) {
    let arr = [];
    menu.forEach(el => {
        arr.push(el.id);
        if (el.childIds) {
            arr = arr.concat(getchild(el.childIds));
        }
    });
    return arr;
}
function buld(arr) {
    let menu = arr;
    for (let index = 0; index < menu.length; index++) {
        const el = menu[index];
        if (el.childIds) {
            menu[index].deepID = getchild(el.childIds);
            menu[index].childIds = buld(menu[index].childIds);
        }
    }
    return menu;
}

```
