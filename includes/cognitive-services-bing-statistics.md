---
title: include dosyası
description: include dosyası
services: cognitive-services
author: scottwhi-msft
ms.service: cognitive-services
ms.topic: include
ms.date: 04/09/2018
ms.author: scottwhi
ms.custom: include file
ms.openlocfilehash: 4e19c1afefdc5bcacebcb0d495193b48c7a6d724
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313719"
---
Bing istatistikleri analizi için Bing arama API'leri sağlar. Analytics arama birimi, en iyi sorgu dizeleri, coğrafi dağıtım ve daha fazlasını içerir. Bing istatistikleri Bing aramanızda Ücretli aboneliği etkinleştirmek için gidin, [Azure Pano](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), ücretli aboneliğinizi seçin ve Bing İstatistikler Etkinleştir'i tıklatın. Bing istatistikleri etkinleştirme artırır, abonelik hızı biraz (bkz [fiyatlandırma](https://aka.ms/bingstatisticspricing)).


> [!NOTE]
> Bing İstatistikler yalnızca ücretli abonelik ile kullanılabilir - ücretsiz deneme aboneliği ile kullanılamaz. 

> [!NOTE]
> Bing istatistikleri Pano kullanılabilir herhangi bir veri, dağıtım üçüncü taraflar için uygulamalar oluşturmak için kullanamazsınız.

Bing 24 saatte analiz verileri güncelleştirir ve yukarı geçmişi 13 ayın kopyalayabilmek için korur.

## <a name="accessing-your-analytics"></a>Analizlerinizi erişme

Analytics panoya erişmek için şu adrese gidin https://bingapistatistics.com. Ücretli aboneliğinizi almak için kullanılan aynı Microsoft hesabı (MSA) kullanarak kapattığınızdan emin olun.


## <a name="filtering-the-data"></a>Verileri filtreleme

Varsayılan olarak, çizelgeler ve grafikler erişiminiz tüm ölçüm verilerini yansıtır. Kaynaklar, pazarda uç noktalar'ı seçerek çizelgeler ve grafikler gösterilen veriler filtreleyebilirsiniz ve raporlama dönemi ilgilendiğiniz. Grafikler ve grafiklerinizi uyguladığınız filtreleri yansıtacak şekilde değiştirin. Aşağıdakiler, değişebilir filtreleri açıklanmaktadır.

- **Kaynak Kimliği**: Azure aboneliğiniz tanımlayan benzersiz kaynak kimliği. Birden fazla Bing arama API katmana abone olursanız liste birden çok kimliği içerir. Varsayılan olarak, tüm kaynaklar seçilir.  
  
- **Pazarda**: sonuçları yeri pazarda. Örneğin, en-us (İngilizce, Amerika Birleşik Devletleri). Varsayılan olarak, tüm pazarda seçilir. Tr WW Pazar Bing çağrısı bir pazar belirlemezse kullanan Pazar ve Bing kullanıcının Pazar belirlenemiyor olduğunu unutmayın.  
  
- **Uç noktaları**: Bing arama API uç noktaları. Liste Ücretli bir aboneliğe sahip tüm uç noktaları içerir. Varsayılan olarak, tüm uç noktaları seçilir.  

- **Zaman dilimi**: raporlama döneminde. Şunları belirtebilirsiniz:  
  
  - Tüm&mdash;veri 13 ayın kopyalayabilmek için içerir ayarlama  
  - Son 24 saat&mdash;son 24 saat analytics'ten içerir  
  - Geçen hafta&mdash;önceki yedi gün analytics'ten içerir  
  - Geçen ay&mdash;önceki 30 gündeki analizi içerir  
  - Özel bir tarih aralığı&mdash;belirtilen tarih aralığı analytics'ten varsa içerir  
  
  > [!NOTE]  
  > Panoda yüzey ölçümleri 24 saate kadar sürebilir. Pano verileri son güncelleştirme saati ve tarihi gösterir.  
  
  > [!NOTE]  
  > Ölçümleri Bing istatistikleri eklenti etkinleştirme saati kullanılabilir. 


## <a name="charts-and-graphs"></a>Grafikler ve grafikler

Pano grafikler ve seçili uç noktası için kullanılabilir ölçümleri grafiklerini gösterir. Tüm ölçümleri tüm uç noktaları için kullanılabilir. Grafikler ve her bitiş noktasıyla ilgili grafikleri statik (grafikler ve görüntülemek için grafikleri seçmeniz değil). Panosu yalnızca grafikler ve var olduğu veri grafikleri gösterir. 

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Olası ölçümler şunlardır: Her ölçüm bitiş noktası kısıtlamaları notlar. 

- **Birim çağrı**: raporlama döneminde yapılan çağrılarının sayısını gösterir. Raporlama süresi bir gün için ise, grafik saat başına yapılan çağrılarının sayısını gösterir. Aksi takdirde, grafik raporlama döneminde günde yapılan çağrılarının sayısını gösterir.  
  
  > [!NOTE]
  > Arama birimi, genellikle yalnızca başarılı çağrıları içeren fatura raporlardan farklı olabilir. 
  
-  **En çok yapılan sorgular**: raporlama döneminde üst sorgular ve her sorgu yinelenme sayısını gösterir. Gösterilen olan sorgu sayısını yapılandırabilirsiniz. Örneğin, üst 25, 50 veya 75 sorguları gösterebilir. En sık kullanılan sorguların şu uç noktalar için kullanılabilir değil:  
  
  - /images/trending
  - / görüntüleri/ayrıntıları
  - / görüntüleri/visualsearch
  - /Videos/trending
  - / Video/ayrıntıları
  - /News
  - / haber/trendingtopics
  - /suggestions  
  
  > [!NOTE]  
  > Bazı sorgu terimlerinin gizlenen vb. gibi e-postalar, telefon numaraları, SSN, gizli bilgileri kaldırmak için.  

- **Coğrafi dağıtım**: sonuçları yeri pazarda. Örneğin, en-us (İngilizce, Amerika Birleşik Devletleri). Bing kullanan `mkt` belirtilirse sorgu parametresi Pazar belirlemek için. Aksi takdirde, Bing arayanın IP adresi gibi sinyalleri Pazar belirlemek için kullanır.  
  
- **Yanıt kodu dağıtım**: HTTP durum kodları raporlama döneminde tüm çağrıları.  
  
- **Kaynak dağıtım çağrı**: kullanıcılar tarafından kullanılan tarayıcılar türleri. Örneğin, kenar, Chrome, Safari ve FireFox. Bot, Postman veya bir konsol uygulamasından curl kullanarak gibi bir tarayıcı dışında yapılan çağrıları kitaplıkları altında gruplandırılır. Kaynak isteğin Kullanıcı aracısını üstbilgi değeri kullanılarak belirlenir. İstek User-Agent üstbilgisi içermiyorsa, Bing kaynağını diğer sinyaller türetilen dener.  
  
- **Güvenli arama dağıtım**: güvenli arama değerleri dağıtım. Örneğin, Orta veya katı kapalı. `safeSearch` Sorgu parametresi belirtilmişse değerini içeriyor. Aksi takdirde, Bing orta değer varsayılan olarak ayarlanır.  
  
- **Yanıtlar istenen dağıtım**: Web arama API içinde istenen yanıtlar `responseFilter` sorgu parametresi.  
  
- **Yanıtlar döndürülen dağıtım**: Web arama API yanıtta döndürülen yanıtlar.  
  
- **Yanıt sunucu dağıtım**: API isteklerinizin sunulan uygulama sunucusu. Olası değerler şunlardır: aratıp (için masaüstü ve dizüstü aygıtlardan sunulan trafik) ve aratıp mobil (için mobil aygıtlardan sunulan trafik). Sunucu isteğin Kullanıcı aracısını üstbilgi değeri kullanılarak belirlenir. İstek User-Agent üstbilgisi içermiyorsa, Bing sunucu diğer sinyaller türetilen dener.  
  


Her uç noktası için kullanılabilir analytics gösterir.

![Uç nokta destek matrisi göre dağıtım](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)


