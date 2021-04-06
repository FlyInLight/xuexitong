 问题描述： (未解决)学习通网课的视频无法拖动进度条
已解决：
没有倍速播放——破解1-16倍速
鼠标离开页面视频会暂停——鼠标可以离开页面
视频播放会弹出小练习必须通过才能继续播放——可以不用做小练习直接播放视频
 - 之前有可以拖进度条的漏洞，现在不知道被改哪去了我比较菜没找到；
 - 不过终于被我get到了另一种方法，通过直接在控制台运行javascript脚本修改视频的倍速，经过测试最高16倍比较稳定，再高容易出错；
 - 本来想在此基础上搞全自动的做题翻页，但是太懒了呃呃呃有肉吃就不想继续了

*提示：16倍速下对电脑性能和网速要求较高，请先确保环境再操作否则会卡顿*
*测试环境：chrome浏览器*

**步骤一：** 首先打开课程视频，按F12打开开发者工具，找到console控制台

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021040617423965.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤二：** 在console控制台粘贴以下代码：
其中第40行"v.playbackRate = 16;"是设置16倍速，根据电脑配置和当前网速可以自定义修改，经过测试16倍以上容易出错

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
            if(v_done > videoNum){
                // 下一节
            } else if(v_done < videoNum){
                watchVideo(frameObj, v_done)
            } else {
                console.log("%c本小节视频播放完毕，等待跳转至下一小节...","font-size:18px");nextUnit();
            }
        });
        // 播放
        watchVideo(frameObj, v_done);
    } else {
        if(window.unitCount < window.unit){
            console.log("%c当前小节中无视频，6秒后将跳转至下一节","font-size:18px");
            nextUnit();
        } else {
            console.log("%c结束","color:red;font-size:18px");
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
        console.log("%c正在跳转下一节，如有章节测试则无法跳转，请先完成测试","color:red;font-size:18px");// (已经跳转" +(++window.unitCount)+"次)");
        if(window.unitCount++ < window.unit){ setTimeout(() => main(), 10000) }
    }, 6000);
}
console.log("%c 欢迎使用本脚本，此科目有%c %d %c个小节，当前为 %c第%d小节 %c-chao", "color:#6dbcff", "color:red", window.unit, "color:#6dbcff", "color:red", window.unitCount, "font-size:8px");
main();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406174846992.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤三：** 粘好后按enter回车，可以看到视频已经按16倍速抽搐播放！！！

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406174926318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤四：** 正常视频播放完毕是可以自动播放下一个的，但是有的视频后面有章节测验，这会导致当前视频循环播放，所以看完视频拿到绿色任务点后要把题做了,但是这时后点击章节测验会出现在调试器中暂停的提示，按F12关闭开发者工具即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406175229767.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)

**步骤五：** 做完题提交后点击下一章继续刷视频，因为做题时间可能比较长所以脚本大概率会挂掉，所以这时后按下F12,goto步骤一

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210406180905333.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2EyMjcyMDYyOTY4,size_16,color_FFFFFF,t_70)
代码思路参考GitHub的大哥：https://github.com/chengjunchao/xuexitongScript/blob/master/xuexitong.js
