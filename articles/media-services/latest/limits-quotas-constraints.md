---
title: Kotalar ve kısıtlamalar Azure Media Services v3 | Microsoft Docs
description: Bu konuda, kotalar ve kısıtlamalar Azure Media Services v3 açıklanmaktadır
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 285f4f4aad18943399511cbcac711933f23f7cb2
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004806"
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
| Media Services hesabı başına Canlı etkinlikler |5|
| Tek bir abonelikte Media Services hesapları | 25 (sabit) |
| Çalışır durumdaki Livestream başına Canlı çıkışları |3|
| Depolama hesapları | 100<sup>(4)</sup> (sabit) |
| Media Services hesabı başına çalışır durumda akış uç noktaları|2|
| Akış İlkeleri | 100 <sup>(3)</sup> |
| Media Services hesabı başına dönüşümler | 100 (sabit)|
| Tek seferde bir varlıkla ilişkilendirilen benzersiz akış bulucuları | 100<sup>(5)</sup> (sabit) |

<sup>1</sup> tek bir blob şu anda en çok 5 TB Azure Blob Depolama için desteklenen en büyük boyutu. Ancak, Azure Media Services'ın hizmet tarafından kullanılan VM boyutlarına göre ek sınırlar geçerlidir. Kaynak dosyanız 260 GB'den daha büyük ise, işinizi büyük olasılıkla başarısız olur. 260 GB sınırından daha büyük olan 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek potansiyel risk azaltma işlemleri için.

<sup>2</sup> bu sayı, sıraya alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. 

Toplam kayıt sayısı üst kota sınırının altında olsa bile, hesabınızdaki 90 günden eski olan tüm iş kayıtları otomatik olarak silinir. 

<sup>3</sup> özel kullanırken [akış ilke](https://docs.microsoft.com/rest/api/media/streamingpolicies), medya hizmeti hesabınız için sınırlı sayıda tür ilkeleri tasarlayın ve aynı şifreleme seçenekleri ve iletişim kuralları için StreamingLocators yeniden kullanabilmesi gerekir gerekli. Yeni bir akış ilke her akış Bulucu için oluşturduğunuz değil.

<sup>4</sup> depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>5</sup> akış Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın.

## <a name="support-ticket"></a>Destek bileti

Sabitlenmemiş kaynaklar için kotalar açarak yükseltilecek için isteyebilir bir [destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest). İstenen kota değişiklikleri, kullanım örneği senaryolarını ve gerekli bölgeleri isteğindeki ayrıntılı bilgiler içerir. <br/>Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

## <a name="next-steps"></a>Sonraki adımlar

[Genel Bakış](media-services-overview.md)
