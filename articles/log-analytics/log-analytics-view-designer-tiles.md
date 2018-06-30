---
title: Azure günlük analizi Görünüm Tasarımcısı döşemeleri Başvuru Kılavuzu | Microsoft Docs
description: Günlük analizi Görünüm Tasarımcısı kullanarak veri görselleştirmeleri çeşitli günlük analizi çalışma alanınızda görüntülemek Azure Portalı'nda özel görünümler oluşturabilirsiniz. Bu makalede, ayarları özel görünümlerde kullanılabilir döşeme için bir başvuru kılavuzdur.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: fc5dc00a05486c1f781016df63877f40d21b0205
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131279"
---
# <a name="reference-guide-to-view-designer-tiles-in-log-analytics"></a>Günlük analizi Görünüm Tasarımcısı döşemeleri Başvuru Kılavuzu
Azure günlük analizi Görünüm Tasarımcısı kullanarak veri görselleştirmeleri günlük analizi çalışma alanınızdaki çeşitli sunmak Azure Portalı'nda özel görünümler oluşturabilirsiniz. Bu makalede, ayarları özel görünümlerde kullanılabilir döşeme için bir başvuru kılavuzdur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Görüntüleme Tasarımcısı](log-analytics-view-designer.md): oluşturma ve özel görünümler düzenleme Görünüm Tasarımcısı ve yordamları genel bakış sağlar.
* [Görselleştirme bölümü başvuru](log-analytics-view-designer-parts.md): başvuru kılavuzu için özel görünümlerde kullanılabilir görselleştirme bölümleri ayarlar sağlar.


Kullanılabilir Görünüm Tasarımcısı döşeme aşağıdaki tabloda açıklanmıştır:  

| Kutucuk | Açıklama |
|:--- |:--- |
| [Sayı](#number-tile) |Sorgudan kayıt sayısı. |
| [İki sayı](#two-numbers-tile) |İki farklı sorgular kayıtları sayar. |
| [Halka](#donut-tile) | Merkezi'ndeki Özet bir değerle bir sorgu dayalı bir grafik. |
| [Çizgi grafiği ve belirtme](#line-chart-amp-callout-tile) | Bir sorgu ve bir Özet değerle belirtme çizgisi temel çizgi grafiği. |
| [Çizgi grafiği](#line-chart-tile) |Sorgu temelli bir çizgi grafiği. |
| [İki zaman çizelgeleri](#two-timelines-tile) | İki serinin içeren bir sütun grafik, her ayrı bir sorguyu temel. |

Sonraki bölümlerde kutucuğu türleri ve özelliklerinin ayrıntılı açıklanmaktadır.

## <a name="number-tile"></a>Sayı döşeme
**Numarası** kutucuğu, her iki günlük sorgu ve etiket kayıtları sayısını görüntüler.

![Sayı döşeme](media/log-analytics-view-designer/tile-number.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Döşeme** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="two-numbers-tile"></a>İki sayı döşeme
Bu kutucuğu her iki farklı günlük sorgular ve etiket kayıtları sayısını görüntüler.

![İki sayı döşeme](media/log-analytics-view-designer/tile-two-numbers.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **İlk döşeme** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| **İkinci döşeme** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıt sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="donut-tile"></a>Halka döşeme
**Halka** kutucuğu günlük sorgu değer sütununda özetlenir tek bir sayı görüntüler. Halka grafik üst üç kayıt sonuçlarını görüntüler.

![Halka döşeme](media/log-analytics-view-designer/tile-donut.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Halka** | |
| Sorgu |Halka için Çalıştır sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. |
| **Halka** |**> Merkezi** |
| Metin |Halka içindeki değeri altında görüntülenen metin. |
| İşlem |Tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Toplam: tüm kayıtları değerleri özellik değeriyle ekleyin.</li><li>Yüzdesi: Tüm kayıtları toplanan değerleriyle karşılaştırılan özellik değeri toplanan değerlerle kayıtlarından yüzdesi.</li></ul> |
| Merkezdeki işlemde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işareti (+) seçin. Sorgu sonuçlarını kayıtları belirttiğiniz özellik değerleri ile sınırlıdır. Hiçbir değer eklediyseniz, tüm kayıtları sorguda dahil edilir. |
| **Halka** |**> Ek seçenekler** |
| Renkler |Her üç üst özellikleri için görüntülenen rengi. Belirli özellik değerleri için alternatif renkleri belirtmek için kullanın *gelişmiş renk eşleme*. |
| Gelişmiş Renk Eşleme |Belirli özellik değerlerini temsil eden bir renk görüntüler. Belirttiğiniz değerle ilk üç ise, alternatif renk yerine standart bir renk görüntülenir. Özelliği ilk üç durumda değilse, renk görüntülenmez. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="line-chart-tile"></a>Satır grafiği döşeme
Bu kutucuğu zaman içinde bir günlük sorgudan birden fazla seri görüntüleyen bir çizgi grafiği ' dir. 

![Satır grafiği ve belirtme çizgisi bölmesi](media/log-analytics-view-designer/tile-line-chart.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için Çalıştır sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseni bu zaman aralığı kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, x ekseni kullanır saatlik aralıklarla. |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Logaritmik ölçek y ekseni için kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değer birimlerini belirtin. Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerleri dönüştürmek için etiketleri görüntülemek için kullanılır. **Birim türü** birim kategorisini belirtir ve tanımlar **geçerli birim türü** kullanılabilir değerler. Bir değer seçerseniz **dönüştürmek** gelen sayısal değerleri dönüştürülür sonra **geçerli birim** için yazın **dönüştürmek** türü. |
| Özel Etiket |Y eksenine etiketi yanında görüntülenen metin *birim* türü. Etiket yok, yalnızca belirtilmişse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="line-chart-and-callout-tile"></a>Satır grafiği ve belirtme çizgisi bölmesi
Bu kutucuğu zaman ve özetlenen bir değerle belirtme çizgisi üzerinde birden fazla seri günlükten sorgu görüntüleyen grafik hem bir satır var. 

![Satır grafiği ve belirtme çizgisi bölmesi](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| **Çizgi grafiği** | |
| Sorgu |Çizgi grafiği için Çalıştır sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseni bu zaman aralığı kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, x ekseni kullanır saatlik aralıklarla. |
| **Çizgi grafiği** |**> Belirtme** |
| Belirtme çizgisi başlık | Belirtme çizgisi değerin görüntülenen metin. |
| Seri adı |Belirtme çizgisi değeri olarak kullanılacak seri özellik değeri. Seri sağlanırsa, sorgudaki tüm kayıtları kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtları değerleri ortalama.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>En son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: En yüksek değer grafikte dahil edilen aralık.</li><li>En küçük: En düşük değer grafikte dahil edilen aralık.</li><li>Toplam: Tüm kayıtları değerleri toplamı.</li></ul> |
| **Çizgi grafiği** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Logaritmik ölçek y ekseni için kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen birimler için değerleri belirtin. Bu bilgiler değer türleri gösteren görüntü grafik etiketleri ve, isteğe bağlı olarak, için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *dönüştürmek*, gelen sayısal değerleri dönüştürülür *geçerli birim* için yazın *dönüştürmek* türü. |
| Özel Etiket |Y eksenine etiketi yanında görüntülenen metin *birim* türü. Etiket yok, yalnızca belirtilmişse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="two-timelines-tile"></a>İki zaman çizelgelerini döşeme
**İki zaman çizelgelerini** kutucuğu sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler. Belirtme çizgisi her seri için görüntülenir. 

![İki zaman çizelgelerini döşeme](media/log-analytics-view-designer/tile-two-timelines.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Döşeme üstünde görüntülenen metin. |
| Açıklama |Döşeme adı altında görüntülenen metin. |
| İlk Grafik | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenen metin. |
| Renk |İlk serisindeki sütunlar için kullanılan rengi. |
| Grafik sorgu |İlk seri için Çalıştır sorgu. Her zaman aralığı üzerinden kayıt sayısı grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtları değerleri ortalama.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>En son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: En yüksek değer grafikte dahil edilen aralık.</li></ul> |
| **İkinci grafik** | |
| Gösterge |İkinci seri için belirtme çizgisi altında görüntülenen metin. |
| Renk |İkinci serideki sütunlar için kullanılan rengi. |
| Grafik Sorgusu |İkinci seri için Çalıştır sorgu. Her zaman aralığı üzerinden kayıt sayısı grafik sütunlara göre temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtları değerleri ortalama.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>En son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: En yüksek değer grafikte dahil edilen aralık. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama bölme için etkinleştirilmesi gerekir, bu bağlantıyı seçin. Veriler kullanılamıyorsa, bu yaklaşımın bir alternatif mesajı sağlar. Normalde zaman görünümü yüklenir ve veriler kullanılabilir duruma geçici süresi boyunca bir ileti sağlamak için yaklaşımı kullanın. |
| Sorgu |Veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın sorgu. Sorgu hiç sonuç döndürürse, ana sorgu değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgu hiç veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız, bir *gerçekleştirme değerlendirme* durum iletisi görüntülenir. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum aramaları](log-analytics-log-searches.md) sorguları döşemeleri desteklemek için.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünüm.
