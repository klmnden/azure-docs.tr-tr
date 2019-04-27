---
title: Geçiş etkinliğini izlemek için Azure veritabanı geçiş hizmeti kullanın. | Microsoft Docs
description: Geçiş etkinliğini izlemek için Azure veritabanı geçiş hizmeti kullanmayı öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 03/12/2019
ms.openlocfilehash: 325bbee3f3d5ad5097f710cb56fe03baff97388a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60532777"
---
# <a name="monitor-migration-activity"></a>Geçiş etkinliğini izleme
Bu makalede, bir veritabanı ve tablo düzeyindeki hem bir geçiş işleminin ilerleme durumunu izlemek nasıl öğrenin.

## <a name="monitor-at-the-database-level"></a>Veritabanı düzeyinde izleme
Veritabanı düzeyinde etkinliğini izlemek için veritabanı düzeyinde dikey penceresini görüntüleyin:

![Veritabanı düzeyinde dikey penceresi](media/how-to-monitor-migration-activity/dms-database-level-blade.png)

> [!NOTE]
> Veritabanı köprü seçerek tablolar ve geçiş aşamaları listesini gösterir.

Aşağıdaki tablo, veritabanı düzeyinde dikey penceresindeki alanları listeler ve her ilişkilendirilmiş çeşitli durum değerleri açıklar.

<table id='overview' class='overview'>
  <thead>
    <tr>
      <th class="x-hidden-focus"><strong>Alan adı</strong></th>
      <th><strong>Alan alt durumu</strong></th>
      <th><strong>Açıklama</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" class="ActivityStatus"><strong>Etkinlik durumu</strong></td>
      <td>Çalışıyor</td>
      <td>Geçiş etkinliği çalışıyor.</td>
    </tr>
    <tr>
      <td>Başarılı oldu</td>
      <td>Geçiş etkinlik sorunlarla başarılı oldu.</td>
    </tr>
    <tr>
      <td>Hatalı</td>
      <td>Geçiş başarısız oldu. Geçiş ayrıntıları için tam hata iletisinin altında 'hata ayrıntılarına bakın' bağlantıyı seçin.</td>
    </tr>
    <tr>
      <td rowspan="4" class="Status"><strong>Durum</strong></td>
      <td>Başlatılıyor</td>
      <td>DMS geçiş ardışık ayarlıyor.</td>
    </tr>
    <tr>
      <td>Çalışıyor</td>
      <td>DMS işlem hattı çalıştırma ve geçişi gerçekleştirme.</td>
    </tr>
    <tr>
      <td>Tamamlama</td>
      <td>Geçiş tamamlandı.</td>
    </tr>
    <tr>
      <td>Başarısız</td>
      <td>Geçiş başarısız oldu. Geçiş hataları görmek için geçiş ayrıntıları tıklayın.</td>
    </tr>
    <tr>
      <td rowspan="5" class="migration-details"><strong>Geçiş ayrıntıları</strong></td>
      <td>Geçiş ardışık düzenini başlatmaktan</td>
      <td>DMS geçiş ardışık ayarlıyor.</td>
    </tr>
    <tr>
      <td>Tam veri yüklemesi devam ediyor</td>
      <td>DMS, ilk yükleme gerçekleştiriyor.</td>
    </tr>
    <tr>
      <td>Hazır tam geçişi</td>
      <td>İlk yükleme tamamlandıktan sonra DMS tam geçişi veritabanı için hazır olarak işaretler. Kullanıcı verileri üzerinde sürekli eşitleme zachytila výjimku, denetlemeniz gerekir.</td>
    </tr>
    <tr>
      <td>Tüm değişiklikler uygulandı</td>
      <td>İlk yükleme ve sürekli Eşitleme tamamlandı. Bu durum ayrıca veritabanı başarıyla tam geçişi tamamlandıktan sonra gerçekleşir.</td>
    </tr>
    <tr>
      <td>Bkz. hata ayrıntıları</td>
      <td>Hata ayrıntılarını görüntülemek için bağlantıya tıklayın.</td>
    </tr>
    <tr>
      <td rowspan="1" class="duration"><strong>Süresi</strong></td>
      <td>Yok</td>
      <td>Toplam süre geçiş başlatılmakta geçiş etkinlikten tamamlanması veya geçiş hatalı.</td>
    </tr>
     </tbody>
</table>

## <a name="monitor-at-table-level--quick-summary"></a>Tablo düzeyinde – kısa özet izleme
Tablo düzeyinde etkinliğini izlemek için tablo düzeyi dikey penceresini görüntüleyin. Dikey pencerenin üst bölümünde yük tam ve artımlı güncelleştirmeleri ayrıntılı satır sayısı geçirilen gösterir. 

Dikey pencerenin alt kısmındaki tabloları listeler ve hızlı geçiş ilerleme durumu özetini gösterir.

![Tablo düzeyi dikey penceresi - hızlı özeti](media/how-to-monitor-migration-activity/dms-table-level-blade-summary.png)

Aşağıdaki tabloda, tablo düzeyi ayrıntıları gösterilen alanlar açıklanır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Tam Yük tamamlandı**      | Tablo sayısı tam veri yüklemesi tamamlandı. |
| **Tam Yük sıraya alındı**      | Tam Yük sıraya alınmasını izleyen tablo sayısı.      |
| **Tam Yük yükleme** | Tablo sayısı başarısız oldu.      |
| **Artımlı güncelleştirmeler**      | Değişiklik verilerini sayısı (CDC) güncelleştirmeleri hedefe uygulanması satırlarda yakalayın. |
| **Artımlı ekler**      | CDC sayısı hedefe uygulanması satır ekler.      |
| **Artımlı siler** | CDC sayısı hedefe uygulanması satırlarını siler.      |
| **Bekleyen değişiklikler**      | CDC uygulandığından hala bekleyen satır sayısını hedef. |
| **Uygulanan değişiklikler**      | CDC toplamı, ekler, güncelleştirir ve hedefe uygulanması satırlarda siler.      |
| **Hata durumundaki tabloları** | Geçiş sırasında 'error' durumda olan tablo sayısı. Hedef tanımlanan yinelemeler olduğunda tabloları hata durumuna gidebilirsiniz bazı örnekler ya da verileri hedef tabloda yüklenirken uyumlu değil.      |

## <a name="monitor-at-table-level--detailed-summary"></a>Tablo düzeyinde – ayrıntılı Özet izleme
Tam Yük ve artımlı veri eşitleme geçiş ilerleme durumunu gösteren iki sekme bulunur.
    
![Tam Yük sekmesi](media/how-to-monitor-migration-activity/dms-full-load-tab.png)

![Artımlı veri eşitleme sekmesi](media/how-to-monitor-migration-activity/dms-incremental-data-sync-tab.png)

Aşağıdaki tabloda, tablo düzeyi geçiş sürüyor gösterilen alanlar açıklanır.

| Alan adı        | Açıklama       |
| ------------- | ------------- |
| **Durumu - eşitleniyor**      | Sürekli eşitleme çalışıyor. |
| **Ekle**      | CDC sayısı hedefe uygulanması satır ekler.      |
| **Güncelleştirme** | CDC güncelleştirmeleri hedefe uygulanması satırların sayısı.      |
| **Silme**      | CDC sayısı hedefe uygulanması satırlarını siler. |
| **Toplam uygulanan**      | CDC toplamı, ekler, güncelleştirir ve hedefe uygulanması satırlarda siler. |
| **Veri hataları** | Bu tabloda veri hatası oluştu. Bazı örnekler hataların *511: 8114 %d, izin verilen en büyük satır boyutundan büyük olan %d boyutunda bir satır oluşturulamıyor: Verileri dönüştürme hatası % ls. %ls yazın.*  Hata ayrıntılarını görmek için Azure hedef dms_apply_exceptions tablosundan müşteri sorgu.    |

> [!NOTE]
> INSERT, Update ve Delete ve toplam uygulanan CDC değerlerini veritabanı tam geçişi veya geçiş yeniden düşebilir.

## <a name="next-steps"></a>Sonraki adımlar
- Microsoft Geçiş Kılavuzu gözden [veritabanı Geçiş Kılavuzu](https://datamigration.microsoft.com/).