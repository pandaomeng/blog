---
title: 微信小程序收集formid
date: 2018-11-29
tag: [miniprogram]
---

## 情景

如果想要小程序向用户推送模板消息，需要获取formId。我们想要突破微信的限制，在小程序的各个地方都收集formId，以备不时之需。（在button上收集的情况就不讨论了）

## 做法

### 一、将一块区域设置为formId收集区

注意：这种方法会盖住 区域的其他子节点，导致他们无法被点击。

1. 抽出一个 FormIdCover 组件，在需要收集 formId 的地方引用即可，组件如下：

   `FormIdCover.wxml`

   ```html
   <form class="form-id-form" bind:submit="getFormId" report-submit="{{true}}">
     <button class="form-id-button form-button" form-type="submit">
     </button>
   </form>
   ```

   <!--more-->

   `FormIdCover.wxss`

   ```scss
   .form-id-form, .form-id-button {
     position: absolute;
     z-index: 10;
     left: 0;
     top: 0;
     width: 100%;
     height: 100%;
   }
   
   /* 去除formId默认样式 */
   button.form-button {
     background-color: transparent;
     padding: 0;
     margin: 0;
     display: inline;
     border: 0;
     padding-left: 0;
     padding-right: 0;
     border-radius: 0;
     border-width: 0;
     font-size: 0rpx;
     color: transparent;
   }
   
   button.form-button::after {
     content: '';
     width: 0;
     height: 0;
     -webkit-transform: scale(1);
     transform: scale(1);
     border-radius: 0;
     border-width: 0;
     background-color: transparent;
   }
   ```

   `FormIdCover.js`

   ```js
   Component({
     methods: {
       getFormId (e) {
         if (!e.detail || !e.detail.formId) return
         if (e.detail.formId === 'the formId is a mock one') {
           console.log('mock 的 formId')
           this.triggerEvent('success')
           return
         }
         // do something
         conosole.log('formId: ', e.detail.formId)
         this.triggerEvent('success')
       }
     }
   })
   ```

2. 设置formIdCover

   `xxx.json`

   ```json
   {
     "component": true,
     "usingComponents": {
       "FormIdCover": "../../components/FormIdCover/FormIdCover"
     }
   }
   ```

   `xxx.wxml`

   ```html
   <view class="parent">
       <view id="child"></view>
       <FormIdCover bind:success="handle"/>
   </view>
   ```

   `xxx.wxml`

   ```css
   .parent {
       width: 200rpx;
       height: 200rpx;
       position: relative;
   }
   ```

   ps：

   - 组件FormIdCover的父节点必须设置position属性，只要不是static（即 将父节点设置为包含块）
   - 设置的FormIdCover会盖住整个view，导致 #child 节点不可点击，但是不影响冒泡，即parent节点是可以捕获点击事件的。如果一定要让 #child 可点击，需要设置他的z-index。

3. **如果点击收集formId的同时进行了跳转的动作**，获取formId的操作有几率被终止，因为两者是同步进行的。所以我们需要将跳转排在获取formId之后，即在bind:success="handle"的handle中进行跳转。



### 二、将view改成button

1. 在 app.wxss 中设置通用样式

   ```css
   /* 去除formId默认样式 */
   button.form-button {
     background-color: transparent;
     padding: 0;
     margin: 0;
     display: inline;
     border: 0;
     padding-left: 0;
     padding-right: 0;
     border-radius: 0;
     border-width: 0;
     font-size: 0rpx;
     color: transparent;
   }
   
   button.form-button::after {
     content: '';
     width: 0;
     height: 0;
     -webkit-transform: scale(1);
     transform: scale(1);
     border-radius: 0;
     border-width: 0;
     background-color: transparent;
   }
   ```

   注意：app.wxss 只会在page级别的页面上生效，component不受其影响

2. 将view改成button，并套上 form-button 的class

   ```html
   <form bind:submit="getFormId" report-submit="{{true}}">
       <button class="form-button" form-type="submit">
   		xxx...
       </button>
   </form>
   ```

   ```
   getFormId (e) {
     if (!e.detail || !e.detail.formId) return
     if (e.detail.formId === 'the formId is a mock one') {
       console.log('mock 的 formId')
       return
     }
     // do something
     conosole.log('formId: ', e.detail.formId)
   }
   ```
