---
title: Bing özel arama için barındırılan bir kullanıcı Arabirimi yapılandırma | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Yapılandırma ve Bing özel arama için barındırılan bir kullanıcı Arabirimi tümleştirmek için bu makaleyi kullanın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.openlocfilehash: af1e65cc7dfe1a0934056ad141f4c62a96627bbb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127557"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan UI deneyiminizi yapılandırın

Bing özel arama bir JavaScript kod parçacığını Web sayfalarını ve web uygulamalarınızla kolayca tümleştirebilirsiniz barındırılan bir kullanıcı Arabirimi sağlar. Bing özel arama portal'ı kullanarak, düzen, renk ve UI arama seçenekleri yapılandırabilirsiniz.



## <a name="configure-the-custom-hosted-ui"></a>Barındırılan özel kullanıcı arabirimini yapılandırma

Web uygulamalarınız için barındırılan bir kullanıcı Arabirimi yapılandırmak için aşağıdaki adımları izleyin. Yaptığınız gibi sağ bölmede, kullanıcı arabiriminin bir önizleme sunar. Görüntülenen arama sonuçlarını Örneğiniz için gerçek sonuçlar değildir.

1. Bing özel arama için oturum açın [portalı](https://customsearch.ai).  
  
2. Bing özel arama örneğinizin seçin.

3. **Barındırılan kullanıcı arabirimi** sekmesine tıklayın.  
  
4. Bir düzen seçin.

    |  |  |
    |---------|---------|
    |Arama çubuğunu ve sonuçlar (varsayılan)    | Bir arama kutusu altındaki arama sonuçlarını görüntüler.         |
    |Yalnızca sonuçları     | Arama sonuçları yalnızca, bir arama kutusu olmadan görüntüler. Bu düzeni kullanılırken, arama sorgusu sağlamanız gerekir (`&q=<query string>`). JavaScript kod parçacığını veya HTML uç noktası bağlantısını istek URL'SİNDE bir sorgu parametresi ekleyin.        |
    |POP üzerinden     | Bir arama kutusu sağlar ve bir kayan kaplama arama sonuçlarını görüntüler.        |
    
5. Bir renk teması seçin. Renkleri tıklayarak uygulamanızı uyacak şekilde özelleştirebilirsiniz **Özelleştir tema**. Bir rengini değiştirmek için rengin RGB ONALTILI değer girin (örneğin, `#366eb8`), üzerinde renk Önizleme'yi tıklatın.

   Portalın sağ taraftaki yaptığınız değişiklikleri önizleyebilirsiniz. Tıklayarak **Varsayılana Sıfırla** seçtiğiniz temanın için varsayılan renkleri yaptığınız değişiklikleri geri.

   > [!NOTE]
   > Erişilebilirlik renkleri seçerken göz önünde bulundurun.

6. Altında **ek yapılandırmalar**, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya bunları kaldırma etkisini görmek için sağda önizleme bölmesinde görebilirsiniz. Kullanılabilir yapılandırma seçenekleri şunlardır:  

7. Arama abonelik anahtarını girin veya aşağı açılan listeden seçin. Açılan listede, Azure hesabınızın aboneliklerden anahtarlarla doldurulur. Bkz: [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).  

8. Otomatik öneri etkinleştirilirse, otomatik öneri abonelik anahtarını girin veya aşağı açılan listeden seçin. Açılan listede, Azure hesabınızın aboneliklerden anahtarlarla doldurulur. Özel otomatik öneri özelliği bir belirli aboneliğe katmanı gerektirir, bkz: [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

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

## <a name="configuration-options"></a>Yapılandırma seçenekleri

Barındırılan kullanıcı Arabirimi davranışı tıklayarak yapılandırabileceğiniz **ek yapılandırmalar**ve değerlerini sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya bunları kaldırma etkisini görmek için sağda önizleme bölmesinde görebilirsiniz. 

### <a name="web-search-configurations"></a>Web arama yapılandırmaları

|  |  |
|---------|---------|
|Etkin web sonuçları    | Web araması etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki bir Web sekmesi görürsünüz)        |
|Otomatik öneri etkinleştir     | Özel otomatik öneri, belirler etkinleştirilir (bkz [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/) ek maliyet).        |
|Web sayfa başına sonuç    | (50 sonuçların sayfa başına en yüksek değer) bir zaman görüntülemek için Web'de arama sonuçlarıyla sayısı.        |
|Resim yazısı   | Görüntü arama sonuçları ile gösterilip gösterilmeyeceğini belirler.|


Eğer aşağıdaki yapılandırmaları gösterilen **Gelişmiş yapılandırmaları Show**:


|  | |
|---------|---------|
|Sözcükleri Vurgula     | Sonuçları arama terimlerini kalın ile gösterilip gösterilmeyeceğini belirler.         |
|Bağlantı hedefi    |  Kullanıcı arama sonucu tıkladığında Web sayfasına yeni bir tarayıcı sekmesinde (boş) veya aynı tarayıcı sekmesini (kendi) açar, belirler.        |

### <a name="image-search-configurations"></a>Görüntü arama yapılandırmaları

| | |
|---------|---------|
|Etkin görüntü sonuçları     | Resim arama etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki bir görüntüler sekmesine görürsünüz).            |
|Sayfa başına sonuç görüntüsü     | Birer birer (150 sonuçların sayfa başına en yüksek değer) görüntülenecek resim araması sonuçları sayısı.          |

Eğer aşağıdaki yapılandırmayı gösterilen **Gelişmiş yapılandırmaları Show**.  
  
| | |
|---------|---------|
| Filtrelerini etkinleştir     | Kullanıcının Bing döndürür görüntülerini filtrelemek için kullanabileceğiniz filtreler ekler. Örneğin, kullanıcı için yalnızca animasyonlu GIF'leri sonuçlarını filtreleyebilirsiniz.|

### <a name="video-search-configurations"></a>Video arama yapılandırmaları

|  | |
|---------|---------|
|Etkin video sonuçları     | Video arama etkinleştirilip etkinleştirilmeyeceğini belirler (sayfanın üst kısmındaki videoları sekme görürsünüz).           |
|Sayfa başına video sonuçları   | Birer birer (150 sonuçların sayfa başına en yüksek değer) görüntülenecek video araması sonuç sayısı.        |

Eğer aşağıdaki yapılandırmayı gösterilen **Gelişmiş yapılandırmaları Show**.  
  
|  | |
|---------|---------|
|Filtrelerini etkinleştir    | Kullanıcının Bing döndürür videoları filtrelemek için kullanabileceğiniz filtreler ekler. Örneğin, kullanıcının belirli bir çözümleme videoları veya son 24 saat içinde bulunan videolar için sonuçları filtreleyebilirsiniz.          |

### <a name="miscellaneous-configurations"></a>Çeşitli yapılandırmalar


| |  |
|---------|---------|
|Sayfa başlığı   | Arama sonuçları sayfasını (değil pop üzerinden düzeni için), başlık alanında görüntülenen metin.        |
|Araç çubuğu tema    | Arama sonuçları sayfası başlık alanı arka plan rengini belirler. |

Eğer aşağıdaki yapılandırmaları gösterilen **Gelişmiş yapılandırmaları Show**.  

|Column1  |Column2  |
|---------|---------|
|Arama kutusu metni yer tutucusu   | Girişi için arama kutusunu önceki görüntülenen metin.        |
|Başlık bağlantı URL'si    |Odkaz na nadpis hedefi.         |
|Logo URL'si     | Başlığın yanında görüntülenen resim.         |
|Favicon    | Tarayıcının başlık çubuğunda görüntülenen simge.          |

Barındırılan kullanıcı Arabirimi (JavaScript kod parçacığını kullanırsanız, bunlar geçerli değildir) HTML uç noktası aracılığıyla yalnızca tükettiğiniz, şu yapılandırmalar geçerlidir.

- Sayfa başlığı
- Araç çubuğu tema
- Başlık bağlantı URL'si
- Logo URL'si
- Faviicon URL'si  

## <a name="next-steps"></a>Sonraki adımlar

- [Metni vurgulamak için süsleme işaretçilerini kullanma](./hit-highlighting.md)
- [Sayfa web sayfaları](./page-webpages.md)
