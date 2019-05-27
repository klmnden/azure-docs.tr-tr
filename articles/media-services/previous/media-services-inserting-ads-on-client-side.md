---
title: İstemci tarafına reklam ekleme | Microsoft Docs
description: Bu konuda, istemci tarafına reklam ekleme işlemini gösterir.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 65c9c747-128e-497e-afe0-3f92d2bf7972
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: 49c836f5e9189104ba77e8f3d865f4db199c4060
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002967"
---
# <a name="inserting-ads-on-the-client-side"></a>İstemci tarafına reklam ekleme
Bu makalede, çeşitli türlerdeki istemci tarafına reklam ekleme hakkında bilgi içerir.

Canlı akış video Kapalı Açıklamalı Altyazı ve ad desteği hakkında daha fazla bilgi için bkz: [desteklenen Kapalı Açıklamalı Altyazı ve Ad ekleme standartları](media-services-live-streaming-with-onprem-encoders.md#cc_and_ads).

> [!NOTE]
> Azure Media Player, reklam şu anda desteklemiyor.
> 
> 

## <a id="insert_ads_into_media"></a>Medyanızı reklam ekleme
Azure Media Services, Windows Media platformu aracılığıyla reklam ekleme için destek sağlar: Oynatıcı çerçeveleri. Ad desteğiyle oynatıcı çerçeveleri, Windows 8, Silverlight, Windows Phone 8 ve iOS cihazlar için kullanılabilir. Her player çerçevesi player uygulamasının nasıl uygulanacağını gösteren örnek kodunu içerir. Reklam medya: listenize eklemek üç farklı tür vardır.

* **Doğrusal** – ana videoyu Duraklat çerçeve reklam dolu.
* **Doğrusal** – ana videoyu oynatmaya olarak görüntülenen katmana reklam genellikle bir logo veya diğer statik resim yerleştirilen player içinde.
* **Yardımcısı** – dışında player görüntülenen reklamlar.

Reklam ana video zaman çizelgesi içinde herhangi bir noktada yerleştirilebilir. Ad yürütmek ne zaman ve hangi reklam yürütmek için player söylemeniz gerekir. Bu işlemi standart XML tabanlı dosyalar kümesi kullanarak: Video Ad hizmet şablonu (VAST), Dijital Video birden çok Ad çalma listesi (VMAP), medya sıralama şablonu (a) ve Dijital Video Oynatıcı Ad arabirim tanımı (VPAID) soyut. BÜYÜK dosyaları görüntülemek için hangi reklam belirtin. VMAP dosyaları çeşitli reklam yürütmek ve geniş XML içeren ne zaman belirtin. A dosyaları, ayrıca geniş bir XML içerebilir sıra reklam için başka bir yoludur. VPAID dosyaları, video oynatıcı ad veya ad sunucusu arasında bir arabirim tanımlar.

Her player çerçevesi farklı şekilde çalışır ve her biri kendi makalesinde ele alınacak. Bu makalede, reklam ekleme için kullanılan temel mekanizmalarını açıklar. Video oynatıcı uygulamaları reklamları bir ad sunucusundan istek. Ad sunucusu, çeşitli yollarla yanıt verebilir:

* Çok sayıda dosya döndürür
* Bir VMAP dosyasıyla (embedded VAST) döndürür
* Bir a dosyasıyla (embedded VAST) döndürür
* GENİŞ bir VPAID reklam dosyasıyla döndürür

### <a name="using-a-video-ad-service-template-vast-file"></a>Bir Video Ad hizmet şablonu (VAST) dosyasını kullanarak
Hangi ad veya görüntülenecek reklamlar çok sayıda dosya belirtir. Aşağıdaki XML, doğrusal bir ad için geniş bir dosya örneğidir:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
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
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                    <![CDATA[http://www.myserver.com/media/myad_200_4x3.wmv]]>
                  </MediaFile>
                  <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
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
```

Doğrusal ad tarafından açıklanan <**doğrusal**> öğesi. Ad süreyi belirten, olayları izleme, izleme'ye tıklayın ve bir dizi aracılığıyla, tıklayın **MediaFile** öğeleri. İzleme olayları içinde belirtilen <**TrackingEvents**> öğesi ve ad görüntülerken gerçekleşen çeşitli olayları izlemek bir ad sunucusu sağlar. Bu durumda başlangıç, Orta, tam ve genişletin olayları izlenir. Ad görüntülendiğinde başlangıç olayı oluşur. Orta olayı en azından ad zaman çizelgesi yüzdesi 50 görüntülenen oluşur. Ad sonuna çalıştırdığınızda tamamlama olayı oluşur. Kullanıcı için tam ekran video oynatıcı genişletirken genişletme olayı oluşur. Clickthroughs ile belirtilen bir <**geçişli tıklatma**> öğesinde bir <**VideoClicks**> öğesi ve kullanıcı ad tıkladığında görüntülenecek bir kaynak için bir URI belirtir. ClickTracking belirtilen bir <**ClickTracking**> öğesini de içinde <**VideoClicks**> öğesi ve kullanıcı ad tıkladığında istemek oyuncu için bir izleme kaynağı belirtir . <**MediaFile**> öğeleri belirli bir ad kodlama hakkında bilgi belirtin. Birden fazla bir tane <**MediaFile**> öğesi, video oynatıcı seçebilir platform için en iyi kodlama.

Belirli bir sırada doğrusal reklam görüntülenebilir. Bunu yapmak için ek Ekle `<Ad>` VAST öğelerine dosya ve dizisi özniteliği kullanılarak sırasını belirtin. Aşağıdaki örnek bunu göstermektedir:

```xml
    <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
      <Ad id="1" sequence="0">
        <InLine>
          <AdSystem version="2.0 alpha">Atlas</AdSystem>
          <AdTitle>Unknown</AdTitle>
          <Description>Unknown</Description>
          <Survey></Survey>
          <Error></Error>
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad1trackingResource]]></Impression>
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
          <Impression id="Atlas"><![CDATA[http://myserver.com/Impression/Ad2trackingResource]]></Impression>
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
```

Doğrusal reklam içinde belirtilen bir `<Creative>` de öğesi. Aşağıdaki örnekte gösterildiği bir `<Creative>` doğrusal ad açıklayan öğesi.

```xml
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
```

<**NonLinearAds**> bir veya daha fazla öğe içerebilir <**NonLinear**> öğeleri, her biri bir doğrusal ad tanımlayabilir. <**NonLinear**> öğesi kaynak için doğrusal bir ad belirtir. Kaynak olabilir bir <**StaticResource**> e <**IFrameResource**>, veya bir <**HTMLResource**>. \<**StaticResource**> HTML olmayan kaynak açıklar ve kaynak nasıl görüntüleneceğini belirten bir creativeType öznitelik tanımlar:

Görüntü/gif, görüntü/jpeg, görüntü/png – kaynak bir HTML biçiminde görüntülenir <**img**> etiketi.

Application/x-javascript – kaynak, bir HTML biçiminde görüntülenir <**betik**> etiketi.

Application/x-shockwave-flash – kaynak bir Flash player görüntülenir.

**IFrameResource** IFRAME içinde görüntülenen bir HTML kaynak açıklar. **HTMLResource** bir web sayfasına eklenen HTML kod parçasını tanımlar. **TrackingEvents** izleme olaylarını ve olay gerçekleştiğinde istemek için URI belirtin. Bu örnekte acceptInvitation ve Daralt olayları izlenir. Daha fazla bilgi için **NonLinearAds** IAB.NET/VAST öğe ve alt öğeleri için bkz. Unutmayın **TrackingEvents** öğe içinde bulunan **NonLinearAds** öğe yerine **NonLinear** öğesi.

Yardımcısı reklam içinde tanımlanmış bir `<CompanionAds>` öğesi. `<CompanionAds>` Bir veya daha fazla öğe içerebilir `<Companion>` öğeleri. Her `<Companion>` öğesi Yardımcısı ad açıklar ve içerebilir bir `<StaticResource>`, `<IFrameResource>`, veya `<HTMLResource>` doğrusal bir ad olduğu gibi aynı şekilde belirtilir. Birden çok yardımcı reklamlar çok sayıda dosya içerebilir ve oynatıcı uygulaması görüntülemek için en uygun ad seçebilirsiniz. VAST hakkında daha fazla bilgi için bkz: [geniş 3.0](https://www.iab.net/media/file/VASTv3.0.pdf).

### <a name="using-a-digital-video-multiple-ad-playlist-vmap-file"></a>Birden çok Ad (VMAP) çalma listesi dosyası Dijital Video kullanma
VMAP dosya ad sonları olduğunda, ne kadar her kesintisidir, kaç reklamları bir sonu içinde görüntülenebilir ve reklam türleri olması olabilir belirtmenizi sağlar bir sonu görüntülenir. Aşağıdaki örnek VMAP dosyasındaki tanımlayan bir tek ad sonu:

```xml
    <vmap:VMAP xmlns:vmap="http://www.iab.net/vmap-1.0" version="1.0">
      <vmap:AdBreak breakType="linear" breakId="mypre" timeOffset="start">
        <vmap:AdSource allowMultipleAds="true" followRedirects="true" id="1">
          <vmap:VASTData>
            <VAST version="2.0" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="oxml.xsd">
              <Ad id="115571748">
                <InLine>
                  <AdSystem version="2.0 alpha">Atlas</AdSystem>
                  <AdTitle>Unknown</AdTitle>
                  <Description>Unknown</Description>
                  <Survey></Survey>
                  <Error></Error>
                  <Impression id="Atlas"><![CDATA[https://view.atdmt.com/000/sview/115571748/direct;ai.201582527;vt.2/01/634364885739970673]]></Impression>
                  <Creatives>
                    <Creative id="video" sequence="0" AdID="">
                      <Linear>
                        <Duration>00:00:32</Duration>
                        <MediaFiles>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_200" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="200" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_200_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_300" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="300" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_2_000_300_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_500" maintainAspectRatio="true" scalable="true"  delivery="progressive" bitrate="500" width="400" height="300" type="video/x-ms-wmv">
                            <![CDATA[http://smf.blob.core.windows.net/samples/ads/media/XBOX_HD_DEMO_700_1_000_500_4x3.wmv]]>
                          </MediaFile>
                          <MediaFile apiFramework="Windows Media" id="windows_progressive_700" maintainAspectRatio="true" scalable="true" delivery="progressive" bitrate="700" width="400" height="300" type="video/x-ms-wmv">
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
```

VMAP dosya ile başlayan bir `<VMAP>` birini veya daha fazlasını içeren `<AdBreak>` öğeleri, her bir ad sonu tanımlama. Her ad sonu sonu türü, kesme kimliği ve saati uzaklığı belirtir. BreakType öznitelik sonu sırasında yürütülebilecek ad türünü belirtir: Doğrusal, doğrusal, veya görüntüleme. GENİŞ Yardımcısı reklam görüntüleme reklam eşlenir. Bir virgülle ayrılmış (boşluksuz) listesinde birden fazla ad türü belirtilebilir. BreakID ad için isteğe bağlı bir tanımlayıcıdır. TimeOffset ad ne zaman görüntüleneceğini belirtir. Aşağıdaki yollardan birinde belirtilebilir:

1. Saat – .mmm milisaniye olduğu SS veya ss:dd:ss.mmm biçiminde. Bu özniteliğin değeri ad sonu başına video zaman çizelgesinin başından süresini belirtir.
2. %N biçimi – yüzde burada n video zaman çizelgesini ad yürütmeden önce yürütülecek yüzdesidir
3. Başlangıç/bitiş – önce veya sonra video görüntülenen bir ad görüntüleneceğini belirtir
4. Getirin – ad sonları zamanlamasını bilinmeyen, canlı akış gibi olduğunda ad sonları sırasını belirtir. Her ad sonu sırasını n 1 veya daha büyük bir tamsayı olduğu #n biçiminde belirtilir. 1 belirten ad yürütülen ad yürütülen ikinci fırsatta vb. 2 ilk fırsatta gösterir.

İçinde `<AdBreak>` öğesini bir <**AdSource**> öğesi. <**AdSource**> öğesi aşağıdaki öznitelikler içerir:

1. Kimliği-ad kaynağı için bir tanımlayıcı belirtir
2. allowMultipleAds – ad sonu sırasında birden çok reklam gösterilip gösterilemeyeceğini belirten bir Boole değeri
3. followRedirects – video oynatıcı uymanız belirten isteğe bağlı bir Boolean değeri içinde bir ad yanıtı yeniden yönlendirir.

<**AdSource**> öğesi bir satır içi ad yanıt veya bir ad yanıt başvurusu oynatıcı sağlar. Aşağıdaki öğelerden birini içerebilir:

* `<VASTAdData>` GENİŞ ad yanıt VMAP dosyasında katıştırılacağını belirtir
* `<AdTagURI>` başka bir sistemden ad yanıt başvuran bir URI
* `<CustomAdData>` BÜYÜK olmayan bir yanıtı temsil eden - isteğe bağlı bir dize

Bu örnekte, bir satır içi ad yanıtı ile belirtilen bir `<VASTAdData>` geniş ad yanıtını içeren öğe. Diğer öğeler hakkında daha fazla bilgi için bkz. [VMAP](https://www.iab.net/guidelines/508676/digitalvideo/vsuite/vmap).

<**AdBreak**> öğesi de içerebilir bir <**TrackingEvents**> öğesi. <**TrackingEvents**> öğesi başlangıç veya bitiş ad sonu veya olup ad sonu sırasında bir hata oluştu izlemenize olanak sağlar. <**TrackingEvents**> bir veya daha fazla öğe içeriyor <**izleme**> öğeleri, bir izleme olayı ve bir izleme URI her biri belirtir. Olası izleme olaylarını şunlardır:

1. bir ad sonu başına breakStart – izler
2. breakEnd – tamamlandığında, bir ad sonu izleyin
3. hata – ad sonu sırasında oluşan hata izler

Aşağıdaki örnek, izleme olaylarını belirten bir VMAP dosyası gösterir.

```xml
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
```

Daha fazla bilgi için <**TrackingEvents**> öğesi ve alt öğeleri için bkz. http://iab.net/VMAP.pdf

### <a name="using-a-media-abstract-sequencing-template-mast-file"></a>Şablon (a) dosyası sıralaması bir medya soyut kullanma
Bir a dosyasını tanımlayan bir ad görüntülendiğinde Tetikleyicileri belirtmenizi sağlar. Tetikleyiciler öncesi Top ad, bir orta Top ad ve sonrası ad içeren bir örnek a dosyası verilmiştir.

```xml
    <MAST xsi:schemaLocation="http://openvideoplayer.sf.net/mast http://openvideoplayer.sf.net/mast/mast.xsd" xmlns="http://openvideoplayer.sf.net/mast" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
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
```


A dosya ile başlayan bir **a** içeren bir öğe **Tetikleyicileri** öğesi. `<triggers>` Öğesi içeren bir veya daha fazla **tetikleyici** ne zaman bir ad yürütülme tanımlayan öğeler.

**Tetikleyici** öğesi içeren bir **startConditions** yürütmek bir ad ne zaman başlaması gerektiğini belirten öğe. **StartConditions** öğesi içeren bir veya daha fazla `<condition>` öğeleri. Yaparken her `<condition>` tetikleyici başlatılır veya olup olmamasına iptal true olarak değerlendirilen `<condition>` içinde yer alan bir **startConditions** veya **endConditions** öğesi sırasıyla. Zaman birden çok `<condition>` öğeleri, bir örtük veya olarak kabul edilir, herhangi bir koşul true olarak değerlendirilmesi başlatmak tetikleyici neden olur. `<condition>` öğeleri içe olabilir. Olduğunda alt `<condition>` öğeleri önceden bir örtük ve kabul edilir, tüm koşullar tetikleyicinin başlatmak true olarak değerlendirilmelidir. `<condition>` Öğesi içeren bir koşul tanımlayarak aşağıdaki öznitelikleri:

1. **tür** – koşulu, olay veya özellik türünü belirtir
2. **adı** – özelliği veya değerlendirme sırasında kullanılacak olay adı
3. **değer** – bir özelliğe karşı hesaplanacak olan değer
4. **İşleç** – değerlendirme sırasında kullanılacak işlemi: EQ (eşittir), NEQ (eşit değildir), GTR (büyük), GEQ (büyük veya buna eşit), LT (küçüktür), LEQ (küçüktür veya eşittir), MOD (mod)

**endConditions** de içeren `<condition>` öğeleri. Bir koşul true tetikleyici değerlendirirken sıfırlanır. `<trigger>` Öğeyi de içeren bir `<sources>` birini veya daha fazlasını içeren `<source>` öğeleri. `<source>` Ad yanıt ve ad yanıtının türünü tanımlayan URI öğesi. Bu örnekte, geniş bir yanıt olarak bir URI verildi.

```xml
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
```

### <a name="using-video-player-ad-interface-definition-vpaid"></a>Video oynatıcı Ad arabirim tanımı (VPAID) kullanma
VPAID, bir video oynatıcı ile iletişim kurmak yürütülebilir ad birimleri etkinleştirmek için bir API'dir. Bu, yüksek oranda etkileşimli ad deneyimler sağlar. Kullanıcının ad ile etkileşim kurabilir ve ad Görüntüleyici tarafından gerçekleştirilen eylemler vermesini sağlayabilirsiniz. Örneğin, bir ad, ad daha uzun bir sürümünü veya daha fazla bilgi görüntülemek kullanıcının olanak tanıyan düğmeleri görüntüleyebilir. Video oynatıcı VPAID API desteklemesi gerekir ve yürütülebilir ad API uygulamanız gerekir. Ne zaman bir oynatıcı VPAID ad içeren geniş bir yanıt bir ad sunucusu ad sunucusundan yanıt ister.

Bir yürütülebilir ad Adobe Flash™ veya yürütülebilir bir web tarayıcısında JavaScript gibi bir çalışma zamanı ortamında yürütülmelidir kod oluşturulur. Bir ad sunucusu VPAID ad içeren geniş bir yanıt döndürüldüğünde apiFramework değeri öznitelik içinde `<MediaFile>` öğe "VPAID" olmalıdır. Bu öznitelik içinde ad VPAID yürütülebilir ad olduğunu belirtir. Type özniteliği "application/x-shockwave-flash" veya "application/x-javascript" gibi yürütülebilir dosya, MIME türüne ayarlanması gerekir. Aşağıdaki XML parçacığını gösterildiği `<MediaFile>` VPAID yürütülebilir ad içeren geniş bir yanıt öğesinden.

```xml
    <MediaFiles>
       <MediaFile id="1" delivery="progressive" type=”application/x-shockwaveflash”
                  width=”640” height=”480” apiFramework=”VPAID”>
           <!-- CDATA wrapped URI to executable ad -->
       </MediaFile>
    </MediaFiles>
```

Yürütülebilir bir ad kullanılarak başlatılabilir `<AdParameters>` öğesiyle `<Linear>` veya `<NonLinear>` öğeleri geniş bir yanıt. Daha fazla bilgi için `<AdParameters>` öğesi bkz [geniş 3.0](https://www.iab.net/media/file/VASTv3.0.pdf). VPAID API'si hakkında daha fazla bilgi için bkz. [VPAID 2.0](https://www.iab.net/media/file/VPAID_2.0_Final_04-10-2012.pdf).

## <a name="implementing-a-windows-or-windows-phone-8-player-with-ad-support"></a>Bir Windows veya Windows Phone 8 Player Ad desteği ile uygulama
Microsoft Media platformu: Windows 8 için Player Framework ve Windows Phone 8, framework kullanarak bir video oynatıcı uygulamanın nasıl uygulanacağını gösteren örnek uygulamalar koleksiyonunu içerir. Player çerçevesi ve örneklerinden indirebilirsiniz [Windows 8 için Player Framework ve Windows Phone 8](https://playerframework.codeplex.com).

Microsoft.PlayerFramework.Xaml.Samples çözümü açtığınızda, klasörleri proje içinde bir dizi görürsünüz. Reklam klasör ilgili bir video oynatıcı ad desteğiyle oluşturmak için örnek kod içerir. İçinde bir reklam klasörü bir XAML/cs dosyaları hangi farklı bir yolla reklam ekleme işlemini gösterir sayısıdır. Aşağıdaki listede her açıklanmaktadır:

* AdPodPage.xaml ad pod görüntüleme işlemini göstermektedir.
* AdSchedulingPage.xaml reklam zamanlama gösterilmektedir.
* FreeWheelPage.xaml FreeWheel eklentisi reklam zamanlamak için nasıl kullanılacağını gösterir.
* MastPage.xaml reklam a dosyasıyla zamanlama gösterilmektedir.
* Program aracılığıyla bir videonun reklam zamanlama ProgrammaticAdPage.xaml gösterir.
* Bir ad geniş dosyası olmadan zamanlama ScheduleClipPage.xaml gösterir.
* Bir doğrusal ekleme ve yardımcı ad VastLinearCompanionPage.xaml gösterir.
* VastNonLinearPage.xaml doğrusal olmayan ad nasıl ekleneceğini gösterir.
* VmapPage.xaml reklam VMAP dosyasıyla belirteceğiniz gösterilmektedir.

Bu örneklerden her biri player framework tarafından tanımlanan MediaPlayer sınıfı kullanır. Çoğu örnekleri çeşitli ad yanıt biçimleri için destek ekleme eklentileri kullanın. ProgrammaticAdPage örnek bir MediaPlayer örneğine program aracılığıyla etkileşim kurar.

### <a name="adpodpage-sample"></a>AdPodPage örnek
Bu örnek, bir ad görüntülemek ne zaman tanımlamak için AdSchedulerPlugin kullanır. Bu örnekte, bir orta Top tanıtım beş saniye sonra yürütülecek zamanlanır. Ad pod (Reklam sırada görüntülemek için bir grubu) bir ad sunucusundan döndürülen geniş bir dosya belirtilir. Çok sayıda dosya URI'si belirtilen `<RemoteAdSource>` öğesi.

```xml
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
```

AdSchedulerPlugin hakkında daha fazla bilgi için bkz: [Windows 8 ve Windows Phone 8 Player Framework Duyurusu](https://playerframework.codeplex.com/wikipage?title=Advertising&referringTitle=Windows%208%20Player%20Documentation)

### <a name="adschedulingpage"></a>AdSchedulingPage
Bu örnek ayrıca AdSchedulerPlugin kullanır. Bu, üç reklam, öncesi ad, bir orta Top ad ve sonrası ad zamanlar. URI VAST her ad için belirtilen bir `<RemoteAdSource>` öğesi.

```xml
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
```

### <a name="freewheelpage"></a>FreeWheelPage
Bu örnek, zamanlama bilgileri ad yanı sıra ad içerik belirten bir SmartXML dosyasına işaret eden bir URI öğesini belirten bir kaynak özniteliğini belirtir FreeWheelPlugin kullanır.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:FreeWheelPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/freewheel.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="mastpage"></a>MastPage
Bu örnek, bir a dosya kullanmanıza olanak tanır MastSchedulerPlugin kullanır. Kaynak özniteliği a dosyasının konumunu belirtir.
```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:MastSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/mast.xml" />
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

### <a name="programmaticadpage"></a>ProgrammaticAdPage
Bu örnek program aracılığıyla MediaPlayer ile etkileşime girer. MediaPlayer ProgrammaticAdPage.xaml dosyası oluşturur:

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4"/>
```

Bir ad görüntülenmesi gerekir ve ardından geniş bir dosyaya bir URI belirterek bir RemoteAdSource yükler ve ardından ad çalar MarkerReached olay işleyicisi ekler ProgrammaticAdPage.xaml.cs dosyayı belirtmek için bir TimelineMarker ekler bir AdHandlerPlugin oluşturur.

```csharp
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
```

### <a name="scheduleclippage"></a>ScheduleClipPage
Bu örnek, bir orta Top ad ad içeren bir .wmv dosyasını belirterek zamanlamak için AdSchedulerPlugin kullanır.

```xml
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
```

### <a name="vastlinearcompanionpage"></a>VastLinearCompanionPage
Bu örnek, bir orta Top doğrusal ad Yardımcısı ad ile zamanlamak için AdSchedulerPlugin kullanılması gösterilmektedir. `<RemoteAdSource>` Öğesi geniş dosyasının konumunu belirtir.

```xml
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
```

### <a name="vastlinearnonlinearpage"></a>VastLinearNonLinearPage
Bu örnek, bir doğrusal zamanlamak için AdSchedulerPlugin ve doğrusal olmayan bir ad kullanır. Çok sayıda dosya konum ile belirtilen `<RemoteAdSource>` öğesi.

```xml
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
```

### <a name="vmappage"></a>VMAPPage
Bu örnek VmapSchedulerPlugin VMAP dosyasını kullanarak reklamları zamanlamak için kullanır. URI VMAP dosyasının kaynak özniteliğinde belirtilen `<VmapSchedulerPlugin>` öğesi.

```xml
    <mmppf:MediaPlayer x:Name="player" Source="http://smf.blob.core.windows.net/samples/videos/bigbuck.mp4">
                <mmppf:MediaPlayer.Plugins>
                    <ads:VmapSchedulerPlugin Source="http://smf.blob.core.windows.net/samples/win8/ads/vmap.xml"/>
                    <ads:AdHandlerPlugin/>
                </mmppf:MediaPlayer.Plugins>
            </mmppf:MediaPlayer>
```

## <a name="implementing-an-ios-video-player-with-ad-support"></a>Bir iOS Video Oynatıcı Ad desteğiyle uygulama
Microsoft Media platformu: İOS için Player Framework framework kullanarak bir video oynatıcı uygulamanın nasıl uygulanacağını gösteren örnek uygulamalar koleksiyonunu içerir. Player çerçevesi ve örneklerinden indirebilirsiniz [Azure Media Player çerçevesi](https://github.com/Azure/azure-media-player-framework). GitHub sayfasında player çerçevesi hakkında daha fazla bilgi içeren bir Wiki bağlantısını ve oynatıcı örnek giriş vardır: [Azure Media Player Wiki](https://github.com/Azure/azure-media-player-framework/wiki/How-to-use-Azure-media-player-framework).

### <a name="scheduling-ads-with-vmap"></a>Reklam VMAP ile zamanlama
Aşağıdaki örnek, reklam VMAP dosyasını kullanarak zamanlama gösterilmektedir.

```csharp
    // How to schedule an Ad using VMAP.
    //First download the VMAP manifest

    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVMAP.xml"]])
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
```

### <a name="scheduling-ads-with-vast"></a>Reklam VAST ile zamanlama
Aşağıdaki örnek, bir geç bağlama geniş ad zamanlama işlemi gösterilmektedir.


```csharp
    //Example:3 How to schedule a late binding VAST ad.
    // set the start time for the ad
    adLinearTime.startTime = 13;
    adLinearTime.duration = 0;
    // Specify the URI of the VAST file
    NSString *vastAd1=@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml";
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
```

   Aşağıdaki örnek, bir erken bağlama geniş ad zamanlama işlemi gösterilmektedir.

```csharp
    //Example:4 Schedule an early binding VAST ad
    //Download the VAST file
    if (![framework.adResolver downloadManifest:&manifest withURL:[NSURL URLWithString:@"https://portalvhdsq3m25bf47d15c.blob.core.windows.net/vast/PlayerTestVAST.xml"]])
    {
        [self logFrameworkError];
    }
    else
    {
        adLinearTime.startTime = 7;
        adLinearTime.duration = 0;

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
```

Aşağıdaki örnek kaba kesme düzenleme (Nak) kullanarak bir ad eklemek nasıl gösterir

```csharp
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
```

Aşağıdaki örnek, bir ad pod zamanlamak gösterilmektedir.

```csharp
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
```

Aşağıdaki örnek, bir Yapışkan Orta Top ad zamanlamak gösterilmektedir. Yapışkan olmayan bir ad, herhangi bir arama bağımsız olarak Görüntüleyicisi gerçekleştirir sonra yalnızca oynatılır.

```csharp
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
```

Aşağıdaki örnek, bir Yapışkan Orta Top ad zamanlamak gösterilmektedir. Belirtilen nokta video zaman çizelgesi üzerinde her erişildiğinde Yapışkan ad görüntülenir.

```csharp
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
```

Aşağıdaki örnek, bir sonrası ad zamanlama işlemi gösterilmektedir.

```csharp
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
```

Aşağıdaki örnek, bir öncesi ad zamanlama işlemi gösterilmektedir.

```csharp
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
```

Aşağıdaki örnek, bir orta Top katmana ad zamanlamak gösterilmektedir.

```csharp
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
```


## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirim gönder
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Ayrıca Bkz.
[Video oynatıcı uygulamaları geliştirme](media-services-develop-video-players.md)

