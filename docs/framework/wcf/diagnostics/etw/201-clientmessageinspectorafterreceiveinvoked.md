---
title: "201 - ClientMessageInspectorAfterReceiveInvoked | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 9ff637f1-cc26-4400-ab9b-546f70e5057d
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# 201 - ClientMessageInspectorAfterReceiveInvoked
## 属性  
  
|||  
|-|-|  
|ID|201|  
|关键字|疑难解答，ServiceModel|  
|级别|信息|  
|通道|Microsoft\-Windows\-应用程序服务器\-应用程序\/分析|  
  
## 说明  
 服务模型在客户端消息检查器上调用 `AfterReceiveReply` 方法之后，将发出此事件。  
  
## 消息  
 调度程序对“%1”类型的 ClientMessageInspector 调用了“AfterReceiveReply”。  
  
## 详细信息  
  
|数据项名称|数据项类型|说明|  
|-----------|-----------|--------|  
|TypeName|`xs:string`|所调用检查器的类型的 CLR FullName。|  
|HostReference|`xs:string`|对于 Web 承载的服务，此字段唯一标识 Web 层次结构中的服务。此字段的格式定义为“网站名称应用程序虚拟路径&#124;服务虚拟路径&#124;服务名称”。示例：“默认网站\/CalculatorApplication&#124;\/CalculatorService.svc&#124;CalculatorService”。|  
|AppDomain|`xs:string`|由 AppDomain.CurrentDomain.FriendlyName 返回的字符串。|