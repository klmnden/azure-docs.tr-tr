---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: b275a86f8fd35c43865fd920d1bfc9994a796a9c
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805346"
---
>[!NOTE]
>Sabit olmayan kaynaklar için kotalar bir artış istemek için bir destek bileti açın. Daha yüksek sınırlar elde etmek için ek Azure Media Services hesapları oluşturmayın.

| Kaynak | Varsayılan limit | 
| --- | --- | 
| Tek bir abonelikte Azure Media Services hesapları | 25 (sabit) |
| Medya ayrılmış birimleri Media Services hesabı başına |25 (S1)<br/>10 (S2, S3)<sup>1</sup> | 
| Media Services hesabı başına iş | 50.000<sup>2</sup> |
| İş başına zincirleme görev sayısı | 30 (sabit) |
| Media Services hesabı başına varlık sayısı | 1.000.000|
| Görev başına varlık sayısı | 50 |
| İş başına varlık sayısı | 100 |
| Tek seferde bir varlıkla ilişkilendirilen benzersiz bulucu sayısı | 5<sup>4</sup> |
| Media Services hesabı başına Canlı kanal sayısı |5|
| Kanal başına durdurulmuş durumdaki program sayısı |50|
| Kanal başına çalışır durumdaki program sayısı |3|
| Media Services hesabı çalışan ya da akış durdurulur uç noktaları|2|
| Akış uç noktası başına akış birimleri |10 |
| Depolama hesapları | 1.000<sup>5</sup> (sabit) |
| İlkeler | 1.000.000<sup>6</sup> |
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen en büyük dosya boyutu sınırı yoktur. <sup>7</sup> |

<sup>1</sup>türünü değiştirirseniz, örneğin, S1, S2'den en fazla ayrılmış birim sınırları sıfırlanır.

<sup>2</sup>bu sayı, sıraya alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Bu, silinmiş işleri içermez. Kullanarak eski işleri silebilirsiniz **Ijob.Delete** veya **Sil** HTTP isteği.

1 Nisan 2017 itibarıyla, hesabınızdaki 90 günden eski olan tüm iş kayıtları otomatik olarak kendi ilişkili görev kayıtlarıyla birlikte silinir. Toplam kayıt sayısı üst kota sınırının altında olsa bile, otomatik silme işlemi gerçekleşir. İş ve görev bilgilerini arşivlemeniz için açıklanan kodu kullanın [Media Services .NET SDK'sı ile varlıkları yönetme](../articles/media-services/previous/media-services-dotnet-manage-entities.md).

<sup>3</sup>listesi iş varlıklarına isteğinde bulunduğunda, istek başına en fazla 1.000 işleri döndürülür. Gönderilen tüm işleri izlemek, üst kullanın veya sorguları açıklandığı atlamak için [OData sorgu seçeneklerinde](/previous-versions/dynamicscrm-2015/developers-guide/gg309461(v=crm.7)).

<sup>4</sup>bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanan değildir. Bireysel kullanıcılara farklı erişim hakları vermek için digital rights management (DRM) çözümleri kullanın. Daha fazla bilgi için [Azure Media Services ile içeriğinizi korumanıza](../articles/media-services/previous/media-services-content-protection-overview.md).

<sup>5</sup>depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6</sup>1.000.000 ilkeleri farklı Media Services ilkeleri için sınır yoktur. Örnek, Bulucu İlkesi veya ContentKeyAuthorizationPolicy için verilebilir. 

>[!NOTE]
> Her zaman aynı günleri kullanıyorsanız ve erişim izinleri, aynı ilke kimliğini kullanın. Bilgi ve örnek için bkz. [Media Services .NET SDK'sı ile varlıkları yönetme](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies).

<sup>7</sup>hizmetinde medya işlemcileri biriyle işlemek için Media Services içindeki bir varlığa içeriği karşıya yükleme, desteklenen en büyük dosya boyutu unutmayın. Media Encoder Standard gibi kodlayıcılar ve Media Encoder Premium iş akışı veya Analiz motorları Face Detector gibi varlıkları içerir.

Tek bir blob için desteklenen en büyük boyutu şu anda Azure Blob Depolama'da 5 TB ' dir. Media Services'ın hizmet tarafından kullanılan VM boyutlarına göre ek sınırlar geçerlidir. Aşağıdaki tabloda, medya ayrılmış birimleri S1, S2 ve S3 sınırları gösterilir. Kaynak dosyanız tabloda tanımlanan sınırları daha büyükse, kodlama işi başarısız olur. 4 K çözümlemesi kaynakları uzun süreli kodlayın, gerekli performansı elde etmek üzere S3 medya ayrılmış birimi kullanmak için gerekli. S3 medya ayrılmış birimi üzerindeki 260 GB sınırdan daha büyük olan 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek potansiyel risk azaltma işlemleri için.

| Medya ayrılmış birimi türü | En büyük giriş boyutu (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
