---
title: Çevrimdışı akış Widevine korumalı içeriğini - Azure hesabınızı yapılandırın
description: Bu konuda, içerik Widevine korumalı çevrimdışı akış için Azure Media Services hesabının nasıl yapılandırılacağı gösterilmektedir.
services: media-services
keywords: DASH, DRM, Widevine çevrimdışı modda ExoPlayer, Android
documentationcenter: ''
author: willzhan
manager: steveng
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2019
ms.author: willzhan
ms.openlocfilehash: 5102720242edd3ffc0a377bbddf0f7f3ade68b63
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64937215"
---
# <a name="offline-widevine-streaming-for-android"></a>Android için akış çevrimdışı Widevine

Çevrimiçi akış içeriği korumaya ek olarak, abonelik ve internet'e bağlı olmadıkları sırada çalışır kiralama Hizmetleri teklif indirilebilir içeriği medya içerik. Telefonunuzu üzerine içerik indirmek ihtiyacınız olabilecek veya tablet çıkış, uçak modu, kayıttan yürütme için ağdan bağlantısı kesilebilir. İçerik indirmek isteyebileceğiniz ek senaryolar:

- Bazı içerik sağlayıcıları DRM lisans teslimat ötesinde bir ülke/bölgenin kenarlık izin verme. Bir kullanıcının kuruluşunun seyahat ederken, içeriği izlemek isterse, çevrimdışı yükleme gereklidir.
- Bazı ülkeler/bölgeler içinde Internet kullanılabilirliği ve/veya bant genişliği sınırlıdır. Kullanıcılar, tatmin edici bir görüntüleme deneyimi için yeterince yüksek çözünürlükte izleyebilirler üzere içerik indirmesi tercih edebilirsiniz.

Bu makalede, Android cihazlar tarafından Widevine korumalı DASH içerik için çevrimdışı modda kayıttan yürütme uygulamak anlatılmaktadır. Çevrimdışı DRM sağlar, abonelik, kiralama ve satın alma sağlamak içeriğinizi kolayca onlarla internet'ten kesildiğinde içerik almak, müşterilerin bu hizmetleri için model.

Android oynatıcı uygulamaları oluşturmak için şu üç seçenek özetlemektedir:

> [!div class="checklist"]
> * ExoPlayer SDK Java API kullanarak bir Yürütücüsü oluşturun
> * Xamarin bağlamasının ExoPlayer SDK'sını kullanarak bir Yürütücüsü oluşturun
> * Chrome mobil tarayıcı v62 veya sonraki bir sürümde şifreli medya uzantısı'nı (EME) ve medya kaynak uzantısı'nı (MSE) kullanarak bir Yürütücüsü oluşturun

Makaleyi de çevrimdışı Widevine korumalı içeriğini ilgili bazı sık sorulan soruları yanıtlar.

## <a name="prerequisites"></a>Önkoşullar 

Android cihazlar için Widevine DRM çevrimdışı uygulamadan önce gerekir:

- Windows'un Widevine DRM kullanarak çevrimiçi içerik koruma için tanıtılan kavramları öğrenin. Bu, aşağıdaki belgeleri/samples ayrıntılı ele alınmıştır:
    - [Erişim denetimi ile çoklu DRM'ye sahip içerik koruma sistemi tasarlama](design-multi-drm-system-with-access-control.md)
    - [DRM dinamik şifreleme ve lisans teslimat hizmeti kullanın](protect-with-drm.md)
- Kopya https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials.git.

    Kodda değiştirmeniz gerekecektir [şifrele .NET kullanarak DRM ile](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/tree/master/AMSV3Tutorials/EncryptWithDRM) Widevine yapılandırmaları eklemek için.  
- Bir açık kaynak video oynatıcı SDK çevrimdışı Widevine DRM kayıttan yürütme destekleme kapasitesine sahip Android için Google ExoPlayer SDK'sı ile sahibi. 
    - [ExoPlayer SDK'sı](https://github.com/google/ExoPlayer)
    - [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html)
    - [EoPlayer Geliştirici blogu](https://medium.com/google-exoplayer)

## <a name="configure-content-protection-in-azure-media-services"></a>Azure Media Services'da içerik korumayı yapılandırma

İçinde [GetOrCreateContentKeyPolicyAsync](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L189) yöntemi, aşağıdaki gerekli adımları mevcut:

1. Belirtin nasıl içerik anahtar teslim lisans teslimat hizmeti yetkili: 

    ```csharp
    ContentKeyPolicySymmetricTokenKey primaryKey = new ContentKeyPolicySymmetricTokenKey(tokenSigningKey);
    List<ContentKeyPolicyTokenClaim> requiredClaims = new List<ContentKeyPolicyTokenClaim>()
    {
        ContentKeyPolicyTokenClaim.ContentKeyIdentifierClaim
    };
    List<ContentKeyPolicyRestrictionTokenKey> alternateKeys = null;
    ContentKeyPolicyTokenRestriction restriction 
        = new ContentKeyPolicyTokenRestriction(Issuer, Audience, primaryKey, ContentKeyPolicyRestrictionTokenType.Jwt, alternateKeys, requiredClaims);
    ```
2. Widevine lisans şablonu yapılandırın:  

    ```csharp
    ContentKeyPolicyWidevineConfiguration widevineConfig = ConfigureWidevineLicenseTempate();
    ```

3. ContentKeyPolicyOptions oluşturun:

    ```csharp
    options.Add(
        new ContentKeyPolicyOption()
        {
            Configuration = widevineConfig,
            Restriction = restriction
        });
    ```

## <a name="enable-offline-mode"></a>Çevrimdışı modunu etkinleştir

Etkinleştirmek için **çevrimdışı** modu Widevine lisansı, yapılandırmanız gereken [Widevine lisans şablonu](widevine-license-template-overview.md). İçinde **policy_overrides** nesne, ayarlama **can_persist** özelliğini **true** (varsayılan değer: false), gösterildiği gibi [ConfigureWidevineLicenseTempate](https://github.com/Azure-Samples/media-services-v3-dotnet-tutorials/blob/master/AMSV3Tutorials/EncryptWithDRM/Program.cs#L563). 

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#ConfigureWidevineLicenseTempate)]

## <a name="configuring-the-android-player-for-offline-playback"></a>Android oynatıcı çevrimdışı kayıttan yürütme için yapılandırma

Android cihazlar için bir yerel player uygulaması geliştirmek için en kolay yolu kullanmaktır [Google ExoPlayer SDK'sı](https://github.com/google/ExoPlayer), bir açık kaynak video oynatıcı SDK. Şu anda Android yerel MediaPlayer MPEG-DASH ve Microsoft kesintisiz akış teslim protokollerine dahil olmak üzere API tarafından desteklenen özellikler ExoPlayer destekler.

Sürüm 2.6 ve daha yüksek ExoPlayer çevrimdışı Widevine DRM kayıttan yürütmeyi destekleyen çok sayıda sınıfları içerir. Özellikle, OfflineLicenseHelper sınıf yükleme, yenileme ve çevrimdışı lisans serbest DefaultDrmSessionManager kullanımını kolaylaştırmak için yardımcı işlevleri sağlar. SDK klasöründe sağlanan sınıfları "kitaplık/çekirdek/src/main/java/com/google/android/exoplayer2/çevrimdışı /" Çevrimdışı video içerik yükleme desteği.

Aşağıdaki listede yer alan sınıfları ExoPlayer SDK'sı Android için çevrimdışı modda kolaylaştırır:

- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/OfflineLicenseHelper.Java  
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DefaultDrmSessionManager.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/DrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ErrorStateDrmSession.Java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/DRM/ExoMediaDrm.Java
- library/core/src/main/java/com/google/android/exoplayer2/offline/SegmentDownloader.java
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/Offline/DownloaderConstructorHelper.Java 
- library/Core/src/Main/Java/com/Google/Android/exoplayer2/Offline/Downloader.Java
- library/dash/src/main/java/com/google/android/exoplayer2/source/dash/offline/DashDownloader.java 

Geliştiriciler başvuruda bulunmalıdır [ExoPlayer Geliştirici Kılavuzu](https://google.github.io/ExoPlayer/guide.html) ve karşılık gelen [Geliştirici Blog](https://medium.com/google-exoplayer) bir uygulamanın geliştirilmesi sırasında. Google Geliştirici Kılavuzu ve blog sınırlı bilgiler, bu nedenle şu anda çevrimdışı Widevine destekleyen ExoPlayer uygulama tümüyle belgelenmiş bir başvuru uygulaması veya örnek kodunu yayımlamadı. 

### <a name="working-with-older-android-devices"></a>Eski Android cihazları ile çalışma

Bazı eski Android cihazları için aşağıdaki değerleri ayarlamalısınız **policy_overrides** özellikleri (tanımlanan [Widevine lisans şablonu](widevine-license-template-overview.md): **rental_duration_seconds**, **playback_duration_seconds**, ve **license_duration_seconds**. Alternatif olarak, bunları sonsuz/sınırsız süresi anlamına gelen sıfır olarak ayarlayabilirsiniz.  

Değer bir tamsayı taşma hatası kaçınmak için ayarlamanız gerekir. Sorun hakkında daha fazla açıklama için bkz: https://github.com/google/ExoPlayer/issues/3150 ve https://github.com/google/ExoPlayer/issues/3112. <br/>Değerleri açıkça ayarlamazsanız için çok büyük değerleri **PlaybackDurationRemaining** ve **LicenseDurationRemaining** (en fazla örnek için 9223372036854775807, atanır pozitif değeri bir 64-bit tamsayı). Sonuç olarak, Widevine Lisans süresi dolmuş görüntülenir ve bu nedenle şifre çözme değil olacağını. 

Android 5.0 ARMv8 tam olarak desteklemek için tasarlanan birinci Android sürümü olduğundan bu sorunu Android 5.0 Lollipop veya sonraki sürümlerde gerçekleşmez ([Gelişmiş RISC makinesi](https://en.wikipedia.org/wiki/ARM_architecture)) ve 64-bit platformlarda Android 4.4 KitKat ederken Başlangıçta ARMv7 ve diğer eski Android sürümleri 32-bit platformlarda olarak destekleyecek şekilde tasarlanmıştır.

## <a name="using-xamarin-to-build-an-android-playback-app"></a>Xamarin Android kayıttan yürütme uygulama oluşturmak için kullanma

Xamarin bağlamalarını ExoPlayer için aşağıdaki bağlantıları kullanarak bulabilirsiniz:

- [Google ExoPlayer kitaplığı için Xamarin bağlamaları kitaplığı](https://github.com/martijn00/ExoPlayerXamarin)
- [ExoPlayer NuGet için Xamarin bağlamaları](https://www.nuget.org/packages/Xam.Plugins.Android.ExoPlayer/)

Ayrıca, aşağıdaki iş parçacığı bakın: [Xamarin bağlama](https://github.com/martijn00/ExoPlayerXamarin/pull/57). 

## <a name="chrome-player-apps-for-android"></a>Android için Chrome oynatıcı uygulamaları

Sürümünden itibaren [v Android için Chrome'un. 62](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates), EME kalıcı lisans desteklenir. [Widevine L1](https://developers.google.com/web/updates/2017/09/chrome-62-media-updates#widevine_l1) artık Chrome'da Android için de desteklenir. Bu sayede, son kullanıcılarınız bu varsa Chrome'da çevrimdışı oynatma uygulamaları oluşturmak (veya üzeri) Chrome sürümü. 

Ayrıca, Google, aşamalı Web App (PWA) örnek üretilen sahiptir ve açık, kaynak: 

- [Kaynak kod](https://github.com/GoogleChromeLabs/sample-media-pwa)
- [Google barındırılan sürüm](https://biograf-155113.appspot.com/ttt/episode-2/) (Chrome v 62 ve daha yüksek Android cihazlarda, yalnızca çalışır)

Mobil Chrome tarayıcınızı v62 (veya üzeri) yükseltirseniz, bir Android telefon ve test Yukarıdaki örnek uygulaması barındırılan, iş, akış çevrimiçi ve çevrimdışı kayıttan yürütme görürsünüz.

Yukarıdaki açık kaynaklı PWA uygulama node.js'de yazılmıştır. Bir Ubuntu sunucusu kendi sürümünde barındırmak istiyorsanız, kayıttan yürütme engelleyebilir aşağıdaki yaygın karşılaşılan sorunları göz önünde bulundurun:

1. CORS sorun: Örnek uygulamada video örnek barındırılan https://storage.googleapis.com/biograf-video-files/videos/. Google, Google bulut depolama kovada barındırılan kendi test örnekleri için CORS ayarladı. CORS giriş açıkça belirtilmesi CORS üst bilgileri ile hizmet verilen: https://biograf-155113.appspot.com (etki alanında hangi google barındırıyorsa bunların örnek) diğer siteler erişimini engelliyor. Denerseniz, şu HTTP hata görürsünüz: Yüklenemedi https://storage.googleapis.com/biograf-video-files/videos/poly-sizzle-2015/mp4/dash.mpd: 'Access-Control-Allow-Origin' üst bilgi, istenen kaynak üzerinde yok. Kaynak ' https:\//13.85.80.81:8080' erişim verilmez. Donuk bir yanıt ihtiyaçlarınıza hizmet veriyorsa, isteğin kaynak devre dışı CORS ile getirmek için 'Hayır-cors' moduna ayarlayın.
2. Sertifika verme: Chrome v 58 ' başlayarak, HTTPS için Widevine EME gerektirir. Bu nedenle, x X509 ile HTTPS üzerinden örnek uygulamasını barındırmak gereken sertifika. Her zamanki test sertifikanın aşağıdaki gereksinimleri nedeniyle çalışmaz: Aşağıdaki minimum gereksinimleri karşılayan bir sertifika gerekir:
    - Chrome ve Firefox SAN konu alternatif adı ayarı sertifikada bulunmasını gerektirir.
    - Güvenilen CA sertifika gerekir ve otomatik olarak imzalanan geliştirme sertifikası çalışmıyor
    - Sertifika bir CN web sunucusu veya ağ geçidi DNS adını eşleşen sahip olmalıdır

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="question"></a>Soru

Nasıl miyim kalıcı lisanslarını (etkin çevrimdışı bazı istemciler/kullanıcıları için) ve diğerleri için kalıcı olmayan lisanslarını (devre dışı çevrimdışı) gerçekleştirebiliyor musunuz? Yinelenen içerik ve ayrı bir içerik anahtarı var mı?

### <a name="answer"></a>Yanıt
Media Services v3 birden çok StreamingLocators sahip bir varlık verdiğinden. Sahip olduğunuz

1.  Bir ContentKeyPolicy license_type ile "kalıcı", "kalıcı" Şirket talep ile ContentKeyPolicyRestriction ve kendi StreamingLocator; =
2.  Başka bir ContentKeyPolicy license_type ile = "kalıcı", "kalıcı" Şirket talep ile ContentKeyPolicyRestriction ve kendi StreamingLocator.
3.  İki StreamingLocators farklı ContentKey vardır.

Özel STS iş mantığı bağlı olarak farklı talepler JWT belirteç verilir. Belirteci ile yalnızca karşılık gelen lisans alınabilir ve yalnızca karşılık gelen URL çalınabilir.

### <a name="question"></a>Soru

Google'nın içinde Widevine güvenlik düzeyleri için [Widevine DRM mimarisine genel bakış doc](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf) belgeleri, üç farklı güvenlik düzeyleri tanımlar. Bununla birlikte, [Widevine lisans şablonu hakkında Azure Media Services belgeleri](widevine-license-template-overview.md), beş farklı güvenlik düzeylerine ana hatlarıyla belirtilen. İlişki veya iki farklı güvenlik düzeyleri kümesi arasındaki eşlemeyi nedir?

### <a name="answer"></a>Yanıt

Google'nın içinde [Widevine DRM mimarisine genel bakış](https://storage.googleapis.com/wvdocs/Widevine_DRM_Architecture_Overview.pdf), aşağıdaki üç güvenlik düzeyleri tanımlar:

1.  Güvenlik düzeyi 1: Güvenilir Yürütme Ortamı'içinde (TEE), tüm içerik işleme, şifreleme ve denetimi gerçekleştirilir. Bazı uygulama modellerinde güvenlik işleme farklı yongalarda gerçekleştirilebilir.
2.  Güvenlik düzeyi 2: TEE içinde şifreleme (ancak değil video işleme) gerçekleştirir: şifresi arabellek uygulama etki alanına döndürdü ve ayrı video donanım veya yazılım işlenir. Düzey 2, ancak şifreleme bilgileri yine de yalnızca TEE içinde işlenir.
3.  Güvenlik düzeyi 3 bir TEE cihazda yok. Şifreleme bilgileri ve konak işletim sisteminde şifresi içeriği korumak için uygun önlemleri izlenebilir. 3\. düzey uygulama ayrıca donanım şifreleme altyapısı içerebilir, ancak performans, güvenlik değil, yalnızca geliştirir.

Aynı anda, buna [Widevine lisans şablonu hakkında Azure Media Services belgeleri](widevine-license-template-overview.md), content_key_specs security_level özelliğini aşağıdaki beş farklı değerlere (kayıttan yürütme için istemci sağlamlık gereksinimleri) sahip olabilir:

1.  Yazılım tabanlı whitebox crypto gereklidir.
2.  Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.
3.  Anahtar malzemesi ve şifreleme işlemleri içinde gerçekleştirilmelidir donanım TEE desteklenir.
4.  Şifreleme ve kod çözme içerik içinde gerçekleştirilmelidir donanım TEE desteklenir.
5.  Şifreleme, kod çözme ve tüm işleme (sıkıştırılmış ve sıkıştırılmamış) ortamının içinde işlenmeli donanım TEE desteklenir.

Her iki güvenlik düzeyleri Google Widevine tarafından tanımlanır. İçindeki kullanım düzeyine fark vardır: Mimari düzeyi veya API düzeyi. Beş güvenlik düzeyleri Widevine API'SİNDE kullanılır. Security_level content_key_specs nesnesini seri durumdan ve Widevine küresel teslim hizmet Azure Media Services Widevine lisans hizmeti tarafından geçirilir. Aşağıdaki tabloda, iki güvenlik düzeyleri arasındaki eşlemeyi gösterir.

| **Widevine mimaride tanımlanan güvenlik düzeyleri** |**Widevine API içinde kullanılan güvenlik düzeyleri**|
|---|---| 
| **Güvenlik düzeyi 1**: Güvenilir Yürütme Ortamı'içinde (TEE), tüm içerik işleme, şifreleme ve denetimi gerçekleştirilir. Bazı uygulama modellerinde güvenlik işleme farklı yongalarda gerçekleştirilebilir.|**security_level = 5**: Şifreleme, kod çözme ve tüm işleme (sıkıştırılmış ve sıkıştırılmamış) ortamının içinde işlenmeli donanım TEE desteklenir.<br/><br/>**security_level = 4**: Şifreleme ve kod çözme içerik içinde gerçekleştirilmelidir donanım TEE desteklenir.|
**Güvenlik düzeyi 2**: TEE içinde şifreleme (ancak değil video işleme) gerçekleştirir: şifresi arabellek uygulama etki alanına döndürdü ve ayrı video donanım veya yazılım işlenir. Düzey 2, ancak şifreleme bilgileri yine de yalnızca TEE içinde işlenir.| **security_level 3 =** : Anahtar malzemesi ve şifreleme işlemleri içinde gerçekleştirilmelidir donanım TEE desteklenir. |
| **Güvenlik düzeyi 3**: Bir TEE cihazda yok. Şifreleme bilgileri ve konak işletim sisteminde şifresi içeriği korumak için uygun önlemleri izlenebilir. 3\. düzey uygulama ayrıca donanım şifreleme altyapısı içerebilir, ancak performans, güvenlik değil, yalnızca geliştirir. | **security_level = 2**: Yazılım şifreleme ve karıştırılmış bir kod çözücü gereklidir.<br/><br/>**security_level = 1**: Yazılım tabanlı whitebox crypto gereklidir.|

### <a name="question"></a>Soru

Neden içerik indirme kadar uzun sürer?

### <a name="answer"></a>Yanıt

Yükleme hızını artırmak için iki yolu vardır:

1.  CDN etkinleştirin; böylelikle son kullanıcılar CDN origin/akış uç noktası için içerik indirme yerine isabet olasılığı daha yüksektir. Kullanıcı akış uç noktasını olursa, her HLS segment veya tire parça dinamik olarak paketlenir ve şifrelenir. Bir saatten uzun video varsa bu gecikme süresi için her segment/parçası, milisaniye ölçek olsa da, birikmiş gecikme süresi uzun indirme neden büyük olabilir.
2.  Son kullanıcılar video kalitesi katmanları ve tüm içeriğini yerine ses parçalarını seçerek yükleme seçeneği sağlar. Çevrimdışı mod için tüm kalite katmanları indirmek için noktası yok. Bunu yapmanın iki yolu vardır:
    1.  Denetlenen istemci: player uygulaması otomatik olarak seçer veya kullanıcının seçtiği video kalitesi katman ve indirmek için; ses parçaları
    2.  Hizmet kontrol: bir özelliği dinamik bildirim Azure Media Services'da HLS çalma listesi veya tire MPD tek video kalitesi katmana sınırlar ve ses parçaları seçildi (Genel) bir filtre oluşturmak için kullanabilirsiniz. Ardından bu filtre son kullanıcılara sunulan indirme URL'sini içerir.

## <a name="summary"></a>Özet

Bu makalede ele alınan nasıl uygulanacağını Android cihazlar tarafından Widevine korumalı DASH içerik için çevrimdışı modda kayıttan yürütme.  Çevrimdışı Widevine korumalı içeriğini ilgili bazı sık sorulan soruları yanıt verdi.
