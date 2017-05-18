---
title: "Wingtip Bilet Platformu (WTP) uygulamasını dağıtma ve keşfetme (Azure SQL Veritabanı’nı kullanan örnek SaaS uygulaması) | Microsoft Belgeleri"
description: "Azure SQL Veritabanı’nı kullanan örnek bir SaaS uygulamasını dağıtma ve keşfetme"
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
ms.openlocfilehash: ff4dc19bb6e24ea9ceeed9721cfb3a85b4d10965
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-azure-sql-database"></a>Azure SQL Veritabanı’nı kullanan çok kiracılı SaaS uygulamasını dağıtma ve keşfetme

Bu öğreticide, Wingtip Bilet Platformu (WTP) SaaS uygulamasını dağıtır ve keşfedersiniz. Uygulama, birden fazla kiracıya hizmet vermek için SaaS uygulama düzeni olan kiracı başına veritabanı kullanır. Uygulama, Azure SQL Veritabanı’nın SaaS senaryolarını mümkün kılan özelliklerini ve SaaS tasarım ve yönetim düzenlerini göstermek için tasarlanmıştır.

Aşağıdaki *Azure'a Dağıt* düğmesine tıkladıktan beş dakika sonra, bulutta çalışır durumda olan ve SQL Veritabanı’nı kullanan çok kiracılı bir SaaS uygulamanız olur. Uygulama, her biri kendi veritabanına sahip olan ve tümü SQL Elastik havuzuna dağıtılan üç örnek kiracıyla dağıtılır. Uygulama, Azure aboneliğinize dağıtılarak her bir uygulama bileşenini incelemeniz ve bunlarla çalışmanız için size tam erişim sağlar.

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * WTP uygulamasını dağıtma
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> * Yeni kiracılar sağlama
> * Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Çeşitli SaaS tasarım ve yönetim düzenlerini keşfetmek için bu ilk dağıtımı temel alan bir [dizi ilgili öğretici](sql-database-wtp-overview.md) bulabilirsiniz. Öğreticilerde ilerlerken, sağlanan betikleri ayrıntılı şekilde inceleyin ve farklı SaaS düzenlerinin nasıl uygulandığını araştırın. SaaS uygulamalarının geliştirilmesini basitleştiren birçok SQL Veritabanı özelliğinin nasıl uygulandığını ayrıntılı olarak anlamak için öğreticilerdeki betiklerde adım adım ilerleyin.

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-the-wingtip-tickets-wtp-saas-application"></a>Wingtip biletleri (WTP) SaaS Uygulamasını Dağıtma

Wingtip bilet platformunu beş dakikadan kısa bir süre içinde dağıtın:

1. Dağıtmak için tıklayın:

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtım için gerekli parametre değerlerini girin:

    > [!IMPORTANT]
    > Gösterim amaçları doğrultusunda bazı kimlik doğrulama işlemlerinin ve sunucu güvenlik duvarlarının güvenliği kasıtlı olarak kaldırılmıştır. **Yeni bir kaynak grubu oluşturun** ve mevcut kaynak gruplarını, sunucuları veya havuzları ve bu uygulamayı ya da oluşturduğu kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.

    * **Kaynak grubu** - **Yeni oluştur**’u seçin ve bir **Ad** ve **Konum** girin.
    * **Kullanıcı** - Bazı kaynaklar için tüm Azure abonelikleri genelinde benzersiz olan adlar gerekir. Benzersiz olmasını sağlamak için oluşturduğunuz kaynakları, Wingtip uygulamasını dağıtan diğer kullanıcıların oluşturduğu kaynaklardan farklı kılacak bir değer sağlayın. Adınız ve soyadınızın baş harfleri ile bir sayı gibi kısa bir **Kullanıcı** adının (örneğin, *bg1*) tercih edilmesi ve ardından bunun kaynak grubu adında (örneğin, *wingtip-bg1*) kullanılması önerilir. **Kullanıcı** parametresi yalnızca harf, sayı ve kısa çizgi içerebilir. İlk ve son karakteri bir harf ya da sayı olmalıdır (tümünün küçük harf olması önerilir).

     ![şablon](./media/sql-database-saas-tutorial/template.png)

1. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul ediyorsanız tıklayın.
    * **Panoya sabitle**’yi seçin.
    * **Satın al**’a tıklayın.

1. **Bildirimler**’e (arama kutusunun sağındaki zil simgesi) tıklayarak dağıtım durumunu izleyin. WTP uygulamasının dağıtılması yaklaşık dört dakika sürer.

   ![dağıtım başarılı](media/sql-database-saas-tutorial/succeeded.png)

## <a name="explore-the-application"></a>Uygulamayı keşfetme

Uygulama, etkinliklerin düzenlendiği konser salonları, caz kulüpleri, spor kulüpleri gibi mekanları gösterir. Mekanlar, etkinlikleri listelemenin ve biletleri satmanın kolay bir yolunu sağlamak için Wingtip Bilet Platformu’nun (WTP) müşterileri (veya kiracıları) olarak kaydedilir. Her mekan, etkinliklerini yönetmek ve bilet satmak için diğer kiracılardan bağımsız ve ayrı olan kişiselleştirilmiş bir web uygulaması alır. Her kiracı, güvenli bir şekilde SQL Elastik havuzuna dağıtılmış bir SQL veritabanı alır.

Merkezi bir **Olay Hub’ı**, dağıtımınıza özel kiracı URL’lerinin bir listesini sağlar. Tüm kiracı URL’leri, özel *Kullanıcı* değerinizi içerir ve şu biçimi takip eder: http://events.wtp.&lt;USER&gt;.trafficmanager.net/*fabrikamjazzclub*. 

1. _Olay Hub’ını_ açın: http://events.wtp.&lt;USER&gt;.trafficmanager.net (Kullanıcı adınızla değiştirin):

    ![olay hub’ı](media/sql-database-saas-tutorial/events-hub.png)

1. *Olay Hub’ında* **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/sql-database-saas-tutorial/fabrikam.png)

1. **Biletler**’e tıklayın ve etkinlik için bilet satın alıp alamayacağınızı inceleyin.

WTP uygulaması, gelen trafiğin dağıtımını kontrol etmek için [*Azure Traffic Manager*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)’ı kullanır. Kiracıya özel olan etkinlikler sayfası, kiracı adlarının URL’lere dahil edilmesini gerektirir. Etkinlikler uygulaması, kiracı adını URL’den ayrıştırır ve [*parça eşleme yönetimini*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-scale-shard-map-management) kullanarak bir kataloğa erişim sağlayacak anahtar oluşturmak için kullanır. Katalog, anahtarı kiracının veritabanı konumuyla eşleştirir. **Olay Hub'ı**, her bir veritabanıyla ilişkili kiracının adını almak için katalogdaki genişletilmiş meta verileri kullanır.

Üretim ortamında, [*şirket internet etki alanının*](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-point-internet-domain) trafik yöneticisi profilini göstermesi için genellikle bir CNAME DNS kaydı oluşturursunuz.

## <a name="get-the-wingtip-application-scripts"></a>Wingtip uygulama betiklerini alma

Wingtip Bilet betikleri ve uygulama kaynağı kodu, [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS) github deposunda bulunabilir. Betik dosyaları, [Öğrenme Modülleri klasöründe](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) yer alır. **Öğrenme Modülleri** klasörünü, klasör yapısını koruyarak yerel bilgisayarınıza indirin.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk dağıtım üç örnek kiracı oluşturur, ancak en iyi öğretici deneyimi için daha fazla kiracı oluşturmamız gerekir. Bu adımda, hızla yeni bir kiracı oluşturacaksınız. Daha sonra [Sağlama ve kataloğa kaydetme öğreticisinde](sql-database-saas-tutorial-provision-and-catalog.md) yeni kiracı sağlamanın ayrıntılarını inceleyerek uygulamaya kayıt bileşeni uygulanmanın ve müşteriler kaydoldukça kiracıları otomatik olarak sağlamanın ne kadar kolay olduğunu görebilirsiniz.

### <a name="initialize-the-user-config-file-for-your-deployment"></a>Dağıtımınız için kullanıcı yapılandırma dosyasını başlatma

Bu ilk WTP uygulama dağıtımından sonra çalıştıracağınız ilk betik olduğundan, kullanıcı yapılandırma dosyasını güncelleştirmeniz gerekir. Değişkenleri, dağıtımınızdaki belirli değerlerle güncelleştirin.

   1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\*UserConfig.psm1*’i açın
   1. _$userConfig.ResourceGroupName_’i, uygulamayı dağıtırken ayarladığınız _kaynak grubu_ ile değiştirin.
   1. _$userConfig.Name_’i, uygulamayı dağıtırken ayarladığınız _Kullanıcı_ adı olarak değiştirin.

### <a name="provision-the-new-tenant"></a>Yeni kiracı sağlama

1. *PowerShell ISE’de* ...\\Öğrenme Modülleri\Sağlama ve Kataloğa Kaydetme\\*Demo-ProvisionAndCatalog.ps1*’i açın.
1. Betiği çalıştırmak için **F5**’e basın (şimdilik betiğin varsayılan değerlerini bırakın).

Yeni kiracı kataloğa kaydedilir. Veritabanları oluşturulur ve SQL elastik havuzuna eklenir. Başarılı sağlama işleminden sonra, yeni kiracının bilet satış sitesi tarayıcınızda görünür:

![Yeni kiracı](./media/sql-database-saas-tutorial/red-maple-racing.png)

Olay Hub'ını yenileyin ve yeni kiracının listede olduğunu doğrulayın.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Artık birkaç kiracı veritabanımız olduğuna göre, bunları çalıştırmaya başlayabiliriz! Tüm kiracı veritabanlarına yönelik çalışan bir iş yükünün benzetimini gerçekleştiren PowerShell betiği sağlanır. Yükleme, varsayılan olarak 60 dakika sürer. Wingtip Biletleri bir SaaS uygulamasıdır ve SaaS uygulamasındaki gerçek yük genellikle düzensiz ve tahmin edilemezdir. Yük oluşturucu, bunun benzetimini gerçekleştirmek için kiracılar genelinde dağıtılan rastgele yük oluşturur. Düzenin ortaya çıkması için birkaç dakika gerekir, bu nedenle aşağıdaki bölümlerde yükü izlemeye çalışmadan önce yük oluşturucunun 5-10 dakika çalışmasını sağlayın.

1. **PowerShell ISE**’de ...\\Öğrenme Modülleri\\Yardımcı Programlar\\*Demo-LoadGenerator.psm1*’i açın
1. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

> [!IMPORTANT]
> Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş olarak çalışır. Bu nedenle, PowerShell oturumunu açık bırakın! PowerShell'i veya yük oluşturucunun başlatıldığı sekmeyi kapatırsanız ya da makinenizi askıya alırsanız işler durur.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Uygulama hakkında bilgi edindiğinize, yeni bir kiracı sağladığınıza ve kiracı koleksiyonuna yönelik yük çalıştırmaya başladığınıza göre, artık dağıtılan kaynaklardan bazılarını inceleyebiliriz.

1. [Azure portalında](http://portal.azure.com) **catalog-&lt;USER&gt;** sunucusunu açın. Katalog sunucusu iki veritabanı içerir. **tenantcatalog** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan boş bir *altın* veritabanı).

   ![veritabanları](./media/sql-database-saas-tutorial/databases.png)

1. Kiracı veritabanlarını barındıran **tenants1-&lt;USER&gt;** sunucusunu açın. Her kiracı veritabanının, 50 eDTU standart havuzundaki _Esnek Standart_ veritabanında bulunduğunu unutmayın. Ayrıca, daha önce sağladığınız bir kiracı olan _Red Maple Yarışı_ veritabanını da göreceksiniz.

   ![sunucu](./media/sql-database-saas-tutorial/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Yük oluşturucu birkaç dakikadır çalışıyorsa havuz ve veritabanlarında yerleşik olarak bulunan izleme olanaklarının bazılarını incelemeye başlamak için yeteri kadar verinin bulunması gerekir.

1. *tenants1-&lt;USER&gt;* sunucusuna göz atın ve **Pool1**’e tıklayarak havuzun kaynak kullanımını görüntüleyin:

   ![havuzu izleme](./media/sql-database-saas-tutorial/monitor-pool.png)

Bu iki grafik, elastik havuzların ve SQL Veritabanı’nın SaaS uygulaması iş yükleri için ne kadar uygun olduğunu gösterir. Her biri 40 eDTU’ya kadar geçiş yapabilen dört veritabanı, 50 eDTU havuzunda kolayca desteklenmektedir. Her birinin bağımsız bir veritabanı olarak sağlanması halinde, bunların geçişlerini desteklemek için S2 (50 DTU) gerekecek, ancak 4 bağımsız S2 veritabanının maliyeti havuz fiyatının neredeyse 3 katı olacaktır. Üstelik, havuzda daha çok veritabanı için yeteri kadar yer kalır. Gerçek dünyada, müşteriler hâlihazırda 200 eDTU havuzunda 500’e kadar veritabanı çalıştırmaktadır. Daha fazla bilgi için [Performans izleme öğreticisini](sql-database-saas-tutorial-performance-monitoring.md) inceleyin.


## <a name="deleting-the-resources-created-with-this-tutorial"></a>Bu öğretici ile oluşturulan kaynakları silme

WTP uygulamasını keşfetmeyi ve bu uygulamayla çalışmayı tamamladığınızda, portalda uygulamanın kaynak grubuna gidin ve bu dağıtımla ilgili tüm faturalandırma işlemlerini durdurmak için uygulamayı silin. İlişkili öğreticilerden herhangi birini kullandıysanız öğreticinin oluşturduğu tüm kaynaklar da uygulamayla birlikte silinecektir.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]

> * WTP uygulamasını dağıtma
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> * Yeni kiracılar sağlama
> * Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Şimdi [Sağlama ve kataloğa kaydetme öğreticisini](sql-database-saas-tutorial-provision-and-catalog.md) inceleyin



## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip Bilet Platformu (WTP) uygulamasının ilk dağıtımına dayalı ek öğreticiler](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* Elastik havuzlar hakkında bilgi edinmek için bkz. [*Azure SQL elastik havuzu nedir?*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)
* Esnek işler hakkında bilgi edinmek için bkz. [*Genişletilmiş bulut veritabanlarını yönetme*](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-jobs-overview)
* Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)

