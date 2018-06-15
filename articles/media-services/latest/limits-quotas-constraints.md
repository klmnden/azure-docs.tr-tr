---
title: Kotalar ve Azure Media Services v3 kısıtlamaları | Microsoft Docs
description: Kotalar ve Azure Media Services v3 kısıtlamaları bu konuda açıklanmaktadır
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 21fc80d7cb274197ae75d2fd5524e76e1e6288d9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788434"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kotalar ve Azure Media Services v3 sınırlamaları

Bu konu, kotalar ve Azure Media Services v3 sınırlamalarını açıklar.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Azure Media Services hesabı başına varlıklar | 1.000.000|
| İş başına JobInputs | 100 |
| İş başına JobOutputs | 30 (sabit) |
| Dosya boyutu| Bazı senaryolarda, Media Services işlemek için desteklenen en büyük dosya boyutu üzerinde bir sınırı yoktur. <sup>(1)</sup> |
| Media Services hesabı başına iş | 50,000<sup>(2)</sup> |
| Media Services hesabı başına LiveEvents |5|
| Tek bir abonelik Media Services hesapları | 25 (sabit) |
| StreamingPolicies | 1.000.000<sup>(3)</sup> |
| Çalışır durumda LiveEvent başına LiveOutputs |3|
| Durdurulmuş durumda LiveEvent başına LiveOutputs |50|
| Depolama hesapları | 1.000<sup>(4)</sup> (sabit) |
| Media Services hesabı başına çalışır durumda akış uç noktaları|2|
| Media Services hesabı başına dönüşümler | 20 |
| Bir seferde bir varlıkla ilişkilendirilen benzersiz StreamingLocators | 20<sup>(5)</sup> |
  
<sup>1</sup>tek bir blob şu anda 5 TB Azure Blob Depolama için desteklenen en büyük boyutu. Ancak, Azure Media Services hizmeti tarafından kullanılan VM boyutları göre ek sınırları uygulayın. Kaynak dosyanızı 260 GB'den büyükse, işinizi büyük olasılıkla başarısız olur. 260 GB sınırından daha büyük olan 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek olası Azaltıcı Etkenler için.

<sup>2</sup> sıraya alınan, tamamlandı, etkin ve iptal edilen işler bu sayı içerir. Silinen işleri içermez. 

Toplam kayıt sayısı en yüksek kota altında olsa bile 90 günden daha eski hesabınızda herhangi bir işi kaydının otomatik olarak silinir. 

<sup>3</sup> 1.000.000 StreamingPolicy girişlerinin farklı Media Services ilkeleri (örneğin, StreamingLocator ilke veya ContentKeyAuthorizationPolicy) için bir sınır yoktur. 

>[!NOTE]
> Kullandığınız günler / erişim izinleri / vb. her zaman aynıysa, aynı ilke kimliğini kullanmanız gerekir. 

<sup>4</sup> depolama hesapları aynı Azure aboneliğinden olması gerekir.

<sup>5</sup> StreamingLocators kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

## <a name="support-ticket"></a>Destek bileti

Açarak çıkarılmasına kotaları isteyebilir sabitlenmemiş kaynaklar için bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). Lütfen istenen kota değişikliklerinden, kullanım örneği senaryoları ve gerekli bölgeleri isteğindeki ayrıntılı bilgiler içerir. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
