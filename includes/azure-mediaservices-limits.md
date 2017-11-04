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
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. <sup>7</sup> |
  
<sup>1</sup> S3 RU’ları Hindistan Batı bölgesinde kullanılamaz. Müşteri türünden (örneğin, S1 S2) değişirse max RU sınırları sıfırlama. 

<sup>2</sup> Bu sayı kuyruğa alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. **IJob.Delete** veya **DELETE** HTTP isteğini kullanarak eski işleri silebilirsiniz.

1 Nisan 2017’den itibaren, hesabınızdaki 90 günden eski olan tüm İş kayıtları, toplam kayıt sayısı üst kota sınırının altında olsa bile ilişkili Görev kayıtlarıyla birlikte otomatik olarak silinecektir. İş/görev bilgilerini arşivlemeniz gerekiyorsa, [burada](../articles/media-services/media-services-dotnet-manage-entities.md) açıklanan kodu kullanabilirsiniz.

<sup>3</sup> İş varlıklarını listeleme isteği yaparken istek başına en fazla 1.000 adet döndürülür. Gönderilen tüm işleri izlemeniz gerekiyorsa, [OData sorgu seçeneklerinde](http://msdn.microsoft.com/library/gg309461.aspx) açıklandığı gibi üste/atla seçeneğini kullanabilirsiniz.

<sup>4</sup> Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın. Daha fazla bilgi için [bu](../articles/media-services/media-services-content-protection-overview.md) bölüme bakın.

<sup>5</sup> Depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6</sup> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu politikası veya ContentKeyAuthorizationPolicy için). 

>[!NOTE]
> Kullandığınız günler / erişim izinleri / vb. her zaman aynıysa, aynı ilke kimliğini kullanmanız gerekir. Bilgi ve bir örnek için [bu](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) bölüme bakın.

<sup>7</sup>içerik Azure Media Services bir varlık için hizmetimizi medya işlemciler biri ile işlem yapma ile karşıya yüklemekte olduğunuz varsa (yani kodlayıcılar ister yüz gibi Medya Kodlayıcısı standart ve Medya Kodlayıcısı Premium iş akışı veya Analiz altyapıları Algılayıcı), en büyük boyutunu kısıtlama farkında olması gerekir. 

15 Mayıs 2017'dan sonra tek bir blob için desteklenen en büyük boyutu 195 TB - bu sınırından dosya largers olan, görev başarısız olur. Bu sınır adresi için bir düzeltme çalışıyoruz. Ayrıca, varlık en büyük boyutunu kısıtlama gibidir.

| Medya Ayrılmış Birimi türü | En büyük giriş boyutu (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
