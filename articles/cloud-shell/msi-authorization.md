---
title: Azure Cloud shell'de Azure kaynakları için yönetilen kimlikleri kullanmak | Microsoft Docs
description: Azure Cloud shell'de MSI ile kod kimlik doğrulaması
services: azure
documentationcenter: ''
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: juluk
ms.openlocfilehash: 09f54efaf3ff89711c34b7960a271438f38cf224
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49345093"
---
# <a name="use-msi-in-azure-cloud-shell"></a>Azure Cloud Shell'de MSI kullanma

Azure Cloud Shell, yetkilendirme yönetilen hizmet kimlikleri (MSI) ile destekler. Azure Hizmetleri ile güvenli iletişim kurabilmesi için erişim belirteçlerini almak için bunu kullanın.

## <a name="about-managed-service-identity-msi"></a>Yönetilen hizmet kimliği (MSI) hakkında
Bulut uygulamaları geliştirmek kimlik bilgilerini güvenli bir şekilde yönetmek nasıl olduğunda ortak bir challenge, bulut Hizmetleri için kimlik doğrulaması için kodunuzda olmanız gerekir. Cloud Shell'de bir komut dosyası gerekebilir bir kimlik bilgisi için anahtar Kasası'ndaki almayı kimlik doğrulaması gerekebilir.

Yönetilen Hizmet Kimliği (MSI), Azure hizmetlerine Azure Active Directory (Azure AD) üzerinde otomatik olarak yönetilen bir kimlik vererek bu soruna daha basit bir çözüm getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

## <a name="acquire-access-token-in-cloud-shell"></a>Cloud shell'de erişim belirteci alma

MSI erişim belirtecinizin bir ortam değişkeni ayarlamak için aşağıdaki komutları yürütün `access_token`.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Belirteç sona erme işleme

Yerel MSI alt belirteçleri önbelleğe alır. Bu nedenle, istediğiniz ve yalnızca bir Azure AD-hat üzerinde arama sonuçları gibi sık çağırabilirsiniz:
- önbellek isabetsizliği önbelleğinde herhangi bir belirteci dolayı gerçekleşir
- belirtecin süresi doldu

Belirteci kodunuzda önbellek, burada kaynak belirtecinin kullanım süresi doldu gösterir senaryoları işlemeye hazırlıklı olmalıdır.

Belirteç hataları işlemek için şurayı ziyaret edin [MSI erişim belirteçleri curling hakkında MSI sayfası](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).

## <a name="next-steps"></a>Sonraki adımlar
[MSI hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[MSI vm'lerden erişim belirteci alma](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
