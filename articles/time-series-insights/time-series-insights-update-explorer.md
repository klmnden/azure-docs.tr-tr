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
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: 862465a7611f1a2bc65dbb0c49c4de512bd239de
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442093"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Önizleme gezginde verileri Görselleştirme

Bu belgede Azure zaman serisi öngörüleri Önizleme arabirimi ve UI/UX özellikleri açıklanmaktadır [demo web uygulaması](https://insights.timeseries.azure.com/preview/demo). Özellikle, barındırılan bir örnek, arabirimi özelleştirme seçenekleri ve sağlanan tanıtım gezinme düzenini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Azure zaman serisi öngörüleri önizlemesi Gezgini ile çalışmaya başlamak için yapmanız gerekir:

* Ayarlanmış bir zaman serisi görüşleri ortamına sahip. Deneyin örneğini sağlama hakkında daha fazla bilgi edinmek için sunduğumuz [Azure zaman serisi öngörüleri önizlemesi](./time-series-insights-update-create-environment.md) öğretici.
* [Veri erişimi sağlamasına](./time-series-insights-data-access.md) hesabı oluşturduğunuz zaman serisi görüşleri ortamına. Diğerleri de kendiniz için farklı erişim sağlayabilir.
* Olay kaynağı, zaman serisi görüşleri ortamına veri göndermek için bir ortam ekleyin:
  * Bilgi [olay hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-eventhub.md)
  * Bilgi [bir IOT hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-iothub.md)

## <a name="learn-about-the-preview-explorer"></a>Önizleme gezginini öğrenin

Azure zaman serisi öngörüleri önizlemesi Gezgini aşağıdaki öğelerden oluşur:

[![Gezgin Görünümü](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

1. <a href="#environment-dropdown">**Ortam paneline**</a>: Azure TSI ortamlarınızda görüntüler.
1. <a href="#navigation-menu">**Gezinti Menüsü**</a>: Arasında geçiş sağlar **Çözümle** ve **modeli** sayfaları.
1. <a href="#hierarchy-tree">**Hiyerarşi ağacı**</a>: Belirli bir model ve grafiği için veri öğeleri seçmenize olanak tanır.
1. <a href="#preview-well">**Serisi iyi zaman**</a>: Renk kodlaması ile tablo biçiminde, seçili olan veri öğeleri görüntüler.
1. <a href="#preview-chart">**Grafik paneli**</a>:  Geçerli çalışma haritanız görüntüler.
1. <a href="#time-editor-panel">**Zaman Çizelgesi**</a>:  Çalışma süre değiştirmenize olanak tanır.
1. <a href="#navigation-panel">**Uygulama çubuğunu**</a>:  Geçerli kiracıya gibi kullanıcı yönetim seçeneklerini içerir ve tema ve dil ayarlarını değiştirmenize olanak sağlar.

## <a name="environment-dropdown"></a>Ortam açılan kutusu

Ortam açılan menüsünün erişiminiz olan tüm zaman serisi görüşleri ortamları görüntüler. Bu liste, Kullandıkça Öde ortamları (Önizleme) ve S1/S2 ortamları (genel kullanılabilirlik veya GA) içerir. 

1. Yalnızca görüntülenen ortamınızı yanındaki açılan oka tıklayın:

   [![Denetim Masası](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Ardından, istediğiniz ortamı seçin.

## <a name="navigation-menu"></a>Gezinti menüsü

  [![Gezinti Menüsü](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Gezinti menüsünde iki görünüm arasında seçmenize olanak sağlar:

* **Analiz**: Grafik ve zengin analizler Modellenen veya modellenmemiş zaman serisi verileriniz üzerinde gerçekleştirmek olanak sağlar.
* **Model**: Time Series Insights modelinize yeni zaman serisi öngörüleri Önizleme türleri, hiyerarşileri ve örnekleri anında olanak sağlar.

## <a name="hierarchy-tree"></a>Hiyerarşi ağacı

Hiyerarşi ağacı modelleri, belirli cihazlar ve algılayıcılar cihazlarınızda dahil olmak üzere seçili veri öğelerini görüntüler.

### <a name="model-search-panel"></a>Arama panelini modeli

Model arama paneli kolayca arama yapın ve grafiğinizde görüntülemek istediğiniz belirli bir zaman serisi örnekleri bulmak için zaman serisi modeli hiyerarşisinde gezinme sağlar. Örneklerinizin seçtiğinizde, geçerli grafik ve veriler için de eklenir.

  [![Model arama paneli](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Model geliştirme

Azure zaman serisi öngörüleri önizlemesi, zaman serisi modeli tam oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri destekler.  

* **Zaman serisi modeli türü**: Zaman serisi görüşleri türlerini tanımlayan değişkenleri veya hesaplamalar yapan formüller etkinleştirin. Belirli bir zaman serisi görüşleri örneğiyle ilişkili oldukları. Bir türü veya daha fazla değişken olabilir.
* **Zaman serisi modeli hiyerarşisi**: Hiyerarşileri, verilerinizin sistematik kuruluşlardır. Time Series Insights veri farklı varlıklar arasındaki ilişkileri hiyerarşileri kullanılırlar.
* **Zaman serisi modeli örneği**: Zaman serisi örneklerdir. Çoğu durumda olduklarını **DeviceID** veya **AssetID**, olduğu varlık ortamda benzersiz tanımlayıcısı.

Zaman serisi modeli hakkında daha fazla bilgi için bkz: [kez serisi modelleri](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>İyi önizleme

Örnek alanları ve seçili TSI örnekleriyle ilişkili diğer meta verileri de görüntüler. Sağ taraftaki onay kutularını gizleme veya görüntüleme geçerli grafikten belirli örnekler sağlar. Kırmızı iyi tıklayarak geçerli verilerinizden belirli veri öğelerini kaldırabilir **Sil** (Çöp Kutusu) öğenin sol tarafındaki denetim.

  [![İyi önizleme](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Ayrıca düzenini yeniden yapılandırabilirsiniz, **Çözümle** üç nokta simgesini sağ üst köşesinde grafik sayfası:

  [![Telemetri Düzen Seçenekleri](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Örneği aşağıdaki iletiyi görürseniz, seçili zaman aralığı sırasında herhangi bir veri yok. Sorunu çözmek için zaman aralığını artırın veya örnek veri gönderme olduğunu onaylayın.
>
> ![Hiçbir veri bildirimi](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Grafik önizlemesi

Grafikle TSI örnekleri çizgilerle görüntüleyebilirsiniz. Grafik büyütmek için web denetimleri tıklayarak ortam paneli, veri modeli ve zaman aralığı Denetim Masası daraltabilirsiniz.

  [![Önizleme grafik genel bakış](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

1. **Seçili tarih aralığı**: Denetimler, hangi veri öğeleri için görselleştirme bulunur.

1. **İç tarih aralığı kaydırıcı aracını**: İstenen zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın.

1. **Zaman aralığı Daralt denetim**: Daraltır ve zaman aralığı paneli Düzenleyicisi genişletir.

1. **Y ekseni biçim denetimi**: Kullanılabilir y ekseni görüntüleme seçenekleri arasında geçiş yapar:

    * `Default`: Her satırda tek bir y ekseni bulunmaktadır.
    * `Stacked`: Y ekseni seçilen satırdaki göre değişen verilerle aynı y ekseni birden çok satırda yığın olanak sağlar.
    * `Shared`: Tüm y ekseni veri birlikte görüntülenir.

1. **Geçerli bir veri öğesi**: Şu anda seçili veri öğesi ve onun ilişkili ayrıntıları.

Daha fazla geçerli grafik veri noktasında Sol tıklatma ve sonra seçilen alan dilediğiniz bitiş noktasına sürükleyerek bir özel veri dilimi detaya gidebilirsiniz. Renkte, seçili alanı sağ tıklatın ve **yakınlaştırma** bu aşağıdaki görüntüde gösterildiği gibi:

  [![Önizleme grafik yakınlaştırma](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Siz gerçekleştirdikten sonra **yakınlaştırma** eylem, seçilen veri kümesi görürsünüz. Y ekseni biçim denetimi, zaman serisi öngörüleri verilerini üç y ekseni temsilleri arasında geçiş yapmak için tıklatın.

  [![Grafik y ekseni önizlemesi](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Burada, paylaşılan y eksenleri örneğini görebilirsiniz:

  [![Y ekseni paylaşımlı Önizleme](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Zaman Düzenleyicisi paneli

Zaman serisi öngörüleri Önizleme ile çalışırken, ilk olarak bir zaman aralığı seçin. Seçilen zaman aralığını, zaman serisi öngörüleri Önizleme pencere öğeleri ile işleme için kullanılabilir veri kümesi denetler. Aşağıdaki web denetimleri, çalışma zaman aralığını seçmek için zaman serisi öngörüleri önizlemede kullanılabilir.

  [![Zaman seçimi paneli](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **İç tarih aralığı kaydırıcı aracını**: İstenen zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın. Bu iç tarih aralığı dış tarih aralığı kaydırıcı denetimi tarafından sınırlanır.

1. **Artırın ve azaltın tarih aralığı düğmeleri**: Artırın veya istediğiniz aralığını ya da düğmesini seçerek, zaman aralığını azaltın.

1. **Zaman aralığı Daralt denetim**: Bu web denetimi iç tarih aralığı kaydırıcı aracını hariç tüm denetimleri gizleme olanak tanır.

1. **Dış-tarih aralığı kaydırıcı denetimi**: Uç nokta denetimleri, iç tarih aralığı denetiminiz için kullanılabilir olacak dış tarih aralığı seçmek için kullanın.

1. **Hızlı süreler tarih aralığı açılan**: Hızlı geçiş son gibi belirlenen zaman aralığı seçimleri arasında sağlar **30 dakika**, **son 12 saat**, veya bir **özel aralık**. Bu değeri değiştirmeniz aralık boyutu kaydırıcı aracını tartışılan kullanılabilir aralığı aralıkları değiştirir.

1. **Aralık boyutu kaydırıcı aracını**: Aynı zaman aralığı içine ve dışına aralıkları yakınlaştırma olanak sağlar. Bu eylem büyük saat dilimleri arasında hareket daha kesin bir denetim sağlar. Ayrıntılı, yüksek çözünürlüklü keser verilerinizi görmenize olanak sağlayan bir milisaniye küçük dilimlerin aşağı kesintisiz eğilimleri gösterir. Kaydırıcının varsayılan başlangıç noktası çözümleme, sorgu hızı ve ayrıntı düzeyi dengeleyen seçimden, verilerin en iyi görünüm olarak ayarlanır.

1. **Tarih aralığı için ve web denetiminden**: Bu web denetimi ile kolayca tıklayın ve istenen tarih ve saat aralığı seçin. Ayrıca, farklı saat dilimleri arasında geçiş yapmak için denetimi de kullanabilirsiniz. Geçerli çalışma alanınıza, uygulanacak değişiklikler yaptıktan sonra seçin **Kaydet**.

   [![Seçim paneli gelen ve giden](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Gezinti Bölmesi

Zaman serisi öngörüleri Önizleme Gezinti panelini TSI uygulamanızı üst kısmında görünür. Bu, aşağıdaki işlevleri sağlar.

### <a name="current-session-share-link-control"></a>Geçerli oturumdaki paylaşımını bağlantı denetimi

  [![Paylaş simgesi](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Yeni **paylaşmak** simgesini bir URL bağlantısı ekibinizle paylaşın.

  [![Örneği URL'nizi paylaşın](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Kiracı bölümü

  [![Kiracı seçimi](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Geçerli zaman serisi görüşleri oturum açma hesap bilgilerini görüntüler.
* Kullanılabilir Time Series Insights temalar arasında geçiş sağlar.
* Önizleme görüntülemenizi sağlar [demo web uygulaması](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Tema seçimi

Yeni bir tema seçmek için sağ üst köşesinde bulunan, profil simgesine tıklayın. Ardından, **temayı Değiştir**.

  [![Tema seçimi](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Dil Seçimi Ayrıca, profil simgesine tıklayarak kullanılabilir.

Azure zaman serisi öngörüleri Önizleme iki tema destekler:

* **Açık tema**: Bu belgede gösterilen varsayılan tema.
* **Koyu tema**:  Gezgin, burada gösterildiği şekilde işlenir:

  [![Seçili koyu tema](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="preview-terms-panel"></a>Önizleme koşulları bölmesi

Bu bölüm yalnızca güncelleştirilmiş kullanıcı Arabiriminde Gezgin kullanma girişimi mevcut S1/S2 ortamlar için geçerlidir. GA ürün ve önizleme birlikte kullanmak isteyebilirsiniz. Bazı işlevler mevcut kullanıcı Arabiriminden güncelleştirilmiş Gezgini'ne ekledik ancak mevcut zaman serisi görüşleri Gezgin ortamında S1/S2 için tam kullanıcı Arabirimi deneyimi elde edebilirsiniz.  

Hiyerarşinin yerine, sorguları ortamınızda tanımladığınız zaman serisi görüşleri terimler paneli görürsünüz. Verilerinizi bir koşulu üzerinde filtre olanak tanır.

  [![Burada paneli sorgulama](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

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

  [![Burada tümcesi örnekleri](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Ölçü**: Tüm sayısal sütunları görüntüleyen açılır (**double**) için geçerli grafik öğeleri olarak kullanabilir.

**Bölme ölçütü**: Bu açılır menü, modelinizdeki verilerinizi göre gruplandırabilirsiniz tüm kullanılabilir kategorik sütunlar (dize) görüntüler. Aynı x eksenine görüntülemek için en fazla beş koşullarını ekleyebilirsiniz. İstenen parametrelerinizi girin ve ardından **Ekle** yeni bir terim eklemek için.

  [![Sorgulanan ve filtrelenmiş görünümü bir](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Gösterebilir ve aşağıdaki görüntüde gösterildiği gibi görünür simgesini seçerek grafiği panel öğeleri gizleyebilirsiniz. Kırmızı tıklayarak sorguları tamamen kaldırabilirsiniz **X**.

  [![Sorgulanan ve filtrelenmiş görünüm iki](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) Azure zaman serisi öngörüleri önizlemede.

- Zaman serisi öngörüleri Önizleme belgeyi okumaya [veri modelleme](./time-series-insights-update-tsm.md).

- Bilgi [tanılayın ve sorunlarını gidermeyi](./time-series-insights-update-how-to-troubleshoot.md) Time Series Insights örneğinizin.
