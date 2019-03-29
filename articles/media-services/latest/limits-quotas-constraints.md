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
ms.date: 03/12/2019
ms.author: juliako
ms.openlocfilehash: d5fc14adab956fae23aad24fa7bc488c8c2041e3
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58621694"
---
# <a name="quotas-and-limitations-in-azure-media-services-v3"></a>Kotalar ve kısıtlamalar Azure Media Services v3

Bu makalede, kotalar ve kısıtlamalar Azure Media Services v3 açıklanır.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Azure Media Services hesabı başına Varlık sayısı | 1.000.000|
| Dinamik Bildirim Filtreleri|100|
| İş başına JobInputs | 50 (sabit)|
| İş başına JobOutputs | 20 (sabit) |
| Dönüşüm içinde TransformOutputs | 20 (sabit) |
| Dosyaları JobInput başına|10 (sabit)|
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen en büyük dosya boyutu sınırı yoktur. <sup>(1)</sup> |
| Media Services hesabı başına iş | 500.000 <sup>(2)</sup> (sabit)|
| Dönüşüm Listeleme|Yanıtı sayfalandırma, sayfa başına 1000 Dönüşüm|
| İşleri Listeleme|Yanıtı sayfalandırma, sayfa başına 500 İş|
| Media Services hesabı başına Canlı Etkinlik sayısı |5|
| Tek bir abonelikte Media Services hesapları | 25 (sabit) |
| Çalışır durumdaki canlı olay başına Canlı çıkışları |3|
| En fazla Canlı çıkış süresi | 25 saat |
| Depolama hesapları | 100<sup>(4)</sup> (sabit) |
| Akış uç noktaları (durduruldu veya çalışıyor) Media Services hesabı başına|2 (sabit)|
| Akış İlkeleri | 100 <sup>(3)</sup> |
| Media Services hesabı başına dönüşümler | 100 (sabit)|
| Tek seferde bir varlıkla ilişkilendirilen benzersiz akış bulucuları | 100<sup>(5)</sup> (sabit) |
| İçerik Anahtarı İlkesi |30 | 

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
