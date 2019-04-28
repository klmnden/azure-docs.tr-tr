---
title: Çok kiracılı SaaS Azure içindeki | Microsoft Docs
description: Sağlama ve kataloğa kaydetme Azure SQL veritabanı çok kiracılı SaaS uygulamasında yeni kiracılar hakkında bilgi edinin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib,andrela,stein
manager: craigg
ms.date: 09/24/2018
ms.openlocfilehash: d29baaad6090cea5eb31f5f50bba444cb3771155
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61486005"
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Parçalı bir çok kiracılı Azure SQL veritabanı kullanan bir SaaS uygulamasında yeni kiracılar sağlama ve kataloğa kaydetme

Bu makale sağlama ve içinde yeni kiracılar kataloglama kapsar bir *çok kiracılı parçalı veritabanlarını* modeli veya desen.

Bu makalede, iki ana bölümden oluşur:

- [Kavramsal tartışma](#goto_2_conceptual) sağlama ve Katalog yeni kiracılar.

- [Öğretici](#goto_1_tutorial) sağlama ve Katalog gerçekleştirir PowerShell komut dosyası kodu vurgulanır.
  - Öğreticide, çok kiracılı parçalı veritabanlarını düzene göre uyarlanmış Wingtip bilet SaaS uygulaması kullanılır.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Veritabanı deseni

Bu bölümde yanı sıra birkaç daha fazla, izleyin, çok kiracılı parçalı veritabanlarını deseninin kavramlar açıklanmaktadır.

Bu çok kiracılı parçalı modelinde, her bir veritabanı içinde tablo şemalarını bir kiracı anahtarı Kiracı verilerini depolayan tablonun birincil anahtarında içerir. Kiracı anahtarı, 0, 1 ya da çok sayıda Kiracı depolamak tek tek her veritabanı sağlar. Parçalı veritabanları kullanımını, çok büyük bir sayı kiracıları desteklemek uygulama sistemi kolaylaştırır. Herhangi bir kiracının tüm verileri bir veritabanında depolanır. Kiracılar çok sayıda çok parçalı veritabanları arasında dağıtılır. Katalog veritabanı eşleme her kiracının kendi veritabanına depolar.

#### <a name="isolation-versus-lower-cost"></a>Yalıtım daha düşük maliyet karşılaştırması

Tüm kendisi için bir veritabanı olan bir kiracı yalıtımı avantajlarından ölçeklenebilme. Kiracı, önceki bir tarihe etkisi diğer kiracılar tarafından kısıtlanmasını olmadan geri veritabanı olabilir. Veritabanı performansı, tek bir kiracı için diğer kiracılarla riske atmak zorunda kalmadan yeniden iyileştirme için ayarlanabilecek. Yalıtım bir veritabanı diğer kiracılarla paylaşmak için maliyetleri daha maliyetleri sorunudur.

Yeni bir kiracı sağlandığında bir veritabanı diğer kiracılar ile paylaşabilir veya kendi yeni bir veritabanı yerleştirilebilir. Daha sonra fikrinizi değiştirirseniz ve diğer durum veritabanını taşıma.

Maliyet veya yalıtım her Kiracı için en iyi duruma getirmek için aynı SaaS uygulamasının birden çok Kiracı ve Kiracı tek veritabanlarıyla karıştırılır.

   ![Kiracı Kataloğu ile parçalı çok kiracılı veritabanı uygulama](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Kiracı Kataloğu düzeni

Her Kiracı en az bir içeren iki veya daha fazla veritabanı varsa, uygulamanın hangi veritabanı geçerli ilgi Kiracı depolar bulmak için bir yol olmalıdır. Bu eşleme katalog veritabanına depolar.

#### <a name="tenant-key"></a>Kiracı anahtarı

Her Kiracı için Wingtip uygulamasının Kiracı anahtar benzersiz bir anahtar türetebilirsiniz. Uygulamanın Web sayfası URL'den Kiracı adını ayıklar. Uygulama adı anahtarı almak için karma hale getirir. Uygulama Kataloğu'na erişmek için anahtar kullanır. Katalog çapraz Kiracı depolandığı veritabanı hakkındaki bilgileri. Uygulamaya bağlanmak için veritabanı bilgileri kullanır. Diğer Kiracı anahtar düzenlerini de kullanılabilir.

Katalog kullanarak uygulama kesintiye uğratmadan sağladıktan sonra değiştirilmesi için bir kiracı veritabanı konumunu ve adını sağlar. Çok kiracılı veritabanı modelinde, bir kiracı veritabanları arasında taşıma Kataloğu sağlar.

#### <a name="tenant-metadata-beyond-location"></a>Kiracı meta verilerinde konumu dışında

Katalog de bir kiracı bakım ya da diğer eylemler için çevrimdışı olup olmadığını belirtebilir. Ve Katalog ek Kiracı veya aşağıdakiler gibi veritabanı meta verileri depolamak için genişletilmiş:
- Bir veritabanının sürümünü veya hizmet katmanı.
- Veritabanı şema sürümü.
- Kiracı adı ve kendi SLA (hizmet düzeyi sözleşmesi).
- Uygulama Yönetimi, müşteri desteği ve devops işlemlerini etkinleştirmek için bilgi.  

Katalog, kiracılar arası raporlama, şema yönetimi etkinleştirmek için de kullanılabilir ve analiz amacıyla veri ayıklayın. 

### <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı 

Wingtip içinde Kataloğu uygulanan *tenantcatalog* veritabanı. *Tenantcatalog* Shard Management özelliklerini kullanmaya oluşturulan [elastik veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). Bir uygulama oluşturmak, yönetmek ve kullanmak kitaplık etkinleştirir bir *parça eşlemesi* bir veritabanında depolanır. Parçalı veritabanını anlamına gelir, parça, bir kiracı anahtarıyla bir parça eşlemesi çapraz.

Kiracı sağlama sırasında EDCL işlevleri uygulamaları veya PowerShell betikleri parça eşlemesinde girişleri oluşturmak için kullanılabilir. Daha sonra EDCL işlevleri doğru veritabanına bağlanmak için kullanılabilir. EDCL, katalog veritabanındaki trafiğini en aza indirmek ve bağlanma işlemi hızlandırmak için bağlantı bilgilerini önbelleğe alır.

> [!IMPORTANT]
> Yapmak *değil* doğrudan erişim aracılığıyla katalog veritabanındaki verileri düzenleyin! Doğrudan güncelleştirmeleri yüksek veri bozulması riski nedeniyle desteklenmez. Bunun yerine, yalnızca EDCL API'lerini kullanarak eşleme verilerini düzenleyin.

## <a name="tenant-provisioning-pattern"></a>Kiracı sağlama düzeni

#### <a name="checklist"></a>Denetim listesi

Paylaşılan veritabanı, var olan bir paylaşılan veritabanına yeni bir kiracı sağlama istediğinizde, aşağıdaki soruları sormaya gerekir:
- Yeni Kiracı için yeterli alan var mı?
- Yeni Kiracı için gereken başvuru verilerini içeren tablolar yok veya verileri eklenebilir?
- Temel şemaya uygun çeşitlemesi yeni kiracının var mı?
- Yeni Kiracı yakın uygun coğrafi konumdaki nedir?
- Bu doğru hizmet katmanında yeni Kiracı için mi?

Yeni Kiracı kendi veritabanında yalıtılmış istiyorsanız, Kiracı belirtimleri karşılayacak şekilde oluşturabilirsiniz.

Sağlama tamamlandıktan sonra Kiracı kataloğa kaydetmeniz gerekir. Son olarak, uygun parçaya yönlendirmesi başvurmak için Kiracı eşlemesi eklenebilir.

#### <a name="template-database"></a>Şablon veritabanı

SQL betikleri yürütülürken, bir bacpac dağıtma ya da bir şablon veritabanı kopyalayarak veritabanı sağlama. Wingtip uygulama, yeni Kiracı veritabanları oluşturmak için bir şablon veritabanını kopyalayın.

Herhangi bir uygulama gibi Wingtip zamanla gelişecek. Wingtip bazen veritabanında değişiklikler yapılmasını gerektirir. Değişiklikler aşağıdaki öğeleri içerebilir:
- Yeni veya değiştirilmiş şema.
- Yeni veya değiştirilmiş başvuru verileri.
- En iyi uygulama performansını sağlayan rutin veritabanı bakım görevlerini.

SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Bu değişikliklerin gelecekteki Kiracı veritabanları olması için bunların sağlama işlemine dahil gerekir. Bu zorluğu incelediniz daha ayrıntılı olarak [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md).

#### <a name="scripts"></a>Betikler

Bu öğreticide Kiracı sağlama betikleri hem de aşağıdaki senaryoları destekler:
- Diğer kiracılar ile paylaşılan mevcut bir veritabanına bir kiracı sağlama.
- Bir kiracı kendi veritabanında sağlama.

Kiracı verilerini sonra başlatılır ve Katalog parça eşlemesinde kayıtlı. Örnek uygulamada, birden çok kiracının içeren veritabanları genel bir ad gibi verilen *tenants1* veya *tenants2*. Tek bir kiracının içeren veritabanları, kiracının adı verilir. Örnekte kullanılan belirli adlandırma kurallarını katalog kullanımı veritabanına atanan adlara izin verdiği ölçüde deseni kritik bir parçası değildir.  

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Öğretici başlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çok kiracılı veritabanına bir kiracı sağlama
> * Tek kiracılı veritabanına bir kiracı sağlama
> * Çok kiracılı hem tek kiracılı veritabanlarına Kiracı grubu sağlama
> * Bir veritabanı ve bir katalogda eşleme Kiracı kaydetme

#### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

- Wingtip bilet SaaS çok kiracılı veritabanı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [dağıtma ve keşfetme Wingtip bilet SaaS çok kiracılı veritabanı uygulama](saas-multitenantdb-get-started-deploy.md)

- Wingtip komut dosyalarını ve kaynak kodu alın:
    - Wingtip bilet SaaS çok kiracılı veritabanı betikleri ve uygulama kaynak kodunu [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub deposu.
    - Bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip betikleri engelini kaldırmak için. 

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Veritabanına bir kiracı sağlama *paylaşılan* diğer kiracıları ile

Bu bölümde, PowerShell betikleri tarafından gerçekleştirilen sağlama için başlıca eylemlerin listesini görürsünüz. Ardından kod eylemleri görmek için komut dosyaları adım adım için PowerShell ISE hata ayıklayıcıyı kullanın.

#### <a name="major-actions-of-provisioning"></a>Sağlama önemli eylemleri

Adım adım sağlama iş akışının temel öğeleri şunlardır:

- **Yeni Kiracı anahtarını hesaplayın**: Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
- **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**: Katalog, anahtarı zaten kaydedilmemiş emin olmak için denetlenir.
- **Varsayılan Kiracı veritabanındaki Kiracı başlatmak**: Kiracı veritabanı, yeni Kiracı bilgileri ekleyecek şekilde güncelleştirilir.  
- **Kiracı kataloğa kaydetme**: Yeni bir kiracı anahtarı ve mevcut tenants1 veritabanı arasındaki eşlemeyi Kataloğu'na eklenir. 
- **Kiracının adını bir katalog uzantısı tablosuna ekleme**: Mekan adı Kataloğu kiracılar tablosuna eklenir.  Bu ayrıca, ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterir.
- **Yeni Kiracı için açık olayları sayfası**: *Bushwillow Blues* olayları sayfası tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Hata ayıklayıcı adımları

Wingtip uygulama yeni Kiracı sağlama paylaşılan bir veritabanı içinde nasıl uyguladığını anlamak için bir kesme noktası ve iş akışı aracılığıyla adım ekleyin:

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionTenants\\*tanıtım ProvisionTenants.ps1* ve aşağıdaki parametreleri ayarlayın:
   - **$TenantName** = **Bushwillow Blues**, yeni mekanın adı.
   - **$VenueType** = **blues**, önceden tanımlanmış mekan türlerinden biri: blues, klasik müzik, dans, jazz, judo, motosiklet yarışı, çok amaçlı, opera, Rock müzik, futbol (küçük, boşluksuz).
   - **$DemoScenario** = **1**, paylaşılan bir veritabanı içinde bir kiracı ile diğer kiracılar sağlama.

2. İmlecinizi her yerde satırı 38, yazan satıra koyarak, bir kesme noktası ekleyin: *Yeni Kiracı '* ve tuşuna **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. Tuşuna basarak betiği çalıştırın **F5**.

4. Betik yürütme kesme noktasında durduktan sonra basın **F11** için kod içine Adımlama.

   ![hata Ayıkla](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. Betiğin yürütülmesini izleme kullanarak **hata ayıklama** menü seçenekleri **F10** ve **F11**üzerinden veya çağrılan işlevlerin adımlamak için.

PowerShell betikleri hata ayıklama hakkında daha fazla bilgi için bkz. [çalışmaya ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

## <a name="provision-a-tenant-in-its-own-database"></a>İçinde bir kiracı sağlama kendi *kendi* veritabanı

#### <a name="major-actions-of-provisioning"></a>Sağlama önemli eylemleri

Betik izleme sırasında adım adım iş akışının temel öğeleri şunlardır:

- **Yeni Kiracı anahtarını hesaplayın**: Kiracı adından kiracı anahtarı oluşturmak için bir karma işlevi kullanılır.
- **Kiracı anahtarının zaten mevcut olup olmadığını denetleyin**: Katalog, anahtarı zaten kaydedilmemiş emin olmak için denetlenir.
- **Yeni bir kiracı veritabanı oluşturmak**: Veritabanı kopyalayarak oluşturulur *basetenantdb* Resource Manager şablonu kullanarak veritabanı.  Yeni veritabanı adını, kiracının adı temel alır.
- **Veritabanı kataloğuna eklediğiniz**: Yeni Kiracı veritabanı, bir parça kataloğunda olarak kaydedilir.
- **Varsayılan Kiracı veritabanındaki Kiracı başlatmak**: Kiracı veritabanı, yeni Kiracı bilgileri ekleyecek şekilde güncelleştirilir.  
- **Kiracı kataloğa kaydetme**: Yeni Kiracı anahtarına arasındaki eşlemeyi ve *sequoiasoccer* veritabanı Kataloğu'na eklenir.
- **Kiracı adı kataloğa eklediğiniz**: Mekan adı Kataloğu kiracılar uzantısı tablosuna eklenir.
- **Yeni Kiracı için açık olayları sayfası**: *Sequoia futbol* olayları sayfası tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Hata ayıklayıcı adımları

Artık bir kiracı kendi veritabanında oluştururken betik işlemlerinin gösterilmesi:

1. Hala... \\Öğrenme modülleri\\ProvisionTenants\\*tanıtım ProvisionTenants.ps1* şu parametreleri ayarlayın:
   - **$TenantName** = **sequoia futbol**, yeni mekanın adı.
   - **$VenueType** = **futbol**, önceden tanımlanmış mekan türlerinden biri: blues, klasik müzik, dans, jazz, judo, motosiklet yarışı, çok amaçlı, opera, Rock müzik, futbol (küçük, boşluksuz).
   - **$DemoScenario** = **2**, kendi veritabanında bir kiracı sağlama.

2. İmlecinizi her yerde satırı 57, yazan satıra koyarak yeni bir kesme noktası ekleyin:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '* basın **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. Tuşuna basarak betiği çalıştırın **F5**.

4. Betik yürütme kesme noktasında durduktan sonra basın **F11** için kod içine Adımlama.  Kullanım **F10** ve **F11** atla ve işlevlere izlemek için adım.

## <a name="provision-a-batch-of-tenants"></a>Bir kiracı grubu sağlama

Bu alıştırmada, 17 Kiracı grubu sağlanır. Çalışmak için daha fazla veritabanı olduğundan, diğer Wingtip bilet öğreticileri başlatmadan önce bu Kiracı grubu sağlama önerilir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionTenants\\*tanıtım ProvisionTenants.ps1* değiştirip *$DemoScenario* parametresi 4:
   - **$DemoScenario** = **4**, paylaşılan bir veritabanı Kiracı grubu sağlamak için.

2. **F5** tuşuna basıp betiği çalıştırın.

### <a name="verify-the-deployed-set-of-tenants"></a>Dağıtılan dizi kiracının doğrulayın 

Bu aşamada, paylaşılan bir veritabanı'na dağıtılan kiracılar ve kiracılar kendi veritabanlarına dağıtılan bir karışımını sahip. Azure portalında oluşturulan veritabanlarını incelemek için kullanılabilir. İçinde [Azure portalında](https://portal.azure.com)açın **tenants1-mt -\<kullanıcı\>**  gözatarak SQL sunucular listesine sunucu.  **SQL veritabanları** listede paylaşılan yer **tenants1** veritabanı ve veritabanlarını kendi veritabanında olan kiracılar için:

   ![veritabanı listesi öğesine tıklayın](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Azure portalını, Kiracı veritabanlarını gösterir, ancak kiracılar görmenize almazlar *içinde* paylaşılan veritabanı. Kiracılar tam bir listesi görülebilir **olay hub'ı** Wingtip ve Katalog gözatarak Web sayfası.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Wingtip bilet olay hub'ı sayfasını kullanarak

Olay hub'ı sayfasını tarayıcıda açın (http:events.wingtip-mt.\<kullanıcı\>. trafficmanager.net)  

#### <a name="using-catalog-database"></a>Katalog veritabanı kullanma

Kiracılar ve her biri için karşılık gelen veritabanı tam listesini katalogda kullanılabilir. SQL veritabanı adına birleştirme Kiracı adı koşuluyla görülmektedir. Görünüm düzgün şekilde kataloğunda depolanan meta verileri genişletme değerini gösterir.
- SQL görünümü tenantcatalog veritabanında kullanılabilir.
- Kiracı adı, kiracılar tablosunda depolanır.
- Veritabanı adı Shard Management tablolarında depolanır.

1. SQL Server Management Studio (SSMS), kiracılar sunucusuna bağlanma **Kataloğu mt'nin\<kullanıcı\>. database.windows.net**, oturum açma ile = **Geliştirici**ve parola = **P\@ssword1**

    ![SSMS bağlantı iletişim kutusu](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. SSMS nesne Gezgini'nde, görünümlerde göz atın *tenantcatalog* veritabanı.

3. Görünümü sağ tıklayın *TenantsExtended* ve **ilk 1000 satırı Seç**. Kiracı adını ve veritabanı arasındaki eşlemeyi farklı kiracıların unutmayın.

    ![Ssms'de ExtendedTenants görüntüle](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu bölümde, ilgi çekici diğer sağlama düzenleri ele alınmaktadır.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Elastik havuzlardaki veritabanlarını önceden hazırlama

Önceden sağlama düzeni, elastik havuzları kullanırken, faturalama havuz için veritabanlarını olmadığını olgu yararlanan. Bu nedenle bunlar ek maliyet olmaksızın gereklidir önce veritabanlarını elastik havuzlara da eklenebilir. Bu önemli ölçüde önceden visioning veritabanına bir kiracı sağlama için geçen süreyi azaltır. Önceden sağlanan veritabanı sayısı, öngörülen sağlama hızı için uygun bir arabellek tutmak için gereken şekilde ayarlanabilir.

#### <a name="auto-provisioning"></a>Otomatik sağlama

Otomatik sağlama düzeni, özel bir sağlama hizmeti, sunucuları, havuzları ve veritabanlarını gerektiğinde otomatik olarak sağlamak için kullanılır. Bu otomasyon, elastik havuzlardaki veritabanlarını önceden sağlanmasını içerir. Ve istediğiniz gibi sağlama hizmeti tarafından veritabanları kullanımdan kaldırıldı ve silinir, bu elastik havuzları oluşturur boşlukları doldurulabilir.

Bu otomatik hizmet türü, basit veya karmaşık olabilir. Örneğin, otomasyon, birden fazla coğrafyada sağlama işlemlerini gerçekleştirebilir ve coğrafi çoğaltma, olağanüstü durum kurtarma için ayarlayabilirsiniz. Otomatik sağlama düzeni ile bir istemci uygulaması veya betik, bir sağlama hizmeti tarafından işlenecek bir sağlama isteği kuyruğa gönderir. Betik tamamlama algılamak için yoklama. Önceden sağlama kullanılırsa, bir arka plan hizmeti bir yedek veritabanının sağlama yönettiğiniz sırada istekleri hızla işlenebilir.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
- [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Paylaşılan çok kiracılı veritabanı ve kendi veritabanı içinde tek bir yeni Kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracılar sağlama ve bunları kataloğa kaydetme ayrıntılarını adım adım

Deneyin [performans izleme Öğreticisi](saas-multitenantdb-performance-monitoring.md).

