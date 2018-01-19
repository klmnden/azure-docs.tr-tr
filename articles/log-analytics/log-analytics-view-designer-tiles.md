---
title: "Görünüm Tasarımcısı'nda Azure günlük analizi için başvuru döşeme | Microsoft Docs"
description: "Görünüm Tasarımcısı'nda günlük analizi veri günlük analizi çalışma alanınızdaki farklı görsel öğeleri içeren Azure portalında özel görünümler oluşturmanıza olanak sağlar. Bu makalede her özel görünümlerde kullanılabilir döşeme ayarlarını bir başvuru sağlar."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: 9512a3f45ba6b03af4b0c9bee444948381f4fdcb
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="log-analytics-view-designer-tile-reference"></a>Günlük analizi Görünüm Tasarımcısı döşeme başvurusu
Görünüm Tasarımcısı'nda günlük analizi veri günlük analizi çalışma alanınızdaki farklı görsel öğeleri içeren Azure portalında özel görünümler oluşturmanıza olanak sağlar. Bu makalede her özel görünümlerde kullanılabilir döşeme ayarlarını bir başvuru sağlar.

Görünüm Tasarımcısı için kullanılabilir diğer makaleler şunlardır:

* [Görüntüleme Tasarımcısı](log-analytics-view-designer.md) -yordamlarına oluşturma ve düzenleme özel görünümleri ve Görünüm Tasarımcısı genel bakış.
* [Görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md) -her özel görünümlerde kullanılabilir döşeme ayarlarını başvuru.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), tüm görünümleri sorgularda yazılmalıdır sonra [yeni sorgu dili](https://go.microsoft.com/fwlink/?linkid=856078).  Çalışma alanı yükseltilmeden önce oluşturulan görünümleri dönüştürülen automtically olacaktır.

Aşağıdaki tabloda döşeme görünümü Tasarımcısı'nda kullanılabilir farklı türlerini listeler.  Aşağıdaki bölümler her döşeme türü ayrıntı ve bunların özelliklerini açıklar.

| Kutucuk | Açıklama |
|:--- |:--- |
| [Sayı](#number-tile) |Bir sorgu ndeki kayıtları sayısını gösteren tek bir sayı. |
| [İki sayı](#two-numbers-tile) |İki farklı sorgular kayıtları sayısını gösteren iki tek sayı. |
| [Halka](#donut-tile) |Halka grafiği Merkezi'ndeki Özet bir değeri ile bir sorguyu temel. |
| [Çizgi grafiği & belirtme](#line-chart-amp-callout-tile) |Bir sorgu ve bir Özet değerle belirtme çizgisi temel çizgi grafiği. |
| [Çizgi grafiği](#line-chart-tile) |Bir sorguyu temel çizgi grafiği. |
| [İki zaman çizelgeleri](#two-timelines-tile) |Sütun grafiği her ayrı bir sorguyu temel iki dizi. |

## <a name="number-tile"></a>Sayı döşeme
**Numarası** kutucuğu günlük sorgu ve etiket kayıtları sayısını gösteren tek bir sayı görüntüler.

![Sayı döşeme](media/log-analytics-view-designer/tile-number.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| **Döşeme** | |
| Gösterge |Değerin altında görüntülenecek metin. |
| Sorgu |Çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="two-numbers-tile"></a>İki sayı döşeme
**İki sayı** kutucuğu, her biri için iki farklı günlük sorgular ve etiket kayıtlarından sayıyı gösteren iki sayı görüntüler.

![İki sayı döşeme](media/log-analytics-view-designer/tile-two-numbers.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| **İlk döşeme** | |
| Gösterge |Değerin altında görüntülenecek metin. |
| Sorgu |Çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| **İkinci döşeme** | |
| Gösterge |Değerin altında görüntülenecek metin. |
| Sorgu |Çalıştırılacak sorgu.  Sorgu tarafından döndürülen kayıt sayısını görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="donut-tile"></a>Halka döşeme
**Halka** kutucuğu günlük sorguda değer sütundan özetlenen tek bir sayı görüntüler.  Halka grafik üst üç kayıt sonuçlarını görüntüler.

![Halka döşeme](media/log-analytics-view-designer/tile-donut.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü. |
| **Halka** |**> Center** |
| Metin |Halka içindeki değeri altında görüntülenecek metin. |
| İşlem |Tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-TOPLA: tüm kayıtları değerleri özellik değeriyle ekleyin.<br>-Yüzdesi: Yüzdesini kayıtlardan toplanan değerler tüm kayıtları toplanan değerleriyle karşılaştırılan özellik değerine sahip. |
| Merkezi işleminde kullanılan sonuç değerleri |İsteğe bağlı olarak bir veya daha fazla değer eklemek için artı işaretine tıklayın.  Sorgu sonuçlarını belirttiğiniz özellik değerlerini kayıtlarıyla sınırlı olacaktır.  Hiçbir değer eklediyseniz, tüm kayıtları sorguda dahil edilir. |
| **Halka** |**> Ek seçenekler** |
| Renkleri |Her üç üst özellik için görüntülenecek rengi.  Belirli özellik değerleri için alternatif renkleri belirtmek istiyorsanız, gelişmiş renk eşleme kullanın. |
| Gelişmiş renk eşleme |Belirli özellik değerleri için renk görüntüler.  Belirttiğiniz değer üst üç ise, alternatif renk yerine standart bir renk görüntülenir.  Özelliği üst üç durumda değilse, renk görüntülenmez. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="line-chart-tile"></a>Satır grafiği döşeme
**Çizgi grafiği** kutucuğu, zaman içinde bir günlük sorgudan birden fazla seri ile bir çizgi grafiği görüntüler.  

![Çizgi grafiği & belirtme çizgisi döşeme](media/log-analytics-view-designer/tile-line-chart.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü.  Sorgu kullanıyorsa **aralığı** anahtar sözcüğü sonra grafiğin x ekseni bu zaman aralığı kullanır.  Sorgu içermiyorsa **aralığı** anahtar sözcüğü sonra saatlik aralıklarla x ekseni için kullanılır. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmayı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin.  Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır.  **Birim türü** birim kategorisini belirtir ve tanımlar **geçerli birim türü** kullanılabilir değerler.  Bir değer seçerseniz **dönüştürmek** gelen sayısal değerleri dönüştürülür sonra **geçerli birim** için yazın **dönüştürmek** türü. |
| Özel Etiket |Unit türü etiketi yanındaki Y ekseni için görüntülenecek metin.  Hiçbir etiket belirtilirse, yalnızca birim türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="line-chart--callout-tile"></a>Satır Grafiği & belirtme çizgisi döşeme
**Çizgi grafiği & belirtme çizgisi** kutucuğu, saat ve özetlenen bir değerle belirtme çizgisi üzerinde birden fazla dizi günlük sorgudan bir çizgi grafiği görüntüler.  

![Çizgi grafiği & belirtme çizgisi döşeme](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu.  İlk özelliği, bir metin değeri ve ikinci özelliğini sayısal bir değer olmalıdır.  Bu genellikle kullanan bir sorgu olur **ölçü** sonuçları özetlemek için anahtar sözcüğü.  Sorgu kullanıyorsa **aralığı** anahtar sözcüğü sonra grafiğin x ekseni bu zaman aralığı kullanır.  Sorgu içermiyorsa **aralığı** anahtar sözcüğü sonra saatlik aralıklarla x ekseni için kullanılır. |
| **Çizgi grafiği** |**> Belirtme** |
| Belirtme çizgisi |Belirtme çizgisi değerini görüntülemek için başlık metni. |
| Seri adı |Belirtme çizgisi değeri kullanmak seri için özellik değeri.  Seri sağlanırsa, sorgudaki tüm kayıtları kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br>-Ortalama: Tüm kayıtları değerinden ortalama.<br><br>-Count: Sorgu tarafından döndürülen tüm kayıtları sayısı.<br>-Son örnek: Grafikte dahil son aralığında değer.<br>-Max: Grafikte dahil aralıkları maksimum değeri.<br>-Min: Grafikte dahil aralıkları Minimum değeri.<br>-TOPLA: Tüm kayıtları değerinden toplamı. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik ölçek kullan |Logaritmik ölçek y ekseni için kullanmayı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin.  Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır.  **Birim türü** birim kategorisini belirtir ve tanımlar **geçerli birim türü** kullanılabilir değerler.  Bir değer seçerseniz **dönüştürmek** gelen sayısal değerleri dönüştürülür sonra **geçerli birim** için yazın **dönüştürmek** türü. |
| Özel Etiket |Unit türü etiketi yanındaki Y ekseni için görüntülenecek metin.  Hiçbir etiket belirtilirse, yalnızca birim türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="two-timelines-tile"></a>İki zaman çizelgelerini döşeme
**İki zaman çizelgelerini** kutucuğu sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler.  Belirtme çizgisi her seri için görüntülenir.  

![İki zaman çizelgelerini döşeme](media/log-analytics-view-designer/tile-two-timelines.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenecek metin. |
| Açıklama |Döşeme adla görüntülenecek metin. |
| İlk grafik | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenecek metin. |
| Renk |İlk serisindeki sütunlar için kullanılacak rengi. |
| Grafik sorgu |İlk seri için çalıştırılacak sorgu.  Her zaman aralığı içindeki kayıtları sayısını grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-Ortalama: Tüm kayıtları değerinden ortalama.<br>-Count: Sorgu tarafından döndürülen tüm kayıtları sayısı.<br>-Son örnek: Grafikte dahil son aralığında değer.<br>-Max: Grafikte dahil aralıkları maksimum değeri. |
| **İkinci grafik** | |
| Gösterge |İkinci seri için belirtme çizgisi altında görüntülenecek metin. |
| Renk |İkinci serideki sütunlar için kullanılacak rengi. |
| Grafik sorgu |İkinci seri için çalıştırılacak sorgu.  Her zaman aralığı içindeki kayıtları sayısını grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<br><br>-Ortalama: Tüm kayıtları değerinden ortalama.<br>-Count: Sorgu tarafından döndürülen tüm kayıtları sayısı.<br>-Son örnek: Grafikte dahil son aralığında değer.<br>-Max: Grafikte dahil aralıkları maksimum değeri. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama için döşeme etkinleştirilmesi gerekiyorsa seçin.  Bu, veri bölme için kullanılabilir değilse, alternatif bir ileti sağlar.  Bu, genellikle zaman görünümü yüklenir ve veriler kullanılabilir gelir geçici süresi boyunca bir ileti sağlamak için kullanılır. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını denetlemek için çalıştırılacak sorgu.  Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenecek ileti.  İleti yok, sağlarsanız, *gerçekleştirme değerlendirme* görüntülenir. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) sorguları döşemeleri desteklemek için.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünüm.
