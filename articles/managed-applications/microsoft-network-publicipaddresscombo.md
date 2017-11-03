---
title: "Azure yönetilen uygulama PublicIpAddressCombo UI öğesi | Microsoft Docs"
description: "Azure yönetilen uygulamalar için Microsoft.Network.PublicIpAddressCombo kullanıcı Arabirimi öğesi açıklar"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 94f1f64a57784254912cb6cf568fafc15c8e74ec
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI öğesi
Yeni veya var olan ortak IP adresi seçme denetimlerini grubudur. Bu öğe kullandığınız zaman [yönetilen bir Azure uygulama oluşturmaya](publish-service-catalog-app.md).

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
    "domainNameLabel": "foobar"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Varsa `constraints.required.domainNameLabel` ayarlanır **doğru**, kullanıcı, yeni bir ortak IP adresi oluştururken, bir etki alanı adı etiketi sağlamanız gerekir. Bir etiketi olmayan olmadan seçime uygun varolan ortak IP adresleri.
- Varsa `options.hideNone` ayarlanır **true**, ardından belirleme seçeneği **hiçbiri** için genel IP adresi gizlenir. Varsayılan değer **false**.
- Varsa `options.hideDomainNameLabel` ayarlanır **doğru**, etki alanı adı etiketi için metin kutusu gizli sonra. Varsayılan değer **false**.
- Varsa `options.hideExisting` kullanıcı mevcut bir ortak IP adresini seçebilir değil sonra true olur. Varsayılan değer **false**.

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
  "domainNameLabel": "foobar",
  "newOrExistingOrNone": "new"
}
```
- Zaman `options.hideNone` olarak belirtilen **true**, `newOrExistingOrNone` yalnızca bir değeri olur **yeni** veya **varolan**.
- Zaman `options.hideDomainNameLabel` olarak belirtilen **true**, `domainNameLabel` bildirilmedi.

## <a name="next-steps"></a>Sonraki adımlar
* Yönetilen uygulamaların giriş için bkz: [Azure yönetilen uygulama genel bakış](overview.md).
* UI tanımları oluşturmak için bir giriş için bkz [CreateUiDefinition ile çalışmaya başlama](create-uidefinition-overview.md).
* Kullanıcı Arabirimi öğeleri ortak özellikleri açıklaması için bkz: [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
