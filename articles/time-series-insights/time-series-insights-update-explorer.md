---
title: Gezgini güncelleştirme verilerini Görselleştirme | Microsoft Docs
description: Azure Time Series Insights Gezgini güncelleştir
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/28/2018
ms.openlocfilehash: bf091eec271e3fb27f6ab312ff5d0096efa7f90c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856887"
---
# <a name="visualize-data-in-the-explorer-update"></a>Explorer güncelleştirme verileri görselleştirin

Bu makalede, özellikleri ve Azure Time Series Insights içinde (Önizleme) Explorer web uygulamasının kullanılabilir seçenekleri açıklar.

## <a name="prerequisites"></a>Önkoşullar

Azure Time Series Insights Gezgini (Önizleme) kullanmadan önce şunları yapmalısınız:

* Sağlanan bir zaman serisi görüşleri ortamına sahip. Bir zaman serisi görüşleri ortamına sağlama hakkında daha fazla bilgi edinin.
* Hesabı oluşturduğunuz zaman serisi görüşleri ortamına veri erişimi sağlar. Diğerlerinin yanı sıra kendiniz erişim sağlanabilir.
* Olay kaynağı, zaman serisi görüşleri ortamına veri göndermek için bir ortam ekleyin.

## <a name="learn-about-the-azure-time-series-insights-preview-explorer"></a>Azure Time Series Insights Gezgini (Önizleme) hakkında bilgi edinin

  ![bir arada Gezgini][1]

Azure Time Series Insights Gezgini (Önizleme), aşağıdaki yedi öğeyi ayrılır:

1. TSI (Önizleme) gezinti çubuğunda, analiz ve model sayfalar arasında geçiş yapmasını sağlar.
1. TSI (Önizleme) hiyerarşi ağacı grafiği için belirli veri öğelerini seçin.
1. TSI (Önizleme) zaman serisi, tüm seçili olan veri öğeleri de gösterir.
1. Geçerli çalışma grafiğinizi TSI (Önizleme) grafik panelini görüntüler.
1. TSI (Önizleme) zaman çizelgesi çalışma süre değiştirebilmenizi sağlar.
1. TSI (Önizleme) uygulama çubuğunu (örneğin, geçerli Kiracı), kullanıcı yönetim seçeneklerini içerir ve tema ve dil ayarlarını değiştirmenizi sağlar.

## <a name="tsi-preview-environment-panel"></a>Ortam paneline TSI (Önizleme)

Ortam paneline erişiminiz olan tüm TSI ortamları görüntüler. Bu, S1/S2 ortamları (GA) yanı sıra Kullandıkça Öde ortamları (Önizleme) bir listesini içerir. Kullanmak istediğiniz TSI ortam tıklamanız yeterlidir.

  ![iki Gezgini][2]

## <a name="time-series-insights-preview-navigation-menu"></a>Time Series Insights (Önizleme) Gezinti Menüsü

  ![üç Gezgini][3]

Gezinti Menüsü TSI uygulamalar arasında geçiş yapmanızı sağlar:

* – Analiz edin, grafik ve zengin analizler Modellenen veya modellenmemiş zaman serisi verileriniz üzerinde gerçekleştirmek sağlar.

* Model - TSI modelinize yeni TSI güncelleştirme türlerini, hiyerarşileri ve örnekleri göndermenize izin verir.

## <a name="time-series-insights-update-model-authoring"></a>Model yazma Time Series Insights'ı güncelleştirme

Bu uygulama, zaman serisi modeli CRUD işlemleri gerçekleştirme olanağı sağlar.  

* TSM türü - TSI türlerini tanımlayan değişkenleri veya hesaplamalar yapan formüller etkinleştirmek, belirli bir TSI örneğiyle ilişkilendirilmiş. Bir türü veya daha fazla değişken olabilir.
* Verilerinizin sistematik kuruluşlar TSM hiyerarşi - hiyerarşi var. Hiyerarşiler TSI verilerinizde farklı varlıklar arasındaki ilişkileri kullanılırlar.
* Zaman serisi TSM örneği - örnekleri var. Çoğu durumda, bu olacaktır **DeviceID** veya **AssetID**, olduğu varlık ortamda benzersiz tanımlayıcısı.

TSM hakkında daha fazla bilgi edinmek için [kez serisi modelleri](./time-series-insights-update-tsm.md).

## <a name="time-series-insights-preview-model-search-panel"></a>Time Series Insights (Önizleme) modeli arama paneli

Model arama panelini kolayca arama yapın ve grafiğinizde görüntülemek istediğiniz belirli bir zaman serisi örnekleri bulmak için TSM hiyerarşisinde gezinme sağlar. Örneklerinizin seçtiğinizde, yalnızca geçerli grafik için eklenmez ancak aynı zamanda verileri de eklenir.

  ![dört Gezgini][4]

## <a name="time-series-insights-preview-well"></a>Time Series Insights (iyi önizleme)

Örnek alanları ve seçilen zaman serisi örnekleri ile ilişkili diğer meta verileri de görüntüler. Sağ taraftaki onay kutularını, geçerli hesap belirli örnekleri görüntülemek veya gizlemek izin verin. Ayrıca, belirli veri öğelerini de kırmızı x denetim öğesi sağındaki tıklayarak geçerli verilerinizden kaldırabilirsiniz.

  ![beş Gezgini][5]

Öğelerin daha iyi dikey görünüm de verilerinizi almak için telemetri paneli açılan.

  ![altı Gezgini][6]

Not: aşağıdaki simgesi görürseniz, örnek veriler seçili zaman aralığı sırasında yok.  Düzeltmek için seçili zaman aralığı artırmak ve/veya örnek veri gönderme olduğunu onaylayın.

  ![yedi Gezgini][7]

## <a name="time-series-insights-preview-chart"></a>Time Series Insights (Önizleme) grafiği

Grafik, zaman serisi örnekleri satırları görüntülemek sağlar. Grafik büyütmek için web denetimleri tıklayarak ortam paneli, veri modeli ve zaman aralığı Denetim Masası daraltabilirsiniz.

  ![sekiz Gezgini][8]

1. Seçili tarih aralığı – grafik paneli, hangi veri öğelerini görselleştirme için kullanılabilir olacak bu denetimleri için şu anda seçili tarih aralığı.

1. İç tarih aralığı kaydırıcı aracını - tıklatarak ve sürükleyerek istediğiniz zaman içinde kullanımı iki uç nokta denetimleri kapsar.

1. Zaman aralığı Daralt denetimi - bu denetim daraltır ve zaman aralığı paneli Düzenleyicisi genişletir.

1. Y ekseni biçim denetimi: denetim kullanılabilir y ekseni görüntüleme seçeneklerini geçiş yapmak için tıklatın. Y ekseni görünümü seçenekler sağlanır:

    * Varsayılan - her satırda tek bir y ekseni bulunmaktadır.
    * Yığılmış – Bu, birden çok satırda aynı y ekseni, y ekseni seçilen satırdaki göre değişen verilerle yığın sağlar
    * Paylaşılan – birlikte görüntülenen tüm y ekseni veri

1. Geçerli veri öğesi – şu anda seçili veri öğesi ve onun ilişkili ayrıntılar.

Bir özel veri dilimi fare basılı tutarak ve ardından seçilen alan dilediğiniz bitiş noktasına sürükleyerek bir veri noktasına geçerli grafik sol tarafından tıklatma daha fazla ayrıntıya gidebilirsiniz. Renkte, seçilen alana sağ tıklayın ve yakınlaştırma aşağıda gösterildiği gibi tıklayın. Aşağıdakileri de yapabilirsiniz:

  ![dokuz Gezgini][9]

Yakınlaştırma eylemi gerçekleştirdikten sonra seçili veri kümeniz şimdi göreceksiniz. Y ekseni biçimi Denetim, üç farklı y ekseni temsillerini TSI verilerinizi arasında geçiş yapmak için tıklayın.

  ![on Gezgini][10]

Burada, paylaşılan bir y eksenleri örneği görebilirsiniz.

  ![on Gezgini][11]

## <a name="time-series-insights-preview-time-editor-panel"></a>Time Series Insights (Önizleme) zamanı düzenleyicisinin paneli

TSI ile çalışırken, bir süre önce seçersiniz. Seçili zaman aralığı TSI güncelleştirme pencere öğeleri ile işleme için kullanılabilir veri kümesi denetler. Aşağıdaki web denetimleri, çalışma zaman aralığını seçmek için TSI güncelleştirme kullanılabilir.

  ![on Gezgini][12]

1. **İç tarih aralığı kaydırıcı aracını** - tıklayarak iki uç nokta denetimleri kullanın ve istediğiniz zaman içinde sürükleyerek span. Bu "İç" tarihe, aşağıdaki başvurulan "Dış tarihi" aralık kaydırıcı denetimi tarafından kısıtlı.

1. Artırma ve azaltma tarih aralığı düğmeleri ** - artış veya istediğiniz aralığını ya da düğmesine tıklayarak, zaman aralığını azaltın.

1. **Zaman aralığı Daralt denetim** -iç tarih aralığı kaydırıcı aracını hariç tüm denetimlerini gizlemek bu web denetimi sağlar.

1. **Dış-tarih aralığı kaydırıcı denetimi** -endpoint denetimleri kullanarak, "İç tarih" aralık denetimi için kullanılabilecek dış tarih aralığını seçin.

1. **Hızlı süreler tarih aralığı açılan** -son 30 dakika, son 12 saat, özel bir aralık, vb. gibi belirlenen zaman aralığı seçimleri arasında hızlıca geçiş olanağı sağlar. Bu değeri değiştirmeniz aralık boyutu kaydırıcı aracını aşağıda açıklanmıştır kullanılabilir aralığı aralıkları değiştirir.

1. **Aralık boyutu kaydırıcı aracını** -aralık boyutu kaydırıcı aracını aynı zaman aralığı içine ve dışına aralıkları yakınlaştırma sağlar. Bu, zaman dilimleri aşağı kesintisiz eğilimlerini ayrıntılı, yüksek çözünürlüklü keser verilerinizi görmenize olanak sağlayan milisaniyelik kadar küçük Göster büyük dilimleri arasında taşıma daha kesin bir denetim sağlar. Kaydırıcının varsayılan başlangıç noktası seçiminizden verilerinin en iyi görünümü ayarlanır; Dengeleme çözümü, sorgu hızı ve ayrıntı düzeyi.

1. **Tarih aralığı için ve web denetiminden** - bu web denetimi kolayca tıklayın ve istenen tarih ve saat aralığı seçin. Ayrıca, farklı saat dilimleri arasında geçiş yapmak için denetimi de kullanabilirsiniz. Geçerli çalışma alanınıza uygulamak istediğiniz değişiklikleri yaptıktan sonra Kaydet'e tıklayın düğmesi.

  ![Explorer On üç][13]

## <a name="time-series-insights-preview-navigation-panel"></a>Zaman serisi görüşleri (Önizleme) Gezinti paneli

TSI güncelleştirme Gezinti bölmesine, aşağıdaki işlevleri sağlar:

  ![on dört Gezgini][14]

### <a name="current-session-share-link-control"></a>Geçerli oturumdaki paylaşımını bağlantı denetimi

  ![beş Gezgini][15]

Daire içinde bağlantı web denetimi kaydetmek veya içeren geçerli Time Series Insights çalışma oturumunuz paylaşmak için bir URL oluşturmak için tıklayın:

* Seçili zaman aralığı
* Şu anda seçili aralık boyutu
* Veriler şu anda seçili

### <a name="tenant-section"></a>Kiracı bölümü

  ![Explorer on altı][16]

* Geçerli TSI oturum açma hesap bilgilerini görüntüler
* Kullanılabilir TSI temalar arasında geçiş sağlar.

### <a name="theme-selection"></a>Tema seçimi

(Önizleme) Azure TSI iki tema destekler:

* **Açık tema**: Bu belgede gösterilen varsayılan tema budur.
* **Koyu tema**: Bu seçenek Gezgini koyu tema aşağıda gösterildiği gibi işler:

  ![Explorer on yedi][17]

Burada arasında desteklenen dilleri değiştirebilirsiniz.

## <a name="s1s2-environment-controls"></a>S1/S2 ortam denetimleri

### <a name="time-series-insights-preview-terms-panel"></a>Time Series Insights (Önizleme) terimler paneli

Bu bölüm yalnızca güncelleştirilmiş kullanıcı Arabiriminde Gezgin kullanılmaya çalışılıyor mevcut S1/S2 ortamlar için de geçerlidir. GA ürün kullanın ve başka bir birlikte (Önizleme) güncelleştirmek için bunu yapmak isteyebilirsiniz. Biz, bazı işlevler, varolan kullanıcı Arabiriminden güncelleştirilmiş Gezgini'ne ekledik ancak, her zaman tam UI deneyimi için mevcut TSI Gezgini S1/S2 ortamında elde edebileceğinizi biliyor.  

Hiyerarşinin yerine TSI terimler paneli görürsünüz. Sorgular, ortamınızda tanımladığınız budur. Verilerinizi kullanarak bir koşula göre filtreleme özelliği sağlar.

  ![Explorer On sekiz][18]

TSI (Önizleme) Koşulları Düzenleyicisi paneli aşağıdaki parametreleri alır.

**Burada**: where yan tümcesi olaylarınızı aşağıda listelenen işlenen kümesini kullanarak hızlı bir şekilde filtrelemenize olanak sağlar. Seçme/tıklayarak arama yapma, koşul güncelleştirme otomatik olarak bu arama temel. Desteklenen işlenen türleri şunlardır:

| İşlem | Desteklenen türler   | Notlar |
| --- | --- | --- |
| `<`, `>`, `<=`, `>=` |    Çift, DateTime, zaman aralığı  | |
| `=`, `!=`, `<>`   | Dize, Bool, Double, DateTime, zaman aralığı, NULL    |
| `IN` |    Dize, Bool, Double, DateTime, zaman aralığı, NULL |    Tüm işlenenler aynı türde veya NULL sabiti olması. |
| `HAS` |   Dize |    Yalnızca sabit dize değişmez değerleri, sağ tarafında izin verilir. Boş dize ve NULL yapılamaz. |

### <a name="examples-of-where-clauses"></a>Örnekleri Where yan tümceleri

  ![on dokuz Gezgini][19]

**Ölçü**: tüm sayısal sütunları bu açılan gösterir (**double**), geçerli grafiğiniz için öğeleri olarak kullanabilirsiniz.

**Bölme ölçütü**: Bu açılan tüm kategorik sütunlar (dize), kullanılabilir modelinizde verilerinizi göre gruplandırmak için kullanabileceğiniz gösterir. Aynı x eksenine görüntülemek için en fazla beş koşullarını ekleyebilirsiniz. İstenen parametrelerinizi girin ve ardından **Ekle** düğmesini yeni bir terim ekleyin.

  ![yirmi Gezgini][20]

Gizleyebilir ve aşağıda gösterildiği gibi görünür simgeye tıklayarak grafik panel öğeleri göster. Kırmızı tıklayarak sorguları tamamen kaldırabilirsiniz `X` aşağıda gösterilmektedir.

  ![Explorer yirmi bir][21]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [Azure TSI (Önizleme) depolama ve giriş](./time-series-insights-update-storage-ingress.md).

Yeni hakkında okuyun [zaman serisi modelleri](./time-series-insights-update-tsm.md).

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