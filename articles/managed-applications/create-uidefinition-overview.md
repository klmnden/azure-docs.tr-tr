---
title: Kullanıcı Arabirimi oluşturma anlamak tanımı Azure yönetilen uygulamalar | Microsoft Docs
description: Azure yönetilen uygulamalar için kullanıcı Arabirimi tanımları oluşturulacağını açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/26/2019
ms.author: tomfitz
ms.openlocfilehash: 3d0a6d97440404904c041369a4631fdd3fb618b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66257558"
---
# <a name="create-azure-portal-user-interface-for-your-managed-application"></a>Yönetilen uygulamanız için Azure portal kullanıcı arabirimi oluşturma
Bu belge, createUiDefinition.json dosyasının temel kavramlar açıklanmaktadır. Azure portalı, yönetilen bir uygulama oluşturmak için kullanılan kullanıcı arabirimi oluşturmak için bu dosyayı kullanır.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Bir CreateUiDefinition her zaman üç özellik içerir: 

* İşleyici
* version
* parametreler

Yönetilen uygulamalar için her zaman işleyicisi olmalıdır `Microsoft.Compute.MultiVm`, ve desteklenen son sürümü `0.1.2-preview`.

Parameters özelliği şema sürümü ve belirtilen işleyici birleşimi bağlıdır. Yönetilen uygulamalar için desteklenen özelliklerdir `basics`, `steps`, ve `outputs`. Temel kavramları ve adımları özelliklerini içeren _öğeleri_ - gibi metin kutuları ve bırakmalar - Azure Portalı'nda görüntülenecek. Outputs özelliğini, Azure Resource Manager dağıtım şablonu parametrelerini için belirtilen öğe çıkış değerlerini eşleştirmek için kullanılır.

Dahil olmak üzere `$schema` önerilir ancak isteğe bağlıdır. Belirtilmişse, değerin için `version` içinde eşleşmelidir `$schema` URI.

UI tanımı korumalı alan oluşturun ve kullanıcı Arabirimi tanım Önizleme için kullanabileceğiniz veya JSON Düzenleyicisi, UI tanımı oluşturmak için kullanabilirsiniz. Korumalı alan hakkında daha fazla bilgi için bkz: [Azure yönetilen uygulamalar için portal Arabirimi Test](test-createuidefinition.md).

## <a name="basics"></a>Temel Bilgiler
Temel bilgileri her zaman ilk adımı Azure portalında dosyasını ayrıştırırken oluşturulan Sihirbazı adımdır. Belirtilen öğelerin görüntüleme yanı sıra `basics`, portal abonelik, kaynak grubu ve dağıtım konumunu seçmek kullanıcıları için öğeleri ekler. Genellikle, bu adımda bir küme veya yönetici kimlik bilgileri adı gibi dağıtım genelinde parametreleri için sorgu öğeleri gitmeniz gerekir.

Bir öğenin davranışı kullanıcının aboneliği, kaynak grubu veya konum bağlıysa, bu öğenin temel bilgileri kullanılamaz. Örneğin, **Microsoft.Compute.SizeSelector** kullanıcının abonelik ve konum kullanılabilir boyutların listesi belirlemek için bağlıdır. Bu nedenle, **Microsoft.Compute.SizeSelector** adımlarda yalnızca kullanılabilir. Genellikle, yalnızca öğeleri **Microsoft.Common** ad alanı, temel bilgileri kullanılabilir. Ancak diğer ad alanlarında bazı öğeleri (gibi **Microsoft.Compute.Credentials**), kullanıcının bağlamına bağlı olmayan, yine de izin verilir.

## <a name="steps"></a>Adımlar
Adımları özelliği, her biri bir veya daha fazla öğe içeren temel bilgileri sonra görüntülemek için sıfır veya daha fazla ek adımlar içerebilir. Rol veya dağıtılan uygulamayı katmanının başına adımları eklemeyi düşünün. Örneğin, bir kümedeki ana düğümleri için girişler için bir adım ve çalışan düğümleri için bir adım ekleyin.

## <a name="outputs"></a>Çıkışlar
Azure portalını kullanır `outputs` öğeleri eşlemek için özellik `basics` ve `steps` Azure Resource Manager dağıtım şablonu parametrelerini için. Bu sözlüğün anahtarlarını şablon parametrelerinin adları ve değerleri başvurulan öğelerin çıkış nesnelerden özellikleridir.

Yönetilen uygulama kaynak adı ayarlamak için adlandırılmış bir değer eklemelisiniz `applicationResourceName` outputs özelliğini de. Bu değer ayarlamazsanız, uygulama adı için bir GUID atar. Bir ad kullanıcıdan isteyen kullanıcı arabiriminde bir metin kutusu ekleyebilirsiniz.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="functions"></a>İşlevler
Benzer şekilde şablon işlevleri, Azure Resource Manager (hem de söz dizimi ve İşlevler), CreateUiDefinition işlevler öğeleri girişleri ve çıkışları ve koşullar gibi özellikler ile çalışmak için sağlar.

## <a name="next-steps"></a>Sonraki adımlar
CreateUiDefinition.json dosya, basit bir şeması vardır. Tüm desteklenen öğeleri ve işlevleri gerçek derinliğini gelir. Bu öğeler daha ayrıntılı açıklanmıştır:

- [Öğeleri](create-uidefinition-elements.md)
- [İşlevler](create-uidefinition-functions.md)

CreateUiDefinition için geçerli bir JSON şeması şuradan ulaşabilirsiniz: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Bir örnek kullanıcı arabirimi dosyasına bakın [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
