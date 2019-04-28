---
title: Azure SQL veritabanı kullanan çok kiracılı bir uygulamada yeni kiracılar sağlama | Microsoft Docs
description: Sağlama ve kataloğa kaydetme Azure SQL veritabanı çok kiracılı SaaS uygulamasında yeni kiracılar hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: 803d05e1aaf4d9c26a6132bde30f101ce3905924
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61388368"
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Yeni kiracılar sağlama ve bunları kataloğa kaydetme hakkında bilgi edinin

Bu öğreticide, sağlama ve kataloğa kaydetme SaaS düzenlerinin öğrenin. Ayrıca bunlar Wingtip bilet SaaS Kiracı başına veritabanı uygulamada nasıl uygulandığı öğrenin. Yeni Kiracı veritabanları başlatmak ve bunları uygulamanın Kiracı kataloğa kaydetme oluşturma. Katalog ile bu çok sayıda SaaS uygulama kiracıların verileri arasında eşleme tutan bir veritabanıdır. Katalog, uygulama ve yönetim isteklerini doğru veritabanına yönlendiren önemli bir rol oynar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> 
> * Tek bir yeni Kiracı sağlama.
> * Ek Kiracı grubu sağlayın.


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip bilet SaaS Kiracı başına veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Wingtip bilet SaaS Kiracı başına veritabanı uygulamayı keşfetme](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS katalog düzenine giriş

Bir veritabanı tarafından desteklenen çok kiracılı SaaS uygulamasında ait bilgilerin nerede depolandığının bilinmesi önemlidir. SaaS katalog düzeninde, her bir kiracı ile verilerinin depolandığı veritabanı arasındaki eşlemeyi tutmak için bir katalog veritabanı kullanılır. Kiracı verilerini birden fazla veritabanında dağıtıldığında bu deseni uygular.

Her kiracının kendi veritabanı konumuyla eşlenmiş kataloğunda bir anahtar tarafından tanımlanır. Wingtip bilet uygulamasında Kiracı adının bir karmada anahtarı oluşturulur. Bu düzen, uygulama URL'si dahil Kiracı adından anahtarı oluşturmak okumasına izin verir. Diğer Kiracı anahtar düzenleri kullanılabilir.  

Katalog adı veya uygulama üzerinde minimum etkiyle değiştirilmesi veritabanının konumunu sağlar. Çok kiracılı veritabanı modelinde, bu özellik, bir kiracı veritabanları arasında taşıma de kapsar. Katalog, bir kiracının veya veritabanı bakım veya diğer eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir. Bu özellik, araştırılan [geri yükleme tek Kiracı Öğreticisi](saas-dbpertenant-restore-single-tenant.md).

Şema sürümü, hizmet planı veya SLA'lar kiracıları için sunulan gibi ek Kiracı veya veritabanı meta veri, katalog da depolayabilirsiniz. Katalog, uygulama yönetimi, müşteri desteği ve DevOps sağlayan diğer bilgiler depolayabilirsiniz. 

SaaS uygulama dışında veritabanı araçları Kataloğu etkinleştirebilirsiniz. Wingtip bilet SaaS Kiracı başına veritabanı örnek olarak araştırılan kiracılar arası sorgu, etkinleştirmek için katalog kullanılır [öğretici Ad hoc raporlama](saas-tenancy-cross-tenant-reporting.md). Veritabanları arası iş yönetimi içinde keşfedilmemiş [Şema Yönetimi](saas-tenancy-schema-management.md) ve [Kiracı analizleri](saas-tenancy-tenant-analytics.md) öğreticiler. 

Wingtip bilet SaaS örneklerinde Shard Management özelliklerini kullanarak Kataloğu uygulanan [elastik veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). EDCL, Java ve .NET Framework içinde kullanılabilir. EDCL, oluşturmak, yönetmek ve bir veritabanı tarafından desteklenen parça eşlemesi kullanmak bir uygulama sağlar. 

Bir parça eşlemesi parçalar (veritabanları) ve anahtarlar (kiracılar) ile parçalar arasındaki eşlemeyi listesini içerir. EDCL işlevleri, Kiracı parça eşlemesinde girişleri oluşturmak için sağlama sırasında kullanılır. Bunlar çalışma zamanında uygulama tarafından doğru veritabanına bağlanmak için kullanılır. EDCL, katalog veritabanına trafiğini en aza indirmek ve uygulamayı hızlandırmak için bağlantı bilgilerini önbelleğe alır. 

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanından erişilebilir, ancak *verileri düzenlemeyin*. Eşleme verilerini yalnızca elastik veritabanı istemci kitaplığı API'sini kullanarak düzenleyin. Eşleme verilerinin doğrudan düzenleme Kataloğu bozabilir ve desteklenmez.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>SaaS sağlama düzeni giriş

Tek kiracılı veritabanı modeli kullanan bir SaaS uygulamasında yeni bir kiracı eklediğinizde, yeni bir kiracı veritabanı hazırlamanız gerekir. Veritabanı Hizmet katmanı ve uygun bir konum içinde oluşturulması gerekir. Ayrıca uygun şema ve başvuru verileri ile başlatılmalıdır. Ve uygun Kiracı anahtarı altındaki kataloğunda kaydedilmelidir. 

Veritabanı sağlama için farklı bir yaklaşım kullanılır. SQL betiklerini yürütme, bir bacpac dağıtma veya şablon veritabanını kopyalayın. 

Veritabanı sağlama şema yönetimi stratejinizin bir parçası olması gerekir. Yeni veritabanları ile en son şema sağlandığından emin olmanız gerekir. Bu gereksinim, araştırılan [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md). 

Wingtip bilet Kiracı başına veritabanı uygulama adlı bir şablon veritabanı kopyalayarak yeni kiracılar sağlar _basetenantdb_, katalog sunucusuna dağıtılabilir. Sağlama uygulamaya bir kaydolma deneyiminden bir parçası olarak tümleştirilebilir. Bu aynı zamanda Çevrimdışı komut dosyalarını kullanarak desteklenir. Bu öğreticide PowerShell kullanarak sağlama keşfediyor. 

Komut dosyaları kopyalama sağlama _basetenantdb_ veritabanını bir elastik havuzda yeni bir kiracı veritabanı oluşturmak için. Eşlenen Kiracı Server Kiracı veritabanı oluşturulduktan _newtenant_ DNS diğer adı. Bu diğer adı sunucunun yeni kiracılar sağlama için kullanılan bir başvuru tutar ve olağanüstü durum kurtarma öğreticilerde kurtarma Kiracı sunucusuna işaret edecek şekilde güncelleştirildi ([georestore kullanarak DR'yi](saas-dbpertenant-dr-geo-restore.md), [kalıcılığınkullanarakDR](saas-dbpertenant-dr-geo-replication.md)). Betikler, ardından veritabanı kiracıya özgü bilgilerle başlatır ve kataloğu parça eşlemesinde kaydedin. Kiracı, verilen Kiracı adına dayandırılan adlara veritabanlarıdır. Bu adlandırma düzeni desen kritik bir parçası değil. Herhangi bir adlandırma kuralı kullanılabilmesi için Katalog veritabanı adı için Kiracı anahtarınızı eşler. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip bilet SaaS Kiracı başına veritabanı uygulama betiklerini alma

Wingtip bilet SaaS betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub deposu. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet SaaS betikleri engelini kaldırmak için.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Wingtip bilet uygulaması yeni Kiracı sağlama nasıl uyguladığını anlamak için bir kesme noktası ekleyin ve bir kiracı sağlama sırasında iş akışını izleyin.

1. PowerShell ISE'de Aç... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo-ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayın:

   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*).
   * **$VenueType** önceden tanımlanmış mekan türlerinden birini =: _blues, klasik müzik, dans, jazz, judo, motor yarışı, çok amaçlı, opera, Rock müzik, futbol_.
   * **$DemoScenario** = **1**, *tek Kiracı sağlamak*.

2. Bir kesme noktası eklemek için imlecinizi her yerde yazan satıra yerleştirin *New-Tenant '*. F9 tuşuna basın.

   ![Kesme noktası](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Betiği çalıştırmak için F5 tuşuna basın.

4. Betik yürütme kesme noktasında durduktan sonra kodda ilerleyebilmeniz için F11 tuşuna basın.

   ![Hata ayıklama](media/saas-dbpertenant-provision-and-catalog/debug.png)



Betiğin yürütülmesini kullanarak izleme **hata ayıklama** menü seçenekleri. Çağrılan işlevler veya üzerinden adımlamak için F10 ve F11 tuşuna basın. PowerShell betikleri hata ayıklama hakkında daha fazla bilgi için bkz. [çalışmaya ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Bu iş akışı açıkça izleyin gerekmez. Bu betik hata ayıklama açıklanmaktadır.

* **CatalogAndDatabaseManagement.psm1 modülünü içeri aktarın.** Üzerinden bir katalog ve Kiracı düzeyinde Özet sağlayan [Shard Management](sql-database-elastic-scale-shard-map-management.md) işlevleri. Bu modül, katalog düzeninin büyük kapsülleyen ve incelenmesi yararlı olan.
* **SubscriptionManagement.psm1 modülünü içeri aktarın.** Bu, Azure'da oturum açma ve birlikte çalışmak istediğiniz Azure aboneliğini seçme işlevlerini içerir.
* **Yapılandırma ayrıntılarını alın.** Get-yapılandırmada F11 kullanarak adım ve uygulama yapılandırmasının nasıl belirtildiğine bakın. Kaynak adları ve uygulamaya özel diğer değerler burada tanımlanır. Betikleri ile ilgili bilgi sahibi olduğunuz kadar bu değerler değişmez.
* **Katalog nesnesini alın.** Oluşturur ve daha üst düzey betikte kullanılır bir katalog nesnesi döndüren Get-Catalog Adımlama. Bu işlev öğesinden içeri aktarılan Shard Management işlevleri kullanır **AzureShardManagement.psm1**. Katalog nesnesini aşağıdaki öğelerden oluşur:

   * $catalogServerFullyQualifiedName, standart gövdeye ek olarak, kullanıcı adınız kullanılarak oluşturulur: _catalog -\<kullanıcı\>. database.windows .net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki _tenantcatalog_ parça eşlemesinden başlatılır. Katalog nesnesini oluşan ve döndürdü. Üst düzey betikte kullanılır.
* **Yeni Kiracı anahtarını hesaplayın.** Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
* **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin.** Katalog, anahtarı kullanılabilir olduğundan emin olmak için denetlenir.
* **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** Nasıl veritabanı kullanarak sağlanan içine adımlamak için F11 kullanın. bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md).

    Veritabanı adı, hangi parçanın hangi kiracıya ait olduğunu netleştirmek üzere kiracı adından oluşturulur. Diğer veritabanı adlandırma kuralları da kullanabilirsiniz. Resource Manager şablonu bir şablon veritabanı kopyalayarak Kiracı veritabanı oluşturur (_baseTenantDB_) katalog sunucusunda. Alternatif olarak, bir veritabanı oluşturun ve bir bacpac içeri aktararak başlatılamıyor. Veya iyi bilinen bir konumdan bir başlatma betiği yürütebilirsiniz.

    Resource Manager şablonu ...\Learning Modules\Common\ dizindir: *tenantdatabasecopytemplate.json*

* **Daha fazla Kiracı veritabanı başlatıldı.** Mekan (Kiracı) adı ve mekan türü eklenir. Ayrıca, diğer başlatma burada yapabilirsiniz.

* **Kiracı veritabanı, kataloğa kaydedilir.** İle kayıtlı *Add-TenantDatabaseToCatalog* Kiracı anahtarını kullanarak. Ayrıntılara adım için F11 kullanın:

    * Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
    * Anahtar değerini parçaya bağlayan eşleme oluşturulur.
    * Kiracı (mekan adı) hakkında ek meta veri Kataloğu kiracılar tablosuna eklenir. Kiracılar tablo Shard Management şemanın parçası değil ve tarafından EDCL yüklü değil. Bu tablo, uygulamaya özgü ek veri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterir.


Sağlama tamamlandıktan, özgün yürütme döndürür sonra *Demo-ProvisionAndCatalog* betiği. **Olayları** tarayıcıdaki yeni kiracının sayfası açılır.

   ![Olayları sayfası](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Bir kiracı grubu sağlama

Bu alıştırmada, 17 Kiracı grubu sağlanır. Diğer Wingtip bilet SaaS Kiracı başına veritabanı öğreticileri başlatmadan önce bu Kiracı grubu sağlama öneririz. Çalışmak için birkaç taneden fazla yalnızca veritabanı yok.

1. PowerShell ISE'de Aç... \\Öğrenme modülleri\\ProvisionAndCatalog\\*Demo-ProvisionAndCatalog.ps1*. Değişiklik *$DemoScenario* parametre 3:

   * **$DemoScenario** = **3**, *bir kiracı grubu sağlama*.
2. Betiği çalıştırmak için F5 tuşuna basın.

Betik, ek kiracı grubu dağıtır. Bunu kullanan bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) grubunu denetleyen ve temsilciler bağlı bir şablona her bir veritabanının sağlama. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonlar, gerekirse yeniden denemeleri paralel ve tutamacı veritabanlarında sağlayın. Betik etkilidir kadar başarısız olur ya da durdurur herhangi bir nedenle yeniden çalıştırın.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Kiracı başarıyla dağıtılan grubu doğrulayın

* İçinde [Azure portalında](https://portal.azure.com)sunucular listesine göz atın ve Aç *tenants1* sunucusu. Seçin **SQL veritabanları**ve veritabanı grubunu içeren 17 ek artık listesinde olduğundan emin olun.

   ![Veritabanı listesi](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama düzenleri:

**Veritabanlarını önceden hazırlama**: Önceden sağlama düzeni ek maliyet bir elastik havuzdaki veritabanları eklemeyen olgu yararlanan. Faturalandırma, elastik havuz için veritabanları ' dir. Boştaki veritabanlarının hiçbir kaynak kullanır. Bir havuz ve gerektiğinde ayırarak veritabanlarını önceden sağlamak tarafından kiracılar eklemek için zaman azaltabilirsiniz. Önceden sağlanan veritabanı sayısı, öngörülen sağlama hızı için uygun bir arabellek tutmak için gereken şekilde ayarlanabilir.

**Otomatik sağlama**: Otomatik sağlama düzeni sağlama hizmeti sunucuları, havuzları ve veritabanlarını gerektiğinde otomatik olarak sağlar. İsterseniz, elastik havuzlardaki veritabanlarını önceden hazırlama içerebilir. Veritabanları kullanımdan kaldırıldı ve silinirse, elastik havuzlardaki boşluklar sağlama hizmeti tarafından doldurulabilir. Böyle bir hizmet basit ya da birden fazla coğrafyada sağlama ve olağanüstü durum kurtarma için coğrafi çoğaltmayı ayarlama işleme gibi karmaşık olabilir. 

Otomatik sağlama düzeni ile bir istemci uygulaması veya betik sağlama hizmeti tarafından işlenecek bir sağlama isteği kuyruğa gönderir. Ardından, işlemin tamamlandığını belirlemek üzere hizmeti yoklar. Önceden sağlama kullanılırsa, istekler hızlı bir şekilde ele alınır. Arka planda bir yedek veritabanının hizmet sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> 
> * Tek bir yeni Kiracı sağlama.
> * Ek Kiracı grubu sağlayın.
> * Kiracılar sağlama ve bunları kataloğa kaydetme ayrıntılarını Adımlama.

Deneyin [performans izleme Öğreticisi](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip bilet SaaS Kiracı başına veritabanı uygulamayı yapı öğreticiler](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
* [Windows PowerShell ISE'de betiklerde hata ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
