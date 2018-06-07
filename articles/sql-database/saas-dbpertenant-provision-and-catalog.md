---
title: Azure SQL Database kullanan çok müşterili bir uygulama yeni kiracılar sağlama | Microsoft Docs
description: Sağlamak ve yeni bir Azure SQL veritabanı çok kiracılı SaaS uygulama kiracılar katalog hakkında bilgi edinin
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 7c526f65be5e4a3ea50de4603441e6184abf8edd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643626"
---
# <a name="learn-how-to-provision-new-tenants-and-register-them-in-the-catalog"></a>Yeni kiracılar sağlamak ve kataloğa kaydetme hakkında bilgi edinin

Bu öğreticide, sağlamak ve SaaS desenleri katalog öğrenin. Ayrıca, bunlar Wingtip biletleri SaaS Kiracı başına veritabanı uygulamada nasıl uygulandığı öğrenin. Yeni Kiracı veritabanlarını başlatmak oluşturmak ve uygulamanın Kiracı kataloğunda kaydedin. Katalog SaaS uygulamanın birçok kiracılar ve verileri arasında eşleme tutan bir veritabanıdır. Katalog uygulama ve doğru veritabanı yönetim isteklerine yönlendiren önemli bir rol oynar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Tek bir yeni Kiracı sağlayın.
> * Ek kiracılar toplu sağlayın.


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS Kiracı başına veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS Kiracı başına veritabanı uygulama keşfetme](saas-dbpertenant-get-started-deploy.md).
* Azure PowerShell’in yüklendiğinden. Daha fazla bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS katalog düzeni giriş

Bir veritabanı yedeği çok kiracılı SaaS uygulamasına, her bir kiracı için bilgi depolandığı bilmeniz önemlidir. SaaS katalog desende bir katalog veritabanı her bir kiracı ile verilerinin depolandığı veritabanı arasında eşleme tutmak için kullanılır. Kiracı verilerini birden çok veritabanı arasında dağıtıldığında bu deseni uygular.

Her bir kiracı kataloğunda kendi veritabanı konumu için eşlenmiş bir anahtar tarafından tanımlanır. Wingtip biletleri uygulamada karmasını kiracının ad anahtarı oluşturulur. Bu düzen uygulama URL'SİNDE bulunan Kiracı adı anahtarından oluşturmak yazmasına izin verir. Diğer Kiracı anahtar düzenleri kullanılabilir.  

Katalog adı veya uygulama üzerinde en az etkiyle değiştirilecek veritabanının konumunu sağlar. Çok müşterili veritabanı modelinde, bu özellik, bir kiracı veritabanları arasında taşıma de kapsar. Katalog, bir kiracı veya veritabanı bakım veya başka eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir. Bu özellik, keşfedilen [geri yükleme tek bir kiracı Öğreticisi](saas-dbpertenant-restore-single-tenant.md).

Şema sürümü, hizmet planı veya SLA kiracılar için sunulan gibi Kataloğu ayrıca ek Kiracı veya veritabanı metaverileri depolayabilirsiniz. Katalog, uygulama yönetimi, müşteri desteği veya DevOps sağlayan diğer bilgileri depolayabilirsiniz. 

SaaS uygulamasına Katalog veritabanı araçları etkinleştirebilirsiniz. Wingtip biletleri SaaS Kiracı başına veritabanı örneğinde, katalog olarak keşfedilen arası Kiracı sorgu etkinleştirmek için kullanılan [geçici raporlama Öğreticisi](saas-tenancy-cross-tenant-reporting.md). Veritabanları arası iş yönetimi incelediniz içinde [Şema Yönetimi](saas-tenancy-schema-management.md) ve [Kiracı analytics](saas-tenancy-tenant-analytics.md) öğreticileri. 

Wingtip biletleri SaaS örneklerinde parça yönetim özelliklerini kullanarak katalog uygulanır [esnek veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). EDCL, Java ve .NET Framework içinde kullanılabilir. EDCL oluşturmak, yönetmek ve bir veritabanı yedeği parça eşlemesi kullanmak için bir uygulama sağlar. 

Bir parça eşleme parça (veritabanları) ve anahtarları (kiracılar) ile parça arasında eşleme listesini içerir. EDCL işlevleri Kiracı parça eşlemesinde girişleri oluşturmak için sağlama sırasında kullanılır. Bunlar çalışma zamanında uygulama tarafından doğru veritabanına bağlanmak için kullanılır. EDCL katalog veritabanına trafiğini en aza indirmek ve uygulamayı oluşturan hızlandırmak için bağlantı bilgilerini önbelleğe alır. 

> [!IMPORTANT]
> Eşleme veri Kataloğu veritabanında erişilebilen ancak *düzenleme yapmadığınız*. Sadece esnek veritabanı istemci kitaplığı API'leri kullanarak eşleme verilerini düzenleyin. Doğrudan eşleme verilerini düzenleme katalog bozulmasını risklerini ve desteklenmiyor.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>SaaS sağlama düzeni giriş

Yeni bir kiracı tek Kiracı veritabanı modelini kullanan bir SaaS uygulamasına eklediğinizde, yeni bir kiracı veritabanı hazırlamanız gerekir. Veritabanı uygun bir konum ve hizmet katmanının oluşturulması gerekir. Ayrıca uygun şemayı ve başvuru verileri ile başlatılmalıdır. Ve uygun Kiracı anahtarı altında kataloğunda kaydedilmelidir. 

Veritabanı sağlama için farklı yaklaşımlara kullanılabilir. SQL komut dosyaları yürütme, bir bacpac dağıtabilir veya bir şablon veritabanı kopyalayın. 

Veritabanı sağlama şema yönetimi stratejinizin parçası olması gerekir. Yeni veritabanları ile en son şema sağlandığından emin olmanız gerekir. Bu gereksinim, keşfedilen [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md). 

Adlı bir şablon veritabanı kopyalayarak yeni kiracılar Wingtip biletleri Kiracı başına veritabanı uygulama hazırlar _basetenantdb_, katalog sunucusunda dağıtılır. Sağlama bir kayıt deneyimi bir parçası olarak uygulamaya tümleştirilebilir. Bu da çevrimdışı komut dosyalarını kullanarak desteklenebilir. Bu öğretici, PowerShell kullanarak sağlama araştırır. 

Komut dosyaları kopyalama sağlama _basetenantdb_ esnek havuzda yeni bir kiracı veritabanı oluşturmak için veritabanı. Kiracı veritabanı eşlenmiş Kiracı sunucu oluşturulan _newtenant_ DNS diğer adı. Bu diğer ad yeni kiracılar sağlamak için kullanılan sunucu başvuru korur ve olağanüstü durum kurtarma eğitimlerine bir kurtarma Kiracı sunucuya işaret edecek şekilde güncelleştirilir ([georestore kullanarak DR](saas-dbpertenant-dr-geo-restore.md), [georeplicationkullanarakDR](saas-dbpertenant-dr-geo-replication.md)). Komut dosyaları sonra Kiracı özgü bilgileri veritabanıyla başlatmak ve Katalog parça eşlemesinde kaydedin. Kiracı veritabanları verilen Kiracı adına göre adlardır. Bu adlandırma şeması düzeni önemli bir parçası değil. Herhangi bir adlandırma kuralı kullanılabilmesi için katalog için veritabanı adı, Kiracı anahtarınızı eşler. 


## <a name="get-the-wingtip-tickets-saas-database-per-tenant-application-scripts"></a>Wingtip biletleri SaaS Kiracı başına veritabanı uygulama komut dosyaları alma

Wingtip biletleri SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS DbPerTenant](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant) GitHub depo. Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri SaaS betikleri engellemesini kaldırmak.


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Wingtip biletleri uygulama sağlama yeni Kiracı nasıl uyguladığını anlamak için bir kesme noktası ekleme ve iş akışı bir kiracı sağlama sırasında izleyin.

1. PowerShell ISE Aç... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayabilirsiniz:

   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*).
   * **$VenueType** önceden tanımlanmış salonundan türlerinden birini =: _mavi, classicalmusic, dance, jazz, judo, yarış, çok amaçlı motor, opera, rockmusic, futbol_.
   * **$DemoScenario** = **1**, *tek bir kiracı sağlama*.

2. Bir kesme noktası eklemek için imlecinizi herhangi bir yere bildiren satıra yerleştirin *yeni Kiracı '*. F9 tuşuna basın.

   ![Kesme noktası](media/saas-dbpertenant-provision-and-catalog/breakpoint.png)

3. Komut dosyasını çalıştırmak için F5 tuşuna basın.

4. Betik yürütme kesme noktasında durdurulduktan sonra koda adım için F11 tuşuna basın.

   ![Hata ayıklama](media/saas-dbpertenant-provision-and-catalog/debug.png)



Komut yürütme kullanarak izleme **hata ayıklama** menü seçenekleri. Üzerinden veya çağrılan işlevler halinde adım için F10 ve F11 tuşlarına basın. PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Bu iş akışı açıkça izlemeniz gerekmez. Bu komut dosyası hata ayıklama açıklanmaktadır.

* **CatalogAndDatabaseManagement.psm1 modülünü içeri aktarın.** Üzerinden bir katalog ve Kiracı düzeyinde Özet sağlayan [parça yönetim](sql-database-elastic-scale-shard-map-management.md) işlevleri. Bu modül katalog düzeni çoğunu yalıtır ve incelenmesi yararlı olur.
* **SubscriptionManagement.psm1 modülünü içeri aktarın.** Azure'da oturum açma ve birlikte çalışmak istediğiniz Azure aboneliği seçmek için işlevler içerir.
* **Yapılandırma ayrıntıları alın.** Get-yapılandırma F11 kullanarak adım ve uygulama yapılandırma nasıl belirtilen bakın. Kaynak adları ve diğer uygulamaya özgü değerleri burada tanımlanır. Komut dosyaları hakkında bilgi sahibi olduktan kadar bu değerleri değiştirmeyin.
* **Katalog nesnesini alın.** Oluşturur ve üst düzey komut dosyasında kullanılan bir katalog nesnesi döndüren Get-katalog adımla. Bu işlev üzerinden içe aktarılan parça yönetim işlevlerini kullanan **AzureShardManagement.psm1**. Katalog nesnesi aşağıdaki öğelerden oluşur:

   * $catalogServerFullyQualifiedName standart stem artı kullanıcı adınızı kullanarak yapılandırılmıştır: _katalog -\<kullanıcı\>. database.windows .net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki _tenantcatalog_ parça eşlemesinden başlatılır. Bir katalog nesnesi oluşur ve döndürdü. Üst düzey komut dosyasında kullanılır.
* **Yeni Kiracı anahtarına hesaplayın.** Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
* **Kiracı anahtarı olup olmadığını denetle.** Katalog anahtar kullanılabilir olduğundan emin olmak için denetlenir.
* **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** F11 nasıl veritabanını kullanarak sağlandığında içine adım kullanacağınız bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md).

    Veritabanı adı, hangi parçanın hangi kiracıya ait olduğunu netleştirmek üzere kiracı adından oluşturulur. Diğer veritabanı adlandırma kuralları da kullanabilirsiniz. Bir şablon veritabanı kopyalayarak Kiracı veritabanı bir Resource Manager şablonu oluşturur (_baseTenantDB_) katalog sunucusunda. Alternatif olarak, bir veritabanı oluşturun ve bir bacpac içeri aktararak başlatın. Veya iyi bilinen bir konumdan bir başlatma komut dosyasını çalıştırabilir.

    Resource Manager şablonu ...\Learning Modules\Common\ klasöründedir: *tenantdatabasecopytemplate.json*

* **Daha fazla Kiracı veritabanı başlatılır.** Salonundan (Kiracı) adı ve salonundan türü eklenir. Diğer başlatma burada da yapabilirsiniz.

* **Kiracı veritabanı kataloğa kayıtlı değil.** Üzerinde kayıtlı *Ekle TenantDatabaseToCatalog* Kiracı anahtarını kullanarak. Ayrıntılara adım için F11 kullanın:

    * Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
    * Anahtar değerini parçaya bağlayan eşleme oluşturulur.
    * Kiracı (salonundan kişinin adı) hakkında ek meta veri Kataloğu kiracılar tablosuna eklenir. Kiracılar tablo Parça Yönetimi şeması parçası olmayan ve tarafından EDCL yüklü değil. Bu tabloda, ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterilmektedir.


Sağlama yürütme asıl döndürür tamamlandıktan sonra *Demo ProvisionAndCatalog* komut dosyası. **Olayları** tarayıcıda yeni Kiracı için sayfası açılır.

   ![Olayları sayfası](media/saas-dbpertenant-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracılar toplu sağlama

Bu alıştırmada 17 kiracılar toplu sağlar. Bu toplu kiracıların diğer Wingtip biletleri SaaS Kiracı başına veritabanı öğreticileri başlatmadan önce sağlamak öneririz. Çalışmak için birden fazla birkaç veritabanı yok.

1. PowerShell ISE Aç... \\Modülleri öğrenme\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1*. Değişiklik *$DemoScenario* parametre 3:

   * **$DemoScenario** = **3**, *kiracılar toplu sağlama*.
2. Komut dosyasını çalıştırmak için F5 tuşuna basın.

Betik, ek kiracı grubu dağıtır. Kullandığı bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) toplu denetler ve temsilciler her bir veritabanı bağlı bir şablonu için sağlama. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonlar, gerekirse paralel ve tanıtıcı deneme veritabanlarında sağlayın. Idempotent, betiğidir kadar başarısız olur ya da herhangi bir nedenle durdurur, yeniden çalıştırın.

### <a name="verify-the-batch-of-tenants-that-successfully-deployed"></a>Başarıyla dağıtılan kiracılar toplu doğrulayın

* İçinde [Azure portal](https://portal.azure.com)sunucuları listesine göz atın ve Aç *tenants1* sunucu. Seçin **SQL veritabanları**ve 17 ek veritabanları toplu şimdi listesinde olduğundan emin olun.

   ![Veritabanı listesi](media/saas-dbpertenant-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide bulunmayan diğer sağlama modelleri:

**Veritabanlarını önceden sağlama**: önceden sağlama düzeni ek bir maliyeti esnek havuzdaki veritabanları eklemeyin olgu yararlanan. Faturalama esnek havuz için veritabanları ' dir. Boşta veritabanları hiçbir kaynaklarını tüketebilir. Bir havuzu ve bunları gerektiğinde ayırma önceden sağlama veritabanı tarafından kiracılar eklemek için zaman azaltabilir. Önceden hazırlanan veritabanı sayısı arabellek beklenen sağlama oranı için uygun tutmak için gerektiği şekilde ayarlanabilir.

**Otomatik sağlama**: otomatik sağlama desende sağlama hizmeti sunucular, havuzları ve veritabanları gerektiğinde otomatik olarak sağlar. İsterseniz, esnek havuzlar veritabanlarında önceden sağlama dahil olabilir. Veritabanları kullanımdan ve silinmiş, esnek havuzlar boşlukları sağlama hizmeti tarafından doldurulabilir. Bu tür bir hizmet, basit veya birden çok farklı coğrafyalara sağlama ve olağanüstü durum kurtarma için coğrafi çoğaltma ayarlama işleme gibi karmaşık olabilir. 

Otomatik sağlama desen ile bir istemci uygulama veya betik sağlama hizmeti tarafından işlenmesi için bir sağlama isteği kuyruğuna gönderir. Ardından, tamamlama belirlemek üzere hizmetini yoklar. Önceden hazırlama kullanılırsa, istekleri hızlı bir şekilde ele alınır. Hizmet değiştirme veritabanı arka planda sağlar.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]

> * Tek bir yeni Kiracı sağlayın.
> * Ek kiracılar toplu sağlayın.
> * Kiracılar sağlama ve kataloğa kaydetme ayrıntılarını adımla.

Deneyin [performans izleme Öğreticisi](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip biletleri SaaS Kiracı başına veritabanı uygulamayı derleme öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
* [Windows PowerShell ISE betiklerde hata ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
