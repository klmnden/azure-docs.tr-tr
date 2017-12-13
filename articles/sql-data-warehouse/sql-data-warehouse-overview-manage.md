---
title: "Azure SQL Data Warehouse veritabanlarında yönetme | Microsoft Docs"
description: "SQL veri ambarı veritabanlarını yönetmeye genel bakış. Yönetim Araçları, Dwu ve sorgu performansı, iyi bir güvenlik ilkeleri oluşturma ve veri bozulması veya bölgesel bir kesintinin bir veritabanını geri yükleme sorunlarını giderme genişleme performans içerir."
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: d7b81c12c31fe7de40acca6baa8972e65c306ee0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse veritabanlarında yönetme
SQL veri ambarı veritabanlarınızı yönetme birçok nokta otomatikleştirir. Örneğin, performans ölçeklendirmek için ayarlamak ve işlem kaynaklarını doğru düzeyde için ödeme ve SQL Data Warehouse ölçeğini genişletme ve geri ölçeklendirme tüm yapması izin vermek yeterlidir.

Kuşkusuz yanı sıra performans gereksinimlerinizi belirlemek için uzun süre çalışan sorgularda sorun giderme, iş yükünü izlemek isteyeceksiniz. Kullanıcı ve oturum açma izinlerini yönetmek için birkaç güvenlik görevlerini gerçekleştirmek gerekir.

Bu genel bakışta bu yönlerini SQL veri ambarı yönetimi ele alınmaktadır.

* Yönetim araçları
* Bilgi işlem
* Duraklatma ve sürdürme
* Performansı en iyi uygulamalar
* Sorgu izlemesi
* Güvenlik
* Yedekleme ve geri yükleme

## <a name="management-tools"></a>Yönetim araçları
SQL Data Warehouse veritabanlarında yönetmek için çeşitli araçları kullanabilirsiniz. Veritabanları yönetirken, yapmanız gereken görevinin her türü için aracı Tercihler geliştireceksiniz.

### <a name="azure-portal"></a>Azure portalına
[Azure portal] [ Azure portal] burada oluşturabilir, güncelleştirme ve veritabanlarını silin ve veritabanı kaynaklarını izleme web tabanlı portal değil. Bu harika bir araçtır yalnızca veri ambarı veritabanlarını, az sayıda yönetme Azure ile çalışmaya Başlarken veya hızlı bir şekilde bir şey yapmanız gerekir.

Azure portalı ile çalışmaya başlamak için bkz: [SQL veri ambarı (Azure portalı) oluşturma][Create a SQL Data Warehouse (Azure portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server veri araçları Visual Studio
[SQL Server veri Araçları] [ SQL Server Data Tools] (SSDT) Visual Studio için bağlanmanıza olanak sağlayan yönetmek ve veritabanlarınızı geliştirin. Bir uygulama geliştiricisi Visual Studio'ya veya diğer tümleşik geliştirme ortamlarını (IDE) hakkında bilgi sahibi değilseniz, Visual Studio'da SSDT kullanmayı deneyin.

SSDT görselleştirme, bağlanmak ve SQL Data Warehouse veritabanlarında komut dosyaları yürütme olanak tanıyan SQL Server Nesne Gezgini içerir. Hızlı bir şekilde SQL veri ambarı'na bağlanmak için basitçe tıklayabilirsiniz **Visual Studio'da Aç** ne zaman veritabanı görüntüleme Azure portalında ayrıntıları çubuğu komut düğmesi.  

Visual Studio'da SSDT ile çalışmaya başlamak için bkz: [sorgu Azure SQL Data Warehouse Visual Studio ile][Query Azure SQL Data Warehouse with Visual Studio].

### <a name="command-line-tools"></a>Komut satırı araçları
Komut satırı araçlarını, iş yüklerini otomatikleştirmek için idealdir.  PowerShell ve sqlcmd işlemlerinizi otomatikleştirmek için iki harika yöntemleridir.  Çok sayıda mantıksal sunucuları yönetme ve gerekli görevleri komut dosyası ve daha sonra otomatik olarak kaynak değişiklikleri bir üretim ortamında dağıtmak için bu araçları öneririz.

### <a name="dynamic-management-views"></a>Dinamik Yönetim görünümlerini
Dmv'leri SQL veri ambarını yönetme ekmek ve ezmesi ' dir. Portalda ortaya çıkarır neredeyse tüm bilgi üzerinde Dmv'leri kullanır. SQL veri ambarı Dmv'leri listesini görmek için bkz: [SQL veri ambarı sistem görünümleri][SQL Data Warehouse system views].

Başlamak için bkz: [bağlanma ve sorgu sqlcmd ile][Connect and query with sqlcmd], ve [veritabanı (PowerShell) oluşturma][Create a database (PowerShell)].

## <a name="scale-compute"></a>Bilgi işlem
SQL veri ambarı'nda performans out veya geri artırarak veya azaltarak işlem kaynakları CPU, bellek ve g/ç bant genişliği tarafından hızlı bir şekilde ölçeklendirebilirsiniz. Performans ölçeklendirmek için yapmanız gereken tek şey SQL Data Warehouse veritabanınıza ayırır veri ambarı birimlerini (Dwu'lar) sayısını ayarlayın. SQL veri ambarı hızlı bir şekilde değişiklik yapar ve donanım veya yazılım için temel alınan tüm değişiklikleri işler.

Dwu ölçeklendirme hakkında daha fazla bilgi için bkz: [ölçeklendirme performans].

## <a name="pause-and-resume"></a>Duraklatma ve sürdürme
Maliyet tasarrufu sağlamak duraklatma ve sürdürme işlem kaynakları isteğe bağlı. Örneğin, veritabanı gece sırasında ve hafta sonları kullandığınız olmaz, bu saatlerde duraklatma ve gün boyunca sürdürün. Veritabanı duraklatıldığında Dwu için sizden ücret olmaz.

Daha fazla bilgi için bkz: [duraklatma işlem][Pause compute], ve [sürdürme işlem][Resume compute].

## <a name="performance-best-practices"></a>Performansı en iyi uygulamalar
İpuçları ve en iyi sağ başından iş püf noktalarını bulma çok fazla ile yeni bir teknoloji Başlarken zaman kaydedebilirsiniz.  Bizim konuları çoğunu tamamında en iyi yöntemler bulacaksınız.

Birçok İş yükünüzün geliştirilirken en önemli konular özetini görmek için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

## <a name="query-monitoring"></a>Sorgu izlemesi
Bazen bir sorgu uzun çalışıyor, ancak hangi sorunlu biri emin değil. SQL veri ambarı hangi sorgu çok uzun sürüyor bulmak için kullanabileceğiniz dinamik yönetim görünümlerini (Dmv'leri) sahiptir.

Uzun süre çalışan sorguları bulmak için bkz: [Dmv'leri kullanarak, iş yükünü izlemek][Monitor your workload using DMVs].

## <a name="security"></a>Güvenlik
Güvenli bir sistemde korumak için uyarıdaki olabilir ve her tür yetkisiz erişime karşı koruma. Güvenlik duvarı kuralları yalnızca yetkili IP adreslerini bağlanabilir karşılandığından emin olmak bir güvenlik sistemi gerekir. Kullanıcı kimlik bilgilerinin düzgün bir kimlik doğrulaması gerekir. Bir kullanıcı veritabanına bağlandı sonra kullanıcı yalnızca en az çeşitli eylemleri gerçekleştirmek için izinlere sahip olmalıdır. Verilerin güvenliğini sağlamak için şifreleme kullanabilirsiniz. Olması önemlidir denetim ve tüm şüpheli etkinlikleri ise olayları yeniden izlemesine şekilde izleme.

Güvenlik yönetme hakkında bilgi edinmek için üzerinden için head [güvenliğine genel bakış][Security overview].

## <a name="backup-and-restore"></a>Yedekleme ve geri yükleme
Verilerinizin güvenilir backps sahip, herhangi bir üretim veritabanını önemli bir parçasıdır. SQL veri ambarı otomatik olarak düzenli aralıklarla, etkin veritabanlarını yedekleyerek, verilerinizi güvenli tutar. Bu yedeklemeler, buradan verilerinizi bozulmuş veya yanlışlıkla veri veya veritabanı bırakılan senaryolarından kurtarmanıza olanak tanır.  Veri yedekleme zamanlaması için bkz: bekletme ilkesi ve bir veritabanını geri yüklemek nasıl [anlık görüntüden geri][Restore from snapshot].

## <a name="next-steps"></a>Sonraki adımlar
İyi veritabanı tasarım ilkeleri SQL Data Warehouse veritabanınızda yönetmenizi kolaylaştırır kullanma. Daha fazla bilgi için üzerinden için head [geliştirmeye genel bakış][Development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[ölçeklendirme performans]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
