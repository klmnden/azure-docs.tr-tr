---
title: Uygulamalarınıza Azure Video Indexer pencere öğeleri ekleyin | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 07/31/2018
ms.author: juliako
ms.openlocfilehash: ba81030c3d6384ca6b66d6a3b14e614d1626e3e0
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2018
ms.locfileid: "41988776"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Video Indexer pencere öğeleri uygulamalarınıza ekleyin

Video Indexer, uygulamanıza ekleme iki tür pencere öğeleri destekler: **Bilişsel İçgörüler** ve **Player**. 

* A **Bilişsel İçgörüler** pencere öğesi içeren dizin oluşturma işlemi videonuzdan ayıklanan tüm görsel Öngörüler. 
    İçgörüler pencere öğesi, aşağıdaki isteğe bağlı URL parametreleri destekler:

    |Ad|Tanım|Açıklama|
    |---|---|---|
    |pencere öğeleri|Virgülle ayırarak dizeleri|İşlemek istediğiniz öngörüleri denetlemenizi sağlar. <br/>Örnek: **pencere öğeleri kişiler, markalar =** yalnızca Kişiler ve markalar UI ınsights işlenir<br/>Mevcut seçenekler: kişiler, anahtar sözcükler, ek açıklamalar, markalar, yaklaşımları, döküm, Ara | 
* A **Player** pencere öğesi, Uyarlamalı bit hızı kullanarak video akışı olanak tanır.

    Oynatıcı pencere öğesi, aşağıdaki isteğe bağlı URL parametreleri destekler:

    |Ad|Tanım|Açıklama|
    |---|---|---|
    |T|Başlangıçtan saniye|İşaret belirtilen süreye yürütmeyi player başlangıç yapar.<br/>Örnek: t = 60|
    |açıklamalı alt yazılar|Dil kodu|Açıklamalı alt yazılar menüde kullanılabilir olmasını yüklenirken pencere öğesi sırasında belirtilen dilde resim yazısı getirir.<br/>Örnek: kapalı açıklamalı alt yazı = en-Us|
    |showCaptions|Bir Boole değeri|Zaten etkin altyazıları yük player yapar.<br/>Örnek: showCaptions = true|
    |type||Bir ses yürütücüsü kaplama (video bölümü kaldırılır) etkinleştirir.<br/>Örnek: türü ses =|
    |otomatik yürütme|Bir Boole değeri|Oyuncu yüklendiğinde videoyu oynatmaya başlatılması gerekip gerekmediğini karar verin (varsayılan: true).<br/>Örnek: autoplay = false|
    |Dil|Dil kodu|Yerelleştirme denetim oynatıcı denetimleri (varsayılan: en-US)<br/>Örnek: dil de-DE =|

## <a name="embedding-public-content"></a>Genel içeriği ekleme

1. Oturum açın, [Video Indexer](https://api-portal.videoindexer.ai/) hesabı. 
2. Videonun altında görüntülenen "ekleme" düğmesine tıklayın.

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Düğmesine tıklandıktan sonra bir ekleme kalıcı görünen ekranda hangi pencere öğesi seçmek, ekleme için uygulamanızda istediğiniz.
    Bir pencere öğesi seçme (**Player** veya **Bilişsel İçgörüler**), uygulamanızda yapıştırmak gömülü kodu oluşturur.
 
3. İstediğiniz pencere öğesi türünü seçin (**Bilişsel İçgörüler** veya **Player**).
4. Ekleme kodunu kopyalayın ve uygulamanıza ekleyin. 

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Özel içeriği ekleme

Alma katıştırabilir kodlarından ekleme açılan pencereler (önceki bölümde gösterildiği gibi) için **genel** yalnızca videolar. 

Eklemek istediğiniz bir **özel** bir erişim belirteci geçirmek zorunda video, **iframe**'s **src** özniteliği:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Kullanma [ **İçgörüler pencere öğesi alma** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) Bilişsel İçgörüler pencere öğesinin içeriğini almak için API veya [ **Video erişim belirteci Al** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) ve olarak eklemek bir Yukarıda da gösterildiği gibi url parametresi sorgulayın. Bu URL olarak belirtmek **iframe**'s **src** değeri.

İçinde katıştırılmış bir pencere öğenizi (web uygulamamıza uyguluyoruz gibi) düzenleme ınsights yetenekleri sağlamak istiyorsanız, düzenleme izinlerine sahip bir erişim belirteci geçmesi gerekir. Kullanım [ **İçgörüler pencere öğesi alma** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) veya [ **Video erişim belirteci Al** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) ile **& allowEdit = true**. 

## <a name="widgets-interaction"></a>Pencere öğeleri etkileşimi

**Bilişsel İçgörüler** pencere öğesi uygulamanızda video ile etkileşim kurabilir. Bu bölümde, bu etkileşim elde etmek gösterilmektedir.

![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Çıkış noktaları arası iletişim

Video Indexer hizmeti diğer bileşenler ile iletişim kurmak için Video Indexer pencere öğeleri almak için şunları yapar:

- Çıkış noktaları arası iletişimi HTML5 yöntemi kullanan **postMessage** ve 
- İleti VideoIndexer.ai kaynak arasında doğrular. 

Kendi Yürütücü kodu uygulamak ve ile tümleştirmeniz seçin **Bilişsel İçgörüler** birkaç pencere öğesinde olduğu VideoIndexer.ai gelen ileti kaynağı doğrulamak için sizin sorumluluğunuzdadır.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Uygulamanıza pencere öğeleri her iki türdeki / blog (önerilir) 

Bir kullanıcı uygulamanızda Insight denetim tıkladığında bu bölüm iki Video Indexer pencere öğeleri arasındaki etkileşimi elde etmek bunu gösterilmektedir, oyuncu için ilgili şu atlar.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Kopyalama **Player** pencere öğesi ekleme kodu.
2. Kopyalama **Bilişsel İçgörüler** ekleme kodu.
3. Ekleme [ **Dünyası dosyası** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) iki pencere arasındaki iletişimi işlemek için:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Artık bir kullanıcı uygulamanızda Insight denetimi tıklattığında, oyuncu için ilgili şu atlar.

Daha fazla bilgi için [bu tanıtım](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Bilişsel İçgörüler pencere öğesi ekleme ve içeriği yürütmek için Azure Media Player'ı kullanın

Bu bölümde, arasındaki etkileşimi elde etmek gösterilmiştir bir **Bilişsel İçgörüler** pencere öğesi ve kullanarak bir Azure Media Player örneği [AMP eklentisi](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Video Indexer eklentisi için AMP player ekleyin.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Azure Media Player ile Video Indexer eklenti örneği.

        // Init Source
        function initSource() {
            var tracks = [{
            kind: 'captions',
            // Here is how to load vtt from VI, you can replace it with your vtt url.
            src: this.getSubtitlesUrl("c4c1ad4c9a", "English"),
            srclang: 'en',
            label: 'English'
            }];

            myPlayer.src([
            {
                "src": "//amssamples.streaming.mediaservices.windows.net/91492735-c523-432b-ba01-faba6c2206a2/AzureMediaServicesPromo.ism/manifest",
                "type": "application/vnd.ms-sstr+xml"
            }
            ], tracks);
        }

        // Init your AMP instance
        var myPlayer = amp('vid1', { /* Options */
            "nativeControlsForTouch": false,
            autoplay: true,
            controls: true,
            width: "640",
            height: "400",
            poster: "",
            plugins: {
            videobreakedown: {}
            }
        }, function () {
            // Activate the plugin
            this.videobreakdown({
            videoId: "c4c1ad4c9a",
            syncTranscript: true,
            syncLanguage: true
            });

            // Set the source dynamically
            initSource.call(this);
        });

3. Kopyalama **Bilişsel İçgörüler** ekleme kodu.

Şimdi, Azure Media Player ile iletişim kurabilmesi.

Daha fazla bilgi için [bu tanıtım](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Video Indexer Bilişsel İçgörüler pencere öğesi ekleme ve (herhangi bir oynatıcı olabilir) kendi bir oynatıcı kullanın

Kendi player'ı kullanırsanız, oyuncu düzenleme halletmeniz sahip kendiniz iletişimi elde etmek için. 

1. Video oynatıcı ekleyin.

    Örneğin, standart bir HTML5 oynatıcıda

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Bilişsel İçgörüler pencere öğesi ekleyin.
3. İletişim bilgilerinizi oyuncusunun "ileti" olaya dinleyerek uygulayın. Örneğin:

        <script>
    
            (function(){
            // Reference your player instance
            var playerInstance = document.getElementById('vid1');
        
            function jumpTo(evt) {
              var origin = evt.origin || evt.originalEvent.origin;
        
              // Validate that event comes from the videobreakdown domain.
              if ((origin === "https://www.videobreakdown.com") && evt.data.time !== undefined){
                
                // Here you need to call your player "jumpTo" implementation
                playerInstance.currentTime = evt.data.time;
               
                // Confirm arrival to us
                if ('postMessage' in window) {
                  evt.source.postMessage({confirm: true, time: evt.data.time}, origin);
                }
              }
            }
        
            // Listen to message event
            window.addEventListener("message", jumpTo, false);
          
            }())    
        
        </script>


Daha fazla bilgi için [bu tanıtım](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Alt yazı ekleme

Video dizinleyici öngörülerini kendi AMP player ile eklerseniz, kullanabileceğiniz **GetVttUrl** kapalı açıklamalı alt yazılar (alt yazılar) almak için yöntemi. Video Indexer AMP eklentisini bir javascript yöntemini de çağırabilirsiniz **getSubtitlesUrl** (daha önce gösterildiği gibi). 

## <a name="customizing-embeddable-widgets"></a>Gömülebilir pencere öğeleri özelleştirme

### <a name="cognitive-insights-widget"></a>Bilişsel içgörüler pencere öğesi
İstediğiniz bir değer olarak belirterek öngörü türlerini seçebilirsiniz ekleme kodu için eklenen aşağıdaki URL parametresi için (API veya web uygulamasından) sahip olursunuz:

**& pencere öğeleri =** \<istenen pencere öğeleri listesi >

Olası değerler şunlardır: kişiler, anahtar sözcükler, yaklaşımları, döküm, arama.

Eklemek istiyorsanız, örneğin, yalnızca kişi ve arama ınsights iframe embed URL içeren bir pencere öğesi şuna benzeyecektir: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people, arama

İframe pencerenin başlığı sağlayarak da özelleştirilebilir **& Başlık =** <YourTitle> iframe URL'si. (Html özelleştireceğim \<başlığı > değer).
İframe pencereniz başlığı "MyInsights" vermek istiyorsanız, örneğin, url şöyle görünür: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. Insights yeni bir pencerede açmak, ihtiyacınız olduğunda bu seçeneği yalnızca durumlarda ilgili olduğuna dikkat edin.

### <a name="player-widget"></a>Oynatıcı pencere öğesi
Video Indexer player eklerseniz iframe boyutunu belirterek oynatıcı boyutunu seçebilirsiniz.

Örneğin:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

Varsayılan Video Indexer oynatıcısının otomatik kapalı açıklamalı alt yazılar Video'dan video karşıya yüklendi, seçilen kaynak diliyle ayıklanan videonun dökümünü temel oluşturulmuş.

İle farklı bir dil eklemek istiyorsanız ekleyebileceğiniz **& açıklamalı alt yazılar = < dil | "tüm" | "false" >** ekleme player URL'sini veya put "all" tüm diller açıklamalı alt yazılar istiyorsanız değeri.
Varsayılan olarak görüntülenecek resim yazıları istiyorsanız geçirebilirsiniz **& showCaptions = true**

Ekleme URL'sini sonra şuna benzeyecektir: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Açıklamalı alt yazılar devre dışı bırakmak isterseniz, "false" olarak açıklamalı alt yazılar parametresi için değer geçirebilirsiniz.

Otomatik Oynat – varsayılan olarak player videoyu oynatmaya başla. için seçebileceğiniz = false olarak yukarıdaki ekleme URL'sini geçirme & autoplay tarafından.

## <a name="next-steps"></a>Sonraki adımlar

Video dizinleyici öngörülerini görüntüleyip hakkında daha fazla bilgi için bkz. [bu](video-indexer-view-edit.md) makalesi.

Ayrıca, kullanıma [Video Indexer codepen](https://codepen.io/videoindexer/pen/eGxebZ).
