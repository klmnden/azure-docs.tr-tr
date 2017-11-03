---
title: "Azure SQL veritabanı kullanan bir çok kiracılı uygulamalarda yeni kiracılar sağlama | Microsoft Docs"
description: "Sağlamak ve bir Azure SQL veritabanı çok kiracılı SaaS uygulamasında yeni kiracılar katalog hakkında bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: sstein
ms.openlocfilehash: eda330a7202de8a325d645b37a0d05ef8df8985b
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="learn-how-to-provision-and-catalog-new-tenants-and-register-them-in-the-catalog"></a>Sağlamak ve yeni kiracılar katalog ve kataloğa kaydetme hakkında bilgi edinin

Bu öğreticide, sağlama ve Katalog SaaS desenleri ve Wingtip SaaS uygulamada nasıl uygulandığı hakkında bilgi edinin. Oluşturun ve yeni Kiracı veritabanlarını başlatmak ve bunları uygulamanın Kiracı kataloğunda kaydedin. Katalog SaaS uygulamanın birçok kiracılar ve verileri arasında eşleme tutan bir veritabanıdır. Katalog doğru veritabanı uygulama isteklerine yönlendiren önemli bir rol oynar.  

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Nasıl bu uygulanan aracılığıyla atlama dahil olmak üzere tek yeni bir kiracı sağlama
> * Ek kiracı grubu sağlama


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip SaaS uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip SaaS uygulamasına keşfedin.](sql-database-saas-tutorial.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS Katalog düzenine giriş

Bir veritabanı yedeği çok kiracılı SaaS uygulamasına, her bir kiracı için bilgi depolandığı bilmeniz önemlidir. SaaS katalog desende bir katalog veritabanı her Kiracı arasında eşleme tutmak için kullanılır ve, verilerini depolandıkları. Wingtip SaaS uygulama tek Kiracı veritabanı mimarisi kullanır, ancak bir çok kiracılı veya tek Kiracı veritabanı kullanılıp kullanılmadığını Kiracı veritabanı eşleme bir katalogda depolanması temel desenini uygular.

Her bir kiracı katalogda tanımlayan bir anahtar atanır ve uygun veritabanı konumunu eşlenmiş. Wingtip SaaS uygulamada karmasını kiracının ad anahtarı oluşturulur. Bu Kiracı anahtarı oluşturmak için kullanılacak uygulama URL'si adı kısmını sağlar. Diğer Kiracı anahtar düzenleri kullanılabilir.  

Katalog adı veya uygulama üzerinde en az etkiyle değiştirilecek veritabanının konumunu sağlar.  Çok Kiracı veritabanı modelinde, bu da 'Kiracı veritabanları arasında taşıma' düzenler.  Katalog, bir kiracı veya veritabanı bakım veya başka eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir. İçinde bu incelediniz [tek bir kiracı öğretici geri](sql-database-saas-tutorial-restore-single-tenant.md).

Ayrıca, bir SaaS uygulaması için bir yönetim veritabanı etkili olur, katalog ek Kiracı veya katmanı veya bir veritabanı sürümü gibi veritabanı meta veri depolayabilirsiniz şema sürümü, hizmet planı veya SLA kiracılar ve sağlayan diğer bilgiler için sunulan Uygulama Yönetimi, müşteri desteği veya devops işlemler.  

SaaS uygulamasına Katalog veritabanı araçları etkinleştirebilirsiniz.  Wingtip SaaS örnek Kataloğu içinde incelediniz arası Kiracı sorgu etkinleştirmek için kullanılan [geçici analytics Öğreticisi](sql-database-saas-tutorial-adhoc-analytics.md). Veritabanları arası iş yönetimi incelediniz içinde [Şema Yönetimi](sql-database-saas-tutorial-schema-management.md) ve [Kiracı analytics](sql-database-saas-tutorial-tenant-analytics.md) öğreticileri. 

Wingtip SaaS uygulama Kataloğu parça yönetim özelliklerini kullanarak uygulanan [esnek veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). EDCL oluşturmak, yönetmek ve bir veritabanı yedeği parça eşlemesi kullanmak için bir uygulama sağlar. Bir parça eşleme parça (veritabanları) ve anahtarları (kiracılar) ile veritabanları arasında eşleme listesini içerir.  EDCL işlevleri uygulamalarından ya da Kiracı parça eşlemesinde girişleri oluşturmak için sağlama sırasında PowerShell betikleri ve uygulamalardan verimli bir şekilde doğru veritabanına bağlanmak için kullanılabilir. EDCL katalog veritabanına trafiğini en aza indirmek ve uygulamayı oluşturan hızlandırmak için bağlantı bilgilerini önbelleğe alır.  

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanından erişilebilir, ancak *verileri düzenlemeyin*! Eşleme verilerini yalnızca Elastik Veritabanı İstemci Kitaplığı API’sini kullanarak düzenleyin. Eşleme verilerinin doğrudan değiştirilmesi, kataloğu bozabilir ve desteklenmez.


## <a name="introduction-to-the-saas-provisioning-pattern"></a>SaaS sağlama düzeni giriş

Ne zaman ekleme tek Kiracı veritabanı modeli yeni bir kiracı veritabanı kullanan bir SaaS uygulaması yeni bir kiracı sağlanmalıdır.  Uygun bir konum ve hizmet katmanı, uygun şemayı ve başvuru verileri ile başlatıldı ve uygun Kiracı anahtarı altında kataloğunda kayıtlı oluşturulması gerekir.  

Farklı yaklaşımlar sağlama, veritabanı için SQL komut dosyaları yürütme, bir bacpac dağıtma ya da 'Altın' şablon veritabanı kopyalayarak içerebilir kullanılabilir.  

Kullandığınız sağlama yaklaşım, yeni veritabanları ile en son şema sağlandığından emin olun, genel şema yönetim stratejinizi içinde comprehended gerekir.  İçinde bu incelediniz [şema yönetimi Öğreticisi](sql-database-saas-tutorial-schema-management.md).  

Basetenantdb, adlı bir altın veritabanı kopyalayarak Wingtip SaaS uygulama hükümleri yeni kiracılar katalog sunucusunda dağıtıldı.  Sağlama bir kayıt deneyimi bir parçası olarak uygulamaya tümleşik ve/veya desteklenen komut dosyalarını kullanarak çevrimdışı. Bu öğretici, PowerShell kullanarak sağlama inceleyeceksiniz. Sağlama komut dosyalarını bir esnek havuzda yeni bir kiracı veritabanı oluşturmak sonra Kiracı özgü bilgiyle başlatmak ve Katalog parça eşlemesinde kaydetmek için basetenantdb kopyalayın.  Örnek uygulama veritabanlarını Kiracı adına göre adları verilir, ancak bu deseni önemli bir parçası değildir – veritabanına atanmış herhangi bir ad katalog kullanılmasına izin verir. + 


## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip SaaS komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github depo. [Wingtip SaaS komut dosyalarını karşıdan yüklemek için adımları](sql-database-wtp-overview.md#download-and-unblock-the-wingtip-saas-scripts).


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Yeni Kiracı sağlama Wingtip uygulama nasıl uyguladığını anlamak için bir kesme noktası ve iş akışı aracılığıyla adım bir kiracı sağlarken ekleyin:

1. Aç... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayabilirsiniz:
   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*).
   * **$VenueType** önceden tanımlanmış salonundan türlerinden birini =: *mavi*, classicalmusic, dance jazz, judo, motorracing, çok amaçlı, opera, rockmusic, futbol.
   * **$DemoScenario** = **1**, kümesine **1** için *tek bir kiracı sağlama*.

1. İmlecinizi herhangi bir yere satırı 48, yazan satırı koyarak kesme noktası ekleme: *yeni Kiracı '*ve basın **F9**.

   ![kesme noktası](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Komut dosyası tuşuna çalıştırmak için **F5**.

1. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.

   ![kesme noktası](media/sql-database-saas-tutorial-provision-and-catalog/debug.png)



Betiğin yürütmeyi kullanarak **hata ayıklama** menü seçeneklerini - **F10** ve **F11** adıma üzerinden veya çağrılan işlev. PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


Değil açıkça izlemeniz gereken adımlar, ancak, aracılığıyla komut dosyası hata ayıklama sırasında adım iş akışı bir açıklaması verilmiştir:

1. Azure’da oturum açma ve birlikte çalıştığınız Azure aboneliğini seçme işlevlerini içeren **SubscriptionManagement.psm1 modülünü içeri aktarın**.
1. [Shard Management](sql-database-elastic-scale-shard-map-management.md) işlevlerinde katalog ve kiracı düzeyinde özet sağlayan **CatalogAndDatabaseManagement.psm1 modülünü içeri aktarın**. Bu, katalog düzeninin büyük bölümünü kapsülleyen ve incelenmesi yararlı olan önemli bir modüldür.
1. **Yapılandırma ayrıntılarını alın**. Get-yapılandırmasını (F11) adımla ve uygulama yapılandırma nasıl belirtilen bakın. Kaynak adları ve uygulamaya özel diğer değerler burada tanımlanır, ancak betikler hakkında bilgi sahibi olmadan bu değerlerin hiçbirini değiştirmeyin.
1. **Katalog nesnesini alın**. Oluşturur ve üst düzey komut dosyasında kullanılan bir katalog nesnesi döndüren Get katalog adımla.  Bu işlev üzerinden içe aktarılan parça yönetim işlevlerini kullanan **AzureShardManagement.psm1**. Katalog nesnesi aşağıdakilerden oluşur:
   * $catalogServerFullyQualifiedName, standart gövdeye ek olarak Kullanıcı adınız kullanılarak oluşturulur: _catalog-\<user\>.database.windows.net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki *tenantcatalog* parça eşlemesinden başlatılır.
   Bir katalog nesnesi oluşturulup döndürülür ve daha üst düzey betikte kullanılır.
1. **Yeni kiracı anahtarını hesaplayın**. Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
1. **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**. Anahtarın olup olmadığından emin olmak için katalog denetlenir.
1. **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** Kullanım **F11** adımını ve veritabanının nasıl olduğunu görmek için kullanılarak hazırlanmış bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md).

Veritabanı adı, hangi parçanın hangi kiracıya ait olduğunu netleştirmek üzere kiracı adından oluşturulur. (Veritabanı adlandırma diğer stratejileri kolayca kullanılabilir.) + Bir Resource Manager şablonu katalog sunucusunda altın veritabanı (baseTenantDB) kopyalayarak bir kiracı veritabanı oluşturmak için kullanılır. Boş bir veritabanı oluşturmak ve bir bacpac içeri aktararak başlatmak için ya da iyi bilinen bir konumdan başlatma komut dosyasını çalıştırmak için alternatif bir yaklaşım olabilir.  

Resource Manager şablonu ...\Learning Modules\Common\ klasöründedir: *tenantdatabasecopytemplate.json*

Kiracı veritabanı oluşturulduktan sonra daha sonra başka **salonundan (Kiracı) adı ve salonundan türü ile başlatılmış**. Burada, diğer başlatma işlemleri de yapılabilir.

**Kataloğa kayıtlı Kiracı veritabanı** ile *Ekle TenantDatabaseToCatalog* Kiracı anahtarı kullanarak. Ayrıntıları görmek için **F11** tuşunu kullanın:

* Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
* Anahtar değerini parçaya bağlayan eşleme oluşturulur.
* Kiracı hakkında ek meta veriler (salonundan kişinin adı), katalog kiracılar tablosuna eklenir.  Kiracılar tablo ShardManagement şema parçası olmayan ve tarafından EDCL yüklü değil.  Bu tabloda, ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterilmektedir.   


Sağlama yürütme asıl döndürür tamamlandıktan sonra *Demo ProvisionAndCatalog* komut dosyası, hangi açılır **olayları** sayfasını tarayıcıda yeni Kiracı için:

   ![etkinlikler](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracılar toplu sağlama

Bu alıştırmada 17 kiracılar toplu sağlar. Bu yüzden çalışmak için birden fazla birkaç veritabanlarını Wingtip SaaS öğreticilere başlatmadan önce bu toplu kiracıların sağlamak önerilir.

1. Aç... \\Öğrenme modülleri\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* içinde *PowerShell ISE* değiştirip *$ DemoScenario* parametre 3:
   * **$DemoScenario** = **3**, değiştirmek **3** için *kiracılar toplu sağlama*.
1. **F5** tuşuna basıp betiği çalıştırın.

Betik, ek kiracı grubu dağıtır. Kiracı grubunu denetleyen ve ardından her bir veritabanının sağlama işlemini bağlı bir şablona devreden bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) kullanır. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonlar mümkün olduğunda veritabanlarını paralel olarak sağlar ve gerekirse genel süreci iyileştirmek için yeniden denemeleri işler. Idempotent betiğidir kadar başarısız olur ya da herhangi bir nedenle durdurur, yeniden çalıştırın.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Kiracı grubunun başarıyla dağıtıldığını doğrulama

* Açık *tenants1* sunucuları listesine giderek sunucu [Azure portal](https://portal.azure.com), tıklatın **SQL veritabanları**ve 17 ek veritabanları toplu şimdi içinde doğrulayın listesi:

   ![veritabanı listesi öğesine tıklayın](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)



## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama düzenleri şunlardır:

**Veritabanlarını önceden hazırlama.** Önceden hazırlama düzeni ek bir maliyeti esnek havuzdaki veritabanları eklemeyin olgu yararlanan. Esnek havuz için veritabanlarını değil, faturalama olduğu ve boşta veritabanları hiçbir kaynaklarını tüketebilir. Bir havuzdaki veritabanları önceden sağlama ve bunları gerektiğinde ayırma Kiracı ekleme zamanı önemli ölçüde azaltılabilir. Önceden sağlanan veritabanı sayısı, öngörülen sağlama hızı için uygun olan bir arabellek belirlemek üzere gereken şekilde ayarlanabilir.

**Otomatik sağlama.** Otomatik sağlama modelinde, sunucular, havuzları ve veritabanları – önceden sağlama veritabanları esnek havuzları, isterseniz de dahil olmak üzere gerektiğinde otomatik olarak sağlamak için ayrılmış bir sağlama hizmeti kullanılır. Veritabanları kullanımdan çıkarılır ve silinirse, elastik havuzlardaki boşluklar sağlama hizmeti tarafından istenen şekilde doldurulabilir. Bu tür bir hizmet Örneğin, birden çok farklı coğrafyalara, sağlama işleme basit veya karmaşık – olabilir ve bu strateji olağanüstü durum kurtarma için kullanılırsa coğrafi çoğaltma otomatik olarak ayarlayabilirsiniz. Otomatik sağlama düzeni ile bir istemci uygulaması veya betik, bir kuyruğa sağlama hizmeti tarafından işlenecek bir sağlama isteği gönderir ve sonra işlemin tamamlandığını belirlemek üzere hizmeti sorgular. Önceden sağlama kullanılırsa, istekler arka planda çalışan bir yedek veritabanının sağlanmasını yöneten hizmet ile hızla işlenebilir.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracılar sağlama ve kataloğa kaydetme ayrıntılarını girme

Deneyin [performans izleme Öğreticisi](sql-database-saas-tutorial-performance-monitoring.md).

## <a name="additional-resources"></a>Ek Kaynaklar

* Ek [Wingtip SaaS uygulamasına yapı öğreticileri](sql-database-wtp-overview.md#sql-database-wingtip-saas-tutorials)
* [Elastik veritabanı istemci kitaplığı](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
