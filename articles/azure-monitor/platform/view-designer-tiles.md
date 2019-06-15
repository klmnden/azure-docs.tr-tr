---
title: Görünüm Tasarımcısı kutucukları Azure İzleyici'de bir başvuru kılavuzu | Microsoft Docs
description: Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında görüntülenir ve görselleştirmeler Log Analytics çalışma alanındaki veriler üzerinde çeşitli içeren özel görünümlerinizi oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir kutucuk ayarlarını bir başvuru kılavuzudur.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 41787c8f-6c13-4520-b0d3-5d3d84fcf142
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/17/2018
ms.author: bwren
ms.openlocfilehash: 9c0283081bd7245b1b886ed82ba03130a7a3bf2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61342063"
---
# <a name="reference-guide-to-view-designer-tiles-in-azure-monitor"></a>Azure İzleyici'de Görünüm Tasarımcısı kutucukları için başvuru kılavuzu
Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında Log Analytics çalışma alanınızdaki veri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir kutucuk ayarlarını bir başvuru kılavuzudur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Görüntüleme Tasarımcısı](view-designer.md): Oluşturma ve düzenleme özel görünümler için Görünüm Tasarımcısı ve yordamları hakkında genel bir bakış sağlar.
* [Görselleştirme bölümü başvurusu](view-designer-parts.md): Bir başvuru kılavuzu için özel görünümlerinizi kullanılabilir görselleştirme bölümleri ayarlarına sağlar.


Kullanılabilir Görünüm Tasarımcısı kutucuklar, aşağıdaki tabloda açıklanmıştır:  

| kutucuğu | Açıklama |
|:--- |:--- |
| [Sayı](#number-tile) |Bir sorgunun kayıt sayısı. |
| [İki sayı](#two-numbers-tile) |İki farklı sorgular kayıtlarını sayar. |
| [Halka](#donut-tile) | Merkezinde Özet bir değerle bir sorgu temel alan grafiği. |
| Çizgi grafik ve belirtme çizgisi | Bir sorgu ve belirtme çizgisi Özet bir değerle temel alan bir çizgi grafiği. |
| [Çizgi grafik](#line-chart-tile) |Bir sorguyu temel alan bir çizgi grafiği. |
| [İki zaman çizelgesi](#two-timelines-tile) | İki dizi sütun grafiği, her bir ayrı bir sorguyu temel alan. |

Sonraki bölümlerde, kutucuk türleri ve bunların özelliklerini ayrıntılı açıklanmaktadır.

> [!NOTE]
> Görünümlerde kutucukları temel [oturum sorguları](../log-query/log-query-overview.md) Log Analytics çalışma alanınızda. Şu anda desteklemediği [çapraz kaynak sorgularını](../log-query/cross-workspace-query.md) uygulama anlayışları'ndan veri alınamadı.

## <a name="number-tile"></a>Bir sayı kutucuğu
**Numarası** kutucuk her iki günlük sorgusu ve bir etiket kayıt sayısını görüntüler.

![Bir sayı kutucuğu](media/view-designer-tiles/tile-number.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **kutucuğu** | |
| Gösterge |Değerin altında görüntülenen metin. |
| Sorgu |Sorgu çalıştırılır. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="two-numbers-tile"></a>İki sayı kutucuğu
Bu kutucuk, her iki farklı günlük sorguları ve bir etiket kayıtları sayısını görüntüler.

![İki sayı kutucuğu](media/view-designer-tiles/tile-two-numbers.png)

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
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="donut-tile"></a>Halka kutucuğu
**Halka** günlük sorgusu değer sütununda özetlenir tek bir sayı kutucuğu görüntülenir. Halka grafik en çok üç kayıtların sonuçlarını görüntüler.

![Halka kutucuğu](media/view-designer-tiles/tile-donut.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. |
| **Halka** |**> Merkezi** |
| Text |Halka içindeki değeri altında görüntülenen metin. |
| İşlem |Tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Toplama: Tüm kayıtların değerleri özellik değeriyle ekleyin.</li><li>Yüzdesi: Tüm kayıtların toplanan değerlere kıyasla özellik değeri toplanan değerleriyle bir kayıt yüzdesi.</li></ul> |
| Merkezdeki işlemde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işaretini (+) seçin. Sorgu sonuçlarını kayıtlarını belirttiğiniz özellik değerleri ile sınırlıdır. Hiçbir değer eklediyseniz tüm kayıtları sorguya dahil edilir. |
| **Halka** |**> Ek seçenekler** |
| Renkleri |Her üç üst özellik için görüntülenecek rengi. Belirli özellik değerleri için alternatif renkleri belirtmek için kullanın *gelişmiş renk eşleme*. |
| Gelişmiş renk eşleme |Belirli özellik değerleri temsil eden bir renk görüntüler. İlk üç belirttiğiniz değer ise alternatif rengi standart renk yerine görüntülenir. Color özelliği ilk üç değilse, görüntülenmez. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="line-chart-tile"></a>Çizgi grafiği kutucuğu
Zaman içinde birden fazla seri günlük sorgudan gösteren bir çizgi grafik kutucuğudur. 

![Çizgi grafik ve belirtme çizgisi kutucuğu](media/view-designer-tiles/tile-line-chart.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseninde bu zaman aralığını kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik ölçek kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimleri belirtin. Bu bilgiler, değer türleri gösteren grafik ve isteğe bağlı olarak değerlerini dönüştürmek için etiketleri görüntülemek için kullanılır. **Birim türü** birim kategorisini belirtir ve tanımlar **geçerli birim türü** kullanılabilir olan değerleri. Bir değer seçerseniz **Dönüştür** gelen sayısal değerleri dönüştürülür sonra **geçerli birim** için yazın **Dönüştür** türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="line-chart-and-callout-tile"></a>Çizgi grafik ve belirtme çizgisi kutucuğu
Bu kutucuk, saati ve bir özetlenmiş değer ile belirtme çizgisi üzerinde birden fazla seri günlükten sorgu görüntüleyen grafik hem bir satırı içeriyor. 

![Çizgi grafik ve belirtme çizgisi kutucuğu](media/view-designer-tiles/tile-line-chart-callout.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılan sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, x ekseninde bu zaman aralığını kullanır. Sorgu kullanmıyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| **Çizgi grafik** |**> Açıklama balonu** |
| Belirtme çizgisi başlığı | Belirtme çizgisi değerinin üzerine görüntülenecek metin. |
| Seri adı |Belirtme çizgisi değeri olarak kullanılacak serisi özellik değeri. Hiçbir serisi sağlanırsa, sorgudaki tüm kayıtlar kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtların değerlerin ortalaması.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>Son örnek: Grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıkları maksimum değer.</li><li>En küçük: Grafikte yer aralıkları en düşük değeri.</li><li>Toplama: Tüm kayıtların değerlerinin toplamı.</li></ul> |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik ölçek kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimlerin belirtin. Bu bilgiler değeri türlerini belirtmek görüntü grafiğini etiketlere ve da isteğe bağlı olarak için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *Dönüştür*, sayısal değerler gelen dönüştürülür *geçerli birim* için yazın *Dönüştür* türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="two-timelines-tile"></a>İki zaman çizelgesi kutucuğu
**İki zaman çizelgesi** kutucuk sütun grafikleri olarak zaman içinde iki günlük sorguların sonuçlarını görüntüler. Belirtme çizgisi, her bir seri için görüntülenir. 

![İki zaman çizelgesi kutucuğu](media/view-designer-tiles/tile-two-timelines.png)

| Ayar | Açıklama |
|:--- |:--- |
| Ad |Kutucuğun üst kısmında görüntülenen metin. |
| Açıklama |Kutucuk adı altında görüntülenen metin. |
| İlk grafik | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenen metin. |
| Renk |İlk dizideki sütunlar için kullanılan renk. |
| Grafik sorgusu |İlk seri için çalıştırılan sorgu. Her zaman aralığında kayıtların sayısını grafiğin sütunları tarafından temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtların değerlerin ortalaması.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>Son örnek: Grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıkları maksimum değer.</li></ul> |
| **İkinci grafik** | |
| Gösterge |İkinci bir seri için çağrı altında görüntülenen metin. |
| Renk |Serinin ikinci sütun için kullanılan renk. |
| Grafik sorgusu |İkinci bir seri için çalıştırılan sorgu. Her zaman aralığında kayıtların sayısını grafiğin sütunları tarafından temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliği üzerinde gerçekleştirilen işlem.<ul><li>Ortalama: Tüm kayıtların değerlerin ortalaması.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>Son örnek: Grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıkları maksimum değer. |
| **Gelişmiş** |**> Veri akışı doğrulama** |
| Enabled |Veri akışı doğrulama kutucuğuna etkinleştirilmesi gerekiyorsa, bu bağlantıyı seçin. Bu yaklaşım, veri kullanılabilir durumda değilse alternatif bir ileti sağlar. Normalde bir ileti zaman görünümü yüklü olduğundan ve veri kullanılabilir olduğunda geçici döneminde sağlamak için bir yaklaşım kullanın. |
| Sorgu |Sorgu veri görünümü için kullanılabilir olup olmadığını belirlemek için çalıştırın. Sorgu hiçbir sonuç döndürmezse, ana sorgunun değeri yerine bir ileti görüntülenir. |
| `Message` |Veri akışı doğrulama sorgusu veri döndürürse görüntülenen ileti. İleti yok, sağlarsanız bir *değerlendirme gerçekleştiriliyor* durum iletisi görüntülenir. |


## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) kutucuklar sorguları desteklemek için.
* Ekleme [görselleştirme bölümleri](view-designer-parts.md) , özel bir görünüm.
