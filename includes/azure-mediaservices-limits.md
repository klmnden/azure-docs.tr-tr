---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: 1e1316ef568cbc6409a8653022d9acff9837b59d
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2018
ms.locfileid: "52280005"
---
>[!NOTE]
>Sabitlenmemiş kaynaklar için bir destek bileti açarak kotaların yükseltilmesini isteyebilirsiniz. Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Tek bir abonelikte Azure Media Services (AMS) hesabı sayısı | 25 (sabit) |
| AMS hesabı başına Medya Ayrılmış Birimleri (RU’lar) |25 (S1)<br/>10 (S2, S3) <sup>(1)</sup> | 
| AMS hesabı başına iş sayısı | 50,000<sup>(2)</sup> |
| İş başına zincirleme görev sayısı | 30 (sabit) |
| AMS hesabı başına varlık sayısı | 1.000.000|
| Görev başına varlık sayısı | 50 |
| İş başına varlık sayısı | 100 |
| Tek seferde bir varlıkla ilişkilendirilen benzersiz bulucu sayısı | 5<sup>(4)</sup> |
| AMS hesabı başına canlı kanal sayısı |5|
| Kanal başına durdurulmuş durumdaki program sayısı |50|
| Kanal başına çalışır durumdaki program sayısı |3|
| AMS hesabı başına çalışır durumdaki akış uç noktaları|2|
| Akış uç noktası başına akış birimleri |10 |
| Depolama hesapları | 1,000<sup>(5)</sup> (sabit) |
| İlkeler | 1,000,000<sup>(6)</sup> |
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen en büyük dosya boyutu sınırı yoktur. <sup>7</sup> |
  
<sup>1</sup> türünden (örneğin, S2'den S1) değiştirirseniz, en fazla RU sınırları sıfırlanır.

<sup>2</sup> Bu sayı kuyruğa alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. **IJob.Delete** veya **DELETE** HTTP isteğini kullanarak eski işleri silebilirsiniz.

Toplam kayıt sayısı üst kota sınırının altında olsa bile, 1 Nisan 2017 itibarıyla, hesabınızdaki 90 günden eski olan tüm iş kayıtları otomatik olarak, ilişkili görev kayıtlarıyla birlikte silinecek. İş/görev bilgilerini arşivlemeniz gerekiyorsa, [burada](../articles/media-services/previous/media-services-dotnet-manage-entities.md) açıklanan kodu kullanabilirsiniz.

<sup>3</sup> istek varlıklar listesine işi yaparken, istek başına en fazla 1.000 işleri döndürülür. Gönderilen tüm işleri izlemeniz gerekiyorsa, [OData sorgu seçeneklerinde](https://msdn.microsoft.com/library/gg309461.aspx) açıklandığı gibi üste/atla seçeneğini kullanabilirsiniz.

<sup>4</sup> Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın. Daha fazla bilgi için [bu](../articles/media-services/previous/media-services-content-protection-overview.md) bölüme bakın.

<sup>5</sup> Depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6</sup> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu politikası veya ContentKeyAuthorizationPolicy için). 

>[!NOTE]
> Kullandığınız günler / erişim izinleri / vb. her zaman aynıysa, aynı ilke kimliğini kullanmanız gerekir. Bilgi ve bir örnek için [bu](../articles/media-services/previous/media-services-dotnet-manage-entities.md#limit-access-policies) bölüme bakın.

<sup>7</sup>hizmetinde medya işlemcileri biriyle işlemek için Azure Media Services içindeki bir varlığa içerik yüklüyorsunuz varsa (diğer bir deyişle, Media Encoder Standard ve Media Encoder Premium iş akışı veya Analiz motorları gibi kodlayıcılar yüz algılayıcısı ister), Ardından, desteklenen en büyük dosya boyutu kısıtlamalar farkında olmalısınız. 

Tek bir blob için desteklenen en büyük boyutu şu anda Azure Blob Depolama'da 5 TB ' dir. Ancak, Azure Media Services'ın hizmet tarafından kullanılan VM boyutlarına göre ek sınırlar geçerlidir. Aşağıdaki tablo her medya ayrılmış birimleri (S1, S2, S3) sınırları gösterir Kaynak dosyanız tabloda tanımlanan sınırları daha büyükse, kodlama işi başarısız olur. Uzun süreli 4 K çözümlemesi kaynakları Kodladığınız S3 medya ayrılmış birimi gerekli performansı elde etmek üzere kullanmak için gereklidir. S3 medya ayrılmış birimi 260 GB sınırına değerinden daha büyük 4 K içerik varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek potansiyel risk azaltma işlemleri için.

| Medya Ayrılmış Birimi türü | En büyük giriş boyutu (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
