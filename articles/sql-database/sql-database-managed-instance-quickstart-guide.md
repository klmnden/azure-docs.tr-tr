---
title: Hızlı Başlangıç - Azure SQL veritabanı yönetilen örneği | Microsoft Docs
description: Azure SQL veritabanı - yönetilen örnek hızlı başlama hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 77deed43c106a451d3de768989233c749e1280e1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55468181"
---
# <a name="getting-started-with-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği ile çalışmaya başlama

[Azure SQL veritabanı yönetilen örneği](sql-database-managed-instance-index.yml) olan SQL Server'ın tam olarak yönetilen PaaS sürümünü Azure bulutunda barındırılan ve kendi sanal ağ özel IP adresiyle yerleştirilir. Bu bölümde, hızlı bir şekilde yapılandırın ve yönetilen örneği oluşturma ve veritabanlarınızı öğreneceksiniz.

## <a name="quickstart-overview"></a>Hızlı genel bakış

Bu bölümde, yönetilen örnekler ile hızlıca çalışmaya başlamanıza yardımcı olabilecek mevcut makaleleri genel bakış görürsünüz. İlk yönetilen Örneğinize oluşturmak için en kolay yolu kullanmaktır [Azure portalında](sql-database-managed-instance-get-started.md) ağ ortamınızda, gerekli parametreleri (kullanıcı adı/parola, çekirdek sayısı, en fazla depolama alanı), yapılandırma ve otomatik olarak Azure'ı oluşturma olmadan ağ ayrıntıları ve altyapı gereksinimleri hakkında bilmeniz gerekir. Bilgisayarınızda yüklü olduğundan emin olmanız yeterlidir bir [abonelik türü](sql-database-managed-instance-resource-limits.md#supported-subscription-types) örneği oluşturmak için izin.

Kullanmak istediğiniz kendi ağ veya ağ özelleştirmek bkz. isterseniz nasıl [ağ ortamını yapılandırmak](#configure-network-environment) yönetilen örneği için.

Yönetilen Örneğinize oluşturduğunuzda, aşağıdaki yaklaşımlardan birini kullanarak örneğe bağlanmak gerekir:

* Oluşturma [Azure sanal makine](sql-database-managed-instance-configure-vm.md) SQL Server Management Studio ve yönetilen Örneğinize nereye yerleştirilir aynı VNet içindeki bir alt ağdaki yönetilen Örneğinize erişmek için kullanılan diğer uygulamalar ile yüklenmiştir. VM, yönetilen örneği ile aynı alt ağda olamaz.
* Ayarlanan [noktadan siteye bağlantı](sql-database-managed-instance-configure-p2s.md) bilgisayarınızda ağınızdaki diğer SQL Server yönetilen örneği nereye yerleştirilir Vnet'e bilgisayarınızı katılın ve yönetilen örneği'ni kullanın olanak tanıyacaktır.

Alternatif olarak, yerel ağınızdan express route veya siteden siteye bağlantı kullanabilirsiniz, ancak bu yaklaşım şu hızlı başlangıçlardan biriyle kapsamı dışında olan.

Bir yönetilen örnek oluşturma ve erişim yapılandırdığınızda, SQL Server şirket içi veya Azure Vm'leri üzerinde yerleştirilen veritabanlarınızı geçirme başlatabilirsiniz. Geçirmek istediğiniz kaynak veritabanında desteklenmeyen bazı özellikler varsa, geçiş başarısız olduğunu unutmayın. Uyumluluğu denetlemek ve hatalarını önlemek için yükleyebilirsiniz [Data Migration Yardımcısı (DMA)](https://www.microsoft.com/download/details.aspx?id=53595) , SQL Server veritabanlarınızı çözümleme ve yönetilen örneğe geçiş varlığını gibi engelleyebilecek herhangi bir sorun bulunamadı FILESTREAM veya birden çok günlük dosyası. Bu sorunları çözmek, veritabanlarınızı yönetilen örneği'ne gitmek hazırsınız. [Veritabanı deneme Yardımcısı](https://blogs.msdn.microsoft.com/datamigration/2018/08/06/release-database-experimentation-assistant-dea-v2-6/) yükünüz üzerindeki SQL Server ve yönetilen tanımlayabilirsiniz böylece örneği üzerinde oraya giden yönetilen örneğe geçiş işlemi gerçekleştirirseniz, herhangi bir performans sorunu olacak şekilde yeniden yürütme kaydedebilen başka yararlı bir araçtır.

Veritabanınızı yönetilen örneğe geçirme emin emin olduktan sonra kullanabileceğiniz [yerel geri](sql-database-managed-instance-get-started-restore.md) Transact-SQL komutunu kullanarak veritabanınızın bir yedeğine oluşturmanıza olanak sağlayan işlevselliği, bir Azure blob depolama alanına yükleyin ve Veritabanı Transact-SQL komutunu kullanarak blob depolama alanından geri yükleyin.

Bu hızlı başlangıçlar, hızlı bir şekilde yapılandırmak, oluşturun ve yönetilen örneklerinizin veritabanları yerleştirin sağlar. Bazı senaryolarda, özelleştirme veya yönetilen örneği ve gerekli ağ ortamı dağıtımını otomatikleştirmek gerekir. Bu senaryolar aşağıdaki açıklanacaktır.

## <a name="customizing-network-environment"></a>Ağ ortamı özelleştirme

VNet/alt ağ kullanarak örneği oluşturulduğunda otomatik olarak yapılandırılabilir ancak [Azure portalında](sql-database-managed-instance-get-started.md), önce VNet parametreleriyle yapılandırabileceğinizden yönetilen örnek oluşturma başlangıç oluşturmak iyi olabilir ve alt ağ. Oluşturun ve ağ ortamını yapılandırmak için en kolay yolu kullanmaktır [Azure kaynak dağıtımını](sql-database-managed-instance-create-vnet-subnet.md) oluşturur ve, ağ ve alt ağ örneği nereye yerleştirilir yapılandırma şablonu. Azure Resource Manager tuşuna basmanız yeterlidir Dağıt düğmesi ve parametrelerle formu doldurun. Alternatif olarak, kullandığınız [PowerShell Betiği](https://www.powershellmagazine.com/2018/07/23/configuring-azure-environment-to-set-up-azure-sql-database-managed-instance-preview/) ağ oluşturulmasını otomatik hale getirmek için.

Zaten bir sanal ağ ve alt ağ, yönetilen örneği dağıtmak istediğiniz varsa, VNet ve alt ağ'ın sağladığını emin olmanız gerekir [gereksinimlerinde](sql-database-managed-instance-connectivity-architecture.md#network-requirements). Bu kullanması gereken [alt ağınızın düzgün yapılandırıldığını doğrulamak için PowerShell Betiği](sql-database-managed-instance-configure-vnet-subnet.md). Bu betik yalnızca ağınızı doğrulamak ve – sorunları bildirmek, hangi değiştirilmelidir ve ayrıca VNet/alt ağda gerekli değişiklikleri yapmanızı sunuyoruz size bildirir. VNet/alt ağınızın el ile yapılandırmak istemiyorsanız, bu betiği çalıştırın ve ayrıca tüm ana ağ altyapınızı yeniden yapılandırma çalıştırmalısınız. Oluşturma ve okuma kendi ağ yapılandırma istiyorsanız [yönetilen örnek belgelerine](sql-database-managed-instance-connectivity-architecture.md) ve [bu kılavuzda](https://medium.com/azure-sqldb-managed-instance/the-ultimate-guide-for-creating-and-configuring-azure-sql-managed-instance-environment-91ff58c0be01).

## <a name="automating-creation-of-managed-instance"></a>Yönetilen örnek oluşturulmasını otomatik hale getirme

 Önceki adımda açıklandığı gibi ağ ortamını oluşturmadıysanız, Azure portalı, sizin için neler yapabileceğini – tek dezavantajı, bunu daha sonra değiştiremezsiniz bazı varsayılan parametreler yapılandıracaksınız gerçeğidir. Alternatif olarak kullanabileceğiniz [PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/06/27/quick-start-script-create-azure-sql-managed-instance-using-powershell/), [PowerShell Resource Manager şablonu ile](scripts/sql-managed-instance-create-powershell-azure-resource-manager-template.md), veya [Azure CLI](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2018/11/14/create-azure-sql-managed-instance-using-azure-cli/).

## <a name="migrating-to-managed-instance-with-minimal-downtime"></a>En düşük kapalı kalma süresi ile yönetilen örneğe geçirme

Bu hızlı başlangıç makalelerinde hızlıca yönetilen örneği ve veritabanlarınızı taşımak etkinleştirin. Veritabanı özellikle büyük olduğunda, ancak yerel geri veritabanlarının geri yüklenmesini beklemeniz gerekir, uygulamanızın kapalı neden olur. Geçişin en düşük kapalı kalma süresi garanti eder daha iyi şekilde geçirmek için üretim veritabanınız taşıyorsanız, büyük olasılıkla gerekir. [Data Migration hizmeti](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance?toc=/azure/sql-database/toc.json) veritabanınızı en düşük kapalı kalma süresi ile bir veritabanı için kaynak veritabanında yapılan değişiklikler artımlı olarak ileterek geçirebileceğiniz bir geçiş hizmeti, olduğunuz yönetilen örneğine geri yükleniyor. Bu şekilde uygulamanızı kaynaktan hedef veritabanına en az kapalı kalma süresiyle hızlı bir şekilde geçiş yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Bulma bir [üst düzey burada yönetilen örneğinde desteklenen özellikler listesini](sql-database-features.md) ve [ayrıntıları ve bilinen sorunlar](sql-database-managed-instance-transact-sql-information.md). 
* Hakkında bilgi edinin [yönetilen örneği Teknik Özellikleri](sql-database-managed-instance-resource-limits.md#instance-level-resource-limits). 
* Öğreticilerde daha gelişmiş bulma [nasıl bölümüne](sql-database-howto-managed-instance.md). 
