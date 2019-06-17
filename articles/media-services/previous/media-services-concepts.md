---
title: Azure Media Services kavramları | Microsoft Docs
description: Bu konu Azure Media Services kavramları genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 2b28dde812dcce120c951730c27809f7f024e122
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64681551"
---
# <a name="azure-media-services-concepts"></a>Azure Media Services kavramları 

> [!NOTE]
> Media Services v2’ye herhangi bir yeni özellik veya işlevsellik eklenmemektedir. <br/>En son sürüm olan [Media Services v3](https://docs.microsoft.com/azure/media-services/latest/)’ü inceleyin. Ayrıca bkz [geçiş kılavuzuna v2'den v3](../latest/migrate-from-v2-to-v3.md)

Bu konu, en önemli Media Services kavramları hakkında genel bir bakış sağlar.

## <a name="a-idassetsassets-and-storage"></a><a id="assets"/>Varlıklar ve depolama
### <a name="assets"></a>Varlıklar
Bir [varlık](https://docs.microsoft.com/rest/api/media/operations/asset) dijital dosyalar (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta veriler içerir. Dijital dosyalar bir varlığa karşıya yüklendikten sonra medya kodlama ve iş akışları akış Hizmetleri'nde kullanılabilir.

Azure depolama hesabındaki blob kapsayıcısına bir varlık eşlenen ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Sayfa blobları, Azure Media Services tarafından desteklenmez.

Medya içeriği karşıya yüklemek ve bir varlığı depolamak için karar verirken aşağıdaki maddeler geçerlidir:

* Bir varlık yalnızca tek, benzersiz örnek medya içeriklerinin içermelidir. Örneğin, bir tek düzen bir TV bölümü, film veya reklam.
* Bir varlık, birden çok yorumlama veya bir görsel ve işitsel dosyasının düzenlemeleri içermemelidir. Birden fazla TV bölümü, tanıtım veya bir varlık içinde tek bir üretimden birden fazla kamera açısını depolamak üzere bir varlığın yanlış bir kullanım örneği çalışıyor. Birden çok yorumlama veya düzenlemeler bir görsel ve işitsel dosyasının bir varlığı depolama akış ve daha sonra iş akışında varlık dağıtımını güvenli hale getirme kodlama işi gönderiliyor zorluklarla neden olabilir.  

### <a name="asset-file"></a>Varlık dosyası
Bir [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) bir blob kapsayıcısında depolanan bir fiili video veya ses dosyasını temsil eder. Bir varlık dosyası her zaman bir varlıkla ilişkilidir ve bir varlık, bir veya daha çok dosya içerebilir. Bir varlık dosyası nesne bir blob kapsayıcısında dijital bir dosyayla ilişkili değilse, medya Hizmetleri Kodlayıcı görev başarısız olur.

**AssetFile** örneği ve gerçek medya dosyası olan iki farklı bir nesne. Medya dosyası gerçek medya içeriği içerirken AssetFile örneği medya dosyası hakkındaki meta verileri içerir.

Medya hizmet API'lerini kullanarak olmadan Media Services tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.

### <a name="asset-encryption-options"></a>Varlık şifreleme seçenekleri
Media Services, karşıya yükleyin, depolayın ve teslim etmek istediğiniz içerik türüne bağlı olarak, aralarından seçim yapabileceğiniz çeşitli şifreleme seçenekleri sağlar.

>[!NOTE]
>Şifreleme kullanılmaz. Varsayılan değer budur. Bu seçeneği tercih edildiğinde, içeriğinizi Aktarımdaki veya depolama bölgesinde, bekleyen korunmaz.

Bir MP4 iletmeyi planlıyorsanız, kullanım aşamalı indirme kullanarak bu seçeneği, içerik yüklemek için.

**StorageEncrypted** – Temizle içeriğinizi yerel olarak AES 256 bit şifreleme kullanarak şifrelemek için bu seçeneği kullanın ve Azure bunu depolandığı bekleme sırasında şifrelenmiş depolama alanına yükleyin. Depolama şifrelemesi ile korunan varlıklar otomatik olarak şifrelenerek ve kodlama önce şifrelenmiş bir dosya sistemine yerleştirilir ve isteğe bağlı olarak yeni çıktı varlığı şeklinde geri bir yüklemeden önce yeniden şifrelenir. Depolama şifrelemesinin birincil kullanım durumu, yüksek kaliteli girdi medya dosyalarınızın bekleyen güçlü şifrelemeyle diskte güvenliğini sağlamak istediğiniz durumdur. 

Bir depolama şifrelenmiş varlık teslim etmek için Media Services, içeriğinizi teslim etmek istediğiniz nasıl olduğunu bilmesi için varlık teslim ilkesini yapılandırmanız gerekir. Varlığınızı akışla önce akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini (örneğin, AES, PlayReady veya şifreleme) kullanarak içeriğinizi akışları. 

**CommonEncryptionProtected** -şifreleme (veya zaten şifrelenmiş karşıya yüklemek) istiyorsanız bu seçeneği kullanın. ortak şifreleme veya PlayReady DRM (örneğin, PlayReady DRM ile korunan kesintisiz akış) ile içerik.

**EnvelopeEncryptionProtected** – korumak (veya zaten korumalı karşıya yüklemek) istiyorsanız bu seçeneği kullanın. HTTP canlı akışı (Gelişmiş Şifreleme Standardı (AES ile) şifrelenmiş HLS). Önceden AES ile şifrelenmiş HLS yüklüyorsanız, Transform Manager tarafından şifrelenmiş gerekir.

### <a name="access-policy"></a>Erişim İlkesi
Bir [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) bir varlığa (örneğin, okuma, yazma ve liste) izinler ve erişim süresini tanımlar. Bir varlıkta bulunan dosyalara erişmek için kullanılacak bir Bulucu için genellikle bir AccessPolicy nesne geçirmeniz gerekir.

>[!NOTE]
>Farklı AMS ilkeleri için sınır 1.000.000 ilkedir (örneğin, Bulucu ilkesi veya ContentKeyAuthorizationPolicy için). Uzun süre boyunca kullanılmak için oluşturulan bulucu ilkeleri gibi aynı günleri / erişim izinlerini sürekli olarak kullanıyorsanız, aynı ilke kimliğini kullanmalısınız (karşıya yükleme olmayan ilkeler için). Daha fazla bilgi için [bu](media-services-dotnet-manage-entities.md#limit-access-policies) konu başlığına bakın.

### <a name="blob-container"></a>BLOB kapsayıcısı
Bir blob kapsayıcı bir dizi blobun bir gruplandırmasını sağlar. BLOB kapsayıcıları Media Services'de, erişim denetimi ve paylaşılan erişim imzası (SAS) bulucuları varlıklar üzerinde sınır noktası olarak kullanılır. Bir Azure depolama hesabında sınırsız sayıda blob kapsayıcıları içerebilir. Kapsayıcıda sınırsız sayıda blob depolanabilir.

>[!NOTE]
> Medya hizmet API'lerini kullanarak olmadan Media Services tarafından oluşturulan blob kapsayıcı içeriğini değiştirme denememeniz gerekir.
> 
> 

### <a name="a-idlocatorslocators"></a><a id="locators"/>Bulucular
[Bulucu](https://docs.microsoft.com/rest/api/media/operations/locator)bir varlıkta bulunan dosyalara erişmek için bir giriş noktası sağlar. Bir erişim ilkesi, bir istemci belirli bir varlık erişimi olduğunu süresi ve izinlerini tanımlamak için kullanılır. Farklı bulucular farklı başlangıç zamanlarını ve bağlantı türleri farklı istemciler için tüm kullanırken süresi ayarları ve aynı izni sağlayabilir, bulucular bir çoktan bire bir ilişkisi olan bir erişim ilkesi olabilir; Ancak, Azure depolama hizmetleri tarafından ayarlanmış bir paylaşılan erişim ilkesi kısıtlama nedeniyle, belirli bir varlık ile tek seferde ilişkilendirilen beşten fazla benzersiz Bulucu sayısı sahip olamaz. 

Media Services iki tür bulucuyu destekler: Medya (örneğin, MPEG DASH, HLS veya kesintisiz akış) akışla aktarmak veya aşamalı medya ve karşıya yükleme veya indirme medya dosyaları to\from Azure depolama için kullanılan SAS URL'SİNİN bulucuları indirmek için kullanılan OnDemandOrigin bulucuları. 

>[!NOTE]
>İzin (AccessPermissions.List) OnDemandOrigin bir Bulucu oluşturma sırasında kullanılmamalıdır. 

### <a name="storage-account"></a>Depolama hesabı
Tüm Azure depolama erişimi bir depolama hesabı üzerinden yapılır. Medya hizmeti hesabı, bir veya daha fazla depolama hesapları ile ilişkilendirebilirsiniz. Kendi toplam boyutu altında her depolama hesabı 500 TB olduğu sürece bir hesapta kapsayıcılar, sınırsız sayıda olabilir.  Media Services SDK düzeyi araçları birden çok depolama hesaplarını yönetme ve Yük Dengelemesi ölçümleri veya rastgele dağılım varlıklarınızı dağıtımını bu hesaplar için karşıya yükleme sırasında göre olanak sağlar. Daha fazla bilgi için bkz. Working with [Azure depolama](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>İşleri ve görevleri
A [iş](https://docs.microsoft.com/rest/api/media/operations/job) genellikle kullanılan işleme (örneğin, dizin veya kodlama) bir ses/video sunumu. Birden çok videoyu işliyorsa, her video kodlanmış bir iş oluşturun.

Bir işi gerçekleştirilecek işlenmesi hakkındaki meta verileri içerir. Her işi bir veya daha fazla içeren [görev](https://docs.microsoft.com/rest/api/media/operations/task)giriş varlıklarını bir atomik işlem görev belirtin s çıktı varlıkları, Medya işleyicisi ve ilişkili ayarları. Bir iş içinde görevler zincirleme yapılabilir birlikte, burada bir görevin çıkış varlık verildiğinde giriş varlığı sonraki görev. Bu şekilde bir işi işlerken bir medya sunumu için gerekli tüm içerebilir.

## <a id="encoding"></a>Kodlama
Azure Media Services, bulutta medya kodlama için birden fazla seçenek sağlar.

Media Services ile başlıyor, codec bileşenleri ve dosya biçimlerini arasındaki farkı anlamak önemlidir.
Codec sıkıştırma/sıkıştırma algoritmaları dosya biçimleri sıkıştırılmış görüntü tutan kapsayıcılardır ise uygulayan yazılımlardır.

Media Services, yeniden paketlemenize gerek kalmadan, Uyarlamalı bit hızı MP4 veya kesintisiz akış kodlanmış içeriğinizi Media Services tarafından (MPEG DASH, HLS, kesintisiz akış) desteklenen akış biçimlerinde göndermenize olanak tanıyan dinamik paketleme sağlar. Akış biçimlerinde.

Yararlanmak için [dinamik paketleme](media-services-dynamic-packaging-overview.md), Ara (kaynak) dosyanızı Uyarlamalı bit hızlı MP4 dosyası ya da Uyarlamalı bit hızlı kesintisiz akış dosyaları kümesine kodlayın ve en az bir standart veya premium akış uç noktası olmayan gerekir Durum başlatıldı.

Media Services, bu makalede açıklanan aşağıdaki şirket içi kodlayıcılar destekler:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium İş Akışı](media-services-encode-asset.md#media-encoder-premium-workflow)

Desteklenen kodlayıcılara hakkında daha fazla bilgi için bkz. [kodlayıcılar](media-services-encode-asset.md).

## <a name="live-streaming"></a>Canlı Akış
Azure Media Services için canlı akış içeriğinin işleneceği bir işlem hattı bir kanalı temsil eder. Bir kanal, Canlı giriş akışları iki yoldan biriyle alır:

* Bir şirket içi Canlı Kodlayıcı, Çoklu bit hızlı RTMP veya kesintisiz akış (parçalanmış MP4) kanalına gönderir. Çoklu bit hızlı kesintisiz akış çıktısı sağlayan şu gerçek zamanlı Kodlayıcıları kullanabilirsiniz: MediaExcel, Ateme, Imagine Communications, Envivio, Cisco ve Elemental. Şu gerçek zamanlı kodlayıcılar RTMP çıktısı: Adobe Flash Live Encoder, Telestream Wirecast, Teradek, Haivision ve Tricaster kodlayıcılar. Diğer kodlama dönüştürme ve kodlama, alınan akışların kanallar aracılığıyla geçirin. İstendiğinde, Media Services akışı müşterilere teslim eder.
* Tek bit hızlı akış (aşağıdaki biçimlerden birinde: RTMP veya kesintisiz akış (parçalanmış MP4)), Media Services ile gerçek zamanlı kodlama gerçekleştirmek için etkinleştirilmiş kanala gönderilir. Ardından Kanal, gelen tek bit hızlı akışın çoklu bit hızlı (uyarlamalı) bir video akışına gerçek zamanlı kodlanmasını gerçekleştirir. İstendiğinde, Media Services akışı müşterilere teslim eder.

### <a name="channel"></a>Kanal
Medya Hizmetleri'nde [kanal](https://docs.microsoft.com/rest/api/media/operations/channel)s canlı akış içeriğinin işlemekten sorumlu. Bir kanalın giriş uç noktası sağlar (alma URL'si) için Canlı bir işlenmesinde ardından sağlayın. Kanal, Canlı giriş akışları Canlı işlenmesinde alır ve bir veya daha fazla Akış akış için kullanılabilir hale getirir. Kanalları da daha fazla işleme edip teslime geçmeden önce akışınızı onaylama için kullandığınız bir önizleme uç noktası (Önizleme URL'si) sağlar.

Kanıl oluşturduğunuzda alma URL'si ve önizleme URL'sini alabilirsiniz. Bu URL'ler almak için kanal başlatılmış durumda olması gerekmez. Kanal canlı bir işlenmesinde veri göndermeye başlamak hazır olduğunuzda, kanal başlatılması gerekir. Veri alma, Canlı işlenmesinde başladıktan sonra akışınızın önizlemesini.

Her bir Media Services hesabı, birden çok kanalda, birden çok programları ve birden çok akış içerebilir. Bant genişliği ve güvenlik gereksinimlerine bağlı olarak, bir veya daha fazla kanala StreamingEndpoint Hizmetleri ayrılabilir. Herhangi bir kanaldan herhangi StreamingEndpoint çekmeden.

### <a name="program-event"></a>Program (olay)
A [Program (olay)](https://docs.microsoft.com/rest/api/media/operations/program) yayımlanması ve depolanmasını Canlı akıştaki segmentlerin denetlemenizi sağlar. (Olayları) programları kanallar yönetir. Kanal ve Program ilişki, burada bir kanal sabit bir içerik akışının vardır ve bir programın bu kanalda zamanlanmış bir olayı kapsamlı geleneksel Medyadaki benzerdir.
Saat ayarlayarak program için kaydedilen içeriği tutmak istediğinizi belirtebilirsiniz **ArchiveWindowLength** özelliği. Bu değer en az 5 dakika, en çok 25 saat olarak ayarlanabilir.

ArchiveWindowLength ayrıca en fazla saat istemcileri geçmişe geçerli Canlı konumdan gidebilecekleri. Olaylar belirtilen süre miktarından uzun sürebilir, ancak pencere uzunluğunun gerisine düşen içerik sürekli olarak atılır. Bu özelliğin bu değeri, istemci bildiriminin ne kadar uzayabileceğini de belirler.

Her bir program (olay) bir varlıkla ilişkilidir. Programı yayımlamak için ilişkili varlığa yönelik bir Bulucu oluşturmanız gerekir. Bu bulucuya sahip olmak, istemcilerinize sağlayabileceğiniz bir akış URL’si oluşturmanıza olanak tanır.

Bir kanal eşzamanlı çalışan üçe kadar olayı destekler, böylece aynı gelen akışın birden fazla arşivini oluşturabilirsiniz. Bu özellik, gerektiğinde bir olayın farklı kısımlarını yayımlamanıza ve arşivlemenize olanak tanır. Örneğin, iş gereksiniminiz bir programın 6 saatini arşivlemek ancak son 10 dakikasını yayınlamak olabilir. Bunu yapmak için, eşzamanlı olarak çalışan iki program oluşturmanız gerekir. Bir program olayı 6 saat arşivlemek için ayarlanır ancak program yayımlanmaz. Diğer program 10 dakika arşivlenecek şekilde ve bu program yayımlanır.

Daha fazla bilgi için bkz.

* [Gerçek zamanlı kodlama gerçekleştirmek ile Azure Media Services için etkinleştirilmiş kanallar ile çalışma](media-services-manage-live-encoder-enabled-channels.md)
* [Şirket İçi Kodlayıcılardan Çoklu Bit Hızlı Canlı Akış Alan Kanallar ile Çalışma](media-services-live-streaming-with-onprem-encoders.md)
* [Kotalar ve sınırlamalar](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>İçerik koruma
### <a name="dynamic-encryption"></a>Dinamik şifreleme
Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlar. Media Services dinamik olarak Gelişmiş Şifreleme Standardı ((128 bit şifreleme anahtarlarını kullanarak) AES) ve PlayReady ve/veya Widevine DRM kullanarak genel şifreleme (CENC) ile şifrelenmiş içerik dağıtmanıza olanak sağlar. Media Services, yetkili istemcilere AES anahtarları ve PlayReady lisans sunma için bir hizmet de sağlar.

Şu anda, akış biçimlerine'şifreleme de yapabilirsiniz: HLS, MPEG DASH ve kesintisiz akış. İlerlemeli indirmeler şifrelenemiyor.

Media Services için bir varlık şifrelemek istiyorsanız bir şifreleme anahtarı (CommonEncryption veya EnvelopeEncryption) Varlığınızı ile ilişkilendirmek ve anahtar için yetkilendirme ilkelerini de yapılandırmanız gerekir.

Bir depolama şifrelenmiş varlığı akışla aktarmak istiyorsanız, varlık teslim nasıl istediğinizi belirtmek için varlık teslim ilkesini yapılandırmanız gerekir.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services belirtilen anahtarı bir zarf şifreleme (AES ile) ya da ortak şifreleme (PlayReady veya Widevine) kullanarak içeriğinizi dinamik olarak şifrelemek için kullanır. Player akış şifresini çözmek için anahtar anahtar teslim hizmetinden ister. Kullanıcı anahtarı almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen Yetkilendirme İlkeleri hizmet tarafından değerlendirilir.

### <a name="token-restriction"></a>Belirteç kısıtlama
İçerik anahtarı yetkilendirme ilkesinin bir veya daha fazla yetkilendirme kısıtlaması olabilir: açın, belirteç kısıtlama veya IP kısıtlaması. Belirteç kısıtlamalı ilkenin beraberinde bir Güvenli Belirteç Hizmeti (STS) tarafından verilmiş bir belirteç bulunmalıdır. Media Services, basit Web belirteçleri (SWT) biçimi ve JSON Web Token (JWT) biçimlerindeki belirteçleri destekler. Media Services, güvenli belirteç hizmeti sağlamaz. Özel STS oluşturabilirsiniz. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar teslim hizmeti, belirteci geçerliyse istemci ve talepleri bu belirteci yapılandırmış anahtar (veya lisans) için istenen anahtar (veya lisans) döndürür.

Belirteç yapılandırma kısıtlanan ilkeyi kullanırken birincil doğrulama anahtarı ve sertifikayı veren İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteci ile imzalanmış kayıt anahtarını içeren, veren belirtecini veren güvenli belirteç hizmeti. Belirtecin amacı (kapsam olarak da adlandırılır) İzleyici açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

Daha fazla bilgi için aşağıdaki makalelere bakın:
- [İçerik genel koruma](media-services-content-protection-overview.md)
- [AES-128 ile koruma](media-services-protect-with-aes128.md)
- [PlayReady/Widevine ile koruma](media-services-protect-with-playready-widevine.md)

## <a name="delivering"></a>Teslim etme
### <a name="a-iddynamicpackagingdynamic-packaging"></a><a id="dynamic_packaging"/>Dinamik paketleme
Media Services ile çalışırken, Uyarlamalı bit hızı MP4 kümesi, Ara dosyaları olarak kodlayın ve daha sonra istediğiniz biçimi kullanarak küme dönüştürmek için önerilir [dinamik paketleme](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Akış uç noktası
Bir StreamingEndpoint içeriği doğrudan bir istemci Yürütücü uygulamasına veya daha fazla dağıtım (Azure Media Services şimdi Azure CDN tümleştirmesi sağlar.) bir içerik teslim ağı'için (CDN) teslim eden bir akış hizmetini temsil eder. Bir akış uç noktası hizmetinize giden akıştan canlı akış ve isteğe bağlı varlığı Media Services hesabınızda olabilir. Media Services müşterileri ihtiyaçlarına göre **Standart** bir akış uç noktası veya bir veya daha fazla **Premium** akış uç noktası seçer. Standart akış uç noktası çoğu akış iş yükü için uygundur. 

Standart Akış Uç Noktası çoğu akış iş yükü için uygundur. Standart akış uç noktalarını HLS, MPEG-DASH ve kesintisiz akış hem de Microsoft PlayReady, Google Widevine, Apple Fairplay, dinamik şifreleme ile dinamik paketleme ile neredeyse tüm cihazlara içerik teslim etmek için esneklik sunar ve AES128.  Bunlar ayrıca çok küçük büyük hedef kitlelerine binlerce eş zamanlı görüntüleyiciler Azure CDN Tümleştirmesi ile ölçeklendirin. Bir Gelişmiş iş yükünüz varsa, akış kapasitesi gereksinimleriniz standart akış uç noktası çıkış hedeflerine uygun olmayan veya büyüyen işlemek için StreamingEndpoint hizmetinin kapasitesini denetlemek istediğiniz bant genişliği gerekiyor, ancak önerilir Ölçek birimi (diğer adıyla premium akış birimleri) ayırın.

Dinamik paketleme ve/veya dinamik şifreleme kullanmanız önerilir.

>[!NOTE]
>AMS hesabınız oluşturulduğunda hesabınıza **Durdurulmuş** durumda bir **varsayılan** akış uç noktası eklenir. İçerik akışını başlatmak ve dinamik paketleme ile dinamik şifrelemeden yararlanmak için içerik akışı yapmak istediğiniz akış uç noktasının **Çalışıyor** durumda olması gerekir. 

Daha fazla bilgi için [bu](media-services-portal-manage-streaming-endpoints.md) konu başlığına bakın.

Varsayılan olarak en fazla akış uç noktaları, Media Services hesabınızdaki 2 olabilir. Daha yüksek bir sınır istemek için bkz [kotaları ve sınırlamaları](media-services-quotas-and-limitations.md).

StreamingEndpoint çalışır durumda olduğunda yalnızca faturalandırılırsınız.

### <a name="asset-delivery-policy"></a>Varlık teslim İlkesi
Media Services içerik teslim iş akışındaki adımlar biri yapılandırma [varlıklar teslim ilkelerini](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy)akışla istediğiniz. Varlık teslim ilkesini, varlık teslim edilmesini istediğiniz Media Services bildirir: dinamik olarak şifrelemek isteyip istemediğinizi hangi akış protokolüne varlığınız dinamik olarak (örneğin, MPEG DASH, HLS, kesintisiz akış veya tümü için), paketlenmesi gereken varlığınız ve nasıl (Zarf veya ortak şifreleme).

Şifrelenmiş depolama varlık Varlığınızı akışla önce varsa, akış sunucusu depolama şifrelemesi kaldırır ve belirtilen teslim ilkesini kullanarak içeriğinizi akışları. Örneğin, Gelişmiş Şifreleme Standardı (AES) şifreleme anahtarıyla şifrelenmiş varlığınız sunmak için DynamicEnvelopeEncryption için ilke türünü ayarlayın. Depolama şifrelemesi kaldırmak ve açık bir varlıkta akış için NoDynamicEncryption için ilke türünü ayarlayın.

### <a name="progressive-download"></a>Aşamalı indirme
Aşamalı indirme, tüm dosya indirilmeden önce medya oynatmayı başlatmak sağlar. Aşamalı olarak yalnızca bir MP4 dosyası indirebilirsiniz.

>[!NOTE]
>Bunları aşamalı indirme için kullanılabilir olmasını isterseniz, şifrelenmiş varlıklar dosyanın şifresini çözmelisiniz.

Aşamalı indirme URL'leri ile kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturarak, varlık için temel yol sağlar. Ardından MP4 dosyası adını eklemek gerekir. Örneğin:

http://amstest1.streaming.mediaservices.windows.net/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Akış URL'leri
İstemcilerin içeriğinize akış. Akış URL'lerini kullanıcılara sağlamak için önce bir OnDemandOrigin Bulucu oluşturmanız gerekir. Bulucu oluşturarak, içerik akışı yapmak istediğiniz içeren bir varlık için temel yol sağlar. Ancak, bu içerik akışı yapmak için bu yolu daha da değiştirmeniz gerekir. Akış bildirim dosyasına tam bir URL oluşturmak için konum yol değeri ve ' % s'bildirimi (filename.ism) birleştirme dosya adı. Ardından, /Manifest ve (gerekirse) uygun bir biçim Bulucu yolunu ekleyin.

Ayrıca, bir SSL bağlantısı üzerinden içeriğinizin akışını yapabilirsiniz. Bunu yapmak için akış URL'leri HTTPS ile Başlat emin olun. Şu anda AMS SSL ile özel etki alanlarını desteklemiyor.  

>[!NOTE]
>İçeriğinizi teslim etmek istediğiniz akış uç noktası 10 Eylül 2014'ten sonra oluşturduysanız yalnızca SSL üzerinden akışını yapabilirsiniz. 10 Eylül oluşturulan akış uç noktaları, akış URL'leri dayalı URL "streaming.mediaservices.windows.net" (yeni biçimde) içerir. "Origin.mediaservices.windows.net" (eski biçimde) içeren akış URL'leri SSL'yi desteklemez. URL'NİZİN biçimi eski olduğundan ve SSL üzerinden akışını yapmak istiyorsanız, yeni bir akış uç noktası oluşturun. SSL üzerinden içerik akışı için yeni akış uç noktasında göre oluşturulan URL'lerini kullanın.

Aşağıdaki listede, farklı akış biçimleri açıklanır ve örnekler verilmektedir:

* Kesintisiz Akış

{akış uç noktası adı-media services hesabı adı}.streaming.mediaservices.windows.net/{konum kimliği}/{dosya adı}.ism/Manifest

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest

* MPEG DASH

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf)

* Apple HTTP canlı akış (HLS) V4

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl)

* Apple HTTP canlı akış (HLS) V3

{Akış uç noktası adı-media services hesabı name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http:\//testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

