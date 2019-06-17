---
title: Azure VirtualNetworkCombo UI öğesi | Microsoft Docs
description: Azure portalına yönelik Microsoft.Network.VirtualNetworkCombo UI öğesi açıklar.
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
ms.openlocfilehash: b0437338b403ff19761173d08be3938d07f13f55
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64708353"
---
# <a name="microsoftnetworkvirtualnetworkcombo-ui-element"></a>Microsoft.Network.VirtualNetworkCombo kullanıcı Arabirimi öğesi
Yeni veya mevcut bir sanal ağ seçme denetimlerini grubudur.

## <a name="ui-sample"></a>Örnek kullanıcı Arabirimi
Kullanıcı yeni bir sanal ağ seçer, kullanıcının her alt ağın adı ve adres ön eki özelleştirebilirsiniz. Alt ağları yapılandırmak isteğe bağlıdır.

![Yeni Microsoft.Network.VirtualNetworkCombo](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-new.png)

Kullanıcı bir sanal ağınız seçer, kullanıcı var olan bir alt ağ için dağıtım şablonu gerektiren her alt ağ eşlemeniz gerekir. Alt yapılandırma bu durumda gereklidir.

![Microsoft.Network.VirtualNetworkCombo var](./media/managed-application-elements/microsoft.network.virtualnetworkcombo-existing.png)

## <a name="schema"></a>Şema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.VirtualNetworkCombo",
  "label": {
    "virtualNetwork": "Virtual network",
    "subnets": "Subnets"
  },
  "toolTip": {
    "virtualNetwork": "",
    "subnets": ""
  },
  "defaultValue": {
    "name": "vnet01",
    "addressPrefixSize": "/16"
  },
  "constraints": {
    "minAddressPrefixSize": "/16"
  },
  "options": {
    "hideExisting": false
  },
  "subnets": {
    "subnet1": {
      "label": "First subnet",
      "defaultValue": {
        "name": "subnet-1",
        "addressPrefixSize": "/24"
      },
      "constraints": {
        "minAddressPrefixSize": "/24",
        "minAddressCount": 12,
        "requireContiguousAddresses": true
      }
    },
    "subnet2": {
      "label": "Second subnet",
      "defaultValue": {
        "name": "subnet-2",
        "addressPrefixSize": "/26"
      },
      "constraints": {
        "minAddressPrefixSize": "/26",
        "minAddressCount": 8,
        "requireContiguousAddresses": true
      }
    }
  },
  "visible": true
}
```

## <a name="remarks"></a>Açıklamalar
- Bu seçenek belirtilmişse, ilk çakışmayan adres ön eki boyutu `defaultValue.addressPrefixSize` kullanıcının aboneliği var olan sanal ağları göre otomatik olarak belirlenir.
- İçin varsayılan değer `defaultValue.name` ve `defaultValue.addressPrefixSize` olduğu **null**.
- `constraints.minAddressPrefixSize` belirtilmelidir. Belirtilen değerden daha küçük bir adres alanı ile var olan tüm sanal ağları seçilemez.
- `subnets` belirtilmelidir ve `constraints.minAddressPrefixSize` her alt ağ için belirtilmelidir.
- Yeni bir sanal ağ oluştururken, her alt ağın adres ön eki göre otomatik olarak sanal ağın adres ön eki ilgili hesaplanır `addressPrefixSize`.
- Sanal varolan kullanırken, ağ, hiçbir alt ağ ilgili daha küçük `constraints.minAddressPrefixSize` seçimi için kullanılamaz. Ayrıca, belirtilmişse, en az olmayan alt ağlar `minAddressCount` kullanılabilir adresleri olan seçilemez. Varsayılan değer **0**. Kullanılabilir adresler bitişik olmasını sağlamak için belirtin **true** için `requireContiguousAddresses`. Varsayılan değer **true**.
- Mevcut bir sanal ağda alt ağlar oluşturma desteklenmiyor.
- Varsa `options.hideExisting` olduğu **true**, kullanıcının mevcut bir sanal ağı seçemezsiniz. Varsayılan değer **false**.

## <a name="sample-output"></a>Örnek çıktı

```json
{
  "name": "vnet01",
  "resourceGroup": "rg01",
  "addressPrefixes": ["10.0.0.0/16"],
  "newOrExisting": "new",
  "subnets": {
    "subnet1": {
      "name": "subnet-1",
      "addressPrefix": "10.0.0.0/24",
      "startAddress": "10.0.0.1"
    },
    "subnet2": {
      "name": "subnet-2",
      "addressPrefix": "10.0.1.0/26",
      "startAddress": "10.0.1.1"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* UI tanımları oluşturma, bir giriş için bkz. [createuidefinition dosyasını kullanmaya başlama](create-uidefinition-overview.md).
* Ortak Özellikler UI öğelerinin açıklaması için bkz. [CreateUiDefinition öğeleri](create-uidefinition-elements.md).
