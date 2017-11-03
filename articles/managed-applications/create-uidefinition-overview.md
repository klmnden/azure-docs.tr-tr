---
title: "Kullanıcı Arabirimi oluşturma anlamak tanımı Azure için yönetilen uygulamalar | Microsoft Docs"
description: "Azure yönetilen uygulamaları için kullanıcı Arabirimi tanımları oluşturmayı açıklar"
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: d8f04d8ed2e56cecb1b7a850bed55a02a9492bb5
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="create-azure-portal-user-interface-for-your-managed-application"></a>Yönetilen uygulamanız için Azure portal kullanıcı arabirimi oluşturma
Bu belge createUiDefinition.json dosyasının temel kavramları tanıtır. Azure portal, yönetilen bir uygulama oluşturmak için kullanılan kullanıcı arabirimi oluşturmak için bu dosyayı kullanır.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Bir CreateUiDefinition her zaman üç özellikleri içerir: 

* işleyici
* Sürüm
* parametreler

Yönetilen uygulamalar için işleyici her zaman olmalıdır `Microsoft.Compute.MultiVm`, ve en son desteklenen sürümünü `0.1.2-preview`.

Parameters özelliği şema sürümü ve belirtilen işleyici birleşimi bağlıdır. Yönetilen uygulamalar için desteklenen özelliklerdir `basics`, `steps`, ve `outputs`. Temel kavramları ve adımları özelliklerini içeren _öğeleri_ - gibi metin kutuları ve bırakmalar - Azure portalında görüntülenecek. Çıktılar özelliğini Azure Resource Manager dağıtım şablonu parametreleri için belirtilen öğelerinin çıkış değerlerini eşlemek için kullanılır.

Dahil olmak üzere `$schema` önerilir ancak isteğe bağlıdır. Belirtilmişse, değerin için `version` içinde eşleşmelidir `$schema` URI.

## <a name="basics"></a>Temel Bilgiler
Temel bilgileri her zaman Azure portalı dosyasını ayrıştırırken oluşturulan Sihirbazı'nın ilk adımı adımdır. Belirtilen öğeleri görüntüleme yanı sıra `basics`, portalı abonelik, kaynak grubu ve dağıtımı için konum seçmek kullanıcıları için öğeleri yerleştirir. Genellikle, bir küme veya yönetici kimlik bilgileri adı gibi dağıtım çapında parametreler için sorgu öğeleri bu adımı tamamlamalıdır.

Bir öğenin davranışı kullanıcının abonelik, kaynak grubu veya konum bağımlı olması durumunda, bu öğenin temel kullanılamaz. Örneğin, **Microsoft.Compute.SizeSelector** kullanıcının abonelik ve kullanılabilir boyutların listesi belirlemek için konum bağlıdır. Bu nedenle, **Microsoft.Compute.SizeSelector** adımlarda yalnızca kullanılabilir. Genellikle, yalnızca öğeleri **Microsoft.Common** ad alanı temelleri kullanılabilir. Ancak bazı öğeler diğer ad (gibi **Microsoft.Compute.Credentials**), kullanıcının içeriğine bağlı verme, hala izin verilir.

## <a name="steps"></a>Adımlar
Adımları özelliği her biri bir veya daha fazla öğe içeren temel bilgileri sonra görüntülemek için sıfır veya daha fazla ek adımlar içerebilir. Rol veya dağıtılan uygulama katmanı başına adımları eklemeyi düşünün. Örneğin, bir adım ana düğüm girdileri için ve çalışan düğümleri için bir adım bir kümede ekleyin.

## <a name="outputs"></a>Çıkışları
Azure Portalı'nı kullanan `outputs` öğelerinden eşlemek için özellik `basics` ve `steps` Azure Resource Manager dağıtım şablonu parametreleri. Bu sözlüğün anahtarlarını şablon parametrelerinin adları ve değerleri başvurulan öğelerin çıkış nesnelerden özellikleridir.

## <a name="functions"></a>İşlevler
Şablon işlevleri Azure Kaynak Yöneticisi'nde (hem sözdizimi ve işlevselliğinde) benzeyen, CreateUiDefinition işlevleri öğeleri girişleri ve çıkışları yanı koşulları gibi özellikler ile çalışmak için sağlar.

## <a name="next-steps"></a>Sonraki adımlar
CreateUiDefinition.json dosya basit bir şeması vardır. Gerçek derinliği tüm desteklenen öğeleri ve işlevleri gelir. Bu öğeler daha ayrıntılı açıklanmıştır:

- [Öğeleri](create-uidefinition-elements.md)
- [İşlevler](create-uidefinition-functions.md)

CreateUiDefinition için geçerli bir JSON şeması burada bulunur: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json. 

Sonraki sürümlerinde aynı konumda kullanılabilir. Değiştir `0.1.2-preview` URL'sinin ve `version` kullanmayı düşündüğünüz sürüm tanıtıcısını değerle. Şu anda desteklenen sürüm tanımlayıcılardır `0.0.1-preview`, `0.1.0-preview`, `0.1.1-preview`, ve `0.1.2-preview`.