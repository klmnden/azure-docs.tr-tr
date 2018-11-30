---
title: Arama site, barındırılan UI Bing özel arama kullanın
titlesuffix: Azure Cognitive Services
description: Nasıl yapılandırılacağını açıklar Bing özel arama kullanıcı Arabirimi barındırılan.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: aahi
ms.openlocfilehash: c71597cf540cca67b9558ce28d20ce1d21ae0243
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52424995"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan UI deneyiminizi yapılandırın

Özel arama örneği kullanımınızın yapılandırdıktan sonra arama sonuçlarını almak ve bunları uygulamanızda görüntülemek için özel arama API'si çağırabilirsiniz. Veya, uygulamanız bir web uygulaması ise, özel arama sağlayan barındırılan bir kullanıcı Arabirimi kullanabilirsiniz.   

## <a name="configure-custom-hosted-ui"></a>Barındırılan özel kullanıcı Arabirimi yapılandırma

Web uygulamanız için barındırılan bir kullanıcı Arabirimi yapılandırmak için aşağıdaki adımları izleyin:

1. Özel arama oturum [portalı](https://customsearch.ai).  
  
2. Özel arama örneği tıklayın. Örneği oluşturmak için bkz [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).  

3. **Barındırılan kullanıcı arabirimi** sekmesine tıklayın.  
  
4. Bir düzen seçin.
  
  - Arama çubuğunu ve sonuçlar (varsayılan) &mdash; bu düzen, arama kutusu ve arama sonuçları ile geleneksel arama sayfasıdır.
  - Sonuçları yalnızca &mdash; bu düzeni yalnızca arama sonuçlarını görüntüler. Bu düzen, bir arama kutusu görüntülemez. Sorgu parametresini ekleyerek arama sorgusu sağlamanız gerekir (& q =\<sorgu dizesi >) istek URL'SİNDE JavaScript kod parçacığını veya HTML uç noktası bağlantısını için.
  - POP üzerinden &mdash; bu düzen, bir arama kutusu sağlar ve bir kayan kaplama arama sonuçlarını görüntüler.
      
5. Bir renk teması seçin. Olası temaları şunlardır: 
  
  - Klasik
  - Koyu
  - Skyline mavi

  Her temaları temayı ile web uygulamanızı en iyi çalıştığını görmek için tıklayın. Web uygulamanıza uygun hale getirmek için renk temasında ayarlama yapmanız gerekiyorsa **Temayı özelleştir**'e tıklayın. Tüm renk yapılandırmaları her düzen temasıyla kullanılamaz. Bir rengi değiştirmek için rengin RGB HEX değerini (örneğin, #366eb8) ilgili metin kutusuna girin. Veya, renk düğmesini tıklatın ve sizin için uygun Gölge'ye tıklayın. 
  
  Bir renk değiştirdikten sonra değişikliğin sağdaki Önizleme örnek etkilemesi denetleyin. Her zaman tıklayabilirsiniz **Varsayılana Sıfırla** seçilen tema için varsayılan renklere dönebilirsiniz.

  > [!NOTE]
  > Renk teması değiştirdiğinizde, Erişilebilirlik renkleri seçerken göz önünde bulundurun.

5. Altında **ek yapılandırmalar**, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya bunları kaldırma etkisini görmek için sağda önizleme bölmesinde görebilirsiniz. Kullanılabilir yapılandırma seçenekleri şunlardır:  
  
  - Web araması yapılandırmalar:
    - Web sonuçları etkin &mdash; web araması etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki bir Web sekmesi görürsünüz).
    - Etkinleştirme otomatik öneri &mdash; belirler özel otomatik öneri, etkin (bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) ek maliyet).
    - Web sayfa başına sonuç &mdash; (50 sonuçların sayfa başına en yüksek değer) bir zaman görüntülemek için Web'de arama sonuçlarıyla sayısı.
    - Resim yazısı &mdash; görüntüleri arama sonuçları ile gösterilip gösterilmeyeceğini belirler.
  
    Eğer aşağıdaki yapılandırmaları gösterilen **Gelişmiş yapılandırmaları Show**.  
  
    - Sözcüklerin vurgulanıp &mdash; sonuçları arama terimlerini kalın ile gösterilip gösterilmeyeceğini belirler. 
    - Bağlantı hedefi &mdash; kullanıcı arama sonucu tıkladığında Web sayfasına yeni bir tarayıcı sekmesinde (boş) veya aynı tarayıcı sekmesini (kendi) açar, belirler. 

  - Görüntü arama yapılandırmalar:
    - Görüntü etkin sonuçları &mdash; resim arama etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki bir görüntüler sekmesine görürsünüz).   
    - Sayfa başına sonuç görüntü &mdash; birer birer (150 sonuçların sayfa başına en yüksek değer) görüntülenecek resim araması sonuçları sayısı.  
  
    Eğer aşağıdaki yapılandırmayı gösterilen **Gelişmiş yapılandırmaları Show**.  
  
    - Filtrelerini etkinleştirme &mdash; kullanıcının Bing döndürür görüntülerini filtrelemek için kullanabileceğiniz filtreler ekler. Örneğin, kullanıcı için yalnızca animasyonlu GIF'leri sonuçlarını filtreleyebilirsiniz.

  - Video arama yapılandırmalar:
    - Video sonuçları etkin &mdash; video arama etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki videoları sekme görürsünüz).  
    - Sayfa başına video sonuçları &mdash; birer birer (150 sonuçların sayfa başına en yüksek değer) görüntülenecek video araması sonuç sayısı.
  
    Eğer aşağıdaki yapılandırmayı gösterilen **Gelişmiş yapılandırmaları Show**.  
  
    - Filtrelerini etkinleştirme &mdash; kullanıcının Bing döndürür videoları filtrelemek için kullanabileceğiniz filtreler ekler. Örneğin, kullanıcının belirli bir çözümleme videoları veya son 24 saat içinde bulunan videolar için sonuçları filtreleyebilirsiniz.

  - Çeşitli yapılandırmalar:
    - Sayfa başlığı &mdash; (için pop üzerinden düzeni) arama sonuçları sayfasının başlık alanında görüntülenen metin.
    - Araç çubuğu tema &mdash; arama sonuçları sayfası başlık alanı arka plan rengini belirler.  
  
    Eğer aşağıdaki yapılandırmaları gösterilen **Gelişmiş yapılandırmaları Show**.  
  
    - Arama kutusu metni yer tutucusu &mdash; girişi için arama kutusunu önceki görüntülenen metin.
    - Başlık bağlantı URL'si &mdash; odkaz na nadpis hedefi.
    - Logo URL'si &mdash; başlığı yanında görüntülenen resim. 
    - Favicon url &mdash; tarayıcının başlık çubuğunda görüntülenen simge.  

    Barındırılan kullanıcı Arabirimi (JavaScript kod parçacığını kullanırsanız, bunlar geçerli değildir) HTML uç noktası aracılığıyla yalnızca tükettiğiniz, şu yapılandırmalar geçerlidir.
    
    - Sayfa başlığı
    - Araç çubuğu tema
    - Başlık bağlantı URL'si
    - Logo URL'si
    - Faviicon URL'si  
  
6. Arama abonelik anahtarını girin veya aşağı açılan listeden seçin. Açılan listede, Azure hesabınızın aboneliklerden anahtarlarla doldurulur. Bkz: [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

7. Otomatik öneri etkinleştirilirse, otomatik öneri abonelik anahtarını girin veya aşağı açılan listeden seçin. Açılan listede, Azure hesabınızın aboneliklerden anahtarlarla doldurulur. Özel otomatik öneri özelliği bir belirli aboneliğe katmanı gerektirir, bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Sağ bölmede, özel barındırılan UI yapılandırmada değişiklik yapmak gibi yapılan değişiklikler için görsel bir başvuru sağlar. Görüntülenen arama sonuçlarını Örneğiniz için gerçek sonuçlar değildir.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Özel kullanıcı Arabirimi kullanma

Barındırılan kullanıcı Arabirimi, ya da kullanmak için: 

- Web sayfanızın komut dosyası Ekle  
  
  ```html
  <html>
      <body>
          <script type="text/javascript" 
              id="bcs_js_snippet"
              src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
          </script>
      </body>    
  </html>
  ```

- Veya bir Web tarayıcısından şu URL'yi kullanın.   
  
  `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`  
  
  > [!NOTE]
  > Aşağıdaki sorgu parametreleri, URL'ye gerektiği gibi ekleyin. Bu parametreler hakkında daha fazla bilgi için bkz: [özel arama API'si](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters) başvuru.
  >
  > - q
  > - Mkt
  > - safesearch
  > - setlang

  > [!IMPORTANT]
  > Gizlilik bildiriminiz veya diğer bildirimler ve koşulları sayfası görüntülenemiyor. Kullanımınız uygunluğu farklılık gösterebilir.  

Özel yapılandırma Kimliğinizi dahil olmak üzere ek bilgi için Git **uç noktaları** altında **üretim** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar

- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
