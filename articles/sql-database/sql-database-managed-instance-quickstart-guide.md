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
ms.reviewer: carlr
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: a70e83737c6b56aee3279375ec653f12810b13b4
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55749823"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Yönetilen örnek Azure SQL veritabanı ile çalışmaya başlama

[Yönetilen örnek](sql-database-managed-instance-index.yml) dağıtım seçeneği neredeyse % 100 uyumluluk altyapısıyla en son SQL Server şirket içi (Enterprise Edition) veritabanı yerel sağlayan, bir veritabanı oluşturma [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) ortak güvenlik endişelerini ortadan uygulama ve [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) şirket içi SQL Server müşterileri için yeterli. Bu bölümde, hızlı bir şekilde yapılandırın ve yönetilen örnek oluşturma ve veritabanlarınızı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı genel bakış

Şu hızlı başlangıçlarda hızlı bir şekilde yönetilen örnek oluşturma, bir sanal makineyi yapılandırmak veya konuma VPN bağlantısı için istemci uygulaması işaret olanak sağlar ve yeni, kullanarak yönetilen örnek bir veritabanı geri bir `.bak` dosyası:

- [Azure portalını kullanarak yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md). Azure portalında gerekli parametreleri (kullanıcı adı/parola, çekirdek sayısı, en fazla depolama alanı), yapılandırma ve Azure ağ ortamında Ağ ayrıntıları ve altyapı gereksinimleri hakkında bilmeniz gereken gerek kalmadan otomatik olarak oluşturun. Yalnızca sahip olduğunuzdan emin olun bir [abonelik türü](sql-database-managed-instance-resource-limits.md#supported-subscription-types) yönetilen örnek oluşturma için verilir. Kullanmak istediğiniz kendi ağ veya ağ özelleştirmek bkz. isterseniz nasıl [ağ ortamını yapılandırmak](#configure-network-environment) yönetilen örnek için.
- Yönetilen örnek genel bir uç nokta ile kendi sanal ağ oluşturulur. İstemci uygulama erişimi için (farklı bir alt ağ) aynı sanal ağda VM oluşturma veya şu hızlı başlangıçlardan biriyle kullanarak istemci bilgisayardan Vnet'e noktadan siteye VPN bağlantısı oluşturma.
  - Oluşturma [Azure sanal makinesi yönetilen örneğinde VNet](sql-database-managed-instance-configure-vm.md) istemci uygulama bağlantısı için SQL Server Management Studio dahil olmak üzere.
  - Ayarlanan [noktadan siteye VPN bağlantısı yönetilen Örneğinize](sql-database-managed-instance-configure-p2s.md) üzerinde sahip olduğunuz SQL Server Management Studio ve diğer istemci bağlantı uygulamaları istemci bilgisayardan. Diğer yönetilen Örneğinize ve kendi sanal ağa bağlantı için iki seçenek budur.

  > [!NOTE]
  > Express route veya siteden siteye bağlantı yerel ağınızdan de kullanabilirsiniz, ancak bu yaklaşımların şu hızlı başlangıçlardan biriyle kapsamı dışında olan.

Yönetilen örnek oluşturma ve erişim'ı yapılandırdıktan sonra şirket içi SQL Server veya Azure Vm'leri üzerinde yerleştirilen veritabanlarınızı geçirme başlayabilirsiniz. Geçirmek istediğiniz kaynak veritabanında desteklenmeyen bazı özellikler varsa, geçiş başarısız olur. Uyumluluğu denetlemek ve hatalarını önlemek için yükleyebilirsiniz [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) , SQL Server veritabanlarınızı analiz eder ve bulduğu aşağıdakilerden sorunu varlığınıgibibiryönetilenörneğegeçişengelleyin[FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) veya birden çok günlük dosyası. Bu sorunları çözmek, veritabanlarınızı yönetilen örneğe geçiş hazır olursunuz. [Veritabanı deneme Yardımcısı](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) iş yükünüze dayalı bir yönetilen örnek üzerinde tanımlayabilirsiniz böylece oraya giden bir yönetilen örneğe geçiş herhangi bir performans sorunu olarak SQL Server ve yeniden yürütme kaydedebilen başka yararlı bir araçtır.

Yönetilen örnek için veritabanınızı geçirebileceğiniz emin olduktan sonra yönetilen bir örneğinden içine bir veritabanını geri yüklemek için yerel SQL Server geri yükleme özelliklerini kullanabilir bir `.bak` dosya. Hızlı Başlangıç için bkz: [yedekten bir yönetilen örneğine geri](sql-database-managed-instance-get-started-restore.md). Bu hızlı başlangıçta, geri bir `.bak` Azure blob depolama kullanarak depolanan dosya `RESTORE` Transact-SQL komutu. 

> [!TIP]
> Kullanılacak `BACKUP` Transact-SQL komutu, Azure blob depolama alanında veritabanınızın bir yedeği oluşturmak için bkz. [URL'ye SQL Server Yedekleme](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url).

Bu hızlı başlangıçlar, hızlı bir şekilde oluşturmanızı, yapılandırmanızı ve yönetilen bir örneği için veritabanı yedeklemesini geri yükleme sağlar. Bazı senaryolarda, özelleştirme veya yönetilen örnek dağıtımı ve gereken ağ ortamı otomatik hale getirmek gerekir. Bu senaryolar aşağıdaki açıklanacaktır.

## <a name="customizing-network-environment"></a>Ağ ortamı özelleştirme

VNet/alt ağ, Azure portalını kullanarak örneği oluşturulduğunda otomatik olarak yapılandırılabilir olsa da, VNet ve alt ağ parametrelerini yapılandırabilirsiniz. böylece yönetilen örnek oluşturma başlatmadan önce VNet/alt ağ oluşturmak isteyebilirsiniz. Kullanılacak oluşturmak ve ağ ortamını yapılandırmak için en kolay yolu olan bir [Azure kaynak dağıtımı](sql-database-managed-instance-create-vnet-subnet.md) şablonu, ağ ve alt ağ için yönetilen örnek oluşturma ve yapılandırma. Azure Resource Manager tuşuna basmanız yeterlidir Dağıt düğmesi ve parametrelerle formu doldurun. 

Alternatif olarak, bu kullanabilirsiniz [PowerShell Betiği](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) ağ oluşturulmasını otomatik hale getirmek için.

Zaten bir sanal ağ ve alt ağ, yönetilen örnek dağıtmak istediğiniz varsa, ağ ve alt sağladığını emin olmanız gerekir [gereksinimlerinde](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Bunu kullanın [alt ağınızın düzgün yapılandırıldığını doğrulamak için PowerShell Betiği](sql-database-managed-instance-configure-vnet-subnet.md). Bu betik, ağ ve rapor ve sorunları bildirir, değişiklikler ve sonra VNet/alt ağda gerekli değişiklikleri yapmak için teklifleri doğrular. VNet/alt ağınızın el ile yapılandırmak istemiyorsanız, bu betiği çalıştırın. Ayrıca tüm önemli yeniden yapılandırma ağ altyapınızın sonra çalıştırabilirsiniz. Oluşturup kendi ağ yapılandırmak istiyorsanız, okuma [bağlantı mimarisi](sql-database-managed-instance-connectivity-architecture.md) ve bu[oluşturup bir yönetilen örnek ortamı yapılandırmaya yönelik kılavuz](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-a-managed-instance"></a>Yönetilen bir örneğinin oluşturulmasını otomatik hale getirme

 Önceki adımda açıklandığı gibi ağ ortamını oluşturmadıysanız, Azure portalı, sizin için neler yapabileceğini – tek dezavantajı, bunu daha sonra değiştiremezsiniz bazı varsayılan parametreler yapılandırır gerçeğidir. Alternatif olarak kullanabilirsiniz:

- [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/)
- [PowerShell Resource Manager şablonu ile](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md)
- [Azure CLI](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).

## <a name="migrating-to-a-managed-instance-with-minimal-downtime"></a>En düşük kapalı kalma süresi ile bir yönetilen örneğe geçirme

Bu hızlı başlangıç makalelerinde hızlı bir şekilde bir yönetilen örneği ve veritabanlarınızı yerel taşımak etkinleştirme `RESTORE` yeteneği. Bununla birlikte, yerel `RESTORE`, geri (ve Azure blob depolama alanına kopyalandığından değilse zaten var. depolanan) veritabanları için beklemeniz gerekir. Bu, bazı kapalı kalma süresi, uygulamanızın daha büyük veritabanları için özellikle neden olur. Üretim veritabanınız taşımak için kullanın [veri geçiş hizmeti (DMS)](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) veritabanınızı en düşük kapalı kalma süresi ile geçirilecek. DMS bunu, kaynak veritabanınızı geri yüklenen yönetilen örnek veritabanına yapılan değişiklikleri artımlı olarak ileterek gerçekleştirir. Bu şekilde uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresiyle hızlı bir şekilde geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Bulma bir [üst düzey burada yönetilen örneğinde desteklenen özellikler listesini](sql-database-features.md) ve [ayrıntıları ve bilinen sorunlar](sql-database-managed-instance-transact-sql-information.md).
- Hakkında bilgi edinin [yönetilen örneği Teknik Özellikleri](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
- Nasıl daha gelişmiş bulma-için kullanıcının [Azure SQL veritabanı'nda bir yönetilen örnek kullanmayı](sql-database-howto-managed-instance.md). 
