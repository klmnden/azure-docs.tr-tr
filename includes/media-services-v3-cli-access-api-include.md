---
title: include dosyası
description: include dosyası
services: media-services
author: WenJason
ms.service: media-services
ms.topic: include
origin.date: 05/29/2018
ms.date: 06/25/2018
ms.author: v-nany
ms.custom: include file
ms.openlocfilehash: 4dde0a47f0452da2dd951df86ccb6e02a44521ed
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309102"
---
## <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Azure AD hizmet sorumlusu kimlik doğrulamasını kullanarak Azure Media Services API’sine bağlanın. Aşağıdaki komut bir Azure AD uygulaması oluşturur ve hesaba bir hizmet sorumlusu ekler. Uygulamanızı yapılandırmak için, döndürülen değerleri kullanmalısınız.

Betiği çalıştırmadan önce, `amsaccount` ve `amsResourceGroup` değerlerini bu kaynakları oluştururken seçtiğiniz adlarla değiştirebilirsiniz. `amsaccount`, hizmet sorumlusunu ekleyeceğiniz Azure Media Services hesabının adıdır.

Aşağıdaki komut bir `json` çıkışı döndürür:

```cli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Bu komut, aşağıdakine benzer bir yanıt oluşturur:

```json
{
  "AadClientId": "00000000-4cdd-418a-8a72-0755ace03de5",
  "AadEndpoint": "https://login.partner.microsoftonline.cn",
  "AadSecret": "00000000-02f5-4bf2-9057-1c4f7baff155",
  "AadTenantId": "00000000-86f1-41af-91ab-2d7cd011db47",
  "AccountName": "amsaccount22",
  "ArmAadAudience": "https://management.core.chinacloudapi.cn/",
  "ArmEndpoint": "https://management.chinacloudapi.cn/",
  "Region": "chinaeast",
  "ResourceGroup": "amsResourceGroup2",
  "SubscriptionId": "00000000-6753-4ca2-b1ae-193798e2c9d8"
}
```

Yanıtta bir `xml` almak istiyorsanız, aşağıdaki komutu kullanın:

```cli
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
