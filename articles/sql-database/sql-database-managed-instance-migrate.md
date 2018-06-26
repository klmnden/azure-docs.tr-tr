---
title: Azure SQL veritabanı yönetilen örneği için SQL Server örneği geçirme | Microsoft Docs
description: Azure SQL veritabanı yönetilen örneği için bir SQL Server örneği geçirmek öğrenin.
keywords: veritabanı geçişi,sql server veritabanı geçişi,veritabanı taşıma araçları,veritabanı taşıma,sql veritabanı geçişi
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: managed instance
ms.topic: conceptual
ms.date: 04/10/2018
ms.author: bonova
ms.openlocfilehash: a5a81279726e5c221d9ae4734466a04ae5912af6
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36936811"
---
# <a name="sql-server-instance-migration-to-azure-sql-database-managed-instance"></a>SQL Server örneği geçiş yönetilen Azure SQL veritabanı örneğine

Bu makalede, Azure SQL veritabanı yönetilen örneğine (Önizleme) bir SQL Server 2005 veya üzeri sürüm örneğini geçirme yöntemleri hakkında bilgi edinin. 

SQL Veritabanı Yönetilen Örneği, mevcut SQL Veritabanı hizmetinin genişletilmiş halidir ve tek veritabanları ile esnek havuzlara ek olarak üçüncü bir dağıtım seçeneği sağlar.  Uygulama yeniden olmadan veritabanı yükseltme-ve-kaydırma tam yönetilen bir PaaS sağlamak için tasarlanmıştır. SQL Veritabanı Yönetilen Örneği, şirket içi SQL Server programlama modeli için yüksek düzeyde uyumluluk sağlamasının yanı sıra SQL Server özelliklerinin büyük bir çoğunluğu ile bunlara eşlik eden araç ve hizmetleri destekleyecek şekilde sunulur.

Yüksek düzeyde, uygulama geçiş işlemi aşağıdaki diyagramda şuna benzer:

![geçiş işlemi](./media/sql-database-managed-instance-migration/migration-process.png)

- [Yönetilen örneği uyumluluk değerlendirme](sql-database-managed-instance-migrate.md#assess-managed-instance-compatibility)
- [Uygulama bağlantı seçeneği seçin](sql-database-managed-instance-migrate.md#choose-app-connectivity-option)
- [En iyi şekilde boyutlandırılmış yönetilen örneğini dağıtma](sql-database-managed-instance-migrate.md#deploy-to-an-optimally-sized-managed-instance)
- [Geçiş yöntemi seçin ve geçirme](sql-database-managed-instance-migrate.md#select-migration-method-and-migrate)
- [Uygulama izleme](sql-database-managed-instance-migrate.md#monitor-applications)

> [!NOTE]
> Tek bir veritabanı tek veritabanı veya esnek havuz geçirmek için bkz [bir SQL Server veritabanını Azure SQL veritabanına geçirme](sql-database-cloud-migrate.md).

## <a name="assess-managed-instance-compatibility"></a>Yönetilen örneği uyumluluk değerlendirme

İlk olarak, yönetilen örnek uygulamanızı veritabanı gereksinimleri ile uyumlu olup olmadığını belirler. Yönetilen örneği kolay yükseltme ve shift geçiş için çoğu şirket içi SQL Server kullanan mevcut uygulamaları ya da sanal makineler sağlamak için tasarlanmıştır. Ancak, bazen özellikleri gerektirebilir veya henüz desteklenmemektedir yetenekleri ve geçici bir çözüm uygulama maliyeti çok yüksek. 

Kullanım [veri geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview) olası algılamak için Azure SQL veritabanı etkileyen veritabanı işlevselliği uyumluluk sorunları. Yönetilen örneği geçiş hedef olarak DMA henüz desteklemiyor, ancak değerlendirme Azure SQL veritabanına karşı çalışırlar ve dikkatle bildirilen özellik eşliği ve uyumluluk sorunları ürün belgelerine karşı listesini gözden geçirmek için önerilir. Azure SQL veritabanı geçiş önleme engelleme sorunları çoğunu yönetilen örneğiyle kaldırılmıştır. Örneği, çapraz veritabanı sorguları gibi özellikleri, aynı örneğinde, diğer SQL kaynakları, CLR, genel geçici tablolara, bağlantılı sunucu arası veritabanı işlemleri için örnek düzeyi görünümleri, hizmet aracısı ve benzeri yönetilen durumlarda kullanılabilir. 

Ancak, bazı durumlar vardır alternatif bir seçenek gibi göz önünde bulundurmanız gereken zaman [azure'daki sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/). İşte bazı örnekler:

- İşletim sistemi veya dosya sistemi, örneği için yükleme üçüncü taraf veya SQL Server ile aynı sanal makineye özel aracıları doğrudan erişimi gerekiyorsa.
- Yine, FILESTREAM gibi desteklenmeyen özellikleri katı bağımlılık varsa / FileTable, PolyBase ve çapraz örnek işlemleri.
- Kesinlikle, SQL Server'ın belirli bir sürümünde kalmak ihtiyacınız varsa (2012 örneği için).
- İşlem gereksinimlerinizi çok daha düşük olduğunda bu yönetilen örnek genel önizlemede sunar (bir vCore örneği için) ve veritabanı birleştirme kabul edilebilir seçeneği değil.

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>En iyi şekilde boyutlandırılmış yönetilen örneğini dağıtma

Yönetilen örneği buluta taşımak için planlama şirket içi iş yükleri için özel olarak oluşturulmuştur. Doğru düzeyde, iş yükleri için kaynakları seçerek daha fazla esneklik sağlayan yeni bir satın alma modeli sunar. Şirket içi dünyada, fiziksel çekirdekleri kullanarak bu iş yükleri boyutlandırma için büyük olasılıkla bilirsiniz. Yönetilen örneği için yeni satın alma modeli sanal çekirdek ya da "ek depolama alanı ve g/ç kullanılabilir" vCores ayrı olarak temel aldığı. VCore modeldir daha basit bir yol, kullandığınız karşı bulutta işlem gereksinimlerinizi anlamak için şirket içi bugün. Bu yeni model hedef ortamınızda bulut sağ boyutuna sağlar.

İşlem seçebilirsiniz ve dağıtım sırasında depolama kaynaklarını süresi ve daha sonra uygulamanız için kesintiye neden oluşturmaksızın değiştirin.

![Yönetilen örneği boyutlandırma](./media/sql-database-managed-instance-migration/managed-instance-sizing.png)

Sanal ağ altyapısı ve yönetilen bir örneği oluşturmayı öğrenmek için bkz: [bir yönetilen örneği oluşturmayı](sql-database-managed-instance-create-tutorial-portal.md).

> [!IMPORTANT]
> Hedef VNet ve alt ağ her zaman içinde belge tutmanız önemlidir [yönetilen örneği VNET gereksinimleri](sql-database-managed-instance-vnet-configuration.md#requirements). Tüm uyumsuzluk yeni örnekleri oluşturma veya önceden oluşturulmuş bu kullanarak engelleyebilirsiniz.

## <a name="select-migration-method-and-migrate"></a>Geçiş yöntemi seçin ve geçirme

Şirket içi veya Iaas veritabanı uygulamaları yığın veritabanı geçiş gerektiren örneği hedefleri kullanıcı senaryolar yönetilen. Kaldırın ve arka uç düzenli olarak örnek düzeyi kullanın ve / veya veritabanları arası işlevler uygulamalara shift gerektiğinde bunların en iyi seçimdir. Senaryonuz varsa, tüm örneğini rearchitecture gerek olmadan karşılık gelen bir ortama Azure uygulamalarınızı taşıyabilirsiniz. 

SQL örnekleri taşımak için dikkatli bir şekilde planlamanız gerekir:

-   (Birlikte bulunan adreslerdir aynı örneğinde çalışan) için gereken tüm veritabanları geçişi
-   Uygulamanızın, oturum açma, kimlik bilgileri, SQL Aracısı işleri ve işleçler ve sunucu düzeyi Tetikleyiciler dahil olmak üzere bağımlı örnek düzeyinde nesneleri geçiş. 

Yönetilen örneği bunlar yerleşik olarak gibi bazı platform normal DBA etkinlikleri temsilci olanak sağlayan tam olarak yönetilen bir hizmettir. Bu nedenle, bazı örnek düzeyinde veri, düzenli yedeklemeler veya her zaman açık yapılandırması, bakım işleri gibi olarak geçirilmesi gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Yönetilen örneği (şu anda bunlar yalnızca desteklenen geçiş yöntemleridir) aşağıdaki veritabanı geçiş seçeneklerini destekler:

- Azure veritabanı geçiş hizmeti - sıfır kapalı kalma süresi ile geçiş
- URL - yerel YEDEKTEN geri yükleyin miktar kapalı kalma süresi gerektirir ve SQL Server yerel yedeklemelerden kullanır

### <a name="azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti

[Azure veritabanı geçiş hizmeti (DMS)](../dms/dms-overview.md) en az kapalı kalma süresi ile Azure veri platformlar için birden fazla veritabanı kaynaktan sorunsuz geçiş sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmet, var olan üçüncü taraf ve SQL Server veritabanlarınızı Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri genel Önizleme sırasında Azure SQL veritabanını, örneği yönetilen ve SQL Server bir Azure sanal makinesine içerir. DMS geçiş önerilen yöntem, kurumsal iş yükleri için ' dir. 

DMS için bu senaryo ve yapılandırma adımları hakkında daha fazla bilgi için bkz: [şirket içi veritabanı DMS kullanarak örneğini yönetilen geçirme](../dms/tutorial-sql-server-to-managed-instance.md).  

### <a name="native-restore-from-url"></a>URL yerel YEDEKTEN geri yükleyin

SQL Server şirket içinden alınan yerel yedeklemeler (.bak dosyaları) geri yükleme veya [sanal makinelerde SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/), kullanılabilir [Azure Storage](https://azure.microsoft.com/services/storage/), SQL DB yönetilen örneğinde anahtar özelliklerinden biridir, hızlı ve kolay çevrimdışı etkinleştirir geçiş veritabanı. 

Aşağıdaki diyagramda yüksek düzeyde işlemi açıklanmaktadır:

![geçiş akış](./media/sql-database-managed-instance-migration/migration-flow.png)

Aşağıdaki tabloda, kullanmakta olduğunuz kaynak SQL Server sürümüne bağlı olarak kullanabileceğiniz yöntemi ilgili daha fazla bilgi sağlar:

|Adım|SQL altyapısı ve sürüm|Yedekleme / geri yükleme yöntemi|
|---|---|---|
|Azure depolama birimine yedek alın|Önceki SQL 2012 SP1 CU2|Azure depolama alanına doğrudan .bak dosyası yükleme|
||2012 SP1 CU2 - 2016|Kullanım dışı doğrudan Yedekleme'yi kullanarak [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) sözdizimi|
||2016 ve üstü|Doğrudan Yedekleme'yi kullanarak [ile SAS kimlik bilgisi](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url)|
|Azure depolama biriminden yönetilen örneğine geri yükleme|[Geri yükleme URL'den SAS kimlik bilgisi](sql-database-managed-instance-restore-from-backup-tutorial.md)|

> [!IMPORTANT]
> Sistem veritabanlarının geri yükleme desteklenmez. (Asıl veya msdb veritabanlarında depolanır) örnek düzeyi nesneleri geçirmek için bunları komut dosyası ve hedef örneğinde T-SQL betikleri çalıştırmak için önerilir.

Bir yönetilen bir SAS kimlik bilgisi kullanma örneği için bir veritabanı yedeğini geri içeren tam bir öğretici için bkz: [yönetilen bir örneğine yedekten geri](sql-database-managed-instance-restore-from-backup-tutorial.md).

## <a name="monitor-applications"></a>Uygulamaları izleme

Uygulama davranışına ve geçiş sonrasında performans izleme. Yönetilen örneğinde bazı değişiklikler yalnızca bir kez etkinleştirilmiş [veritabanı uyumluluk düzeyi değiştirildi](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database). Azure SQL veritabanı için veritabanı geçiş özgün uyumluluk düzeyi çoğu durumda tutar. Bir kullanıcı veritabanı uyumluluk düzeyi 100 veya daha yüksek geçişten önce idiyse, aynı geçişten sonra kalır. Bir kullanıcı veritabanı uyumluluk düzeyi 90 yükseltilen veritabanında geçişten önce olduysa uyumluluk düzeyi en düşük desteklenen uyumluluk düzeyini yönetilen örneğinde olan 100'e ayarlanır. Sistem veritabanlarının uyumluluk düzeyi 140 ' dir.

Geçiş riskleri azaltmak için veritabanı uyumluluk düzeyi yalnızca performans izleme sonra değiştirin. Kullanım Query Store açıklandığı gibi iş yükü performansı önce ve sonra veritabanı uyumluluk düzeyi değiştirme hakkında bilgi almak için en iyi aracı olarak [tutmak performans kararlılık yükseltme sırasında SQL Server sürüme](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade).

Tam olarak yönetilen bir platformda olduktan sonra SQL veritabanı hizmetinin bir parçası olarak otomatik olarak sağlanan avantajları alın. Örneğin, yönetilen örneğinde yedeklemeler oluşturmak zorunda değilsiniz - hizmet yedeklemeler sizin için otomatik olarak gerçekleştirir. Artık zamanlama, alma ve yedeklemeleri yönetme hakkında endişelenmeniz gerekir. Yönetilen örneği kullanarak bu Bekletme dönemi içinde zaman içinde herhangi bir noktaya kadar geri yükleme yeteneği sağlar [noktası zaman Kurtarma (PITR içinde)](sql-database-recovery-using-backups.md#point-in-time-restore). Genel Önizleme sırasında saklama süresi yedi gün için sabittir.
Ayrıca, yüksek oranda kullanılabilir ayarlama endişelenmeniz gerekmez [yüksek kullanılabilirlik](sql-database-high-availability.md) yerleşiktir.

Güvenliği güçlendirmek için kullanabileceğiniz özelliklerden bazılarını kullanmayı dikkate alın:
- Azure Active Directory ile kimlik doğrulaması veritabanı düzeyinde
- Denetim ve tehdit Algılama'nın etkinliklerini izlemek için
- Hassas ve ayrıcalıklı veri erişimini denetleme ([satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) ve [dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)).

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekleri hakkında daha fazla bilgi için bkz: [yönetilen örneği nedir?](sql-database-managed-instance.md).
- Bir yedekten geri içeren bir öğretici için bkz [bir yönetilen örneği oluşturmayı](sql-database-managed-instance-create-tutorial-portal.md).
- DMS kullanarak Eğitmen gösteren geçiş için bkz: [şirket içi veritabanı DMS kullanarak örneğini yönetilen geçirme](../dms/tutorial-sql-server-to-managed-instance.md).  
