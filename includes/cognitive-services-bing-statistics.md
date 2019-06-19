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
ms.openlocfilehash: 6016b13fe7d3e1f3b673bd2446d2f68b04878cd6
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188654"
---
Bing Statistics, Bing arama API'leri için analiz sağlar. Analytics çağrı hacmi, en iyi sorgu dizeleri, coğrafi dağılımı ve daha fazlasını içerir. Bing Statistics, Bing arama Ücretli aboneliği etkinleştirmek için gidin, [Azure panosuna](https://portal.azure.com/#create/Microsoft.CognitiveServicesBingSearch-v7), ücretli aboneliğinizi seçin ve Bing Statistics Etkinleştir'e tıklayın. Bing Statistics etkinleştirme artırır, abonelik oranı biraz (bkz [fiyatlandırma](https://aka.ms/bingstatisticspricing)).

> [!NOTE]
> Bing Statistics, yalnızca ücretli aboneliklerle kullanılabilir - ücretsiz deneme abonelikleri ile kullanılabilir değil.

> [!NOTE]
> Bing Statistics Pano kullanılabilir olan herhangi bir veri dağıtımı için üçüncü taraf uygulamaları oluşturmak için kullanamazsınız.

Bing analiz verileri 24 saatte bir güncelleştirir ve ayarlama geçmişi için 13 ay tutarında tutar.

## <a name="accessing-your-analytics"></a>Analiz erişme

Analytics panonuzu erişmek için Git https://bingapistatistics.com. Ücretli aboneliğinizi almak için kullandığınız aynı Microsoft hesabı (MSA) kullanarak açtığınızdan emin olun.

## <a name="filtering-the-data"></a>Verileri filtreleme

Varsayılan olarak, grafikler ve graflar erişimi olan tüm ölçüm verileri yansıtır. Kaynaklar, pazarda, uç noktaları'ı seçerek grafikler ve graflar gösterilen verileri filtreleyebilirsiniz ve raporlama dönemi ilgilendiğiniz. Grafikler ve graflar uyguladığınız filtreler yansıtacak şekilde değiştirin. Aşağıda, değişebilir filtreleri açıklanmaktadır.

- **Kaynak Kimliği**: Azure aboneliğinizi tanımlayan benzersiz bir kaynak kimliği. Birden fazla Bing arama API'si katmana abone olursanız liste birden çok kimliği içerir. Tüm kaynaklar varsayılan olarak seçilir.  
  
- **Pazarlara**: Sonuçları nereden geldiğini marketlere. Örneğin, en-us (İngilizce, Amerika Birleşik Devletleri). Varsayılan olarak, tüm pazarlar seçilir. Bing arama bir pazar belirtmezse kullanan piyasadaki en WW markettir ve Bing kullanıcının Pazar belirlenemiyor unutmayın.  
  
- **Uç noktalar**: Bing arama API'si uç noktaları. Ücretli bir aboneliğe sahip tüm uç noktalar listesi içerir. Varsayılan olarak, tüm uç noktalar seçilir.  

- **Zaman çerçevesini**: Raporlama süresi. Belirtebilirsiniz:
  - Tüm&mdash;13 aylık değerinde veri için içerir ayarlama  
  - Son 24 saat&mdash;son 24 saat analytics'ten içerir  
  - Geçen hafta&mdash;son yedi gün analytics'ten içerir  
  - Geçen ay&mdash;son 30 gün analytics'ten içerir  
  - Özel bir tarih aralığı&mdash;varsa, belirtilen tarih aralığı analytics'ten içerir  

  > [!NOTE]  
  > Bu Panoda yüzey ölçümleri 24 saate kadar sürebilir. Pano, tarih ve saat verileri en son güncelleştirildiği gösterir.  

  > [!NOTE]  
  > Bing Statistics eklentisi etkinleştirdiğiniz andan ölçümler kullanılabilir.

## <a name="charts-and-graphs"></a>Grafikler ve graflar

Pano, seçilen uç nokta için mevcut olan ölçümler grafiklerini gösterir. Tüm ölçümler, tüm uç noktalar için kullanılabilir. Grafikler ve graflar her uç nokta için statik (grafikler ve graflar görüntülemek için seçtiğiniz değil). Panoyu yalnızca grafikler ile graflar için var olan verileri gösterir.

<!--
For example, if you don't include the User-Agent header in your calls, the dashboard will not include device-related graphs.
-->

Aşağıdaki olası ölçümleridir. Her ölçüm bitiş noktası kısıtlamaları notlar.

- **Toplu arama**: Raporlama döneminde yapılan çağrıların sayısını gösterir. Raporlama süresi bir gün için ise, grafik saatlik yapılan çağrıların sayısını gösterir. Aksi takdirde, grafik raporlama döneminde günde yapılan çağrıların sayısını gösterir.  
  
  > [!NOTE]
  > Çağrı hacmi, genellikle yalnızca başarılı çağrı içeren faturalama raporlardan farklı olabilir.

- **En çok yapılan sorgular**: Raporlama döneminde en sık kullanılan sorgular ve her bir sorguyu oluşum sayısını gösterir. Gösterilen sorguların sayısını yapılandırabilirsiniz. Örneğin, üst 25, 50 veya 75 sorguları gösterebilirsiniz. En sık kullanılan sorgular için aşağıdaki uç noktaların kullanılabilir değil:  

  - /images/trending
  - / Resimler/ayrıntıları
  - / Resimler/visualsearch
  - /Videos/trending
  - / Video/ayrıntıları
  - /News
  - /news/trendingtopics
  - /suggestions  
  
  > [!NOTE]  
  > Bazı sorgu terimleriyle bastırılabilir vb. gibi e-posta, telefon numaraları, SSN, gizli bilgileri kaldırmak için.

- **Coğrafi dağıtım**: Sonuçları nereden geldiğini marketlere. Örneğin, en-us (İngilizce, Amerika Birleşik Devletleri). Bing kullanan `mkt` belirtilen sorgu parametresi marketiyle ilgili belirlemek için. Aksi takdirde, Bing, Pazar belirlemek için sinyal arayan IP adresi gibi kullanır.

- **Yanıt kodu dağıtımı**: HTTP durum kodları raporlama dönemi boyunca tüm çağrıları.

- **Kaynak dağıtım çağrısı**: Kullanıcı tarafından kullanılan tarayıcılar türleri. Örneğin, Microsoft Edge, Chrome, Safari ve FireFox. Botlar, Postman ya da bir konsol uygulamasından curl kullanarak gibi bir tarayıcı dışında yapılan çağrılar kitaplıkları altında gruplandırılır. Kaynak, isteğin Kullanıcı aracısını üstbilgi değeri kullanılarak belirlenir. İsteğin kullanıcı aracısını üst bilgisi içermiyorsa, Bing, kaynağı diğer sinyaller türetilen dener.  

- **Güvenli arama dağıtım**: Güvenli arama değerler dağıtımı. Örneğin, Orta veya katı kapalı. `safeSearch` Sorgu parametresi değeri belirtilmişse içeriyor. Aksi takdirde, Bing, orta değer varsayılan olarak.  

- **Yanıtlar istenen dağıtım**: Web araması API'si, istenen yanıt verir `responseFilter` sorgu parametresi.  

- **Yanıtlar döndürülen dağıtım**: Web araması API'si yanıtta döndürülen yanıt.

- **Yanıt sunucu dağıtımı**: API isteklerinizin sunulan uygulama sunucusu. Olası değerler şunlardır: (için trafiği masaüstü ve dizüstü cihazlardan sunulan) Bing.com ve Bing.com mobil (için sunulan mobil cihazlardan gelen trafik). Sunucu isteğin Kullanıcı aracısını üstbilgi değeri kullanılarak belirlenir. İsteğin kullanıcı aracısını üst bilgisi içermiyorsa, Bing, sunucunun diğer sinyaller türetilen dener.

Her uç nokta için kullanılabilir olan analytics gösterir.

![Uç nokta destek matrisi göre dağılım](./media/cognitive-services-bing-statistics/bing-statistics-matrix.PNG)
