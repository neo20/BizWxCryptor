# BizWxCryptor
+ 企业微信自建应用加密解密类，在python3.5上测试
+ 大部分代码来自项目：https://github.com/sbzhu/weworkapi_python 
+ 修改了其不兼容python3的部分 

## 文件说明

+ 主要是WXBizMsgCrypt.py和ierror.py这两个文件
+ BizWxUtil.py是为django2.x写的一个工具。如果用其他框架不用这个，但可以参照此文件的业务逻辑。

## 依赖安装
    

    pip3 install pycryptodome

> 注意有很多Crypto，安装上面这个是C大写的，并且带AES

## Django示例代码

> 首先在BizWxUtil.py里设置参数sToken、sEncodingAESKey、sCorpID


    from django.shortcuts import render
    from django.http import HttpResponse
    from django.views.decorators.csrf import csrf_exempt
    
    from . import WorkWXUtil 

    @csrf_exempt
    def index(request):
        # 回应url验证
        if 'echostr' in request.GET:
            return WorkWXUtil.responseEcho(request)

        if request.method == 'POST':
            message = WorkWXUtil.parseMessage(request)
            # 示例，原文返回
            text = {
                'to_user':message['FromUserName'],
                'from_user':message['ToUserName'],
                'type':'text',
                'content':message['Content'],
            }
            return WorkWXUtil.parseResponse(text)
