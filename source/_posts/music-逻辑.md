---
title: music-逻辑
---

# url跳转传参
## 设置参数
在musicList组件，点击图片跳转入itemMusic界面
需求是跳转时传入图片数据中的id
```html
<van-swipe-item v-for="(item,index) in musicList" :key="index" class="musicSwpie">
                        <router-link :to="{name:'ItemMusic',params:{id: item.id } }">
                            <div class="pic"><img :src="item.picUrl"></div>
                            <div class="playCount">
                                <svg class="icon" aria-hidden="true">
                                    <use xlink:href="#icon-Playerplay"></use>
                                </svg>
                                {{changeNum(item.playCount)}}
                            </div>
                            <span class="name">{{item.name}}</span>
                        </router-link>
                    </van-swipe-item>
```
将id以params传入新的界面

为itemMusic规定的url为
```
path: '/itemMusic/:id',
    name: 'ItemMusic',
```
## 接收参数
在itemMusicList界面中，参数储存在当前路由中，用``this.$route.params.id``访问，然后请求所需歌单即可
``let res = await getMusicItemList(this.$route.params.id)``

补：在itemMusicTop中，需要很多歌单数据，直接父子通信，传入playList对象；在itemMusic中，只需要收藏量，传入subScribeCount字符串
# 数据处理
## 播放量处理
```
methods: {
            changeNum(num) {
                if (num >= 10000000)
                    return (num / 100000000).toFixed(1) + "亿"
                else if (num >= 10000)
                    return (num / 10000).toFixed(1) + "万"
            }
        }
```
# 播放流程
## 在歌单处点击歌曲，访问歌曲
``@click="playMusic(i)"``点击传入歌曲下标，更新全局数据：
playList和playListIndex
```JavaScript
playMusic:function(i){
                this.updatePlayList(this.itemList)
                this.updatePlayListIndex(i)
                this.$store.dispatch("getLyric",
                this.playList[this.playListIndex].id)
            },
```
## 在footMusic配置播放器audio
```JavaScript
<audio ref="audio" @timeupdate="timeUpdate" preload="auto" 
:src="`https://music.163.com/song/media/outer/url?id=${playList[playListIndex].id}.mp3`">
</audio>
```
由于音乐的源在于网易云服务器，和api不统一，所以直接使用完整url
### 访问audio标签
使用ref访问``this.$refs.audio``
[知乎 ref](https://zhuanlan.zhihu.com/p/50638655)
## 暂停与播放
```
<svg class="icon" aria-hidden="true" v-if="isbtnShow" @click="play">
    <use xlink:href="#icon-bofang"></use>// 暂停时显示播放
</svg>

<svg class="icon" aria-hidden="true" v-else @click="play">
    <use xlink:href="#icon-zanting"></use>// 播放时显示暂停
</svg>
```
使用全局变量isbtnShow管理播放与暂停状态
使用函数play()修改isbtnShow，并修改播放器状态``this.$refs.audio.play()/this.$refs.audio.pause()``
```
methods:{
	play: function(){
                if(this.$refs.audio.paused){
                    this.$refs.audio.play();
                    this.updateIsbtnShow(false)
                }else{
                    this.$refs.audio.pause();
                    this.updateIsbtnShow(true)
                }
    }
}
```
## 切换歌曲
在footMusic下，以歌单不更改为前提，监视歌曲下标，更新：
歌曲自动播放，歌词，歌曲长度
```JavaScript
watch:{
	playListIndex: function () {
                this.$refs.audio.autoplay=true
                if(this.$refs.audio.paused){
                    this.updateIsbtnShow(false)
                }
                this.$store.dispatch("getLyric",this.playList[this.playListIndex].id)
                this.updateDuration(this.$refs.audio.duration)
            }
}
```
在切换歌单后
如果下标不一致，触发检测playListIndex，歌曲完成更新
如果下标一致，点击时已触发更新playList，补充歌曲自动播放即可
```
watch:{
	playList: function () {
                if(this.IsbtnShow){
                    this.$refs.audio.autoplay=true
                    tihs.updateIsbtnShow(false)
                }
            }
}
```
# 歌曲详情
```html
<van-popup v-model="detailShow" position="bottom" :style="{ height: '100%',width:'100%' }">
    <MusicDetail :musicList="playList[playListIndex]" 
    :play="play" :setTime="setTime"/>
</van-popup>
```
采用上弹框弹出歌曲详情页面，传入参数
:musicList="playList[playListIndex]"用于访问歌曲详情
:play="play"用于控制audio播放
## 歌词
### 获取歌词
在点击歌曲，或者切换歌单时，都会触发获取歌词
### 处理歌词
获取到的歌词为长字符串，先以换行符为界，分割成数组
```JavaScript
arr = this.lyricList.lyric.split(/[(\r\n)\r\n]+/).map((item, i) => {...}）

arr.forEach((item, i) => {
	if (i === arr.length - 1 || isNaN(arr[i + 1].time)) {
		item.pre = 100000
	}else{
	item.pre = arr[i + 1].time
	}
});

this.lyric = arr
```
在...内，封装数组内容为新对象，返回给arr
```JavaScript
let min = item.slice(1, 3)
let sec = item.slice(4, 6)
let mill = item.slice(7, 10)
let lrc = item.slice(11, item.length)
let time = parseInt(min) * 60 * 1000 + parseInt(sec) * 1000 + parseInt(mill)
// 若毫秒没有数值
if (isNaN(Number(mill))) {
	mill = item.slice(7, 9)
	lrc = item.slice(10, item.length)
	time = parseInt(min) * 60 * 1000 + parseInt(sec) * 1000 + parseInt(mill)
}
return { min, sec, mill, lrc, time }
```
此时就能用this.lyric访问歌词了
### 显示歌词
```html
<div class="musicLyric" v-show="isLyricShow" 
@click="lyricShow()" ref="musicLyric">
            <p v-for="(item,i) in lyric" :key="i"
                :class="{lyricActive:
                (currentTime *1000>=item.time 
                && currentTime *1000<item.pre)}">
                {{item.lrc}}
            </p>
</div>
```
规定歌曲封面和歌词交替出现
在歌词界面，每句歌词都有time和pre两个属性，分别表示歌词开始时间和歌词结束时间
audio有一个属性currentime，表示当前播放时间，通过逻辑可确定当前播放的是哪句歌词，执行字体突出布局
``(currentTime *1000>=item.time && currentTime *1000<item.pre)``
### 歌词锁定
要求歌词界面随着歌词滚动，正在播放的歌词要锁定在屏幕中央
选择字体突出的歌词，其属性offsetTop表示到顶端的距离
监视currentTime，当offsetTop超出设定值时修改即可
```JavaScript
watch:{
	currentTime:function(){
		let p = document.querySelector("p.lyricActive")
		if (p && p.offsetTop > 300) {
			this.$refs.musicLyric.scrollTop = 
				p.offsetTop - 300
		}
	}
}
```
# 歌曲进度条
需求是进度条随着歌曲播放前进，且可以通过拖动进度条重新定位
核心：audio属性currentTime
## 访问currentTime
使用vant组件中的滑块实现
```html
<van-slider v-model="currentTime" @change="onChange" :max="duration" :min="0" />
```
滑块最小值定为0，最大值定为歌曲长度duration
通过audio的timeUpdate事件，滑块位置和currentTime绑定
```html
<audio ref="audio" @timeupdate="timeUpdate" preload="auto">
```
```JavaScript
timeUpdate(e){
                // 此处更新currentTime
                this.updateCurrent(e.target.currentTime)
            },
```
## 修改
使用滑块事件@change，执行滑动结束的回调函数，传入参数为滑块最终位置数值
```JavaScript
onChange(value) {
                this.updateCurrent(value)
                this.setTime(value)
            }
```
其中setTime是footMusic组件中传递过来的函数
```JavaScript
setTime(value){
                this.$refs.audio.currentTime = value
            }
```
注：由于网易云服务器问题，返回的audio组件有时会出现currentTime=0的bug，导致滑块不能实现，但可以播放音乐