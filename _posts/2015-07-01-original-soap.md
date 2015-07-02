---
layout: post
title: "使用SOAP访问WebService"
summary: "一个简单的例子"
description: ""
tags: ["SOAP","nodejs"]
categories: ["笔记"]
---
SOAP的基本介绍可以参考网页[SOAP Introduction](http://www.w3schools.com/webservices/ws_soap_intro.asp)。

我这里主要是介绍一个简单的例子，前面的步骤可以参考[Sending SOAP Requests by Using Visual Studio .NET Client (C#)](https://technet.microsoft.com/en-us/library/aa226051(v=sql.80).aspx)。

{% highlight csharp %}

using System;
using Soap.ClarifyService;
using System.ServiceModel;

namespace Soap
{
    class Program
    {
        static void Main(string[] args)
        {
            BasicHttpBinding binding = new BasicHttpBinding();
            binding.MaxReceivedMessageSize = Int32.MaxValue;
            EndpointAddress address = new EndpointAddress("http://xxx.adsk.com/clarify/webservice.asmx");

            WebServiceSoapClient service = new WebServiceSoapClient(binding, address);
            String bug = "1515643,1512171";
            var rootNode = service.GetDefects(bug);
            Console.WriteLine(rootNode.InnerXml);
            Console.Write("Press any key to continue . . . ");
            Console.ReadKey(true);
        }
    }
}

{% endhighlight %}

Node.js也可以访问SOAP，而且更简单，示例代码如下：

{% highlight javascript %}

var soap = require("soap");
var url = "http://xxx.adsk.com/clarify/webservice.asmx?WSDL";

reqURL = soap.createClient(url, function(err, client){
    if(err) {
        console.log(err);
        return;
    }

    var args = {sDefectIDs: '1515643,1512171'};

    client.WebService.WebServiceSoap.GetDefects(args, function(err, result){
        if(err) {
            console.log(err);
            return;
        }

        console.log(result.GetDefectsResult.DEFECTS.ROW);
    });
});

{% endhighlight %}

有几个地方需要澄清：

1. xxx.adsk.com不是真实网址，真实网址在我所在公司内网，不便于公开。
2. client.**WebService.WebServiceSoap.GetDefects**这个statement后面的名字从哪里来的？这就需要查看wsdl文件。

如果你想尝试一个可以运行的例子，可以试试下面的代码：

{% highlight javascript %}
var soap = require("soap");
var url = 'http://www.webservicex.net/stockquote.asmx?WSDL';

reqURL = soap.createClient(url, function(err, client){
    if(err) {
        console.log(err);
        return;
    }

    client.StockQuote.StockQuoteSoap.GetQuote({symbol:'NKE'}, function(err, response){
            if(err) {
                console.log(err);
                return;
            }

            console.log(response);
    });
});

{% endhighlight %}



