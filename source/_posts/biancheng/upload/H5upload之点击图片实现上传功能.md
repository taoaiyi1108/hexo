---
title: H5upload之点击图片实现上传功能
date: 2018-11-23 09:11:42
tags: upload
categories: 编程
---
本节我们来实现很多UI框架上漂亮的上传功能

<!-- more -->

啥都不说先看设计图和需求流程
- 直接怼H5原生的样子
```html
    <input type="file" name="uploadFile" id="uploadFile" class="filepath"/>
```
<div algin=center>![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/upload/upload4.png)</div>

- 再看设计图和需求图
<div>![](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/upload/upload5.jpg)</div>

- 好了，我们现在开始完成，二话不说直接上代码
```html
<div class="upload-item">
    <input id="file1" type="file" class="upfile">
</div>

<script >
$('.upload-item').on("click",function(){
    var $this = $(this);
    var $input = $(this).children('input');
    $input.on("change" , function(){
        var file = $(this)[0].files[0],
            imgSrc = $(this)[0].value;
        console.log(imgSrc)
        if (!/\.(jpg|jpeg|png|JPG|PNG|JPEG)$/.test(imgSrc)){
            alert("请上传jpg或png格式的图片！");
            return false;
        }else{
            var file_reader = new FileReader();
            file_reader.onload = (function(){
                var image_url = this.result;        /*这是图片的路径*/
                $this.css('background-image', "url("+image_url+")");
    
            });
            file_reader.readAsDataURL(file);
        }
    })
})
</script>
```

- 思路：给div一个默认的背景图片，同时隐藏div中的input（透明度为0，宽高100%），点击div时会事件穿透，input也会被点击到，通过input的change事件，创建FileReader对象，完成图片的上传；在FileReader.onload方法的result就是被转化成base64的图片数据流，此时改变div的背景图片路径就是实现了预览的效果；但此时图片并没有被传到服务器上去；可以在提交事件时通过post把图片的数据流传到服务器上边去（不建议在FileReader.onload的回调中直接post）