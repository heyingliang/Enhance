# Enhance

## Array

ES6中新增了一个keys()方法，该方法返回新的Array迭代器，主要用于访问下一个元素.[@Array.prototype.keys()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/keys)

这个挺好玩的，个人扩展/封装了一个循环迭代器loopkeys
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
            throw 'argument is not Array or Number';
    }
    var i = 0;
    return {
        next : function(){
            i++;
            if(typeof arr[i] == 'undefined'){
                arr[0];
            }else{
                return arr[i];
            }
        },
        prev : function(){
            i--;
            if(i == -1){
                i = arr.length - 1;
            }
            return arr[i];
        },
        target : arr
    };
}
```

### 示例

```javascript
// example 1
var loop1 = loopkeys(['a','b','c']);
console.log(loop1.target) // ['a','b','c']
console.log(loop.next()) // 'b'
console.log(loop.next()) // 'c'
console.log(loop.next()) // 'a'
console.log(loop.prev()) // 'c'

// example 2
var loop2 = loopkeys(3);
console.log(loop2.target) // [0,1,2]
```
