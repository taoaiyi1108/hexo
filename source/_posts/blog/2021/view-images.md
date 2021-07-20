---
title: vue图片放大查看器
date: 2021-1-12 08:46
tags: 
    - view 
    - vue
categories: '2021'
---
借助v-view封装一个图片放大查看器，技术有限多多包涵

<!-- more -->

```html
    <!-- template -->
    <template>
        <div class="view_img">
            <div class="view_img_list" v-if="option.images && option.images.length">
                <div class="images" v-viewer="{movable: true}" v-show="option.smallImgShow">
                    <img v-for="(src,index) in option.images" :src="src.fileUrl" :key="index" :style="{'height':option.imgHeight}"/>
                </div>
                <div class="view_button" @click="show" v-show="option.buttonShow">
                    <slot name='btn'></slot>
                </div>
            </div>
            <div class="view_img_list" v-if="option.image">
                <div class="images" v-viewer="{movable: true}" v-show="option.smallImgShow" :style="{'height':option.imgHeight}">
                    <img :src="option.image" />
                </div>
                <div class="view_button" @click="show" v-show="option.buttonShow">
                    <slot name='btn'></slot>
                </div>
            </div>
        </div>
    </template>
```

```js
    /* script */
    import 'viewerjs/dist/viewer.css'
    import Viewer from 'v-viewer'
    import Vue from 'vue'
    Vue.use(Viewer, {
        defaultOptions: {
            zIndex: 9999
        }
    })
    export default {
        props: {
            options: {
                type: Object,
                default: () => {}
            }
        },
        computed: {
            option: {
                get() {
                    return Object.assign({
                        images: [], //多张图片
                        image: '', // 单个图片
                        smallImgShow: true, // 缩略图点击放大查看
                        buttonShow: true, // 按钮点击放大查看
                        imgHeight: 'auto'
                    },this.options)
                }
            }
        },
        methods: {
            show () {
                const vuer = this.$el.querySelector('.images').$viewer
                vuer.show()
            }
        }
    }
```

```scss
<style type="text/css" lang="scss" scoped>
    .images {
        img {
            display: inline-block;
            height: 100%;
            margin: 0 auto;
            cursor: pointer;
            margin:5px 10px;
        }
    }
</style>
```