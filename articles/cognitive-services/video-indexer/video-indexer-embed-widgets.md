---
title: Uygulamalarınıza Azure Video dizin oluşturucu pencere öğeleri ekleme | Microsoft Docs
description: ''
services: cognitive services
documentationcenter: ''
author: juliako
manager: erikre
ms.service: cognitive-services
ms.topic: article
ms.date: 04/04/2018
ms.author: juliako
ms.openlocfilehash: 8da06253c58a2de474e879f18013fa1e57f5668d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35355637"
---
# <a name="embed-video-indexer-widgets-into-your-applications"></a>Uygulamalarınıza video dizin oluşturucu pencere öğeleri ekleme

Video dizin oluşturucu uygulamanıza pencere öğeleri katıştırma iki türlerini destekler: **Bilişsel Insights** ve **Player**. 

* A **Bilişsel Insights** pencere öğesi dizin oluşturma işlemi videonuzu ayıklanmış tüm visual ınsights içerir. 
    Öngörüler pencere öğesi aşağıdaki isteğe bağlı URL parametreleri destekler:

    |Ad|Tanım|Açıklama|
    |---|---|---|
    |pencere nesneleri|Virgülle ayrılmış dizeler|İşlemek istediğiniz Öngörüler denetlemenize olanak verir. <br/>Örnek: **pencere öğeleri kişiler, markalar =** yalnızca Kişiler ve markalar UI Öngörüler oluşturmaz<br/>Mevcut seçenekler: kişi, anahtar sözcükler, ek açıklamalar, markalar, düşüncelerin, dökümü, Ara | 
* A **Player** pencere öğesi Uyarlamalı bit hızı kullanarak video akışı olanak sağlar.

    Player pencere öğesi aşağıdaki isteğe bağlı URL parametreleri destekler:

    |Ad|Tanım|Açıklama|
    |---|---|---|
    |T|Başlangıçtan saniye|Noktası belirtilen süreden çalma player başlangıç yapar.<br/>Örnek: t = 60|
    |Resim yazıları|Dil kodu|Pencere öğesi resim yazıları menüde kullanılabilir olması için yükleme sırasında verilen dildeki resim yazısını getirir.<br/>Örnek: başlıkları = en-Us|
    |showCaptions|Bir Boole değeri|Zaten etkin resim yazıları ile yük oynatıcı sağlar.<br/>Örnek: showCaptions = true|
    |type||(Video bölümü kaldırılır) bir ses yürütücüsü kaplama etkinleştirir.<br/>Örnek: yazın ses =|
    |Otomatik Kullan|Bir Boole değeri|Player yüklendiğinde video oynatma başlatılması gerekip gerekmediğini karar verin (varsayılan değer true).<br/>Örnek: Otomatik Yürüt'ü = false|
    |Dil|Dil kodu|Denetim player denetimleri yerelleştirme (varsayılan olarak en-US)<br/>Örnek: dil de-DE =|

## <a name="embedding-public-content"></a>Ortak içeriğe katıştırma

1. Oturum açın, [Video dizin oluşturucu](https://api-portal.videoindexer.ai/) hesabı. 
2. Video görünüyor "ekleme" düğmesini tıklatın.

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget01.png)

    Embed kalıcı düğmesine tıkladıktan sonra görünen hangi pencere öğesi seçebileceğiniz ekranda uygulamanızda için embed istiyor.
    Bir pencere öğesi seçme (**Player** veya **Bilişsel Insights**), katıştırılmış kodu, uygulamanızda yapıştırmak oluşturur.
 
3. İstediğiniz pencere öğesi türünü seçin (**Bilişsel Insights** veya **Player**).
4. Ekleme kodunu kopyalayın ve uygulamanıza ekleyin. 

    ![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget02.png)

## <a name="embedding-private-content"></a>Özel içerik katıştırma

Alma katıştırmak kodlarından katıştırmak açılan pencereler (önceki bölümde gösterildiği gibi) için **ortak** yalnızca videolar. 

Eklemek istiyorsanız, bir **özel** bir erişim belirteci geçirmek zorunda video **IFRAME**'s **src** özniteliği:

     https://www.videoindexer.ai/embed/[insights | player]/<accountId>/<VideoId>/?accessToken=<accessToken>
    
Kullanın [ **alma Öngörüler pencere öğesi** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) Bilişsel Öngörüler pencere öğesi içeriği almak için API veya [ **Video erişim belirteci almak** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) ve aynı ekleyin bir param URL'ye yukarıda gösterildiği gibi sorgu. Bu URL olarak belirtmek **IFRAME**'s **src** değeri.

Katıştırılmış pencere öğesinde (web uygulamamız sahibiz gibi) düzenleme Öngörüler yetenekleri sağlamak istiyorsanız, bir erişim belirteci düzenleme izinlerine sahip geçmesi gerekir. Kullanım [ **alma Öngörüler pencere öğesi** ](https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-insights-widget?) veya [ **Video erişim belirteci almak** ](https://api-portal.videoindexer.ai/docs/services/authorization/operations/Get-Video-Access-Token?) ile **& allowEdit = true**. 

## <a name="widgets-interaction"></a>Pencere öğeleri etkileşimi

**Bilişsel Insights** pencere öğesi, uygulamanızın üzerinde bir video ile etkileşim kurabilir. Bu bölümde, bu etkileşimi elde gösterilmektedir.

![Pencere öğesi](./media/video-indexer-embed-widgets/video-indexer-widget03.png)

### <a name="cross-origin-communications"></a>Çıkış noktaları arası iletişim

Diğer bileşenlerle iletişim kurmak için Video dizin oluşturucu pencere öğeleri almak için Video dizin oluşturucu hizmeti şunları yapar:

- Çıkış noktaları arası iletişimi HTML5 yöntemi kullanan **postMessage** ve 
- İleti VideoIndexer.ai kaynak arasında doğrular. 

İle tümleştirme, kendi player kodunu uygulamak ve seçerseniz **Bilişsel Insights** widgets, olmasından VideoIndexer.ai gelen iletinin kaynağını doğrulamayı sizin sorumluluğunuzdadır.

### <a name="embed-both-types-of-widgets-in-your-application--blog-recommended"></a>Her iki tür pencere öğeleri, uygulamanızda katıştırmak / blog (önerilir) 

Bir kullanıcı, uygulamanızın Insight denetimi tıklattığında bu bölüm iki Video dizin oluşturucu pencere öğeleri arasındaki etkileşim elde etmek bunu gösterilmektedir, ilgili şu player atlar.

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script> 

1. Kopya **Player** pencere öğesi ekleme kodu.
2. Kopya **Bilişsel Insights** ekleme kodu edinin.
3. Ekleme [ **Dünyası dosyası** ](https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js) iki pencere öğeleri arasındaki iletişimi işlemek için:

    <script src="https://breakdown.blob.core.windows.net/public/vb.widgets.mediator.js"></script>

Şimdi bir kullanıcı, uygulamanızın Insight denetimi tıklattığında player ilgili şu atlar.

Daha fazla bilgi için bkz: [bu demo](https://api-portal.videoindexer.ai/demo-all-widgets).

### <a name="embed-the-cognitive-insights-widget-and-use-azure-media-player-to-play-the-content"></a>Bilişsel Öngörüler pencere öğesi ekleme ve içeriği yürütmek için Azure Media Player kullanın

Bu bölümde arasındaki etkileşim elde etmek gösterilmiştir bir **Bilişsel Insights** pencere öğesi ve bir Azure Media Player örneğini kullanarak [AMP eklentisi](https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js).
 
1. Bir Video Dizin Oluşturucu Eklentisi için AMP player ekleyin.

        <script src="https://breakdown.blob.core.windows.net/public/amp-vb.plugin.js"></script>


2. Azure Media Player ile Video Dizin Oluşturucu Eklentisi örneği oluşturur.

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

3. Kopya **Bilişsel Insights** ekleme kodu edinin.

Artık Azure Media Player ile iletişim kurabilmesi.

Daha fazla bilgi için bkz: [bu demo](https://api-portal.videoindexer.ai/demo-your-amp).

### <a name="embed-video-indexer-cognitive-insights-widget-and-use-your-own-player-could-be-any-player"></a>Video dizin oluşturucu Bilişsel Öngörüler pencere öğesi ekleme ve (herhangi player olabilir) kendi oynatıcı kullanın

Kendi player'ı kullanırsanız, player düzenleme ilgilenebilmek sahip kendiniz iletişimi elde etmek için. 

1. Video oynatıcı ekleyin.

    Örneğin, standart bir HTML5 player

        <video id="vid1" width="640" height="360" controls autoplay preload>
           <source src="//breakdown.blob.core.windows.net/public/Microsoft%20HoloLens-%20RoboRaid.mp4" type="video/mp4" /> 
           Your browser does not support the video tag.
        </video>    

2. Bilişsel Öngörüler pencere öğesi ekleyin.
3. İletişim bilgilerinizi player için "iletisi" olay dinleyerek uygulayın. Örneğin:

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


Daha fazla bilgi için bkz: [bu demo](https://api-portal.videoindexer.ai/demo-your-player).

## <a name="adding-subtitles"></a>Altyazıları ekleme

Kendi AMP player ile Video dizin oluşturucu Öngörüler ekleme, kullanabileceğiniz **GetVttUrl** kapalı açıklamalı alt yazıları (altyazıları) almak için yöntemi. Video dizin oluşturucu AMP eklentisi ayrıca bir javascript yöntemini çağırabilir **getSubtitlesUrl** (daha önce gösterildiği gibi). 

## <a name="customizing-embeddable-widgets"></a>Gömülebilir pencere öğeleri özelleştirme

### <a name="cognitive-insights-widget"></a>Bilişsel Öngörüler pencere öğesi
İstediğiniz bir değer olarak belirterek Öngörüler türlerini seçebilirsiniz ekleme kodu eklenen şu URL parametresi için (API veya web uygulamasından) alırsınız:

**& pencere öğeleri =** \<istenen pencere öğeleri listesi >

Olası değerler şunlardır: kişi, anahtar sözcükler, düşüncelerin, dökümü, Ara.

Eklemek istiyorsanız, örneğin, yalnızca Kişiler ve arama Öngörüler IFRAME katıştırmak URL içeren bir pencere öğesi şuna benzeyecektir: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?widgets=people, arama

IFRAME penceresinin başlık sağlayarak özelleştirilebilir **& Başlık =** <YourTitle> IFRAME URL. (Html özelleştireceğiniz \<title > değeri).
IFRAME pencerenizi "MyInsights" başlık vermek istiyorsanız, örneğin, url şöyle görünür: https://www.videoindexer.ai/embed/insights/c4c1ad4c9a/?title=MyInsights. Öngörüler yeni bir pencerede açmak gerektiğinde bu seçeneği yalnızca durumlarda ilgili olduğuna dikkat edin.

### <a name="player-widget"></a>Player pencere öğesi
Video dizin oluşturucu player katıştırılmış içerirse IFRAME boyutunu belirterek player boyutu seçebilirsiniz.

Örneğin:

    <iframe width="640" height="360" src="https://www.videoindexer.ai/embed/player/{id}” frameborder="0" allowfullscreen />

Varsayılan görüntü dizin oluşturucu oynatıcısının video görüntü karşıya yükleyen seçilmedi kaynak diliyle ayıklandı video dökümü göre kapalı açıklamalı alt yazıları otomatik oluşturulan.

Farklı bir dil olan eklemek istiyorsanız, ekleyebileceğiniz **& Resim yazıları = < dil | "tümü" | "false" >** embed player URL veya put "tümü", tüm diller açıklamalı alt yazıları olduğu istiyorsanız değeri olarak.
Varsayılan olarak görüntülenecek resim yazıları istiyorsanız geçirebilirsiniz **& showCaptions = true**

Embed URL ardından şuna benzeyecektir: https://www.videoindexer.ai/embed/player/9a296c6ec3/?captions=italian. Resim yazıları devre dışı bırakmak istiyorsanız "false" resim yazıları parametresinin değeri olarak geçirebilirsiniz.

Otomatik Yürüt – varsayılan olarak player video oynatılır. değil seçebilirsiniz geçirme & otomatik olarak = false yukarıdaki embed URL.

## <a name="next-steps"></a>Sonraki adımlar

Görüntülemek ve Video dizin oluşturucu Öngörüler düzenleme hakkında daha fazla bilgi için bkz: [bu](video-indexer-view-edit.md) makalesi.

## <a name="see-also"></a>Ayrıca bkz.

[Video dizin oluşturucu genel bakış](video-indexer-overview.md)
