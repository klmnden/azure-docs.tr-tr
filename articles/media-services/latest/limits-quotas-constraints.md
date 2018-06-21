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
ms.date: 06/13/2018
ms.author: juliako
ms.openlocfilehash: 14779306815681c368a98d698a6688d528a6c747
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36294038"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kotalar ve Azure Media Services v3 sınırlamaları

Bu makalede, kotalar ve Azure Media Services v3 sınırlamalar açıklanır.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Azure Media Services hesabı başına varlıklar | 1.000.000|
| İş başına JobInputs | 50 (sabit)|
| Bir dönüştürme işi/TransformOutputs başına JobOutputs | 20 (sabit) |
| JobInput başına dosyaları|10 (sabit)|
| Dosya boyutu| Bazı senaryolarda, Media Services işlemek için desteklenen en büyük dosya boyutu üzerinde bir sınırı yoktur. <sup>(1)</sup> |
| Media Services hesabı başına iş | 500.000 <sup>(2)</sup> (sabit)|
| Dönüşümler listeleme|Sayfa başına 1000 dönüşümler ile yanıt sayfalara bölme|
| İşlerini listeleme|Sayfa başına 500 işleriyle yanıt sayfalara bölme|
| Media Services hesabı başına LiveEvents |5|
| Tek bir abonelik Media Services hesapları | 25 (sabit) |
| StreamingPolicies | 1.000.000<sup>(3)</sup> |
| Çalışır durumda LiveEvent başına LiveOutputs |3|
| Durdurulmuş durumda LiveEvent başına LiveOutputs |50|
| Depolama hesapları | 100<sup>(4)</sup> (sabit) |
| Media Services hesabı başına çalışır durumda akış uç noktaları|2|
| Media Services hesabı başına dönüşümler | 100 (sabit)|
| Bir seferde bir varlıkla ilişkilendirilen benzersiz StreamingLocators | 20<sup>(5)</sup> |

<sup>1</sup> tek bir blob şu anda 5 TB Azure Blob Depolama için desteklenen en büyük boyutu. Ancak, Azure Media Services hizmeti tarafından kullanılan VM boyutları göre ek sınırları uygulayın. Kaynak dosyanızı 260 GB'den büyükse, işinizi büyük olasılıkla başarısız olur. 260 GB sınırından daha büyük olan 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek olası Azaltıcı Etkenler için.

<sup>2</sup> sıraya alınan, tamamlandı, etkin ve iptal edilen işler bu sayı içerir. Silinen işleri içermez. 

Toplam kayıt sayısı en yüksek kota altında olsa bile 90 günden daha eski hesabınızda herhangi bir işi kaydının otomatik olarak silinir. 

<sup>3</sup> özel kullanırken [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), medya hizmeti hesabınızı gibi ilkelerin sınırlı sayıda tasarım ve aynı şifreleme seçenekleri ve protokolleri olduğunda bunları, StreamingLocators için aşağıdaki yeniden kullanmanız gerekir gerekli. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

<sup>4</sup> depolama hesapları aynı Azure aboneliğinden olması gerekir.

<sup>5</sup> StreamingLocators kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

## <a name="support-ticket"></a>Destek bileti

Açarak çıkarılmasına kotaları isteyebilir sabitlenmemiş kaynaklar için bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). İstenen kota değişikliklerinden, kullanım örneği senaryoları ve gerekli bölgeleri isteğindeki ayrıntılı bilgiler içerir. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
