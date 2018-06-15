---
title: Azure PublicIpAddressCombo UI öğesi | Microsoft Docs
description: Azure portalı için Microsoft.Network.PublicIpAddressCombo kullanıcı Arabirimi öğesi açıklar.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: tomfitz
ms.openlocfilehash: bf0ef5be609fba14ab12e1e6f9f97bc63f032aae
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34260569"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI öğesi
Yeni veya var olan ortak IP adresi seçme denetimlerini grubudur.

## <a name="ui-sample"></a>Kullanıcı Arabirimi örneği
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Kullanıcının ortak IP adresi için ' None' seçerse, etki alanı adı etiketi metin kutusu gizlenir.
- Kullanıcı var olan bir ortak IP adresi seçer, etki alanı adı etiketi metin kutusu devre dışı bırakılır. Seçili IP adresinin etki alanı adı etiketi değeri olduğu.
- Otomatik olarak seçilen konum temelinde etki alanı adı soneki (örneğin, westus.cloudapp.azure.com) güncelleştirmeler.

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false,
    "zone": 3
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Varsa `constraints.required.domainNameLabel` ayarlanır **doğru**, kullanıcı, yeni bir ortak IP adresi oluştururken, bir etki alanı adı etiketi sağlamanız gerekir. Bir etiketi olmayan olmadan seçime uygun varolan ortak IP adresleri.
- Varsa `options.hideNone` ayarlanır **true**, ardından belirleme seçeneği **hiçbiri** için genel IP adresi gizlenir. Varsayılan değer **false**.
- Varsa `options.hideDomainNameLabel` ayarlanır **doğru**, etki alanı adı etiketi için metin kutusu gizli sonra. Varsayılan değer **false**.
- Varsa `options.hideExisting` kullanıcı mevcut bir ortak IP adresini seçebilir değil sonra true olur. Varsayılan değer **false**.
- İçin `zone`, yalnızca genel IP adresi için belirtilen bölge veya bölge dayanıklı genel IP adresleri kullanılabilir.

## <a name="sample-output"></a>Örnek çıktı
Kullanıcının ortak IP adresi seçerse, aşağıdaki çıkış bekleniyor:
```json
{
  "newOrExistingOrNone": "none"
}
```

Kullanıcı yeni veya var olan bir IP adresi seçerse, aşağıdaki çıkış bekleniyor:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "newOrExistingOrNone": "new"
}
```
- Zaman `options.hideNone` olarak belirtilen **true**, `newOrExistingOrNone` yalnızca bir değeri olur **yeni** veya **varolan**.
- Zaman `options.hideDomainNameLabel` olarak belirtilen **true**, `domainNameLabel` bildirilmedi.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
