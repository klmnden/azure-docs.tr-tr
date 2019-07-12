---
title: Media Services dinamik şifreleme - Azure'ı kullanarak, içerik koruma | Microsoft Docs
description: Bu makalede, dinamik şifreleme ile içerik koruma genel bir bakış sağlar. Akış protokolleri ve şifreleme türleri hakkında konuşuyor.
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
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ee65aa5753d9967022ca91a66433b11761b73a32
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67835804"
---
# <a name="content-protection-with-dynamic-encryption"></a>Dinamik şifreleme ile içerik koruma

Azure Media Services, depolama, işleme ve teslim üzerinden bilgisayarınıza çıkışında medyanızdaki güvenliğini sağlamak için kullanabilirsiniz. Media Services sayesinde, Gelişmiş Şifreleme Standardı (AES-128) veya üç ana dijital hak yönetimi (DRM) sistemlerinden ile dinamik olarak şifrelenmiş canlı ve isteğe bağlı içerik teslim edebilirsiniz: Microsoft PlayReady, Google Widevine ve FairPlay Apple. Media Services de AES anahtarları ve DRM sunmaya yönelik bir hizmet sağlar (PlayReady, Widevine ve FairPlay) lisansları yetkili istemcilere. 

Aşağıdaki resimde Media Services content protection iş akışı gösterilmektedir: 

![İçerik koruma](./media/content-protection/content-protection.svg)

&#42;*dinamik şifreleme, AES-128 "şifresiz anahtar" ve CBCS CENC destekler. Ayrıntılar için destek matrisi bkz [burada](#streaming-protocols-and-encryption-types).*

Bu makalede, kavramlar ve terminoloji content protection ile Media Services anlamak için ilgili açıklanmaktadır.

## <a name="main-components-of-a-content-protection-system"></a>Bir içerik koruma sisteminin ana bileşenleri

"Content protection" sistem/uygulama tasarımınızı başarıyla tamamlamak için kapsamı çalışmasının tam olarak anlamak için gerekir. Aşağıdaki liste, üç bölümden uygulamanız gereken genel bir bakış sağlar. 

1. Azure Media Services kod
  
   [DRM](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs) örnek, birden çok DRM sistemiyle .NET ile Media Services v3 uygulamak nasıl gösterir. Ayrıca, Media Services lisans/anahtar teslim hizmetin nasıl kullanılacağını gösterir. Her bir varlığı birden fazla şifreleme türü (AES-128, PlayReady, Widevine, FairPlay) ile şifreleyebilirsiniz. Birlikte kullanılabilecek türler hakkında bilgi almak için bkz. [Akış protokolleri ve şifreleme türleri](#streaming-protocols-and-encryption-types).
  
   Örnekte gösterildiği nasıl yapılır:

   1. Oluşturma ve yapılandırma bir [içerik anahtar ilkeleri](content-key-policy-concept.md). Oluşturduğunuz bir **içerik anahtarı ilke** istemcileri sonuna (güvenli erişim sağlayan varlıklarınız için) bir içerik anahtarı nasıl teslim edildiğini yapılandırmak için.    

      * JWT talepleri temel yetkilendirme denetiminin mantık belirtme lisans teslim yetkilendirme tanımlayın.
      * Yapılandırma [PlayReady](playready-license-template-overview.md), [Widevine](widevine-license-template-overview.md), ve/veya [FairPlay](fairplay-license-overview.md) lisansları. Şablonları hakları ve izinleri her kullanılan benzeri DRM yapılandırmanıza olanak sağlar.

        ```
        ContentKeyPolicyPlayReadyConfiguration playReadyConfig = ConfigurePlayReadyLicenseTemplate();
        ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
        ContentKeyPolicyFairPlayConfiguration fairPlayConfig = ConfigureFairPlayPolicyOptions();
        ```
   2. Oluşturma bir [akış Bulucu](streaming-locators-concept.md) şifrelenmiş varlık akışını sağlamak için yapılandırılmış. 
  
      **Akış Bulucu** ilişkilendirilmesi gereken bir [akış ilke](streaming-policy-concept.md). Örnekte, "Predefined_MultiDrmCencStreaming" ilkeye StreamingLocator.StreamingPolicyName ayarladık. PlayReady ve Widevine şifrelemeler uygulanır, anahtar yapılandırılmış DRM lisansları temel kayıttan yürütme istemciye teslim edilir. Akışınızı CBCS (FairPlay) ile de şifrelemek isterseniz "Predefined_MultiDrmStreaming" öğesini kullanın.
      
      Akış Bulucu Ayrıca ilişkili olduğu **içerik anahtarı ilke** , tanımlandı.
    
   3. Bir test belirteci oluşturun.

      **GetTokenAsync** yöntemi bir test oluşturmak nasıl belirteci gösterir.
   4. Akış URL'sini oluşturun.

      **GetDASHStreamingUrlAsync** yöntemi akış URL'si oluşturmak nasıl gösterir. Bu durumda, URL akışları **DASH** içeriği.

2. AES veya DRM istemci oynatıcı. Bir oynatıcı SDK (yerel veya tarayıcı tabanlı) tabanlı bir video oynatıcı uygulaması aşağıdaki gereksinimleri karşılaması gerekir:
   * Oyuncu SDK gerekli DRM istemcileri destekler
   * Oyuncu SDK gerekli akış protokollerini destekler: Kesintisiz, DASH ve/veya HLS
   * Oyuncu SDK lisansı edinme istekte bir JWT belirteci geçirme işleyebilir olması gerekir
  
     Bir oynatıcı kullanarak oluşturabileceğiniz [Azure Media Player API'sine](https://amp.azure.net/libs/amp/latest/docs/). Kullanma [Azure Media Player ProtectionInfo API'sine](https://amp.azure.net/libs/amp/latest/docs/) farklı DRM platformlarda kullanılacak DRM teknolojileri belirtmek için.

     Test AES veya şifrelenmiş CENC (Widevine ve/veya PlayReady) için içerik, kullanabileceğiniz [Azure Media Player](http://aka.ms/azuremediaplayer). "Gelişmiş Seçenekler" seçeneğini tıklatın ve denetimi, şifreleme seçenekleri emin olun.

     FairPlay şifreli içeriği test etmek istediğiniz kullanırsanız [bu test yürütücünün](https://aka.ms/amtest). Player, Widevine, PlayReady, destekler ve benzeri FairPlay DRM ve bunun yanı sıra AES-128 şifresiz anahtar şifrelemesiyle koruyun. 
    
     Farklı benzeri DRM test etmek için doğru tarayıcı seçmeniz gerekebilir: Chrome/Opera/Firefox'ta Widevine, Microsoft Edge/ıe11 PlayReady için Safari macOS FairPlay için.

3. Belirteç Hizmeti (JSON Web Token (JWT) olarak arka uç kaynağına erişim için erişim belirteci verir STS), güvenli hale getirin. AMS lisans teslim hizmetleri arka uç kaynağı olarak kullanabilirsiniz. STS sahip aşağıdaki tanımlamak:

   * Veren ve İzleyici (veya kapsam)
   * İçerik koruma, iş gereksinimleri bağımlı olan talepleri
   * İmza doğrulaması için simetrik ya da asimetrik doğrulama
   * Anahtar geçişi desteği (gerekirse)

     Kullanabileceğiniz [bu STS araç](https://openidconnectweb.azurewebsites.net/DRMTool/Jwt) doğrulama anahtarı tüm 3 türlerini destekleyen STS, test için: simetrik, asimetrik veya Azure AD ile anahtar geçişi. 

> [!NOTE]
> Odaklanmanıza ve her parça (yukarıda açıklanmıştır) tam olarak test sonraki adımına geçmeden önce önerilir. "Content protection" sisteminizi test etmek için yukarıdaki listede belirtilen araçları kullanın.  

## <a name="streaming-protocols-and-encryption-types"></a>Akış protokolleri ve şifreleme türleri

Media Services PlayReady, Widevine ve FairPlay kullanarak AES şifresiz anahtarını veya DRM şifreleme ile dinamik olarak şifrelenmiş içeriğinizi teslim etmek için kullanabilirsiniz. Şu anda, HTTP canlı akışı (HLS), MPEG DASH ve kesintisiz akış biçimlerinde şifreleyebilirsiniz. Her protokolü, aşağıdaki şifreleme yöntemlerini destekler:

### <a name="hls"></a>HLS

HLS protokolü, şifreleme düzenleri ve aşağıdaki kapsayıcı biçimlerini destekler.

|Kapsayıcı biçimi|Şifreleme şeması|URL örneği|
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbc)`|
|TS MPG2 |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cbcs-aapl)`|
|CMAF(fmp4) |CBCS (FairPlay) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`|
|TS MPG2 |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-aapl,encryption=cenc)`|
|CMAF(fmp4) |CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=m3u8-cmaf,encryption=cenc)`|

HLS/CMAF + FairPlay (HEVC dahil olmak üzere / H.265) aşağıdaki cihazlarda desteklenir:

* iOS v11 veya üzeri 
* iPhone 8 veya üzeri
* MacOS high Sierra Intel 7 nesil CPU

### <a name="mpeg-dash"></a>MPEG-DASH

MPEG-DASH protokolü, şifreleme düzenleri ve aşağıdaki kapsayıcı biçimlerini destekler.

|Kapsayıcı biçimi|Şifreleme şeması|URL örnekleri
|---|---|---|
|Tümü|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cbc)`|
|CSF(fmp4) |CENC (Widevine + PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-csf,encryption=cenc)`|
|CMAF(fmp4)|CENC (Widevine + PlayReady)|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(format=mpd-time-cmaf,encryption=cenc)`|

### <a name="smooth-streaming"></a>Kesintisiz Akış

Kesintisiz Akış Protokolü, şifreleme düzenleri ve aşağıdaki kapsayıcı biçimlerini destekler.

|Protocol|Kapsayıcı biçimi|Şifreleme şeması|
|---|---|---|
|fMP4|AES|`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cbc)`|
|fMP4 | CENC (PlayReady) |`https://amsv3account-usw22.streaming.media.azure.net/00000000-0000-0000-0000-000000000000/ignite.ism/manifest(encryption=cenc)`|

### <a name="browsers"></a>Tarayıcılar

Ortak tarayıcıları aşağıdaki DRM istemcileri destekler:

|Browser|Şifreleme|
|---|---|
|Chrome|Widevine|
|Edge, IE 11|PlayReady|
|Firefox|Widevine|
|Opera|Widevine|
|Safari|FairPlay|

## <a name="aes-128-clear-key-vs-drm"></a>AES-128 şifresiz anahtarını vs. DRM

Müşteriler genellikle bunlar AES şifrelemesi veya DRM sistem kullanması gerekip gerekmediğini merak ediyor. İki sistem arasındaki başlıca fark, böylece anahtar Aktarımdaki ancak hiçbir ek şifreleme ("clear") olmadan şifreli AES şifreleme ile içerik anahtarı istemciye TLS üzerinden iletilen olduğunu ' dir. Sonuç olarak, içeriğin şifresini çözmek için kullanılan anahtar istemci Player'da erişilebilir olduğundan ve ağ izleme düz metin içinde bulunan istemciye görüntülenebilir. AES-128 şifresiz anahtar şifrelemesi Görüntüleyicisi (çalışanlar tarafından görüntülenmek üzere şirket içinde dağıtılmış gibi şifreleme şirket videolarınızı) güvenilen taraf olduğu kullanım örnekleri için uygundur.

DRM sistemleri, PlayReady, Widevine ve FairPlay tüm bir ek düzeyi şifreleme için AES-128 şifresiz anahtar karşılaştırıldığında içeriğin şifresini çözmek için kullanılan anahtarı sağlamak istiyor. İçerik anahtarı DRM çalışma zamanı tarafından korunan bir anahtarı için şifrelenmiş TLS tarafından sağlanan aktarım düzeyinde şifreleme için ek. Ayrıca, şifre çözme, kötü niyetli bir kullanıcı saldırı daha zor olduğu işletim sistemi düzeyinde güvenli bir ortamda ele alınır. DRM burada Görüntüleyicisi güvenilen taraf olmayabilir ve yüksek düzeyde güvenlik gerektiren kullanım durumları için önerilir.

## <a name="dynamic-encryption-and-key-delivery-service"></a>Dinamik şifreleme ve anahtar dağıtımı hizmetiyle

Media Services v3 sürümünde bir içerik anahtarı akış Bulucu ile ilişkilendirilir (bkz [Bu örnek](protect-with-aes128.md)). Media Services anahtar teslim hizmeti, Azure Media Services, içerik anahtarı sizin için oluşturmasına izin verebilirsiniz. İçerik anahtarı kendiniz, kendi anahtar dağıtımı hizmetiyle kullanıyorsanız veya, iki veri merkezlerinde aynı içerik anahtarı olması gereken bir yüksek kullanılabilirlik senaryonun işlenmesi gerekiyorsa oluşturmanız gerekir.

Bir akışa bir oynatıcı tarafından istendiğinde Media Services dinamik olarak içeriğinizi AES şifresiz anahtar veya DRM şifreleme kullanarak şifrelemek için belirtilen anahtar kullanır. Akış şifresini çözmek için Media Services anahtar dağıtımı hizmetiyle veya belirtilen anahtar dağıtımı hizmetiyle anahtar player ister. Kullanıcı anahtarı almak için yetkili olup olmadığına karar vermek için anahtar için belirtilen içerik anahtarı ilkesi hizmet tarafından değerlendirilir.

Media Services DRM (PlayReady, Widevine, FairPlay) lisansları ve AES anahtarları yetkili istemcilere sunmak için bir anahtar dağıtımı hizmetiyle sağlar. Lisanslar ve anahtarları için yetkilendirme ve kimlik doğrulama ilkelerini yapılandırmak için REST API veya bir Media Services istemci Kitaplığı'nı kullanabilirsiniz.

### <a name="custom-key-and-license-acquisition-url"></a>Özel anahtar ve lisans edinme URL'si

Bir farklı anahtar ve lisans teslimat hizmeti (değil, Media Services) belirtmek istiyorsanız aşağıdaki şablonları kullanın. Varlık başına akış bir ilke oluşturmak yerine çoğu varlık arasında akış ilkenizi paylaşabileceği şablonları iki değiştirilebilir alanları vardır. 

* EnvelopeEncryption.CustomKeyAcquisitionUrlTemplate - son kullanıcı oyuncular anahtarları teslim özel hizmet URL'si için şablonu'nu tıklatın. Azure Media Services'ı, anahtar verme kullanırken gerekli değildir. Şablon hizmet isteği özgü değer ile çalışma zamanında güncelleştirecektir değiştirilebilir belirteçleri destekler.  Şu anda desteklenen belirteç {AlternativeMediaId} değerler, değiştirilir StreamingLocatorId.AlternativeMediaId ve {ContentKeyId} değeriyle, istenen anahtarının tanımlayıcısının değeri ile değiştirilir.
* StreamingPolicyPlayReadyConfiguration.CustomLicenseAcquisitionUrlTemplate - URL oyuncular son kullanıcı lisansları teslim etmek üzere özel hizmet Şablonu'nu tıklatın. Azure Media Services'ı, lisans verme kullanırken gerekli değildir. Şablon hizmet isteği özgü değer ile çalışma zamanında güncelleştirecektir değiştirilebilir belirteçleri destekler. Şu anda desteklenen belirteç {AlternativeMediaId} değerler, değiştirilir StreamingLocatorId.AlternativeMediaId ve {ContentKeyId} değeriyle, istenen anahtarının tanımlayıcısının değeri ile değiştirilir. 
* StreamingPolicyWidevineConfiguration.CustomLicenseAcquisitionUrlTemplate - yukarıdakiyle aynı, yalnızca Widevine için. 
* StreamingPolicyFairPlayConfiguration.CustomLicenseAcquisitionUrlTemplate - yukarıdakiyle aynı, yalnızca FairPlay için.  

Örneğin:

```csharp
streamingPolicy.EnvelopEncryption.customKeyAcquisitionUrlTemplate = "https://mykeyserver.hostname.com/envelopekey/{AlternativeMediaId}/{ContentKeyId}";
```

`ContentKeyId` İstenen anahtar değerine sahiptir ve `AlternativeMediaId` istek bir varlık, tarafında eşlemek isterseniz kullanılabilir. Örneğin, `AlternativeMediaId` izinlerini Ara yardımcı olmak için kullanılabilir.

Özel kullanan diğer örnekler anahtar ve lisans edinme URL'ler için bkz [ilkeleri - akış oluşturma](https://docs.microsoft.com/rest/api/media/streamingpolicies/create)

## <a name="control-content-access"></a>İçerik erişimi denetleme

İçeriğinizi içerik anahtarı ilkesi yapılandırarak kimlerin erişebileceğini kontrol edebilirsiniz. Media Services, anahtar isteğinde bulunan kullanıcıları yetkilendirmenin birden çok yöntemini destekler. İçerik anahtarı İlkesi yapılandırmanız gerekir. Anahtarın istemciye teslim edilebilmesi için istemci (oynatıcı) ilkeyi karşılaması gerekir. İçerik anahtarı ilkeniz olabilir **açın** veya **belirteci** kısıtlama. 

Bir belirteç kısıtlamalı içerik anahtar ilkesiyle, içerik anahtarı, anahtar/lisans istekte geçerli JSON Web Token (JWT) veya basit web belirteci (SWT) sunan bir istemciye gönderilir. Bu belirteci bir güvenlik belirteci hizmeti (STS) tarafından verilmiş olması gerekir. Azure Active Directory STS kullanın veya özel STS dağıtın. STS belirteci kısıtlama yapılandırmasında belirtilen belirtilen anahtarı ve sorunu talepleri ile imzalanmış bir belirteç oluşturmak için yapılandırılmalıdır. Media Services anahtar dağıtımı hizmetiyle belirteç geçerliyse ve belirteçteki talepler için bir anahtar/lisans yapılandırılanlar eşleşen istemci için istenen anahtar/lisans döndürür.

Belirteç kısıtlamalı ilkenin yapılandırdığınızda, birincil doğrulama anahtarı, veren ve İzleyici parametrelerini belirtmeniz gerekir. Birincil doğrulama anahtarı belirteç birlikte imzalandığı anahtarını içerir. Verici belirteci veren güvenli belirteç hizmetidir. Belirtecin amacı kapsam olarak da adlandırılan, hedef kitle açıklayan veya kaynak belirteci erişimini yetkilendirir. Media Services anahtar dağıtımı hizmetiyle belirtecindeki bu değerleri şablon değerleri eşleştiğini doğrular.

Müşteriler özel talepler belirteçte farklı ContentKeyPolicyOptions farklı DRM lisans parametrelerle (kiralama lisans karşı bir abonelik Lisansı) arasında seçim veya içerik anahtarını temsil eden bir talep içerecek şekilde dahil etmek için özel STS genellikle kullanın. erişim belirteci verir anahtar tanımlayıcısı.
 
## <a name="storage-side-encryption"></a>Depolama tarafında şifreleme

Bekleyen veri varlıklarınızı korumanın varlıklar tarafından depolama tarafı şifrelemesi şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifrelemesi Media Services v3 sürümünde nasıl çalıştığı gösterilmektedir:

|Şifreleme seçeneği|Açıklama|Media Services v3|
|---|---|---|
|Media Services'ı depolama şifrelemesi| Media Services tarafından yönetilen AES-256 şifreleme anahtarı|Desteklenmeyen<sup>(1)</sup>|
|[Bekleyen veriler için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifrelemesi, Azure Depolama tarafından sunulan anahtarı Azure tarafından veya müşteri tarafından yönetilen|Desteklenen|
|[Depolama istemci tarafı şifreleme](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar Kasası'nda müşteri tarafından yönetilen bir kiracı anahtarı tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|

<sup>1</sup> , Media Services v3 (AES-256 şifreleme) depolama şifrelemesi, yalnızca varlıklarınızı Media Services v2 ile oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifreli ancak yenilerini oluşturulmasına izin vermez.

## <a name="troubleshoot"></a>Sorun giderme

Alırsanız `MPE_ENC_ENCRYPTION_NOT_SET_IN_DELIVERY_POLICY` hata, akış uygun ilkeyi belirttiğinizden emin olun.

İle biten hatalarıyla karşılaşırsanız `_NOT_SPECIFIED_IN_URL`, URL'de şifreleme biçimini belirttiğinizden emin olun. Örneğin: `…/manifest(format=m3u8-cmaf,encryption=cbcs-aapl)`. Bkz: [akış protokolleri ve şifreleme türlerini](#streaming-protocols-and-encryption-types).

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, görüşlerinizi, güncelleştirmeleri alın

Kullanıma [Azure Media Services topluluğu](media-services-community.md) soru sorun, görüşlerinizi ve medya hizmetleri hakkında güncelleştirmeler almak farklı yollarını görmek için makaleyi.

## <a name="next-steps"></a>Sonraki adımlar

* [AES şifrelemesi ile koruma](protect-with-aes128.md)
* [DRM ile koruma](protect-with-drm.md)
* [Erişim denetimi ile birden çok DRM içerik koruma sistemi tasarlayın](design-multi-drm-system-with-access-control.md)
* [Sık sorulan sorular](frequently-asked-questions.md)

