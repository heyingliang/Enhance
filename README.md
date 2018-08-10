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
