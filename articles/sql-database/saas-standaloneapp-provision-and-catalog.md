---
title: Çok kiracılı SaaS Öğreticisi - Azure SQL veritabanı | Microsoft Docs
description: Tek başına uygulama desenini kullanarak sağlama ve kataloğa yeni Kiracı
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: 28deb9b7ba15744b9bd3d273d02db4398d2b2ef3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61484599"
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Uygulama başına Kiracı SaaS düzeni kullanarak sağlama ve kataloğa yeni Kiracı

Bu makale, sağlama ve Kiracı SaaS düzeni başına tek başına uygulamayı kullanarak yeni bir kiracı grubu kataloglama kapsar.
Bu makalede, iki ana bölümden oluşur:
* Sağlama ve yeni kiracılar kataloglama kavramsal tartışma
* Sağlama ve Katalog gerçekleştirir ve örnek PowerShell kodu vurgular bir öğretici
    * Öğretici Kiracı deseni başına tek başına uygulamaya göre uyarlanmış Wingtip bilet örnek SaaS uygulaması kullanır.

## <a name="standalone-application-per-tenant-pattern"></a>Kiracı deseni başına tek başına uygulama

Kiracı deseni başına tek başına uygulamayı çok kiracılı SaaS uygulamaları için birkaç modelinden biridir.  Bu düzende, her Kiracı için tek başına uygulama sağlanır. Uygulama, uygulama düzeyinde bileşenleri ve bir SQL veritabanı oluşur.  Her Kiracı uygulamanın satıcının abonelikte dağıtılabilir.  Alternatif olarak, Azure'un sunduğu bir [yönetilen uygulamaların program](https://docs.microsoft.com/azure/managed-applications/overview) , bir uygulama içinde bir kiracının aboneliğinin dağıtılabilir ve kiracının adınıza satıcı tarafından yönetilen içinde. 

   ![Kiracı başına uygulama düzeni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Bir kiracı için bir uygulama dağıtım yaparken, uygulama ve veritabanı için bir kiracı oluşturulduğunda yeni bir kaynak grubu sağlanır.  Ayrı kaynak gruplarını kullanma, her bir kiracının uygulama kaynakları yalıtan ve bunları birbirinden bağımsız olarak yönetilmesini sağlar. Her kaynak grubu içinde her uygulama örneği, ilgili veritabanını doğrudan erişmek için yapılandırılır.  Bu bağlantı modeli Kataloğu'nu uygulama ve veritabanı arasında aracı bağlantılar için diğer desenleri ile karşılaştırır.  Ve kaynak paylaşımı gibi her Kiracı veritabanı, yoğun yükünü işlemek için yeterli kaynak sağlanması gerekir. Bu düzen daha az kiracılar, SaaS uygulamaları için kullanılan eğilimindedir olduğunda güçlü bir vurguyu Kiracı yalıtımı ve kaynak maliyetleri indirimlere daha az.  

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Uygulama başına Kiracı düzeni ile bir kiracı kataloğunu kullanma

Her bir kiracının uygulama ve veritabanı tamamen yalıtılmış olsa da, çeşitli yönetim ve analiz senaryolarına kiracılar genelinde çalışabilir.  Örneğin, uygulamanın yeni bir yayın için bir şema değişikliği uygulayarak, her Kiracı veritabanının şemasında yapılan değişiklikler gerektirir. Raporlama ve analiz senaryoları da bunlar dağıtıldığı bağımsız olarak tüm Kiracı veritabanlarına erişim gerektirebilir.

   ![Kiracı başına uygulama düzeni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

Kiracı Kataloğu Kiracı tanımlayıcısı ve bir sunucu ve veritabanı adı çözülmesi için bir tanımlayıcı sağlayan bir kiracı veritabanı arasındaki eşlemeyi içerir.  Wingtip SaaS uygulamasında Kiracı tanımlayıcısı, diğer düzenleri kullanılabilir olsa da Kiracı adı, karma olarak hesaplanır.  Tek başına uygulamalar bağlantıları yönetmek için katalog gerekmez ancak katalog kapsamı Kiracı veritabanları kümesi için diğer eylemler için kullanılabilir. Örneğin, esnek sorgu, katalog kiracılar arası raporlama için sorgular, üzerinden dağıtılan veritabanları kümesini belirlemek için kullanabilirsiniz.

## <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı

Wingtip örnek uygulama Kataloğu parça yönetim özelliklerine göre uygulanır [elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) (EDCL).  Kitaplığı oluşturmak, yönetmek ve bir veritabanında depolanan bir parça eşlemesi kullanmak bir uygulama sağlar. Katalog depolanır Wingtip bilet örnek *Kiracı Kataloğu* veritabanı.  Parça parça (veritabanı) için bir kiracı anahtarı, kiracının verileri depolanır eşler.  EDCL işlevlerini yönetmek bir *genel parça eşleme* tablolarında depolanan *Kiracı Kataloğu* veritabanı ve *yerel parça eşlemesi* her parça içinde depolanmış.

EDCL İşlevler, uygulamaları veya PowerShell betikleri oluşturmak ve parça eşlemesi girişleri yönetmek için çağrılabilir. Diğer EDCL işlevleri parçalar kümesini almak veya belirtilen doğru veritabanına bağlanmak için kullanılan Kiracı anahtarı. 

> [!IMPORTANT]
> Katalog veritabanındaki verilerle veya Kiracı veritabanlarını yerel parça eşlemesinde doğrudan düzenlemeyin. Doğrudan güncelleştirmeleri yüksek veri bozulması riski nedeniyle desteklenmez. Bunun yerine, yalnızca EDCL API'lerini kullanarak eşleme verilerini düzenleyin.

## <a name="tenant-provisioning"></a>Kiracı sağlama 

Her Kiracı ve kaynaklar içinde sağlanabilir önce oluşturulması gereken yeni bir Azure kaynak grubu gerektirir. Kaynak grubu mevcut olduğunda, uygulama bileşenlerini ve veritabanınızı dağıtmak ve ardından veritabanı bağlantısını yapılandırmak için bir Azure kaynak yönetimi şablonu kullanılabilir. Veritabanı şeması'nı başlatmak için şablon bir bacpac dosyasını içeri aktarabilirsiniz.  Alternatif olarak, veritabanı, 'şablon' veritabanının bir kopyasını oluşturulabilir.  Veritabanı sonra daha fazla ilk mekan verilerle güncelleştirilir ve kataloğa kayıtlı.

## <a name="tutorial"></a>Öğretici

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

* Bir kataloğu hazırlama
* Dağıttığınız örnek Kiracı veritabanları daha önce kataloğa kaydetme
* Bir ek Kiracı sağlama ve kataloğa kaydetme

Bir Azure Resource Manager şablonu dağıtma ve uygulama yapılandırma, Kiracı veritabanı oluşturmak ve sonra başlatmak üzere bir bacpac dosyasını içeri aktarmak için kullanılır. İçeri aktarma isteği kuyruğa actioned önce birkaç dakika.

Bu öğreticinin sonunda, kataloğa her bir veritabanı ile tek başına Kiracı uygulamalar kümesine sahiptir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun: 

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Üç örnek Kiracı uygulamalar dağıtılır. Beş dakikadan kısa bir süre içinde bu uygulamaları dağıtmak için bkz: [Dağıt ve Wingtip bilet SaaS tek başına uygulama desenini keşfedin](saas-standaloneapp-get-started-deploy.md).

## <a name="provision-the-catalog"></a>Kataloğu hazırlama

Bu görevde, tüm Kiracı veritabanlarına kaydetmek için kullanılan Kataloğu hazırlama hakkında bilgi edinin. Yapacaklarınız: 

* **Katalog veritabanı sağlama** bir Azure kaynak yönetimi şablonu ile. Veritabanını bacpac dosyasını içeri aktararak başlatılır.  
* **Örnek Kiracı uygulamaları kaydetme** daha önce dağıttığınız.  Her bir kiracı oluşturulur karmasını Kiracı adı bir anahtar kullanılarak kaydedilir.  Kiracı adı, ayrıca kataloğunda bir uzantı tablosunda depolanır.

1. PowerShell ISE'de açın *...\Learning Modules\UserConfig.psm* ve güncelleştirme **\<kullanıcı\>** üç örnek uygulamaları dağıtırken kullanılan değer için değer.  **Dosyayı kaydedin**.  
1. PowerShell ISE'de açın *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ayarlayıp **$Scenario = 1**. Kiracı Kataloğu dağıtma ve önceden tanımlanmış kiracılar kaydedin.

1. İmlecinizi her yerde yazan satıra koyarak, bir kesme noktası ekleyin `& $PSScriptRoot\New-Catalog.ps1`ve tuşuna **F9**.

    ![İzleme için bir kesme noktası ayarlama](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. Tuşuna basarak betiği çalıştırın **F5**. 
1.  Betik yürütme kesme noktasında durduktan sonra basın **F11** için New-Catalog.ps1 kodun içine Adımlama.
1.  Çağrılan işlevler veya üzerinden adımlamak için F10 ve F11, hata ayıklama menü seçeneklerini kullanarak betiğin yürütülmesini izleme.
    *   PowerShell betikleri hata ayıklama hakkında daha fazla bilgi için bkz. [çalışmaya ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

Betik tamamlandıktan sonra katalog bulunur ve tüm örnek Kiracı kaydedilir. 

Artık oluşturduğunuz kaynakları arayın.

1. Açık [Azure portalında](https://portal.azure.com/) ve kaynak gruplarını bulun.  Açık **wingtip-sa-catalog -\<kullanıcı\>**  kaynak grubunda ve katalog sunucusu ve veritabanı not edin.
1. Veritabanını seçin ve portal içinde açın *Veri Gezgini* sol taraftaki menüden.  Oturum açma komutunu tıklatın ve parolayı girin = **P\@ssword1**.


1. Şemasını keşfetme *tenantcatalog* veritabanı.  
   * Nesneleri `__ShardManagement` şema tüm sağlanır elastik veritabanı istemci kitaplığı tarafından.
   * `Tenants` Tablo ve `TenantsExtended` görünümü olan örnekte eklenen uzantılar, ek değer sağlamak için katalog nasıl genişletebileceğiniz gösterilmektedir.
1. Sorguyu çalıştırma `SELECT * FROM dbo.TenantsExtended`.          

   ![Veri Gezgini](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Veri Gezgini'ni kullanarak alternatif olarak SQL Server Management Studio'dan veritabanına bağlanabilirsiniz. Bunu yapmak için sunucu wingtip - için bağlanma 

    
    Not - katalog verileri doğrudan düzenleyebilirsiniz değil, her zaman kullanın parça yönetim API'leri. 

## <a name="provision-a-new-tenant-application"></a>Yeni bir kiracı uygulaması sağlayın

Bu görevde, tek kiracılı bir uygulama sağlama hakkında bilgi edinin. Yapacaklarınız:  

* **Yeni bir kaynak grubu oluşturma** Kiracı. 
* **Uygulama ve veritabanı sağlama** bir Azure kaynak yönetimi şablonu ile yeni kaynak grubuna.  Bu eylem, bir bacpac dosyasını içeri aktararak veritabanını ortak şema ve başvuru verileri başlatma içerir. 
* **Temel Kiracı bilgileri veritabanıyla başlatmak**. Bu eylem, kendi olaylarını web sitesindeki bir arka plan olarak kullanılan fotoğraf belirleyen mekan türü belirtme içerir. 
* **Katalog veritabanındaki veritabanı kaydettirilmeye**. 

1. PowerShell ISE'de açın *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ayarlayıp **$Scenario = 2**. Kiracı Kataloğu dağıtma ve önceden tanımlanmış kiracılar kaydetme

1. İmlecinizi her yerde yazan satıra 49 yerleştirerek komut dosyasında bir kesme noktası ekleyin `& $PSScriptRoot\New-TenantApp.ps1`ve tuşuna **F9**.
1. Tuşuna basarak betiği çalıştırın **F5**. 
1.  Betik yürütme kesme noktasında durduktan sonra basın **F11** için New-Catalog.ps1 kodun içine Adımlama.
1.  Çağrılan işlevler veya üzerinden adımlamak için F10 ve F11, hata ayıklama menü seçeneklerini kullanarak betiğin yürütülmesini izleme.

Kiracı sağlandıktan sonra yeni kiracının olayları Web sitesi açılır.

   ![Red maple yarışı](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Ardından Azure portalında oluşturulan yeni kaynakları inceleyebilirsiniz. 

   ![Red maple yarış kaynakları](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Faturalandırmayı durdurmak için kaynak gruplarını silin.

Örneği keşfetme tamamladıktan sonra ilgili faturalandırmayı durdurmak için oluşturulan tüm kaynak gruplarını silin.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS veritabanı uygulamaları hakkında daha fazla bilgi için bkz: [çok kiracılı SaaS uygulamaları için Tasarım Düzenleri](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip bilet SaaS tek başına uygulama dağıtma
> * Sunucuları ve uygulamayı oluşturan veritabanları hakkında.
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme yapma.

Katalog Kiracı başına veritabanı sürümünü kullanarak çeşitli kiracılar arası senaryoları desteklemek için nasıl kullanıldığını keşfedebilirsiniz [Wingtip bilet SaaS uygulaması](saas-dbpertenant-wingtip-app-overview.md).  
