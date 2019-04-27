---
title: Azure SQL veri ambarı yedekleme ve geri yükleme - anlık görüntüler, coğrafi olarak yedekli | Microsoft Docs
description: Yedekleme ve geri yükleme şeklini Azure SQL veri ambarı'nda öğrenin. Birincil bölgede bir geri yükleme noktası veri ambarınıza geri yüklemek için veri ambarı yedeklemelerini kullanın. Farklı bir coğrafi bölgeye geri yüklemek için coğrafi olarak yedekli yedeklemelerini kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/01/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: ebe45bf8f562b5be9ae2afda9d5940296396f155
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60679316"
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Yedekleme ve geri yükleme Azure SQL veri ambarı

Yedekleme ve Azure SQL veri ambarı'nda geri yükleme hakkında bilgi edinin. Veri ambarı geri yükleme noktaları kurtarmak veya önceki bir duruma birincil bölgedeki veri Ambarınızı kopyalamak için kullanın. Farklı bir coğrafi bölgeye geri yüklemek için coğrafi olarak yedekli yedeklemeleri kullanım veri ambarı.

## <a name="what-is-a-data-warehouse-snapshot"></a>Veri ambarı anlık görüntüsünü nedir

A *veri ambarı anlık görüntü* kurtarmak veya önceki bir duruma veri ambarınız kopyalama yararlanabilir bir geri yükleme noktası oluşturur.  SQL Data Warehouse dağıtılmış bir sistemde olduğundan, bir veri ambarı anlık görüntü, Azure depolamada bulunan çok sayıda dosya oluşur. Anlık görüntüler, artımlı değişiklikler, veri ambarında depolanan verilerden yakalar.

A *veri ambarı geri yükleme* varolan bir geri yükleme noktasından oluşturulan yeni veri ambarı veya silinen veri ambarı. Verilerinizi yanlışlıkla Bozulması veya silinmesi durumunda sonra yeniden oluştuğundan, veri ambarını geri yüklemek bir önemli bir iş sürekliliği ve olağanüstü durum kurtarma stratejinize parçasıdır. Veri ambarı veri Ambarınızı test veya geliştirme amacıyla kopyalarını oluşturmak için güçlü bir mekanizma da dağıtılır.  SQL veri ambarı geri yükleme hızları, kaynak ve hedef veri ambarı konumunu ve veritabanı boyutuna bağlı olarak değişebilir. Aynı bölge içinde ortalama olarak geri yükleme hızları, genellikle yaklaşık 20 dakika alın. 

## <a name="automatic-restore-points"></a>Otomatik Geri Yükleme Noktaları

Yerleşik bir özellik oluşturur hizmetinin geri yükleme noktaları anlık görüntüleridir. Bu özelliği etkinleştirmeniz gerekmez. Otomatik geri yükleme noktaları şu anda bu geri yükleme hizmeti kullandığı yere işaret kurtarma için SLA korumak için kullanıcılar tarafından silinemez.

SQL veri ambarı, veri ambarınızın yedi gün boyunca kullanılabilir geri yükleme noktası oluşturma gün boyunca anlık görüntüleri alır. Bu bekletme dönemi değiştirilemez. SQL veri ambarı sekiz saatlik kurtarma noktası hedefi (RPO) destekler. Veri ambarınız birincil bölgedeki herhangi biri son yedi gün içinde alınan anlık görüntülere geri yükleyebilirsiniz.

Son anlık görüntünün başlatıldığında görmek için çevrimiçi SQL veri ambarınıza bu sorguyu çalıştırın.

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs
order by run_id desc
;
```

## <a name="user-defined-restore-points"></a>Kullanıcı Tanımlı Geri Yükleme Noktaları

Bu özellik, veri ambarınızın geri yükleme noktaları önce ve sonra büyük değişiklikler oluşturmak için el ile tetikleyici anlık görüntüleri sağlar. Bu özellik, herhangi bir iş yükünün kesintiye uğraması veya kullanıcı hata durumunda Hızlı Kurtarma zamanı ek veri koruma sağlayan, geri yükleme noktaları mantıksal olarak tutarlı olmasını sağlar. Kullanıcı tanımlı bir geri yükleme noktaları yedi gün boyunca kullanılabilir ve sizin adınıza otomatik olarak silinir. Kullanıcı tanımlı bir geri yükleme noktalarının saklama süresini değiştiremezsiniz. **Kullanıcı tanımlı 42 geri yükleme noktaları** olması gerekir böylece zaman içinde herhangi bir noktada garanti [silinmiş](https://go.microsoft.com/fwlink/?linkid=875299) başka oluşturmadan önce geri yükleme noktası. Kullanıcı tanımlı bir geri yükleme noktaları aracılığıyla oluşturmak için anlık görüntüleri tetikleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/new-azsqldatabaserestorepoint#examples) veya Azure portalında.

> [!NOTE]
> Geri yükleme noktaları 7 günden daha uzun süre gerekiyorsa, lütfen bu özellik için oy [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/35114410-user-defined-retention-periods-for-restore-points). Ayrıca, bir kullanıcı tanımlı bir geri yükleme noktası oluşturmak ve yeni bir veri ambarı yeni oluşturulan geri yükleme noktasından geri yükleme. Geri yükledikten sonra çevrimiçi veri ambarına sahip ve, duraklatma süresiz olarak bilgi işlem maliyetlerinden tasarruf için. Duraklatılmış veritabanı, Azure Premium depolama fiyattan depolama ücreti alınmaz. Geri yüklenen veri ambarı etkin bir kopyasına ihtiyacınız olursa, hangi yalnızca birkaç dakika sürer devam edebilir.

### <a name="restore-point-retention"></a>Geri yükleme noktası bekletme

Geri yükleme noktası uzun saklama süreleri için aşağıdaki listeleri ayrıntıları:

1. SQL veri ambarı, 7 günlük saklama süresi ulaştığında bir geri yükleme noktası siler **ve** en az 42 toplam geri yükleme noktası (kullanıcı tanımlı hem otomatik dahil) olduğunda
2. Veri ambarı duraklatıldığında anlık görüntülerinin alınma değil
3. Bir geri yükleme noktası yaşını geri yükleme noktasını veri ambarı duraklatıldığında dahil geçen zamandan itibaren mutlak takvim günü tarafından ölçülür
4. Herhangi bir noktada, veri ambarı kadar 42 kullanıcı tanımlı bir geri yükleme noktalarını depolamak için sağlanır ve bu geri yükleme noktaları sürece 42 otomatik geri yükleme noktaları 7 günlük saklama süresi ulaştınız değil
5. Bir anlık görüntü oluşturulduğunda, veri ambarı 7 günden daha sonra duraklatıldı ve ardından devam ettirir, geri yükleme noktası kalmayana kadar 42 toplam geri yükleme noktası (kullanıcı tanımlı hem otomatik dahil) kalıcı hale getirmek mümkündür

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Veri ambarı bırakıldığında anlık görüntü saklama

Veri ambarı düşürdüğünüzde, SQL veri ambarı son anlık görüntüsünü oluşturur ve yedi gün boyunca kaydeder. Veri ambarı silme işlemi sırasında oluşturulan son geri yükleme noktasına geri yükleyebilirsiniz.

> [!IMPORTANT]
> Mantıksal bir SQL server örneği silerseniz, örneği ait tüm veritabanlarını da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.

## <a name="geo-backups-and-disaster-recovery"></a>Coğrafi yedekleme ve olağanüstü durum kurtarma

SQL veri ambarı coğrafi yedekleme için günde bir kez gerçekleştiren bir [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md). Bir coğrafi geri yükleme için RPO 24 saattir. Coğrafi yedekleme, SQL veri ambarı desteklendiği Bölgesi'nde bir sunucuya geri yükleyebilirsiniz. Coğrafi yedekleme, geri yükleme noktalarını, birincil bölgedeki erişemiyor durumunda veri ambarını geri yükleme sağlar.

Coğrafi yedekleme varsayılan olarak etkindir. Veri ambarınız Gen1 ise yapabilecekleriniz [çıkma](/powershell/module/az.sql/set-azsqldatabasegeobackuppolicy) istiyorsanız. Veri koruma garanti yerleşik olarak 2. nesil için coğrafi-yedeklemelerden kapatılamaz.

> [!NOTE]
> Coğrafi yedekleme için daha kısa bir RPO gerektiriyorsa, bu özellik için oy [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse). Ayrıca, bir kullanıcı tanımlı bir geri yükleme noktası oluşturma ve farklı bir bölgede yeni bir veri ambarı yeni oluşturulan geri yükleme noktasından geri yükleme. Geri yükledikten sonra çevrimiçi veri ambarına sahip ve, duraklatma süresiz olarak bilgi işlem maliyetlerinden tasarruf için. Duraklatılmış veritabanı, Azure Premium depolama fiyattan depolama ücreti alınmaz. Veri ambarı etkin bir kopyasını gerekir, hangi yalnızca birkaç dakika sürer devam edebilir.

## <a name="backup-and-restore-costs"></a>Yedekleme ve geri yükleme maliyetleri

Azure fatura bir satır öğesi depolama ve bir satır öğesi için olağanüstü durum kurtarma depolama için olduğunu fark edeceksiniz. Depolama anlık görüntüleri tarafından yakalanan artımlı değişikliklerin yanı sıra birincil bölgedeki verilerinizi depolamak için toplam maliyeti ücrettir. Anlık görüntüleri nasıl ücretlendirilir daha ayrıntılı açıklaması için başvurmak [anlık görüntüleri nasıl tahakkuk anlama ücretleri](https://docs.microsoft.com/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges?redirectedfrom=MSDN#snapshot-billing-scenarios). Coğrafi olarak yedekli ücret coğrafi yedekleri depolamak için maliyetini kapsar.  

Toplam Maliyet birincil veri ambarı ve anlık görüntü değişikliklerinin yedi gün için en yakın TB değerine yuvarlanır. Örneğin, veri ambarınızın boyutu 1,5 TB ise ve 100 GB anlık görüntü yakalar, 2 TB veri sayfasındaki Azure Premium depolama fiyatları üzerinden faturalandırılırsınız.

Coğrafi olarak yedekli depolamayı kullanıyorsanız ayrı bir depolama ücreti alırsınız. Coğrafi olarak yedekli depolama, standart okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) fiyatı üzerinden faturalandırılır.

SQL veri ambarı fiyatlandırması hakkında daha fazla bilgi için bkz. [SQL veri ambarı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) ve [çıkış ücretleri](https://azure.microsoft.com/pricing/details/bandwidth/) bölgeler arası geri yükleme sırasında.

## <a name="restoring-from-restore-points"></a>Geri yükleme noktalarından geri yükleme

Anlık görüntü başladığı saati temsil eden bir geri yükleme noktası her anlık görüntü oluşturur. Bir veri ambarını geri yüklemek için bir geri yükleme noktası seçin ve geri yükleme komutu.  

Geri yüklenen veri ambarı ve geçerli tutun veya bunlardan birini silin. Geçerli veri ambarı geri yüklenen veri ambarı ile değiştirmek istiyorsanız, bunu kullanarak adlandırabilirsiniz [ALTER DATABASE (Azure SQL veri ambarı)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) adı değiştirme seçeneği.

Bir veri ambarını geri yüklemek için bkz: [Azure portalını kullanarak bir veri ambarını geri yükleme](sql-data-warehouse-restore-database-portal.md), [PowerShell kullanarak bir veri ambarını geri yükleme](sql-data-warehouse-restore-database-powershell.md), veya [RESTAPI'lerinikullanarakbirveriambarınıgeriyükleme](sql-data-warehouse-restore-database-rest-api.md).

Silinmiş ya da duraklatılmış veri ambarını geri yükleme için [bir destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="cross-subscription-restore"></a>Çapraz abonelik geri yükleme

Doğrudan abonelik üzerinden geri yüklemeniz gerekirse, bu özellik için oy [burada](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/36256231-enable-support-for-cross-subscription-restore). Farklı bir mantıksal sunucuya geri yüklemek ve ['Taşıma'](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) sunucu çapraz aboneliği geri yüklemeyi gerçekleştirmek için abonelikler arasında. 

## <a name="geo-redundant-restore"></a>Coğrafi olarak yedekli geri yükleme

Yapabilecekleriniz [, veri ambarını geri yükleme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) SQL veri ambarı, seçilen performans düzeyinde destekleyen herhangi bir bölgesine.

> [!NOTE]
> Coğrafi olarak yedekli bir geri yüklemeyi gerçekleştirmek için bu özelliği bayraklarımızın gerekir değil.

## <a name="next-steps"></a>Sonraki adımlar

Olağanüstü durum planlama hakkında daha fazla bilgi için bkz. [iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)
