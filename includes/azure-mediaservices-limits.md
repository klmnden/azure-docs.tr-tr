>[!NOTE]
>Sabitlenmemiş kaynaklar için bir destek bileti açarak kotaların yükseltilmesini isteyebilirsiniz. Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Tek bir abonelikte Azure Media Services (AMS) hesabı sayısı | 25 (sabit) |
| AMS hesabı başına Medya Ayrılmış Birimleri (RU’lar) |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
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
| Dosya boyutu| Bazı senaryolarda, Media Services işlemek için desteklenen en büyük dosya boyutu üzerinde bir sınırı yoktur. <sup>7</sup> |
  
<sup>1</sup> S3 RU’ları Hindistan Batı bölgesinde kullanılamaz. Türünden (örneğin, S1 S2) değiştirirseniz, max RU sınırları sıfırlanır.

<sup>2</sup> Bu sayı kuyruğa alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. **IJob.Delete** veya **DELETE** HTTP isteğini kullanarak eski işleri silebilirsiniz.

Toplam kayıt sayısı en yüksek kota altında olsa bile 1 Nisan 2017 itibariyle 90 günden daha eski hesabınızda herhangi bir işi kaydının otomatik olarak, ilişkili görev kayıtlarını yanı sıra, silinir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, [burada](../articles/media-services/media-services-dotnet-manage-entities.md) açıklanan kodu kullanabilirsiniz.

<sup>3</sup> bir istek varlıkları işi listesine yaparken, istek başına en çok 1.000 işleri döndürülür. Gönderilen tüm işleri izlemeniz gerekiyorsa, [OData sorgu seçeneklerinde](http://msdn.microsoft.com/library/gg309461.aspx) açıklandığı gibi üste/atla seçeneğini kullanabilirsiniz.

<sup>4</sup> Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın. Daha fazla bilgi için [bu](../articles/media-services/media-services-content-protection-overview.md) bölüme bakın.

<sup>5</sup> Depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6</sup> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu politikası veya ContentKeyAuthorizationPolicy için). 

>[!NOTE]
> Kullandığınız günler / erişim izinleri / vb. her zaman aynıysa, aynı ilke kimliğini kullanmanız gerekir. Bilgi ve bir örnek için [bu](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) bölüme bakın.

<sup>7</sup>hizmet medya işlemcileri birine işlemek için Azure Media Services'de bir varlık için içerik yüklüyorsunuz varsa (diğer bir deyişle, yüz algılayıcısı Medya Kodlayıcısı standart ve Medya Kodlayıcısı Premium iş akışı veya Analiz altyapıları gibi kodlayıcılar ister), ardından desteklenen en büyük dosya boyutu kısıtlamalar haberdar olmanız gerekir. 

Tek bir blob için desteklenen en fazla şu anda 5 TB Azure Blob Depolama boyutudur. Ancak, Azure Media Services hizmeti tarafından kullanılan VM boyutları göre ek sınırları uygulayın. Aşağıdaki tabloda her medya ayrılmış birimleri (S1, S2, S3) sınırları gösterilir Kaynak dosyanızı tabloda tanımlanan sınırların daha büyükse, kodlama işinin başarısız olur. Uzun süreli 4 K çözümleme kaynakları kodlama, gerekli bir performans elde S3 medya ayrılmış birimleri kullanması gerekir. S3 medya ayrılmış birimleri 260 GB sınırını büyük 4 K içeriğiniz varsa, adresinden bize başvurun amshelp@microsoft.com senaryonuz desteklemek olası Azaltıcı Etkenler için.

| Medya Ayrılmış Birimi türü | En büyük giriş boyutu (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
