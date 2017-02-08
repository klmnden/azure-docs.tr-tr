>[!NOTE]
>Sabitlenmemiş kaynaklar için bir destek bileti açarak kotaların yükseltilmesini isteyebilirsiniz. Daha yüksek sınırlar elde etmek için başka Azure Media Services hesapları **oluşturmayın**.

| Kaynak | Varsayılan Sınır | 
| --- | --- | 
| Tek bir abonelikte Azure Media Services (AMS) hesabı sayısı | 25 (sabit) |
| AMS hesabı başına varlık sayısı | 1.000.000|
| İş başına zincirleme görev sayısı | 30 (sabit) |
| Görev başına varlık sayısı | 50 |
| İş başına varlık sayısı | 100 |
| AMS hesabı başına iş sayısı | 50.000<sup>2</sup> |
| Tek seferde bir varlıkla ilişkilendirilen benzersiz bulucu sayısı | 5<sup>4</sup> |
| AMS hesabı başına canlı kanal sayısı |5|
| Kanal başına durdurulmuş durumdaki program sayısı |50|
| Kanal başına çalışır durumdaki program sayısı |3|
| AMS hesabı başına çalışır durumdaki akış uç noktaları|2|
| Akış uç noktası başına akış birimleri |10 |
| AMS hesabı başına Medya Ayrılmış Birimleri (RU’lar) |25 (S1, S2)<br/>10 (S3) <sup>1</sup> | 
| Depolama hesapları | 1.000<sup>5</sup> (sabit) |
| İlkeler | 1.000.000<sup>6</sup> |
| Dosya boyutu| Bazı senaryolarda, Media Services ile işleme için desteklenen dosya boyutlarına yönelik üst sınır uygulanır. <sup>7</sup> |
 
<sup>1</sup> S3 RU’ları Hindistan Batı bölgesinde kullanılamaz.

<sup>2</sup> Bu sayı kuyruğa alınan, tamamlanan, etkin ve iptal edilmiş işleri içerir. Silinmiş işleri içermez. **IJob.Delete** veya **DELETE** HTTP isteğini kullanarak eski işleri silebilirsiniz.

<sup>3</sup> İş varlıklarını listeleme isteği yaparken istek başına en fazla 1.000 adet döndürülür. Gönderilen tüm işleri izlemeniz gerekiyorsa, [OData sorgu seçeneklerinde](http://msdn.microsoft.com/library/gg309461.aspx) açıklandığı gibi üste/atla seçeneğini kullanabilirsiniz.

<sup>4</sup> Bulucular kullanıcı başına erişim denetimini yönetmek için tasarlanmamıştır. Ayrı kullanıcılara farklı erişim hakları vermek için Digital Rights Management (DRM) çözümlerini kullanın. Daha fazla bilgi için [bu](../articles/media-services/media-services-content-protection-overview.md) bölüme bakın.

<sup>5</sup> Depolama hesapları aynı Azure aboneliğine ait olmalıdır.

<sup>6</sup> Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu politikası veya ContentKeyAuthorizationPolicy için). 

>[!NOTE]
> Kullandığınız günler / erişim izinleri / vb. her zaman aynıysa, aynı ilke kimliğini kullanmanız gerekir.

<sup>7</sup>Azure Media Services içindeki bir Varlığa hizmetimizdeki medya işlemcilerinden biriyle işleme amacıyla içerik yüklüyorsanız (örn. Media Encoder Standard ve Media Encoder Premium İş Akışı gibi kodlayıcılar veya Face Detector gibi analiz altyapıları), aşağıdaki sınırları bilmeniz gerekir. 

| Medya Ayrılmış Birimi türü | En Büyük Dosya Boyutu (GB)| 
| --- | --- | 
|S1 | 35|
|S2 | 75|
|S3 | 260|


<!--HONumber=Jan17_HO5-->


