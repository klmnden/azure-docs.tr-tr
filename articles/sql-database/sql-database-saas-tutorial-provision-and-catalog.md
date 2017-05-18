---
title: "Yeni kiracılar sağlama ve kataloğa kaydetme (Azure SQL Veritabanı kullanan örnek SaaS uygulaması) | Microsoft Belgeleri"
description: "Yeni kiracılar sağlama ve kataloğa kaydetme"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 4eeada941f8615fa04624bc725efcb44f05d56c7
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="provision-new-tenants-and-register-them-in-the-catalog"></a>Yeni kiracılar sağlama ve bunları kataloğa kaydetme

Bu öğreticide, Wingtip Bilet Platformu (WTP) SaaS uygulamasında yeni kiracılar sağlayacaksınız. Kiracılar ve kiracı veritabanları oluşturup bu kiracıları kataloğa kaydedeceksiniz. *Katalog*, birçok SaaS uygulamaları kiracısı ile bu kiracıların verileri arasında eşleme sağlayan bir veritabanıdır. Kullanılan sağlama ve kataloğa kaydetme düzenlerini keşfetmek ve kataloğa yeni kiracı kaydetme işleminin nasıl uygulandığını öğrenmek için bu betikleri kullanın. Katalog, uygulama isteklerini doğru veritabanlarına yönlendirmede önemli bir rol oynar.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Yeni kiracı sağlama ve bu kiracıları kataloğa kaydetme ile ilgili ayrıntılara gidin.


Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* WTP uygulamasının dağıtıldığından. Beş dakikadan daha kısa sürede dağıtmak için [WTP SaaS uygulamasını dağıtma ve keşfetme](sql-database-saas-tutorial.md) bölümünü inceleyin
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-saas-catalog-pattern"></a>SaaS Katalog düzenine giriş

Veritabanı tarafından desteklenen çok kiracılı bir SaaS uygulamasında, kiracılara ait bilgilerin nerede depolandığının bilinmesi önemlidir. SaaS katalog düzeninde, kiracılar ile verilerinin depolandığı yer arasındaki eşlemeyi tutmak için bir katalog veritabanı kullanılır. WTP uygulaması tek kiracılı bir veritabanı mimarisi kullanır. Ancak bir katalogdaki depolanan kiracı-veritabanı eşlemesinin temel düzeni, çok kiracılı veya tek kiracılı bir veritabanının kullanılmasına bağlı olarak uygulanır.

Her kiracıya, katalogdaki verilerini ayırt eden bir anahtar atanır. Bu anahtar, WTP uygulamasında kiracı adının bir karmasından oluşturulur. Bu düzen, anahtarı oluşturmak ve belirli bir kiracının bağlantısını almak üzere uygulama URL’sindeki kiracı adı kısmının kullanılmasına olanak tanır. Diğer kimlik şemaları, genel düzeni etkilemeden kullanılabilir.

WTP uygulamasındaki katalog, [Elastik Veritabanı İstemci Kitaplığı (EDCL)](sql-database-elastic-database-client-library.md) içindeki Shard Management teknolojisi kullanılarak uygulanır. EDCL, _parça eşlemesinin_ sağlandığı veritabanı tarafından desteklenen bir _kataloğu_ oluşturup yönetmekten sorumludur. Katalog, anahtarlar (kiracılar) ile veritabanları (parçalar) arasındaki eşlemeyi içerir.

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanından erişilebilir, ancak *verileri düzenlemeyin*! Eşleme verilerini yalnızca Elastik Veritabanı İstemci Kitaplığı API’sini kullanarak düzenleyin. Eşleme verilerinin doğrudan değiştirilmesi, kataloğu bozabilir ve desteklenmez.

Wingtip SaaS uygulaması bir *altın* veritabanı kopyalayarak yeni kiracılar sağlar.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk WTP öğreticisinde zaten bir kiracı oluşturduysanız, sonraki bölüme atlayabilirsiniz: [kiracı grubu sağlama](#provision-a-batch-of-tenants).

Bir kiracıyı hızlıca oluşturup kataloğa kaydetmek için *Demo-ProvisionAndCatalog* betiğini çalıştırın:

1. PowerShell ISE’de **Demo-ProvisionAndCatalog.ps1**’i açıp aşağıdaki değerleri ayarlayın:
   * **$TenantName** = yeni mekanın adı (örneğin, *Bushwillow Blues*). 
   * **$VenueType** = önceden tanımlanmış mekan türlerinden biri: blues, klasik müzik, dans, jazz, judo, motosiklet yarışı, çok amaçlı, opera, rock müzik, futbol.
   * **$DemoScenario** = 1, **Tek kiracı sağlamak** için bu ayarı _1_ olarak bırakın.

1. **F5** tuşuna basıp betiği çalıştırın.

Betik tamamlandıktan sonra yeni kiracı sağlanır ve bu kiracının *Etkinlikler* uygulaması tarayıcıda açılır:

![Yeni kiracı](./media/sql-database-saas-tutorial-provision-and-catalog/new-tenant.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracı grubu sağlama

Bu alıştırmada ek kiracı grubu sağlanır. Diğer WTP öğreticilerini tamamlamadan önce bunu yapmanız önerilir.

1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\Yardımcı Programlar\\*Demo-ProvisionAndCatalog.ps1*’i açıp aşağıdaki değeri ayarlayın:
   * **$DemoScenario** = **3**, **Kiracı grubu sağlamak** için **3** olarak ayarlayın.
1. **F5** tuşuna basıp betiği çalıştırın.

Betik, ek kiracı grubu dağıtır. Kiracı grubunu denetleyen ve ardından her bir veritabanının sağlama işlemini bağlı bir şablona devreden bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-manager-template-walkthrough.md) kullanır. Şablonların bu şekilde kullanılması, Azure Resource Manager’ın betiğinizin sağlama işlemine aracılık etmesine olanak tanır. Şablonlar mümkün olduğunda veritabanlarını paralel olarak sağlar ve gerekirse genel süreci iyileştirmek için yeniden denemeleri işler. Betik bir kez etkili olduğundan kesintiye uğraması halinde yeniden çalıştırmanız gerekir.

### <a name="verify-the-batch-of-tenants-successfully-deployed"></a>Kiracı grubunun başarıyla dağıtıldığını doğrulama

* [Azure portalında](https://portal.azure.com) *tenants1* sunucusunu açıp **SQL veritabanları**:

   ![veritabanı listesi öğesine tıklayın](media/sql-database-saas-tutorial-provision-and-catalog/database-list.png)


## <a name="provision-and-catalog-detailed-walkthrough"></a>Sağlama ve kataloğa kaydetme ile ilgili ayrıntılı kılavuz

Wingtip uygulamasının yeni kiracı sağlama işlemini nasıl uyguladığını daha iyi anlamak için *Demo-ProvisionAndCatalog* betiğini yeniden çalıştırıp başka bir kiracı sağlayın. Bu defa, bir kesme noktası ekleyin ve iş akışında ilerleyin:

1. ...\\Öğrenme Modülleri\Yardımcı Programlar\_Demo-ProvisionAndCatalog.ps1_ öğesini açıp aşağıdaki değerleri geçerli katalogda yer almayan yeni kiracı değerlerine ayarlayın:
   * **$TenantName** = yeni bir ad olarak ayarlayın (örneğin, *Hackberry Hitters*).
   * **$VenueType** = önceden tanımlanmış mekan türlerinden birini kullanın (örneğin, *judo*).
   * **$DemoScenario** = 1, **Tek kiracı sağlamak** için **1** olarak ayarlayın.

1. İmlecinizi *New-Tenant `* satırında herhangi bir yere getirerek bir kesme noktası ekleyin ve **F9**’a basın.

   ![kesme noktası](media/sql-database-saas-tutorial-provision-and-catalog/breakpoint.png)

1. Betiği çalıştırmak için **F5**'e basın. Kesme noktasına isabet edildiğinde girmek için **F11** tuşuna basın. Çağrılan işlevleri atlamak veya bu işlevlere girmek için **F10** ve **F11** tuşlarını kullanarak betiğin yürütme işlemini izleyin. [PowerShell betikleriyle çalışmaya ve bu betiklerde hata ayıklamaya yönelik ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

### <a name="examine-the-provision-and-catalog-implementation-in-detail-by-stepping-through-the-script"></a>Betiğe girerek sağlama ve kataloğa kaydetme uygulamasını ayrıntılı olarak inceleme

Betik, aşağıdaki adımları uygulayarak yeni kiracılar sağlar ve kataloğunu oluşturur:

1. Azure’da oturum açma ve birlikte çalıştığınız Azure aboneliğini seçme işlevlerini içeren **SubscriptionManagement.psm1 modülünü içeri aktarın**.
1. [Shard Management](sql-database-elastic-scale-shard-map-management.md) işlevlerinde katalog ve kiracı düzeyinde özet sağlayan **CatalogAndDatabaseManagement.psm1 modülünü içeri aktarın**. Bu, katalog düzeninin büyük bölümünü kapsülleyen ve incelenmesi yararlı olan önemli bir modüldür.
1. **Yapılandırma ayrıntılarını alın**. _Get-Configuration_ betiğine girin (**F11** ile) ve uygulama yapılandırmasının nasıl belirtildiğine bakın. Kaynak adları ve uygulamaya özel diğer değerler burada tanımlanır, ancak betikler hakkında bilgi sahibi olmadan bu değerlerin hiçbirini değiştirmeyin.
1. **Katalog nesnesini alın**. **AzureShardManagement.psm1** öğesinden içeri aktarılan Shard Management işlevleri kullanılarak kataloğun nasıl başlatıldığını görmek için *Get-Catalog* betiğine girin. Katalog aşağıdaki nesnelerden oluşur:
   * $catalogServerFullyQualifiedName, standart gövdeye ek olarak Kullanıcı adınız kullanılarak oluşturulur: _catalog-\<user\>.database.windows.net_.
   * $catalogDatabaseName, *tenantcatalog* yapılandırmasından alınır.
   * $shardMapManager nesnesi, katalog veritabanından başlatılır.
   * $shardMap nesnesi, katalog veritabanındaki *tenantcatalog* parça eşlemesinden başlatılır.
   Bir katalog nesnesi oluşturulup döndürülür ve daha üst düzey betikte kullanılır.
1. **Yeni kiracı anahtarını hesaplayın**. Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
1. **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**. Anahtarın olup olmadığından emin olmak için katalog denetlenir.
1. **Kiracı veritabanına New-TenantDatabase öğesi sağlanır.** **F11** tuşunu kullanarak girin ve bir Resource Manager şablonu kullanılarak veritabanının nasıl sağlandığını görün.
    
Veritabanı adı, hangi parçanın hangi kiracıya ait olduğunu netleştirmek üzere kiracı adından oluşturulur. (Diğer veritabanı adlandırma stratejileri kolayca kullanılabilir.)

Katalog sunucusunda **bir *altın* veritabanı kopyalayarak kiracı veritabanı oluşturmak** (baseTenantDB) için Resource Manager şablonu kullanılır.  Bir diğer yöntem ise boş bir veritabanı oluşturmak ve bir bacpac öğesini içeri aktararak veritabanını başlatmaktır.

Resource Manager şablonu … \\Öğrenme Modülleri\\Ortak\\ klasöründe bulunur: *tenantdatabasecopytemplate.json*

Kiracı veritabanı oluşturulduktan sonra, mekan (kiracı) adı ve mekan türü ile başlatılır. Burada, diğer başlatma işlemleri de yapılabilir.

Kiracı veritabanı, kiracı anahtarı kullanılarak *Add-TenantDatabaseToCatalog* öğesi ile kataloğa kaydedilir. Ayrıntıları görmek için **F11** tuşunu kullanın:

* Katalog veritabanı, parça eşlemesine eklenir (bilinen veritabanları listesi).
* Anahtar değerini parçaya bağlayan eşleme oluşturulur.
* Kiracıyla ilgili ek meta veriler (mekan adı) eklenir.

Sağlama tamamlandıktan sonra, yürütme işlemi orijinal *Demo-ProvisionAndCatalog* betiğine geri döner ve yeni kiracının **etkinlikler** sayfası tarayıcıda açılır:

   ![etkinlikler](media/sql-database-saas-tutorial-provision-and-catalog/new-tenant2.png)


## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama düzenleri şunlardır:

**Veritabanlarını önceden hazırlama.** Bu düzen, bir elastik havuzdaki veritabanlarının ek maliyet çıkarmamasından (faturalandırma veritabanları için değil, elastik havuz için yapılır) ve boştaki veritabanlarının hiçbir kaynak kullanmamasından yararlanır. Bir havuzdaki veritabanlarını önceden sağlayıp, daha sonra gerektiğinde ayırarak kiracı ekleme süresini önemli ölçüde azaltabilirsiniz. Önceden sağlanan veritabanı sayısı, öngörülen sağlama hızı için uygun olan bir arabellek belirlemek üzere gereken şekilde ayarlanabilir.

**Otomatik sağlama.** Bu düzende, istendiği takdirde elastik havuzlardaki veritabanlarını önceden sağlamak dahil olmak üzere, gerektiğinde sunucu, havuz ve veritabanlarını sağlamak için özel bir sağlama hizmeti kullanılır. Veritabanları kullanımdan çıkarılır ve silinirse, elastik havuzlardaki boşluklar sağlama hizmeti tarafından istenen şekilde doldurulabilir. Birden fazla coğrafyada sağlama işlemini gerçekleştirme gibi bir hizmet basit ya da karmaşık olabilir ve DR için coğrafi çoğaltma kullanılıyorsa bu stratejiyi otomatik olarak ayarlayabilir. Otomatik sağlama düzeni ile bir istemci uygulaması veya betik, bir kuyruğa sağlama hizmeti tarafından işlenecek bir sağlama isteği gönderir ve sonra işlemin tamamlandığını belirlemek üzere hizmeti sorgular. Önceden sağlama kullanılırsa, istekler arka planda çalışan bir yedek veritabanının sağlanmasını yöneten hizmet ile hızla işlenebilir.


## <a name="stopping-wingtip-saas-application-related-billing"></a>Wingtip SaaS uygulaması ile ilgili faturalandırmayı durdurma

Başka bir öğreticiye geçmeyi planlamıyorsanız, faturalandırmayı askıya almak için tüm kaynakları silmeniz önerilir. WTP uygulamasının dağıtıldığı kaynak grubunu sildiğinizde tüm kaynakları silinir.

* Portalda uygulamanın kaynak grubuna gidin ve bu WTP dağıtımıyla ilgili tüm faturalandırma işlemlerini durdurmak için uygulamayı silin.

## <a name="tips"></a>İpuçları

* EDCL, istemci uygulamalarının kataloğa bağlanıp kataloğu değiştirmesini sağlayan önemli özellikler de sunar. EDCL’yi kullanarak belirli bir anahtar değeri için ADO.NET bağlantısı alabilir, uygulamanın doğru veritabanına bağlanmasını sağlayabilirsiniz. İstemci, katalog veritabanına giden trafiği en aza indirmek ve uygulamayı hızlandırmak amacıyla bu bağlantı bilgilerini önbelleğe alır.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Yeni kiracı sağlama ve bu kiracıları kataloğa kaydetme ile ilgili ayrıntılara gidin.

[Performans izleme öğreticisi](sql-database-saas-tutorial-performance-monitoring.md)

## <a name="additional-resources"></a>Ek Kaynaklar

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Elastik veritabanı istemci kitaplığı](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library)
* [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)

