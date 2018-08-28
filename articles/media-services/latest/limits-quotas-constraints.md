---
title: Kotalar ve kısıtlamalar Azure Media Services v3 | Microsoft Docs
description: Bu konuda, kotalar ve kısıtlamalar Azure Media Services v3 açıklanmaktadır
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 08/26/2018
ms.author: juliako
ms.openlocfilehash: 49b834325ce819f20978e06d85ee308955510ac1
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43051118"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kotalar ve kısıtlamalar Azure Media Services v3

Bu makalede, kotalar ve kısıtlamalar Azure Media Services v3 açıklanır.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Azure Media Services hesabı başına varlık sayısı | 1.000.000|
| Dinamik bildirim filtreleri|100|
| İş başına JobInputs | 50 (sabit)|
| Dönüşüm iş/TransformOutputs başına JobOutputs | 20 (sabit) |
| Dosyaları JobInput başına|10 (sabit)|
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen en büyük dosya boyutu sınırı yoktur. <sup>(1)</sup> |
| Media Services hesabı başına iş | 500.000 <sup>(2)</sup> (sabit)|
| Dönüşümler listeleme|Sayfa başına 1000 dönüşümlerle yanıt sayfalandırma|
| İşleri listeleme|Sayfa başına 500 işleriyle yanıt sayfalandırma|
| Media Services hesabı başına LiveEvents |5|
| Tek bir abonelikte Media Services hesapları | 25 (sabit) |
| Çalışır durumdaki Livestream başına LiveOutputs |3|
| Depolama hesapları | 100<sup>(4)</sup> (sabit) |
| Media Services hesabı başına çalışır durumda akış uç noktaları|2|
| StreamingPolicies | 100 <sup>(3)</sup> |
| Media Services hesabı başına dönüşümler | 100 (sabit)|
| Tek seferde bir varlıkla ilişkilendirilen benzersiz StreamingLocators | 100<sup>(5)</sup> (sabit) |

<sup>1</sup> tek bir blob şu anda en çok 5 TB Azure Blob Depolama için desteklenen en büyük boyutu. Ancak, Azure Media Services'ın hizmet tarafından kullanılan VM boyutlarına göre ek sınırlar geçerlidir. Kaynak dosyanız 260 GB'den daha büyük ise, işinizi büyük olasılıkla başarısız olur. 260 GB sınırından daha büyük olan 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek potansiyel risk azaltma işlemleri için.

<sup>2</sup> bu sayı, sıraya alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. 

Toplam kayıt sayısı üst kota sınırının altında olsa bile, hesabınızdaki 90 günden eski olan tüm iş kayıtları otomatik olarak silinir. 

<sup>3</sup> özel kullanırken [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies), medya hizmeti hesabınız için sınırlı sayıda tür ilkeleri tasarlayın ve aynı şifreleme seçenekleri ve iletişim kuralları için StreamingLocators yeniden kullanabilmesi gerekir gerekli. Her StreamingLocator için yeni bir StreamingPolicy oluşturmamanız gerekir.

<sup>4</sup> depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>5</sup> StreamingLocators kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

## <a name="support-ticket"></a>Destek bileti

Sabitlenmemiş kaynaklar için kotalar açarak yükseltilecek için isteyebilir bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). İstenen kota değişiklikleri, kullanım örneği senaryolarını ve gerekli bölgeleri isteğindeki ayrıntılı bilgiler içerir. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
