---
title: Azure PublicIpAddressCombo UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Network.PublicIpAddressCombo UI öğesi açıklar.
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
ms.date: 06/28/2018
ms.author: tomfitz
ms.openlocfilehash: c3e8c99f6648f0f4927140f3215978566afb9eb8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251110"
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo kullanıcı Arabirimi öğesi
Yeni veya var olan genel IP adresi seçme denetimlerini grubudur.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Genel IP adresi için ' None' kullanıcının seçtiği etki alanı adı etiketi metin kutusu gizlenir.
- Kullanıcı var olan bir genel IP adresini seçerse, etki alanı adı etiketi metin kutusu devre dışı bırakıldı. Seçili IP adresi, etki alanı ad etiketi kendi değerdir.
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
- Varsa `constraints.required.domainNameLabel` ayarlanır **true**, kullanıcı, yeni bir ortak IP adresi oluştururken bir etki alanı adı etiketi sağlamanız gerekir. Bir etiketi olmayan olmadan seçilebilir var olan bir genel IP adresleri.
- Varsa `options.hideNone` ayarlanır **true**, ardından seçme seçeneği **hiçbiri** için genel IP adresi gizlidir. Varsayılan değer **false**.
- Varsa `options.hideDomainNameLabel` ayarlanır **true**, sonra etki alanı adı etiketi için metin kutusu gizlenir. Varsayılan değer **false**.
- Varsa `options.hideExisting` kullanıcı var olan bir genel IP adresi seçin sağlayamadığı sonra true olur. Varsayılan değer **false**.
- İçin `zone`, yalnızca genel IP adresleri için belirtilen bölgesi veya bölge dayanıklı genel IP adresleri kullanılabilir.

## <a name="sample-output"></a>Örnek çıktı
Genel IP adresi yok kullanıcının seçtiği denetimi aşağıdaki çıktı döndürür:

```json
{
  "newOrExistingOrNone": "none"
}
```

Yeni veya mevcut bir IP adresi kullanıcının seçtiği denetimi aşağıdaki çıktı döndürür:

```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "sku": "Basic",
  "newOrExistingOrNone": "new"
}
```

- Zaman `options.hideNone` olarak belirtilen **true**, `newOrExistingOrNone` yalnızca değerine sahip **yeni** veya **mevcut**.
- Zaman `options.hideDomainNameLabel` olarak belirtilen **true**, `domainNameLabel` bildirilmedi.

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
