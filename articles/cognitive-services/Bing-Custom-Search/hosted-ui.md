---
title: 'Bing özel arama: Site araması | Microsoft Docs'
description: Barındırılan UI yapılandırmayı açıklar
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 593ea4d23f8ddcec8efc4be632afa2aab1a5210f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35352877"
---
# <a name="configure-your-hosted-ui-experience"></a>Barındırılan UI deneyiminizi yapılandırın
Özel arama örneğinizi yapılandırdıktan sonra arama sonuçlarını almak ve bunları uygulamanızda görüntülemek için özel arama API'si çağırabilirsiniz. Veya, uygulamanızı bir web uygulaması ise, özel arama sağlar barındırılan UI kullanabilirsiniz.   

## <a name="configure-custom-hosted-ui"></a>Barındırılan özel kullanıcı Arabirimi yapılandırma
Web uygulamanızda eklenecek barındırılan bir kullanıcı Arabirimi yapılandırmak için aşağıdaki yönergeleri kullanın.
1.  Özel arama oturum [portal](https://customsearch.ai).
2.  Özel arama örneği tıklatın. Örnek oluşturmak için bkz: [ilk Bing özel arama örneğinizi oluşturmak](quick-start.md).
3.  Tıklatın **barındırılan UI** sekmesi.
4.  Bir düzen seçin.
    - Arama çubuğunu ve sonuçları (varsayılan) &mdash; arama kutusunu görüntülemek ve arama sonuçları
    - Yalnızca sonuçları &mdash; olmayan bir arama kutusu görüntüler, yalnızca sonuçlarını gösterir
    - POP üzerinden &mdash; olmayan bir arama kutusu görüntüler, kayan bir katmana yalnızca sonuçlarını gösterir
    
   > [!IMPORTANT]
   > Seçerken customConfig sorgu parametresi eklediğinizden emin olun **yalnızca sonuçları** düzeni bkz [sorgu parametreleri](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters).

5.  Altında **ek yapılandırmalar**, uygulamanız için uygun değerleri sağlayın. Bu ayarlar isteğe bağlıdır. Uygulama veya kaldırarak etkisini görmek için sağ taraftaki önizleme bölmesine bakın.  Kullanılabilen yapılandırma seçenekleri şunlardır:
    - Web ara yapılandırmalar:
        - Web etkin sonuçları &mdash; web Ara sonuçların döndürülmesini belirler
        - Etkinleştirme otomatik öneri &mdash; belirler otomatik öneri özel durumunda etkin
        - Web sayfa başına sonuç &mdash; aynı anda görüntülemek için web arama sonuçları sayısı
        - Resim yazısı &mdash; görüntüleri Arama sonuçlarıyla gösterilip gösterilmeyeceğini belirler
        - Sözcükleri Vurgula &mdash; sonuçları arama terimlerini kalın yazı tipiyle gösterilip gösterilmeyeceğini belirler
    - Görüntü arama yapılandırmalar:
        - Görüntü etkin sonuçları &mdash; resim arama sonuçları döndürülür belirler
    - Çeşitli yapılandırmalar:
        - Sayfa başlığı &mdash; sayfa başlığı alanında görüntülenen metin
        - Araç çubuğu tema &mdash; sayfa başlık alanı arka plan rengini belirler
        - Arama kutusu metni yer tutucusu &mdash; girişi için arama kutusunu önceki görüntülenen metin
        - Başlık bağlantısı URL'sinin &mdash; başlık bağlantı hedefi
        - Logo URL'si &mdash; yanındaki başlığının görüntülenip görüntüsü 
        - Favicon url &mdash; simgesi tarayıcının başlık çubuğunda görüntülenir

   > [!IMPORTANT]
   > Görüntü arama veya Web arama en az biri etkinleştirilmelidir.

6.  Arama abonelik anahtarı girin veya bir aşağı açılan seçin. Aşağı açılan hesabınızın Azure aboneliklerini anahtarları ile doldurulur. Bkz: [Bilişsel Hizmetleri API hesap](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account).
7.  Autosuggest etkinleştirilirse, autosuggest abonelik anahtarı girin veya bir aşağı açılan seçin. Aşağı açılan hesabınızın Azure aboneliklerini anahtarları ile doldurulur. Özel Autosuggest belirtilen abonelik katman gerekiyorsa, bkz: [sayfaları fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/).

> [!NOTE]
> Barındırılan özel kullanıcı Arabirimi yapılandırmaya değişiklik gibi sağdaki bölmesinde yapılan değişiklikler için görsel bir başvuru sağlar. Görüntülenen arama sonuçlarını Örneğiniz için gerçek sonuçları değildir

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="consume-custom-ui"></a>Özel kullanıcı Arabirimi kullanma
Barındırılan UI ya da kullanmak için: 

- Web sayfanızda betik ekleyin
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
  > Gizlilik bildiriminizin veya diğer bildirim ve şartları sayfa görüntülenemiyor. Kullanımınız uygunluğuna farklılık gösterebilir.  

Özel yapılandırma Kimliğinizi dahil olmak üzere ek bilgi için Git **uç noktaları** altında **üretim** sekmesi.

## <a name="next-steps"></a>Sonraki adımlar
- [Metni vurgulama için decoration işaretlerini kullanın](./hit-highlighting.md)
- [Sayfa Web sayfaları](./page-webpages.md)