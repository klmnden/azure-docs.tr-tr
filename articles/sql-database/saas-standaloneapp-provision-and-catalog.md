---
title: "Çok kiracılı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs"
description: "Tek başına uygulama desenini kullanarak sağlama ve Katalog yeni kiracılar"
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: SaaS
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2018
ms.author: billgib
ms.openlocfilehash: eec7f9262dd8e8cccb5ba68cbe2f12581cd01470
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="provision-and-catalog-new-tenants-using-the--application-per-tenant-saas-pattern"></a>Sağlama ve Kiracı SaaS deseni başına uygulamayı kullanarak yeni kiracılar katalog

Bu makalede, sağlama ve Kiracı SaaS deseni başına tek başına uygulamayı kullanarak yeni kiracıların katalog kapsar.
Bu makalede iki ana bölümden oluşur:
* Sağlama ve yeni kiracılar katalog kavramsal tartışma
* Sağlama ve Katalog gerçekleştirir örnek PowerShell kodu vurgular bir öğretici
    * Öğretici Kiracı deseni başına tek başına App uyarlanan Wingtip biletleri örnek SaaS uygulaması kullanır.

## <a name="application-per-tenant-pattern"></a>Kiracı deseni başına uygulama
Kiracı deseni başına tek başına app çok kiracılı SaaS uygulamaları için birkaç modelinden biridir.  Bu modelinde, her bir kiracı için bir tek başına uygulama sağlanır. Uygulama, uygulama düzeyinde bileşenleri ve bir SQL veritabanı oluşur.  Her Kiracı uygulama satıcısının abonelikte dağıtılabilir.  Alternatif olarak, Azure'un sunduğu bir [yönetilen uygulamaların program](https://docs.microsoft.com/en-us/azure/managed-applications/overview) , bir uygulama bir kiracının abonelikte dağıtılabilir ve kiracının adınıza satıcı tarafından yönetilen içinde. 

   ![Kiracı başına uygulama düzeni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern.png)

Bir kiracı için bir uygulama dağıtımı sırasında uygulama ve veritabanı Kiracı için oluşturulan yeni bir kaynak grubu içinde sağlanır.  Ayrı kaynak gruplarını kullanma, her bir kiracının uygulama kaynakları ayırır ve bunları bağımsız olarak yönetilmesini sağlar. Her kaynak grubu her uygulama örneği ilgili veritabanını doğrudan erişmek için yapılandırılır.  Uygulama ve veritabanı arasındaki Aracısı bağlantıları kataloğa kullanan diğer desenlerle Bu bağlantı modeli karşılaştırır.  Ve kaynak paylaşımı gibi her bir kiracı veritabanı, yoğun yük işlemek için yeterli kaynaklara sahip sağlanması gerekir. Bu deseni daha az kiracılar, SaaS uygulamaları için kullanılması eğilimindedir söz konusu olduğunda Kiracı üzerinde güçlü Vurgu yalıtım ve kaynak maliyetleri indirimlere daha az.  

## <a name="using-a-tenant-catalog-with-the-application-per-tenant-pattern"></a>Kiracı deseni başına uygulama ile bir kiracı Kataloğu'nu kullanarak
Her bir kiracının uygulama ve veritabanı tamamen yalıtılmış olsa da, çeşitli yönetim ve analiz senaryoları kiracılar arasında çalışabilir.  Örneğin, uygulamanın yeni bir sürüm için bir şema değişikliği uygulamadan her Kiracı veritabanı şemasında yapılan değişiklikler gerektirir. Raporlama ve analiz senaryoları da bunlar dağıtıldığı bağımsız olarak tüm Kiracı veritabanlarına erişim gerektirebilir.

   ![Kiracı başına uygulama düzeni](media/saas-standaloneapp-provision-and-catalog/standalone-app-pattern-with-catalog.png)

Kiracı katalog Kiracı tanıtıcısı ve bir sunucu ve veritabanı adı çözümlenmesi bir tanımlayıcı sağlayan bir kiracı veritabanı arasında bir eşleme tutar.  Diğer düzenleri kullanılabilir olsa da Wingtip SaaS uygulamada, Kiracı tanımlayıcı bir kiracı adı karması hesaplanır.  Bağımsız uygulamalar bağlantıları yönetmek için kataloğu gerekmez, ancak katalog, Kiracı veritabanları kümesi için başka eylemler kapsamını belirlemek için kullanılabilir. Örneğin, esnek sorgu katalog arası Kiracı raporlama için hangi sorguları dağıtılan veritabanları belirlemek için kullanabilirsiniz.

## <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı
Wingtip örnek uygulama Kataloğu parça yönetim özelliklerine göre uygulanır [esnek veritabanı istemci Kitaplığı](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) (EDCL).  Kitaplık oluşturmak, yönetmek ve bir veritabanında depolanan bir parça eşlemesi kullanmak için bir uygulama sağlar. Katalog depolanır Wingtip biletleri örnek *Kiracı katalog* veritabanı.  Parça parça (veritabanı) için bir kiracı anahtarı bu kiracının veri depolanır eşler.  EDCL işlevlerini yönetmek bir *genel parça eşleme* tablolarında depolanır *Kiracı katalog* veritabanı ve *yerel parça eşleme* her parça depolanır.

Uygulamalarından ya da oluşturmak ve parça eşleme girdileri yönetmek için PowerShell betiklerini EDCL işlevler çağrılabilir. Diğer EDCL işlevleri parça kümesi almak veya verili doğru veritabanına bağlanmak için kullanılan Kiracı anahtarı. 
    
> [!IMPORTANT] 
> Katalog veritabanındaki verileri veya Kiracı veritabanları yerel parça eşlemesinde doğrudan düzenlemeyin. Doğrudan güncelleştirmeler veri bozulması nedeniyle yüksek riskli desteklenmiyor. Bunun yerine, yalnızca EDCL API'lerini kullanarak eşleme verilerini düzenleyin.

## <a name="tenant-provisioning"></a>Kiracı sağlama 
Her bir kiracı içinde kaynakları hazırlanmadan önce hangi oluşturulmalıdır yeni bir Azure kaynak grubu gerektirir. Kaynak grubu mevcut olduğunda, bir Azure kaynak yönetimi şablon uygulama bileşenlerini ve veritabanı dağıtmak ve veritabanı bağlantısını yapılandırmak için kullanılabilir. Veritabanı şeması başlatmak için şablon bir bacpac dosyasını içeri aktarabilirsiniz.  Alternatif olarak, veritabanı 'şablon' veritabanının bir kopyasını oluşturulabilir.  Veritabanı sonra daha fazla ilk salonundan verilerle güncelleştirilir ve kataloğa kayıtlı.

## <a name="tutorial"></a>Öğretici

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
* Bir kataloğu hazırlama
* Daha önce kataloğa dağıtılan Kiracı veritabanları kaydetme
* Bir ek Kiracı sağlamak ve kataloğa kaydetmek

Bir Azure Resource Manager şablonu dağıtma ve uygulama yapılandırmak, Kiracı veritabanı oluşturmak ve başlatmak için bir bacpac dosyasını içeri aktarmak için kullanılır. İçeri aktarma isteği işleme alınan önce birkaç dakika sıraya.

Bu öğreticinin sonunda ve kataloğa kayıtlı her veritabanı ile tek başına Kiracı uygulamaları kümesine sahiptir.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun: 
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)
* Üç örnek Kiracı uygulamaları dağıtılır. Beş dakikadan daha kısa bir süre içinde bu uygulamaları dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS tek başına uygulama düzeni keşfetme](https://docs.microsoft.com/en-us/azure/sql-database/saas-standaloneapp-get-started-deploy).

## <a name="provision-the-catalog"></a>Kataloğu hazırlama
Bu görevde, tüm Kiracı veritabanları kaydetmek için kullanılan katalog hazırlamayı öğrenin. Yapacaklarınız: 
* **Katalog veritabanı sağlama** bir Azure kaynak yönetimi şablonu kullanarak. Veritabanı bir bacpac dosyasını içeri aktararak başlatılır.  
* **Örnek Kiracı uygulamaları kaydetmek** daha önce Dağıtılmış.  Her bir kiracı karmasını Kiracı adı oluşturulan bir anahtar kullanarak kayıtlı değil.  Kiracı adı da katalog uzantısı tablosunda depolanır.

1. PowerShell ISE'yi açın *...\Learning Modules\UserConfig.psm* ve güncelleştirme  **\<kullanıcı\>**  değerini üç örnek uygulamaları dağıtırken kullandığınız değerle.  **Dosyayı kaydedin**.  
1. PowerShell ISE'yi açın *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ve **$Scenario = 1**. Kiracı katalog dağıtmak ve önceden tanımlanmış kiracılar kaydedin.

1. İmlecinizi herhangi bir yere şunu satırda koyarak kesme noktası ekleme `& $PSScriptRoot\New-Catalog.ps1`ve tuşuna basarak **F9**.

    ![İzleme için bir kesme noktası ayarlama](media/saas-standaloneapp-provision-and-catalog/breakpoint.png)

1. Tuşuna basarak komut dosyasını çalıştırmak **F5**. 
1.  Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** yeni Catalog.ps1 betiğe adıma.
1.  Betiğin yürütmeyi F10 ve F11 hata ayıklama menü seçeneklerini kullanarak, üzerinde veya içine adım için işlevleri çağrılır.
    *   PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

Komut dosyası tamamlandıktan sonra katalog mevcut olur ve tüm örnek kiracılar kaydedilir. 

Şimdi, oluşturduğunuz kaynaklara bakın.

1. Açık [Azure portal](https://portal.azure.com/) ve kaynak gruplarını göz atın.  Açık **wingtip-sa-katalog -\<kullanıcı\>**  kaynak grubu ve katalog sunucusu ve veritabanı not edin.
1. Veritabanını seçin ve portal içinde açmak *Veri Gezgini* sol taraftaki menüden.  Oturum açma komutuna tıklayın ve sonra parolayı girin =  **P@ssword1** .


1. Şeması keşfedin *tenantcatalog* veritabanı.  
   * İçindeki nesneler `__ShardManagement` şema tüm sağlanır esnek veritabanı istemci kitaplığı tarafından.
   * `Tenants` Tablo ve `TenantsExtended` görünüm olan ek değeri sağlamak için katalog nasıl genişletebileceğini gösteren örnek eklenen uzantıları.
1. Sorguyu çalıştırmak `SELECT * FROM dbo.TenantsExtended`.          

   ![Veri Gezgini](media/saas-standaloneapp-provision-and-catalog/data-explorer-tenantsextended.png)

    Veri Gezgini'ni kullanarak alternatif olarak SQL Server Management Studio'dan veritabanına bağlanabilir. Bunu yapmak için sunucu wingtip - Bağlan 

    
    Not - katalog verileri doğrudan düzenlemelisiniz değil her zaman kullanın. Parça Yönetimi API'leri. 

## <a name="provision-a-new-tenant-application"></a>Yeni bir kiracı uygulama sağlama

Bu görevde, tek bir kiracı uygulama sağlama öğrenin. Yapacaklarınız:  
* **Yeni bir kaynak grubu oluşturma** Kiracı. 
* **Uygulama ve veritabanı sağlama** bir Azure kaynak yönetimi şablonunu kullanarak yeni kaynak grubuna.  Bu eylem, ortak bir şema ve başvuru verileri veritabanıyla bir bacpac dosyasını içeri aktararak başlatma içerir. 
* **Temel Kiracı bilgilerle veritabanını başlatılamadı**. Bu eylem, kendi olaylarını web sitesindeki arka planı olarak kullanılan fotoğraf belirler salonundan türünü belirtme içerir. 
* **Katalog veritabanına veritabanını kaydetmeniz**. 

1. PowerShell ISE'yi açın *...\Learning Modules\ProvisionTenants\Demo-ProvisionAndCatalog.ps1* ve **$Scenario = 2**. Kiracı katalog dağıtmak ve önceden tanımlanmış kiracılar kaydetme

1. İmlecinizi herhangi bir yere yazan satırda 49 koyarak komut dosyasında kesme noktası ekleme `& $PSScriptRoot\New-TenantApp.ps1`ve tuşuna basarak **F9**.
1. Tuşuna basarak komut dosyasını çalıştırmak **F5**. 
1.  Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** yeni Catalog.ps1 betiğe adıma.
1.  Betiğin yürütmeyi F10 ve F11 hata ayıklama menü seçeneklerini kullanarak, üzerinde veya içine adım için işlevleri çağrılır.

Kiracı sağlandıktan sonra yeni kiracının olayları Web sitesi açılır.

   ![kırmızı yarış Akçaağaç](media/saas-standaloneapp-provision-and-catalog/redmapleracing.png)

Ardından Azure portalında oluşturulan yeni kaynaklar inceleyebilirsiniz. 

   ![kırmızı Akçaağaç yarış kaynakları](media/saas-standaloneapp-provision-and-catalog/redmapleracing-resources.png)


## <a name="to-stop-billing-delete-resource-groups"></a>Faturalama durdurmak için kaynak gruplarını Sil ##

Örnek araştırma tamamladığınızda, ilişkili faturalama durdurmak için oluşturulan tüm kaynak grupları silin.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS veritabanı uygulamaları hakkında daha fazla bilgi için bkz: [tasarım desenleri çok kiracılı SaaS uygulamaları için](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> * Wingtip biletleri SaaS tek başına uygulamayı dağıtmak nasıl.
> * Sunucular ve veritabanları hakkında uygulaması olun.
> * Nasıl ilgili faturalama durdurmak için örnek kaynaklar silinir.
