---
title: "Resource Manager şablonu işlevleri | Microsoft Docs"
description: "Değerleri almak, dizeler ve sayısal türler ile çalışır ve dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanılacak işlevleri açıklanmaktadır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/18/2017
ms.author: tomfitz
ms.openlocfilehash: 54580abdca8b6be10576cf74ad23e8ff2665341c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="azure-resource-manager-template-functions"></a>Azure Resource Manager şablonu işlevleri
Bu makalede Azure Resource Manager şablonunda kullanabileceğiniz işlevleri açıklanmaktadır.

Köşeli ayraç içinde yazarak, şablonlarda işlevler eklemek: `[` ve `]`sırasıyla. Dağıtım sırasında değerlendirilen ifade. Bir dize yazılmış olsa da, bir dizi, nesne veya tamsayı gibi farklı bir JSON türünde ifade değerlendirme sonucu olabilir. JavaScript'te, işlev çağrıları olarak biçimlendirilmiş gibi yalnızca `functionName(arg1,arg2,arg3)`. Nokta ve [dizin] işleçleri kullanarak özellikleri başvuru.

Bir şablon ifadesi 24.576 karakterden uzun olamaz.

Şablon işlevleri ve bunların parametrelerini büyük/küçük harf duyarlıdır. Örneğin, Resource Manager çözümler **variables('var1')** ve **VARIABLES('VAR1')** aynı olarak. Değerlendirildiğinde, işlevi açıkça durumda (örneğin, toUpper veya toLower) değiştirir sürece işlevi durum korur. Belirli kaynak türlerine işlevleri nasıl değerlendirildiği yedeklemiş servis talebi gereksinimleri olabilir.

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="json" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a>Dizi ve nesne işlevleri
Kaynak Yöneticisi, nesneler ve diziler ile çalışmak için birkaç işlevleri sağlar.

* [Dizi](resource-group-template-functions-array.md#array)
* [coalesce](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [içerir](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [empty](resource-group-template-functions-array.md#empty)
* [first](resource-group-template-functions-array.md#first)
* [intersection](resource-group-template-functions-array.md#intersection)
* [json](resource-group-template-functions-array.md#json)
* [Son](resource-group-template-functions-array.md#last)
* [uzunluğu](resource-group-template-functions-array.md#length)
* [min](resource-group-template-functions-array.md#min)
* [max](resource-group-template-functions-array.md#max)
* [Aralık](resource-group-template-functions-array.md#range)
* [skip](resource-group-template-functions-array.md#skip)
* [Al](resource-group-template-functions-array.md#take)
* [birleşim](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Karşılaştırma işlevleri
Resource Manager şablonlarınızı içinde karşılaştırmaları yapmak için çeşitli işlevleri sağlar.

* [eşittir](resource-group-template-functions-comparison.md#equals)
* [daha az](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [greater](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Dağıtım değer işlevleri
Resource Manager şablonu ve dağıtımıyla ilgili değerleri bölümlerden değerleri almak için aşağıdaki işlevleri sunar:

* [deployment](resource-group-template-functions-deployment.md#deployment)
* [parametreler](resource-group-template-functions-deployment.md#parameters)
* [değişkenleri](resource-group-template-functions-deployment.md#variables)

<a id="and" />
<a id="bool" />
<a id="if" />
<a id="not" />
<a id="or" />

## <a name="logical-functions"></a>Mantıksal işlevleri
Resource Manager mantıksal koşulları ile çalışmak için aşağıdaki işlevleri sunar:

* [Ve](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [not](resource-group-template-functions-logical.md#not)
* [Veya](resource-group-template-functions-logical.md#or)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="numeric-functions"></a>Sayısal İşlevler
Resource Manager tamsayılar ile çalışmak için aşağıdaki işlevleri sunar:

* [add](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [Kayan nokta](resource-group-template-functions-numeric.md#float)
* [Int](resource-group-template-functions-numeric.md#int)
* [min](resource-group-template-functions-numeric.md#min)
* [max](resource-group-template-functions-numeric.md#max)
* [mod](resource-group-template-functions-numeric.md#mod)
* [mul](resource-group-template-functions-numeric.md#mul)
* [sub](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Kaynak işlevleri
Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [listKeys ve liste {Value}](resource-group-template-functions-resource.md#listkeys)
* [sağlayıcıları](resource-group-template-functions-resource.md#providers)
* [reference](resource-group-template-functions-resource.md#reference)
* [resourceGroup](resource-group-template-functions-resource.md#resourcegroup)
* [resourceId](resource-group-template-functions-resource.md#resourceid)
* [aboneliği](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="guid" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a>Dize işlevleri
Resource Manager dizelerle çalışmak için aşağıdaki işlevleri sunar:

* [base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [içerir](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [empty](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [first](resource-group-template-functions-string.md#first)
* [GUID](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [Son](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [uzunluğu](resource-group-template-functions-string.md#length)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [Değiştir](resource-group-template-functions-string.md#replace)
* [skip](resource-group-template-functions-string.md#skip)
* [split](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [Dize](resource-group-template-functions-string.md#string)
* [substring](resource-group-template-functions-string.md#substring)
* [Al](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [trim](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [uri](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md)
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md)
* Belirtilen sayıda yinelemek için kaynak türünü oluştururken, bkz: [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md)
* Oluşturduğunuz şablon dağıtma hakkında bilgi için bkz: [Azure Resource Manager şablonu ile bir uygulamayı dağıtma](resource-group-template-deploy.md)
