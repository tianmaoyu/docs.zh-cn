---
title: ".NET Framework 部署指南（针对管理员） | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "VB"
  - "CSharp"
  - "C++"
  - "jsharp"
helpviewer_keywords: 
  - "管理员指南, 部署 .NET Framework"
  - "开发 [.NET Framework], 管理员指南"
ms.assetid: bee14036-0436-44e8-89f5-4bc61317977a
caps.latest.revision: 40
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 38
---
# .NET Framework 部署指南（针对管理员）
本文分步说明系统管理员可以如何使用 Microsoft System Center Configuration Manager 在网络中部署 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 及其系统依赖项。  本文假定所有目标客户端计算机都满足 .NET Framework 的最低要求。  有关安装 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 的软件和硬件要求列表，请参参阅 [系统需求](../../../docs/framework/get-started/system-requirements.md)。  
  
> [!NOTE]
>  本文档中提到的软件（包括但不限于 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)]、System Center Configuration Manager 和 Active Directory）均受许可条款和条件的约束。  下列说明假定，软件的适当被许可方已查看并接受此类许可条款和条件。  这些说明不免除此类许可协议中的任何条款和条件。  
>   
>  有关支持 .NET Framework 的信息，请参见 Microsoft 支持网站上的 [Microsoft .NET Framework 支持生命周期策略](http://go.microsoft.com/fwlink/?LinkId=196607)。  
  
 本主题包含以下各节：  
  
 [部署过程](#the_deployment_process)   
 [部署 .NET Framework](#deploying_in_a_test_environment)  
 [创建集合](#creating_a_collection)  
 [创建包和程序](#creating_a_package)  
 [选择分发点](#select_dist_point)  
 [部署包](#deploying_package)  
 [资源](#resources)  
 [疑难解答](#troubleshooting)  
  
<a name="the_deployment_process"></a>   
## 部署过程  
 在设置好支持基础结构之后，可以使用 System Center 2012 Configuration Manager 将 .NET Framework 可再发行组件包部署到网络上的计算机。  构建基础结构涉及创建并定义 5 个主要区域：集合、软件的包和程序、分发点以及部署。  
  
-   **集合**是将 .NET Framework 部署到的 Configuration Manager 资源（用户、用户组或计算机）组。  有关详细信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的集合](http://technet.microsoft.com/library/gg682169.aspx)。  
  
-   **包和程序**通常表示要安装在客户端计算机上的软件应用程序，但它们还可能包含单个文件、更新，甚至是单个命令。  有关详细信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的包和程序](http://technet.microsoft.com/library/gg699369.aspx)。  
  
-   **分发点**是存储在客户端计算机上运行软件所需的文件的 Configuration Manager 站点系统角色。  在 Configuration Manager 客户端收到并处理软件部署时，该客户端会与分发点联系以下载与相应软件关联的内容并开始安装过程。  有关详细信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的内容管理简介](http://technet.microsoft.com/library/gg682083.aspx)。  
  
-   **部署**指示指定目标集合的相应成员安装软件包。  有关详细信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的如何部署应用程序](http://technet.microsoft.com/library/gg682082.aspx)。  
  
> [!IMPORTANT]
>  本主题中的过程包含用于创建及部署包和程序的典型设置，可能不包含所有可能的设置。  有关其他 Configuration Manager 部署选项，请参见 [Configuration Manager 文档库](http://technet.microsoft.com/library/gg682041.aspx)。  
  
<a name="deploying_in_a_test_environment"></a>   
## 部署 .NET Framework  
 你可以使用 System Center 2012 Configuration Manager 部署 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 的无提示安装（用户不与安装过程进行交互）。  请执行这些步骤：  
  
1.  [创建集合](#creating_a_collection)。  
  
2.  [为 .NET Framework 可再发行组件创建包和程序](#creating_a_package)。  
  
3.  [选择分发点](#select_dist_point)。  
  
4.  [部署包](#deploying_package)。  
  
<a name="creating_a_collection"></a>   
### 创建集合  
 在此步骤中，选择包和程序将部署到的计算机，并将这些计算机组合到一个设备集合中。  若要在 Configuration Manager 中创建集合，可使用直接成员身份规则（手动指定集合成员）或查询规则（Configuration Manager 根据你指定的条件确定集合成员）。  有关成员身份规则的更多信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的集合简介](http://technet.microsoft.com/library/gg682177.aspx)。  
  
 创建集合：  
  
1.  在 Configuration Manager 控制台中，选择**“资产和遵从性”**。  
  
2.  在**“资产和遵从性”**工作区中，选择**“设备集合”**。  
  
3.  在**“主页”**选项卡上的**“创建”**组中，选择**“创建设备集合”**。  
  
4.  在**“创建设备集合向导”**的**“常规”**页上，输入集合的名称。  
  
5.  选择**“浏览”**以指定限制集合。  
  
6.  在**“成员身份规则”**页上，选择**“添加规则”**，然后选择**“直接规则”**以打开**“创建直接成员身份规则向导”**。  选择**“下一步”**。  
  
7.  在**“搜索资源”**页上，在**“资源类”**列表中，选择**“系统资源”**。  在**“特性名”**列表中，选择**“名称”**。  在**“值”**字段中，键入 `%`，然后选择**“下一步”**。  
  
8.  在**“选择资源”**页上，选中要将 .NET Framework 部署到的每台计算机所对应的复选框。  选择**“下一步”**，然后完成向导。  
  
9. 在**“创建设备集合向导”**的**“成员身份规则”**页上，选择**“下一步”**，然后完成该向导。  
  
 有关集合的详细信息，请参见 Configuration Manager 文档库中的 [Configuration Manager 中的集合](http://technet.microsoft.com/library/bb693730.aspx)。  
  
<a name="creating_a_package"></a>   
### 为 .NET Framework 可再发行组件包创建包和程序  
 以下步骤为 .NET Framework 可再发行组件手动创建包。  该包包含用于安装 .NET Framework 的指定参数以及要用于将包分发到目标计算机的位置。  
  
 创建包：  
  
1.  在 Configuration Manager 控制台中，选择**“软件库”**。  
  
2.  在**“软件库”**工作区中，展开**“应用程序管理”**，然后选择**“包”**。  
  
3.  在**“主页”**选项卡上的**“创建”**组中，选择**“创建包”**。  
  
4.  在**“创建包和程序向导”**的**“包”**页上，输入以下信息：  
  
    -   名称：`.NET Framework 4.5`  
  
    -   制造商：`Microsoft`  
  
    -   语言：  `简体中文`  
  
5.  选择**“此包包含源文件”**，然后选择**“浏览”**以选择包含 .NET Framework 安装文件的本地或网络文件夹。  在选择该文件夹后，选择**“确定”**，然后选择**“下一步”**。  
  
6.  在向导的**“程序类型”**页上，选择**“标准程序”**，然后选择**“下一步”**。  
  
7.  在**“创建包和程序向导”**的**“程序”**页上，输入以下信息：  
  
    1.  **名称：** `.NET Framework 4.5`  
  
    2.  **命令行：** `dotNetFx45_Full_x86_x64.exe /q /norestart /ChainingPackage ADMINDEPLOYMENT`（这些步骤后面的表中描述了命令行选项）  
  
    3.  **运行：**选择**“隐藏”**。  
  
    4.  **程序可以运行：**选择指定程序可以运行（不管用户是否登录）的选项。  
  
8.  在**“要求”**页上，选择**“下一步”**以接受默认值，然后完成该向导。  
  
 下表描述了步骤 7 中指定的命令行选项。  
  
|选项|描述|  
|--------|--------|  
|**\/q**|设置安静模式。  不需要用户输入，也不显示输出。|  
|**\/norestart**|防止安装程序自动重新启动。  如果你使用此选项，则 Configuration Manager 必须处理计算机重新启动。|  
|**\/chainingpackage** *PackageName*|指定执行链接的包的名称。  该信息与那些注册了 [Microsoft 客户体验改善计划 \(CEIP\)](http://go.microsoft.com/fwlink/p/?LinkId=248244) 的用户的其他安装会话信息一起报告。  如果包名称包含空格，则可以用双引号作为分隔符；例如：**\/chainingpackage "Chaining Product"**。|  
  
 这些步骤创建了一个名为“.NET Framework 4.5”的包。  程序将部署 .NET Framework 4.5 的无提示安装。  在无提示安装中，用户不与安装过程进行交互，并且链接应用程序必须捕获返回代码并处理重新启动操作；请参见 MSDN 库中的[从安装软件包获取进度信息](http://go.microsoft.com/fwlink/?LinkId=179606)。  
  
<a name="select_dist_point"></a>   
### 选择分发点  
 若要通过服务器将包和程序分发到客户端计算机，你必须先指定一个站点系统为分发点，然后将包分发到该分发点。  
  
 使用下列步骤可为你在上一节中创建的 .NET Framework 4.5 包选择分发点：  
  
1.  在 Configuration Manager 控制台中，选择**“软件库”**。  
  
2.  在**“软件库”**工作区中，展开**“应用程序管理”**，然后选择**“包”**。  
  
3.  从包列表中，选择你在上一节中创建的包**“.NET Framework 4.5”**。  
  
4.  在**“主页”**选项卡上的**“部署”**组中，选择**“分发内容”**。  
  
5.  在**“分发内容向导”**的**“常规”**选项卡上，选择**“下一步”**。  
  
6.  在该向导的**“内容目标”**页上，选择**“添加”**，然后选择**“分发点”**。  
  
7.  在**“添加分发点”**对话框中，选择将承载包和程序的分发点，然后选择**“确定”**。  
  
8.  完成该向导。  
  
 包现在包含无提示部署 .NET Framework 4.5 所需的所有信息。  在部署包和程序之前，请确认已将其安装在分发点上；请参见 Configuration Manager 文档库中的 [Configuration Manager 中的内容管理的操作和维护](http://technet.microsoft.com/library/gg712694.aspx#BKMK_MonitorContent)的“监视内容”一节。  
  
<a name="deploying_package"></a>   
### 部署包  
 部署 .NET Framework 4.5 包和程序：  
  
1.  在 Configuration Manager 控制台中，选择**“软件库”**。  
  
2.  在**“软件库”**工作区中，展开**“应用程序管理”**，然后选择**“包”**。  
  
3.  从包列表中，选择你创建的名为**“.NET Framework 4.5”**的包。  
  
4.  在**“主页”**选项卡上的**“部署”**组中，选择**“部署”**。  
  
5.  在**“部署软件向导”**的**“常规”**页上，选择**“浏览”**，然后选择先前创建的集合。  选择**“下一步”**。  
  
6.  在该向导的**“内容”**页上，确认已显示要从其分发软件的点，然后选择**“下一步”**。  
  
7.  在该向导的**“部署设置”**页上，确认**“操作”**已设置为**“安装”**，且**“目标”**设置为**“必需”**。  这可确保软件包将是目标计算机上的必需安装。  选择**“下一步”**。  
  
8.  在该向导的**“计划”**页上，指定希望何时安装 .NET Framework。  你可以选择**“新建”**以指定安装时间，也可以指示在用户登录或注销时安装软件或尽快安装软件。  选择**“下一步”**。  
  
9. 在该向导的**“用户体验”**页上，使用默认值并选择**“下一步”**。  
  
    > [!WARNING]
    >  你的生产环境可能具有需要选择不同的部署计划的策略。  有关这些选项的信息，请参见 TechNet 库中的[播发名称属性：“计划”选项卡](http://technet.microsoft.com/library/bb694016.aspx)。  
  
10. 在该向导的**“分发点”**页上，使用默认值并选择**“下一步”**。  
  
11. 完成该向导。  你可在**“监控”**工作区的**“部署”**节点下监控部署的进度。  
  
 包现在将部署到目标集合，并且将开始 .NET Framework 4.5 的无提示安装。  有关 .NET Framework 4.5 安装错误代码的信息，请参见本主题后面的[返回代码](#return_codes)一节。  
  
<a name="resources"></a>   
## 资源  
 有关用于测试 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 可再发行组件包的部署的基础结构的更多信息，请参见下列资源。  
  
 **Active Directory、DNS 和 DHCP：**  
  
-   [Windows Server 2008 的 Active Directory 域服务](http://technet.microsoft.com/library/dd378891.aspx)  
  
-   [小型网络中的 DNS 的 Windows Server 2008 分步指南](http://www.microsoft.com/download/details.aspx?id=17157)  
  
-   [DNS 服务器](http://technet.microsoft.com/library/cc732997.aspx)  
  
-   [DHCP 服务器](http://technet.microsoft.com/library/cc896553.aspx)  
  
 **SQL Server 2008：**  
  
-   [安装 SQL Server 2008（SQL Server 视频）](http://technet.microsoft.com/library/dd299415.aspx)  
  
-   [面向数据库管理员的 SQL Server 2008 安全概览](http://download.microsoft.com/download/a/c/d/acd8e043-d69b-4f09-bc9e-4168b65aaa71/SQL2008SecurityOverviewforAdmins.docx)  
  
 **System Center 2012 Configuration Manager（既充当管理点又充当分发点）：**  
  
-   [System Center 2012 Configuration Manager 的站点管理](http://technet.microsoft.com/library/gg681983.aspx)  
  
-   [Configuration Manager 单个站点规划和部署](http://technet.microsoft.com/library/bb680961.aspx)  
  
 **适用于 Windows 计算机的 System Center 2012 Configuration Manager 客户端：**  
  
-   [部署适用于 System Center 2012 Configuration Manager 的客户端](http://technet.microsoft.com/library/gg699391.aspx)  
  
<a name="troubleshooting"></a>   
## 疑难解答  
  
### 日志文件位置  
 在 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 安装过程中，会生成下列日志文件：  
  
 %temp%\\Microsoft .NET Framework 4.5\*.txt                                 
 %temp%\\Microsoft .NET Framework 4.5\*.html  
  
 你可以使用[日志收集工具](http://www.microsoft.com/download/details.aspx?id=12493)收集 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 日志文件，并创建可减小这些文件的大小的压缩 cabinet \(.cab\) 文件。  
  
<a name="return_codes"></a>   
### 返回代码  
 下表列出了 [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] 可再发行安装程序中的最常见的返回代码。  所有版本的安装程序的返回代码都是相同的。  
  
 有关指向详细信息的链接，请参见下一节[下载错误代码](#additional_error_codes)。  
  
|返回代码|描述|  
|----------|--------|  
|0|已成功完成安装。|  
|1602|用户已取消安装。|  
|1603|安装期间发生错误。|  
|1641|需要重新启动才能完成安装。  此消息指示安装成功。|  
|3010|需要重新启动才能完成安装。  此消息指示安装成功。|  
|5100|用户计算机不满足系统要求。|  
  
<a name="additional_error_codes"></a>   
### 下载错误代码  
  
-   [后台智能传输服务 \(BITS\) 错误代码](http://msdn.microsoft.com/library/aa362823.aspx)  
  
-   [URL 名字对象错误代码](http://msdn.microsoft.com/library/ms775145.aspx)  
  
-   [WinHttp 错误代码](http://msdn.microsoft.com/library/aa383770.aspx)  
  
 其他错误代码：  
  
-   [Windows 安装程序错误代码](http://msdn.microsoft.com/library/aa368542.aspx)  
  
-   [Windows 更新代理结果代码](http://technet.microsoft.com/library/cc720442.aspx)  
  
## 请参阅  
 [部署指南（针对开发人员）](../../../docs/framework/deployment/deployment-guide-for-developers.md)   
 [系统需求](../../../docs/framework/get-started/system-requirements.md)