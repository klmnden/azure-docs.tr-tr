---
title: Azure kullanıcı Arabirimi tanım öğesi oluşturun | Microsoft Docs
description: Azure portalı kullanıcı Arabirimi tanımları oluştururken kullanılacak öğeleri açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: tomfitz
ms.openlocfilehash: 41a583a77f85bb1524112fa20d9098e18bc4f431
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60587947"
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition öğeleri
Bu makalede, şema ve bir CreateUiDefinition desteklenen tüm öğelerin özelliklerini açıklar. 

## <a name="schema"></a>Şema

Öğelerin çoğu Şeması aşağıdaki gibidir:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Provide a descriptive name.",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| name | Evet | Bir öğenin belirli bir örneğine başvurmak için iç tanımlayıcı. Öğe adı, en yaygın kullanım bulunduğu `outputs`, burada belirtilen öğelerin çıkış değerleri şablon parametreleri eşlenir. Ayrıca, bir öğeye çıkış değeri bağlamak için kullanabilirsiniz `defaultValue` başka bir öğe. |
| type | Evet | Öğe için işlenecek UI denetimi. Desteklenen türler bir listesi için bkz. [öğeleri](#elements). |
| label | Evet | Öğenin görünen metin. Değer, birden çok dizeyi içeren bir nesne olabilir. Bu nedenle bazı öğe türleri, birden çok etiket içerir. |
| defaultValue | Hayır | Öğesinin varsayılan değeri. Değer, bir nesne olabilir. Bu nedenle bazı öğe türü karmaşık varsayılan değerleri destekler. |
| Araç İpucu | Hayır | Öğenin araç ipucunda görüntülenecek metin. Benzer şekilde `label`, bazı öğeleri birden çok araç ipucu dizeleri destekler. Satır içi bağlantıları, Markdown söz dizimi kullanılarak eklenebilir.
| constraints | Hayır | Öğe doğrulama davranışını özelleştirmek için kullanılan bir veya daha fazla özellikleri. Kısıtlamalar için desteklenen özellikler öğesi türüne göre değişir. Bazı öğe türleri doğrulama davranışını özelleştirmesini desteklemiyor ve bu nedenle hiçbir kısıtlamaları özelliğine sahiptir. |
| options | Hayır | Öğe davranışını özelleştirmek ek özellikler. Benzer şekilde `constraints`, desteklenen özellikler öğesi türüne göre farklılık gösterir. |
| Görünür | Hayır | Öğenin görüntülenip görüntülenmeyeceğini belirtir. Varsa `true`, öğeyi ve geçerli alt öğelerini görüntülenir. Varsayılan değer `true` şeklindedir. Kullanım [mantıksal işlevler](create-uidefinition-functions.md#logical-functions) dinamik olarak bu özelliğin değerini denetlemek için.

## <a name="elements"></a>Öğeler

Belgeler her öğe içeren bir UI örnek için şema öğesi (genellikle ilgili doğrulama ve desteklenen özelleştirme) ve örnek çıktı davranışını açıklamalar.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.InfoBox](microsoft-common-infobox.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBlock](microsoft-common-textblock.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Sonraki adımlar
UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
