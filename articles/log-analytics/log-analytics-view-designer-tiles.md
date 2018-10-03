---
title: Azure Log analytics'te Görünüm Tasarımcısı kutucukları bir başvuru kılavuzu | Microsoft Docs
description: Log Analytics'te görünüm Tasarımcısını kullanarak, Azure portalında Log Analytics çalışma alanınızda veri görselleştirmeleri çeşitli görüntüleyen özel görünümlerinizi oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir kutucuk ayarlarını bir başvuru kılavuzudur.
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
ms.component: ''
ms.openlocfilehash: 1c9c93c198b4d88da55ecd241e096e73e4a40d5d
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2018
ms.locfileid: "48042198"
---
# <a name="reference-guide-to-view-designer-tiles-in-log-analytics"></a>Görünüm Tasarımcısı kutucukları Log analytics'te Başvuru Kılavuzu
Azure Log Analytics'te Görünüm Tasarımcısı kullanarak veri görselleştirmeleri Log Analytics çalışma alanınızdaki çeşitli sunan Azure portalında özel görünümlerinizi oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir kutucuk ayarlarını bir başvuru kılavuzudur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Görüntüleme Tasarımcısı](log-analytics-view-designer.md): oluşturma ve düzenleme özel görünümler için Görünüm Tasarımcısı ve yordamları hakkında genel bir bakış sağlar.
* [Görselleştirme bölümü başvurusu](log-analytics-view-designer-parts.md): bir başvuru kılavuzu için özel görünümlerinizi kullanılabilir görselleştirme bölümleri ayarlarına sağlar.


Kullanılabilir Görünüm Tasarımcısı kutucuklar, aşağıdaki tabloda açıklanmıştır:  

| Kutucuk | Açıklama |
|:--- |:--- |
| [Sayı](#number-tile) |Bir sorgunun kayıt sayısı. |
| [İki sayı](#two-numbers-tile) |İki farklı sorgular kayıtlarını sayar. |
| [Halka](#donut-tile) | Merkezinde Özet bir değerle bir sorgu temel alan grafiği. |
| [Çizgi grafik ve belirtme çizgisi](#line-chart-amp-callout-tile) | Bir sorgu ve belirtme çizgisi Özet bir değerle temel alan bir çizgi grafiği. |
| [Çizgi grafik](#line-chart-tile) |Bir sorguyu temel alan bir çizgi grafiği. |
| [İki zaman çizelgesi](#two-timelines-tile) | İki dizi sütun grafiği, her bir ayrı bir sorguyu temel alan. |

Sonraki bölümlerde, kutucuk türleri ve bunların özelliklerini ayrıntılı açıklanmaktadır.

## <a name="number-tile"></a>Bir sayı kutucuğu
**Numarası** kutucuk her iki günlük sorgusu ve bir etiket kayıt sayısını görüntüler.

![Bir sayı kutucuğu](media/log-analytics-view-designer/tile-number.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **kutucuğu** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Sorgu çalıştırılır. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="two-numbers-tile"></a>İki sayı kutucuğu
Bu kutucuk, her iki farklı günlük sorguları ve bir etiket kayıtları sayısını görüntüler.

![İki sayı kutucuğu](media/log-analytics-view-designer/tile-two-numbers.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **İlk kutucuk** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Sorgu çalıştırılır. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| **İkinci kutucuk** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Sorgu çalıştırılır. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="donut-tile"></a>Halka kutucuğu
**Halka** günlük sorgusu değer sütununda özetlenir tek bir sayı kutucuğu görüntülenir. Halka grafik en çok üç kayıtların sonuçlarını görüntüler.

![Halka kutucuğu](media/log-analytics-view-designer/tile-donut.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. |
| **Halka** |**> Merkezi** |
| Metin |Halka içindeki değeri altında görüntülenen metin. |
| İşlem |Tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Toplam: özellik değeri ile tüm kayıtları değerlerini ekleyin.</li><li>Yüzdesi: Tüm kayıtların toplanan değerlere kıyasla özellik değeri toplanan değerleriyle bir kayıt yüzdesi.</li></ul> |
| Merkezdeki işlemde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işaretini (+) seçin. Sorgu sonuçlarını kayıtlarını belirttiğiniz özellik değerleri ile sınırlıdır. Hiçbir değer eklediyseniz tüm kayıtları sorguya dahil edilir. |
| **Halka** |**> Ek seçenekler** |
| Renkler |Her üç üst özellik için görüntülenecek rengi. Belirli özellik değerleri için alternatif renkleri belirtmek için kullanın *gelişmiş renk eşleme*. |
| Gelişmiş Renk Eşleme |Belirli özellik değerleri temsil eden bir renk görüntüler. İlk üç belirttiğiniz değer ise alternatif rengi standart renk yerine görüntülenir. Color özelliği ilk üç değilse, görüntülenmez. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="line-chart-tile"></a>Çizgi grafiği kutucuğu
Zaman içinde birden fazla seri günlük sorgudan gösteren bir çizgi grafik kutucuğudur. 

![Çizgi grafik ve belirtme çizgisi kutucuğu](media/log-analytics-view-designer/tile-line-chart.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseninde bu zaman aralığını kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimleri belirtin. Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerlerini dönüştürmek için etiketleri görüntülemek için kullanılır. **Birim türü** birim kategorisini belirtir ve tanımlar **geçerli birim türü** kullanılabilir olan değerleri. Bir değer seçerseniz **Dönüştür** gelen sayısal değerleri dönüştürülür sonra **geçerli birim** için yazın **Dönüştür** türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="line-chart-and-callout-tile"></a>Çizgi grafik ve belirtme çizgisi kutucuğu
Bu kutucuk, saati ve bir özetlenmiş değer ile belirtme çizgisi üzerinde birden fazla seri günlükten sorgu görüntüleyen grafik hem bir satırı içeriyor. 

![Çizgi grafik ve belirtme çizgisi kutucuğu](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseninde bu zaman aralığını kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| **Çizgi grafik** |**> Açıklama balonu** |
| Belirtme çizgisi başlığı | Belirtme çizgisi değerinin üzerine görüntülenecek metin. |
| Seri adı |Belirtme çizgisi değeri olarak kullanılacak serisi özellik değeri. Hiçbir serisi sağlanırsa, sorgudaki tüm kayıtlar kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardan değerlerin ortalamasını.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıklarının maksimum değer.</li><li>En düşük: En küçük değerini grafikte yer aralıkları.</li><li>Toplam: Tüm kayıtlardan değerlerinin toplamı.</li></ul> |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimlerin belirtin. Bu bilgiler değeri türlerini belirtmek görüntü grafiğini etiketlere ve da isteğe bağlı olarak için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *Dönüştür*, sayısal değerler gelen dönüştürülür *geçerli birim* için yazın *Dönüştür* türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="two-timelines-tile"></a>İki zaman çizelgesi kutucuğu
**İki zaman çizelgesi** kutucuk sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler. Belirtme çizgisi, her bir seri için görüntülenir. 

![İki zaman çizelgesi kutucuğu](media/log-analytics-view-designer/tile-two-timelines.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| İlk Grafik | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenen metin. |
| Renk |İlk dizideki sütunlar için kullanılan renk. |
| Grafik sorgusu |İlk seri için çalıştırılan sorgu. Her zaman aralığında kayıtların sayısını grafiğin sütunları tarafından temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardan değerlerin ortalamasını.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıklarının maksimum değer.</li></ul> |
| **İkinci grafik** | |
| Gösterge |İkinci bir seri için çağrı altında görüntülenen metin. |
| Renk |Serinin ikinci sütun için kullanılan renk. |
| Grafik Sorgusu |İkinci bir seri için çalıştırılan sorgu. Her zaman aralığında kayıtların sayısını grafiğin sütunları tarafından temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtlardan değerlerin ortalamasını.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtların sayısı.</li><li>Son örnek: grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıklarının maksimum değer. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Etkin |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| İleti |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [günlük aramaları](log-analytics-log-searches.md) kutucuklar sorguları desteklemek için.
* Ekleme [görselleştirme bölümleri](log-analytics-view-designer-parts.md) , özel bir görünüm.
