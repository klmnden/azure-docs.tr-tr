---
title: "Azure günlük analizi Görünüm Tasarımcısı bölümlerinde Başvuru Kılavuzu | Microsoft Docs"
description: "Günlük analizi Görünüm Tasarımcısı kullanarak veri görselleştirmeleri çeşitli günlük analizi çalışma alanınızda görüntülemek Azure Portalı'nda özel görünümler oluşturabilirsiniz. Bu makalede, ayarları özel görünümlerde kullanılabilir görselleştirme bölümleri için bir başvuru kılavuzdur."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: 6fd19cce955e1f06c9b6f5a9ef5d85d9fd63c1c1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-log-analytics"></a>Günlük analizi Görünüm Tasarımcısı görselleştirme bölümlerinde Başvuru Kılavuzu
Azure günlük analizi Görünüm Tasarımcısı kullanarak veri görselleştirmeleri günlük analizi çalışma alanı, çeşitli sunmak Azure Portalı'nda özel görünümler oluşturabilirsiniz. Bu makalede, ayarları özel görünümlerde kullanılabilir görselleştirme bölümleri için bir başvuru kılavuzdur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Görüntüleme Tasarımcısı](log-analytics-view-designer.md): oluşturma ve özel görünümler düzenleme Görünüm Tasarımcısı ve yordamları genel bakış sağlar.
* [Başvuru döşeme](log-analytics-view-designer-tiles.md): ayarları kendi özel görünümlerinizi de kullanılabilir her bölme için bir başvuru sağlar.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), tüm görünümleri sorgularda yazılmalıdır [yeni sorgu dili](https://go.microsoft.com/fwlink/?linkid=856078). Çalışma alanı yükseltmeden önce oluşturulmuş görünümler otomatik olarak dönüştürülür.

Kullanılabilir Görünüm Tasarımcısı döşeme türleri aşağıdaki tabloda açıklanmıştır:

| Görünüm türü | Açıklama |
|:--- |:--- |
| [Sorgu listesi](#list-of-queries-part) |Günlük arama sorguları listesini görüntüler. Her sorgu sonuçlarını görüntülemeyi seçebilirsiniz. |
| [Sayı ve listesi](#number-amp-list-part) |Üstbilgi, günlük arama sorgusu kayıtları sayısını gösteren tek bir sayı görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler. |
| [İki sayı ve listesi](#two-numbers-amp-list-part) |Üstbilgi sayıları farklı günlük arama sorguları kayıtlarını Göster iki sayı görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler. |
| [Halka ve listesi](#donut-amp-list-part) |Üstbilgi, günlük sorgu değer sütununda özetlenir tek bir sayı görüntüler. Halka grafik üst üç kayıt sonuçlarını görüntüler. |
| [İki zaman çizelgeleri ve listesi](#two-timelines-amp-list-part) |Üstbilgi, sütun grafikler, bir günlük sorgu değer sütununda özetlenir tek bir sayı görüntüleyen belirtme çizgisi olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler. |
| [Bilgi](#information-part) |Üstbilgi statik metin ve isteğe bağlı bir bağlantı görüntüler. Bir veya daha fazla öğe statik başlık ve metin ile birlikte görüntüler. |
| [Çizgi grafiği, belirtme çizgisi ve listesi](#line-chart-callout-amp-list-part) |Üstbilgi saati ve bir belirtme çizgisi özetlenen bir değerle üzerinden günlük sorgudan birden fazla seri ile bir çizgi grafiği görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler. |
| [Çizgi grafiği ve listesi](#line-chart-amp-list-part) |Üstbilgi zaman içinde bir günlük sorgudan birden fazla seri ile bir çizgi grafiği görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler. |
| [Çizgi grafikler bölümü yığını](#stack-of-line-charts-part) |Zaman içinde bir günlük sorgudan birden fazla seri üç ayrı çizgi grafiklerde görüntüler. |

Sonraki bölümlerde kutucuğu türleri ve özelliklerinin ayrıntılı açıklanmaktadır.

## <a name="list-of-queries-part"></a>Sorguları bölümü listesi
Sorguları bölümü listesini günlük arama sorguları listesini görüntüler. Her sorgu sonuçlarını görüntülemeyi seçebilirsiniz. Varsayılan olarak tek bir sorgu görünümü içerir ve seçebileceğiniz **+ sorgu** ek sorgular eklemek için.

![Sorgular görünümünü listesi](media/log-analytics-view-designer/view-list-queries.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Başlık |Görünüm üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Önceden seçilen filtreleri |Bir sorgu seçtiğinizde sol Filtre Bölmesi'nde dahil etmek için özelliklerin virgülle ayrılmış listesi. |
| Mod işleme |Sorgu seçildiğinde görüntülenen ilk görünüm. Sorgu açtıktan sonra tüm kullanılabilir görünümleri seçebilirsiniz. |
| **Sorgular** | |
| Arama sorgusu |Çalıştırılacak sorgu. |
| Kolay ad | Görüntülenen açıklayıcı adı. |

## <a name="number-and-list-part"></a>Sayı ve listesi bölümü
Üstbilgi, günlük arama sorgusu kayıtları sayısını gösteren tek bir sayı görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler.

![Sorgular görünümünü listesi](media/log-analytics-view-designer/view-number-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünüm üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **Başlık** | |
| Gösterge |Üstbilgi üstünde görüntülenen metin. |
| Sorgu |Başlığı için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sonuçları ilk on kayıtlar için ilk iki özellikleri görüntülenir. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Çubukları sayısal sütun göreli değeri temel alan otomatik olarak oluşturulur.<br><br>Kullanım `Sort` listedeki kayıtları sıralamak için sorgu komutu. Sorgu çalıştırmak ve tüm kayıtları döndürmek için seçebileceğiniz **tümünü görmek**. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcısı. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="two-numbers-and-list-part"></a>İki sayı ve liste bölümü
Üstbilgisi ayrı günlük arama sorguları kayıtları sayısını görüntüleyin iki sayının sahiptir. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler.

![İki sayı & Liste Görünümü](media/log-analytics-view-designer/view-two-numbers-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünüm üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **Başlık** | |
| Gösterge |Üstbilgi üstünde görüntülenen metin. |
| Sorgu |Başlığı için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sonuçları ilk on kayıtlar için ilk iki özellikleri görüntülenir. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Çubukları, sayısal sütun göreli değerine bağlı olarak otomatik olarak oluşturulur.<br><br>Kullanım `Sort` listedeki kayıtları sıralamak için sorgu komutu. Sorgu çalıştırmak ve tüm kayıtları döndürmek için seçebileceğiniz **tümünü görmek**. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcısı. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="donut-and-list-part"></a>Halka ve listesi bölümü
Üstbilgi, günlük sorgu değer sütununda özetlenir tek bir sayı görüntüler. Halka grafik üst üç kayıt sonuçlarını görüntüler.

![Halka ve liste görünümü](media/log-analytics-view-designer/view-donut-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenen metin. |
| Alt Başlık |Üstbilginin üst başlığı altında görüntülenen metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. |
| **Halka** |**> Center** |
| Metin |Halka içindeki değeri altında görüntülenen metin. |
| İşlem |Tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<ul><li>Toplam: tüm kayıtların değerlerini ekler.</li><li>Yüzdesi: Değerleri tarafından döndürülen kayıtları oranını **neden merkezi işleminde kullanılan değerleri** sorguda toplam kayıtları için.</li></ul> |
| Merkezi işleminde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işareti (+) seçin. Sorgu sonuçlarını kayıtları belirttiğiniz özellik değerleri ile sınırlıdır. Hiçbir değer eklediyseniz, tüm kayıtları sorguda dahil edilir. |
| **Ek Seçenekler** |**> Renkleri** |
| Rengi 1<br>Renk 2<br>Rengi 3 |Halka görüntülenen değerlerin her biri için rengi seçin. |
| **Ek Seçenekler** |**> Gelişmiş renk eşleme** |
| Alan değeri |Halka eklenirse farklı bir renk olarak görüntülenecek bir alanın adını yazın. |
| Renk |Benzersiz alan rengini seçin. |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcısı. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="two-timelines-and-list-part"></a>İki zaman çizelgeleri ve liste bölümü
Üstbilgi, sütun grafikler, bir günlük sorgu değer sütununda özetlenir tek bir sayı görüntüleyen belirtme çizgisi olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler.

![İki zaman çizelgelerini ve liste görüntüle](media/log-analytics-view-designer/view-two-timelines-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **İlk grafik<br>ikinci grafik** | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenen metin. |
| Renk |Serideki sütunlar için kullanılacak rengi. |
| Sorgu |İlk seri için çalıştırılacak sorgu. Her zaman aralığı üzerinden kayıt sayısı grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<ul><li>Toplam: Tüm kayıtları değerleri toplamı.</li><li>Ortalama: Tüm kayıtları değerleri ortalama.</li><li>En son örnek: grafikte dahil son aralığı değeri.</li><li>İlk örnek: grafikte dahil birinci aralık değerinden.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li></ul> |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="information-part"></a>Bilgi bölümü
Üstbilgi statik metin ve isteğe bağlı bir bağlantı görüntüler. Bir veya daha fazla öğe statik başlık ve metin ile birlikte görüntüler.

![Bilgileri görüntüle](media/log-analytics-view-designer/view-information.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Renk |Başlık arka plan rengi. |
| **Üstbilgi** | |
| Görüntü |Üst bilgi görüntülenen görüntü dosyası. |
| Etiket |Üst bilgi görüntülenen metin. |
| **Üstbilgi** |**> Bağlantı** |
| Etiket |Bağlantı metni. |
| Url |Bağlantı için Url. |
| **Bilgi öğeleri** | |
| Başlık |Her öğe başlığı için görüntülenen metin. |
| İçerik |Her öğe için görüntülenen metin. |

## <a name="line-chart-callout-and-list-part"></a>Çizgi grafiği, belirtme çizgisi ve liste bölümü
Üstbilgi zaman ve özetlenen bir değerle belirtme çizgisi üzerinde birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler.

![Çizgi grafiği, belirtme çizgisi ve liste görünümü](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenen metin. |
| Alt Başlık |Üstbilginin üst başlığı altında görüntülenen metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseni bu zaman aralığı kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, x ekseni kullanır saatlik aralıklarla. |
| **Çizgi grafiği** |**> Belirtme** |
| Belirtme çizgisi başlık |Belirtme çizgisi değerin görüntülenen metin. |
| Seri adı |Belirtme çizgisi değeri kullanmak seri için özellik değeri. Seri sağlanırsa, sorgudaki tüm kayıtları kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<ul><li>Ortalama: Tüm kayıtları değerleri ortalama.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>En son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: En büyük değeri grafikte dahil aralıkları.</li><li>En küçük: En düşük değerden grafikte dahil aralıkları.</li><li>Toplam: Tüm kayıtları değerleri toplamı.</li></ul> |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen birimler için değerleri belirtin. Bu bilgiler değer türleri gösteren görüntü grafik etiketleri ve, isteğe bağlı olarak, için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *dönüştürmek*, gelen sayısal değerleri dönüştürülür *geçerli birim* için yazın *dönüştürmek* türü. |
| Özel Etiket |Y eksenine etiketi yanında görüntülenen metin *birim* türü. Etiket yok, yalnızca belirtilmişse *birim* türü görüntülenir. |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcısı. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="line-chart-and-list-part"></a>Satır grafiği ve liste bölümü
Üstbilgi zaman içinde bir günlük sorgudan birden fazla seri ile bir çizgi grafiği görüntüler. Sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik olan bir sorgu üst on sonuçlarını görüntüler.

![Satır grafiği ve liste görünümü](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| Simgesini kullanın |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenen metin. |
| Alt Başlık |Üstbilginin üst başlığı altında görüntülenen metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseni bu zaman aralığı kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, x ekseni kullanır saatlik aralıklarla. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen birimler için değerleri belirtin. Bu bilgiler değer türleri gösteren görüntü grafik etiketleri ve, isteğe bağlı olarak, için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *dönüştürmek*, gelen sayısal değerleri dönüştürülür *geçerli birim* için yazın *dönüştürmek* türü. |
| Özel Etiket |Y eksenine etiketi yanında görüntülenen metin *birim* türü. Etiket yok, yalnızca belirtilmişse *birim* türü görüntülenir. |
| **List** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| Grafik Gizle |Grafiğin sağ tarafında sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcısı. Daha fazla bilgi için bkz: [ortak ayarları](#sparklines). |
| Gezinti sorgu |Listeden bir öğe seçtiğinizde çalıştırılacak sorgu. Daha fazla bilgi için bkz: [ortak ayarları](#navigation-query). |
| **List** |**> Sütun başlıkları** |
| Ad |İlk sütunun üstünde görüntülenen metin. |
| Değer |İkinci sütunun üstünde görüntülenen metin. |
| **List** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için bkz: [ortak ayarları](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Çizgi grafikler bölümü yığını
Çizgi grafiği yığını, aşağıda gösterildiği gibi üç ayrı çizgi grafiklerde, zaman içindeki günlük sorgudan birden fazla seri görüntüler:

![Çizgi grafiklerde yığını](media/log-analytics-view-designer/view-stack-line-charts.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenen metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde, yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Result üstbilgisinde yanındaki görüntülenen görüntü dosyası. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**> Üstbilgisi** |
| Başlık |Grafik üstünde görüntülenen metin. |
| Alt Başlık |Grafik üstündeki başlığı altında görüntülenen metin. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**Çizgi grafiği** |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseni bu zaman aralığı kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, x ekseni kullanır saatlik aralıklarla. |
| **Grafik** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen birimler için değerleri belirtin. Bu bilgiler değer türleri gösteren görüntü grafik etiketleri ve, isteğe bağlı olarak, için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *dönüştürmek*, gelen sayısal değerleri dönüştürülür *geçerli birim* için yazın *dönüştürmek* türü. |
| Özel Etiket |Y eksenine etiketi yanında görüntülenen metin *birim* türü. Etiket yok, yalnızca belirtilmişse *birim* türü görüntülenir. |

## <a name="common-settings"></a>Genel ayarları
Aşağıdaki bölümlerde, çeşitli görselleştirme bölümleri için ortak olan ayarları açıklanmaktadır.

### <a name="name-value-separator"></a>Ad ve değer ayırıcı
Ad ve değer ayırıcı birden çok değer listesi sorgusu metin özelliğinden ayrıştırmak için kullanılacak tek karakterlik sınırlayıcı ' dir. Bir sınırlayıcı belirtirseniz, aynı sınırlayıcı ayırarak her bir alan için adlarını sağlayabilen **adı** kutusu.

Örneğin, adlı bir özellik göz önünde bulundurun *konumu* gibi değerler dahil *Redmond yapı 41* ve *Bellevue yapı 12*. Bir tire (-) için ad ve değer ayırıcı belirtebilirsiniz ve *Şehir yapı* adı. Bu yaklaşım her değeri adlı iki özellik ayrıştırır *Şehir* ve *yapı*.

### <a name="navigation-query"></a>Gezinti sorgu
Listeden bir öğe seçtiğinizde çalıştırmak için sorgu Gezinti sorgudur. Kullanım *{seçili öğe}* kullanıcı seçili öğe için söz dizimi dahil etmek için.

Sorgu adlı bir sütun varsa, örneğin, *bilgisayar* ve gezinti sorgu *{seçili öğe}*, gibi bir sorgu *bilgisayar "Bilgisayarım" =* seçtiğinizde çalıştırılan bir bilgisayar. Gezinti sorgu ise *türü olay {seçili öğe} =*, sorgu *türü olay bilgisayar = "Bilgisayarım" =* çalıştırılır.

### <a name="sparklines"></a>Mini Grafikler
Bir mini grafik zaman içinde bir liste girdisi değerini gösteren bir küçük çizgi grafiktir. Bir liste görselleştirme bölümleri için sayısal bir sütun veya zamanla değerini gösteren bir mini göreli değerini gösteren bir yatay çubuğu görüntülenip görüntülenmeyeceğini seçebilirsiniz.

Aşağıdaki tabloda Mini Grafikler için ayarları açıklanır:

| Ayar | Açıklama |
|:--- |:--- |
| Mini Grafikler etkinleştir |Yatay çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. |
| İşlem |Mini Grafikler etkinleştirilirse, Mini Grafik için değerleri hesaplamak için listedeki her bir özellik üzerinde gerçekleştirilecek işlem budur.<ul><li>En son örnek: zaman aralığında seri için son değeri.</li><li>En yüksek: Zaman aralığında seri için maksimum değeri.</li><li>Min: Zaman aralığında serisi için en düşük değer.</li><li>Toplam: Değerlerin toplamını zaman aralığında seri için.</li><li>Özet: aynı kullanan `measure` üstbilgisinde sorgu olarak komutu.</li></ul> |

### <a name="thresholds"></a>Eşikleri
Eşikleri kullanarak, listedeki her öğesinin yanındaki renkli bir simge görüntüleyebilirsiniz. Eşikleri belirli bir değeri aşması veya belirli bir aralıkta öğelerin hızlı görsel bir gösterge verin. Örneğin, bir hata değeri aşarsa, kabul edilebilir değer, bir uyarı gösterir bir aralıkta değerse sarı ve kırmızı ile öğeleri için yeşil bir simge görüntüleyebilirsiniz.

Bir bölümü için eşikler etkinleştirdiğinizde, bir veya daha fazla eşikleri belirtmeniz gerekir. Bir öğenin değeri eşik değerden daha büyük ve sonraki eşik değerden daha düşük ise, bu değer rengini kullanılır. Öğesi en yüksek eşik değerinden büyükse, başka bir renk kullanılır. 

Bir eşik değerini her eşik ayarlanmış **varsayılan**. Diğer değerler aşılırsa ayarlanmış rengi budur. Ekleyebilir veya seçerek eşikleri kaldırmak **Ekle** (+) veya **silmek** (x) düğmesi.

Aşağıdaki tabloda eşikleri için ayarları açıklanır:

| Ayar | Açıklama |
|:--- |:--- |
| Eşikleri etkinleştir |Her değer sol tarafında bir renk simgesi görüntülemek için bu bağlantıyı seçin. Simge belirtilen eşikler göre değerinin durumu gösterir. |
| Ad |Eşik değeri adı. |
| Eşik |Eşik değeri. Her liste öğesi için sistem durumu rengi renk tarafından öğenin değeri aşıldı yüksek eşik değerinin ayarlanır. Eşik değerleri aşılırsa, varsayılan renk kullanılır. |
| Renk |Eşik değerini gösteren rengi. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) sorguları görselleştirme bölümlerinde desteklemek için.
