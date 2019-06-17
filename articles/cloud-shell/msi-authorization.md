---
title: Azure Cloud shell'de Azure kaynakları için yönetilen kimlikleri kullanmak | Microsoft Docs
description: Azure Cloud shell'de MSI ile kod kimlik doğrulaması
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: damaerte
ms.openlocfilehash: 7cadaaf67f9c6923ee9e9eb2596941aa8e1f0c9b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60199491"
---
# <a name="use-managed-identities-for-azure-resources-in-azure-cloud-shell"></a>Azure Cloud shell'de Azure kaynakları için yönetilen kimlikleri kullanmak

Azure Cloud Shell, Azure kaynakları için yönetilen kimliklerle yetkilendirme destekler. Azure Hizmetleri ile güvenli iletişim kurabilmesi için erişim belirteçlerini almak için bunu kullanın.

## <a name="about-managed-identities-for-azure-resources"></a>Azure kaynakları için yönetilen kimlikler hakkında
Bulut uygulamaları geliştirmek kimlik bilgilerini güvenli bir şekilde yönetmek nasıl olduğunda ortak bir challenge, bulut Hizmetleri için kimlik doğrulaması için kodunuzda olmanız gerekir. Cloud Shell'de bir komut dosyası gerekebilir bir kimlik bilgisi için anahtar Kasası'ndaki almayı kimlik doğrulaması gerekebilir.

Azure kaynakları için yönetilen kimlikleri, Azure hizmetleri otomatik olarak yönetilen bir kimlik Azure Active Directory (Azure AD) sağlayarak bu sorunu daha basit çözme hale getirir. Bu kimliği kullanarak, Key Vault da dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

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
