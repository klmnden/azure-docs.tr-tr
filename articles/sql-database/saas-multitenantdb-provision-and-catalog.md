---
title: "Sağlamak ve yeni bir çok kiracılı Azure SQL veritabanı SQL veritabanı kullanarak bir SaaS uygulaması kiracılar katalog | Microsoft Docs"
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
ms.date: 11/17/2017
ms.author: billgib
ms.openlocfilehash: f6707b85cc80178da663d7e2b95eeb5c9550789c
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-multi-tenant-sql-database"></a>Çok kiracılı SQL veritabanı kullanarak bir SaaS uygulaması sağlama ve Katalog yeni kiracılar

Bu öğreticide, sağlama ve kiracılar parçalı çok Kiracı veritabanı modeliyle çalışırken katalog için desenleri hakkında bilgi edinin. 

Çok kiracılı şema verileri tek bir veritabanında depolanması için birden çok kiracıdan sağlar. Çok sayıda kiracıları desteklemek için Kiracı verilerini birden çok parça veya veritabanları arasında dağıtılır. Herhangi bir kiracı için verileri tam olarak her zaman tek bir veritabanında yer alır.  Bir katalog veritabanlarına kiracılar eşleme tutmak için kullanılır.   

Bazı veritabanları yalnızca tek bir kiracı ile doldurmak seçebilirsiniz. Birden çok kiracıya tutun veritabanlarını Kiracı yalıtımı ödün verme pahasına Kiracı başına daha düşük maliyetli bir ayrıcalık tanıma.  Yalnızca tek bir kiracı tutun veritabanları yalıtım maliyet ayrıcalık tanıma.  Birden çok kiracılar ve tek kiracılar veritabanlarıyla maliyet veya her bir kiracı yalıtımını iyileştirmek için tek bir SaaS uygulaması karışabilir. Kiracılar, sağlanan ya da kendi veritabanına daha sonra taşınabilir kendi veritabanı verilebilir.

   ![Kiracı Kataloğu ile parçalı çok Kiracı veritabanı uygulama](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)


## <a name="tenant-catalog-pattern"></a>Kiracı katalog düzeni

Kiracı verilerini birden çok veritabanı arasında dağıtılırsa, her bir kiracının verilerinin depolandığı bilmeniz önemlidir. Katalog veritabanına bir kiracı ve verilerini depolayan veritabanı arasındaki eşlemeyi tutar.

Her bir kiracı anahtarı Kataloğu tarafından tanımlanır. Wingtip biletleri SaaS uygulamalarında karmasını kiracının ad Kiracı anahtarı oluşturulur. Bu uygulamanın bir web sayfası URL'den Kiracı adı ayıklamak ve uygun veritabanına bağlanmak için bunu kullanın sağlar. Diğer Kiracı anahtar düzenlerini de kullanılabilir.

Bir katalog kullanan uygulama kesintiye uğratmadan sağladıktan sonra değiştirilecek Kiracı veritabanı konumunu veya adını olanak tanır.  Çok Kiracı veritabanı modelinde, bu Kiracı veritabanları arasında taşıma düzenler.  Katalog, bir uygulama için bir kiracı Bakım veya başka eylemler için çevrimdışı olup olmadığını belirtmek için de kullanılabilir.

Katalog ek Kiracı veya veritabanı gibi meta verilerinde katmanı sürümünü veya bir veritabanı, veritabanı şeması sürümü, Kiracı adı ve hizmet planı veya SLA ve diğer bilgileri uygulama yönetimi, müşteri desteği veya devops etkinleştirmek için depolamak için genişletilmiş işler.  

Katalog arası Kiracı raporlama, şema yönetimini etkinleştirmek için de kullanılabilir ve verileri analiz amaçlı ayıklayın. 

### <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı 
Wingtip biletleri SaaS uygulamalarında katalog uygulanan *tenantcatalog* parça yönetim özelliklerini kullanarak veritabanı [esnek veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). Bir uygulama oluşturmak, yönetmek ve veritabanı destekli kullanmak kitaplık etkinleştirir 'parça eşleme'. Bir parça eşleme parça (veritabanları) ve anahtarları (kiracılar) ile parça arasında eşleme listesini içerir.  EDCL işlevleri parça eşlemesinde girişleri oluşturmak için sağlama ve daha sonra doğru veritabanına bağlanmak için Kiracı sırasında uygulamaları veya PowerShell betikleri kullanılabilir. Kitaplık Kataloğu veritabanında trafiğini en aza indirmek ve bağlantı hızlandırmak için bağlantı bilgilerini önbelleğe alır. 

> [!IMPORTANT]
> Eşleme verilerine katalog veritabanından erişilebilir, ancak *verileri düzenlemeyin*! Eşleme verilerini yalnızca Elastik Veritabanı İstemci Kitaplığı API’sini kullanarak düzenleyin. Eşleme verilerinin doğrudan değiştirilmesi, kataloğu bozabilir ve desteklenmez.


## <a name="tenant-provisioning-pattern"></a>Kiracı sağlama düzeni

Paylaşılan bir veritabanına sağlanan veya kendi veritabanı verilen Kiracı ise, bu yeni bir kiracı parçalı çok Kiracı veritabanı modelinde sağlanırken, ilk belirlenmelidir. Paylaşılan bir veritabanı varsa, varolan bir veritabanında alanı ise veya yeni bir veritabanı gerekli belirlenmesi gerekir. Yeni bir veritabanı gerekiyorsa, uygun bir konum ve hizmet katmanı ile uygun şema ve başvuru verileri başlatılmadı ve kataloğa kayıtlı sağlanmalıdır. Son olarak, Kiracı eşlemesini uygun parça başvurmak için eklenebilir.

Veritabanı sağlama SQL komut dosyaları yürütme bir bacpac dağıtma veya bir şablon veritabanı kopyalama elde edilebilir. Wingtip biletleri SaaS uygulamaları yeni Kiracı veritabanı oluşturmak için bir şablon veritabanı kopyalayın.

Yeni veritabanları ile en son şema sağlandığından emin olmak için gereken genel şema yönetimi stratejisi içinde yaklaşımı sağlama veritabanı comprehended gerekir.  Bu incelediniz daha ayrıntılı olarak [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md).  

Bu öğreticide Kiracı sağlama komut bir kiracı var olan bir çok kiracılı veritabanına sağlama hem yeni bir kiracı veritabanı oluşturmak içerir. Kiracı veri sonra başlatıldı ve Katalog parça eşlemesinde kayıtlı. Örnek uygulamayı tek bir kiracı içeren veritabanları verilen Kiracı adına göre adlardır. Birden çok kiracıya içeren veritabanları genel verilen _tenantsN_ yalnızca bir tek-Kiracı veritabanlarıyla kiracının adı verilen sırada adı. Katalog veritabanına atanmış herhangi bir ad kullanılmasına gibi örnekte kullanılan belirli adlandırma kuralları düzeni önemli bir parçası değildir.  

## <a name="provision-and-catalog-tutorial"></a>Sağlama ve kataloğu Öğreticisi

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]

> * Çok kiracılı veritabanına bir kiracı sağlama
> * Bir kiracı tek Kiracı veritabanına sağlama
> * Çok kiracılı ve tek Kiracı veritabanlarına kiracılar toplu sağlama
> * Bir veritabanı ve bir katalogda eşleme Kiracı kaydetme

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama keşfedin.](saas-multitenantdb-get-started-deploy.md)
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="get-the-wingtip-tickets-management-scripts"></a>Wingtip biletleri yönetim komut dosyaları alma

Yönetim komut dosyaları ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultiTenantDB](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub depo. <!--See [Steps to download the Wingtip SaaS scripts](saas-tenancy-wingtip-app-guidance-tips.md#download-and-unblock-the-wingtip-saas-scripts).-->


## <a name="provision-tenant-walkthrough-1"></a>Sağlama Kiracı gözden geçirme #1

Yeni Kiracı ile çok Kiracı veritabanı sağlama Wingtip biletleri uygulama nasıl uyguladığını anlamak için bir kesme noktası ve iş akışı aracılığıyla adım paylaşılan veritabanında bir kiracı ile diğer kiracılar sağlarken ekleyin:

1. İçinde _PowerShell ISE_açın... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ ve aşağıdaki parametreleri ayarlayabilirsiniz:
   * **$TenantName** = **Bushwillow mavi**, yeni bir salonundan adı.
   * **$VenueType** = **mavi**, önceden tanımlanmış salonundan türlerinden birini: Mavi, classicalmusic, dance, jazz, judo, motorracing, çok amaçlı, opera, rockmusic, futbol (küçük harf, boşluk yok).
   * **$DemoScenario** = **1**, *paylaşılan veritabanında bir kiracı ile diğer kiracılar sağlamak*.

1. İmlecinizi herhangi bir yere satırı 38, yazan satırı koyarak kesme noktası ekleme: *yeni Kiracı '*ve basın **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

1. Komut dosyası tuşuna çalıştırmak için **F5**.

1. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.

   ![Hata ayıklama](media/saas-multitenantdb-provision-and-catalog/debug.png)

Betiğin yürütmeyi kullanarak **hata ayıklama** menü seçeneklerini - **F10** ve **F11** adıma üzerinden veya çağrılan işlev. PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).


İş akışı, aracılığıyla betik izleme sırasında adım temel öğeleri şunlardır:

* **Yeni kiracı anahtarını hesaplayın**. Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
* **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**. Katalog anahtar zaten kaydedilmedi emin olmak için denetlenir.
* **Varsayılan Kiracı veritabanındaki Kiracı başlatma**. Kiracı veritabanı yeni Kiracı bilgiler eklenerek güncelleştirildi.  
* **Kiracı kataloğa kaydetmek** yeni Kiracı anahtarını ve varolan tenants1 veritabanı arasında eşleme Kataloğu'na eklenir. 
* **Kiracı adı kataloğa eklediğiniz**. Salonundan ad katalog kiracılar tablosuna eklenir.  Bu ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterir.
* **Yeni Kiracı için açık olayları sayfası**. *Bushwillow mavi* Etkinlikler sayfasını tarayıcıda açılır:

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)


## <a name="provision-tenant-walkthrough-2"></a>Sağlama Kiracı gözden geçirme #2

Şimdi gözden geçirme işlemi bir kiracı kendi veritabanında oluştururken:

1. Hala... \\Öğrenme modülleri\\ProvisionAndCatalog\\_Demo ProvisionAndCatalog.ps1_ aşağıdaki parametreleri ayarlayın:
   * **$TenantName** = **sequoia futbol**, yeni bir salonundan adı.
   * **$VenueType** = **futbol**, önceden tanımlanmış salonundan türlerinden birini: Mavi, classicalmusic, dance, jazz, judo, motorracing, çok amaçlı, opera, rockmusic, futbol (küçük harf, boşluk yok).
   * **$DemoScenario** = **2**, *paylaşılan veritabanında bir kiracı ile diğer kiracılar sağlamak*.

1. Yeni bir kesme noktası imlecinizi herhangi bir yere satırı 57, yazan satırı koyarak ekleyin:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*ve basın **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

1. Betiği çalıştırmak için **F5**'e basın.

1. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.  Kullanım **F10** ve **F11** üzerinden adım ve yürütmeyi işlevleri adım.

İş akışı, aracılığıyla betik izleme sırasında adım temel öğeleri şunlardır:

* **Yeni kiracı anahtarını hesaplayın**. Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
* **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**. Katalog anahtar zaten kaydedilmedi emin olmak için denetlenir.
* **Yeni bir kiracı veritabanı oluşturmak**. Veritabanı kopyalama tarafından oluşturulan *basetenantdb* Resource Manager şablonu kullanarak veritabanı.  Veritabanı adı kiracının adını temel alır.
* **Veritabanı kataloğa eklemek**. Kiracı veritabanı, bir parça kataloğunda olarak kaydedilir.
* **Varsayılan Kiracı veritabanındaki Kiracı başlatma**. Kiracı veritabanı yeni Kiracı bilgiler eklenerek güncelleştirildi.  
* **Kiracı kataloğa kaydetmek** yeni Kiracı anahtarına arasında eşleme ve *sequoiasoccer* veritabanı Kataloğu'na eklenir.
* **Kiracı adı kataloğa eklediğiniz**. Salonundan ad katalog kiracılar tablosuna eklenir.
* **Yeni Kiracı için açık olayları sayfası**. *Sequoia futbol* Etkinlikler sayfasını tarayıcıda açılır:

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)


## <a name="provision-a-batch-of-tenants"></a>Kiracılar toplu sağlama

Bu alıştırmada, hızlı bir şekilde 17 kiracılar toplu sağlar. Bu yüzden çalışmak için birden fazla birkaç veritabanlarını Wingtip biletleri öğreticilere başlatmadan önce bu toplu kiracıların sağlamak önerilir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionAndCatalog\\*Demo ProvisionAndCatalog.ps1* değiştirip *$DemoScenario* parametre 3:
   * **$DemoScenario** = **3**, *paylaşılan bir veritabanına kiracılar toplu sağlama*.
1. **F5** tuşuna basıp betiği çalıştırın.


### <a name="verify-the-deployed-set-of-tenants"></a>Kiracılar dağıtılan kümesini doğrulayın 
Bu aşamada, paylaşılan bir veritabanına dağıtılan kiracılar ve kiracıların kendi veritabanlarına dağıtılan bir karışımını vardır. Azure portalında oluşturulan veritabanlarını incelemek için kullanılabilir:  

* İçinde [Azure portal](https://portal.azure.com), açık **tenants1-mt -\<kullanıcı\>**  SQL sunucuları listesine giderek sunucu.  **SQL veritabanları** listede paylaşılan yer **tenants1** veritabanı ve veritabanları kendi veritabanında olan kiracılar için:

   ![veritabanı listesi öğesine tıklayın](media/saas-multitenantdb-provision-and-catalog/databases.png)

Azure portalı Kiracı veritabanları gösterilmektedir, ancak bu, kiracılar içinde görelim paylaşılan veritabanı. Kiracılar tam listesini Wingtip biletleri olayları hub sayfasında görülebilir.   

* Olay hub'ı sayfasını tarayıcıda açın (http:events.wingtip-mt.\<kullanıcı\>. trafficmanager.net)  

Kiracılar ve bunların karşılık gelen veritabanı tam listesini katalogda kullanılabilir. Veritabanı adı parça yönetim tablolardaki kiracılar tabloya depolanan Kiracı adı birleştirir tenantcatalog veritabanında bir SQL görünümü sağlanmaktadır. Bu görünüm sorunsuz şekilde kataloğunda depolanan meta verileri genişletme değerini gösterir.

* İçinde *SQL Server Management Studio (SSMS)* kiracılar sunucusuna bağlanma **tenants1 yüksekliğindeki\<kullanıcı\>. database.windows.net**, oturum: **Geliştirici** , Parola:**P@ssword1**

    ![SSMS bağlantı iletişim kutusu](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

* İçinde *Object Explorer*, Gözat görünümleri için *tenantcatalog* veritabanı.
* Sağ tıklayın *ExtendedTenants* ve **ilk 1000 satırı Seç** ve Kiracı adı ve veritabanı arasındaki eşlemeyi farklı kiracılar için not edin.

    ![SSMS ExtendedTenants görünümünde](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu öğreticide yer almayan diğer sağlama düzenleri şunlardır:

**Esnek havuzlar veritabanlarında önceden sağlama.** Önceden hazırlama düzeni ek bir maliyeti esnek havuzdaki veritabanları eklemeyin olgu yararlanan. Esnek havuz için veritabanlarını değil, faturalama olduğu ve boşta veritabanları hiçbir kaynaklarını tüketebilir. Bir havuzdaki veritabanları önceden sağlama ve bunları gerektiğinde ayırma kendi veritabanına yerleşik kiracılar için harcanan süre önemli ölçüde azaltılabilir. Önceden sağlanan veritabanı sayısı, öngörülen sağlama hızı için uygun olan bir arabellek belirlemek üzere gereken şekilde ayarlanabilir.

**Otomatik sağlama.** Otomatik sağlama modelinde, sunucular, havuzları ve veritabanları – önceden sağlama veritabanları esnek havuzları, isterseniz de dahil olmak üzere gerektiğinde otomatik olarak sağlamak için ayrılmış bir sağlama hizmeti kullanılır. Veritabanları kullanımdan çıkarılır ve silinirse, elastik havuzlardaki boşluklar sağlama hizmeti tarafından istenen şekilde doldurulabilir. Bu tür bir hizmet Örneğin, birden çok farklı coğrafyalara, sağlama işleme basit veya karmaşık – olabilir ve olağanüstü durum kurtarma için coğrafi çoğaltma ayarlayabilirsiniz. Otomatik sağlama desen ile bir istemci uygulama veya betik sağlama hizmeti tarafından işlenmesi için bir sağlama isteği kuyruğuna göndermek ve tamamlanma belirlemek için yoklaması. Önceden hazırlama kullanılırsa, istekleri hızlı bir şekilde, arka planda çalışan değiştirme veritabanı sağlama yönetme hizmeti ile ele alınması.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunları öğrendiniz:

> [!div class="checklist"]

> * Tek bir yeni kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracılar sağlama ve kataloğa kaydetme ayrıntılarını girme

Deneyin [performans izleme Öğreticisi](saas-multitenantdb-performance-monitoring.md).

## <a name="additional-resources"></a>Ek kaynaklar

<!--* Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
* [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
* [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)
