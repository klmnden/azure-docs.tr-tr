---
title: Azure zaman serisi öngörüleri önizlemesi gezginde verileri Görselleştirme | Microsoft Docs
description: Bu makalede, özellikleri ve Azure zaman serisi öngörüleri önizlemesi explorer web App'te kullanılabilir seçenekleri açıklar.
author: ashannon7
ms.author: dpalled
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 05/03/2019
ms.custom: seodec18
ms.openlocfilehash: c4f3053063ce33d2777387da2c53effd61b05f1a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66399871"
---
# <a name="visualize-data-in-the-explorer-preview"></a>Önizleme gezginde verileri Görselleştirme

Bu belgede kullanıcı arabirimi ve kullanıcı deneyimi özellikleri ve Azure zaman serisi öngörüleri Önizleme arabiriminin açıklanmaktadır [demo web uygulaması](https://insights.timeseries.azure.com/preview/demo). Özellikle, barındırılan bir örnek, arabirimi özelleştirme seçenekleri ve sağlanan tanıtım gezinme düzenini açıklar.

## <a name="prerequisites"></a>Önkoşullar

Azure zaman serisi öngörüleri önizlemesi Gezgini ile çalışmaya başlamak için yapmanız gerekir:

* Ayarlanmış bir zaman serisi görüşleri ortamına sahip. Bir örneğini sağlama hakkında daha fazla bilgi edinmek için deneyin [Azure zaman serisi öngörüleri önizlemesi](./time-series-insights-update-create-environment.md) öğretici.
* [Veri erişimi sağlamasına](./time-series-insights-data-access.md) hesabı oluşturduğunuz zaman serisi görüşleri ortamına. Diğerleri de kendiniz için farklı erişim sağlayabilir.
* Olay kaynağı, zaman serisi görüşleri ortamına veri göndermek için bir ortam ekleyin:
  * Bilgi [olay hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-eventhub.md).
  * Bilgi [bir IOT hub'ına bağlanma](./time-series-insights-how-to-add-an-event-source-iothub.md).

## <a name="learn-about-the-preview-explorer"></a>Önizleme gezginini öğrenin

Azure zaman serisi öngörüleri önizlemesi Gezgini aşağıdaki öğelerden oluşur:

[![Gezgin Görünümü](media/v2-update-explorer/explorer-one.png)](media/v2-update-explorer/explorer-one.png#lightbox)

- <a href="#environment-drop-down-list">Ortam paneline</a>: Azure Time Series Insights ortamınızı görüntüler.
- <a href="#navigation-menu">Gezinti Menüsü</a>: Arasında geçiş yapmak için kullanın **Çözümle** ve **modeli** sayfaları.
- <a href="#hierarchy-tree">Hiyerarşi ağacı</a>: Grafiği oluşturulacak belirli modeli ve veri öğeleri seçmek için kullanın.
- <a href="#preview-well">Serisi iyi zaman</a>: Renk kodlaması ile tablo biçiminde, seçili olan veri öğeleri görüntüler.
- <a href="#preview-chart">Grafik paneli</a>: Geçerli çalışma haritanız görüntüler.
- <a href="#time-editor-panel">Zaman Çizelgesi</a>: Çalışma zaman aralığını değiştirmek için kullanın.
- <a href="#navigation-panel">Uygulama çubuğunu</a>: Geçerli kiracıya gibi kullanıcı yönetim seçeneklerini içerir. Tema ve dil ayarlarını değiştirmek için kullanabilirsiniz.

## <a name="environment-drop-down-list"></a>Ortam açılan listesi

Erişiminiz olan tüm zaman serisi görüşleri ortamları ortam açılan liste görüntüler. Bu liste, zaman serisi öngörüleri Önizleme gibi Kullandıkça Öde ortamlarını içerir. Bu liste, S1/S2 ortamları, genel kullanıma sunulmuştur da içerir.

1. Görüntülenen ortamınızı yanındaki aşağı açılan oku seçin.

   [![Denetim Masası](media/v2-update-explorer/explorer-two.png)](media/v2-update-explorer/explorer-two.png#lightbox)

1. Ardından istediğiniz ortamı seçin.

## <a name="navigation-menu"></a>Gezinti menüsü

  [![Gezinti Menüsü](media/v2-update-explorer/explorer-three.png)](media/v2-update-explorer/explorer-three.png#lightbox)

Gezinti menüsünde, iki görünüm arasında seçmek için kullanın:

* **Analiz**: Grafik ve zengin analizler Modellenen veya modellenmemiş zaman serisi verileriniz üzerinde gerçekleştirmek için kullanın.
* **Model**: Time Series Insights modelinize yeni zaman serisi öngörüleri Önizleme türleri, hiyerarşileri ve örnekleri göndermek için bunu kullanın.

## <a name="hierarchy-tree"></a>Hiyerarşi ağacı

Hiyerarşi ağacı modelleri, belirli cihazlar ve algılayıcılar cihazlarınızda seçili veri öğelerini görüntüler.

### <a name="model-search-panel"></a>Arama panelini modeli

Kolayca arama yapın ve grafiğinizde görüntülemek istediğiniz belirli bir zaman serisi örnekleri bulmak için zaman serisi modeli hiyerarşisinde gezinme modeli arama panelini kullanabilirsiniz. Örneklerinizin seçtikten sonra bunlar geçerli grafik ve veriler için de eklenir.

  [![Model arama paneli](media/v2-update-explorer/explorer-four.png)](media/v2-update-explorer/explorer-four.png#lightbox)

### <a name="model-authoring"></a>Model geliştirme

Tam Azure zaman serisi öngörüleri önizlemesi destekler oluşturma, okuma, güncelleştirme ve silme (CRUD) işlemleri, zaman serisi modeli.

* **Zaman serisi modeli türü**: Time Series Insights türleri hesaplamaları yapmak için formülleri veya değişkenleri tanımlamak için kullanabilirsiniz. Bunlar, belirli bir zaman serisi görüşleri örneği ile ilişkili. Bir türü veya daha fazla değişken olabilir.
* **Zaman serisi modeli hiyerarşisi**: Hiyerarşileri, verilerinizin sistematik kuruluşlardır. Time Series Insights veri farklı varlıklar arasındaki ilişkileri hiyerarşileri kullanılırlar.
* **Zaman serisi modeli örneği**: Zaman serisi örneklerdir. Çoğu durumda olduklarını **DeviceID** veya **AssetID**, olduğu varlık ortamda benzersiz tanımlayıcısı.

Zaman serisi modeli hakkında daha fazla bilgi için bkz: [kez serisi modelleri](./time-series-insights-update-tsm.md).

## <a name="preview-well"></a>İyi önizleme

Örnek alanları ve seçili Time Series Insights örnekleriyle ilişkili diğer meta verileri de görüntüler. Sağ taraftaki onay kutularını seçerek gizleyebilir veya geçerli grafikten belirli örneklerini görüntülemek. Kırmızı iyi seçerek geçerli verilerinizden belirli veri öğelerini kaldırabilirsiniz **Sil** (Çöp Kutusu) öğenin sol tarafındaki denetim.

  [![İyi önizleme](media/v2-update-explorer/explorer-five.png)](media/v2-update-explorer/explorer-five.png#lightbox)

Düzenini yapılandırmak için **Çözümle** grafik sayfası, sağ üst köşedeki üç nokta simgesini seçin:

  [![Telemetri Düzen Seçenekleri](media/v2-update-explorer/explorer-six.png)](media/v2-update-explorer/explorer-six.png#lightbox)

> [!NOTE]
> Aşağıdaki iletiyi görürseniz, örneği, seçili zaman aralığı sırasında herhangi bir veri yok. Sorunu çözmek için zaman aralığını artırın veya örnek veri gönderme olduğunu onaylayın.
>
> ![Hiçbir veri bildirimi](media/v2-update-explorer/explorer-seven.png)

## <a name="preview-chart"></a>Grafik önizlemesi

Grafik ile zaman serisi görüşleri örnekleri çizgilerle görüntüleyebilirsiniz. Grafik büyütmek için web denetimleri seçerek ortam paneli, veri modeli ve zaman aralığı Denetim Masası daraltabilirsiniz.

  [![Önizleme grafik genel bakış](media/v2-update-explorer/explorer-eight.png)](media/v2-update-explorer/explorer-eight.png#lightbox)

- **Seçili tarih aralığı**: Denetimler, hangi veri öğeleri için görselleştirme bulunur.

- **İç tarih aralığı kaydırıcı aracını**: İstediğiniz zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın.

- **Zaman aralığı Daralt denetim**: Daraltır ve zaman aralığı paneli Düzenleyicisi genişletir.

- **Y ekseni biçim denetimi**: Kullanılabilir y ekseni görüntüleme seçenekleri arasında geçiş yapar:

    * `Default`: Her satırda tek bir y ekseni bulunmaktadır.
    * `Stacked`: Y ekseni seçilen satırdaki göre değişen verilerle aynı y ekseni birden çok satırda yığın oluşturmak için kullanın.
    * `Shared`: Tüm y ekseni veri birlikte görüntülenir.

- **Geçerli bir veri öğesi**: Şu anda seçili veri öğesi ve onun ilişkili ayrıntıları.

Detaya gitmek için daha fazla bir özel veri dilimi geçerli grafik veri noktasında sol ve ardından seçtiğiniz uç nokta için seçilen alan sürükleyin. Seçilen gri alana sağ tıklayın ve seçin **yakınlaştırma**bu aşağıdaki görüntüde gösterildiği gibi:

  [![Önizleme grafik yakınlaştırma](media/v2-update-explorer/explorer-nine.png)](media/v2-update-explorer/explorer-nine.png#lightbox)

Siz gerçekleştirdikten sonra **yakınlaştırma** eylem, seçilen veri kümesi görürsünüz. Time Series Insights verilerinizi üç y ekseni temsillerini arasında geçiş yapmak için y ekseni biçim denetimi seçin.

  [![Önizleme grafik y ekseni](media/v2-update-explorer/explorer-ten.png)](media/v2-update-explorer/explorer-ten.png#lightbox)

Burada, paylaşılan Y eksenleri örneğini görebilirsiniz:

  [![Y ekseni paylaşımlı Önizleme](media/v2-update-explorer/explorer-eleven.png)](media/v2-update-explorer/explorer-eleven.png#lightbox)

## <a name="time-editor-panel"></a>Zaman Düzenleyicisi paneli

Zaman serisi öngörüleri Önizleme ile çalışırken, ilk olarak bir zaman aralığı seçin. Seçilen zaman aralığını, zaman serisi öngörüleri Önizleme pencere öğeleri ile işleme için kullanılabilir veri kümesi denetler. Aşağıdaki web denetimleri, çalışma zaman aralığını seçmek için zaman serisi öngörüleri önizlemede kullanılabilir:

  [![Zaman seçimi paneli](media/v2-update-explorer/explorer-twelve.png)](media/v2-update-explorer/explorer-twelve.png#lightbox)

1. **İç tarih aralığı kaydırıcı aracını**: İstediğiniz zaman aralığı sürükleyerek iki uç nokta denetimleri kullanın. Bu iç tarih aralığı dış tarih aralığı kaydırıcı denetimi tarafından sınırlanır.

1. **Artırın ve azaltın tarih aralığı düğmeleri**: Artırın veya istediğiniz aralığını ya da düğmesini seçerek, zaman aralığını azaltın.

1. **Zaman aralığı Daralt denetim**: Bu web denetimi iç tarih aralığı kaydırıcı aracını hariç tüm denetimleri gizleme olanak tanır.

1. **Dış tarih aralığı kaydırıcı denetimi**: Uç nokta denetimleri, iç tarih aralığı denetiminiz için kullanılabilir olacak dış tarih aralığı seçmek için kullanın.

1. **Hızlı süreler tarih aralığı açılan liste**: Son gibi belirlenen zaman aralığı seçimleri arasında hızlıca geçiş kullanmak **30 dakika**, **son 12 saat**, veya bir **özel aralık**. Bu değeri değiştirmeniz aralık boyutu kaydırıcı aracını tartışılan kullanılabilir aralığı aralıkları değiştirir.

1. **Aralık boyutu kaydırıcı aracını**: Aynı zaman aralığı içine ve dışına aralıkları yakınlaştırmak için kullanın. Bu eylem büyük saat dilimleri arasında hareket daha kesin bir denetim sağlar. Bir milisaniye küçük dilimlerin aşağı kesintisiz eğilimleri gösterir. Verilerinizi ayrıntılı, yüksek çözünürlüklü keser görmek için kullanabilirsiniz. Kaydırıcının varsayılan başlangıç noktası çözümleme, sorgu hızı ve ayrıntı düzeyi dengeleyen seçimden, verilerin en iyi görünüm olarak ayarlanır.

1. **Tarih aralığı içine ve dışına web denetimi**: Bu web denetimiyle istediğiniz tarih ve saat aralığı kolayca seçebilirsiniz. Ayrıca, farklı saat dilimleri arasında geçiş yapmak için denetimi de kullanabilirsiniz. Geçerli çalışma alanınıza uygulanacak değişiklikler yaptıktan sonra seçin **Kaydet**.

   [![Seçim paneli gelen ve giden](media/v2-update-explorer/explorer-thirteen.png)](media/v2-update-explorer/explorer-thirteen.png#lightbox)

## <a name="navigation-panel"></a>Gezinti Bölmesi

Zaman serisi öngörüleri Önizleme Gezinti panelini, zaman serisi görüşleri uygulamanızın üstünde görünür. Bu, aşağıdaki işlevleri sağlar.

### <a name="current-session-share-link-control"></a>Geçerli oturumdaki paylaşımını bağlantı denetimi

  [![Paylaş simgesi](media/v2-update-explorer/explorer-fifteen.png)](media/v2-update-explorer/explorer-fifteen.png#lightbox)

Yeni **paylaşmak** simgesini bir URL bağlantısı ekibinizle paylaşın.

  [![Örneği URL'nizi paylaşın](media/v2-update-explorer/url-share.png)](media/v2-update-explorer/url-share.png#lightbox)

### <a name="tenant-section"></a>Kiracı bölümü

  [![Kiracı seçimi](media/v2-update-explorer/explorer-sixteen.png)](media/v2-update-explorer/explorer-sixteen.png#lightbox)

* Geçerli zaman serisi görüşleri oturum açma hesap bilgilerini görüntüler.
* Kullanılabilir Time Series Insights temalar arasında geçiş yapmak için kullanın.
* Önizleme görüntülemek için kullanın [demo web uygulaması](https://insights.timeseries.azure.com/preview/demo).

### <a name="theme-selection"></a>Tema seçimi

Yeni bir tema seçmek için sağ üst köşesinde bulunan, profili simgesini seçin. Ardından, **temayı Değiştir**.

  [![Tema seçimi](media/v2-update-explorer/theme-selection.png)](media/v2-update-explorer/theme-selection.png#lightbox)

> [!TIP]
> Dil Seçimi Ayrıca, profili simgesini seçerek kullanılabilir.

Azure zaman serisi öngörüleri Önizleme iki tema destekler:

* **Açık tema**: Bu belgede gösterilen varsayılan tema.
* **Koyu tema**: Gezgin, burada gösterildiği şekilde işlenir:

  [![Seçili koyu tema](media/v2-update-explorer/explorer-seventeen.png)](media/v2-update-explorer/explorer-seventeen.png#lightbox)

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="preview-terms-panel"></a>Önizleme koşulları bölmesi

Bu bölüm yalnızca güncelleştirilmiş kullanıcı Arabiriminde Gezgin kullanma girişimi mevcut S1/S2 ortamlar için geçerlidir. Genel kullanıma sunulan ürün ve önizleme birlikte kullanmak isteyebilirsiniz. Bazı işlevler mevcut kullanıcı Arabiriminden güncelleştirilmiş Gezgini'ne ekledik ancak mevcut zaman serisi görüşleri Gezgin ortamında S1/S2 için tam kullanıcı Arabirimi deneyimi elde edebilirsiniz. 

Hiyerarşinin yerine sorguları ortamınızda tanımladığınız zaman serisi görüşleri terimler paneli bakın. Verilerinizi üzerinde bir koşula göre filtrelemek için kullanın.

  [![Burada paneli sorgulama](media/v2-update-explorer/explorer-eighteen.png)](media/v2-update-explorer/explorer-eighteen.png#lightbox)

Zaman serisi öngörüleri Önizleme Koşulları Düzenleyicisi paneli aşağıdaki parametreleri alır:

**Burada**: Kullanmak where yan tümcesi işlenen kümesini kullanarak, olayları hızlı bir şekilde filtrelemek için aşağıdaki tabloda listelenen. Bir işlenen seçerek bir arama yapın, o aramaya bağlı koşul otomatik olarak güncelleştirilir. Desteklenen işlenen türleri şunlardır:

| İşlem | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` | Çift, DateTime, zaman aralığı | |
| `=`, `!=`, `<>` | Dize, Bool, Double, DateTime, zaman aralığı, NULL |
| `IN` | Dize, Bool, Double, DateTime, zaman aralığı, NULL | Tüm işlenenler aynı türde veya NULL sabiti olması. |
| `HAS` | String | Sağ tarafta yalnızca sabit dize değişmez değerleri izin verilir. Boş dize ve NULL olmasına izin verilmez. |

Desteklenen sorgu işlemleri ve veri türleri hakkında daha fazla bilgi için bkz: [zaman serisi ifade (TSX)](https://docs.microsoft.com/rest/api/time-series-insights/preview-tsx).

### <a name="examples-of-where-clauses"></a>Örnekleri where yan tümceleri

  [![Burada tümcesi örnekleri](media/v2-update-explorer/explorer-nineteen.png)](media/v2-update-explorer/explorer-nineteen.png#lightbox)

**Ölçü**: Tüm sayısal sütunları görüntüleyen bir açılır liste (**double**) için geçerli grafik öğeleri olarak kullanabilirsiniz.

**Bölme ölçütü**: Bu açılan liste, modelinizdeki verilerinizi göre gruplandırabilirsiniz tüm kullanılabilir kategorik sütunlar (dize) görüntüler. Aynı x eksenine görüntülemek için en fazla beş koşullarını ekleyebilirsiniz. Girin ve ardından parametreleri **Ekle** yeni bir terim eklemek için.

  [![Sorgulanan ve filtrelenmiş görünümü bir](media/v2-update-explorer/explorer-twenty.png)](media/v2-update-explorer/explorer-twenty.png#lightbox)

Gösterebilir ve öğeleri grafik panelinde görünür simgesini seçerek aşağıdaki görüntüde gösterildiği gibi gizle. Sorguları tamamen kaldırmak için kırmızı seçin **X**.

  [![Sorgulanan ve filtrelenmiş görünüm iki](media/v2-update-explorer/explorer-twenty-one.png)](media/v2-update-explorer/explorer-twenty-one.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [depolama ve giriş](./time-series-insights-update-storage-ingress.md) Azure zaman serisi öngörüleri önizlemede.
- Zaman serisi öngörüleri Önizleme belgeyi okumaya [veri modelleme](./time-series-insights-update-tsm.md).
- Bilgi [tanılayın ve sorunlarını gidermeyi](./time-series-insights-update-how-to-troubleshoot.md) Time Series Insights örneğinizin.
