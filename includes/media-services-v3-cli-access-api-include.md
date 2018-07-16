---
title: include dosyası
description: include dosyası
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 05/29/2018
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: f4600413e05950446db71f988c4c4302f0dcacb3
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39063066"
---
## <a name="access-the-media-services-api"></a>Media Services API’sine erişim

Azure AD hizmet sorumlusu kimlik doğrulamasını kullanarak Azure Media Services API’sine bağlanın. Aşağıdaki komut bir Azure AD uygulaması oluşturur ve hesaba bir hizmet sorumlusu ekler. Uygulamanızı yapılandırmak için, döndürülen değerleri kullanmalısınız.

Betiği çalıştırmadan önce, `amsaccount` ve `amsResourceGroup` değerlerini bu kaynakları oluştururken seçtiğiniz adlarla değiştirebilirsiniz. `amsaccount`, hizmet sorumlusunu ekleyeceğiniz Azure Media Services hesabının adıdır.

Aşağıdaki komut bir `json` çıkışı döndürür:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup
```

Bu komut, aşağıdakine benzer bir yanıt oluşturur:

```json
{
  "AadClientId": "00000000-0000-0000-0000-000000000000",
  "AadEndpoint": "https://login.microsoftonline.com",
  "AadSecret": "00000000-0000-0000-0000-000000000000",
  "AadTenantId": "00000000-0000-0000-0000-000000000000",
  "AccountName": "amsaccount",
  "ArmAadAudience": "https://management.core.windows.net/",
  "ArmEndpoint": "https://management.azure.com/",
  "Region": "West US 2",
  "ResourceGroup": "amsResourceGroup",
  "SubscriptionId": "00000000-0000-0000-0000-000000000000"
}
```

Yanıtta bir `xml` almak istiyorsanız, aşağıdaki komutu kullanın:

```azurecli-interactive
az ams account sp create --account-name amsaccount --resource-group amsResourceGroup --xml
```
