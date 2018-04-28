---
title: Azure SQL Data Warehouse yedekleme ve geri yükleme - anlık görüntüler, coğrafi olarak yedekli | Microsoft Docs
description: Yedekleme ve geri yükleme şeklini Azure SQL Data Warehouse'da öğrenin. Veri ambarınız birincil bölge içinde bir geri yükleme noktası geri yüklemek için veri ambarı yedekleri kullanın. Coğrafi olarak yedekli yedeklemeleri farklı bir coğrafi bölgeye geri yüklemek için kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: a4f24aad95f13315eaeac790c9006ca00f61af69
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Yedekleme ve geri yükleme Azure SQL veri ambarı
Yedekleme ve geri yükleme şeklini Azure SQL Data Warehouse'da öğrenin. Veri ambarınız birincil bölge içinde bir geri yükleme noktası geri yüklemek için veri ambarı yedekleri kullanın. Coğrafi olarak yedekli yedeklemeleri farklı bir coğrafi bölgeye geri yüklemek için kullanın. 

## <a name="what-is-backup-and-restore"></a>Yedekleme ve geri yükleme nedir?
A *veri ambarı yedekleme* kopyasıdır veritabanınızın bir veri ambarı geri yüklemek için kullanabilirsiniz.  SQL veri ambarı dağıtılmış bir sistemde olduğundan, bir veri ambarı yedekleme Azure depolama alanında bulunan çok sayıda dosya oluşur. Veri ambarı yedekleme, hem yerel veritabanı anlık görüntüleri hem de coğrafi yedeklemeleri tüm veritabanlarını ve veri ambarı ile ilişkili olan dosyaları içerir. 

A *veri ambarı geri yükleme* varolan bir yedek kopyadan oluşturulan yeni veri ambarı veya silinen veri ambarı. Geri yüklenen veri ambarı yedeklenen veri ambarı belirli bir zamanda yeniden oluşturur. Verilerinizin yanlışlıkla Bozulması veya silme işlemi sonrasında yeniden oluşturduğundan, veri ambarını geri tüm iş sürekliliği ve olağanüstü durum kurtarma stratejisi önemli bir bölümünü oluşturur.

Hem yerel hem de coğrafi geri yüklemeler SQL Data Warehouse'un olağanüstü durum kurtarma özellikleri bir parçasıdır. 

## <a name="local-snapshot-backups"></a>Yerel anlık görüntüsü yedekleri
Yerel anlık görüntüsü yedekleri hizmetinin yerleşik bir özelliktir.  Bunları etkinleştirmeniz gerekmez. 

SQL Data Warehouse veri Ambarınızı gün boyunca anlık görüntüleri alır. Anlık görüntüler yedi gün için kullanılabilir. SQL veri ambarı bir sekiz saat kurtarma noktası hedefi (RPO) destekler. Veri ambarınız birincil bölge içinde herhangi bir son yedi gün içinde alınan anlık görüntü geri yükleyebilirsiniz.

Son anlık görüntünün başlatıldığında görmek için bu sorguyu çevrimiçi SQL veri ambarı üzerinde çalıştırın. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

### <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Veri ambarı duraklatıldığında, anlık görüntü saklama
SQL Data Warehouse anlık görüntülerini oluşturma değil ve bir veri ambarı duraklatıldığında anlık görüntüleri dolmaz. Veri ambarı duraklatıldığında anlık görüntü yaş değiştirmez. Anlık görüntü saklama, veri ambarı Takvim günleri çevrimiçi olduğu gün sayısını temel alır.

Örneğin, bir anlık görüntü 4'e 1 Ekim başlatır ve 4'e veri ambarı Ekim 3 olarak duraklatıldı, anlık görüntüleri iki güne kadar eski demektir. Veri ambarı tekrar çevrimiçi olduğunda iki gün anlık görüntüsüdür. Veri ambarı Ekim 5 4'e çevrimiçi olursa, anlık görüntü iki gün önce yapılmışsa ve daha fazla beş gün boyunca devam eder.

Veri ambarı tekrar çevrimiçi olduğunda, SQL Data Warehouse Yeni Anlık görüntülerin sürdürür ve yedi günden fazla veri olduğunda anlık görüntüleri süresi dolar.

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Veri ambarı bırakıldığında anlık görüntü saklama
Veri ambarı bıraktığınızda, SQL Data Warehouse son anlık görüntü oluşturur ve yedi gün için kaydeder. Veri ambarı silme işlemi sırasında oluşturulan son geri yükleme noktası geri yükleyebilirsiniz. 

> [!IMPORTANT]
> Mantıksal bir SQL server örneği silerseniz örneği ait tüm veritabanlarının da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.
> 

## <a name="geo-backups"></a>Coğrafi yedekleri
SQL veri ambarı gerçekleştirir coğrafi yedekleme için günde bir kez bir [eşleştirilmiş veri merkezi](../best-practices-availability-paired-regions.md). RPO bir coğrafi geri yükleme için 24 saattir. Bir sunucuya SQL Data Warehouse burada desteklenen başka bir bölgede coğrafi yedekleme geri yükleyebilirsiniz. Coğrafi yedekleme anlık görüntüleri birincil bölgenizde erişemiyor durumunda, veri ambarı geri yükleyebilirsiniz sağlar.

Coğrafi yedeklemeleri varsayılan olarak açıktır. Veri ambarınız Gen1 ise, yapabilecekleriniz [çıkma](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy) istiyorsanız. Veri koruma yerleşik gurantee olarak Gen2 için coğrafi yedeklemeleri dışında'ni kaldıramaz.

## <a name="backup-costs"></a>Yedekleme maliyetleri
Azure Premium Storage ve coğrafi olarak yedekli depolama için bir satır öğesi için bir satır öğesi Azure faturasını sahip fark edeceksiniz. Premium depolama ücret anlık görüntüleri içeren birincil bölgede verilerinizi depolamak için toplam maliyetidir.  Coğrafi olarak yedekli ücret coğrafi yedeklemelerini depolamak için maliyet kapsar.  

Maliyet birincil veri ambarı ve Azure Blob anlık görüntü yedi gün için toplam yakın TB yuvarlanır. Örneğin, veri Ambarınızı 1,5 TB ise ve 100 GB anlık görüntüleri kullanmak, 2 TB veri Azure Premium Storage hızlarında için faturalandırılır. 

> [!NOTE]
> Her anlık görüntü başlangıçta boş ve birincil veri ambarına değişiklik gibi büyür. Tüm anlık görüntüleri veri ambarı değişiklikleri boyutunu artırın. Bu nedenle, anlık görüntüler için depolama maliyetleri değişme oranı göre artar.
> 
> 

Coğrafi olarak yedekli depolama birimi kullanıyorsanız, ayrı depolama ücret alırsınız. Coğrafi olarak yedekli depolama standart okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) oranı faturalandırılır.

SQL Data Warehouse fiyatlandırması hakkında daha fazla bilgi için bkz: [SQL Data Warehouse fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restoring-from-restore-points"></a>Geri yükleme noktalarından geri yükleme
Her anlık görüntü anlık görüntü başlama zamanı temsil eden bir geri yükleme noktası vardır. Veri ambarı geri yüklemek için bir geri yükleme noktası seçin ve geri yükleme komutu.  

SQL veri ambarı her zaman yeni bir veri ambarı yedeğini geri yükler. Geri yüklenen veri ambarı ve geçerli bir tutun veya bunlardan birini silin. Geçerli veri ambarıyla geri yüklenen veri ambarı ile değiştirmek istiyorsanız, bunu kullanarak adlandırabilirsiniz [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) adı Değiştir seçeneği. 

Bir veri ambarı geri yüklemek için bkz: [Azure portalını kullanarak bir veri ambarı geri](sql-data-warehouse-restore-database-portal.md), [PowerShell kullanarak bir veri ambarı geri](sql-data-warehouse-restore-database-powershell.md), veya [T-SQL kullanarak bir veri ambarı geri](sql-data-warehouse-restore-database-rest-api.md) .

Silinmiş ya da duraklatılmış veri ambarını geri yüklemek için [destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md). 


## <a name="geo-redundant-restore"></a>Coğrafi olarak yedekli geri yükleme
Veri ambarı Azure SQL Data Warehouse, seçilen performans düzeyinde destekleyen herhangi bir bölgeyi geri yükleyebilirsiniz. 

> [!NOTE]
> Coğrafi olarak yedekli geri yüklemeyi gerçekleştirmek için bu özellik dışında çevirdiniz gerekir değil.
> 
> 

## <a name="restore-timeline"></a>Zaman çizelgesi geri yükleme
Son yedi gün içinde herhangi bir kullanılabilir geri yükleme noktası bir veritabanını geri yükleyebilirsiniz. Anlık görüntüler için dört sekiz saatte başlatmak ve yedi gün için kullanılabilir. Bir anlık görüntü yedi günden daha eski olduğunda süresi dolmadan ve geri yükleme noktası artık kullanılamıyor. 

Yedeklemeleri duraklatılmış veri ambarına karşı durum değil. Veri ambarınız yedi günden fazla bir süre için duraklatıldığında, herhangi bir geri yükleme noktası olmaz. 

## <a name="restore-costs"></a>Maliyetleri geri yükleme
Azure Premium Storage hızında geri yüklenen veri ambarı için depolama ücret faturalandırılır. 

Geri yüklenen veri ambarı duraklatmak, depolama Azure Premium Storage oranı için ücretlendirilirsiniz. Duraklatma avantajı, bilgi işlem kaynakları için sizden ücret istenmese olmasıdır.

SQL Data Warehouse fiyatlandırması hakkında daha fazla bilgi için bkz: [SQL Data Warehouse fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="restore-use-cases"></a>Kullanım örnekleri geri yükleme
Birincil veri ambarı geri yüklemek için yanlışlıkla veri kaybı veya bozulması sonra verileri kurtarmak için kullanılır. Veri ambarı geri yükleme, bir yedekleme yedi günden daha uzun süre tutmak için de kullanabilirsiniz. Yedeklemeyi geri yüklendikten sonra veri ambarı çevrimiçi olması ve, duraklatma, işlem maliyetleri süresiz olarak kaydetmek için. Duraklatılmış veritabanı depolama ücretleri Azure Premium Storage oranı doğurur. 

## <a name="next-steps"></a>Sonraki adımlar
Olağanüstü durum planlama hakkında daha fazla bilgi için bkz: [iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)
