---
layout: post
title: "使用soap访问webservice"
summary: "一个简单的例子"
description: ""
tags: ["soap"]
categories: ["笔记"]
---
例子很简单，前面的步骤可以参考[Sending SOAP Requests by Using Visual Studio .NET Client (C#)](https://technet.microsoft.com/en-us/library/aa226051(v=sql.80).aspx)。

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
            EndpointAddress address = new EndpointAddress("http://xxx.com/clarify/webservice.asmx");

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
