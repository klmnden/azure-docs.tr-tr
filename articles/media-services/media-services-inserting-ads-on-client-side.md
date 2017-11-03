---
title: "İstemci tarafında reklam ekleme | Microsoft Docs"
description: "Bu konuda, istemci tarafında ads Ekle gösterilmektedir."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: 52ba731f88c630830560e3cf8406ba2e9613c8a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="inserting-ads-on-the-client-side"></a>İstemci tarafında reklam ekleme
Bu konu, çeşitli türlerdeki istemci tarafında reklam ekleme hakkında bilgi içerir.

Canlı akış videoları kapalı açıklamalı alt yazı ve ad desteği hakkında daha fazla bilgi için bkz: [desteklenen alanında Kapalı açıklamalı alt yazı ve Ad ekleme standartları](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player Ads şu anda desteklemiyor.
> 
> 

## <a id="insert_ads_into_media"></a>Reklam medyanızı ekleme
Azure Media Services, Windows ortam platformu aracılığıyla ad ekleme için destek sağlar: oynatıcı çerçevelerine. Ad desteğiyle oynatıcı çerçevelerine Windows 8, Silverlight, Windows Phone 8 ve iOS cihazları için kullanılabilir. Her player framework oynatıcı uygulaması uygulamak nasıl oluşturulduğunu gösteren örnek kodunu içerir. Medya: listesine ekleyebilirsiniz ads üç farklı türde vardır.

* **Doğrusal** – tam ana video duraklatma çerçevesi ads.
* **Doğrusal** – ana video yürütme gibi görüntülenen katmana ads genellikle bir logo veya diğer statik görüntü yerleştirilen player içinde.
* **Yardımcı** – dışında player görüntülenen ads.

Reklam ana videonun Zaman Çizelgesi herhangi bir noktada yerleştirilebilir. Ad yürütmek ne zaman ve hangi ads yürütmek için player bildirmeniz gerekir. Bu yapılır bir dizi standart XML tabanlı dosyaları kullanılarak: Video Ad Hizmeti şablonu (VAST), Dijital Video birden çok Ad çalma listesi (VMAP), medya soyut sıralama şablonu (a) ve Dijital Video Oynatıcı Ad arabirim tanımı (VPAID). BÜYÜK dosyaları görüntülemek için hangi ads belirtin. VMAP dosyaları çeşitli reklam oynatın ve büyük XML içeren zamanı belirtin. A dosyaları da büyük XML içerebilen dizisi reklam için başka bir yoludur. VPAID dosyaları video oynatıcı ve ad veya ad sunucusu arasında bir arabirim tanımlar.

Her player framework farklı şekilde çalışır ve her biri kendi konusunda ele alınacaktır. Bu konu, reklam eklemek için kullanılan temel mekanizmaları anlatmaktadır. Video oynatıcı uygulamaları ads bir ad sunucusundan isteyin. Ad sunucusu, çeşitli yollarla yanıt verebilir:

* BÜYÜK bir dosya
* İade VMAP dosyası (ile katıştırılmış VAST)
* Bir a dosyasıyla (katıştırılmış VAST) döndürür
* VPAID ads sahip büyük bir dosya Döndür

### <a name="using-a-video-ad-service-template-vast-file"></a>Bir Video Ad Hizmeti şablonu (VAST) dosyasını kullanma
Hangi ad veya görüntülemek için reklam büyük dosyayı belirtir. Aşağıdaki XML doğrusal ad için büyük bir dosya örneğidir:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="115571748">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://www.myserver.com/tracking-resource]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <TrackingEvents>
                  <Tracking event="start"><![CDATA[http://www.myserver.com/start-tracking-resource]]></Tracking>
                  <Tracking event="midpoint"><![CDATA[http://www.myserver.com/midpoint-tracking-resource]]></Tracking>
                  <Tracking event="complete"><![CDATA http://www.myserver.com/complete-tracking-resource]]></Tracking>
                  <Tracking event="expand"><![CDATA[http://www.myserver.com/expand-tracking-resource]]></Tracking>
                </TrackingEvents>
                <VideoClicks>
                  <ClickThrough id="Atlas Redirect"><![CDATA[http://www.myserver.com/click-resource]]></ClickThrough>
                  <ClickTracking id="Spare"></ClickTracking>
                </VideoClicks>
                <MediaFiles>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_300_4x3.wmv]]>
                  </MediaFile>
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
          <Extensions>
            <Extension type="Atlas">
            </Extension>
          </Extensions>
        </InLine>
      </Ad>
    </VAST>

Doğrusal ad tarafından açıklanan <**doğrusal**> öğesi. Olayları izleme, izleme'ye tıklayın ve bir dizi aracılığıyla,'yi tıklatın, ad süresini belirtir **MediaFile** öğeleri. İzleme olaylarını içinde belirtilen <**TrackingEvents**> öğesi ve ad görüntülerken oluşan çeşitli olayları izlemek bir ad sunucusu izin verin. Başlangıç, Orta, tam, bu durumda ve genişletin olayları izlenir. Ad görüntülendiğinde başlangıç olayı oluşur. Orta olayı en azından ad zaman çizelgesi % 50'si görüntülenen oluşur. Complete olayını ad sonuna çalıştırdığınızda oluşur. Kullanıcı için tam ekran video oynatıcı genişletirken genişletme olayı oluşur. Clickthroughs ile belirtilen bir <**geçişli tıklatma**> öğesinde bir <**VideoClicks**> öğesi ve kullanıcı üzerinde ad tıklattığında görüntülenecek bir kaynak için bir URI belirtir. ClickTracking belirtilen bir <**ClickTracking**> öğesi, ayrıca içinde <**VideoClicks**> öğesi ve kullanıcı üzerinde ad tıklattığında istemek player için bir izleme kaynağı belirtir . <**MediaFile**> öğeleri belirli bir kodlama bir ad ilgili bilgileri belirtin. Olduğunda birden fazla <**MediaFile**> öğesi, video oynatıcı seçebilirsiniz platform için en iyi kodlama. 

Doğrusal ads belirli bir sırada görüntülenebilir. Bunu yapmak için ek ekleyin <Ad> VAST öğelerine dosya ve sıra özniteliğini kullanarak düzenini belirtin. Aşağıdaki örnekte bu gösterilmektedir:

    <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:32</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
      <Ad id="2" sequence="1">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResouce]]></Impression>
          <Creatives>
            <Creative id="video" sequence="0" AdID="">
              <Linear>
                <Duration>00:00:30</Duration>
                <MediaFiles>
                  <!-- ... -->
                </MediaFiles>
              </Linear>
            </Creative>
          </Creatives>
        </InLine>
      </Ad>
    </VAST>

Doğrusal ads belirtilir bir <Creative> de öğesi. Aşağıdaki örnekte gösterildiği bir <Creative> doğrusal ad açıklar öğesi.

    <Creative id="video" sequence="1" AdID="">
      <NonLinearAds>
        <NonLinear width="216" height="121" minSuggestedDuration="00:00:15">
          <StaticResource creativeType="image/png"><![CDATA[http://myserver/images/image.png]]></StaticResource>
          <StaticResource creativeType="image/jpg"><![CDATA[http://myserver/images/image.jpg]]></StaticResource>
        </NonLinear>
        <TrackingEvents>
             <Tracking event="acceptInvitation"><![CDATA[http://myserver/tracking/trackingID]></Tracking>
             <Tracking event="collapse"><![CDATA[http://myserver/tracking/trackingID2]]></Tracking>
         </TrackingEvents>
       </NonLinearAds>
    </Creative>


<**NonLinearAds**> öğesi bir veya daha fazla içerebilir <**NonLinear**> öğeleri, her biri bir doğrusal ad tanımlayabilir. <**NonLinear**> öğesi kaynak için doğrusal ad belirtir. Kaynak olabilir bir <**StaticResouce**> e <**IFrameResource**>, veya bir <**HTMLResouce**>. <**StaticResource**> olarak HTML olmayan kaynak açıklar ve kaynak nasıl görüntülendiğini belirten bir creativeType özniteliği tanımlar:

Görüntü/GIF, görüntü/jpeg resim/png – kaynak bir HTML biçiminde görüntülenir <**img**> etiketi.

Uygulama/x-javascript – kaynak, bir HTML biçiminde görüntülenir <**betik**> etiketi.

Uygulama/x-shockwave-flash – kaynak bir Flash player görüntülenir.

**IFrameResource** IFRAME içerisinde görüntülenen bir HTML kaynak açıklar. **HTMLResource** bir web sayfasına eklenecek HTML kod parçası açıklar. **TrackingEvents** izleme olayları ve olay ortaya çıktığında istemek için URI belirtin. Bu örnekte acceptInvitation ve Daralt olayları izlenir. Daha fazla bilgi için **NonLinearAds** öğeyi ve alt öğelerini IAB.NET/VAST bakın. Unutmayın **TrackingEvents** öğesidir içinde bulunduğu **NonLinearAds** öğesi yerine **NonLinear** öğesi.

Yardımcı ads içinde tanımlanmış bir <CompanionAds> öğesi. <CompanionAds> Öğesi bir veya daha fazla içerebilir <Companion> öğeleri. Her <Companion> öğesi Yardımcısı ad açıklar ve içerebilir bir <StaticResource>, <IFrameResource>, veya <HTMLResource> doğrusal olmayan bir ad olduğu gibi aynı şekilde belirtilir. BÜYÜK bir dosya birden çok yardımcı ads içerebilir ve oynatıcı uygulaması görüntülemek için en uygun ad seçebilirsiniz. VAST hakkında daha fazla bilgi için bkz: [büyük 3.0](http://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Birden çok Ad çalma listesi (VMAP) dosyası bir Dijital Video kullanma
VMAP dosya ad sonları olduğunda, her sonu ne kadar olacağını, kaç tane ads sonu içinde görüntülenebilir ve ne ads türlerini olabilir belirtmenize olanak tanır sonu sırasında görüntülenir. Aşağıdaki örnek VMAP dosyasındaki bir tek ad sonu tanımlayan:

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[http://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scaleable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scaleable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv]]>
                          </MediaFile>
                        </MediaFiles>
                      </Linear>
                    </Creative>
                  </Creatives>
                </InLine>
              </Ad>
            </VAST>
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

VMAP dosya ile başlayan bir <VMAP> içeren bir veya daha fazla öğe <AdBreak> öğeleri, her bir ad sonu tanımlama. Her ad sonu sonu türü, kesme kimliği ve saat konumu belirtir. BreakType özniteliği sırasında sonu çalınabilir ad türünü belirtir: Doğrusal, doğrusal, veya görüntüleme. Reklam harita büyük Yardımcısı reklamları görüntüleyin. Birden fazla ad türü (boşluksuz) virgülle ayrılmış liste belirtilebilir. BreakID ad için isteğe bağlı bir tanımlayıcıdır. TimeOffset ad ne zaman görüntüleneceğini belirtir. Aşağıdaki yollardan biri belirtilebilir:

1. Saat – .mmm milisaniye olduğu ss: dd: veya ss:dd:ss.mmm biçimi. Bu özniteliğin değeri video zaman çizelgesinin en baştan zaman ad sonu başlangıcına belirtir.
2. Yüzde – %n biçiminde burada n video zaman çizelgesi ad yürütmeden önce yürütülecek yüzdesidir
3. Başlangıç/bitiş – önce veya video görüntülendikten sonra bir ad görüntüleneceğini belirtir
4. Konum – ad sonları zamanlama bilinmeyen, canlı akış olduğu gibi böyle olduğunda ad sonları sırasını belirtir. Her ad sonu sırasını n 1 veya daha büyük bir tamsayı olduğu #n biçiminde belirtilir. 1 güveninin ad yürütülen ad yürütülen ikinci fırsatta vb. ilk fırsatta 2 olduğunu belirtir.

İçinde <**AdBreak**> öğesi var. bir olabilir <**AdSource**> öğesi. <**AdSource**> öğesi aşağıdaki öznitelikleri içerir:

1. Kimliği – ad kaynağı için bir tanımlayıcı belirtir
2. allowMultipleAds – birden çok ads sırasında ad sonu görüntülenip belirten bir Boole değeri
3. followRedirects – video oynatıcı dikkate belirten isteğe bağlı bir Boole değeri içinde bir ad yanıtı yeniden yönlendirir.

<**AdSource**> öğesi, bir satır içi ad yanıt veya bir ad yanıt başvurusu oynatıcı sağlar. Aşağıdaki öğelerden birini içerebilir:

* <VASTAdData>BÜYÜK ad yanıt VMAP dosyanın içinde ekli gösterir
* <AdTagURI>başka bir sistemden bir ad yanıt başvuran bir URI
* <CustomAdData>-bir rastgele bu respresents büyük olmayan bir yanıt dize

Bu örnekte, bir satır içi ad yanıt ile belirtilen bir <VASTAdData> büyük ad yanıtı içeren öğe. Diğer öğeler hakkında daha fazla bilgi için bkz: [VMAP](http://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

<**AdBreak**> öğesi de içerebilir bir <**TrackingEvents**> öğesi. <**TrackingEvents**> öğesi başlangıç veya bitiş bir ad sonu veya bir hata ad sonu sırasında oluşup oluşmadığını izlemenize olanak sağlar. <**TrackingEvents**> öğesi içeren bir veya daha fazla <**izleme**> öğeleri, bir izleme olayı ve izleme URI her biri belirtir. Olası izleme olaylarını şunlardır:

1. breakStart – izleyen bir ad sonu başlangıcı
2. breakEnd – bir ad sonu tamamlanmasından izleme
3. hata – ad kesme sırasında oluşan hata izler

Aşağıdaki örnek, izleme olaylarını belirten bir VMAP dosyası gösterir.

    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <!--Inline VAST -->
          </vmap:VASTData>
        </vmap:AdSource>
        <vmap:TrackingEvents>
          <vmap:Tracking event="breakStart">
            http://MyServer.com/breakstart.gif
          </vmap:Tracking>
          <vmap:Tracking event="breakend">
            http://MyServer.com/breakend.gif
          </vmap:Tracking>
          <vmap:Tracking event="error">
            http://MyServer.com/error.gif
          </vmap:Tracking>
        </vmap:TrackingEvents>
      </vmap:AdBreak>
    </vmap:VMAP>

Daha fazla bilgi için <**TrackingEvents**> öğesi ve alt öğelerini http://iab.org/VMAP.pdf bakın

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Şablon (a) dosyası sıralama medya Özet kullanma
A dosya, bir ad görüntülendiğinde tanımlamak Tetikleyicileri belirtmenizi sağlar. Tetikleyiciler öncesi toplama ad, bir orta toplama ad ve sonrası ad içeren bir örnek a dosyanız verilmiştir.

    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
      <triggers>
        <trigger id="preroll" description="preroll every item"  >
          <startConditions>
            <condition type="event" name="OnItemStart" />
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="midroll" description="midroll at 15 sec."  >
          <startConditions>
            <condition type="property" name="Position" value="00:00:15.0" operator="GEQ" />
          </startConditions>
          <endConditions>
            <condition type="event" name="OnItemEnd"/>
            <!--This 'resets' the trigger for the next clip-->
          </endConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>

        <trigger id="postroll" description="postroll"  >
          <startConditions>
            <condition type="event" name="OnItemEnd"/>
          </startConditions>
          <sources>
            <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
              <sources />
            </source>
          </sources>
        </trigger>
      </triggers>
    </MAST>



Bir a dosyası ile başlayan bir **a** içeriyor öğesi **Tetikleyicileri** öğesi. <triggers> Öğesi içeren bir veya daha fazla **tetikleyici** bir ad zaman oynanan tanımlayan öğeleri. 

**Tetikleyici** öğesi içeren bir **startConditions** yürütmek bir ad ne zaman başlaması gerektiğini belirten öğesi. **StartConditions** öğesi içeren bir veya daha fazla <condition> öğeleri. Zaman her <condition> tetikleyici başlatılır veya olup olmamasına iptal true olarak değerlendirilir <condition> kapsamında yer alan bir **startConditions** veya **endConditions** öğesi sırasıyla. Zaman birden çok <condition> öğeleri, örtük veya olarak kabul edilir, true olarak değerlendiriliyor herhangi bir koşul başlatmak tetikleyici neden olur. <condition>öğeleri içe olamaz. Zaman alt <condition> öğeleri önceden bir örtük ve kabul edilir, tüm koşulların başlatmak tetikleyici için true olarak değerlendirmeniz gerekir. <condition> Öğesi koşulu tanımla aşağıdaki öznitelikleri içerir: 

1. **tür** – koşul, olay veya özelliğin türünü belirtir.
2. **ad** – özelliği veya değerlendirme sırasında kullanılacak olay adı
3. **değer** – bir özelliğe göre hesaplanan değer
4. **İşleç** – değerlendirme sırasında kullanılacak işlemi: EQ (eşittir), NEQ (eşit değildir), GTR (büyük), GEQ (büyük veya buna eşit), LT (küçük), LEQ (daha az veya buna eşit) MOD (modül)

**endConditions** de içeren <condition> öğeleri. Bir koşul tetikleyici true değerlendirirken sıfırlanır. <trigger> Öğesi de içerir bir <sources> içeren bir veya daha fazla öğe <source> öğeleri. <source> Ad yanıt ve ad yanıtının türünü tanımlayan URI öğesi. Bu örnekte, büyük bir yanıt olarak bir URI verildi. 

    <trigger id="postroll" description="postroll"  >
      <startConditions>
        <condition/>
      </startConditions>
      <sources>
        <source uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" format="vast">
          <sources />
        </source>
      </sources>
    </trigger>


### <a name="using-video-player-ad-interface-definition-vpaid"></a>Video oynatıcı Ad arabirim tanımı (VPAID) kullanma
VPAID, bir video oynatıcı ile iletişim kurmak yürütülebilir ad birimleri etkinleştirmek için bir API'dir. Bu, yüksek oranda etkileşimli ad deneyimleri sağlar. Kullanıcının ad ile etkileşim kurabilir ve ad Görüntüleyici tarafından gerçekleştirilen eylemler yanıt vermesini sağlayabilirsiniz. Örneğin bir ad ad daha uzun bir sürümü veya daha fazla bilgi görüntülemek kullanıcı izin düğmeleri görüntüleyebilir. Video oynatıcı VPAID API desteklemesi ve yürütülebilir ad API uygulamanız gerekir. Bir ad güvenlik sunucusu AD'den VPAID ad içeren büyük bir yanıt ile yanıt verebilir bir oynatıcı isteğinde bulunduğunda.

Yürütülebilir bir ad, bir çalışma zamanı ortamında Adobe Flash™ veya bir web tarayıcısında yürütülebilecek JavaScript gibi yürütülmelidir kod oluşturulur. Bir ad sunucusu VPAID ad içeren büyük bir yanıtı geri döndüğünde, apiFramework değerini özniteliği <MediaFile> öğesi "VPAID" olması gerekir. Bu öznitelik kapsanan ad VPAID yürütülebilir ad olduğunu belirtir. Type özniteliği "application/x-shockwave-flash" veya "uygulama/x-javascript" gibi yürütülebilir dosya MIME türü için ayarlamanız gerekir. Aşağıdaki XML parçacığını gösterilmektedir <MediaFile> VPAID yürütülebilir ad içeren büyük bir yanıtı öğesinden. 

    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>


Yürütülebilir bir ad kullanılarak başlatılabilir <AdParameters> öğesi içinde <Linear> veya <NonLinear> öğeleri büyük bir yanıt. Daha fazla bilgi için <AdParameters> öğesi, bkz: [büyük 3.0](http://www.iab.net/media/file/VASTv3.0.pdf). VPAID API'si hakkında daha fazla bilgi için bkz: [VPAID 2.0](http://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Bir Windows veya Windows Phone 8 Player Ad desteği ile uygulama
Microsoft Media Platform: Windows 8 için Player Framework ve Windows Phone 8 nasıl framework kullanarak bir video oynatıcı uygulaması uygulandığını göstermektedir örnek uygulamaları koleksiyonunu içerir. Player Framework ve örnekleri indirin [Player Framework için Windows 8 ve Windows Phone 8](https://playerframework.codeplex.com).

Microsoft.PlayerFramework.Xaml.Samples çözümü açtığınızda klasörleri projedeki çeşitli görürsünüz. Reklam klasör ad desteğiyle bir video oynatıcı oluşturmak için ilgili örnek kodunu içerir. İçinde reklam XAML/cs dosyaları sayısı, farklı bir şekilde reklam ekleme Göster klasörüdür. Aşağıdaki listede her açıklanmaktadır:

* AdPodPage.xaml nasıl ad pod görüntüleneceğini gösterir.
* AdSchedulingPage.xaml ads zamanlama gösterilmektedir.
* FreeWheelPage.xaml FreeWheel eklentisi ads zamanlamak için nasıl kullanılacağını gösterir.
* MastPage.xaml a dosyasıyla ads zamanlama gösterilmektedir.
* ProgrammaticAdPage.xaml program aracılığıyla bir video ads zamanlama gösterilmektedir.
* ScheduleClipPage.xaml bir ad büyük dosyası olmadan zamanlama gösterilmektedir.
* VastLinearCompanionPage.xaml bir doğrusal ekleme ve yardımcı ad gösterir.
* VastNonLinearPage.xaml doğrusal olmayan bir ad eklemek nasıl gösterir.
* VmapPage.xaml nasıl ads VMAP dosyasıyla belirtileceği gösterir.

Bu örneklerin her player çerçevesi tarafından tanımlanan MediaPlayer sınıfını kullanır. Çoğu örnekleri çeşitli ad yanıt biçimleri için destek eklemek eklentileri kullanır. ProgrammaticAdPage örnek program aracılığıyla MediaPlayer örneği ile etkileşime girer.

### <a name="adpodpage-sample"></a>AdPodPage örnek
Bu örnek AdSchedulerPlugin bir reklam görüntülemek ne zaman tanımlamak için kullanır. Bu örnekte, 5 saniye sonra çalınacak Orta toplama tanıtım zamanlandı. Ad pod (Reklam sırada görüntülemek için bir grup), bir ad sunucusundan döndürülen büyük dosyasında belirtilir. BÜYÜK dosya URI'si belirtilen <RemoteAdSource> öğesi.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">

        <mmppf:MediaPlayer.Plugins>
            <ads:AdSchedulerPlugin>
                <ads:AdSchedulerPlugin.Advertisements>

                    <ads:MidrollAdvertisement Time="00:00:05">
                        <ads:MidrollAdvertisement.Source>
                            <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_adpod.xml" Type="vast"/>
                        </ads:MidrollAdvertisement.Source>
                    </ads:MidrollAdvertisement>

                </ads:AdSchedulerPlugin.Advertisements>
            </ads:AdSchedulerPlugin>
            <ads:AdHandlerPlugin/>
        </mmppf:MediaPlayer.Plugins>
    </mmppf:MediaPlayer>

AdSchedulerPlugin hakkında daha fazla bilgi için bkz: [Windows 8 ve Windows Phone 8 Player Framework'teki tanıtma](http://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Bu örnek ayrıca AdSchedulerPlugin kullanır. Üç reklam, yayın öncesi ad, bir orta toplama ad ve sonrası ad zamanlar. URI VAST her ad için belirtilen bir <RemoteAdSource> öğesi.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:PrerollAdvertisement>
                                <ads:PrerollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PrerollAdvertisement.Source>
                            </ads:PrerollAdvertisement>

                            <ads:MidrollAdvertisement Time="00:00:15">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                            <ads:PostrollAdvertisement>
                                <ads:PostrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml" Type="vast"/>
                                </ads:PostrollAdvertisement.Source>
                            </ads:PostrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>


### <a name="freewheelpage"></a>FreeWheelPage
Bu örnek planlama bilgilerini ad yanı sıra ad içerik belirten bir SmartXML dosyasına işaret bir URI belirten bir kaynak özniteliği belirten FreeWheelPlugin kullanır.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="mastpage"></a>MastPage
Bu örnek, bir a dosyası kullanmanıza olanak sağlayan MastSchedulerPlugin kullanmaktadır. Kaynak özniteliği a dosyasının konumunu belirtir.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Bu örnek program aracılığıyla MediaPlayer ile etkileşime girer. ProgrammaticAdPage.xaml dosya MediaPlayer başlatır:

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>

Bir ad görüntülenmesi gerekir ve büyük bir dosya için bir URI belirten bir RemoteAdSource yükler ve ardından ad çalan MarkerReached olay işleyicisi ekler ProgrammaticAdPage.xaml.cs dosyası belirtmek için bir TimelineMarker ekler bir AdHandlerPlugin oluşturur.

    public sealed partial class ProgrammaticAdPage : Microsoft.PlayerFramework.Samples.Common.LayoutAwarePage
        {
            AdHandlerPlugin adHandler;

            public ProgrammaticAdPage()
            {
                this.InitializeComponent();
                adHandler = new AdHandlerPlugin();
                player.Plugins.Add(new AdHandlerPlugin());
                player.Markers.Add(new TimelineMarker() { Time = TimeSpan.FromSeconds(5), Type = "myAd" });
                player.MarkerReached += pf_MarkerReached;
            }

            async void pf_MarkerReached(object sender, TimelineMarkerRoutedEventArgs e)
            {
                if (e.Marker.Type == "myAd")
                {
                    var adSource = new RemoteAdSource() { Type = VastAdPayloadHandler.AdType, Uri = new Uri("http://smf.blob.core.windows.net/samples/win8/ads/vast_linear.xml") };
                    //var adSource = new AdSource() { Type = DocumentAdPayloadHandler.AdType, Payload = SampleAdDocument };
                    var progress = new Progress<AdStatus>();
                    try
                    {
                        await player.PlayAd(adSource, progress, CancellationToken.None);
                    }
                    catch { /* ignore */ }
                }
            }

### <a name="scheduleclippage"></a>ScheduleClipPage
Bu örnek AdSchedulerPlugin Orta toplama ad ad içeren bir .wmv dosyasını belirterek zamanlamak için kullanır.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.cloudapp.net/html5/media/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:AdSource Type="clip">
                                        <ads:AdSource.Payload>
                                            <ads:ClipAdPayload MediaSource="http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_700_4x3.wmv" MimeType="video/x-ms-wmv" />
                                        </ads:AdSource.Payload>
                                    </ads:AdSource>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Bu örnek AdSchedulerPlugin Orta toplama doğrusal ad Yardımcısı ad ile zamanlamak için nasıl kullanılacağı gösterilmektedir. <RemoteAdSource> Öğesi büyük dosyasının konumunu belirtir.

    <mmppf:MediaPlayer Grid.Row="1"  x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_companions.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
Bu örnek, bir doğrusal zamanlamak için AdSchedulerPlugin ve doğrusal olmayan bir ad kullanır. BÜYÜK dosya konumu ile belirtilen <RemoteAdSource> öğesi.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:AdSchedulerPlugin>
                        <ads:AdSchedulerPlugin.Advertisements>

                            <ads:MidrollAdvertisement Time="00:00:05">
                                <ads:MidrollAdvertisement.Source>
                                    <ads:RemoteAdSource Uri="http://smf.blob.core.windows.net/samples/win8/ads/vast_linear_nonlinear.xml" Type="vast"/>
                                </ads:MidrollAdvertisement.Source>
                            </ads:MidrollAdvertisement>

                        </ads:AdSchedulerPlugin.Advertisements>
                    </ads:AdSchedulerPlugin>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

### <a name="vmappage"></a>VMAPPage
Bu örnek, bir VMAP dosyası kullanarak reklamları zamanlamak için VmapSchedulerPlugin kullanır. URI VMAP dosya için kaynak özniteliği belirtilen <VmapSchedulerPlugin> öğesi.

    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Video Oynatıcı Ad desteğiyle iOS uygulama
Microsoft Media Platform: İOS için Player Framework nasıl framework kullanarak bir video oynatıcı uygulaması uygulandığını göstermektedir örnek uygulamaları koleksiyonunu içerir. Player Framework ve örnekleri indirin [Azure Media Player Framework](https://github.com/Azure/azure-media-player-framework). Github sayfasında bir bağlantı player framework hakkında daha fazla bilgi içeren bir Wiki ve player örnek giriş vardır: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>VMAP birlikte bir reklam planlama
Aşağıdaki örnek, bir VMAP dosyası kullanarak reklamları zamanlamak gösterilmiştir.

    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
            {
                [self logFrameworkError];
            }
            else
            {
                // Schedule a list of ads using the downloaded VMAP manifest
                if (![framework scheduleVMAPWithManifest:manifest])
                {
                    [self logFrameworkError];
                }          
            }

### <a name="scheduling-ads-with-vast"></a>VAST birlikte bir reklam planlama
Aşağıdaki örnek, geç bağlama büyük ad zamanlama gösterilmektedir.

    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
    // Create an AdInfo object
     AdInfo *vastAdInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URL to VAST file
    vastAdInfo1.clipURL = [NSURL URLWithString:vastAd1];
    // set running time of ad
    vastAdInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    vastAdInfo1.mediaTime.clipBeginMediaTime = 0;
    vastAdInfo1.mediaTime.clipEndMediaTime = 10;
    vastAdInfo1.policy = @"policy for late binding VAST";
    // specify ad type
    vastAdInfo1.type = AdType_Midroll;
    vastAdInfo1.appendTo=-1;
    adIndex = 0;
    // schedule ad
    if (![framework scheduleClip:vastAdInfo1 atTime:adLinearTime forType:PlaylistEntryType_VAST andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

   Aşağıdaki örnek, bir erken bağlama büyük ad zamanlama gösterilmektedir.
Örnek: 4 erken bir bağlama büyük ad //Download VAST dosya, zamanlama (! [ framework.adResolver downloadManifest: & bildirim withURL: [NSURL URLWithString:@"http://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]]) {[kendini logFrameworkError];} else {adLinearTime.startTime = 7; adLinearTime.duration = 0;

        // Create AdInfo instance
        AdInfo *vastAdInfo2 = [[[AdInfo alloc] init] autorelease];
        vastAdInfo2.mediaTime = [[[MediaTime alloc] init] autorelease];
        vastAdInfo2.policy = @"policy for early binding VAST";
        // specify ad type
        vastAdInfo2.type = AdType_Midroll;
        vastAdInfo2.appendTo=-1;
        // schedule ad
        if (![framework scheduleVASTClip:vastAdInfo2 withManifest:manifest atTime:adLinearTime andGetClipId:&adIndex])
        {
            [self logFrameworkError];
        }
    }

Aşağıdaki örnek kaba kesme düzenleme (RCE) kullanarak bir ad eklemek nasıl gösterir

    //Example:1 How to use RCE.
    // specify manifest for ad content
    NSString *secondContent=@"http://wamsblureg001orig-hs.cloudapp.net/6651424c-a9d1-419b-895c-6993f0f48a26/The%20making%20of%20Microsoft%20Surface-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // specify ad length
    mediaTime.currentPlaybackPosition = 0;
    mediaTime.clipBeginMediaTime = 0;
    mediaTime.clipEndMediaTime = 80;
    // append ad content
    if (![framework appendContentClip:[NSURL URLWithString:secondContent] withMediaTime:mediaTime andGetClipId:&clipId])
    {
        [self logFrameworkError];
    }

Aşağıdaki örnek, bir ad pod zamanlama gösterilmektedir.

    //Example:5 Schedule an ad Pod.
    // Set start time for ad
    adLinearTime.startTime = 23;
    adLinearTime.duration = 0;

    // Specify URL to content
    NSString *adpodSt1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    // Create an AdInfo instance
    AdInfo *adpodInfo1 = [[[AdInfo alloc] init] autorelease];
    // set URI to ad content
    adpodInfo1.clipURL = [NSURL URLWithString:adpodSt1];
    // Set ad running time
    adpodInfo1.mediaTime = [[[MediaTime alloc] init] autorelease];
    adpodInfo1.mediaTime.clipBeginMediaTime = 0;
    adpodInfo1.mediaTime.clipEndMediaTime = 17;
    adpodInfo1.policy = @"policy for ad pod 1";
    // Set ad type
    adpodInfo1.type = AdType_Midroll;
    adpodInfo1.appendTo=-1;
    adIndex = 0;
    // Schedule ad
    if (![framework scheduleClip:adpodInfo1 atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Aşağıdaki örnek, Yapışkan olmayan Orta toplama ad zamanlama gösterilmektedir. Herhangi bir aramayı bağımsız olarak Görüntüleyici gerçekleştirir sonra Yapışkan olmayan ad yalnızca oynatılır.

    //Example:6 Schedule a single non sticky mid roll Ad
    // specify URL to content
    NSString *oneTimeAd=@"http://wamsblureg001orig-hs.cloudapp.net/5389c0c5-340f-48d7-90bc-0aab664e5f02/Windows%208_%20You%20and%20Me%20Together-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";

    // create an AdInfo instance
    AdInfo *oneTimeInfo = [[[AdInfo alloc] init] autorelease];
    // set URL of ad
    oneTimeInfo.clipURL = [NSURL URLWithString:oneTimeAd];
    oneTimeInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    oneTimeInfo.mediaTime.clipBeginMediaTime = 0;
    oneTimeInfo.mediaTime.clipEndMediaTime = -1;
    oneTimeInfo.policy = @"policy for one-time ad";
    // set ad start time
    adLinearTime.startTime = 43;
    adLinearTime.duration = 0;
    // set ad type
    oneTimeInfo.type = AdType_Midroll;
    // non sticky ad
    oneTimeInfo.deleteAfterPlayed = YES;
    // schedule ad
    if (![framework scheduleClip:oneTimeInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Aşağıdaki örnek, Yapışkan Orta toplama ad zamanlama gösterilmektedir. Yapışkan ad video zaman çizelgesi belirtilen noktasında her erişildiğinde görüntülenir.

    //Example:7 Schedule a single sticky mid roll Ad
    NSString *stickyAd=@"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *stickyAdInfo = [[[AdInfo alloc] init] autorelease];
    // set URI to ad
    stickyAdInfo.clipURL = [NSURL URLWithString:stickyAd];
    stickyAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    stickyAdInfo.mediaTime.clipBeginMediaTime = 0;
    stickyAdInfo.mediaTime.clipEndMediaTime = 15;
    stickyAdInfo.policy = @"policy for sticky mid-roll ad";
    // set ad start time
    adLinearTime.startTime = 64;
    adLinearTime.duration = 0;
    // set ad type
    stickyAdInfo.type = AdType_Midroll;
    stickyAdInfo.deleteAfterPlayed = NO;
    // schedule ad
    if (![framework scheduleClip:stickyAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }


Aşağıdaki örnek, bir sonrası reklam zamanlama gösterilmektedir.

    //Example:8 Schedule Post Roll Ad
    NSString *postAdURLString=@"http://wamsblureg001orig-hs.cloudapp.net/aa152d7f-3c54-487b-ba07-a58e0e33280b/wp-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    // create AdInfo instance
    AdInfo *postAdInfo = [[[AdInfo alloc] init] autorelease];
    postAdInfo.clipURL = [NSURL URLWithString:postAdURLString];
    postAdInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    postAdInfo.mediaTime.clipBeginMediaTime = 0;
    // set ad length
    postAdInfo.mediaTime.clipEndMediaTime = 45;
    postAdInfo.policy = @"policy for post-roll ad";
    // set ad type
    postAdInfo.type = AdType_Postroll;
    adLinearTime.duration = 0;
    if (![framework scheduleClip:postAdInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Aşağıdaki örnek, bir yayın öncesi ad zamanlama gösterilmektedir.

    //Example:9 Schedule Pre Roll Ad
    NSString *adURLString = @"http://wamsblureg001orig-hs.cloudapp.net/2e4e7d1f-b72a-4994-a406-810c796fc4fc/The%20Surface%20Movement-m3u8-aapl.ism/Manifest(format=m3u8-aapl)";
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 40; //You could play a portion of an Ad. Yeh!
    adInfo.mediaTime.clipEndMediaTime = 59;
    adInfo.policy = @"policy for pre-roll ad";
    adInfo.appendTo = -1;
    adInfo.type = AdType_Preroll;
    adLinearTime.duration = 0;
    // schedule ad
    if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }

Aşağıdaki örnek, bir orta toplama katmana ad zamanlama gösterilmektedir.

    // Example10: Schedule a Mid Roll overlay Ad
    NSString *adURLString = @"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/asset-e47b43fd-05dc-4587-ac87-5916439ad07f/Windows%208_%20Cliffjumpers.mp4?st=2012-11-28T16%3A31%3A57Z&se=2014-11-28T16%3A31%3A57Z&sr=c&si=2a6dbb1e-f906-4187-a3d3-7e517192cbd0&sig=qrXYZBekqlbbYKqwovxzaVZNLv9cgyINgMazSCbdrfU%3D";
    //Create AdInfo instance
    AdInfo *adInfo = [[[AdInfo alloc] init] autorelease];
    adInfo.clipURL = [NSURL URLWithString:adURLString];
    adInfo.mediaTime = [[[MediaTime alloc] init] autorelease];
    adInfo.mediaTime.currentPlaybackPosition = 0;
    adInfo.mediaTime.clipBeginMediaTime = 0;
    // specify ad length
    adInfo.mediaTime.clipEndMediaTime = 20;
    adInfo.policy = @"policy for mid-roll overlay ad";
    adInfo.appendTo = -1;
    // specify ad type
    adInfo.type = AdType_Midroll;
    // specify ad start time & duration
    adLinearTime.startTime = 300;
    adLinearTime.duration = 20;
    // schedule ad            if (![framework scheduleClip:adInfo atTime:adLinearTime forType:PlaylistEntryType_Media andGetClipId:&adIndex])
    {
        [self logFrameworkError];
    }



## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Video oynatıcı uygulamaları geliştirme](media-services-develop-video-players.md)

