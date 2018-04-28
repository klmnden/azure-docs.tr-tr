---
title: Azure bulut Kabuğu'nda MSI kullanın | Microsoft Docs
description: Azure bulut kabuğunda MSI koduyla kimlik doğrulaması
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
ms.openlocfilehash: 99577faf7328dc773a9da5f7c1227aa63600aa0a
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="use-msi-in-azure-cloud-shell"></a>Azure bulut Kabuğu'nda MSI kullanın

Azure bulut Kabuk yetkilendirme yönetilen hizmet kimlikleri (MSI) ile destekler. Güvenli bir şekilde Azure Hizmetleri ile iletişim kurmak için erişim belirteçleri almak için bunu kullanın.

## <a name="about-managed-service-identity-msi"></a>Yönetilen hizmet kimliği (MSI) hakkında
Bulut uygulamaları derleme kimlik bilgilerini güvenli bir şekilde yönetmek nasıl olduğunda ortak bir challenge, bulut hizmetlerine kimlik doğrulaması için kodunuzda olması gerekir. Bulut Kabuğu'nda anahtar Kasası'ndan alma, bir komut dosyası gerekebilir kimlik için kimlik doğrulaması gerekebilir.

Yönetilen hizmet kimliği (MSI), Azure hizmetleri otomatik olarak yönetilen bir kimliği Azure Active Directory (Azure AD) vererek daha basit bu sorunun çözümüne yapar. Bu kimlik, anahtar kasası, kodunuzda herhangi bir kimlik bilgisi olmadan dahil olmak üzere Azure AD kimlik doğrulamasını destekleyen herhangi bir hizmeti için kimlik doğrulaması kullanabilirsiniz.

## <a name="acquire-access-token-in-cloud-shell"></a>Bulut kabuğunda erişim belirteci alması

MSI erişim belirteci bir ortam değişkeni ayarlamak için aşağıdaki komutları yürütün `access_token`.
```
response=$(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="handling-token-expiration"></a>Belirteç süre sonu işleme

Yerel MSI alt sistemi belirteçleri önbelleğe alır. Bu nedenle, istediğiniz ve yalnızca Azure AD üzerinde hat çağrısına sonuçları kadar çağırabilirsiniz:
- önbellek isabetsizliği önbelleğinde herhangi bir belirteci nedeniyle oluşur
- belirtecin süresi doldu

Kodunuzda belirteç önbelleğe alma, burada belirtecinin süresi doldu kaynağı gösterir senaryoları işlemek için hazırlıklı olmalıdır.

Belirteç hataları işlemek için ziyaret [MSI sayfa MSI erişim belirteçleri curling hakkında](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token#error-handling).

## <a name="next-steps"></a>Sonraki adımlar
[MSI hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview)  
[MSI Vm'lerden gelen erişim belirteçleri alınıyor](https://docs.microsoft.com/azure/active-directory/managed-service-identity/how-to-use-vm-token)
