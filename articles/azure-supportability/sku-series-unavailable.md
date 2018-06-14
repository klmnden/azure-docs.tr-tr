---
title: SKU serisi kullanılamaz | Microsoft Docs
description: Bazı SKU serisi bu bölge için seçilen abonelik için kullanılamaz.
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
ms.openlocfilehash: 62964d0c5d75168226a35b25e5c256a1b57f3f81
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
ms.locfileid: "24870587"
---
# <a name="region-or-sku-unavailable"></a>Bölge veya SKU kullanılamıyor
Bu makalede sorunun bir bölge veya VM SKU erişimi olmaması bir Azure aboneliğinin nasıl çözümleneceği açıklanır.

## <a name="symptoms"></a>Belirtiler

### <a name="when-deploying-a-virtual-machine-you-receive-one-of-the-following-error-messages"></a>Bir sanal makine dağıtımı sırasında aşağıdaki hata iletilerinden birini alabilirsiniz:
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

### <a name="when-purchasing-reserved-virtual-machine-instances-you-receive-one-of-the-following-error-messages"></a>Ayrılmış sanal makine örnekleri satın alırken, aşağıdaki hata iletilerinden birini alabilirsiniz:

```
Message: Your subscription doesn’t support virtual machine reservation in <location>. Choose a 
different location. Supported locations are: <list of locations>  
```

```
Message: This size is currently unavailable in this location for this subscription
```

### <a name="when-creating-a-support-request-to-increase-compute-core-quota-a-region-or-a-sku-family-is-not-available-for-selection"></a>İşlem çekirdek Kotayı artırmak için bir destek isteği oluşturulurken bir bölge veya SKU ailesi seçime uygun değil.

## <a name="solution"></a>Çözüm
Önce bir alternatif bölge veya iş gereksinimlerinize uygun SKU ele almanızı öneririz. Uygun bölge veya SKU bulamıyor varsa, "Abonelik yönetimi" oluşturun [destek isteği](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) aşağıdaki adımları izleyerek:


- Temel bilgileri sayfasında "Abonelik yönetimi" olarak sorun türü seçin, aboneliği seçin ve "İleri" yi tıklatın.

![Temel Bilgiler dikey penceresi](./media/SKU-series-unavailable/BasicsSubMgmt.png)


-   Sorun sayfasında "diğer genel sorular" sorun türü seçin.
- Ayrıntılar bölümünde:
  - Sanal makineler dağıtmak veya ayrılmış sanal makine örnekleri satınalma arıyorsanız, lütfen belirtin
  - Lütfen bölge, SKU ve dağıtmak veya satın alma için planlama sanal makine örneği sayısını belirtin


![Sorun](./media/SKU-series-unavailable/ProblemSubMgmt.png)

-   İletişim bilgilerinizi girin ve "Oluştur" seçeneğine tıklayın.

![İletişim Bilgileri](./media/SKU-series-unavailable/ContactInformation.png)

## <a name="feedback"></a>Geri Bildirim
Biz her zaman görüş ve öneriler için açık! Bize gönderin, [önerileri](https://feedback.azure.com/forums/266794-support-feedback). Ayrıca, bizimle devreye [Twitter](https://twitter.com/azuresupport) veya [MSDN Forumları](https://social.msdn.microsoft.com/Forums/azure).

## <a name="learn-more"></a>Daha fazla bilgi edinin
[Azure destek SSS](https://azure.microsoft.com/support/faq)

