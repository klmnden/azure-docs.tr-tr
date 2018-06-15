---
title: Hesabınızı Widevine korumalı içeriği - Azure çevrimdışı akış için yapılandırma
description: Bu konu Azure Media Services hesabınızı korumalı Widevine çevrimdışı akış için yapılandırma içerik gösterilmektedir.
services: media-services
keywords: TİRE, DRM, Widevine çevrimdışı modda, ExoPlayer, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: willzhan, dwgeo
ms.openlocfilehash: 158b58c13aee4d6241900db4a5e2b3fe8a45cc3c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33790506"
---
# <a name="offline-widevine-streaming-for-android"></a>Android için akış çevrimdışı Widevine

Çevrimiçi akışla içerik korumaya ek olarak, abonelik ve internet'e bağlı olmadıkları sırada çalışır kiralama Hizmetleri teklif indirilebilir içerik medya içerik. Telefonunuz üzerine içerik indirmek gerekebilecek veya tablet giden, uçak modu kayıttan yürütme için ağ bağlantısı kesilmiş. İlave Senaryolar, içeriği yüklemek isteyebilirsiniz:

- Bazı içerik sağlayıcıları DRM lisans teslimat bir ülkenin kenarlık ötesinde izin vermeyecek. Bir kullanıcı içeriği yurtdışına seyahat ederken izlemek isterse, çevrimdışı yükleme gereklidir.
- Bazı ülkelerde Internet kullanılabilirliği ve/veya bant genişliği sınırlıdır. Kullanıcılar, tatmin edici görüntüleme deneyimi için yeterince yüksek çözünürlük izleyebilirler üzere içerik indirmesi tercih edebilirsiniz.

Bu makalede, Android cihazlarda Widevine tarafından korunan tire içeriği çevrimdışı modda kayıttan uygulamak nasıl anlatılmaktadır. Çevrimdışı DRM abonelik, kiralama ve satın alma sağlamanıza olanak tanır hizmetlerinizi müşterileri kolayca onlarla internet bağlantısı kesilmiş olduğunda içerik almak etkinleştirme içeriğiniz için modeller.

Android oynatıcı uygulamaları oluşturmak için şu üç seçenekten özetlemektedir:

> [!div class="checklist"]
> * ExoPlayer SDK Java API kullanarak bir oynatıcı derleme
> * ExoPlayer SDK Xamarin bağlama kullanarak bir oynatıcı derleme
> * Chrome mobil tarayıcı v62 veya sonraki sürümlerde şifrelenmiş medya uzantısı (EME) ve medya kaynak uzantısı (MSE) kullanarak bir oynatıcı derleme

Makale ayrıca Widevine korumalı içeriği çevrimdışı akış için ilgili bazı sık sorulan soruları yanıtlar.

## <a name="requirements"></a>Gereksinimler 

Çevrimdışı DRM Widevine için Android aygıtlarda uygulamadan önce ilk gerekir:

- Widevine DRM kullanarak çevrimiçi içerik koruma için kavramları öğrenmeniz. Bu, aşağıdaki belgeleri/samples bölümlerinde ayrıntılı ele alınmıştır:
    - [DRM lisansları veya AES anahtarları göndermek için Azure Media Services'i kullanma](media-services-deliver-keys-and-licenses.md)
    - [Çoklu DRM ve Access Control ile CENC: Azure ve Azure Media Services Tasarım ve Uygulama Başvurusu](media-services-cenc-with-multidrm-access-control.md)
    - [PlayReady ve/veya Widevine dinamik ortak şifreleme .NET ile kullanma](https://azure.microsoft.com/resources/samples/media-services-dotnet-dynamic-encryption-with-drm/)
    - [.NET ile PlayReady ve/veya Widevine lisansları teslim etmek için Azure Media Services'i kullanma](https://azure.microsoft.com/resources/samples/media-services-dotnet-deliver-playready-widevine-licenses/)
- Bir açık kaynak video oynatıcı SDK çevrimdışı Widevine DRM kayıttan yürütme destekleme kapasitesine sahip Android için Google ExoPlayer SDK'sı ile Windows'un öğrenin. 
    - [ExoPlayer SDK'sı](https://github.com/google/ExoPlayer)
    - [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html)
    - [EoPlayer Geliştirici blogu](https://medium.com/google-exoplayer)

## <a name="content-protection-configuration-in-azure-media-services"></a>Azure Media Services içerik koruma yapılandırması

Media Services ile bir varlık Widevine korumasını yapılandırdığınızda, aşağıdaki üç şey belirtilen ContentKeyAuthorizationPolicyOption oluşturmanız gerekir:

1. DRM sistemini (Widevine)
2. Lisans teslimat hizmetinin nasıl içerik anahtar teslim belirtme ContentKeyAuthorizationPolicyRestriction yetkili (açık veya belirteç yetkilendirme)
3. (Widevine) DRM lisans şablonu

Etkinleştirmek için **çevrimdışı** modu Widevine lisansları için yapılandırmanız gereken [Widevine lisans şablonu](media-services-widevine-license-template-overview.md). İçinde **policy_overrides** nesne, ayarlamak **can_persist** özelliğine **true** (varsayılan değer false). 

.NET kullanan etkinleştirmek için aşağıdaki kod örneği **çevrimdışı** Widevine lisans modu. Kod dayanır [ kullanarak PlayReady ve/veya Widevine dinamik ortak şifreleme .NET ile](https://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm) örnek. 

```
private static string ConfigureWidevineLicenseTemplateOffline(Uri keyDeliveryUrl)
{
    var template = new WidevineMessage
    {
        allowed_track_types = AllowedTrackTypes.SD_HD,
        content_key_specs = new[]
        {
            new ContentKeySpecs
            {
                required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                security_level = 1,
                track_type = "SD"
            }
        },
        policy_overrides = new
        {
            can_play = true,
            can_persist = true,
            //can_renew = true,                             //if you set can_renew = false, you do not need renewal_server_url
            //renewal_server_url = keyDeliveryUrl.ToString(),   //not mandatory, renewal_server_url is needed only if license_duration_seconds is set
            can_renew = false,
            //rental_duration_seconds = 1209600,
            //playback_duration_seconds = 1209600,
            //license_duration_seconds = 1209600
        }
        };

    string configuration = Newtonsoft.Json.JsonConvert.SerializeObject(template);
    return configuration;
}
```

## <a name="configuring-the-android-player-for-offline-playback"></a>Android player çevrimdışı kayıttan yürütme için yapılandırma

Android cihazlar için bir yerel player uygulama geliştirmek için en kolay yolu kullanmaktır [Google ExoPlayer SDK](https://github.com/google/ExoPlayer), bir açık kaynak video oynatıcı SDK. ExoPlayer şu anda Android'ın yerel MediaPlayer MPEG-DASH ve Microsoft kesintisiz akış teslimi protokolleri de dahil olmak üzere API tarafından desteklenmeyen özellikleri destekler.

ExoPlayer sürüm 2.6 ve daha yüksek çevrimdışı Widevine DRM kayıttan yürütmeyi destekleyen çok sayıda sınıflar içerir. Özellikle, OfflineLicenseHelper sınıfı indirme, yenileme ve çevrimdışı lisans serbest DefaultDrmSessionManager kullanımını kolaylaştırmak için yardımcı işlevleri sağlar. SDK klasöründe sağlanan sınıfları "kitaplığı/core/src/main/java/com/google/android/exoplayer2/çevrimdışı /" Çevrimdışı video içerik indirme destekler.

Aşağıdaki listesi sınıfları, Android için ExoPlayer SDK'sı çevrimdışı modda kolaylaştırmak:

- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/OfflineLicenseHelper.Java  
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSessionManager.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ErrorStateDrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ExoMediaDrm.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/Offline/SegmentDownloader.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/Offline/DownloaderConstructorHelper.Java 
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/Offline/Downloader.Java
- library/dash/src/Main/Java/com/Google/Android/exoplayer2/Source/dash/Offline/DashDownloader.Java 

Geliştiriciler başvuruda bulunmalıdır [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html) ve karşılık gelen [Geliştirici Blog](https://medium.com/google-exoplayer) bir uygulamanın geliştirilmesi sırasında. Google bilgileri Geliştirici Kılavuzu ve blog sınırlı olacak şekilde şu anda çevrimdışı Widevine destekleyen ExoPlayer uygulama için bir tam olarak belgelenmiş başvuru uygulaması veya örnek kodu yayımlamadı. 

### <a name="working-with-older-android-devices"></a>Eski Android aygıtlar ile çalışma

Bazı eski Android cihazlar için aşağıdaki değerleri ayarlamalısınız **policy_overrides** özellikleri (tanımlanan [Widevine lisans şablonu](media-services-widevine-license-template-overview.md): **rental_duration_seconds**, **playback_duration_seconds**, ve **license_duration_seconds**. Alternatif olarak, bunları sonsuz/sınırsız süre anlamına gelir sıfır olarak ayarlayabilirsiniz.  

Değerleri tamsayı taşma hatasını önlemek için ayarlamanız gerekir. Sorun hakkında daha fazla açıklama için bkz: https://github.com/google/ExoPlayer/issues/3150 ve https://github.com/google/ExoPlayer/issues/3112. <br/>Değerleri açıkça ayarlanmazsa için çok büyük değerler **PlaybackDurationRemaining** ve **LicenseDurationRemaining** (en fazla örnek için 9223372036854775807, atanır pozitif değeri bir 64-bit tamsayı). Sonuç olarak, Widevine Lisans süresi dolmuş görünür ve bu nedenle şifre çözme değil gerçekleşir. 

Android 5.0 ARMv8 tam olarak desteklemesi için tasarlanmış ilk Android sürümü olduğundan bu sorun Android 5.0 Lolipop veya sonraki sürümlerde oluşmaz ([Gelişmiş RISC makinesi](https://en.wikipedia.org/wiki/ARM_architecture)) ve Android 4.4 KitKat ederken 64-bit platformları ilk olarak ARMv7 ve 32-bit platformlarda diğer önceki Android sürümlerle desteklemek üzere tasarlanmıştır.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Xamarin Android kayıttan yürütme uygulama oluşturmak için kullanma

Aşağıdaki bağlantıları kullanarak ExoPlayer için Xamarin bağlamaları bulabilirsiniz:

- [Google ExoPlayer kitaplığı için Xamarin bağlamaları kitaplığı](https://github.com/martijn00/ExoPlayerXamarin)
- [ExoPlayer NuGet için Xamarin bağlamaları](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Ayrıca, aşağıdaki iş parçacığı bkz: [Xamarin bağlama](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Chrome oynatıcı uygulamaları Android için

Sürümünden itibaren [Chrome Android v. 62](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates), EME kalıcı lisans desteklenir. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) şimdi Chrome'Android için de desteklenir. Bu, bu, son kullanıcılarınız varsa, Chrome'Çevrimdışı kayıttan yürütme uygulamaları oluşturmanızı sağlar (veya sonrası) Chrome sürümü. 

Ayrıca, Google, aşamalı Web App (PWA) örnek üretilen sahiptir ve bunu Aç-kaynaklanan: 

- [Kaynak kodu](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Barındırılan Google sürüm](https://biograf-155113.appspot.com/ttt/episode-2/) (yalnızca Chrome v 62 ve Android cihazlarda yüksek çalışır)

Mobil Chrome tarayıcınızı v62 (veya sonrası) yükseltirseniz, bir Android telefonunuzu ve test Yukarıdaki örnek uygulaması barındırılan, iş bu akış çevrimiçi ve çevrimdışı kayıttan yürütme görürsünüz.

Yukarıdaki açık kaynak PWA uygulama Node.js içinde yazılmıştır. Ubuntu server üzerindeki kendi sürüm barındırmak istiyorsanız, kayıttan yürütme engelleyebilirsiniz aşağıdaki sık karşılaşılan sorunları göz önünde bulundurun:

1. CORS sorunu: örnek uygulamasında video örnek içinde barındırılan https://storage.googleapis.com/biograf-video-files/videos/. Google, Google bulut depolama demet barındırılan tüm kendi test örnekleri için CORS ayarladı. CORS giriş açıkça belirtilmesi CORS üstbilgilerini ile sunulan: https://biograf-155113.appspot.com (etki alanında hangi google barındıran kendi örnek) diğer siteler tarafından erişimi engelleme. Denerseniz, aşağıdaki HTTP hata iletisiyle karşılaşırsınız: yüklenemedi https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: 'Access-Control-Allow-Origin' üst bilgi istenen kaynak üzerinde yok. Kaynak 'https://13.85.80.81:8080' Bu nedenle erişime izin verilmiyor. Donuk bir yanıt ihtiyaçlarınıza hizmet veriyorsa, isteğin modu devre dışı CORS'yi kaynağı getirmek için 'no-cors' olarak ayarlanmış.
2. Sertifika sorunu: 58 Chrome v ile başlayarak, Widevine EME HTTPS gerektirir. Bu nedenle, bir X509 ile HTTPS üzerinden örnek uygulamayı barındırmak gereken sertifika. Her zamanki test sertifikanın aşağıdaki gereksinimleri nedeniyle çalışmıyor: aşağıdaki minimum gereksinimleri karşılayan bir sertifika edinmeniz gerekir:
    - Chrome ve Firefox SAN konu alternatif adı ayarı sertifikada bulunmasını gerektirir
    - Güvenilen CA sertifika gerekir ve otomatik olarak imzalanan geliştirme sertifikası çalışmıyor
    - Sertifika web sunucusunun veya ağ geçidi DNS adı ile eşleşen bir CN sahip olmalıdır

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="question"></a>Soru

Nasıl ı kalıcı lisansları (çevrimdışı bazı istemciler/kullanıcılar için etkin) ve diğerleri için kalıcı olmayan lisans (devre dışı çevrimdışı) sunabilir? İçerik yinelenen ve ayrı içerik anahtarı kullanmak var mı?

### <a name="answer"></a>Yanıt
İçerik yinelenen gerekmez. Yalnızca içeriği ve tek bir ContentKeyAuthorizationPolicy, ancak iki ayrı ContentKeyAuthorizationPolicyOption'ın tek bir kopyasını kullanabilirsiniz:

1. IContentKeyAuthorizationPolicyOption 1: kalıcı lisans ve license_type gibi bir talep içeren ContentKeyAuthorizationPolicyRestriction 1 kullanır "Kalıcı" =
2. IContentKeyAuthorizationPolicyOption 2: kalıcı olmayan lisans ve license_type gibi bir talep içeren ContentKeyAuthorizationPolicyRestriction 2 kullanır "Nonpersistent" =

Bu şekilde, bir lisans isteği istemci uygulamasından geldiğinde lisans isteğinden fark yoktur. Ancak, farklı son kullanıcı/cihaz için farklı JWT belirteçleri farklı talep (Yukarıdaki iki license_type's biri) içeren iş mantığını STS olması gerekir. JWT belirteci talep değeri lisans hizmeti tarafından ne tür bir lisans vermek için karar vermek için kullanılan: kalıcı veya kalıcı olmayan.

Bu, güvenli belirteç hizmeti (STS) belirtece karşılık gelen talep değeri eklemek için iş mantığı ve istemci/aygıt bilgileri sahip olması anlamına gelir.

### <a name="question"></a>Soru

Google Widevine güvenlik düzeylerinin [Widevine DRM mimarisine genel bakış belge](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) belgeleri, üç farklı güvenlik düzeylerine tanımlar. Bununla birlikte, [Widevine lisans şablonu Azure Media Services belgelerine](https://docs.microsoft.com/azure/media-services/media-services-widevine-license-template-overview), beş farklı güvenlik düzeylerine ana hatlarıyla. İlişki veya güvenlik düzeyleri iki farklı kümesi arasında eşleme nedir?

### <a name="answer"></a>Yanıt

Google içinde [Widevine DRM mimarisine genel bakış](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf), aşağıdaki üç güvenlik düzeyleri tanımlar:

1.  Güvenlik düzeyi 1: Tüm içerik işleme, şifreleme ve denetim güvenilir Yürütme Ortamı'içinde (t) gerçekleştirilir. Bazı uygulama modellerinde güvenlik işleme farklı yongalarında gerçekleştirilebilir.
2.  Güvenlik düzeyi 2: şifreleme (ancak değil video işleme) içinde t gerçekleştirir: şifresi çözülmüş arabellek uygulama etki alanına döndürülen ve ayrı video donanım veya yazılım işlendi. Düzey 2 olduğunda, ancak şifreleme bilgileri hala yalnızca t içinde işlenir.
3.  Güvenlik düzeyi 3 bir t cihazda yok. Şifreleme bilgileri ve ana bilgisayar işletim sistemi şifresi çözülmüş içeriği korumak için gerekli önlemleri alınması. Düzey 3 uygulama aynı zamanda bir donanım şifreleme altyapısı içerebilir ancak performans, güvenlik değil, yalnızca geliştirir.

Aynı anda, buna [Widevine lisans şablonu Azure Media Services belgelerine](https://docs.microsoft.com/azure/media-services/media-services-widevine-license-template-overview), content_key_specs security_level özelliğinin aşağıdaki beş farklı değer (kayıttan yürütme için istemci sağlamlık gereksinimleri) sahip olabilir:

1.  Yazılım tabanlı whitebox crypto gereklidir.
2.  Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.
3.  Anahtar malzemesi ve şifreleme işlemleri içinde gerçekleştirilmesi gereken donanım t yedeklenir.
4.  Şifreleme ve kod çözme içerik içinde gerçekleştirilmesi gereken donanım t yedeklenir.
5.  Şifreleme, kod çözme ve (sıkıştırılmış ve sıkıştırılmamış) ortamının tüm işleme içinde ele alınması gereken bir donanım t yedeklenir.

Her iki güvenlik düzeyleri Google Widevine tarafından tanımlanır. İçindeki kullanım düzeyine fark vardır: mimarisi düzeyi veya API düzeyi. Beş güvenlik düzeyleri Widevine API'SİNDE kullanılır. Security_level içeren content_key_specs nesne seri durumdan ve Azure Media Services Widevine lisans hizmeti tarafından Widevine genel teslimat hizmeti geçirilecek. Aşağıdaki tabloda, bu iki güvenlik düzeyleri kümesi arasında eşleme gösterilmektedir.

| **Widevine mimarisinde tanımlanan güvenlik düzeyleri** |**Widevine API kullanılan güvenlik düzeyleri**|
|---|---| 
| **Güvenlik düzeyi 1**: tüm içerik işleme, şifreleme ve denetim güvenilir Yürütme Ortamı'içinde (t) gerçekleştirilir. Bazı uygulama modellerinde güvenlik işleme farklı yongalarında gerçekleştirilebilir.|**security_level = 5**: şifreleme, kod çözme ve (sıkıştırılmış ve sıkıştırılmamış) ortamının tüm işleme içinde ele alınması gereken donanım t yedeklenir.<br/><br/>**security_level = 4**: şifreleme ve kod çözme içerik içinde gerçekleştirilmelidir donanım t yedeklenir.|
**Güvenlik düzeyi 2**: şifreleme (ancak değil video işleme) içinde t gerçekleştirir: şifresi çözülmüş arabellek uygulama etki alanına döndürülen ve ayrı video donanım veya yazılım işlendi. Düzey 2 olduğunda, ancak şifreleme bilgileri hala yalnızca t içinde işlenir.| **security_level = 3**: anahtar malzemesi ve şifreleme işlemleri içinde gerçekleştirilmesi gereken donanım t yedeklenir. |
| **Güvenlik düzeyi 3**: bir t cihazda yok. Şifreleme bilgileri ve ana bilgisayar işletim sistemi şifresi çözülmüş içeriği korumak için gerekli önlemleri alınması. Düzey 3 uygulama aynı zamanda bir donanım şifreleme altyapısı içerebilir ancak performans, güvenlik değil, yalnızca geliştirir. | **security_level = 2**: yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.<br/><br/>**security_level = 1**: whitebox yazılım tabanlı şifreleme gereklidir.|

### <a name="question"></a>Soru

Neden içerik indirme kadar uzun sürer?

### <a name="answer"></a>Yanıt

İndirme hızını artırmak için iki yolu vardır:

1.  Son kullanıcılar yerine içerik indirmek için kaynak ve akış uç noktası CDN isabet olasılığı; böylece CDN etkinleştirin. Kullanıcı akış uç değerse, her HLS segment veya tire parça dinamik olarak paketlenir ve şifrelenir. Bir saatten uzun video varsa, bu gecikme her segment/parça için milisaniye ölçek olsa bile, birikmiş gecikme uzun indirme neden büyük olabilir.
2.  Son kullanıcılar video kalitesini katmanları ve tüm içeriğini yerine ses izleri seçmeli olarak yükleme seçeneği sağlar. Çevrimdışı modda için tüm kalite katmanları indirmek için noktası yok. Bunu başarmak için iki yolu vardır:
    1.  Denetlenen istemci: oynatıcı uygulaması otomatik seçer veya kullanıcının seçtiği video kalitesini katman ve indirmek için; ses izleri
    2.  Denetlenen hizmet: biri dinamik bildirimi Özelliği Azure Media Services HLS çalma listesi veya tire MPD tek video kalitesini katmana ve ses izleri seçili (Genel) bir filtre oluşturmak için kullanabilirsiniz. Ardından son kullanıcılara sunulan indirme URL'si, bu filtre dahil edilir.

## <a name="summary"></a>Özet

Bu makalede ele alınan çevrimdışı modda kayıttan yürütme Android cihazlarda Widevine tarafından korunan tire içeriği için uygulama.  Widevine korumalı içeriği çevrimdışı akış için ilgili bazı sık sorulan soruları yanıt verdi.
