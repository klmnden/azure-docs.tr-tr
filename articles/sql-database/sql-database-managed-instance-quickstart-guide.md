---
title: Hızlı Başlangıç - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Azure SQL veritabanı - yönetilen örnek hızlı başlama hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlr
manager: craigg
ms.date: 03/25/2019
ms.openlocfilehash: 739afe52403633b1a37f57f0005a85972cc78a39
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61314747"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Yönetilen örnek Azure SQL veritabanı ile çalışmaya başlama

[Yönetilen örnek](sql-database-managed-instance-index.yml) dağıtım seçeneği, veritabanı ile % 100 yerel sağlayarak en son SQL Server şirket içi (Enterprise Edition) veritabanı altyapısı, uyum yakın oluşturur [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) ortak güvenlik endişelerini ortadan uygulama ve [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) şirket içi SQL Server müşterileri için yeterli. Bu makalede, hızlı bir şekilde yapılandırın ve yönetilen örnek oluşturma ve veritabanlarınızı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı genel bakış

Şu hızlı başlangıçlarda hızlı bir şekilde yönetilen örnek oluşturma, bir sanal makineyi yapılandırmak veya konuma VPN bağlantısı için istemci uygulaması işaret olanak sağlar ve yeni, kullanarak yönetilen örnek bir veritabanı geri bir `.bak` dosya.

### <a name="configure-environment"></a>Ortamı yapılandırma

İlk adım, ilk yönetilen Örneğinize nereye yerleştirilir ve bilgisayar veya sanal makine nerede yönetilen örnek sorguları çalıştırma bağlantısını etkinleştirme ağ ortamı oluşturmak gerekir. Aşağıdaki kılavuzlarda kullanabilirsiniz:

- [Azure portalını kullanarak yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md). Azure portalında, gerekli parametreleri (kullanıcı adı/parola, en fazla depolama alanı miktarı ve çekirdek sayısı) yapılandırma ve otomatik olarak Azure ağ ortamında Ağ ayrıntıları ve altyapı gereksinimleri hakkında bilmek zorunda kalmadan oluşturun. Yalnızca sahip olduğunuzdan emin olun bir [abonelik türü](sql-database-managed-instance-resource-limits.md#supported-subscription-types) , şu anda izin verilmiştir yönetilen örnek oluşturma. Kullanmak istediğiniz kendi ağ varsa veya istediğiniz ağ özelleştirmek bkz [Azure SQL veritabanı yönetilen örneği için mevcut bir sanal ağ yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md) veya [Azure SQL veritabanı için sanal ağ oluşturma Yönetilen örnek](sql-database-managed-instance-create-vnet-subnet.md).
- Yönetilen örnek genel bir uç nokta ile kendi sanal ağ oluşturulur. İstemci uygulama erişimi için aşağıdakilerden birini yapabilirsiniz **(farklı bir alt ağ) aynı sanal ağda VM oluşturma** veya **istemci bilgisayarınızdan Vnet'e noktadan siteye VPN bağlantısı oluşturma** bunlardan birini kullanma Hızlı başlangıçlar:

  - Oluşturma [Azure sanal makinesi yönetilen örneğinde VNet](sql-database-managed-instance-configure-vm.md) istemci uygulama bağlantısı için SQL Server Management Studio dahil olmak üzere.
  - Ayarlanan [noktadan siteye VPN bağlantısı yönetilen Örneğinize](sql-database-managed-instance-configure-p2s.md) üzerinde sahip olduğunuz SQL Server Management Studio ve diğer istemci bağlantı uygulamaları istemci bilgisayardan. Diğer yönetilen Örneğinize ve kendi sanal ağa bağlantı için iki seçenek budur.

  > [!NOTE]
  > Express route veya siteden siteye bağlantı yerel ağınızdan de kullanabilirsiniz, ancak bu yaklaşımların şu hızlı başlangıçlardan biriyle kapsamı dışında olan.

### <a name="migrate-your-databases"></a>Veritabanlarınızı geçirme

Yönetilen örnek oluşturma ve erişim'ı yapılandırdıktan sonra şirket içi SQL Server ya da Azure Vm'lerini veritabanlarınızı geçirme başlayabilirsiniz. Geçirmek istediğiniz kaynak veritabanında desteklenmeyen bazı özellikler varsa, geçiş başarısız olur. Uyumluluğu denetlemek ve hatalarını önlemek için yükleyebilirsiniz [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) , SQL Server veritabanlarınızı analiz eder ve bulduğu aşağıdakilerden sorunu varlığınıgibibiryönetilenörneğegeçişengelleyin[FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) veya birden çok günlük dosyası. Bu sorunları çözmek, veritabanlarınızı yönetilen örneğe geçiş hazır olursunuz. [Veritabanı deneme Yardımcısı](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) iş yükünüze dayalı bir yönetilen örnek üzerinde tanımlayabilirsiniz böylece oraya giden bir yönetilen örneğe geçiş herhangi bir performans sorunu olarak SQL Server ve yeniden yürütme kaydedebilen başka yararlı bir araçtır.

Yönetilen örnek için veritabanınızı geçirebileceğiniz emin olduktan sonra yönetilen bir örneğinden içine bir veritabanını geri yüklemek için yerel SQL Server geri yükleme özelliklerini kullanabilir bir `.bak` dosya. Veritabanlarını SQL Server veritabanı altyapısı yüklü şirket içi ya da Azure sanal geçirmek için bu yöntemi kullanabilirsiniz. Hızlı Başlangıç için bkz: [yedekten bir yönetilen örneğine geri](sql-database-managed-instance-get-started-restore.md). Bu hızlı başlangıçta, geri bir `.bak` Azure Blob Depolama kullanarak depolanan dosya `RESTORE` Transact-SQL komutu.

> [!TIP]
> Kullanılacak `BACKUP` Transact-SQL komutu, Azure Blob Depolama alanında veritabanınızın bir yedeği oluşturmak için bkz. [URL'ye SQL Server Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Bu hızlı başlangıçlar, hızlı bir şekilde oluşturmanızı, yapılandırmanızı ve yönetilen bir örneği için veritabanı yedeklemesini geri yükleme sağlar. Bazı senaryolarda, özelleştirme veya yönetilen örnek dağıtımı ve gereken ağ ortamı otomatik hale getirmek gerekir. Bu senaryolar aşağıdaki açıklanacaktır.

## <a name="customize-network-environment"></a>Ağ ortamını özelleştirin

VNet/alt ağ kullanarak örneği oluşturulduğunda otomatik olarak yapılandırılabilir ancak [Azure portalında](sql-database-managed-instance-get-started.md), önce VNet parametreleriyle yapılandırabileceğinizden yönetilen örnek oluşturma başlangıç oluşturmak iyi olabilir ve alt ağ. Oluşturun ve ağ ortamını yapılandırmak için en kolay yolu kullanmaktır [Azure kaynak dağıtımını](sql-database-managed-instance-create-vnet-subnet.md) oluşturur ve, ağ ve alt ağ örneği nereye yerleştirilir yapılandırma şablonu. Azure Resource Manager tuşuna basmanız yeterlidir Dağıt düğmesi ve parametrelerle formu doldurun.

Alternatif olarak, kullandığınız [PowerShell Betiği](https://www.powershellmagazine.com/20../../configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) ağ oluşturulmasını otomatik hale getirmek için.

Alternatif olarak, bu kullanabilirsiniz [PowerShell Betiği](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) ağ oluşturulmasını otomatik hale getirmek için.

Zaten bir sanal ağ ve alt ağ, yönetilen örnek dağıtmak istediğiniz varsa, ağ ve alt sağladığını emin olmanız gerekir [gereksinimlerinde](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Bunu kullanın [alt ağınızın düzgün yapılandırıldığını doğrulamak için PowerShell Betiği](sql-database-managed-instance-configure-vnet-subnet.md). Bu betik, ağ ve rapor ve sorunları bildirir, değişiklikler ve sonra VNet/alt ağda gerekli değişiklikleri yapmak için teklifleri doğrular. VNet/alt ağınızın el ile yapılandırmak istemiyorsanız, bu betiği çalıştırın. Ayrıca tüm önemli yeniden yapılandırma ağ altyapınızın sonra çalıştırabilirsiniz. Oluşturup kendi ağ yapılandırmak istiyorsanız, okuma [bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md) ve bu[oluşturup bir yönetilen örnek ortamı yapılandırmaya yönelik kılavuz](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-a-managed-instance"></a>Yönetilen bir örneğinin oluşturulmasını otomatik hale getirme

 Önceki adımda açıklandığı gibi ağ ortamını oluşturmadıysanız, Azure portalı, sizin için neler yapabileceğini – tek dezavantajı, bunu daha sonra değiştiremezsiniz bazı varsayılan parametreler yapılandırır gerçeğidir. Alternatif olarak kullanabilirsiniz:

- [PowerShell](scripts/sql-database-create-configure-managed-instance-powershell.md)
- [PowerShell Resource Manager şablonu ile](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)
- [Azure CLI](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).
- [Resource Manager Şablonu](sql-database-single-database-get-started-template.md)

## <a name="migrating-to-a-managed-instance-with-minimal-downtime"></a>En düşük kapalı kalma süresi ile bir yönetilen örneğe geçirme

Bu hızlı başlangıç makalelerinde hızlı bir şekilde bir yönetilen örneği ve veritabanlarınızı yerel taşımak etkinleştirme `RESTORE` yeteneği. Bununla birlikte, yerel `RESTORE`, geri (ve Azure Blob depolama alanına kopyalandığından değilse zaten var. depolanan) veritabanları için beklemeniz gerekir. Bu, bazı kapalı kalma süresi, uygulamanızın daha büyük veritabanları için özellikle neden olur. Üretim veritabanınız taşımak için kullanın [veri geçiş hizmeti (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) veritabanınızı en düşük kapalı kalma süresi ile geçirilecek. DMS bunu, kaynak veritabanınızı geri yüklenen yönetilen örnek veritabanına yapılan değişiklikleri artımlı olarak ileterek gerçekleştirir. Bu şekilde uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresiyle hızlı bir şekilde geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bulma bir [üst düzey burada yönetilen örneğinde desteklenen özellikler listesini](sql-database-features.md) ve [ayrıntıları ve bilinen sorunlar](sql-database-managed-instance-transact-sql-information.md).
- Hakkında bilgi edinin [yönetilen örnek Teknik Özellikleri](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits).
- Nasıl daha gelişmiş bulma-için kullanıcının [Azure SQL veritabanı'nda bir yönetilen örnek kullanmayı](sql-database-howto-managed-instance.md).
