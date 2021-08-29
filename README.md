# 浏览器控制台创建js脚本,破解学习通网课16倍速播放,和鼠标焦点锁定

*提示：16倍速下对电脑性能和网速要求较高，请先确保环境再操作否则会卡顿*
*测试环境：chrome浏览器*

**步骤一：** 首先打开课程视频，按F12打开开发者工具，找到console控制台

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021040617423965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤二：** 在console控制台粘贴以下代码：
其中第40行"v.playbackRate = 16;"是设置16倍速，根据电脑配置和当前网速可以自定义修改，经过测试16倍以上容易出错

//2021/8/29更新 - 自动跳过下一集
```
window.unitCount = $(".ncells h4").index($(".currents")) + 1;

// 获取小节数量
window.unit = $(".ncells h4").length;

function main(){
    const frameObj = $("iframe").eq(0).contents().find("iframe.ans-insertvideo-online");
    const videoNum = frameObj.length;
    if(videoNum > 0){
        console.log("%c当前小节中包含 " + videoNum + " 个视频","color:#FF7A38;font-size:18px");
        var v_done = 0;
        // 添加事件处理程序
        addEventListener("playdone" ,()=>{
            v_done++;
            watchVideo(frameObj, v_done)

        });
        // 播放
        watchVideo(frameObj, v_done);
    } else {
        if(window.unitCount < window.unit){
            console.log("%c当前小节中无视频，6秒后将跳转至下一节","font-size:18px");
            nextUnit();
        } else {
            console.log("%c好了好了，毕业了","color:red;font-size:18px");
        }
    }
}
function watchVideo(frameObj, v_done){
    // 添加播放事件
    var playDoneEvent = new Event("playdone");
    // 获取播放对象
    var v = undefined;
    v = frameObj.contents().eq(v_done).find("video#video_html5_api").get(0);window.a = v;
    // 设置倍速
    try{ v.playbackRate = 16;}
    catch(e){console.error("倍速设置失败！此节可能有需要回复内容，不影响，跳至下一节。错误信息："+e); nextUnit(); return;}
    // 播放
    v.play();
    console.log("%c正在 " + v.playbackRate + " 倍速播放第 " + (v_done + 1) + " 个视频","font-size:18px");
    // 循环获取播放进度
    window.inter = setInterval(()=>{
        v = window.a;
        if(v.currentTime >= v.duration){
            dispatchEvent(playDoneEvent);
            clearInterval(window.inter);
        }
        if(v.paused){
            v.play();
        }
    },1000);
}
function nextUnit(){
    console.log("%c即将进入下一节...","color:red;font-size:18px");
    setTimeout(() => {
        $(document).scrollTop($(document).height()-$(window).height());
        $(".orientationright").click();
        console.log("%c行了别看了，我知道你学会了，下一节","color:red;font-size:18px");// (已经跳转" +(++window.unitCount)+"次)");
        if(window.unitCount++ < window.unit){ setTimeout(() => main(), 10000) }
    }, 6000);
}
main();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406174846992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤三：** 粘好后按enter回车，可以看到视频已经按16倍速抽搐播放！！！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406174926318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤四：** 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406175229767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤五：** 做完题提交后点击下一章继续刷视频，因为做题时间可能比较长所以脚本大概率会挂掉，所以这时后按下F12,goto步骤一

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406180905333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)
