---
layout:     post                    # 使用的布局（不需要改）
title:      Vue axios 的proxy代理               # 标题 
subtitle:   Vue axios 的proxy代理  #副标题
date:       2020-04-06              # 时间
author:     BY                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - 生活
    - vue	
    - 遇到的麻烦
---

配置 vue.config.js

    // vue.config.js 中设置proxy 代理
    module.exports = {
      devServer: {
        proxy: {
          //配置跨域
          "/api": {
            // 自己node.js 开放接口
            target: "http://127.0.0.1:5012", //这里后台的地址模拟的;应该填写你们真实的后台接口
            ws: true,
            changOrigin: true, //允许跨域
            pathRewrite: {
              "^/api": "" //请求的时候使用这个api就可以
            }
          }
        }
      }
    };
    

通过插件的方式设置 axios

目录下创建 plugins => http.js

    // 自定义插件
    import axios from 'axios'
    const myHttpServer = {}
    myHttpServer.instaill = (Vue) => {
      //设置请求的baseurl 为 配置的代理名
      axios.default.baseURL = '/api'
      Vue.prototype.$http = axios
    }
    export default = myHttpServer

在 main.js 入口文件引入

    import MyHttpServer from '@/plugins/http.js'
    // 插件的挂载
    Vue.use(MyHttpServer)

别处调用

    handleLogin(){
      this.$htttp.post('login',this.formdata)
      	.then(res => {
        
      })
    }


