# qcloudsms
a qcloudsms sdk  with es6 class and promise
一个参考腾讯云官方规则封装的更加优雅的短信发送sdk,[官方Demo](https://github.com/qcloudsms/qcloudsms/tree/master/demo/js)


## start 开始使用

> before you start to use this package,you should get this appid and appkey from [TencentCloud](https://cloud.tencent.com/product/sms) ,dont use the phoneNumbers in demos,its just to show you how to run it.

>在使用之前,请先去申请[腾讯云短信应用](https://cloud.tencent.com/product/sms),以便获取必要的appid和appkey,并且需要申请相关的短信签名，短信模板,否则,你将会得到各种错误码返回值。[错误码](https://cloud.tencent.com/document/product/382/3771)

[实例 Demo](https://github.com/unliar/qcloudsms/blob/master/demo.js)
```
1、install 安装

npm install qcloudsms --save

2、require 导入

const Qsms=require("qcloudsms")

3、init 初始化

const qsms=new Qsms("appid",'appkey')

4、send SMS 调用接口发送短信

5、check result 接收返回结果
```
## return 返回值 
> 类型：[Promise](http://es6.ruanyifeng.com/#docs/promise)
```

// demo中的res.data值示例
// 错误码列表 https://cloud.tencent.com/document/product/382/3771

// 单发返回值
{
  result: 0, // 成功字段值为0,失败则会显示错误状态码。
  errmsg: 'OK', //成功字段值为"OK",失败则会显示错误原因
  ext: '',
  callid: '...' 
}

// 群发返回值
{
  result: 0,
  errmsg: 'OK',
  ext: '',
  detail: [{
      result: 0,
      errmsg: 'OK',
      mobile: '177883232323',
      nationcode: '86',
      sid: '8:Kdasdasddasdasd20171028',
      fee: 1
    },
    {
      result: 0,
      errmsg: 'OK',
      mobile: '17603073232',
      nationcode: '86',
      sid: '8:Lfdasdasds20171028',
      fee: 1
    }
  ]
}
```



## methods
```
//demo public import

const Qsms=require("qcloudsms")

const qsms=new Qsms(idnumber,'key')
```
1. singeSend({
      phoneNumber,
      msg,
      msgType = 0,
      nationCode = "86",
      extend = "",
      ext = ""
    }) 

```
  /**
   * 单发短信
   * @param {string} phoneNumber 手机号 
   * @param {string} msg 短信正文，如果需要带签名，签名请使用【】标注
   * @param {number} msgType 短信类型，0 普通短信，1 营销短信。默认值:0
   * @param {string} nationCode 国家码,默认值:"86"
   * @param {string} extend 扩展字段，默认值:""
   * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
   */
   
//demo

 qsms.singleSend({
    phoneNumber,
    msg,
    msgType = 0,
    nationCode = "86",
    extend = "",
    ext = ""
  }).then(res=>{
    console.log(res.data)
    if(res.data.result===0){
      // do success
    }else{
      //errors
      //find your error code in this list
      //https://cloud.tencent.com/document/product/382/3771
    }
  })

```

2. singleSendWithParams({
      phoneNumber,
      tpl_id,
      params,
      sign,
      nationCode = "86",
      ext = "",
      extend = ""
    })
  ```
  /**
    * 模板单发短信
    * @param {string} phoneNumber 手机号
    * @param {number} tpl_id 短信模板id， 详情：https://console.qcloud.com/sms/smsContent 
    * @param {array} params 模板参数数组,元素个数请不要超过模板参数个数
    * @param {string} sign 短信签名
    * @param {string} nationCode 国家码,默认值:"86"
    * @param {string} extend 扩展字段，默认值:""
    * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
  */

  //demo

    qsms.singleSendWithParam({
        phoneNumber: 176030703,
        params: [12345],
        tpl_id: 4266,
        sign: '签名'
      }).then(res => {
            console.log(res.data)
            if(res.data.result===0){
              //  do success
            }else{
              //errors
              //find your error code in this list
              //https://cloud.tencent.com/document/product/382/3771
            }
        })  
  ```
  3. multiSend({
        phoneNumbers,
        msg,
        msgType = 0,
        nationCode = "86",
        extend = "",
        ext = ""
     })
  ```
  /*
  * 群发短信【仅国内,一次不超过200】
  * @param {array} phoneNumbers 群发手机号数组 
  * @param {string} msg 短信正文，如果需要带签名，签名请使用【】标注
  * @param {number} msgType 短信类型，0 普通短信，1 营销短信。 默认值:0
  * @param {string} nationCode 国家码,默认值:"86"
  * @param {string} extend 扩展字段，默认值:""
  * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
  */

  //demo
  qsms.multiSend({
      phoneNumbers: [17603070288, 17788770699],
      msg: "您的验证码6789，此验证码10分钟内有效，请勿向他人泄露"
    }).then(res =>{
      console.log(res.data)
      if(res.data.result===0){
        //do success
      }else{
        //errors
        //find your error code in this list
        //https://cloud.tencent.com/document/product/382/3771
      }
    })

  ```
  4. multiSendWithParams({
      phoneNumbers,
      tpl_id,
      params,
      sign,
      nationCode = "86",
      ext = "",
      extend = ""
    }) 

  ```
  /**
    * 模板群发短信【仅国内,一次不超过200】
    * @param {array} phoneNumbers 群发手机号数组 
    * @param {number} tpl_id 短信模板id， 详情：https://console.qcloud.com/sms/smsContent 
    * @param {array} params 模板参数数组,元素个数请不要超过模板参数个数
    * @param {string} sign 短信签名
    * @param {string} nationCode 国家码,默认值:"86"
    * @param {string} extend 扩展字段，默认值:""
    * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
  */
  
  //demo
  qsms.multiSendWithParams({
      phoneNumbers: [17603070288, 17788770668],
      params: [4523],
      tpl_id: 423866,
      sign: '哈哈'
    }).then(res =>{
        console.log(res.data)
        if(res.data.result===0){
          //do success
          //
        }else{
          //errors
          //find your error code in this list
          //https://cloud.tencent.com/document/product/382/3771
        }
    })

  ```
  
  5. sendVoice({
        phoneNumber,
        msg,
        playtimes = 2,
        nationCode = "86",
        ext = ""
      })
  ```
  /* 语音验证码
  * @param {string} nationCode 国家码,默认值:"86"
  * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
  * @param {number} playtimes 重播次数，默认2，最大3。
  * @param {number} phoneNumber 手机号码
  * @param {number|string} msg 验证码，支持英文字母、数字及组合。
  */
  qsms.sendVoice({
      phoneNumber: 17603070437,
      msg: "876123"
    }).then(res => {
      if(res.data.result===0){
          //do success
          //
        }else{
          //errors
          //find your error code in this list
          //https://cloud.tencent.com/document/product/382/3771
      }
    })
  ```
  
  6. sendVoicePrompt({
        phoneNumber,
        promptfile,
        prompttype = 2,
        nationCode = "86",
        playtimes = 2,
        ext = ""
      })

  ```
  /*发送语音通知
  * @param {number} phoneNumber 手机号码
  * @param {string} promptfile 通知内容，utf8编码，支持中文英文、数字及组合，需要和语音* 内容模版相匹配
  * @param {number} prompttype 语音类型,目前固定为2
  * @param {number} playtimes 重播次数，默认2，最大3。
  * @param {string} nationCode 国家码,默认值:"86"
  * @param {string} ext 此字段腾讯云后台服务器会按原样在应答中,默认值:""
  */
  qsms.sendVoicePrompt({
        phoneNumber: 17603070235,
        promptfile: "您好雷锋，您的参会申请已经审核通过，请于11点按时参加会议，期待您的到来。"
      }).then(res => {
        if(res.data.result===0){
          //do success
          //
        }else{
          //errors
          //find your error code in this list
          //https://cloud.tencent.com/document/product/382/3771
        }
      })
  ```