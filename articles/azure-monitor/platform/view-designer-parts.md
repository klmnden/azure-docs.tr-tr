---
title: Görünüm Tasarımcısı bölümlerine Azure İzleyici'de bir başvuru kılavuzu | Microsoft Docs
description: Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında görüntülenir ve görselleştirmeler Log Analytics çalışma alanındaki veriler üzerinde çeşitli içeren özel görünümlerinizi oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir görselleştirme bölümleri ayarlarını bir başvuru kılavuzudur.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: 5718d620-b96e-4d33-8616-e127ee9379c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/12/2018
ms.author: bwren
ms.openlocfilehash: dead1fae9bc3287ed0fc80c6120914e965ef96dd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341932"
---
# <a name="reference-guide-to-view-designer-visualization-parts-in-azure-monitor"></a>Azure İzleyici'de Görünüm Tasarımcısı görselleştirme bölümü başvurusu Kılavuzu
Azure İzleyici'de görünüm Tasarımcısını kullanarak, Azure portalında Log Analytics çalışma alanınızdaki veri görselleştirmenize yardımcı olabilecek çeşitli özel görünümler oluşturabilirsiniz. Bu makalede, özel görünümlerde kullanılabilir görselleştirme bölümleri ayarlarını bir başvuru kılavuzudur.

Görünüm Tasarımcısı hakkında daha fazla bilgi için bkz:

* [Görüntüleme Tasarımcısı](view-designer.md): Oluşturma ve düzenleme özel görünümler için Görünüm Tasarımcısı ve yordamları hakkında genel bir bakış sağlar.
* [Kutucuk başvurusu](view-designer-tiles.md): Kendi özel görünümlerinizi de kullanılabilir her döşeme için ayarları bir başvuru sağlar.


Kullanılabilir Görünüm Tasarımcısı kutucuğu türleri, aşağıdaki tabloda açıklanmıştır:

| Görünüm türü | Açıklama |
|:--- |:--- |
| [Sorgu listesi](#list-of-queries-part) |Günlük sorguları listesini görüntüler. Her sorgu sonuçlarını görüntülemeyi seçebilirsiniz. |
| [Sayı ve liste](#number-and-list-part) |Üstbilgi, günlük sorgusu kayıtları sayısını gösteren tek bir sayı görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler. |
| [İki sayı ve liste](#two-numbers-and-list-part) |Üstbilgi ayrı günlük sorguları kayıtları sayısını gösterir iki sayıyı görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler. |
| [Halka ve liste](#donut-and-list-part) |Üstbilgi, günlük sorgusu değer sütununda özetlenir tek bir sayı görüntüler. Halka grafik en çok üç kayıtların sonuçlarını görüntüler. |
| [İki zaman çizelgesi ve liste](#two-timelines-and-list-part) |Üstbilgi, günlük sorgusu değer sütununda özetlenir tek bir sayı görüntüler bir belirtme çizgisi içeren sütun grafikleri, zaman içinde iki günlük sorguların sonuçlarını görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler. |
| [Bilgi](#information-part) |Üst bilgi, statik metin ve isteğe bağlı bir bağlantı görüntülenir. Bir veya daha fazla statik başlık ve metin öğeleri listede görüntülenir. |
| [Çizgi grafik, belirtme çizgisi ve liste](#line-chart-callout-and-list-part) |Üstbilgi ile günlük sorgu süresi ve belirtme çizgisi bir özetlenmiş değer ile birden çok serisinden bir çizgi grafiği görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler. |
| [Çizgi grafik ve liste](#line-chart-and-list-part) |Üstbilgi ile birden çok zaman içinde bir günlük sorgusu serisinden bir çizgi grafiği görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler. |
| [Yığın çizgi grafikler bölümü](#stack-of-line-charts-part) |Zaman içinde bir günlük sorgusu birden çok serisinden üç ayrı çizgi grafiklerde görüntüler. |

Sonraki bölümlerde, kutucuk türleri ve bunların özelliklerini ayrıntılı açıklanmaktadır.

> [!NOTE]
> Görünümlerde parçaları temel [oturum sorguları](../log-query/log-query-overview.md) Log Analytics çalışma alanınızda. Şu anda desteklemediği [çapraz kaynak sorgularını](../log-query/cross-workspace-query.md) uygulama anlayışları'ndan veri alınamadı.

## <a name="list-of-queries-part"></a>Sorgular bölüm listesi
Günlük sorguları listesi sorguların parçası listesini görüntüler. Her sorgu sonuçlarını görüntülemeyi seçebilirsiniz. Varsayılan olarak tek bir sorgu görünümü içerir ve seçebileceğiniz **+ sorgu** ek sorgular eklemek için.

![Sorgu Görünümü Listesi](media/view-designer-parts/view-list-queries.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Unvan |Görünümü üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Önceden seçilen filtreler |Bir sorgu seçin filtre sol bölmesinde dahil edilecek özelliklerin virgülle ayrılmış listesi. |
| Oluşturma modu |Sorgu seçildiğinde görüntülenen başlangıç görünümü. Bunlar sorguyu açtıktan sonra tüm kullanılabilir görünümleri seçebilirsiniz. |
| **Sorgular** | |
| Arama sorgusu |Çalıştırılacak sorgu. |
| Kolay ad | Tanımlayıcı adı görüntülenir. |

## <a name="number-and-list-part"></a>Sayı ve liste bölümü
Üstbilgi, günlük sorgusu kayıtları sayısını gösteren tek bir sayı görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler.

![Sorgu Görünümü Listesi](media/view-designer-parts/view-number-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünümü üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Başlık** | |
| Gösterge |Üst bilgi üst kısmında görüntülenen metin. |
| Sorgu |Üst bilgisi için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. İlk on kayıtlara ilk iki özellikleri görüntülenir. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Çubukları sayısal sütun göreli değerini temel alan otomatik olarak oluşturulur.<br><br>Kullanım `Sort` listedeki kayıtları sıralamak için sorgu komutu. Sorgu çalıştırmak ve tüm kayıtları döndürmek için seçebileceğiniz **tümünü gör**. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| Ad ve değer ayırıcı |Birden çok değer metin özelliğini ayrıştırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="two-numbers-and-list-part"></a>İki sayı ve liste bölümü
Üst bilgisi ayrı günlük sorguları kayıtları sayısını görüntüler iki sayı yok. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler.

![İki sayı ve liste görünümü](media/view-designer-parts/view-two-numbers-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Görünümü üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Başlık Gezinti** | |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Başlık** | |
| Gösterge |Üst bilgi üst kısmında görüntülenen metin. |
| Sorgu |Üst bilgisi için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. İlk on kayıtlara ilk iki özellikleri görüntülenir. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Çubukları, göreli sayısal bir sütun değerine bağlı olarak otomatik olarak oluşturulur.<br><br>Kullanım `Sort` listedeki kayıtları sıralamak için sorgu komutu. Sorgu çalıştırmak ve tüm kayıtları döndürmek için seçebileceğiniz **tümünü gör**. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Birden çok değer metin özelliğini ayrıştırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="donut-and-list-part"></a>Halka ve liste bölümü
Üstbilgi, günlük sorgusu değer sütununda özetlenir tek bir sayı görüntüler. Halka grafik en çok üç kayıtların sonuçlarını görüntüler.

![Halka ve liste görünümü](media/view-designer-parts/view-donut-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Unvan |Üst bilgi üst kısmında görüntülenen metin. |
| Alt Başlık |Başlığının üst başlığı altında görüntülenen metin. |
| **Halka** | |
| Sorgu |Halka için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Halka** |**> Merkezi** |
| Text |Halka içindeki değeri altında görüntülenen metin. |
| İşlem |Tek bir değer özetlemek için değer özelliği üzerinde gerçekleştirilecek işlem.<ul><li>Toplama: Tüm kayıtların değerleri toplar.</li><li>Yüzdesi: Değerler tarafından döndürülen kayıtları oranını **neden merkezdeki işlemde kullanılan değerleri** sorgu toplam kayıtlara.</li></ul> |
| Merkezdeki işlemde kullanılan sonuç değerleri |İsteğe bağlı olarak, bir veya daha fazla değer eklemek için artı işaretini (+) seçin. Sorgu sonuçlarını kayıtlarını belirttiğiniz özellik değerleri ile sınırlıdır. Hiçbir değer eklediyseniz tüm kayıtları sorguya dahil edilir. |
| **Ek Seçenekler** |**> Renkler** |
| Renkli 1<br>Renk 2<br>Renk 3 |Halka, görüntülenen değerleri için renk seçin. |
| **Ek Seçenekler** |**> Gelişmiş renk eşleme** |
| Alan değeri |Halkada yer alıyorsa farklı bir renkte görüntülenecek bir alanı adını yazın. |
| Renk |Benzersiz alan rengini seçin. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Birden çok değer metin özelliğini ayrıştırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="two-timelines-and-list-part"></a>İki zaman çizelgesi ve liste bölümü
Üstbilgi, günlük sorgusu değer sütununda özetlenir tek bir sayı görüntüler bir belirtme çizgisi içeren sütun grafikleri, zaman içinde iki günlük sorguların sonuçlarını görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler.

![İki zaman çizelgesi ve liste görüntüle](media/view-designer-parts/view-two-timelines-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Başlık Gezinti** | |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **İlk grafik<br>ikinci grafik** | |
| Gösterge |Belirtme çizgisi ilk serisinin altında görüntülenen metin. |
| Renk |Sütun serisi için kullanılacak rengi. |
| Sorgu |İlk seri için çalıştırılacak sorgu. Her zaman aralığı içindeki kayıtları sayısı, grafiğin sütunları tarafından temsil edilir. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliğinde gerçekleştirilecek işlem.<ul><li>Toplama: Tüm kayıtların değerlerinin toplamı.</li><li>Ortalama: Tüm kayıtların değerlerin ortalaması.</li><li>Son örnek: Grafikte dahil son aralığı değeri.</li><li>İlk örnek: Grafikte bulunan ilk aralık değeri.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li></ul> |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="information-part"></a>Bilgi bölümü
Üst bilgi, statik metin ve isteğe bağlı bir bağlantı görüntülenir. Bir veya daha fazla statik başlık ve metin öğeleri listede görüntülenir.

![Bilgileri görüntüle](media/view-designer-parts/view-information.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Renk |Başlık arka plan rengi. |
| **Üst bilgi** | |
| Image |Üst bilgide görüntülenen resim dosyası. |
| Etiket |Üst bilgide görüntülenen metin. |
| **Üst bilgi** |**> Bağlantı** |
| Etiket |Bağlantı metni. |
| Url |Bağlantı için Url. |
| **Bilgi öğeleri** | |
| Unvan |Başlığı her öğe için görüntülenen metin. |
| İçerik |Her öğe için görüntülenen metin. |

## <a name="line-chart-callout-and-list-part"></a>Çizgi grafik, belirtme çizgisi ve liste bölümü
Üstbilgi günlük sorgusu birden çok serisinden bir çizgi grafiğin zaman ve belirtme çizgisi bir özetlenmiş değer ile üzerinden görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler.

![Çizgi grafik, belirtme çizgisi ve liste görünümü](media/view-designer-parts/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Unvan |Üst bilgi üst kısmında görüntülenen metin. |
| Alt Başlık |Başlığının üst başlığı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseninin bu zaman aralığını kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Çizgi grafik** |**> Açıklama balonu** |
| Belirtme çizgisi başlığı |Belirtme çizgisi değerinin üzerine görüntülenecek metin. |
| Seri Adı |Belirtme çizgisi değeri için kullanılacak bir seri için özellik değeri. Hiçbir serisi sağlanırsa, sorgudaki tüm kayıtlar kullanılır. |
| İşlem |Belirtme çizgisi için tek bir değer olarak özetlemek için değer özelliğinde gerçekleştirilecek işlem.<ul><li>Ortalama: Tüm kayıtların değerlerin ortalaması.</li><li>Sayısı: Sorgu tarafından döndürülen tüm kayıtları sayısı.</li><li>Son örnek: Grafikte dahil son aralığı değeri.</li><li>En fazla: Grafikte yer aralıkları maksimum değeri.</li><li>En küçük: Grafikte yer aralıkları en düşük değeri.</li><li>Toplama: Tüm kayıtların değerlerinin toplamı.</li></ul> |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimlerin belirtin. Bu bilgiler değeri türlerini belirtmek görüntü grafiğini etiketlere ve da isteğe bağlı olarak için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *Dönüştür*, sayısal değerler gelen dönüştürülür *geçerli birim* için yazın *Dönüştür* türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Birden çok değer metin özelliğini ayrıştırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="line-chart-and-list-part"></a>Çizgi grafik ve liste bölümü
Üstbilgi, günlük sorgusu birden çok serisinden bir çizgi grafiğin zaman içinde görüntüler. Listenin bir sorgudan göreli değerini sayısal bir sütun veya zaman içinde değişiklik gösteren bir grafiği ile en iyi on sonuçlar görüntüler.

![Çizgi grafik ve liste görünümü](media/view-designer-parts/view-line-chart-callout-list.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| Simge Kullan |Simge görüntülemek için bu bağlantıyı seçin. |
| **Üst bilgi** | |
| Unvan |Üst bilgi üst kısmında görüntülenen metin. |
| Alt Başlık |Başlığının üst başlığı altında görüntülenen metin. |
| **Çizgi grafik** | |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseninin bu zaman aralığını kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Çizgi grafik** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimlerin belirtin. Bu bilgiler değeri türlerini belirtmek görüntü grafiğini etiketlere ve da isteğe bağlı olarak için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *Dönüştür*, sayısal değerler gelen dönüştürülür *geçerli birim* için yazın *Dönüştür* türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |
| **Liste** | |
| Sorgu |Liste için çalıştırılacak sorgu. Sorgu tarafından döndürülen kayıtları sayısı görüntülenir. |
| Grafiği gizle |Grafiğin sağ tarafındaki sayısal sütun devre dışı bırakmak için bu bağlantıyı seçin. |
| Mini Grafikleri etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Renk |Mini Grafikler ve çubuk rengi. |
| İşlem |Mini Grafik için gerçekleştirilecek işlem. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Ad ve değer ayırıcı |Birden çok değer metin özelliğini ayrıştırmak için kullanılacak tek karakterli sınırlayıcı. Daha fazla bilgi için [ortak ayarları](#sparklines). |
| Tıklama Gezinti | Listedeki bir öğeye tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Liste** |**> Sütun başlıkları** |
| Ad |İlk sütun üst kısmında görüntülenen metin. |
| Değer |İkinci sütun üst kısmında görüntülenen metin. |
| **Liste** |**> Eşikleri** |
| Eşikleri Etkinleştir |Eşikleri etkinleştirmek için bu bağlantıyı seçin. Daha fazla bilgi için [ortak ayarları](#thresholds). |

## <a name="stack-of-line-charts-part"></a>Yığın çizgi grafikler bölümü
Çizgi grafik yığını, burada gösterildiği gibi zaman içinde bir günlük sorgusu birden çok serisinden üç ayrı çizgi grafiklerde görüntüler:

![Çizgi grafik yığını](media/view-designer-parts/view-stack-line-charts.png)

| Ayar | Açıklama |
|:--- |:--- |
| **Genel** | |
| Grup başlığı |Kutucuğun üst kısmında görüntülenen metin. |
| Yeni Grup |Geçerli görünüme başlangıç Görünümü'nde yeni bir grup oluşturmak için bu bağlantıyı seçin. |
| Simge |Üst bilgisindeki sonucu yanında görüntülenen resim dosyası. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**> Üst bilgi** |
| Unvan |Grafiğin üst kısmında görüntülenen metin. |
| Alt Başlık |Grafiğin üst kısmındaki başlık altında görüntülenen metin. |
| **Grafik 1<br>grafik 2<br>grafik 3** |**Çizgi grafik** |
| Sorgu |Çizgi grafiği için çalıştırılacak sorgu. Bir metin değeri ilk özelliğidir ve ikinci özelliği sayısal bir değerdir. Bu sorgu normalde kullandığı *ölçü* sonuçları özetlemek için anahtar sözcüğü. Sorgu kullanıyorsa *aralığı* anahtar sözcüğü, grafiğin x ekseninin bu zaman aralığını kullanır. Sorgu içermiyorsa *aralığı* anahtar sözcüğü, saatlik aralıklarla x ekseni kullanır. |
| Tıklama Gezinti | Başlığında'a tıkladığınızda gerçekleştirilen eylem.  Daha fazla bilgi için [ortak ayarları](#click-through-navigation). |
| **Grafik** |**> Y ekseni** |
| Logaritmik Ölçek Kullan |Y ekseni için Logaritmik ölçek kullanmak için bu bağlantıyı seçin. |
| Birimler |Sorgu tarafından döndürülen değerler için birimlerin belirtin. Bu bilgiler değeri türlerini belirtmek görüntü grafiğini etiketlere ve da isteğe bağlı olarak için kullanılan değerleri dönüştürün. *Birim* türü birim kategorisini belirtir ve kullanılabilir tanımlar *geçerli birim* değerler girin. Bir değer seçerseniz *Dönüştür*, sayısal değerler gelen dönüştürülür *geçerli birim* için yazın *Dönüştür* türü. |
| Özel Etiket |Y ekseninin yanındaki etiketi için görüntülenecek metni *birim* türü. Etiket yoktur, yalnızca belirtilirse *birim* türü görüntülenir. |

## <a name="common-settings"></a>Ortak ayarları
Aşağıdaki bölümlerde, birkaç görselleştirme bölümleri için ortak olan ayarları açıklanmaktadır.

### <a name="name-value-separator"></a>Ad ve değer ayırıcı
Ad ve değer ayırıcı metin özelliği bir liste sorgusu içinde birden çok değer ayrıştırmak için kullanılacak tek karakterli sınırlayıcı ' dir. Sınırlayıcı belirtirseniz aynı sınırlayıcıyla ayrılmış her bir alan adları sağlayabilir **adı** kutusu.

Örneğin, adlı bir özellik düşünün *konumu* gibi değerleri dahil *Redmond yapı 41* ve *Bellevue yapı 12*. Bir tire (-) için ad ve değer ayırıcı belirtebilirsiniz ve *Şehir yapı* adı. Bu yaklaşım her değer adlı iki özellik ayrıştırır *Şehir* ve *yapı*.

### <a name="click-through-navigation"></a>Tıklama aracılığıyla gezinti
Gezinti tıklama eylemi bir üst bilgi veya liste öğesi görünümünde tıkladığınızda gerçekleştirilecek tanımlar.  Bu sorguda açar [Log Analytics](../../azure-monitor/log-query/portals.md) veya başka bir görünüme başlatın.

Aşağıdaki tabloda, tıklama gezinme için ayarları açıklar.

| Ayar           | Açıklama |
|:--|:--|
| Günlük Arama (Otomatik) | Üstbilgi öğesi seçtiğinizde çalıştırmak için günlük sorgusu.  Öğenin temel aldığı aynı günlük sorgusu budur.
| Günlük araması        | Bir listede bir öğe seçtiğinizde çalıştırmak için günlük sorgusu.  Sorguyu yazın **Gezinti sorgusu** kutusu.   Kullanım *{seçili öğe}* kullanıcı seçili öğe için söz dizimi dahil etmek için.  Örneğin, sorgu adlı bir sütun varsa *bilgisayar* ve gezinti sorgusu *{seçili öğe}*, gibi bir sorgu *bilgisayar "Bilgisayarım" =* seçtiğinizde çalıştırmak bir bilgisayar. Gezinti sorgusu ise *türü = {seçili öğe} olay*, sorgu *türü olay bilgisayar = "Bilgisayarım" =* çalıştırılır. |
| Görünüm              | Bir listede bir üstbilgi öğesi veya bir öğe seçtiğinizde açmak için görüntüleyin.  Çalışma alanınızda bir görünüm adı seçin **Görünüm adı** kutusu. |



### <a name="sparklines"></a>Mini Grafikler
Bir mini zaman içinde bir liste Girişi değerini gösteren bir küçük çizgi grafiğidir. Bir liste ile görselleştirme bölümleri için sayısal bir sütun veya değeri zaman içinde gösterir. bir mini göreli değerini gösteren yatay bir çubuk görüntülenip görüntülenmeyeceğini seçebilirsiniz.

Aşağıdaki tabloda, Mini Grafikler için ayarları açıklar:

| Ayar | Açıklama |
|:--- |:--- |
| Mini Grafikleri Etkinleştir |Yatay bir çubuk yerine bir mini görüntülemek için bu bağlantıyı seçin. |
| İşlem |Mini Grafikler etkinleştirildiğinde, Mini Grafik değerleri hesaplamak için listedeki her bir özellik üzerinde gerçekleştirilecek işlem budur.<ul><li>Son örnek: Zaman aralığı üzerinden bir seri için son değeri.</li><li>En fazla: Zaman aralığı üzerinden bir seri için en büyük değer.</li><li>En küçük: Zaman aralığı üzerinden bir seri için minimum değeri.</li><li>Toplama: Zaman aralığı üzerinden bir seri için değerlerinin toplamı.</li><li>Özet: Aynı kullanan `measure` üst bilgisindeki sorgu olarak komutu.</li></ul> |

### <a name="thresholds"></a>Eşikleri
Eşikleri kullanarak bir listedeki her öğenin yanında renkli bir simgesi görüntüleyebilirsiniz. Eşikleri belirli bir değeri ya da belirli bir aralıkta öğeleri hızlı bir gösterge sağlar. Örneğin, bir hata değeri aşarsa öğeleri kabul edilebilir bir değer, değer bir uyarı gösterir bir aralıkta ise sarı ve red için yeşil bir simge görüntüleyebilirsiniz.

Bir bölümü için eşikler etkinleştirdiğinizde, bir veya daha fazla eşikleri belirtmeniz gerekir. Bir öğenin değeri eşik değerden daha büyük ve İleri eşik değerden daha düşük ise, bu değer rengini kullanılır. Öğesi en yüksek eşik değerinden büyükse, başka bir renk kullanılır. 

Bir eşik değerine sahip her eşiği ayarlanmış **varsayılan**. Diğer değerler aşılırsa ayarlanır renk budur. Ekleyebilir veya eşikleri seçerek kaldırmak **Ekle** (+) veya **Sil** (x) düğmesini.

Aşağıdaki tabloda, eşikleri için ayarları açıklar:

| Ayar | Açıklama |
|:--- |:--- |
| Eşikleri Etkinleştir |Her bir değerin sol renkli bir simge görüntülemek için bu bağlantıyı seçin. Simge belirtilen eşikler göreli değer durumu gösterir. |
| Ad |Eşik değeri adı. |
| Eşik |Eşik değeri. Her liste öğesi için sistem durumu rengi, rengi öğenin değeri tarafından aşan en yüksek eşik değeri olarak ayarlanır. Hiçbir eşik değerleri aşılırsa, varsayılan renk kullanılır. |
| Renk |Eşik değerini gösteren rengi. |

## <a name="next-steps"></a>Sonraki adımlar
* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) görselleştirme bölümlerinde sorguları desteklemek için.
