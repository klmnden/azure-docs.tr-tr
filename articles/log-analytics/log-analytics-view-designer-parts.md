---
title: "Görünüm Tasarımcısı'nda OMS günlük analizi için başvuru Kısım | Microsoft Docs"
description: "Görünüm Tasarımcısı'nda günlük analizi, OMS deposundaki verileri farklı görselleştirmesini içeren OMS konsolunda özel görünümler oluşturmanıza olanak sağlar. Bu makalede her özel görünümlerde kullanılabilir görselleştirme bölümlerinin ayarlarını bir başvuru sağlar."
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
ms.date: 07/17/2017
ms.author: bwren
ms.openlocfilehash: 40a6101576708936404447576d704a49666143fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="log-analytics-view-designer-visualization-part-reference"></a>Günlük analizi Görünüm Tasarımcısı görselleştirme bölümü başvurusu
Görünüm Tasarımcısı'nda günlük analizi, farklı veri görselleştirmeleri OMS depodan içeren OMS konsolunda özel görünümler oluşturmanıza olanak sağlar. Bu makalede her özel görünümlerde kullanılabilir görselleştirme bölümlerinin ayarlarını bir başvuru sağlar.

Görünüm Tasarımcısı için kullanılabilir diğer makaleler şunlardır:

* [Görüntüleme Tasarımcısı](log-analytics-view-designer.md) -yordamlarına oluşturma ve düzenleme özel görünümleri ve Görünüm Tasarımcısı genel bakış.
* [Döşeme başvuru](log-analytics-view-designer-tiles.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), tüm görünümleri sorgularda yazılmalıdır sonra [yeni sorgu dili](https://go.microsoft.com/fwlink/?linkid=856078).  Çalışma alanı yükseltilmeden önce oluşturulan görünümleri dönüştürülen automtically olacaktır.

Aşağıdaki tabloda döşeme görünümü Tasarımcısı'nda kullanılabilir farklı türleri açıklanmaktadır.  Aşağıdaki bölümler her döşeme türü ayrıntı ve bunların özelliklerini açıklar.

| Görünüm türü | Açıklama |
|:--- |:--- |
| [Sorgu listesi](#list-of-queries-part) |Günlük arama sorguları listesini görüntüler.  Kullanıcı sonuçlarını görüntülemek için her sorgu tıklatabilirsiniz. |
| [Sayı & listesi](#number-amp-list-part) |Üstbilgisi tek sayı gösteren kayıt sayısı günlük arama sorgusundan sahiptir.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler. |
| [İki sayı & listesi](#two-numbers-amp-list-part) |Üstbilgisi ayrı günlük arama sorgularından kayıt sayısını gösteren iki sayının sahiptir.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler. |
| [Halka & listesi](#donut-amp-list-part) |Üstbilgi, günlük sorguda değer sütundan özetlenen tek bir sayı görüntüler.  Halka grafik üst üç kayıt sonuçlarını görüntüler. |
| [İki zaman çizelgelerini & listesi](#two-timelines-amp-list-part) |Üstbilgi günlük sorguda değer sütundan özetlenen tek bir sayı görüntüleme belirtme çizgisi içeren sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler. |
| [Bilgi](#information-part) |Üstbilgi statik metin ve isteğe bağlı bir bağlantı gösterir.  Bir veya daha fazla öğeleriyle statik metin ve başlık görüntüler. |
| [Çizgi grafiği, belirtme çizgisi & listesi](#line-chart-callout-amp-list-part) |Üstbilgi saati ve bir belirtme çizgisi özetlenen bir değerle üzerinden birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler. |
| [Çizgi grafiği & listesi](#line-chart-amp-list-part) |Üstbilgi zaman içinde birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler. |
| [Çizgi grafikler bölümü yığını](#stack-of-line-charts-part) |Birden fazla seri günlük sorgudan üç ayrı çizgi grafiklerde zaman içinde görüntüler. |

## <a name="list-of-queries-part"></a>Sorguları bölümü listesi
Günlük arama sorguları listesini görüntüler.  Kullanıcı sonuçlarını görüntülemek için her sorgu tıklatabilirsiniz.  Varsayılan olarak tek bir sorgu görünümü içerir ve tıklayabilirsiniz **+ sorgu** ek sorgular eklemek için.

![Sorgular görünümünü listesi](media/log-analytics-view-designer/view-list-queries.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Başlık |Görünüm üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Önceden seçilen filtreleri |Virgülle ayrılmış kullanıcı bir sorgu seçtiğinde sol Filtre Bölmesi'nde dahil edilecek özellik listesi. |
| Mod işleme |Sorgu seçildiğinde görüntülenen ilk görüntüleyin.  Kullanıcı, sorgu açtıktan sonra tüm kullanılabilir görünümleri seçebilirsiniz. |
| **Sorgular** | |
| Arama sorgusu |Çalıştırılacak sorgu. |
| Kolay ad |Kullanıcıya görüntülenecek sorgusunun açıklayıcı adı. |

## <a name="number--list-part"></a>Sayı & listesi bölümü
Üstbilgisi tek sayı gösteren kayıt sayısı günlük arama sorgusundan sahiptir.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler.

![Sorgular görünümünü listesi](media/log-analytics-view-designer/view-number-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünüm üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **Başlık** | |
| Gösterge |Üstbilgi üstünde görüntülenecek metin. |
| Sorgu |Başlığı için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sonuçları ilk on kayıtlar için ilk iki özellikleri görüntülenir.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Çubukları, sayısal sütun göreli değerine bağlı olarak otomatik olarak oluşturulur.<br><br>Listedeki kayıtları sıralamak için sorguda sıralama komutunu kullanın.  Kullanıcı sorgu çalıştırmak ve tüm kayıtları döndürmek için bkz: tüm tıklatabilirsiniz. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırılamıyor istiyorsanız, tek karakter sınırlayıcısı.  Bkz: [ortak ayarları](#name-value-separator) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="two-numbers--list-part"></a>İki sayı & Liste bölümü
Üstbilgisi ayrı günlük arama sorgularından kayıt sayısını gösteren iki sayının sahiptir.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler.

![İki sayı & Liste Görünümü](media/log-analytics-view-designer/view-two-numbers-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünüm üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **Başlık** | |
| Gösterge |Üstbilgi üstünde görüntülenecek metin. |
| Sorgu |Başlığı için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sonuçları ilk on kayıtlar için ilk iki özellikleri görüntülenir.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Çubukları, sayısal sütun göreli değerine bağlı olarak otomatik olarak oluşturulur.<br><br>Listedeki kayıtları sıralamak için sorguda sıralama komutunu kullanın.  Kullanıcı sorgu çalıştırmak ve tüm kayıtları döndürmek için bkz: tüm tıklatabilirsiniz. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırılamıyor istiyorsanız, tek karakter sınırlayıcısı.  Bkz: [ortak ayarları](#name-value-separator) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="donut--list-part"></a>Halka & listesi bölümü
Üstbilgi, günlük sorguda değer sütundan özetlenen tek bir sayı görüntüler.  Halka grafik üst üç kayıt sonuçlarını görüntüler.

![Halka & Liste Görünümü](media/log-analytics-view-designer/view-donut-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenecek metin. |
| Alt Başlığı |Üstbilginin üst başlığı altında görüntülenecek metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır. |
| **Halka** |**> Merkezi** |
| Metin |Halka içindeki değeri altında görüntülenecek metin. |
| İşlem |Tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-TOPLA: tüm kayıtların değerlerini ekleyin.<br>-Yüzdesi: Yüzde değerleri tarafından döndürülen kayıt **neden merkezi işleminde kullanılan değerleri** sorguda toplam kayıtları için. |
| Merkezi işleminde kullanılan sonuç değerleri |İsteğe bağlı olarak bir veya daha fazla değer eklemek için artı işaretine tıklayın.  Sorgu sonuçlarını belirttiğiniz özellik değerlerini kayıtlarıyla sınırlı olacaktır.  Hiçbir değer eklediyseniz, tüm kayıtları sorguda dahil edilir. |
| **Ek Seçenekler** |**> Renkleri** |
| Rengi 1<br>Renk 2<br>Rengi 3 |Her bir halka görüntülenen değerler için rengi seçin. |
| **Ek Seçenekler** |**> Gelişmiş renk eşleme** |
| Alan değeri |Halka eklenirse farklı bir renk olarak görüntülenecek bir alanın adını yazın. |
| Renk |Benzersiz alan rengini seçin. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırılamıyor istiyorsanız, tek karakter sınırlayıcısı.  Bkz: [ortak ayarları](#name-value-separator) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="two-timelines--list-part"></a>İki zaman çizelgelerini & listesi bölümü
Üstbilgi günlük sorguda değer sütundan özetlenen tek bir sayı görüntüleme belirtme çizgisi içeren sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler.

![İki zaman çizelgelerini & listesini görüntüleyin](media/log-analytics-view-designer/view-two-timelines-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **İlk grafik<br>ikinci grafik** | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenecek metin. |
| Renk |Serideki sütunlar için kullanılacak rengi. |
| Sorgu |İlk seri için çalıştırılacak sorgu.  Her zaman aralığı içindeki kayıtları sayısını grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-TOPLA: Tüm kayıtları değerinden toplamı.<br>-Ortalama: Tüm kayıtları değerinden ortalama.<br>-Son örnek: Grafikte dahil son aralığında değer.<br>-İlk örneği: Grafikte dahil ilk aralığı değeri.<br>-Count: Sorgu tarafından döndürülen tüm kayıtları sayısı. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="information-part"></a>Bilgi bölümü
Üstbilgi statik metin ve isteğe bağlı bir bağlantı gösterir.  Bir veya daha fazla öğeleriyle statik metin ve başlık görüntüler.

![Bilgileri görüntüle](media/log-analytics-view-designer/view-information.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Renk |Başlık arka plan rengi. |
| **Üstbilgi** | |
| Görüntü |Üst bilgi görüntülemek için görüntü dosyası. |
| Etiket |Üst bilgi görüntülenecek metin. |
| **Üstbilgi** |**> Bağlantı** |
| Etiket |Bağlantı metni. |
| Url |Bağlantı için URL. |
| **Bilgi öğeleri** | |
| Başlık |Her öğe başlığı için görüntülenecek metin. |
| İçerik |Her öğe için görüntülenecek metin. |

## <a name="line-chart-callout--list-part"></a>Çizgi grafiği, belirtme çizgisi & Liste bölümü
Üstbilgi saati ve bir belirtme çizgisi özetlenen bir değerle üzerinden birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler.

![Çizgi grafiği, belirtme çizgisi & Liste Görünümü](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenecek metin. |
| Alt Başlığı |Üstbilginin üst başlığı altında görüntülenecek metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü.  Sorgu kullanıyorsa **aralığı** anahtar sözcüğü sonra grafiğin x ekseni bu zaman aralığı kullanır.  Sorgu içermiyorsa **aralığı** anahtar sözcüğü sonra saatlik aralıklarla x ekseni için kullanılır. |
| **Çizgi grafiği** |**> Belirtme** |
| Belirtme çizgisi başlık |Belirtme çizgisi değeri görüntülenecek metin. |
| Seri adı |Belirtme çizgisi değeri kullanmak seri için özellik değeri.  Seri sağlanırsa, sorgudaki tüm kayıtları kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-Ortalama: Tüm kayıtları değerinden ortalama.<br>-Sorgu tarafından döndürülen tüm kayıtları Count sayısı.<br>-Son örnek: Grafikte dahil son aralığında değer.<br>-Max: Grafikte dahil aralıkları maksimum değeri.<br>-Min: Grafikte dahil aralıkları Minimum değeri.<br>-TOPLA: Tüm kayıtları değerinden toplamı. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmayı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin.  Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır.  Birim türü birim kategorisini belirtir ve kullanılabilir geçerli birim türü değerleri tanımlar.  Bir değer seçerseniz sayısal değerleri dönüştürme türü için geçerli birim türünden dönüştürülür sonra dönüştürülemiyor. |
| Özel Etiket |Unit türü etiketi yanındaki Y ekseni için görüntülenecek metin.  Hiçbir etiket belirtilirse, yalnızca birim türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırılamıyor istiyorsanız, tek karakter sınırlayıcısı.  Bkz: [ortak ayarları](#name-value-separator) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="line-chart--list-part"></a>Satır Grafiği & listesi bölümü
Üstbilgi zaman içinde birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler.  En iyi on sonuç sorgudan ile sayısal bir sütun veya zaman içinde değişiklik göreli değerini gösteren bir grafik görüntüler.

![Satır Grafiği & Liste Görünümü](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| Simgesini kullanın |Simge görüntü seçin. |
| **Üstbilgi** | |
| Başlık |Üstbilgi üstünde görüntülenecek metin. |
| Alt Başlığı |Üstbilginin üst başlığı altında görüntülenecek metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü.  Sorgu kullanıyorsa **aralığı** anahtar sözcüğü sonra grafiğin x ekseni bu zaman aralığı kullanır.  Sorgu içermiyorsa **aralığı** anahtar sözcüğü sonra saatlik aralıklarla x ekseni için kullanılır. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmayı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin.  Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır.  Birim türü birim kategorisini belirtir ve kullanılabilir geçerli birim türü değerleri tanımlar.  Bir değer seçerseniz sayısal değerleri dönüştürme türü için geçerli birim türünden dönüştürülür sonra dönüştürülemiyor. |
| Özel Etiket |Unit türü etiketi yanındaki Y ekseni için görüntülenecek metin.  Hiçbir etiket belirtilirse, yalnızca birim türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| Grafik Gizle |Sayısal sütun sağındaki grafiğe devre dışı bırakmak için seçin. |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Renk |Mini Grafikler ve çubukları rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem.  Bkz: [ortak ayarları](#sparklines) Ayrıntılar için. |
| Ad ve değer ayırıcı |Metin özelliği birden çok değer ayrıştırılamıyor istiyorsanız, tek karakter sınırlayıcısı.  Bkz: [ortak ayarları](#name-value-separator) Ayrıntılar için. |
| Gezinti sorgu |Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Bkz: [ortak ayarları](#navigation-query) Ayrıntılar için. |
| **Liste** |**> Sütun başlıkları** |
| Ad |Listesinin ilk sütunun en üstünde görüntülenecek metin. |
| Değer |İkinci sütun listesinin en üstünde görüntülenecek metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri etkinleştir |Eşikleri etkinleştirmek için seçin.  Bkz: [ortak ayarları](#thresholds) Ayrıntılar için. |

## <a name="stack-of-line-charts-part"></a>Çizgi grafikler bölümü yığını
Birden fazla seri günlük sorgudan üç ayrı çizgi grafiklerde zaman içinde görüntüler.

![Çizgi grafiklerde yığını](media/log-analytics-view-designer/view-stack-line-charts.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Döşeme üstünde görüntülenecek metin. |
| Yeni Grup |Geçerli Görünüm başlangıç görünümünde yeni bir grup oluşturmak için seçin. |
| Simgesi |Result üstbilgisinde yanındaki görüntülemek için görüntü dosyası. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**> Üstbilgisi** |
| Başlık |Grafik üstünde görüntülenecek metin. |
| Alt Başlığı |Grafik üstündeki başlığı altında görüntülenecek metin. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**Çizgi grafiği** |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü.  Sorgu kullanıyorsa **aralığı** anahtar sözcüğü sonra grafiğin x ekseni bu zaman aralığı kullanır.  Sorgu içermiyorsa **aralığı** anahtar sözcüğü sonra saatlik aralıklarla x ekseni için kullanılır. |
| **Grafik** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmayı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin.  Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır.  Birim türü birim kategorisini belirtir ve kullanılabilir geçerli birim türü değerleri tanımlar.  Bir değer seçerseniz sayısal değerleri dönüştürme türü için geçerli birim türünden dönüştürülür sonra dönüştürülemiyor. |
| Özel Etiket |Unit türü etiketi yanındaki Y ekseni için görüntülenecek metin.  Hiçbir etiket belirtilirse, yalnızca birim türü görüntülenir. |

## <a name="common-settings"></a>Genel ayarları
Aşağıdaki bölümlerde, çeşitli görselleştirme bölümleri için genel ayarları açıklanmaktadır.

### <a name="name-value-separator">Ad ve değer ayırıcı</a>
Bir liste sorgusu metin özelliğinden birden çok değer ayrıştırma istiyorsanız, tek karakter sınırlayıcısı.  Bir sınırlayıcı belirtirseniz, ad kutusuna aynı sınırlayıcı ayırarak her bir alan adları sağlayabilir.

Örneğin, adlı bir özellik göz önünde bulundurun *konumu* gibi değerler dahil *Redmond yapı 41* ve *Bellevue Building12*.  – Ad ve değer ayırıcı için belirttiğiniz ve *Şehir yapı* adı.  Bu her değeri adlı iki özellik ayrıştırın *Şehir* ve *yapı*.

### <a name="navigation-query">Gezinti sorgu</a>
Kullanıcı bir öğeyi listeden seçtiğinde çalıştırılacak sorgu.  Kullanım *{seçili öğe}* kullanıcı seçili öğe için söz dizimi dahil etmek için.

Sorgu adlı bir sütun varsa, örneğin, *bilgisayar* ve gezinti sorgu *{seçili öğe}*, gibi bir sorgu *bilgisayar "Bilgisayarım" =* kullanıcı bir bilgisayar seçildiğinde çalıştırılır.  Gezinti sorgu ise *türü olay {seçili öğe} =* ardından sorgu *türü olay bilgisayar = "Bilgisayarım" =* çalıştırılması.

### <a name="sparklines">Mini Grafikler</a>
Bir mini grafik zaman içinde bir liste girdisi değerini gösteren bir küçük çizgi grafiktir.  Bir liste görselleştirme bölümleri için yatay bir çubukla sayısal bir sütun veya zamanla değerini gösteren bir mini göreli değerini belirten görüntülenip görüntülenmeyeceğini seçebilirsiniz.

Aşağıdaki tabloda Mini Grafikler için ayarları açıklanır.

| Ayar | Açıklama |
|:--- |:--- |
| Mini Grafikler etkinleştir |Mini Grafik yatay çubuk yerine görüntülemek için seçin. |
| İşlem |Mini Grafikler etkinleştirilirse, Mini Grafik için değerleri hesaplamak için listedeki her bir özellik üzerinde gerçekleştirilecek işlem budur.<br><br>-Son örnek: seri için değer zaman aralığı içinde son.<br>-Max: Seri zaman aralığında maksimum değeri.<br>-Min: Seri zaman aralığında Minimum değeri.<br>-TOPLA: Zaman aralığında serisi değerlerinin toplamı.<br>-Özeti: aynı ölçü komut üstbilgisinde sorgu olarak kullanır. |

### <a name="thresholds">Eşikleri</a>
Eşikleri, belirli bir değeri aşması veya belirli bir aralıkta öğelerin hızlı görsel bir gösterge vermiş listedeki her öğesinin yanındaki renkli bir simge görüntülemek izin verir.  Örneğin, bir hata değeri aşarsa, kabul edilebilir değer, bir uyarı gösterir bir aralıkta değerse sarı ve kırmızı ile öğeleri için yeşil bir simge görüntüleyebilirsiniz.

Bir bölümü için eşikler etkinleştirdiğinizde, bir veya daha fazla eşikleri belirtmeniz gerekir.  Bir öğenin değeri eşik değerden daha büyük ve sonraki eşik değerden daha düşük ise, bu renk kullanılır.  Öğe sonra yüksek eşik değerinden büyükse, bu rengi ayarlanır.   

Bir eşik değerini her eşik ayarlanmış **varsayılan**.  Renk diğer değerler aşılırsa kümesi budur.  Ekleyip tıklatarak eşikleri kaldırdığınızda  **+**  veya **x** düğmesi.

Aşağıdaki tabloda tresholds için ayarları açıklanır.

| Ayar | Açıklama |
|:--- |:--- |
| Eşikleri etkinleştir |Belirtilen eşikler göre sistem durumunu gösteren her değer solundaki renkli bir simge görüntülemek için seçin. |
| Ad |Eşik değeri belirlemek için ad. |
| Eşik |Eşik değeri.  Her liste öğesi için sistem durumu rengi renk tarafından öğenin değeri aşıldı yüksek eşik değerinin ayarlanır.  Eşik değerleri aşılırsa, renk bir varsayılan eşik yoktur. |
| Renk |Eşik değeri rengi. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) sorguları görselleştirme bölümlerinde desteklemek için.
