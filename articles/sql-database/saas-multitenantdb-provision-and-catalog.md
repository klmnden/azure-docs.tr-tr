---
title: "SaaS çok kiracılı Azure sağlama | Microsoft Docs"
description: "Sağlamak ve bir Azure SQL veritabanı çok kiracılı SaaS uygulamasında yeni kiracılar katalog hakkında bilgi edinin"
keywords: "sql veritabanı öğreticisi"
services: sql-database
author: MightyPen
manager: craigg
ms.reviewer: billgib;andrela;genemi
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 12/19/2017
ms.author: billgib
ms.openlocfilehash: fb2f2bcbbc8b7f0b0012c4e7baf4a274671d4af0
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="provision-and-catalog-new-tenants-in-a-saas-application-using-a-sharded-multi-tenant-azure-sql-database"></a>Parçalı çok Kiracı Azure SQL veritabanı kullanarak bir SaaS uygulaması sağlama ve Katalog yeni kiracılar

Bu makalede sağlama ve, yeni kiracılar katalog kapsayan bir *çok kiracılı parçalı veritabanı* modeli veya desen.

Bu makalede iki ana bölümden oluşur:

- [Kavramsal tartışma](#goto_2_conceptual) sağlama ve yeni kiracılar kataloglanmasını gerektirir.

- [Öğretici](#goto_1_tutorial) sağlama ve Katalog gerçekleştirir PowerShell komut dosyası kodu vurgular.
    - Öğretici için çok kiracılı parçalı veritabanı düzeni uyarlanan Wingtip biletleri SaaS uygulamasına kullanır.

<a name="goto_2_conceptual"/>

## <a name="database-pattern"></a>Veritabanı düzeni

Bu bölümde, artı birkaç daha fazla, izleyin, çok kiracılı parçalı veritabanı düzeni kavramlar açıklanmaktadır.

Bu çok kiracılı parçalı modelinde, her veritabanı içinde tablo şemalarını Kiracı verilerini depolayan tabloların birincil anahtardaki bir kiracı anahtarı içerir. Kiracı anahtarı, 0, 1 ya da çok sayıda Kiracı depolamak için tek tek her veritabanı sağlar. Parçalı veritabanlarının kullanımını, kiracılar çok fazla sayıda desteklemek uygulama sistemi kolaylaştırır. Herhangi bir kiracı için tüm verileri bir veritabanında depolanmaktadır. Kiracılar çok sayıda pek çok parçalı veritabanları arasında dağıtılır. Katalog veritabanı, veritabanı için her bir kiracı eşleme depolar.

#### <a name="isolation-versus-lower-cost"></a>Yalıtım daha düşük maliyetli karşılaştırması

Tüm kendisi için bir veritabanı sahip bir kiracı yalıtımı yararları özgürlüğüne. Kiracı tarafından diğer kiracılar üzerindeki etkiyi sınırlı kalmayarak olmadan önceki bir tarihe geri yüklenen veritabanı olabilir. Veritabanı performansı ile diğer kiracılar bozmasına gerek kalmadan bir kiracı için yeniden iyileştirmek için ayarlanmış. Yalıtım bir veritabanı diğer kiracılarla paylaşmak için maliyetleri fazlasını maliyetleri sorunudur.

Yeni bir kiracı sağlandığında, diğer kiracılar ile bir veritabanı paylaşabilir veya kendi yeni veritabanına yerleştirilebilir. Daha sonra fikrinizi değiştirirseniz ve veritabanı için diğer durum taşıyın.

Birden çok kiracılar ve tek kiracılar veritabanlarıyla maliyet veya yalıtımı her Kiracı için en iyi duruma getirme uygulamada aynı SaaS karıştırılır.

   ![Kiracı Kataloğu ile parçalı çok Kiracı veritabanı uygulama](media/saas-multitenantdb-provision-and-catalog/MultiTenantCatalog.png)

## <a name="tenant-catalog-pattern"></a>Kiracı katalog düzeni

Her en az bir kiracı içeren iki veya daha fazla veritabanı varsa, uygulamanın hangi veritabanı geçerli ilgi Kiracı depolar bulmak için bir yol olması gerekir. Bu eşleme katalog veritabanına depolar.

#### <a name="tenant-key"></a>Kiracı anahtarı

Her bir kiracı için Kiracı anahtarı benzersiz bir anahtar Wingtip uygulama türetilemeyeceğini. Uygulama, Web sayfası URL'den Kiracı adı ayıklar. Uygulama adı anahtarı edinmek için karma hale getirir. Uygulama Kataloğu'na erişmek için anahtarı kullanır. Katalog çapraz Kiracı depolandığı veritabanı hakkında bilgi. Uygulama veritabanı bilgisi bağlamak için kullanır. Diğer Kiracı anahtar düzenlerini de kullanılabilir.

Bir katalog kullanan uygulama kesintiye uğratmadan sağladıktan sonra değiştirilecek Kiracı veritabanı konumunu veya adını olanak tanır. Çok Kiracı veritabanı modelinde, bir kiracı veritabanları arasında taşıma katalog düzenler.

#### <a name="tenant-metadata-beyond-location"></a>Konum ötesinde Kiracı meta verileri

Katalog ayrıca bir kiracı Bakım veya başka eylemler için çevrimdışı olup olmadığını gösterebilir. Ve ek Kiracı veya aşağıdaki öğeleri gibi veritabanı meta verileri depolamak için katalog genişletilmiş:
- Hizmet katmanını veya bir veritabanı sürümü.
- Veritabanı şeması sürümü.
- Kiracı adı ve SLA'sını (hizmet düzeyi sözleşmesi).
- Uygulama Yönetimi, müşteri desteği veya devops işlemlerini etkinleştirmek için bilgi.  

Katalog arası Kiracı raporlama, şema yönetimini etkinleştirmek için de kullanılabilir ve verileri analiz amaçlı ayıklayın. 

### <a name="elastic-database-client-library"></a>Elastik Veritabanı İstemci Kitaplığı 

Wingtip içinde katalog uygulanan *tenantcatalog* veritabanı. *Tenantcatalog* parça yönetim özelliklerine kullanılarak oluşturulan [esnek veritabanı istemci kitaplığı (EDCL)](sql-database-elastic-database-client-library.md). Bir uygulama oluşturmak, yönetmek ve kullanmak kitaplık etkinleştirir bir *parça eşleme* bir veritabanında depolanır. Parça eşleme parçalı veritabanını anlamına gelir, parça Kiracı anahtarıyla çapraz.

Kiracı sağlama sırasında EDCL işlevleri uygulamalar veya PowerShell komut dosyaları parça eşlemesinde girişleri oluşturmak için kullanılabilir. Daha sonra EDCL işlevleri doğru veritabanına bağlanmak için kullanılabilir. EDCL Katalog veritabanı üzerinde trafiğini en aza indirmek ve bağlanma işlemi hızlandırmak için bağlantı bilgilerini önbelleğe alır.

> [!IMPORTANT]
> Yapmak *değil* doğrudan erişim aracılığıyla katalog veritabanındaki verileri düzenleme! Doğrudan güncelleştirmeler veri bozulması nedeniyle yüksek riskli desteklenmiyor. Bunun yerine, yalnızca EDCL API'lerini kullanarak eşleme verilerini düzenleyin.

## <a name="tenant-provisioning-pattern"></a>Kiracı sağlama düzeni

#### <a name="checklist"></a>Denetim listesi

Paylaşılan veritabanı var olan bir paylaşılan veritabanına yeni bir kiracı sağlamak istediğinizde, aşağıdaki soruları sormaya gerekir:
- Yeni Kiracı için sol yeterli alan var mı?
- Yeni Kiracı için gereken başvuru verileri tablolarla yok veya veri eklenebilir mi?
- Yeni Kiracı için temel şemaya uygun varyasyonunu var mı?
- Yeni Kiracı yakın uygun coğrafi konumda nedir?
- Yeni Kiracı için doğru hizmet katmanını nedir?

Yeni Kiracı kendi veritabanında yalıtılmış istediğinizde, Kiracı belirtimlerine uygun şekilde oluşturabilirsiniz.

Sağlama tamamlandıktan sonra Kiracı katalogda kaydetmeniz gerekir. Son olarak, Kiracı eşlemesini uygun parça başvurmak için eklenebilir.

#### <a name="template-database"></a>Şablon veritabanı

Veritabanını SQL komut dosyaları yürütme bir bacpac dağıtma veya bir şablon veritabanı kopyalama sağlamak. Wingtip uygulamaları yeni Kiracı veritabanı oluşturmak için bir şablon veritabanı kopyalayın.

Herhangi bir uygulama gibi Wingtip zamanla gelişmesi. Bazen Wingtip veritabanı değişiklikleri gerektirir. Aşağıdaki öğeler değişiklikler şunlar olabilir:
- Yeni veya değiştirilmiş şema.
- Yeni veya değiştirilmiş başvuru verileri.
- Rutin veritabanı bakım görevlerini en iyi uygulama performansı elde etmek için.

SaaS uygulamasıyla, bu değişikliklerin oldukça büyük olabilecek bir kiracı veritabanı filosunda eşgüdümlü şekilde dağıtılması gerekir. Bu değişiklikler Kiracı veritabanları gelecekte olmasını sağlama işlemine yapılması ihtiyaç duyar. Bu sorunu incelediniz daha ayrıntılı olarak [şema yönetimi Öğreticisi](saas-tenancy-schema-management.md).

#### <a name="scripts"></a>Betikler

Bu öğreticide Kiracı sağlama kodları aşağıdaki senaryolardan her ikisi de destekler:
- Diğer kiracılar ile paylaşılan var olan bir veritabanı içine bir kiracı sağlama.
- Bir kiracı kendi veritabanına sağlama.

Kiracı veri sonra başlatıldı ve Katalog parça eşlemesinde kayıtlı. Örnek uygulamayı birden çok kiracıya içeren veritabanları genel adı gibi verilir *tenants1* veya *tenants2*. Tek bir kiracı içeren veritabanları kiracının adı verilir. Katalog veritabanına atanmış herhangi bir ad kullanılmasına gibi örnekte kullanılan belirli adlandırma kuralları düzeni önemli bir parçası değildir.  

<a name="goto_1_tutorial"/>

## <a name="tutorial-begins"></a>Öğretici başlar

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Çok kiracılı veritabanına bir kiracı sağlama
> * Bir kiracı tek Kiracı veritabanına sağlama
> * Çok kiracılı ve tek Kiracı veritabanlarına kiracılar toplu sağlama
> * Bir veritabanı ve bir katalogda eşleme Kiracı kaydetme

#### <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

- Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Wingtip biletleri SaaS çok Kiracı veritabanı uygulama keşfedin.](saas-multitenantdb-get-started-deploy.md)

- Wingtip komut dosyaları ve kaynak kodu alın:
    - Wingtip biletleri SaaS çok Kiracı veritabanı komut dosyalarını ve uygulama kaynak koduna kullanılabilir olan [WingtipTicketsSaaS MultitenantDB](https://github.com/microsoft/WingtipTicketsSaaS-MultiTenantDB) GitHub depo.
    - Bkz: [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip betikleri engellemesini kaldırmak. 

## <a name="provision-a-tenant-into-a-database-shared-with-other-tenants"></a>Bir veritabanı bir kiracı sağlamak *paylaşılan* diğer kiracılar ile

Bu bölümde, PowerShell komut dosyaları tarafından gerçekleştirilen önemli Eylemler sağlama listesini görürsünüz. Ardından kod eylemleri görmek için komut dosyaları aracılığıyla adım için PowerShell ISE hata ayıklayıcı kullanın.

#### <a name="major-actions-of-provisioning"></a>Sağlama ana Eylemler

Adım adım sağlama iş akışı temel öğeleri şunlardır:

- **Yeni Kiracı anahtarına hesaplamak**: bir karma işlevi Kiracı adından Kiracı anahtarını oluşturmak için kullanılır.
- **Kiracı anahtarı olup olmadığını denetle**: Katalog anahtar olmayan zaten kaydettirildi emin olmak için denetlenir.
- **Varsayılan Kiracı veritabanındaki Kiracı başlatma**: Kiracı veritabanı yeni Kiracı bilgiler eklenerek güncelleştirildi.  
- **Kiracı kataloğa kaydetmek**: yeni Kiracı anahtarını ve varolan tenants1 veritabanı arasında eşleme Kataloğu'na eklenir. 
- **Katalog uzantısı tabloya kiracının adı ekleme**: salonundan ad katalog kiracılar tablosuna eklenir.  Bu ayrıca, ek uygulamaya özgü verileri desteklemek için Katalog veritabanı nasıl Genişletilebilir gösterir.
- **Yeni Kiracı için açık olayları sayfası**: *Bushwillow mavi* Etkinlikler sayfasını tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/bushwillow.png)

#### <a name="debugger-steps"></a>Hata ayıklayıcı adımları

Yeni Kiracı paylaşılan bir veritabanında sağlama Wingtip uygulama nasıl uyguladığını anlamak için bir kesme noktası ve iş akışı aracılığıyla adımı ekleyin:

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionTenants\\*Demo ProvisionTenants.ps1* ve aşağıdaki parametreleri ayarlayabilirsiniz:
   - **$TenantName** = **Bushwillow mavi**, yeni bir salonundan adı.
   - **$VenueType** = **mavi**, önceden tanımlanmış salonundan türlerinden birini: Mavi, classicalmusic, dance, jazz, judo, motorracing, çok amaçlı, opera, rockmusic, futbol (küçük, boşluk yok).
   - **$DemoScenario** = **1**, paylaşılan veritabanında bir kiracı diğer kiracılar ile sağlamak için.

2. İmlecinizi herhangi bir yere satırı 38, yazan satırı koyarak kesme noktası ekleme: *yeni Kiracı '*ve tuşuna basarak **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint.png)

3. Tuşuna basarak komut dosyasını çalıştırmak **F5**.

4. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.

   ![hata Ayıkla](media/saas-multitenantdb-provision-and-catalog/debug.png)

5. Betiğin yürütmeyi kullanarak **hata ayıklama** menü seçeneklerini **F10** ve **F11**üzerinden veya çağrılan işlevler halinde adım için.

PowerShell komut dosyalarını hata ayıklama hakkında daha fazla bilgi için bkz: [ile çalışma ve PowerShell betikleri hata ayıklama ipuçları](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise).

## <a name="provision-a-tenant-in-its-own-database"></a>Kiracı sağlama kendi *kendi* veritabanı

#### <a name="major-actions-of-provisioning"></a>Sağlama ana Eylemler

İş akışı, aracılığıyla betik izleme sırasında adım temel öğeleri şunlardır:

- **Yeni Kiracı anahtarına hesaplamak**: bir karma işlevi Kiracı adından Kiracı anahtarını oluşturmak için kullanılır.
- **Kiracı anahtarı olup olmadığını denetle**: Katalog anahtar olmayan zaten kaydettirildi emin olmak için denetlenir.
- **Yeni bir kiracı veritabanı oluşturmak**: veritabanı kopyalayarak oluşturulur *basetenantdb* Resource Manager şablonu kullanarak veritabanı.  Yeni bir veritabanı adı kiracının adını temel alır.
- **Veritabanı kataloğa eklemek**: yeni bir kiracı veritabanı parça kataloğunda olarak kaydedilir.
- **Varsayılan Kiracı veritabanındaki Kiracı başlatma**: Kiracı veritabanı yeni Kiracı bilgiler eklenerek güncelleştirildi.  
- **Kiracı kataloğa kaydetmek**: yeni Kiracı anahtarına arasında eşleme ve *sequoiasoccer* veritabanı Kataloğu'na eklenir.
- **Kiracı adı kataloğa eklediğiniz**: salonundan ad katalog kiracılar uzantısı tablosuna eklenir.
- **Yeni Kiracı için açık olayları sayfası**: *Sequoia futbol* Etkinlikler sayfasını tarayıcıda açılır.

   ![etkinlikler](media/saas-multitenantdb-provision-and-catalog/sequoiasoccer.png)

#### <a name="debugger-steps"></a>Hata ayıklayıcı adımları

Şimdi komut dosyası işlemi boyunca bir kiracı kendi veritabanında oluştururken, yol:

1. Hala... \\Öğrenme modülleri\\ProvisionTenants\\*Demo ProvisionTenants.ps1* aşağıdaki parametreleri ayarlayın:
   - **$TenantName** = **sequoia futbol**, yeni bir salonundan adı.
   - **$VenueType** = **futbol**, önceden tanımlanmış salonundan türlerinden birini: Mavi, classicalmusic, dance, jazz, judo, motorracing, çok amaçlı, opera, rockmusic, futbol (küçük harf, boşluk yok).
   - **$DemoScenario** = **2**, bir kiracı kendi veritabanına sağlamak için.

2. Yeni bir kesme noktası imlecinizi herhangi bir yere satırı 57, yazan satırı koyarak ekleyin:  *& &nbsp;$PSScriptRoot\New-TenantAndDatabase '*ve basın **F9**.

   ![kesme noktası](media/saas-multitenantdb-provision-and-catalog/breakpoint2.png)

3. Tuşuna basarak komut dosyasını çalıştırmak **F5**.

4. Betik yürütme kesme noktasında durdurulduktan sonra basın **F11** koda adım.  Kullanım **F10** ve **F11** üzerinden adım ve yürütmeyi işlevleri adım.

## <a name="provision-a-batch-of-tenants"></a>Kiracılar toplu sağlama

Bu alıştırmada 17 kiracılar toplu sağlar. Çalışmak için daha fazla veritabanı olduklarından Wingtip biletleri öğreticilere başlatmadan önce bu toplu kiracıların sağlamak önerilir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\ProvisionTenants\\*Demo ProvisionTenants.ps1* değiştirip *$DemoScenario* parametresi 4:
   - **$DemoScenario** = **4**, paylaşılan bir veritabanına kiracılar toplu sağlayacak.

2. **F5** tuşuna basıp betiği çalıştırın.

### <a name="verify-the-deployed-set-of-tenants"></a>Kiracılar dağıtılan kümesini doğrulayın 

Bu aşamada, paylaşılan bir veritabanına dağıtılan kiracılar ve kiracıların kendi veritabanlarına dağıtılan bir karışımını sahip. Azure portalında oluşturulan veritabanlarını incelemek için kullanılabilir. İçinde [Azure portal](https://portal.azure.com), açık **tenants1-mt -\<kullanıcı\>**  SQL sunucuları listesine giderek sunucu.  **SQL veritabanları** listede paylaşılan yer **tenants1** veritabanı ve veritabanları kendi veritabanında olan kiracılar için:

   ![veritabanı listesi öğesine tıklayın](media/saas-multitenantdb-provision-and-catalog/Databases.png)

Azure portalı Kiracı veritabanları gösterirken, kiracılar görmenize almazlar *içinde* paylaşılan veritabanı. Kiracılar tam listesi görülebilir **olay hub'ı** Wingtip ve Katalog giderek Web sayfası.

#### <a name="using-wingtip-tickets-events-hub-page"></a>Wingtip biletleri olayları hub sayfası kullanma

Olay hub'ı sayfasını tarayıcıda açın (http:events.wingtip-mt.\<kullanıcı\>. trafficmanager.net)  

#### <a name="using-catalog-database"></a>Katalog veritabanı kullanma

Kiracılar ve her biri için karşılık gelen veritabanı tam listesini katalogda kullanılabilir. Birleştirmeler Kiracı adı sağlanan veritabanı adı bir SQL görünümü olmamasıdır. Görünüm sorunsuz şekilde kataloğunda depolanan meta verileri genişletme değerini gösterir.
- SQL görünümü tenantcatalog veritabanında kullanılabilir.
- Kiracı adı kiracılar tablosunda depolanır.
- Veritabanı adı parça yönetim tablolarında depolanır.

1. Kiracılar sunucuda SQL Server Management Studio (SSMS) içinde bağlanmak **katalog yüksekliğindeki\<kullanıcı\>. database.windows.net**, oturum açma ile = **Geliştirici**ve parola = **P@ssword1**

    ![SSMS bağlantı iletişim kutusu](media/saas-multitenantdb-provision-and-catalog/SSMSConnection.png)

2. SSMS nesne Gezgini'nde görünümleri için Gözat *tenantcatalog* veritabanı.

3. Görünümü sağ tıklayın *TenantsExtended* ve **ilk 1000 satırı Seç**. Kiracı adı ve veritabanı arasındaki eşlemeyi farklı kiracılar için unutmayın.

    ![SSMS ExtendedTenants görünümünde](media/saas-multitenantdb-provision-and-catalog/extendedtenantsview.png)
      
## <a name="other-provisioning-patterns"></a>Diğer sağlama düzenleri

Bu bölüm, diğer ilginç sağlama desenleri açıklar.

#### <a name="pre-provisioning-databases-in-elastic-pools"></a>Esnek havuzlar veritabanlarında önceden sağlama

Önceden hazırlama düzeni esnek havuzlarını kullanırken faturalama havuzu için veritabanlarını olmadığını olgu yararlanan. Bu nedenle ek bir maliyeti yansıtılmasını olmadan gereklidir önce veritabanlarının bir esnek havuz için eklenebilir. Bu önemli ölçüde önceden visioning veritabanına bir kiracı sağlamak için geçen süreyi azaltır. Önceden hazırlanan veritabanı sayısı arabellek beklenen sağlama oranı için uygun tutmak için gerektiği şekilde ayarlanabilir.

#### <a name="auto-provisioning"></a>Otomatik sağlama

Otomatik sağlama modelinde, sunucular, havuzları ve gerektiğinde otomatik olarak veritabanları sağlamak için ayrılmış bir sağlama hizmeti kullanılır. Bu Otomasyon esnek havuzlar veritabanlarında önceden sağlanmasını içerir. Ve istediğiniz gibi sağlama hizmeti tarafından veritabanları kullanımdan ve silindiğinde, bu esnek havuzlarını oluşturur boşluklar doldurulabilir.

Bu tür otomatik hizmet basit veya karmaşık olabilir. Örneğin, Otomasyon birden çok farklı coğrafyalara sağlama ele ve olağanüstü durum kurtarma için coğrafi çoğaltma ayarlayabilirsiniz. Otomatik sağlama desen ile bir istemci uygulama veya betik sağlama hizmeti tarafından işlenmesi için bir sağlama isteği kuyruğa gönderin. Komut dosyası tamamlama algılamak için yoklama. Önceden hazırlama kullanılırsa, bir arka plan hizmeti bir değişiklik veritabanı sağlama yönettiğiniz sırada istekleri hızlı bir şekilde, ele alınması.

## <a name="additional-resources"></a>Ek kaynaklar

<!-- - Additional [tutorials that build upon the Wingtip SaaS application](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)-->
- [Elastik veritabanı istemci kitaplığı](sql-database-elastic-database-client-library.md)
- [Windows PowerShell ISE’de Betik Hatalarını Ayıklama](https://msdn.microsoft.com/powershell/scripting/core-powershell/ise/how-to-debug-scripts-in-windows-powershell-ise)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Paylaşılan bir çok Kiracı veritabanı ve kendi veritabanı tek bir yeni Kiracı sağlama
> * Ek kiracı grubu sağlama
> * Kiracılar sağlama ve kataloğa kaydetme ayrıntılarını adım adım

Deneyin [performans izleme Öğreticisi](saas-multitenantdb-performance-monitoring.md).

