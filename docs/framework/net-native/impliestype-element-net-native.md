---
title: "&lt;暗示类型&gt;元素 (.NET Native) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 3abd2071-0f28-40ba-b9a0-d52bd94cd2f6
caps.latest.revision: 7
author: "rpetrusha"
ms.author: "ronpet"
manager: "wpickett"
caps.handback.revision: 7
---
# &lt;暗示类型&gt;元素 (.NET Native)
如果该策略已应用到该包含类型或方法，将该策略应用到一个类型。  
  
## <a name="syntax"></a>语法  
  
```vb  
  
<ImpliesType Name="type_name"  
             Activate="policy_type"  
             Browse="policy_type"  
             Dynamic="policy_type"  
             Serialize="policy_type"   
             DataContractSerializer="policy_setting"  
             DataContractJsonSerializer="policy_setting"  
             XmlSerializer="policy_setting"  
             MarshalObject="policy_setting"  
             MarshalDelegate="policy_setting"  
             MarshalStructure="policy_setting" />  
  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|特性|特性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定类型名称。|  
|`Activate`|映像|可选特性。 控制运行时对构造函数的访问，以启用实例激活。|  
|`Browse`|映像|可选特性。 控制对有关程序元素信息的查询，但并不启用任何运行时访问。|  
|`Dynamic`|映像|可选特性。 控制运行时对所有类型成员的访问，包括构造函数、方法、字段、属性和事件，以启用动态编程。|  
|`Serialize`|序列化|可选特性。 控制运行时对构造函数、字段和属性的访问，使类型实例得到序列化和反序列化处理，这是通过库进行的，例如 Newtonsoft JSON 序列化程序。|  
|`DataContractSerializer`|序列化|可选特性。 控制使用的序列化策略<xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=fullName>类。|  
|`DataContractJsonSerializer`|序列化|可选特性。 控制使用的 JSON 序列化策略<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=fullName>类。|  
|`XmlSerializer`|序列化|可选特性。 控制使用的 XML 序列化策略<xref:System.Xml.Serialization.XmlSerializer?displayProperty=fullName>类。|  
|`MarshalObject`|Interop|可选特性。 控制封送引用类型到 Windows 运行时和 COM 的策略。|  
|`MarshalDelegate`|Interop|可选特性。 控制将委托类型作为函数指针封送到本机代码的策略。|  
|`MarshalStructure`|Interop|可选特性。 控制封送处理值类型到本机代码的策略。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|*类型 _ 名称*|类型名称。 如果该类型由`<ImpliesType>`元素位于相同的命名空间及其包含`<Type>`元素，*类型 _ 名称*可以包括而无需其命名空间的类型的名称。 否则为*类型 _ 名称*必须包含完全限定的类型名称。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|说明|  
|-----------|-----------------|  
|*policy_setting*|该设置将应用到这种策略类型。 可能值为 `All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal` 以及 `Required All`。 有关详细信息，请参阅[运行时指令策略设置](../../../docs/framework/net-native/runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[<>\>](../../../docs/framework/net-native/type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
|[<>\>](../../../docs/framework/net-native/typeinstantiation-element-net-native.md)|将反射策略应用到一种构造泛型类型及其所有成员。|  
|[<>\>](../../../docs/framework/net-native/method-element-net-native.md)|将反射策略应用到一个方法。|  
  
## <a name="remarks"></a>备注  
 该 `<ImpliesType>` 元素主要是供库使用的。 它讨论的是以下情景：  
  
-   如果一个例程需要反射到一个类型，它必然也要反射到另一种类型。  
  
-   否则，第二种类型的暗示的实例化元数据就不可用，因为静态分析没有指示这一步骤是必须的。  
  
 最常见的两种类型是附带共享类型参数的泛型实例化。  
  
 对 `<ImpliesType>` 元素的定义是以一个假设为基础的，即如果需要反射到父元素指定的类型上暗示着也需要反射到 `<ImpliesType>` 元素指定的类型上。 例如，以下反射指令应用到两种类型，`Explicit<T>` 和 `Implicit<T>`。  
  
```xml  
  
<Type Name=”Explicit{ET}”>  
    <ImpliesType Name=”Implicit{ET}” Dynamic=”Required Public” />  
</Type>  
  
```  
  
 这个指令是不起作用的，除非 `Explicit` 的一个实例化具有一个已定义的 `Dynamic` 策略设置。 例如，如果对于 `Explicit<Int32>` 情况如此，`Implicit<Int32>` 会同其根公共成员一起被实例化，并且它们的元数据就能由动态编程访问。  
  
 以下是一个至少可以应用到序列化程序的实际实例。 指令获得到的要求，反射到的内容类型化为`IList<`*内容*`>`也需要反射到相应`List<`*内容*`>`类型，而不需要任何每个应用程序注释。  
  
```xml  
  
<Type Name=”System.Collections.Generic.IList{T}”>  
   <ImpliesType Name=”System.Collections.Generic.List{T}” Serialize=”Public” />  
</Type>  
  
```  
  
 `<ImpliesType>` 元素也可能出现在 `<Method>` 元素内部，因为在某些情况下泛型方法暗示着反射到了一个类型实例化上。 例如，假设一个泛型方法`IEnumerable<T> MakeEnumerable<T>(string` `spelling``, T` `defaultValue``)`的给定的库可以动态访问以及相关<xref:System.Collections.Generic.List%601>和<xref:System.Array>类型。 这可以表示为：  
  
```xml  
  
<Type Name=”MyType”>  
    <Method Name=”MakeEnumerable{T}” Signature=”(System.String, T)” Dynamic=”Included”>  
        <ImpliesType Name=”T[]” Dynamic=”Public” />  
        <ImpliesType Name=”System.Collections.Generic.List{T}” Dynamic=”Public”>  
    </Method>  
</Type>  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [运行时指令 (rd.xml) 配置文件引用](../../../docs/framework/net-native/runtime-directives-rd-xml-configuration-file-reference.md)   
 [运行时指令元素](../../../docs/framework/net-native/runtime-directive-elements.md)   
 [运行时指令策略设置](../../../docs/framework/net-native/runtime-directive-policy-settings.md)