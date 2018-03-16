---
title: "Azure SQL veritabanı kullanan bir çok kiracılı uygulamalarda yeni kiracılar sağlama | Microsoft Docs"
description: "Sağlamak ve bir Azure SQL veritabanı çok kiracılı SaaS uygulamasında yeni kiracılar katalog hakkında bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: 21f0bca3a16164ead4e0990842a968fd9b95c33f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Yeni kiracılar sağlamak ve kataloğa kaydetme hakkında bilgi edinin

Bu öğreticide, sağlama ve Katalog SaaS desenleri ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanında nasıl uygulandığı hakkında bilgi edinin. Oluşturun ve yeni Kiracı veritabanlarını başlatmak ve bunları uygulamanın Kiracı kataloğunda kaydedin. Katalog SaaS uygulamanın birçok kiracılar ve verileri arasında eşleme tutan bir veritabanıdır. Katalog uygulama ve doğru veritabanı yönetim isteklerine yönlendiren önemli bir rol oynar.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS veritabanı Kiracı uygulama başına dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfedin.](saas-dbpertenant-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS Katalog düzenine giriş

Bir veritabanı yedeği çok kiracılı SaaS uygulamasına, her bir kiracı için bilgi depolandığı bilmeniz önemlidir. SaaS katalog desende bir katalog veritabanı her bir kiracı ile verilerinin depolandığı veritabanı arasında eşleme tutmak için kullanılır. Kiracı verilerini birden çok veritabanı arasında dağıtıldığında bu deseni uygular.

Her bir kiracı kataloğunda kendi veritabanı konumu için eşlenmiş bir anahtar tarafından tanımlanır. Wingtip biletleri uygulamada karmasını kiracının ad anahtarı oluşturulur. Bu uygulama URL'SİNDE bulunan Kiracı adı anahtarından oluşturmak yazmasına izin verir. Diğer Kiracı anahtar düzenleri kullanılabilir.  

Katalog adı veya uygulama üzerinde en az etkiyle değiştirilecek veritabanının konumunu sağlar.  Çok Kiracı veritabanı modelinde, bu da 'Kiracı veritabanları arasında taşıma' düzenler.  Katalog, bir kiracı veya veritabanı bakım veya başka eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir. İçinde bu incelediniz [tek bir kiracı öğretici geri](saas-dbpertenant-restore-single-tenant.md).

Katalog ayrıca ek Kiracı veya veritabanı gibi meta veriler, şema sürümü, hizmet planı veya kiracılar, yanı sıra uygulama yönetimi, müşteri desteği veya devops sağlayan diğer bilgiler sunulan SLA depolayabilirsiniz.  

SaaS uygulamasına Katalog veritabanı araçları etkinleştirebilirsiniz.  Kiracı örnek başına Wingtip biletleri SaaS veritabanında Kataloğu, keşfedilen arası Kiracı sorgu etkinleştirmek için kullanılan [öğretici ad hoc raporlama](saas-tenancy-cross-tenant-reporting.md). Veritabanları arası iş yönetimi incelediniz içinde [Şema Yönetimi](saas-tenancy-schema-management.md) ve [Kiracı analytics](saas-tenancy-tenant-analytics.md) öğreticileri. 

Wingtip biletleri SaaS örneklerinde parça yönetim özelliklerini kullanarak katalog uygulanır [esnek veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). Java ve .net EDCL kullanılabilir Framework. EDCL oluşturmak, yönetmek ve bir veritabanı yedeği parça eşlemesi kullanmak için bir uygulama sağlar. Bir parça eşleme parça (veritabanları) ve anahtarları (kiracılar) ile parça arasında eşleme listesini içerir.  EDCL işlevleri, doğru veritabanına bağlanmak için uygulamalar tarafından Kiracı parça eşlemesindeki ve çalışma zamanında girişleri oluşturmak için sağlama sırasında kullanılır. EDCL katalog veritabanına trafiğini en aza indirmek ve uygulamayı oluşturan hızlandırmak için bağlantı bilgilerini önbelleğe alır.  

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanından erişilebilir, ancak *verileri düzenlemeyin*! Eşleme verilerini yalnızca Elastik Veritabanı İstemci Kitaplığı API’sini kullanarak düzenleyin. Eşleme verilerinin doğrudan değiştirilmesi, kataloğu bozabilir ve desteklenmez.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>SaaS sağlama düzeni giriş

Model ekleme tek Kiracı tarafından kullanılan bir SaaS uygulaması yeni bir kiracı veritabanı zaman yeni bir kiracı veritabanı sağlanmalıdır.  Veritabanı Hizmet katmanı ile uygun şema ve başvuru verileri başlatılmadı ve uygun Kiracı anahtarı altında kataloğunda kayıtlı ve uygun konuma oluşturulmalıdır.  

SQL betikleri yürütülürken, bir bacpac dağıtma ya da bir şablon veritabanı kopyalayarak içeren veritabanı sağlama için farklı yaklaşımlara kullanılabilir.  

Veritabanı sağlama yeni veritabanları ile en son şema sağlandığından emin olun, şema yönetim stratejisinin bir parçası olması gerekir. Bu gereksinim, keşfedilen [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md).  

Kiracı uygulama başına Wingtip biletleri veritabanı adlı bir şablon veritabanı kopyalayarak yeni kiracılar hazırlar _basetenantdb_, katalog sunucusunda dağıtılmış.  Sağlama bir kayıt deneyimi bir parçası olarak uygulamaya tümleşik ve/veya desteklenen komut dosyalarını kullanarak çevrimdışı. Bu öğretici, PowerShell kullanarak sağlama araştırır. Komut dosyaları kopyalama sağlama _basetenantdb_ bir esnek havuzda yeni bir kiracı veritabanı oluşturmak sonra Kiracı özgü bilgileri veritabanıyla başlatmak ve Katalog parça eşlemesinde kaydetmek için veritabanı.  Kiracı veritabanları verilen Kiracı adına göre adlardır, ancak katalog Kiracı anahtarı herhangi bir adlandırma kuralı kullanılan veritabanı adı eşlemeleri gibi bu adlandırma şeması düzeni – önemli bir parçası değildir. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Başına Wingtip biletleri SaaS veritabanı Kiracı uygulama komut dosyaları alma

Wingtip biletleri SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Yeni Kiracı sağlama Wingtip biletleri uygulama nasıl uyguladığını anlamak için bir kesme noktası ve iş akışı aracılığıyla adım bir kiracı sağlarken ekleyin:

1. İçinde _PowerShell ISE_açın... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayabilirsiniz:
   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*).
   * **$VenueType** önceden tanımlanmış salonundan türlerinden birini =: _mavi, classicalmusic, dance, jazz, judo, yarış, çok amaçlı motor, opera, rockmusic, futbol_.
   * **$DemoScenario** = **1**, *tek bir kiracı sağlama*.

1. İmlecinizi herhangi bir yere bildiren satırda koyarak kesme noktası ekleme: *yeni Kiracı '*ve basın **F9**.

   ![kesme noktası](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

1. Komut dosyası tuşuna çalıştırmak için **F5**.

1. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.

   ![Hata ayıklama](media/saas-dbpertenant-provision-and-catalog/debug.png)



Betiğin yürütmeyi kullanarak **hata ayıklama** menü seçeneklerini - **F10** ve **F11** adıma üzerinden veya çağrılan işlev. PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Değil açıkça izlemeniz gereken adımlar, ancak, aracılığıyla komut dosyası hata ayıklama sırasında adım iş akışı bir açıklaması verilmiştir:

1. [Shard Management](sql-database-elastic-scale-shard-map-management.md) işlevlerinde katalog ve kiracı düzeyinde özet sağlayan **CatalogAndDatabaseManagement.psm1 modülünü içeri aktarın**. Bu modül katalog düzeni çoğunu yalıtır ve incelenmesi yararlı olur.
1. Azure’da oturum açma ve birlikte çalıştığınız Azure aboneliğini seçme işlevlerini içeren **SubscriptionManagement.psm1 modülünü içeri aktarın**.
1. **Yapılandırma ayrıntılarını alın**. Get-yapılandırmasını (F11) adımla ve uygulama yapılandırma nasıl belirtilen bakın. Kaynak adları ve diğer uygulamaya özgü değerleri burada tanımlanan ancak kodlarla bilginiz kadar bu değerleri değiştirmeyin.
1. **Katalog nesnesini alın**. Oluşturur ve üst düzey komut dosyasında kullanılan bir katalog nesnesi döndüren Get-katalog adımla.  Bu işlev üzerinden içe aktarılan parça yönetim işlevlerini kullanan **AzureShardManagement.psm1**. Katalog nesnesi aşağıdaki öğelerden oluşur:
   * $catalogServerFullyQualifiedName standart stem artı kullanıcı adınızı kullanarak yapılandırılmıştır: _katalog -\<kullanıcı\>. database.windows .net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki _tenantcatalog_ parça eşlemesinden başlatılır.
   Bir katalog nesnesi oluşturulup döndürülür ve daha üst düzey betikte kullanılır.
1. **Yeni kiracı anahtarını hesaplayın**. Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
1. **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**. Anahtarın olup olmadığından emin olmak için katalog denetlenir.
1. **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** Kullanım **F11** adımını ve veritabanının nasıl olduğunu görmek için kullanılarak hazırlanmış bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md).

    Veritabanı adı, Kiracı için hangi parça ait diğer veritabanı adlandırma kuralları kullanılabilir olsa da temizleyin yapmak için Kiracı adından oluşturulur.
    Bir şablon veritabanı kopyalayarak Kiracı veritabanı bir Resource Manager şablonu oluşturur (_baseTenantDB_) katalog sunucusunda. Alternatif olarak, bir veritabanı oluşturmayı ve bir bacpac içeri aktararak başlatmak veya iyi bilinen bir konumdan bir başlatma betiği yürütün.

    Resource Manager şablonu ...\Learning Modules\Common\ klasöründedir: *tenantdatabasecopytemplate.json*

1. Kiracı veritabanı oluşturulduktan sonra daha sonra başka **salonundan (Kiracı) adı ve salonundan türü ile başlatılmış**. Burada, diğer başlatma işlemleri de yapılabilir.

1. **Kataloğa kayıtlı Kiracı veritabanı** ile *Ekle TenantDatabaseToCatalog* Kiracı anahtarı kullanarak. Ayrıntıları görmek için **F11** tuşunu kullanın:

    * Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
    * Anahtar değerini parçaya bağlayan eşleme oluşturulur.
    * Kiracı (salonundan kişinin adı) hakkında ek meta veriler eklenen *kiracılar* kataloğunda tablo.  Kiracılar tablo ShardManagement şema parçası olmayan ve tarafından EDCL yüklü değil.  Bu tabloda, ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterilmektedir.   


Sağlama yürütme asıl döndürür tamamlandıktan sonra *Demo ProvisionAndCatalog* komut dosyası, hangi açılır **olayları** sayfasını tarayıcıda yeni Kiracı için:

   ![etkinlikler](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracılar toplu sağlama

Bu alıştırmada 17 kiracılar toplu sağlar. Bu yüzden çalışmak için birden fazla birkaç veritabanlarını diğer Wingtip biletleri SaaS veritabanı Kiracı öğreticileri başına başlatmadan önce bu toplu kiracıların sağlamak önerilir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* değiştirip *$DemoScenario* parametre 3:
   * **$DemoScenario** = **3**, *kiracılar toplu sağlama*.
1. Betiği çalıştırmak için **F5**'e basın.

Betik, ek kiracı grubu dağıtır. Kullandığı bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) batch ve hangi denetleyen temsilciler her bir veritabanı bağlı bir şablonu için sağlama. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonları sağlama paralel olarak veritabanları ve gerekirse, yeniden deneme işler. Idempotent, betiğidir kadar başarısız olur ya da herhangi bir nedenle durdurur, yeniden çalıştırın.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Kiracı grubunun başarıyla dağıtıldığını doğrulama

* İçinde [Azure portal](https://portal.azure.com)sunucuları listesine göz atın ve Aç *tenants1* sunucu. Tıklatın **SQL veritabanları** ve 17 ek veritabanları toplu şimdi listede doğrulayın:

   ![veritabanı listesi öğesine tıklayın](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama düzenleri şunlardır:

**Veritabanlarını önceden hazırlama.** Önceden hazırlama düzeni ek bir maliyeti esnek havuzdaki veritabanları eklemeyin olgu yararlanan. Esnek havuz için veritabanlarını değil, faturalama olduğu ve boşta veritabanları hiçbir kaynaklarını tüketebilir. Bir havuzdaki veritabanları önceden sağlama ve bunları gerektiğinde ayırma Kiracı ekleme zamanı önemli ölçüde azaltılabilir. Önceden hazırlanan veritabanı sayısı arabellek beklenen sağlama oranı için uygun tutmak için gerektiği şekilde ayarlanabilir.

**Otomatik sağlama.** Otomatik sağlama deseni, bir sağlama hizmeti hükümleri sunucusu, havuzları ve otomatik olarak gerektiğinde – veritabanları esnek havuzlar önceden sağlama veritabanlarında isterseniz de dahil. Ve veritabanları XML'deki yaptırılan ve silinmiş, esnek havuzlar boşlukları sağlama hizmeti tarafından doldurulabilir. Bu tür bir hizmet Örneğin, birden çok farklı coğrafyalara, sağlama işleme basit veya karmaşık – olabilir ve olağanüstü durum kurtarma için coğrafi çoğaltma ayarlayabilirsiniz. Otomatik sağlama desen ile bir istemci uygulama veya betik sağlama hizmeti tarafından işlenmek üzere sıraya sağlama isteği gönderir ve tamamlanma belirlemek üzere hizmetini yoklar. Önceden hazırlama kullanılırsa, istekleri hızlı bir şekilde, arka planda değiştirme veritabanı sağlama hizmeti ile ele alınması.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracılar sağlama ve kataloğa kaydetme ayrıntılarını girme

Deneyin [performans izleme Öğreticisi](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* Ek [Kiracı uygulama başına Wingtip biletleri SaaS veritabanı üzerine yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
* [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
