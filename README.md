## 这是啥
阿里云提供的短信发送平台接口
根据个人需求和使用场景改了改，便于测试用

## Versions
- 1.0 
直接提取出来SDK包，不 setup 了，哪用放哪
python 2 改成 python 3的，目录命名和 python 3下的 `http.client` 库有冲突，改掉了

### 阿里文件清单：

- api_demo(短信服务API接口调用DEMO工程)	- 删掉了
- api_sdk(短信服务API接口依赖的SDK)		- 改过的
- msg_demo(短信回执消息的DEMO)			- 删掉了
- msg_sdk(短信回执消息的SDK)			- 还没改


### 阿里文件说明:
SDK工具包中一共包含了2个目录：
- aliyun-python-sdk-core：阿里云api调用的核心代码库，python版本。
- alicom-python-sdk-dysmsapi：流量直冲相关接口调用的客户端以及示例代码。
确定本机已经安装了python，版本要求：2.6.5 或以上版本。
进入aliyun-python-sdk-core 执行：python setup.py install。
运行demo示例。进入alicom-python-sdk- dysmsapi目录执行：python demo.py 。


    # -*- coding: utf-8 -*-
	from aliyunsdkdysmsapi.request.v20170525 import SendSmsRequest
	from aliyunsdkdysmsapi.request.v20170525 import QuerySendDetailsRequest
	from aliyunsdkcore.client import AcsClient
	import uuid

	"""
	短信产品-发送短信接口
	Created on 2017-06-12
	"""

	REGION = "cn-hangzhou"

    # ACCESS_KEY_ID/ACCESS_KEY_SECRET 根据实际申请的账号信息进行替换
	ACCESS_KEY_ID = "yourAccessKeyId"
	ACCESS_KEY_SECRET = "yourAccessKeySecret"
	acs_client = AcsClient(ACCESS_KEY_ID, ACCESS_KEY_SECRET, REGION)

	# 请参考本文档步骤2
	def send_sms(business_id, phone_number, sign_name, template_code, template_param=None):
		smsRequest = SendSmsRequest.SendSmsRequest()
		# 申请的短信模板编码,必填
		smsRequest.set_TemplateCode(template_code)
		# 短信模板变量参数,友情提示:如果JSON中需要带换行符,请参照标准的JSON协议对换行符的要求,比如短信内容中包含\r\n的情况在JSON中需要表示成\\r\\n,否则会导致JSON在服务端解析失败
		if template_param is not None:
			smsRequest.set_TemplateParam(template_param)
		# 设置业务请求流水号，必填。
		smsRequest.set_OutId(business_id)
		# 短信签名
		smsRequest.set_SignName(sign_name);
		# 短信发送的号码，必填。支持以逗号分隔的形式进行批量调用，批量上限为1000个手机号码,批量调用相对于单条调用及时性稍有延迟,验证码类型的短信推荐使用单条调用的方式
		smsRequest.set_PhoneNumbers(phone_number)
		# 发送请求
		smsResponse = acs_client.do_action_with_exception(smsRequest)
		return smsResponse

	business_id = uuid.uuid1()
	print __business_id
	params = "{\"code\":\"12345\",\"product\":\"云通信\"}"
	print send_sms(__business_id, "1500000000", "云通信产品", "SMS_000000", params)

