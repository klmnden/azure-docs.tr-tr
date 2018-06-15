---
title: Azure SQL Data Warehouse yedeklemeleri - anlık görüntüler, coğrafi olarak yedekli | Microsoft Docs
description: Bir geri yükleme noktası veya farklı bir coğrafi bölge için bir Azure SQL Data Warehouse geri yüklemek etkinleştirmeniz SQL Data Warehouse yerleşik veritabanı yedeklemeleri hakkında bilgi edinin.
services: sql-data-warehouse
documentationcenter: ''
author: barbkess
manager: jhubbard
editor: ''
ms.assetid: b5aff094-05b2-4578-acf3-ec456656febd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.custom: backup-restore
ms.date: 10/23/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 159a1d34caba829750da33dbc4ad403fb21cd147
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30198508"
---
# <a name="backup-and-restore-in-sql-data-warehouse"></a>Yedekleme ve geri yükleme SQL veri ambarı
Bu makalede SQL Data Warehouse yedeklemelerin özellikleri açıklanmaktadır. Birincil bölge için anlık görüntü bir veritabanını geri yüklemek için veri ambarı yedekleri kullanın veya coğrafi yedekleme coğrafi eşleştirilmiş bölgenizi geri yükleyin. 

## <a name="what-is-a-data-warehouse-backup"></a>Veri ambarı yedekleme nedir?
Veri ambarı yedeklemesi bir veri ambarı geri yüklemek için kullanabilirsiniz, veritabanınızı kopyasıdır.  SQL veri ambarı dağıtılmış bir sistemde olduğundan, bir veri ambarı yedekleme Azure depolama alanında bulunan çok sayıda dosya oluşur. Veri ambarı yedekleme, hem yerel veritabanı anlık görüntüleri hem de coğrafi yedeklemeleri tüm veritabanlarını ve veri ambarı ile ilişkili olan dosyaları içerir. 

## <a name="local-snapshot-backups"></a>Yerel anlık görüntüsü yedekleri
SQL Data Warehouse veri Ambarınızı gün boyunca anlık görüntüleri alır. Anlık görüntüler yedi gün için kullanılabilir. SQL veri ambarı bir sekiz saat kurtarma noktası hedefi (RPO) destekler. Veri ambarınız birincil bölge içinde herhangi bir son yedi gün içinde alınan anlık görüntü geri yükleyebilirsiniz.

Son anlık görüntünün başlatıldığında görmek için bu sorguyu çevrimiçi SQL veri ambarı üzerinde çalıştırın. 

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs 
order by run_id desc
;
```

## <a name="geo-backups"></a>Coğrafi yedekleri
SQL veri ambarı gerçekleştirir coğrafi yedekleme için günde bir kez bir [eşleştirilmiş veri merkezi](../best-practices-availability-paired-regions.md). RPO bir coğrafi geri yükleme için 24 saattir. Sunucunun coğrafi eşleştirilmiş bölgede coğrafi yedekleme geri yükleyebilirsiniz. coğrafi yedekleme anlık görüntüleri birincil bölgenizde erişemiyor durumunda, veri ambarı geri yükleyebilirsiniz sağlar.

Coğrafi yedeklemeleri varsayılan olarak açıktır. Veri ambarınız esneklik için optimize edilmiştir, yapabilecekleriniz [çıkma](/powershell/module/azurerm.sql/set-azurermsqldatabasegeobackuppolicy) istiyorsanız. En iyi duruma getirilmiş ile coğrafi yedek dışında kabul edemiyor işlem performans katmanı.

## <a name="backup-costs"></a>Yedekleme maliyetleri
Azure Premium Storage ve coğrafi olarak yedekli depolama için bir satır öğesi için bir satır öğesi Azure faturasını sahip fark edeceksiniz. Premium depolama ücret anlık görüntüleri içeren birincil bölgede verilerinizi depolamak için toplam maliyetidir.  Coğrafi olarak yedekli ücret coğrafi yedeklemelerini depolamak için maliyet kapsar.  

Maliyet birincil veri ambarı ve Azure Blob anlık görüntü yedi gün için toplam yakın TB yuvarlanır. Örneğin, veri Ambarınızı 1,5 TB ise ve 100 GB anlık görüntüleri kullanmak, 2 TB veri Azure Premium Storage hızlarında için faturalandırılır. 

> [!NOTE]
> Her anlık görüntü başlangıçta boş ve birincil veri ambarına değişiklik gibi büyür. Tüm anlık görüntüleri veri ambarı değişiklikleri boyutunu artırın. Bu nedenle, anlık görüntüler için depolama maliyetleri değişme oranı göre artar.
> 
> 

Coğrafi olarak yedekli depolama birimi kullanıyorsanız, ayrı depolama ücret alırsınız. Coğrafi olarak yedekli depolama standart okuma erişimli coğrafi olarak yedekli depolama (RA-GRS) oranı faturalandırılır.

SQL Data Warehouse fiyatlandırması hakkında daha fazla bilgi için bkz: [SQL Data Warehouse fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="snapshot-retention-when-a-data-warehouse-is-paused"></a>Veri ambarı duraklatıldığında, anlık görüntü saklama
SQL Data Warehouse anlık görüntülerini oluşturma değil ve bir veri ambarı duraklatıldığında anlık görüntüleri dolmaz. Veri ambarı duraklatıldığında anlık görüntü yaş değiştirmez. Anlık görüntü saklama, veri ambarı Takvim günleri çevrimiçi olduğu gün sayısını temel alır.

Örneğin, bir anlık görüntü 4'e 1 Ekim başlatır ve 4'e veri ambarı Ekim 3 olarak duraklatıldı, anlık görüntüleri iki güne kadar eski demektir. Veri ambarı tekrar çevrimiçi olduğunda iki gün anlık görüntüsüdür. Veri ambarı Ekim 5 4'e çevrimiçi olursa, anlık görüntü iki gün önce yapılmışsa ve daha fazla beş gün boyunca devam eder.

Veri ambarı tekrar çevrimiçi olduğunda, SQL Data Warehouse Yeni Anlık görüntülerin sürdürür ve yedi günden fazla veri olduğunda anlık görüntüleri süresi dolar.

## <a name="can-i-restore-a-dropped-data-warehouse"></a>Bırakılan veri ambarını geri yükleyebilir miyim?
Veri ambarı bıraktığınızda, SQL Data Warehouse son anlık görüntü oluşturur ve yedi gün için kaydeder. Veri ambarı silme işlemi sırasında oluşturulan son geri yükleme noktası geri yükleyebilirsiniz. 

> [!IMPORTANT]
> Mantıksal bir SQL server örneği silerseniz örneği ait tüm veritabanlarının da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz.
> 

## <a name="next-steps"></a>Sonraki adımlar
SQL data warehouse geri yüklemek için bkz: [SQL data warehouse geri](sql-data-warehouse-restore-database-overview.md).

İş sürekliliğine genel bakış için bkz: [iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)
