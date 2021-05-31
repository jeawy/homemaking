
### 关于Twilio
Twilio是一家全球范围内的视频、音频、短信服务商。我们测试过Twilio的视频通话，<strong>在国内与美国、澳大利亚等地区进行视频通话，信号非常稳定、画质和音频也都很清晰。</strong>
<br/>
Twilio是收费的，收费模式：https://www.twilio.com/video/pricing 可以按照通话时间收费 
费用不高每分钟是$0.004，可以接受。
### 示例工程的使用
<a href="http://tl.chidict.com:8081/wefxs/twiliotestapp.apk" >点击此处下载安卓版本的安卓包</a>
测试：
通话的双方分别使用账户：18811082245，密码：889886 和13955818600密码123456 在两个手机端进行登录。
登录后，点击“我的”-->"我的预约",在“我的预约”页面选择2021年3月9号，然后点击红色的视频通话图标进行通话。
如有疑问，联系QQ：461882709
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
```
# python 版本服务端产生token的代码

from twilio.jwt.access_token import AccessToken
from twilio.jwt.access_token.grants import VideoGrant

# Substitute your Twilio AccountSid and ApiKey details
ACCOUNT_SID = 'account_sid'
API_KEY_SID = 'api_key_sid'
API_KEY_SECRET = 'api_key_secret'

# Create an Access Token
token = AccessToken(ACCOUNT_SID, API_KEY_SID, API_KEY_SECRET)

# Set the Identity of this token
token.identity = 'example-user'

# Grant access to Video
grant = VideoGrant(room='cool room')
token.add_grant(grant)

# Serialize the token as a JWT
jwt = token.to_jwt()
print(jwt)
```
### 常见问题
#### 1 Token超时导致app退出
    理想的应用应该是通话的时候，从服务端获取最新的token。

#### Twilio的设置
    Twilio需要SID(个人身份id)和product key（应用的key）设置方式，可以联系QQ461882709协助设置。
