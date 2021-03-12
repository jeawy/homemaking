
### 关于Twilio
Twilio是一家全球范围内的视频、音频、短信服务商。我们测试过Twilio的视频通话，<strong>在国内与美国、澳大利亚等地区进行视频通话，信号非常稳定、画质和音频也都很清晰。</strong>
<br/>
Twilio的收费模式：https://www.twilio.com/pricing可以按照通话时间收费 

### 示例工程的使用
联系QQ461882709来索取测试demo的安卓安装包、或者iOS的app（需要有testflight）
### 使用方法： 

引入插件：
```
// 放在script标签之下，export default之前
const zjw_twilio = uni.requireNativePlugin('zjw-VideoCallTwilio'); 
 
开启视频通话：
zjw_twilio.VideoCall({ 
        token: token, //token需要在服务端产生,
        roomname: "autwilio"+bookid,// roomid可以自定义，但是这个roomid产生token的一个参数。
    }, result => {
        console.log(result);
}); 
```
  
### 常见问题
#### 1 Token超时导致app退出
    理想的应用应该是通话的时候，从服务端获取最新的token。

#### Twilio的设置
    Twilio需要SID(个人身份id)和product key（应用的key）设置方式，可以联系QQ咨询。
