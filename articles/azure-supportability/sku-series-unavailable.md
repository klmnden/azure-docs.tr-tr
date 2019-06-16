---
title: SKU serileri kullanılamıyor | Microsoft Docs
description: Bazı SKU serileri bu bölge için seçili abonelik için kullanılamaz.
services: Azure Supportability
documentationcenter: ''
author: stevendotwang
manager: rajatk
editor: ''
ms.service: azure-supportability
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: xingwan
ms.openlocfilehash: a57899e36a6716a6fd59cb018119c225b7396c0d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60649132"
---
# <a name="region-or-sku-unavailable"></a>Bölge veya SKU kullanılamıyor
Bu makalede bir Azure aboneliğinin bir bölge veya sanal makine SKU'suna erişimin olmaması sorunun nasıl çözüleceği açıklanır.

## <a name="symptoms"></a>Belirtiler

### <a name="when-deploying-a-virtual-machine-you-receive-one-of-the-following-error-messages"></a>Bir sanal makineyi dağıtırken aşağıdaki hata iletilerinden birini alabilirsiniz:
```
Code: SkuNotAvailable
Message: The requested size for resource '<resource>' is currently not available in location 
'<location>' zones '<zone>' for subscription '<subscriptionID>'. Please try another size or 
deploy to a different location or zones. See https://aka.ms/azureskunotavailable for details.
```

```
Message: Your subscription doesn’t support virtual machine creation in <location>. Choose a 
different location. Supported locations are <list of locations>
```

```
Code: NotAvailableForSubscription
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-purchasing-reserved-virtual-machine-instances-you-receive-one-of-the-following-error-messages"></a>Ayrılmış sanal makine örnekleri satın alırken aşağıdaki hata iletilerinden birini alabilirsiniz:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-creating-a-support-request-to-increase-compute-core-quota-a-region-or-a-sku-family-is-not-available-for-selection"></a>İşlem çekirdek kotasını artırmak için bir destek isteği oluştururken, bir bölge veya SKU ailesi seçimi için kullanılamaz.

## <a name="solution"></a>Çözüm
Öncelikle, bir alternatif bölgelere veya iş gereksinimlerinize uyan SKU ele almanızı öneririz. Uygun bir bölge veya SKU bulamıyor varsa "Abonelik yönetimi" oluşturun [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) aşağıdaki adımları izleyin:


- Temel bilgileri sayfasında, sorun türü olarak "Abonelik yönetimi" seçin, aboneliği seçin ve "İleri" ye tıklayın.

![Temel Bilgiler dikey penceresi](./media/SKU-series-unavailable/BasicsSubMgmt.png)


-   Sorun sayfasında "diğer genel sorular" sorun türü seçin.
- Ayrıntılar bölümünde:
  - Sanal makineleri dağıtma veya ayrılmış sanal makine örnekleri satın arıyorsanız Lütfen belirtin
  - Lütfen bölge, SKU ve dağıtmayı veya satın planlama sanal makine örneği sayısını belirtin


![Sorun](./media/SKU-series-unavailable/ProblemSubMgmt.png)

-   İletişim bilgilerinizi girin ve "Oluştur" a tıklayın.

![İletişim Bilgileri](./media/SKU-series-unavailable/ContactInformation.png)

## <a name="feedback"></a>Geri Bildirim
Her zaman geri bildirim ve öneriler için açık duyuyoruz! Bize gönderin, [önerileri](https://feedback.azure.com/forums/266794-support-feedback). Ayrıca, üzerinden bize ile görüşebilirsiniz [Twitter](https://twitter.com/azuresupport) veya [MSDN Forumları](https://social.msdn.microsoft.com/Forums/azure).

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Azure desteği SSS](https://azure.microsoft.com/support/faq)

