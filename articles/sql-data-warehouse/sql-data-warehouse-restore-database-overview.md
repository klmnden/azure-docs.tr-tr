---
title: Azure veri ambarı - yerel ve coğrafi olarak yedekli geri | Microsoft Docs
description: Azure SQL Data Warehouse bir veritabanına kurtarmak için veritabanı geri yükleme seçeneklerine genel bakış.
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: ''
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: abf8f0b1005aec71812dc8ebfd12fe65250d7a0e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
ms.locfileid: "30198480"
---
# <a name="sql-data-warehouse-restore"></a>SQL veri ambarı geri yükleme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

SQL veri ambarı hem yerel hem de coğrafi geri yükleme, veri ambarı olağanüstü bir parçası olarak kurtarma özellikleri sunar. Veri ambarınız birincil bölge içinde bir geri yükleme noktası geri yüklemek için veri ambarı yedekleri kullanın veya farklı bir coğrafi bölgeye geri yüklemek için coğrafi olarak yedekli yedeklemeleri kullanın. Bu makalede, bir veri ambarı geri yükleme özellikleri açıklanmaktadır.

## <a name="what-is-a-data-warehouse-restore"></a>Veri ambarı geri nedir?
Bir veri ambarı geri yükleme, varolan bir yedek kopyadan oluşturulan yeni veri ambarı veya silinen veri ambarı ' dir. Geri yüklenen veri ambarı yedeklenen veri ambarı belirli bir zamanda yeniden oluşturur. SQL veri ambarı dağıtılmış bir sistemde olduğundan, bir veri ambarı geri yükleme dosyalarından Azure BLOB'ları depolanan birçok yedekleme oluşturulur. 

Veritabanı geri yükleme tüm iş sürekliliği ve olağanüstü durum kurtarma stratejisi önemli bir parçası olduğundan, verilerinizin yanlışlıkla Bozulması veya silme işlemi sonrasında yeniden oluşturur.

Daha fazla bilgi için bkz.

* [SQL veri ambarı yedekleri](sql-data-warehouse-backups.md)
* [İş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Veri ambarı geri yükleme noktaları
Azure Premium Storage kullanmanın avantajı birincil veri ambarı yedeklemek için Azure Storage Blobuna anlık görüntüleri SQL Data Warehouse kullanır. Her anlık görüntü anlık görüntü başlama zamanı temsil eden bir geri yükleme noktası vardır. Veri ambarı geri yüklemek için bir geri yükleme noktası seçin ve geri yükleme komutu.  

SQL veri ambarı her zaman yeni bir veri ambarı yedeğini geri yükler. Geri yüklenen veri ambarı ve geçerli bir tutun veya bunlardan birini silin. Geçerli veri ambarıyla geri yüklenen veri ambarı ile değiştirmek istiyorsanız, onu yeniden adlandırabilirsiniz.

Silinmiş ya da duraklatılmış veri ambarını geri yüklemeniz gerekiyorsa şunları yapabilirsiniz [destek bileti oluşturma](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore the last available restore point.

Yes, for the next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps the data warehouse and its snapshots for seven days just in case you need the data. After seven days, you won't be able to restore to any of the restore points. -->

## <a name="geo-redundant-restore"></a>Coğrafi olarak yedekli geri yükleme
Veri ambarı Azure SQL Data Warehouse, seçilen performans düzeyinde destekleyen herhangi bir bölgeyi geri yükleyebilirsiniz. 9000 ve 18000 DWU desteklenmez, tüm bölgelerde Önizleme süresince unutmayın.

> [!NOTE]
> Coğrafi olarak yedekli geri yüklemeyi gerçekleştirmek için bu özellik dışında çevirdiniz gerekir değil.
> 
> 

## <a name="restore-timeline"></a>Zaman çizelgesi geri yükleme
Son yedi gün içinde herhangi bir kullanılabilir geri yükleme noktası bir veritabanını geri yükleyebilirsiniz. Anlık görüntüler için dört sekiz saatte başlatmak ve yedi gün için kullanılabilir. Bir anlık görüntü yedi günden daha eski olduğunda süresi dolmadan ve geri yükleme noktası artık kullanılamıyor.

## <a name="restore-costs"></a>Maliyetleri geri yükleme
Azure Premium Storage hızında geri yüklenen veri ambarı için depolama ücret faturalandırılır. 

Geri yüklenen veri ambarı duraklatmak, depolama Azure Premium Storage oranı için ücretlendirilirsiniz. Duraklatma avantajı DWU bilgi işlem kaynakları için sizden ücret istenmese olmasıdır.

SQL Data Warehouse fiyatlandırması hakkında daha fazla bilgi için bkz: [SQL Data Warehouse fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Geri yükleme için de kullanır
Birincil veri ambarı geri yüklemek için yanlışlıkla veri kaybı veya bozulması sonra verileri kurtarmak için kullanılır.

Veri ambarı geri yükleme, bir yedekleme yedi günden daha uzun süre tutmak için de kullanabilirsiniz. Yedeklemeyi geri yüklendikten sonra veri ambarı çevrimiçi olması ve, duraklatma, işlem maliyetleri süresiz olarak kaydetmek için. Duraklatılmış veritabanı depolama ücretleri Azure Premium Storage oranı doğurur. 

## <a name="related-topics"></a>İlgili konular
### <a name="scenarios"></a>Senaryolar
* İş sürekliliğine genel bakış için bkz: [iş sürekliliğine genel bakış](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

Veri ambarı geri yüklemeyi gerçekleştirmek için kullanarak geri yükleyin:

* Azure portal, bkz: [Azure portalını kullanarak bir veri ambarı geri yükle](sql-data-warehouse-restore-database-portal.md)
* PowerShell cmdlet'leri görmek [PowerShell cmdlet'lerini kullanarak bir veri ambarı geri yükle](sql-data-warehouse-restore-database-powershell.md)
* REST API için bkz: [REST API'lerini kullanarak bir veri ambarı geri yükle](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
