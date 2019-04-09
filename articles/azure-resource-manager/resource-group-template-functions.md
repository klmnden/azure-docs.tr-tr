---
title: Resource Manager şablonu işlevleri | Microsoft Docs
description: Değerleri almak, dizeler ve sayısal türler ile çalışma ve dağıtım bilgilerini almak için bir Azure Resource Manager şablonunda kullanmak için işlevleri açıklar.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/08/2019
ms.author: tomfitz
ms.openlocfilehash: b5a1f12a877008a3ce2ff7bd9635b9ed47b379f7
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59280324"
---
# <a name="azure-resource-manager-template-functions"></a>Azure Resource Manager şablonu işlevleri
Bu makalede bir Azure Resource Manager şablonunda kullanabileceğiniz işlevleri açıklanmaktadır. Şablonunuzda işlevleri kullanma hakkında daha fazla bilgi için bkz: [şablon söz dizimi](resource-group-authoring-templates.md#syntax).

Kendi işlev oluşturmak için bkz [kullanıcı tanımlı işlevleri](resource-group-authoring-templates.md#functions).

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
Resource Manager, nesneleri ve dizileri ile çalışmak için çeşitli işlevler sunar.

* [array](resource-group-template-functions-array.md#array)
* [birleşim](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [içerir](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [boş](resource-group-template-functions-array.md#empty)
* [ilk](resource-group-template-functions-array.md#first)
* [kesişimi](resource-group-template-functions-array.md#intersection)
* [json](resource-group-template-functions-array.md#json)
* [Son](resource-group-template-functions-array.md#last)
* [Uzunluğu](resource-group-template-functions-array.md#length)
* [dk](resource-group-template-functions-array.md#min)
* [en çok](resource-group-template-functions-array.md#max)
* [Aralığı](resource-group-template-functions-array.md#range)
* [atla](resource-group-template-functions-array.md#skip)
* [sınav zamanı](resource-group-template-functions-array.md#take)
* [birleşim](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Karşılaştırma işlevleri
Resource Manager şablonlarınızı karşılaştırmaları yapmak için çeşitli işlevler sunar.

* [şuna eşittir:](resource-group-template-functions-comparison.md#equals)
* [daha az](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [daha büyük](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Dağıtım değeri işlevleri
Resource Manager şablonu ve dağıtımıyla ilgili değerleri bölümlerden değerleri almak için aşağıdaki işlevleri sunar:

* [dağıtım](resource-group-template-functions-deployment.md#deployment)
* [parametreler](resource-group-template-functions-deployment.md#parameters)
* [Değişkenleri](resource-group-template-functions-deployment.md#variables)

<a id="and" />
<a id="bool" />
<a id="if" />
<a id="not" />
<a id="or" />

## <a name="logical-functions"></a>Mantıksal işlevler
Resource Manager, mantıksal koşul ile çalışmak için aşağıdaki işlevleri sunar:

* [ve](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [değil](resource-group-template-functions-logical.md#not)
* [or](resource-group-template-functions-logical.md#or)

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

## <a name="numeric-functions"></a>Sayısal işlevler
Resource Manager, tamsayı ile çalışmak için aşağıdaki işlevleri sunar:

* [add](resource-group-template-functions-numeric.md#add)
* [Copyındex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [float](resource-group-template-functions-numeric.md#float)
* [int](resource-group-template-functions-numeric.md#int)
* [dk](resource-group-template-functions-numeric.md#min)
* [en çok](resource-group-template-functions-numeric.md#max)
* [mod](resource-group-template-functions-numeric.md#mod)
* [mul](resource-group-template-functions-numeric.md#mul)
* [alt](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Kaynak işlevleri
Resource Manager kaynak değerlerini almak için aşağıdaki işlevleri sunar:

* [listAccountSas](resource-group-template-functions-resource.md#list)
* [Listkeys'i](resource-group-template-functions-resource.md#listkeys)
* [listSecrets](resource-group-template-functions-resource.md#list)
* [Liste *](resource-group-template-functions-resource.md#list)
* [sağlayıcıları](resource-group-template-functions-resource.md#providers)
* [Başvuru](resource-group-template-functions-resource.md#reference)
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
Resource Manager, dizeleri ile çalışmak için aşağıdaki işlevleri sunar:

* [Base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [içerir](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [boş](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [ilk](resource-group-template-functions-string.md#first)
* [biçim](resource-group-template-functions-string.md#format)
* [GUID](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [Son](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [Uzunluğu](resource-group-template-functions-string.md#length)
* [newGuid](resource-group-template-functions-string.md#newguid)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [Değiştir](resource-group-template-functions-string.md#replace)
* [atla](resource-group-template-functions-string.md#skip)
* [split](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [string](resource-group-template-functions-string.md#string)
* [alt dize](resource-group-template-functions-string.md#substring)
* [sınav zamanı](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [Kırpma](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [uri](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)
* [utcNow](resource-group-template-functions-string.md#utcnow)

## <a name="next-steps"></a>Sonraki adımlar
* Bir Azure Resource Manager şablonu olarak bölümlerde açıklaması için bkz: [Azure Resource Manager şablonları yazma](resource-group-authoring-templates.md)
* Birden fazla şablon birleştirmek için bkz: [Azure Resource Manager ile bağlı şablonları kullanma](resource-group-linked-templates.md)
* Belirtilen sayıda yineleme için bir kaynak türünü oluştururken bkz [Azure Resource Manager'da kaynakları birden çok örneğini oluşturma](resource-group-create-multiple.md)
* Oluşturduğunuz bir şablonu dağıtmayı öğrenmek için bkz [Azure Resource Manager şablonu ile uygulama dağıtma](resource-group-template-deploy.md)
