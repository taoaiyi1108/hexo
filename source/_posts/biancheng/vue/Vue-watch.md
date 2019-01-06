---
title: Vue-watch
date: 2018-12-27 16:20:10
tags: 
    - vue 
    - vue-watch
categories: 编程
---
Vue-watch的使用技巧和注意事项

<!-- more -->

```javascript
export default{
    data(){
        return {
            a:'',
            obj:{
                a:''
            }
        }
    },
    watch:{
        /*普通的watch监听*/
        'a':function(val, oldVal){
            console.log(val);
        },
        /*深度监听，可监听到对象、数组的变化*/
        'obj':{
            handler(val, oldVal){
                console.log("obj.a: "+val.a, oldVal.a);/*但是这两个值打印出来却都是一样的*/
            },
            deep:true
        }
        /*
        watch中不能使用箭头函数的方法，例如
        
        ‘a’:(val, oldVal) =>{
            console.log(this.a);
            此时就报错，因为此时的this已不再Vue了
        }
        这样写会报错
        */
    }
}
```