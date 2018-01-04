---
title: "Azure Media Services kavramları | Microsoft Docs"
description: "Bu konu Azure Media Services kavramlarına genel bir fikir veren"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: dcefc8bc-e2ea-4b38-a643-9010f4436fb5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: juliako
<<<<<<< HEAD
ms.openlocfilehash: fb21280921f353d2300767059290a1a8fac05e71
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: HT
=======
ms.openlocfilehash: bb02aaf541d2d2f4b1206136847af2b46621501d
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="azure-media-services-concepts"></a>Azure Media Services kavramları
Bu konu en önemli Media Services kavramları hakkında genel bakış sağlar.

## <a name="a-idassetsassets-and-storage"></a><a id="assets"/>Varlıklar ve depolama
### <a name="assets"></a>Varlıklar
Bir [varlık](https://docs.microsoft.com/rest/api/media/operations/asset) dijital dosyaları (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta verileri içerir. Dijital dosyalar bir varlığa karşıya yükledikten sonra medya kodlama ve iş akışları akış Hizmetleri'nde kullanılabilir.

Bir varlık Azure depolama hesabındaki bir blob kapsayıcısını eşlenen ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Sayfa bloblarını Azure Media Services tarafından desteklenmiyor.

Karşıya yükleyin ve bir varlığı depolamak medya içeriği karar verirken aşağıdaki maddeler geçerlidir:

* Bir varlık yalnızca bir tek, benzersiz örneği medya içeriği içermelidir. Örneğin, bir tek düzen TV bölüm, film veya reklam.
* Bir varlık, birden çok yorumlama veya bir görsel ve işitsel dosyasının düzenlemeleri içeremez. Birden fazla TV bölüm, tanıtım veya bir varlık içindeki tek bir üretimden birden fazla Kamera Açısı depolamak bir varlık hatalı bir kullanımını bir örneği çalışıyor. Birden çok yorumlama veya düzenlemeleri görsel ve işitsel bir dosyanın bir varlığı depolama akış ve daha sonra iş akışında varlık teslim güvenli hale getirme kodlama işlerini göndermenin sorunlar neden olabilir.  

### <a name="asset-file"></a>Varlık dosyası
Bir [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) bir blob kapsayıcısında depolanan asıl ses veya video dosyası temsil eder. Bir varlık dosyası her zaman bir varlıkla ilişkilidir ve bir varlığı bir veya daha çok dosyalar içerebilir. Bir varlık dosyası nesne bir blob kapsayıcısında dijital bir dosyayla ilişkili değilse Media Services Kodlayıcısı görev başarısız olur.

**AssetFile** örneği ve gerçek medya dosyası olan iki farklı nesneler. Medya dosyasının gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

Medya hizmeti API'ları kullanmadan Media Services tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.

### <a name="asset-encryption-options"></a>Varlık şifreleme seçenekleri
Karşıya yükleme, depolama ve teslim etmek istediğiniz içerik türüne bağlı olarak, Media Services aralarından seçim yapabileceğiniz çeşitli şifreleme seçenekleri sağlar.

>[!NOTE]
>Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçenek kullanıldığında, içeriğinizin aktarım veya deposunda kalan korunmaz.

Bir MP4 iletmeyi planlıyorsanız, kullanım aşamalı indirme kullanarak bu seçenek, içerik yüklemek için.

**StorageEncrypted** – Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek için bu seçeneği kullanın ve Azure depolanır şifrelenen depolama birimine yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenmemiş ve kodlamadan önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıkış varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, güçlü şifreleme REST diskte, yüksek kaliteli giriş medya dosyalarınızın güvenliğini sağlamak istediğiniz durumdur. 

Media Services, içeriğinizi teslim etmek istediğiniz nasıl bilmesi için depolama şifrelenmiş varlık teslim etmek için varlığın teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışı önce akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini (örneğin, AES, PlayReady veya şifreleme) kullanarak içeriğinizi akışlarını. 

**CommonEncryptionProtected** -şifrelemek (veya zaten şifrelenmiş karşıya yüklemek) istiyorsanız bu seçeneği kullanın ortak şifreleme veya PlayReady DRM (örneğin, PlayReady DRM ile korunan kesintisiz akış) ile içerik.

**EnvelopeEncryptionProtected** – korumak (veya zaten korumalı karşıya yüklemek) istiyorsanız bu seçeneği kullanın HTTP canlı akışı (Gelişmiş Şifreleme Standardı (AES ile) şifrelenmiş HLS). Zaten AES ile şifrelenmiş HLS yüklüyorsanız bu Transform Manager tarafından şifrelenmiş gerekir.

### <a name="access-policy"></a>Erişim ilkesi
Bir [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) (örneğin, okuma, yazma ve liste) izinler ve erişim süresi bir varlık için tanımlar. Genellikle, bir varlıkta bulunan dosyalara erişmek için kullanılacak bir Bulucu için AccessPolicy nesneyi geçip geçmeyeceğini.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="blob-container"></a>BLOB kapsayıcısı
Bir blob kapsayıcı BLOB'lar kümesinin bir gruplandırma sağlar. BLOB kapsayıcıları Media Services'de, erişim denetimi ve paylaşılan erişim imzası (SAS) bulucular varlıklar üzerinde sınır noktası olarak kullanılır. Bir Azure Storage hesabı sınırsız sayıda blob kapsayıcı içerebilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.

>[!NOTE]
> Medya hizmeti API'ları kullanmadan Media Services tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.
> 
> 

### <a name="a-idlocatorslocators"></a><a id="locators"/>Belirleyicileri
[Bulucu](https://docs.microsoft.com/rest/api/media/operations/locator)s bir varlıkta bulunan dosyalara erişmek için bir giriş noktası sağlar. Bir erişim ilkesi izinleri ve bir istemci belirli bir varlık erişimi olduğunu süresini tanımlamak için kullanılır. Farklı bulucular farklı başlangıç zamanlarını ve bağlantı türleri farklı istemcilere tüm kullanırken aynı izni ve süresi ayarları sağlayabilir, bulucular çoğa bir ilişki bir erişim ilkesi ile olabilir; Ancak, Azure storage services tarafından ayarlanan bir paylaşılan erişim ilkesi kısıtlama nedeniyle, aynı anda belirli bir varlıkla ilişkilendirilen beşten fazla benzersiz bulucular sahip olamaz. 

Media Services iki tür Bulucuyu destekler: karşıya yükleme veya indirme medya dosyaları to\from Azure depolama için kullanılan OnDemandOrigin bulucuları ve medya (örneğin, MPEG DASH, HLS veya kesintisiz akış) akışla aktarmak veya aşamalı medya ve SAS URL bulucular indirmek için kullanılır. 

>[!NOTE]
>Liste izni (AccessPermissions.List) bir OnDemandOrigin Bulucu oluştururken kullanılmamalıdır. 

### <a name="storage-account"></a>Depolama hesabı
Tüm Azure Storage erişimi bir depolama hesabıyla yapılır. Bir ortam hizmet hesabı bir veya daha fazla depolama hesapları ile ilişkilendirebilirsiniz. Altında 500 TB depolama hesabı başına toplam kendi boyuttur sürece bir hesapta sınırsız sayıda kapsayıcı, olabilir.  Media Services birden çok depolama hesaplarını yönetebilir ve Yük Dengelemesi varlıklarınızı dağıtım bu hesaplar için karşıya yükleme sırasında ölçümleri veya rastgele dağıtım göre izin vermek için SDK düzeyi araçları sağlar. Daha fazla bilgi için bkz. Working with [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>İşler ve görevler
A [iş](https://docs.microsoft.com/rest/api/media/operations/job) genelde kullanılan işleme (örneğin, dizin veya kodlamak) bir ses/video sunu. Birden çok videolar işleme varsa, her video kodlanması için bir iş oluşturun.

Bir işi gerçekleştirilecek işlenmesi hakkındaki meta veriler içeriyor. Her işi bir veya daha fazla içeren [görev](https://docs.microsoft.com/rest/api/media/operations/task)giriş varlıklarını bir atomik işlem görevi belirtin s çıktı varlıklar, medya işlemcisi ve ilişkili ayarları. Bir iş içindeki görevlerin zincirleme yapılabilir birlikte, burada bir görevin çıkış varlığına verilen giriş varlık sonraki görev. Bu şekilde bir iş tüm medya sunumu için gerekli işlemleri içerebilir.

## <a id="encoding"></a>Kodlama
Azure Media Services, bulutta medya kodlama için birden çok seçenekler sağlar.

Media Services ile başlıyor, codec bileşenleri ve dosya biçimlerini arasındaki farkı anlamak önemlidir.
Codec bileşenleri sıkıştırma/açma algoritmaları dosya biçimleri sıkıştırılmış video tutan kapsayıcılar ise uygulayan yazılımlardır.

Media Services, bu yeniden paketlemeden zorunda kalmadan, bit hızı Uyarlamalı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services tarafından (MPEG DASH, HLS, kesintisiz akış) desteklenen akış biçimlerinde göndermenizi sağlayan dinamik paketleme sağlar. Akış biçimlerine.

Yararlanmak için [dinamik paketleme](media-services-dynamic-packaging-overview.md), Ara (kaynak) dosyanızı Uyarlamalı bit hızlı MP4 veya uyarlamalı bit hızlı kesintisiz akış dosyaları kümesine kodlayın ve en az bir standart veya premium akış uç noktası olması gerekir durumu başlatıldı.

Media Services, bu makalede açıklanan aşağıdaki isteğe bağlı kodlayıcılar destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Desteklenen kodlayıcılar hakkında daha fazla bilgi için bkz: [kodlayıcılar](media-services-encode-asset.md).

## <a name="live-streaming"></a>Canlı Akış
Azure Media Services ile bir kanal canlı akış içeriğinin işlemek için bir işlem hattı temsil eder. Bir kanal Canlı giriş akışları iki yoldan biriyle alır:

* Bir şirket içi gerçek zamanlı Kodlayıcı, Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) kanala gönderir. Çoklu bit hızlı kesintisiz akış çıkışı aşağıdaki gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, düşünün iletişimleri, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıkışı: Adobe Flash Live Kodlayıcı, Telestream Wirecast, Teradek, Haivision ve Tricaster kodlayıcılar. Alınan akışların kanallardan başka kodlama dönüştürme ve kodlama geçirin. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Tek bit hızlı akış (aşağıdaki biçimlerden birinde: RTP (MPEG-TS)), RTMP veya kesintisiz akış (parçalanmış MP4)) Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala gönderilir. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

### <a name="channel"></a>Kanal
Media Services [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlemekten sorumlu. Bir giriş uç noktası bir kanal sağlar (URL alma) için dinamik bir dönüştürücü ardından sağlayın. Kanal, Canlı giriş akışları Canlı dönüştürücü alır ve bir veya daha fazla Akış akış için kullanılabilir hale getirir. Kanal Önizleme ve başka bir işleme ve teslim önce akışınızı doğrulamak için kullanacağınız bir önizleme uç noktası (Önizleme URL) de sağlar.

Kanal oluşturduğunuzda alma URL'si ve önizleme URL'sini alabilirsiniz. Bu URL'leri almak için kanal başlatılmış durumda olması gerekmez. Veri canlı bir dönüştürücü kanal dağıtmaya başlamak için hazır olduğunuzda, kanal başlatılması gerekir. Veri alma Canlı dönüştürücü başladıktan sonra akışınızın önizlemesini.

Her Media Services hesabı birden fazla kanal, birden çok program ve birden çok akış içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, bir veya daha fazla kanala StreamingEndpoint Hizmetleri ayrılabilir. Tüm StreamingEndpoint herhangi bir kanaldan çeker.

### <a name="program-event"></a>Program (olay)
A [Program (olay)](https://docs.microsoft.com/rest/api/media/operations/program) yayımlama ve canlı akıştaki kesimleri depolanmasını denetlemenizi sağlar. Kanallar, programları (olayları) yönetir. Kanal ve Program arasındaki ilişki, burada bir kanal sabit bir içerik akışının bulunduğu ve bir programın bu kanalda zamanlanmış bir olayı kapsamlıdır geleneksel bir ortama benzer.
Program için kaydedilen içeriği ayarlayarak korumak istediğiniz saat sayısını belirtebilirsiniz **ArchiveWindowLength** özelliği. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength de maksimum zaman istemcilerine geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her bir program (olay) bir varlıkla ilişkilidir. Programı yayımlamak için ilişkili varlığa yönelik bir Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Daha fazla bilgi için bkz.

* [Gerçek zamanlı kodlama gerçekleştirmek Azure Media Services ile için etkinleştirilmiş kanallar ile çalışma](media-services-manage-live-encoder-enabled-channels.md)
* [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)
* [Kotalar ve kısıtlamaları](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>İçerik koruma
### <a name="dynamic-encryption"></a>Dinamik şifreleme
Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdan güvenli olanak sağlar. Media Services dinamik olarak Gelişmiş Şifreleme Standardı ((128-bit şifreleme anahtarları kullanılarak) AES) ve ortak şifreleme kullanarak PlayReady ve/veya Widevine DRM (CENC) ile şifrelenmiş içeriğinizi teslim etmenizi sağlar. Media Services de AES anahtarları ve PlayReady lisansları yetkili istemcilere teslim etmek için bir hizmet sunar.

Aşağıdaki akış biçimlerine şifrelemek şu anda: HLS, MPEG DASH ve kesintisiz akış. Aşamalı indirme şifrelenemiyor.

Media Services'ın bir varlık şifrelemek istiyorsanız, bir şifreleme anahtarı (CommonEncryption veya EnvelopeEncryption), varlıkla ilişkilendirin ve anahtarı için yetkilendirme ilkelerini de yapılandırmanız gerekir.

Bir depolama şifrelenmiş varlığı akışla aktarmak istiyorsanız, nasıl, varlık teslim etmek istediğinizi belirtmek için varlığın teslim ilkesini yapılandırmanız gerekir.

Bir akış player tarafından istendiğinde Media Services belirtilen anahtarı dinamik olarak bir zarf şifrelemeyle (AES) veya ortak şifreleme (PlayReady veya Widevine) kullanarak içeriğinizi şifrelemek için kullanır. Akış şifresini çözmek için player anahtar teslim hizmetinden anahtarı ister. Kullanıcının anahtarını almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet değerlendirir.

### <a name="token-restriction"></a>Belirteç kısıtlama
İçerik anahtarı yetkilendirme ilkesini bir veya daha fazla yetkilendirme kısıtlamaları olabilir: açın, simge restriction ya da IP kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit Web belirteçleri (SWT) biçimini ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Media Services, güvenli belirteç hizmetleri sağlamaz. Özel bir STS oluşturabilir veya Microsoft Azure ACS sorunu belirteçleri yararlanın. STS, belirteç kısıtlamasına yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri imzalı bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti istenen anahtarı (veya lisans) istemcinin belirtecin geçerli olup olmadığını ve bu belirteci yapılandırmış anahtarı (veya lisans) için talepleri döndürür.

Belirteç yapılandırma İlkesi sınırlı olduğunda, birincil doğrulama anahtarı, veren ve İzleyici parametreleri belirtmeniz gerekir. Birincil doğrulama anahtar belirteci ile imzalandığı anahtarı içerir, veren belirtecini veren güvenli belirteç hizmeti. Kaynak belirteci erişimini yetkilendirir veya (bazen kapsam denir) İzleyici belirteç amacı açıklanır. Media Services anahtar teslim hizmeti, bu değerleri belirteci şablon değerleri eşleştiğini doğrular.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [İçerik genel koruma](media-services-content-protection-overview.md)
- [AES-128 ile koruma](media-services-protect-with-aes128.md)
- [PlayReady/Widevine ile koruma](media-services-protect-with-playready-widevine.md)

## <a name="delivering"></a>Teslim etme
### <a name="a-iddynamicpackagingdynamic-packaging"></a><a id="dynamic_packaging"/>Dinamik paketleme
Media Services ile çalışırken, Uyarlamalı bit hızlı MP4 kümesi mezzanine dosyalarınızı kodlamak ve ardından istenen biçim kullanmaya kümesi dönüştürmek için önerilir [dinamik paketleme](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Akış uç noktası
Bir StreamingEndpoint istemci oynatıcı uygulaması için doğrudan veya bir içerik teslim ağı (CDN) için daha fazla (Azure Media Services artık Azure CDN tümleştirme sağlar.) dağıtım için içerik ileten bir akış hizmetini temsil eder Bir akış uç noktası hizmetinden giden akış canlı akış veya isteğe bağlı varlığı Media Services hesabınızda olabilir. Media Services müşterileri ihtiyaçlarına göre **Standart** bir akış uç noktası veya bir veya daha fazla **Premium** akış uç noktası seçer. Akış uç noktası standart çoğu akış iş yükleri için uygundur. 

Standart Akış Uç Noktası çoğu akış iş yükü için uygundur. Standart akış uç noktaları neredeyse her aygıtı HLS, MPEG-DASH, kesintisiz akış yanı ve Microsoft PlayReady, Google Widevine, Apple Fairplay dinamik şifreleme içine dinamik paketleme aracılığıyla içeriğinizi teslim etmek için esneklik sunar ve AES128.  Bunlar ayrıca çok küçük Azure CDN tümleştirme yoluyla eşzamanlı görüntüleyiciler binlerce ile çok büyük izleyiciler için ölçeklendirin. Gelişmiş bir iş yükü varsa veya akış kapasite gereksinimlerinizi standart akış uç noktası üretilen iş hedeflerini uygun olmayan veya büyüyen işlemek için kapasite StreamingEndpoint hizmetinin denetlemek istediğiniz bant genişliği gerekiyor, için önerilir. Ölçek birimi (olarak da bilinen premium akış birimleri) ayırın.

Dinamik paketleme ve/veya dinamik şifreleme kullanılması önerilir.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Daha fazla bilgi için [bu](media-services-portal-manage-streaming-endpoints.md) konu başlığına bakın.

Varsayılan olarak, en fazla akış uç noktaları Media Services hesabınızda 2 olabilir. Daha yüksek bir sınır istemek için bkz: [kotaları ve kısıtlamaları](media-services-quotas-and-limitations.md).

StreamingEndpoint çalışır durumda olduğunda yalnızca faturalandırılır.

### <a name="asset-delivery-policy"></a>Varlık teslim ilkesini
Medya Hizmetleri içerik teslim akışındaki adımlardan biri olan yapılandırma [varlıklar için teslim ilkeleri ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)akışı istediğiniz. Media Services, varlık teslim edilmesini istediğiniz varlık teslim ilkesini bildirir: hangi Akış Protokolü varlığınız dinamik olarak paketlenir (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü için), dinamik olarak Varlığınızı şifrelemek isteyip istemediğinizi ve nasıl içine (Zarf veya ortak şifreleme).

Varlığınızı akışı önce depolama şifrelenmiş varlık varsa, akış sunucusu depolama şifreleme kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışlarını. Örneğin, Gelişmiş Şifreleme Standardı (AES) şifreleme anahtarıyla şifrelenir, varlık teslim etmek için DynamicEnvelopeEncryption için ilke türünü ayarlayın. Depolama şifrelemesi kaldırmak ve varlık temiz akışını NoDynamicEncryption için ilke türünü ayarlayın.

### <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme, dosyanın tamamı indirilmeden önce media çalma başlatmanıza olanak verir. Bir MP4 dosyası yalnızca aşamalı olarak indirebilirsiniz.

>[!NOTE]
>Bunları aşamalı indirme için kullanılabilir olmasını istiyorsanız, şifrelenmiş varlıklar şifresini gerekir.

Aşamalı indirme URL'leri kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturarak, size varlık için temel yolu sağlar. Ardından MP4 dosyasının adını eklemek gerekir. Örneğin:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9B03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Akış URL'leri
İstemcilerin içeriğinize akış. Akış URL'lerini kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturulması, akış istediğiniz içerik içerir varlık için taban yol sunar. Ancak, bu içerik akışı edebilmek için daha fazla bu yolu değiştirmeniz gerekir. Akış bildirim dosyası için tam bir URL oluşturmak için konum belirleyici'nın yol değeri ve bildirimi (filename.ism) birleştirme dosya adı. Ardından, /Manifest ve (gerekirse) uygun bir biçim Konum Belirleyicisi yolunu ekleyin.

Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını sağlayabilirsiniz. Bunu yapmak için HTTPS ile akış URL'leri Başlat emin olun. Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.  

>[!NOTE]
>İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014 sonra oluşturduysanız yalnızca SSL üzerinden akışını sağlayabilirsiniz. Akış URL'leri Eylül 10 sonra oluşturulan akış uç noktalarını dayanır, URL "streaming.mediaservices.windows.net" (yeni biçimde) içerir. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL desteklemez. URL'niz eski biçimindedir ve SSL üzerinden akışla aktarmak istiyorsanız, yeni bir akış uç noktası oluşturun. SSL üzerinden içeriğinizin akışını sağlamak için yeni akış uç noktasında göre oluşturulan URL kullanın.

Aşağıdaki listede, farklı akış biçimlerine açıklar ve örnekler verilmektedir:

* Kesintisiz Akış

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ism/manifest

* MPEG DASH

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf) akış

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ism/manifest(Format=mpd-Time-CSF)

* Apple HTTP canlı akış (HLS) V4

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl) akış

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ism/manifest(Format=m3u8-aapl)

* Apple HTTP canlı akış (HLS) V3

{uç nokta adı media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3) akış

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8B70-203e86b0df58/BigBuckBunny.ism/manifest(Format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

