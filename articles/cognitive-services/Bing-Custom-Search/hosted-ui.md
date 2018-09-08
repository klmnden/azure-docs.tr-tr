---
title: 'Bing özel arama: Site araması | Microsoft Docs'
description: Barındırılan UI yapılandırılması açıklanmaktadır
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 7f2b97479ffcdb7ec8b3a1a635562d1fe68c3269
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44158440"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan UI deneyiminizi yapılandırın
Özel arama örneği kullanımınızın yapılandırdıktan sonra arama sonuçlarını almak ve bunları uygulamanızda görüntülemek için özel arama API'si çağırabilirsiniz. Veya, uygulamanız bir web uygulaması ise, özel arama sağlayan barındırılan bir kullanıcı Arabirimi kullanabilirsiniz.   

## <a name="configure-custom-hosted-ui"></a>Barındırılan özel kullanıcı Arabirimi yapılandırma
Web uygulamanıza dahil etmek için barındırılan bir kullanıcı Arabirimi yapılandırmak için aşağıdaki yönergeleri kullanın.
1.  Özel arama oturum [portalı](https://customsearch.ai).
2.  Özel arama örneği tıklayın. Örneği oluşturmak için bkz [ilk Bing özel arama örneğinizin oluşturma](quick-start.md).
3.  Tıklayın **barındırılan UI** sekmesi.
4.  Bir düzen seçin.
    - Arama çubuğunu ve sonuçlar (varsayılan) &mdash; bir arama kutusu görüntüleme ve arama sonuçları
    - Sonuçları yalnızca &mdash; yoksa, bir arama kutusu yalnızca sonuçları göster
    - POP üzerinden &mdash; yoksa, bir arama kutusu içinde kayan bir katmana yalnızca sonuçları göster
    
   > [!IMPORTANT]
   > Seçerken customConfig sorgu parametresi eklediğinizden emin olun **yalnızca sonuçları** düzeni bkz [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Altında **ek yapılandırmalar**, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya bunları kaldırma etkisini görmek için sağda önizleme bölmesinde görebilirsiniz.  Kullanılabilir yapılandırma seçenekleri şunlardır:
    - Web araması yapılandırmalar:
        - Web sonuçları etkin &mdash; Web'de arama sonuçlarıyla döndürülen olmadığını belirler
        - Etkinleştirme otomatik öneri &mdash; belirler özel otomatik öneri, etkin
        - Web sayfa başına sonuç &mdash; sayı aynı anda görüntülenecek web arama sonuçları
        - Resim yazısı &mdash; görüntüleri arama sonuçları ile gösterilip gösterilmeyeceğini belirler
        - Sözcüklerin vurgulanıp &mdash; sonuçları arama terimlerini kalın ile gösterilip gösterilmeyeceğini belirler
    - Görüntü arama yapılandırmalar:
        - Görüntü etkin sonuçları &mdash; resim araması sonuçları döndürüleceğini belirler
    - Çeşitli yapılandırmalar:
        - Sayfa başlığı &mdash; sayfası başlık alanında görüntülenen metin
        - Araç çubuğu tema &mdash; sayfası başlık alanı arka plan rengini belirler
        - Arama kutusu metni yer tutucusu &mdash; girişi için arama kutusunu önceki görüntülenen metin
        - Başlık bağlantı URL'si &mdash; odkaz na nadpis hedefi
        - Logo URL'si &mdash; başlığı yanında görüntülenen resmi 
        - Favicon url &mdash; tarayıcının başlık çubuğunda görüntülenen simgesi

   > [!IMPORTANT]
   > En az bir resim arama veya Web araması etkinleştirilmelidir.

6.  Arama abonelik anahtarını girin veya açılır listeden seçin. Aşağı açılan hesabınızın Azure abonelikleri anahtarlarından doldurulur. Bkz: [Bilişsel hizmetler API hesabı](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Otomatik öneri etkinleştirilirse, otomatik öneri abonelik anahtarını girin veya açılır listeden seçin. Aşağı açılan hesabınızın Azure abonelikleri anahtarlarından doldurulur. Özel otomatik öneri özelliği, belirtilen abonelik katmanı gerektirir, bkz: [fiyatlandırma sayfalarına](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Sağ bölmede, özel barındırılan UI yapılandırmada değişiklik yapmak gibi yapılan değişiklikler için görsel bir başvuru sağlar. Görüntülenen arama sonuçlarını Örneğiniz için gerçek sonuçlar değildir.

[!INCLUDE [publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Özel kullanıcı Arabirimi kullanma
Barındırılan kullanıcı Arabirimi, ya da kullanmak için: 

- Web sayfanızın komut dosyası Ekle
    ``` html
    <html>
        <body>
            <script type="text/javascript"
                id="bcs_js_snippet"            
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">            
            </script>
        </body>    
    </html>
    ```

- Sağlanan URL kullanın `https://ui.customsearch.ai/hosted?customConfig=YOUR-CUSTOM-CONFIG-ID`

  > [!IMPORTANT]
  > Gizlilik bildiriminiz veya diğer bildirimler ve koşulları sayfası görüntülenemiyor. Kullanımınız uygunluğu farklılık gösterebilir.  

Özel yapılandırma Kimliğinizi dahil olmak üzere ek bilgi için Git **uç noktaları** altında **üretim** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar
- [Metni vurgulayacak şekilde decoration işaretçileri kullanma](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)