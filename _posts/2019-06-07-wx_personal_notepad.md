---
layout:     post
title:      "微信小程序——个人笔记本"
subtitle:   ""
date:       2019-06-07
author:     "高鑫"
header-img: "img/miniprogram.jpg"
tags:
    - 微信小程序
---



## 话不多说先上效果图:比较简陋
### 写界面:
![写界面](https://raw.githubusercontent.com/visiter-1998/wx_notepad/master/imgs/WNPad.png "写界面")
### 查看界面:
![查看界面](https://raw.githubusercontent.com/visiter-1998/wx_notepad/master/imgs/VNPad.png "查看界面")
### 滑动删除选项:
![删除选项](https://raw.githubusercontent.com/visiter-1998/wx_notepad/master/imgs/DeleteNPad.png "删除选项")


## 代码部分(详情参考注释,注释统一用‘//’标识)
### 先附上需要的学习内容：

[小程序·云开发视频教学](https://cloud.tencent.com/edu/learning/course-100018-1367)：自己看文档累的话可以看看视频教学，很基础

[vant weapp UI组件](https://youzan.github.io/vant-weapp/#/intro)：上面连接的视频有简单介绍这个的用法

[小程序·配置](https://developers.weixin.qq.com/miniprogram/dev/framework/)：对于本程序主要看基础组件和云开发部分

PS:那啥，wxss和json就不介绍了，不会啊，这两玩意挺恶心的。

### writeNotePad:

#### wxml:这玩意看看文档就能知道咋用了，就不介绍了（下同）

```
<view class="all">
<input class="input"
    value="{{ value }}"
    placeholder="请输入标题"
    border="{{ true }}"
    bindinput="onChangeTitle"
    maxlength="6"
  />
<textarea class="textarea" auto-focus cursor-spacing="0"
    value="{{ value }}" 
    placeholder="请在这输入内容" 
    maxlength="-1"
    bindinput="onChangeNote"
  />
<button class="btn" bindtap='addPerNote'>保存</button>
</view>
```
#### js:

```
const db = wx.cloud.database()
const personalNote = db.collection('personal_note')//连接personal_note数据库
Page({

  /**
   * 页面的初始数据
   */
  data: {
   noteTitle:"",
   noteValue:"",
   time:"",
   year:"",
   month:"",
   day:"",
  },
  onChangeTitle: function(event) {
    // event.detail 为当前输入的值
    this.data.noteTitle = event.detail;
    //console.log(event.detail);
  },
  onChangeNote:function(event) {
    // event.detail 为当前输入的值
    this.data.noteValue = event.detail;
    //console.log(event.detail);
  },
  addPerNote:function (event) {
    var that = this;
    var date=new Date();
    that.data.time=date;
    that.data.year = date.getFullYear();
    that.data.month = date.getMonth() + 1;
    that.data.day = date.getDate();
    //上面是获取时间的
    //console.log(event)
    
    personalNote.add({//在表中增添一条数据，用法很固定
      data: {
        title: that.data.noteTitle,
        value: that.data.noteValue,
        time: that.data.time,
        dateYear: that.data.year,
        dateMonth: that.data.month,
        dateDay: that.data.day,
      }
    }).then(res => {
      console.log(res)
    })
  }
})
```
### viewNotePad:这一部分调用了vant weapp的UI组件

#### wxml:
```
<block wx:for="{{notes}}">
<van-swipe-cell id="swipe-cell" left-width="{{ 1 }}" right-width="{{ 65 }}"  bind:close="onClose" async-close position="outside" 
data-note-id="{{item._id}}"
data-note-title="{{item.title}}"
data-note-value="{{item.value}}"
>
//这玩意是滑块，可以左右滑动的，由于小程序基础组件中没有现成的好用的模板，
//只能用这个咯，自己写很麻烦，详情看vant weapp文档
<van-cell-group>//
  <van-cell title="{{item.title.value}}" bind:click="jumpTo" label="{{item.dateYear}}-{{item.dateMonth}}-{{item.dateDay}}"
    data-note-id="{{item._id}}"
    data-note-title="{{item.title}}"
    data-note-value="{{item.value}}"
  />
</van-cell-group>
<view slot="right">删除</view>
</van-swipe-cell>
</block>
//下面是筛选器，按照输入的时间来筛选需要的数据的，wxss修饰很重要，不然丑爆了
<view class="timePicker">
<button bindtap="choosedate">按日期筛选</button>
</view>
//这是个弹出窗口，参见vant weapp
<van-popup class="popup"
  show="{{ show }}"
  position="bottom"
  overlay="{{ true }}"
  bind:close="onClose"
>
<view>
  <input class="inputYear"
    value="{{ value }}"
    placeholder="请输入年份"
    border="{{ true }}"
    bindinput="onChangeYear"
    maxlength="4"
  />
</view>
<view>
  <input class="inputMonth"
    value="{{ value }}"
    placeholder="请输入月份"
    border="{{ true }}"
    bindinput="onChangeMonth"
    maxlength="2"
  />
</view>
<button bindtap="release">取消</button><button bindtap="inputdone">完成</button>
</van-popup>
```
#### js:
```
const db = wx.cloud.database()
const personalNote = db.collection('personal_note')
Page({
  data: {
    page:0,
    show: false,
    dataYear:"",
    dataMonth:"",
    flag:false//标志位，为true则默认全部查找，为false则按时间查找
  }, 
  choosedate: function (event) {//当show为true时，弹窗显示出来
    this.setData({ show: true })
  },
  release:function(event){//关闭弹窗咯~~
    this.setData({show:false})
  },
  //下两个绑定参数的
  inputYear:function(enent){
    //输入年份
    this.data.dataYear=event.detail
  },
  inputMonth:function(event){
    //输入月份
    this.data.dataMonth = event.detail
  },
  inputdone:function(event){
    //日期输入完成
    this.setData({ 
      show: false,
      flag:false
      })
    this.onShow();
  },
  jumpTo:function(event){//页面跳转以及页面之间传递参数
    var id = event.currentTarget.dataset.noteId;
    var title = event.currentTarget.dataset.noteTitle;
    var value = event.currentTarget.dataset.noteValue;
    wx.navigateTo({
      url: '/pages/modifierNoteBook/readnotebook?id=' + id + '&title=' + title + '&value=' + value
      //wx,navigateTo跳转后可以返回~~，传递一个参数?id=' + id直接这样写就行了
      //传递多个参数，那就要用&声明一下，就像上面的用法
      //传过去后有一个问题，还没解决，问题在下面再提
    });
  },
  
  onClose: function (event) {//这玩意，滑块特有的用法，可以仔细看下，有点意思
    const { position, instance } = event.detail;
    switch (position){
        //我把向右划删了，所以没有left了
      case 'right':
        //删除
        var info = event.currentTarget.dataset.noteId;
        personalNote.doc(info).remove().then(res => {
          this.onShow();//这玩意很重要，删除掉当前的，全靠调用这个函数刷新了
          instance.close();//这个是让滑块收回来的操作
        });
        break;
      case 'outside':
        //这玩意没啥用
        instance.close();
        break;
    }
  },
  onLoad: function (event) {
    //console.log("vo");
    //一个页面只调用一次！！！重点
  },
  
  //触底加载，小程序一页最多一次显示20条数据，所以就要触底刷新咯，教学视频有详解
  onReachBottom: function (res) {
    //console.log("~~~")
    let page=this.data.page+20;
    personalNote.skip(page).get().then(res => {
      let new_data=res.data
      let old_data=this.data.notes
      this.setData({
        notes: old_data.concat(new_data),
        page:page
      }),res=>{
        console.log(res);
      }
    })
  }, 

  onShow:function(event) {
      //这里多说几句，刷新页面就靠这玩意了，没记错的话onload是加载页面框架用的，
      //这个是渲染用的，查数据也可以放在onload中，但是我试过没法刷新
      //而且从下一个页面返回后（数据变了），并不会自行onload，只执行onshow
      //所以刷新放这里就好了，数据库用法挺简单的，固定套路
    if(this.data.flag){
      console.log("true")
      personalNote.get().then(res => {
        this.setData({
          notes: res.data
        })
      });
    }
    else{
      console.log("false")
      personalNote.where({
        dateYear: 2018,
        dateMonth: 6
      }).get().then(res=>{
        console.log(res.data)
        this.setData({
          notes: res.data,
          flag: true
        })
      })
    }
  },

  onUnload: function (event) {
    //这个啥功能忘了····好像挺有用···
  },
  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```
### modifierNotePad

    这玩意和writeNotePad.wxml基本上一样····没设计好就多了一个这个页面,还是贴一下吧，不同的地方参考注释

#### wxml:
```
<view class="all">
<input class="input"
    value="{{ notes.title.value }}"
    placeholder="请输入标题"
    border="{{ true }}"
    bindinput="onChangeTitle"
    maxlength="6"
  />
<textarea class="textarea" auto-focus cursor-spacing="0"
    value="{{ notes.value.value }}" 
    placeholder="请在这输入内容" 
    maxlength="-1"
    bindinput="onChangeNote"
  />
<button class="btn" bindtap='updatePerNote'>保存</button>
</view>
```
#### js:
```
const db = wx.cloud.database()
const personalNote = db.collection('personal_note')
Page({
  data: {
    id:'',
    noteTitle: "",
    noteValue: ""
  },
  onChangeTitle: function (event) {
    // event.detail 为当前输入的值
    this.data.noteTitle = event.detail;//顺序很重要
    console.log(event.detail);
  },
  onChangeNote: function (event) {
    // event.detail 为当前输入的值
    this.data.noteValue = event.detail;
    console.log(event.detail);
  },
  updatePerNote: function (event) {//都是套路，没啥技术含量
    personalNote.doc(this.data.id).update({
    data: {
        title: this.data.noteTitle,
        value: this.data.noteValue
      }
    }).then(res => {
      wx.showToast({//此处仅仅是消息框的持续时间，不保证数据更新后再跳转
        title: '更新成功并返回',
        icon: 'success',
        duration: 3000
      })
    });
    setTimeout(function () {//延时指令，保证数据库内容得到更新
      //要延时执行的代码
      wx.navigateBack({//返回前一个页面
        
      });
    }, 3000);
  },
  onLoad: function (options) {
    //重点来了，注意到上面不是event了没
    // 页面初始化 options为页面跳转所带来的参数 
    //这个带来的参数，即使是多个但也只是一个字符串···比如id10086title111value[··]
    //提取起来挺麻烦
    personalNote.doc(options.id).get({
      success:res=>{
        this.setData({
          notes:res.data
        });
      this.data.id = options.id;//在这可以直接通过options.id来获取传递的id值
      this.data.noteTitle = res.data.title;//但是，这里就不能yongoptions.title
                                           //来获取title的值了，原因可能就是上
                                           //面说的，我猜的，所以我直接通过id
                                           //查询全部信息·····
      this.data.noteValue = res.data.value;
      //console.log(res.data);
      //console.log(this.data.noteTitle);
      //console.log(this.data.noteValue);
      //print大法好啊
      }
    })
  }, 
  onshow: function (event) {

  },
  onUnload: function (event) {
    
  }
})
```

以上就是全部内容了~~css是真的有毒······真不想碰这玩意，噢，vant weapp的UI组件渲染网上教程不多,所以很头大我就没过多渲染······下面跟一个例子吧，以供参考
```
//这是viewNotePad中红色删除那部分的wxss···{}里面到是和css差不多，但是声明没搞懂
//他的规则
.van-swipe-cell__right {
  display: inline-block;
  width: 65px;
  height: 44px;
  font-size: 15px;
  line-height: 44px;
  color: rgb(255, 255, 255);
  text-align: center;
  background-color: rgb(255, 0, 0);
}
```
