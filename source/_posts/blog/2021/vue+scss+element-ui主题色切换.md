---
title: Vue+Scss+ElementUI主题色切换
date: 2021-7-20 16:55
tags: 'Vue'
categories: '2021'
---
千呼万唤始出来，学习总结吧

<!-- more -->

### 写在开头

虽然说已经有很多的同学写过这方面的技术blog，但自己在做一遍，加深记忆，也不是蛮不错的。

#### 效果展示

<video autoplay="autoplay" loop="loop" muted style="width: 100%">   
    <source src="https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/video/blog/element-ui-theme-change.mp4" type="video/mp4"> 
</video>

#### 思路

- 全局替换主题色的颜色值，创建一个新的`style`,覆盖原来的样式，实现主题色更换

#### 注意点

- 我本地缓存的是替换颜色后整个 `css`文本，并不是单独缓存了颜色值，这样做的目的是解决修改主题色后，刷新页面会有一个小的颜色切换闪烁问题
- 自己写的元素主题颜色最好和 `element-ui`的主题色文本值保持一致，不然的话会出现自己写的那一部分的颜色无法被替换修改的问题

#### 核心代码 [完整代码](https://taoaiyi1108.oss-cn-beijing.aliyuncs.com/post/element/joydigit-element-ui.zip)

```vue
<template>
  <el-color-picker v-model="theme" :predefine="predefine" @change="themePickerChange"/>
</template>

<script>
const version = require("element-ui/package.json").version; // element-ui version from node_modules
const ORIGINAL_THEME = "#409EFF"; // default color

export default {
  data() {
    return {
      chalk: "", // content of theme-chalk css
      theme: "",
      predefine: [
        "#409EFF",
        "#1890ff",
        "#304156",
        "#212121",
        "#11a983",
        "#13c2c2",
        "#6959CD",
        "#f5222d",
      ],
    };
  },
  computed: {
    defaultTheme() {
      return this.$store.state.theme;
    },
    defaultVersion() {
      return this.$store.state.version;
    },
    defaultChalk() {
      return this.$store.state.chalk;
    }
  },
  watch: {
    defaultTheme: {
      handler: function (val, oldVal) {
        this.theme = val;
        this.themeMethod(val)
      },
      immediate: true,
    },
    async theme(val) {
      // this.themeMethod(val)
    },
  },

  methods: {
    // 替换 element-ui css 中颜色值
    async themeMethod(val) {
      const oldVal = this.chalk ? this.theme : ORIGINAL_THEME;
      if (typeof val !== "string") return;
      const themeCluster = this.getThemeCluster(val.replace("#", ""));
      const originalCluster = this.getThemeCluster(oldVal.replace("#", ""));

      const $message = this.$message({
        message: "  Compiling the theme",
        customClass: "theme-message",
        type: "success",
        duration: 0,
        iconClass: "el-icon-loading",
      });
      // 通过id拿取 style
      const getHandler = (variable, id) => {
        return () => {
          const originalCluster = this.getThemeCluster(ORIGINAL_THEME.replace("#", ""));
          const newStyle = this.updateStyle( this[variable], originalCluster, themeCluster );
          let styleTag = document.getElementById(id);
          if (!styleTag) {
            styleTag = document.createElement("style");
            styleTag.setAttribute("id", id);
            document.head.appendChild(styleTag);
          }
          styleTag.innerText = newStyle;
        };
      };
      // 判断本地有没缓存了 替换后的颜色主题css文本 避免刷新颜色主题闪烁问题
      if(this.defaultVersion !== version || !this.defaultChalk) {
        const url = `https://unpkg.com/element-ui@${version}/lib/theme-chalk/index.css`;
        await this.getCSSString(url, "chalk");
      } else {
        this.chalk = this.defaultChalk
      }

      const chalkHandler = getHandler("chalk", "chalk-style");
      chalkHandler();

      const styles = [].slice .call(document.querySelectorAll("style")).filter((style) => {
        const text = style.innerText;
        return (
          new RegExp(oldVal, "i").test(text) && !/Chalk Variables/.test(text)
        );
      });
      styles.forEach((style) => {
        const { innerText } = style;
        if (typeof innerText !== "string") return;
        style.innerText = this.updateStyle(
          innerText,
          originalCluster,
          themeCluster
        );
      });

      this.$emit("change", val);

      $message.close();
      this.cachesTheme()
    },
    // 通过正则替换颜色值
    updateStyle(style, oldCluster, newCluster) {
      let newStyle = style;
      oldCluster.forEach((color, index) => {
        newStyle = newStyle.replace(new RegExp(color, "ig"), newCluster[index]);
      });
      return newStyle;
    },
    // 自定义XMLHttpRequest 请求 css 样式文件
    getCSSString(url, variable) {
      return new Promise((resolve) => {
        const xhr = new XMLHttpRequest();
        xhr.onreadystatechange = () => {
          if (xhr.readyState === 4 && xhr.status === 200) {
            this[variable] = xhr.responseText.replace(/@font-face{[^}]+}/, "");
            resolve();
          }
        };
        xhr.open("GET", url);
        xhr.send();
      });
    },
    // 颜色值转换
    getThemeCluster(theme) {
      const tintColor = (color, tint) => {
        let red = parseInt(color.slice(0, 2), 16);
        let green = parseInt(color.slice(2, 4), 16);
        let blue = parseInt(color.slice(4, 6), 16);

        if (tint === 0) {
          // when primary color is in its rgb space
          return [red, green, blue].join(",");
        } else {
          red += Math.round(tint * (255 - red));
          green += Math.round(tint * (255 - green));
          blue += Math.round(tint * (255 - blue));

          red = red.toString(16);
          green = green.toString(16);
          blue = blue.toString(16);

          return `#${red}${green}${blue}`;
        }
      };

      const shadeColor = (color, shade) => {
        let red = parseInt(color.slice(0, 2), 16);
        let green = parseInt(color.slice(2, 4), 16);
        let blue = parseInt(color.slice(4, 6), 16);

        red = Math.round((1 - shade) * red);
        green = Math.round((1 - shade) * green);
        blue = Math.round((1 - shade) * blue);

        red = red.toString(16);
        green = green.toString(16);
        blue = blue.toString(16);

        return `#${red}${green}${blue}`;
      };

      const clusters = [theme];
      for (let i = 0; i <= 9; i++) {
        clusters.push(tintColor(theme, Number((i / 10).toFixed(2))));
      }
      clusters.push(shadeColor(theme, 0.1));
      return clusters;
    },
    // 缓存主题色
    cachesTheme() {
      // index -1 说明你选中的颜色没有在预制颜色中
      const index = this.predefine.findIndex(item => item.toLocaleLowerCase() === this.theme.toLocaleLowerCase()) || 0
      window.document.documentElement.setAttribute("data-theme", `theme${index !== -1 ? index : 0}`);
      sessionStorage.setItem('theme', this.theme)
      sessionStorage.setItem('chalk', this.chalk)
      sessionStorage.setItem('version', version)
    },
    // 通过选择器 修改主题色
    themePickerChange(themeColor) {
      sessionStorage.setItem('theme', themeColor)
      // 刷新页面 是为了： 不在预制颜色的主题色，只有elementUI内部组件会变，scss变量设置的颜色值不会变，是因为对应的style文本没用动态替换
      location.reload()
    }
  },
};
</script>

```

