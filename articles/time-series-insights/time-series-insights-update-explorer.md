---
title: Azure zaman serisi öngörüleri önizlemesi gezginde verileri Görselleştirme | Microsoft Docs
description: Bu makalede, özellikleri ve Azure zaman serisi öngörüleri önizlemesi explorer web App'te kullanılabilir seçenekleri açıklar.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: d348592589f448dab9b8b4f3a1a3eb286d464417
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64723457"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Önizleme gezginde verileri Görselleştirme

Bu makalede özellikleri ve seçenekleri Azure zaman serisi öngörüleri Önizleme aşamasında kullanıma [explorer web uygulaması](https://insights.timeseries.azure.com/preview/samples).

## <a name="prerequisites"></a>Önkoşullar

Azure zaman serisi öngörüleri önizlemesi Gezgini kullanmadan önce şunları yapmalısınız:

* Ayarlanmış bir zaman serisi görüşleri ortamına sahip. Daha fazla bilgi için [Öğreticisi: Azure Time Series Insights Önizleme](./time-series-insights-update-create-environment.md).
* Hesap, oluşturduğunuz zaman serisi görüşleri ortamına veri erişimi sağlar. Diğerleri de kendiniz için farklı erişim sağlayabilir.
* Olay kaynağı, zaman serisi görüşleri ortamına veri göndermek için bir ortam ekleyin.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Azure zaman serisi öngörüleri önizlemesi gezginini öğrenin

  ![bir arada Gezgini][1]

Azure zaman serisi öngörüleri önizlemesi Gezgini aşağıdaki öğelerden oluşur:

* **Gezinti çubuğu**: Analiz ve model sayfalar arasında geçiş sağlar.
* **Hiyerarşi ağacı**: Grafiği oluşturulacak belirli veri öğeleri seçmenize olanak sağlar.
* **Serisi iyi zaman**: Seçili olan veri öğeleri görüntüler.
* **Grafik paneli**: Geçerli çalışma haritanız görüntüler.
* **Zaman Çizelgesi**: Çalışma süre değiştirmenize olanak tanır.
* **Uygulama çubuğunu**: Geçerli kiracıya gibi kullanıcı yönetim seçeneklerini içerir ve tema ve dil ayarlarını değiştirmenize olanak sağlar.

## <a name="time-series-insights-preview-environment-panel"></a>Zaman serisi öngörüleri Önizleme ortamı paneli

Ortam paneline erişiminiz olan tüm zaman serisi görüşleri ortamları görüntüler. Bu liste, Kullandıkça Öde ortamları Önizleme ve S1/S2 ortamları (GA) içerir. Kullanmak istediğiniz zaman serisi görüşleri ortamına tıklamanız yeterlidir.

  ![iki Gezgini][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Zaman serisi öngörüleri Önizleme Gezinti Menüsü

  ![üç Gezgini][3]

Gezinti Menüsü ile zaman serisi görüşleri uygulamalar arasında geçiş yapabilirsiniz:

* **Analiz**: Grafik ve zengin analizler Modellenen veya modellenmemiş zaman serisi verileriniz üzerinde gerçekleştirmek olanak sağlar.

* **Model**: Time Series Insights modelinize yeni zaman serisi öngörüleri Önizleme türleri, hiyerarşileri ve örnekleri anında olanak sağlar.

## <a name="time-series-insights-preview-model-authoring"></a>Zaman serisi öngörüleri Önizleme model yazma

Bu uygulama sayesinde, zaman serisi modeli oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri gerçekleştirebilirsiniz.  

* **Zaman serisi modeli türü**: Zaman serisi görüşleri türlerini tanımlayan değişkenleri veya hesaplamalar yapan formüller etkinleştirin. Belirli bir zaman serisi görüşleri örneğiyle ilişkili oldukları. Bir türü veya daha fazla değişken olabilir.
* **Zaman serisi modeli hiyerarşisi**: Hiyerarşileri, verilerinizin sistematik kuruluşlardır. Time Series Insights veri farklı varlıklar arasındaki ilişkileri hiyerarşileri kullanılırlar.
* **Zaman serisi modeli örneği**: Zaman serisi örneklerdir. Çoğu durumda, cihaz kimliği veya varlık ortamda benzersiz tanımlayıcısı olan AssetID değildirler.

Zaman serisi modeli hakkında daha fazla bilgi için bkz: [kez serisi modelleri](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Zaman serisi öngörüleri Önizleme modeli arama paneli

Model arama paneli kolayca arama yapın ve grafiğinizde görüntülemek istediğiniz belirli bir zaman serisi örnekleri bulmak için zaman serisi modeli hiyerarşisinde gezinme sağlar. Örneklerinizin seçtiğinizde, geçerli grafik ve veriler için de eklenir.

  ![dört Gezgini][4]

## <a name="time-series-insights-preview-well"></a>Zaman serisi öngörüleri Önizleme iyi

Örnek alanları ve seçilen zaman serisi örnekleri ile ilişkili diğer meta verileri de görüntüler. Sağ tarafındaki onay kutularını, gizleme veya görüntüleme geçerli grafikten belirli örnekler sağlar. Ayrıca, belirli veri öğelerini de kırmızı x denetim öğesi sağındaki tıklayarak geçerli verilerinizden kaldırabilirsiniz.

  ![beş Gezgini][5]

Öğelerin daha iyi dikey görünüm de verilerinizi almak için telemetri paneli açılan.

  ![altı Gezgini][6]

> [!NOTE]
> Örneği aşağıdaki iletiyi görürseniz, seçili zaman aralığı sırasında herhangi bir veri yok. Sorunu çözmek için zaman aralığını artırın veya örnek veri gönderme olduğunu onaylayın.
>
> ![yedi Gezgini][7]

## <a name="time-series-insights-preview-chart"></a>Zaman serisi öngörüleri Önizleme grafiği

Grafik ile zaman serisi örnekleri çizgilerle görüntüleyebilirsiniz. Grafik büyütmek için web denetimleri tıklayarak ortam paneli, veri modeli ve zaman aralığı Denetim Masası daraltabilirsiniz.

  ![sekiz Gezgini][8]

1. **Seçili tarih aralığı**: Denetimler, hangi veri öğeleri için görselleştirme bulunur.

1. **İç tarih aralığı kaydırıcı aracını**: İstenen zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın.

1. **Zaman aralığı Daralt denetim**: Daraltır ve zaman aralığı paneli Düzenleyicisi genişletir.

1. **Y ekseni biçim denetimi**: Kullanılabilir y ekseni görüntüleme seçenekleri arasında geçiş yapar:

    * `Default`: Her satırda tek bir y ekseni bulunmaktadır.
    * `Stacked`: Y ekseni seçilen satırdaki göre değişen verilerle aynı y ekseni birden çok satırda yığın olanak sağlar.
    * `Shared`: Tüm y ekseni veri birlikte görüntülenir.

1. **Geçerli bir veri öğesi**: Şu anda seçili veri öğesi ve onun ilişkili ayrıntıları.

Daha fazla geçerli grafik veri noktasında Sol tıklatma ve sonra seçilen alan dilediğiniz bitiş noktasına sürükleyerek bir özel veri dilimi detaya gidebilirsiniz. Renkte, seçilen alana sağ tıklayın ve yakınlaştırma bu aşağıdaki görüntüde gösterildiği gibi tıklayın:

  ![dokuz Gezgini][9]

Yakınlaştırma eylemi gerçekleştirdikten sonra seçilen veri kümesi görürsünüz. Y ekseni biçim denetimi, zaman serisi öngörüleri verilerini üç y ekseni temsilleri arasında geçiş yapmak için tıklatın.

  ![on Gezgini][10]

Burada, paylaşılan y eksenleri örneğini görebilirsiniz:

  ![on Gezgini][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Zaman serisi öngörüleri Önizleme zaman Düzenleyicisi paneli

Zaman serisi öngörüleri Önizleme ile çalışırken, ilk olarak bir zaman aralığı seçin. Seçilen zaman aralığını, zaman serisi öngörüleri Önizleme pencere öğeleri ile işleme için kullanılabilir veri kümesi denetler. Aşağıdaki web denetimleri, çalışma zaman aralığını seçmek için zaman serisi öngörüleri önizlemede kullanılabilir.

  ![on Gezgini][12]

1. **İç tarih aralığı kaydırıcı aracını**: İstenen zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın. Bu iç tarih aralığı dış tarih aralığı kaydırıcı denetimi tarafından sınırlanır.

1. **Artırın ve azaltın tarih aralığı düğmeleri**: Artırın veya istediğiniz aralığını ya da düğmesini seçerek, zaman aralığını azaltın.

1. **Zaman aralığı Daralt denetim**: Bu web denetimi iç tarih aralığı kaydırıcı aracını hariç tüm denetimleri gizleme olanak tanır.

1. **Dış-tarih aralığı kaydırıcı denetimi**: Uç nokta denetimleri, iç tarih aralığı denetiminiz için kullanılabilir olacak dış tarih aralığı seçmek için kullanın.

1. **Hızlı süreler tarih aralığı açılan**: Son 30 dakika, son 12 saat ya da özel bir aralık gibi belirlenen zaman aralığı seçimleri arasında hızlıca geçiş sağlar. Bu değeri değiştirmeniz aralık boyutu kaydırıcı aracını tartışılan kullanılabilir aralığı aralıkları değiştirir.

1. **Aralık boyutu kaydırıcı aracını**: Aynı zaman aralığı içine ve dışına aralıkları yakınlaştırma olanak sağlar. Bu eylem büyük saat dilimleri arasında hareket daha kesin bir denetim sağlar. Ayrıntılı, yüksek çözünürlüklü keser verilerinizi görmenize olanak sağlayan bir milisaniye küçük dilimlerin aşağı kesintisiz eğilimleri gösterir. Kaydırıcının varsayılan başlangıç noktası çözümleme, sorgu hızı ve ayrıntı düzeyi dengeleyen seçimden, verilerin en iyi görünüm olarak ayarlanır.

1. **Tarih aralığı için ve web denetiminden**: Bu web denetimi ile kolayca tıklayın ve istenen tarih ve saat aralığı seçin. Ayrıca, farklı saat dilimleri arasında geçiş yapmak için denetimi de kullanabilirsiniz. Geçerli çalışma alanınıza, uygulanacak değişiklikler yaptıktan sonra seçin **Kaydet**.

   ![Explorer On üç][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Zaman serisi öngörüleri Önizleme Gezinti Bölmesi

Zaman serisi öngörüleri Önizleme Gezinti bölmesine, aşağıdaki işlevleri sağlar:

  ![on dört Gezgini][14]

### <a name="current-session-share-link-control"></a>Geçerli oturumdaki paylaşımını bağlantı denetimi

  ![beş Gezgini][15]

İçeren kaydetmeyi ya da, geçerli Azure Time Series Insights çalışma oturumu paylaşan bir URL'yi oluşturmak için (vurgulu) bağlantı web denetimi seçin:

* Seçili zaman aralığı
* Şu anda seçili aralık boyutu
* Veriler şu anda seçili

### <a name="tenant-section"></a>Kiracı bölümü

  ![Explorer on altı][16]

* Geçerli zaman serisi görüşleri oturum açma hesap bilgilerini görüntüler.
* Kullanılabilir Time Series Insights temalar arasında geçiş sağlar.

### <a name="theme-selection"></a>Tema seçimi

Azure zaman serisi öngörüleri Önizleme iki tema destekler:

* **Açık tema**: Bu belgede gösterilen varsayılan tema.
* **Koyu tema**:  Gezgin, burada gösterildiği şekilde işlenir:

  ![Explorer on yedi][17]

Burada arasında desteklenen dilleri değiştirebilirsiniz.

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="time-series-insights-preview-terms-panel"></a>Zaman serisi öngörüleri Önizleme terimler paneli

Bu bölüm yalnızca güncelleştirilmiş kullanıcı Arabiriminde Gezgin kullanma girişimi mevcut S1/S2 ortamlar için geçerlidir. GA ürün ve önizleme birlikte kullanmak isteyebilirsiniz. Bazı işlevler mevcut kullanıcı Arabiriminden güncelleştirilmiş Gezgini'ne ekledik ancak mevcut zaman serisi görüşleri Gezgin ortamında S1/S2 için tam kullanıcı Arabirimi deneyimi elde edebilirsiniz.  

Hiyerarşinin yerine, sorguları ortamınızda tanımladığınız zaman serisi görüşleri terimler paneli görürsünüz. Verilerinizi bir koşulu üzerinde filtre olanak tanır.

  ![Explorer On sekiz][18]

Zaman serisi öngörüleri Önizleme Koşulları Düzenleyicisi paneli aşağıdaki parametreleri alır:

**Burada**: Where yan tümcesi sayesinde hızlı bir şekilde işlenen kümesini kullanarak olaylarınızı aşağıdaki tabloda listelenen filtre. Bir işlenen seçerek bir arama yapın, o aramaya bağlı koşul otomatik olarak güncelleştirilir. Desteklenen işlenen türleri şunlardır:

| İşlem | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Çift, DateTime, zaman aralığı | |
| `=`, `!=`, `<>` | Dize, Bool, Double, DateTime, zaman aralığı, NULL |
| `IN` | Dize, Bool, Double, DateTime, zaman aralığı, NULL | Tüm işlenenler aynı türde veya NULL sabiti olması. |
| `HAS` | Dize | Yalnızca sabit dize değişmez değerleri, sağ tarafında izin verilir. Boş dize ve NULL yapılamaz. |

Desteklenen sorgu işlemleri ve veri türleri hakkında daha fazla bilgi edinmek [zaman serisi ifade (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Örnekleri Where yan tümceleri

  ![on dokuz Gezgini][19]

**Ölçü**: Bu açılan tüm sayısal sütunlarını görüntüler (**double**), geçerli grafiğiniz için öğeleri olarak kullanabilirsiniz.

**Bölme ölçütü**: Bu açılır menü, modelinizdeki verilerinizi göre gruplandırabilirsiniz tüm kullanılabilir kategorik sütunlar (dize) görüntüler. Aynı x eksenine görüntülemek için en fazla beş koşullarını ekleyebilirsiniz. İstenen parametrelerinizi girin ve ardından **Ekle** yeni bir terim eklemek için.

  ![yirmi Gezgini][20]

Gösterebilir ve aşağıdaki görüntüde gösterildiği gibi görünür simgesini seçerek grafiği panel öğeleri gizleyebilirsiniz. Kırmızı tıklayarak sorguları tamamen kaldırabilirsiniz **x**.

  ![Explorer yirmi bir][21]

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) Azure zaman serisi öngörüleri önizlemede.

- Zaman serisi öngörüleri Önizleme belgeyi okumaya [veri modelleme](./time-series-insights-update-tsm.md).

- Bilgi [tanılayın ve sorunlarını gidermeyi](./time-series-insights-update-how-to-troubleshoot.md) Time Series Insights örneğinizin.

<!-- Images -->
[1]: media/v2-update-explorer/explorer-one.png
[2]: media/v2-update-explorer/explorer-two.png
[3]: media/v2-update-explorer/explorer-three.png
[4]: media/v2-update-explorer/explorer-four.png
[5]: media/v2-update-explorer/explorer-five.png
[6]: media/v2-update-explorer/explorer-six.png
[7]: media/v2-update-explorer/explorer-seven.png
[8]: media/v2-update-explorer/explorer-eight.png
[9]: media/v2-update-explorer/explorer-nine.png
[10]: media/v2-update-explorer/explorer-ten.png
[11]: media/v2-update-explorer/explorer-eleven.png
[12]: media/v2-update-explorer/explorer-twelve.png
[13]: media/v2-update-explorer/explorer-thirteen.png
[14]: media/v2-update-explorer/explorer-fourteen.png
[15]: media/v2-update-explorer/explorer-fifteen.png
[16]: media/v2-update-explorer/explorer-sixteen.png
[17]: media/v2-update-explorer/explorer-seventeen.png
[18]: media/v2-update-explorer/explorer-eighteen.png
[19]: media/v2-update-explorer/explorer-nineteen.png
[20]: media/v2-update-explorer/explorer-twenty.png
[21]: media/v2-update-explorer/explorer-twenty-one.png
