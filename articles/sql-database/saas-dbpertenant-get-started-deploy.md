---
title: "Çok kiracılı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs"
description: "Dağıtma ve Azure SQL veritabanı kullanarak SaaS desenleri gösteren Wingtip SaaS çok kiracılı uygulama keşfedin."
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
ms.date: 11/10/2017
ms.author: sstein
ms.openlocfilehash: 6270656ba172f1e85df557d59173ecdcf7d6cbcb
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="deploy-and-explore-a-multi-tenant-application-that-uses-azure-sql-database---wingtip-saas"></a>Dağıtma ve Azure SQL veritabanı - Wingtip SaaS kullanan çok kiracılı uygulama keşfedin

Bu öğreticide dağıtın ve Wingtip SaaS uygulamasına keşfedin. Uygulama, birden fazla kiracıya hizmet vermek için SaaS uygulama düzeni olan kiracı başına veritabanı kullanır. Uygulama, etkinleştirme SaaS senaryoları basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Aşağıdaki *Azure'a Dağıt* düğmesine tıkladıktan beş dakika sonra, bulutta çalışır durumda olan ve SQL Veritabanı’nı kullanan çok kiracılı bir SaaS uygulamanız olur. Uygulama, her bir SQL esnek havuza tüm dağıtılan kendi veritabanına sahip üç örnek kiracıları ile dağıtılır. Uygulama keşfedin ve tek tek uygulama bileşenleri ile çalışmak için tam erişim vererek Azure aboneliğinize dağıtılır. Uygulama kaynak kodu ve yönetim komut dosyaları WingtipSaaS GitHub depo kullanılabilir.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Wingtip SaaS uygulamasına dağıtma
> * Uygulama kaynak koduna ve yönetim komut dosyaları nereden
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * Kiracıların kendi veri ile nasıl eşlendiğini *Kataloğu*
> * Yeni bir kiracı sağlama
> * Uygulamasında Kiracı Etkinlik izleme

Çeşitli SaaS tasarım ve yönetim düzenlerini keşfetmek için bu ilk dağıtımı temel alan bir [dizi ilgili öğretici](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) bulabilirsiniz. Öğreticilerde ilerlerken, sağlanan betikleri ayrıntılı şekilde inceleyin ve farklı SaaS düzenlerinin nasıl uygulandığını araştırın. SaaS uygulamalarının geliştirilmesini basitleştiren birçok SQL Veritabanı özelliğinin nasıl uygulandığını ayrıntılı olarak anlamak için öğreticilerdeki betiklerde adım adım ilerleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-the-wingtip-saas-application"></a>Wingtip SaaS uygulamasına dağıtmak

Wingtip dağıtmak SaaS uygulama:

1. Tıklatarak **Azure'a Dağıt** düğme Wingtip SaaS dağıtım şablonu için Azure Portalı'nı açar. Şablonu iki parametre değerlerini gerektirir; Yeni bir kaynak grubu için bir ad ve bu dağıtım Wingtip SaaS uygulamasının diğer dağıtımlardan ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için Ayrıntılar sağlar.

   Bunları daha sonra bir yapılandırma dosyasına girmeniz gerekir çünkü kullanın, tam değerlerini not emin olun.

   <a href="http://aka.ms/deploywtpapp" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtım için gerekli parametre değerlerini girin:

    > [!IMPORTANT]
    > Gösterim amaçları doğrultusunda bazı kimlik doğrulama işlemlerinin ve sunucu güvenlik duvarlarının güvenliği kasıtlı olarak kaldırılmıştır. **Yeni bir kaynak grubu oluşturma**ve mevcut kaynak grupları, sunucular veya havuzları kullanmayın. Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.

    * **Kaynak grubu** - seçin **Yeni Oluştur** ve sağlayan bir **adı** kaynak grubu için. Seçin bir **konumu** aşağı açılan listeden.
    * **Kullanıcı** -bazı kaynaklar genel benzersiz adlar gerektirir. Benzersizlik emin olmak için dağıttığınız her uygulama sağlamasına Wingtip uygulama herhangi başka bir dağıtım tarafından oluşturulan kaynaklardan oluşturma kaynakları ayırt etmek için bir değer. Adınız ve soyadınızın baş harfleri ile bir sayı gibi kısa bir **Kullanıcı** adının (örneğin, *bg1*) tercih edilmesi ve ardından bunun kaynak grubu adında (örneğin, *wingtip-bg1*) kullanılması önerilir. **Kullanıcı** parametresi yalnızca harf, rakam ve kısa çizgi (boşluksuz) içerebilir. İlk ve son karakteri bir harf ya da sayı olmalıdır (tümünün küçük harf olması önerilir).


1. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul için tıklatın.
    * **Satın al**’a tıklayın.

1. **Bildirimler**’e (arama kutusunun sağındaki zil simgesi) tıklayarak dağıtım durumunu izleyin. Wingtip SaaS uygulama dağıtma yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-saas-scripts"></a>Karşıdan yükleme ve Wingtip SaaS betikleri Engellemeyi Kaldır

Uygulama dağıtımı sırasında kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken, .zip dosyasını ayıklanıyor önce engellemesini kaldırmak için aşağıdaki adımları izleyin. Bu komut dosyalarını çalıştırma izni sağlar.

1. Gözat [Wingtip SaaS github deposuna](https://github.com/Microsoft/WingtipSaaS).
1. Tıklatın **Kopyala veya indir**.
1. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
1. Sağ **WingtipSaaS-master.zip** dosyasını bulun ve seçin **özellikleri**.
1. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**, tıklatıp **Uygula**.
1. **Tamam** düğmesine tıklayın.
1. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\WingtipSaaS ana\\öğrenme modülleri* klasör.

## <a name="update-the-configuration-file-for-this-deployment"></a>Bu dağıtım için yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce ayarlayın *kaynak grubu* ve *kullanıcı* değerler **UserConfig.psm1**. Dağıtım sırasında ayarlanan değerleri için bu değişkenleri ayarlayın.

1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\*UserConfig.psm1*’i açın
1. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
1. Değişiklikleri kaydedin!

Bu ayarı burada basitçe, her komut dosyası bu dağıtım özgü değerleri güncelleştirmek zorunda kalmaktan tutar.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama, etkinliklerin düzenlendiği konser salonları, caz kulüpleri, spor kulüpleri gibi mekanları gösterir. Görebildikleri olayları listelemek ve bilet satabilir kolay bir yol için müşterinin (veya) Wingtip platformun kaydedin. Her mekan, etkinliklerini yönetmek ve bilet satmak için diğer kiracılardan bağımsız ve ayrı olan kişiselleştirilmiş bir web uygulaması alır. Her bir kiracı perde arkasında SQL esnek havuza dağıtılan bir SQL veritabanı alır.

Merkezi bir **Olay Hub’ı**, dağıtımınıza özel kiracı URL’lerinin bir listesini sağlar.

1. Açık _olay hub'ı_ web tarayıcınızda: http://events.wtp.&lt; Kullanıcı&gt;. trafficmanager.net (dağıtımınızın kullanıcı adıyla değiştirin):

    ![olay hub’ı](media/saas-dbpertenant-get-started-deploy/events-hub.png)

1. *Olay Hub’ında* **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Etkinlikler](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)


Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Kiracıya özel olan etkinlikler sayfası, kiracı adlarının URL’lere dahil edilmesini gerektirir. Tüm kiracı URL’leri, özel *Kullanıcı* değerinizi içerir ve şu biçimi takip eder: http://events.wtp.&lt;USER&gt;.trafficmanager.net/*fabrikamjazzclub*. Etkinlikler uygulaması, kiracı adını URL’den ayrıştırır ve [*parça eşleme yönetimini*](sql-database-elastic-scale-shard-map-management.md) kullanarak bir kataloğa erişim sağlayacak anahtar oluşturmak için kullanır. Katalog kiracının veritabanı konumu için anahtar eşler. **Olay hub'ı** URL'lerin listesini sağlamak üzere her bir veritabanı ile ilişkili kiracının adını almak için genişletilmiş meta veri Kataloğu'nda kullanır.

Üretim ortamında, [*şirket internet etki alanının*](../traffic-manager/traffic-manager-point-internet-domain.md) trafik yöneticisi profilini göstermesi için genellikle bir CNAME DNS kaydı oluşturursunuz.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Uygulamanın dağıtıldığı, şimdi uygulamaya koyun! *Demo LoadGenerator* PowerShell komut dosyasını tüm Kiracı veritabanları karşı çalışan bir iş yükünü başlatır. Birçok SaaS uygulamalarını gerçek Yük genellikle durumlarıyla ve tahmin edilemez. Bu tür yük benzetimini yapmak için tüm kiracılar, rastgele aralıklarla gerçekleşen her bir kiracı üzerinde rastgele WINS'e ile dağıtılmış bir yük üreteci oluşturur. Bu nedenle çıkmaya yük düzeni için birkaç dakika sürer, böylece yük izleme önce en az üç veya dört dakika çalıştırmak Oluşturucu izin vermek en iyisidir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\yardımcı programları\\*Demo LoadGenerator.ps1* komut dosyası.
1. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

> [!IMPORTANT]
> Diğer komut dosyalarını çalıştırmak için yeni bir PowerShell ISE penceresi açın. Yük oluşturucu, yerel PowerShell oturumunuzda bir dizi iş çalıştırıyor. *Demo LoadGenerator.ps1* betik devreye bir ön plan görev artı arka plan yük oluşturma işleri bir dizi olarak çalışan gerçek yük Oluşturucu betik kapalı. Bir yük Oluşturucu işi ve kataloğa kayıtlı her veritabanı için çağrılır. PowerShell oturumu kapatma tüm işleri durdurur şekilde işleri yerel PowerShell oturumunuzda çalışır. Makinenizi askıya alırsanız yük oluşturma duraklatıldı ve, makineyi uyandırmak devam edecek.

Yük Oluşturucu yük oluşturma işleri her bir kiracı için çağırır. sonra ön plan görev, sonradan sağlanan yeni kiracılar için ek arka plan işleri başladığı bir işi çağırma durumda kalır. Kullanabileceğiniz *Ctrl-C* veya basın *durdurmak* ön plan görevi Durdur düğmesine, ancak varolan arka plan işleri her veritabanı oluşturma devam edecek. Gerekiyorsa izlemek ve arka plan işleri denetlemek, kullanma *Get-Job*, *Receive-Job* ve *işini durdurmayı*. Ön plan Görev yürütülürken başka betikleri çalıştırmak için aynı PowerShell oturumunda kullanamazsınız. Diğer komut dosyalarını çalıştırmak için yeni bir PowerShell ISE penceresi açın.

Yük Oluşturucu oturumu yeniden başlatmak istiyorsanız, örneğin farklı parametrelerle ön plan görev durdurabilir ve yeniden çalıştırın *Demo LoadGenerator.ps1* komut dosyası. Yeniden çalıştırmayı *Demo LoadGenerator.ps1* herhangi şu anda çalışan işler ve işleri geçerli parametrelerini kullanarak yeni bir dizi devreye Oluşturucu yeniden başlatır ilk durdurur.

Şimdilik, iş çağırma durumda çalışan yük Oluşturucu bırakın.


## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk üç örnek kiracılar oluşturur, ancak bu dağıtılmış uygulamanın nasıl etkilediğini görmek için başka bir kiracı oluşturalım. Wingtip SaaS sağlama kiracılar iş akışı içinde ayrıntılı [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md). Bu adımda, yeni bir kiracı hızla oluşturun.

1. *PowerShell ISE’de* ...\\Öğrenme Modülleri\Sağlama ve Kataloğa Kaydetme\\*Demo-ProvisionAndCatalog.ps1*’i açın.
1. Tuşuna **F5** komut dosyasını çalıştırmak için (şu an için varsayılan değerleri bırakın).

   > [!NOTE]
   > Birçok Wingtip SaaS komut *$PSScriptRoot* diğer komut dosyalarında işlevleri çağırmak için klasörleri gezinme izin vermek için. Tuşuna basarak komut çalıştırıldığında bu değişken yalnızca hesaplanan **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) neden hataları, bu nedenle basın **F5** betikleri çalışırken.

Yeni bir kiracı veritabanı bir SQL esnek havuzu oluşturulur, başlatılmış ve kataloğa kayıtlı. Yeni Kiracı bilet satış başarılı sağladıktan sonra *olayları* sitesi tarayıcınızda görüntülenir:

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Yenileme *olay hub'ı* ve yeni Kiracı şimdi listede görüntülenir.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Bir yük kiracılar koleksiyonu karşı çalışan başladıktan, dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com)SQL sunucuları listesine göz atın ve Aç **katalog -&lt;kullanıcı&gt;**  sunucu. Katalog sunucusu iki veritabanı içerir. **Tenantcatalog**ve **basetenantdb** (boş bir *altın* veya yeni kiracılar oluşturmak için kopyalanan şablon veritabanı).

   ![veritabanı](./media/saas-dbpertenant-get-started-deploy/databases.png)

1. SQL sunucu listesine geri gidin ve açmak **tenants1 -&lt;kullanıcı&gt;**  Kiracı veritabanlarını barındıran sunucu. Her Kiracı veritabanı bir _esnek standart_ 50 eDTU standart havuzdaki veritabanı. Ayrıca fark bir _kırmızı Akçaağaç yarış_ veritabanı, daha önce sağlanan Kiracı veritabanı.

   ![sunucu](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Yük oluşturucu birkaç dakikadır çalışıyorsa havuz ve veritabanlarında yerleşik olarak bulunan izleme olanaklarının bazılarını incelemeye başlamak için yeteri kadar verinin bulunması gerekir.

1. Sunucuya Gözat **tenants1 -&lt;kullanıcı&gt;**, tıklatıp **Pool1** (yük oluşturucuyu çalışan bir saat için aşağıdaki grafiklerde) havuz için kaynak kullanımı görüntülemek için:

   ![havuzu izleme](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

Bu iki grafik, elastik havuzların ve SQL Veritabanı’nın SaaS uygulaması iş yükleri için ne kadar uygun olduğunu gösterir. Her biri 40 eDTU’ya kadar geçiş yapabilen dört veritabanı, 50 eDTU havuzunda kolayca desteklenmektedir. Tek başına veritabanı olarak sağlanan varsa, her bir S2 olması gerekiyor yaptıkları (WINS'e desteklemek için 50 DTU). 4 tek başına S2 veritabanları maliyetini yaklaşık 3 kez fiyat havuzunun olacaktır ve havuzunun hala yeterli boş alan çok daha fazla veritabanı için vardır. Gerçek dünya durumlarda, SQL veritabanı müşteriler 200 eDTU havuzu 500 veritabanlarında kadar şu anda çalışmıyor. Daha fazla bilgi için [Performans izleme öğreticisini](saas-dbpertenant-performance-monitoring.md) inceleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]

> * Wingtip SaaS uygulamasına dağıtma
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> * Yeni kiracılar sağlama
> * Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Şimdi deneyin [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md).



## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Wingtip SaaS uygulamasının yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* Elastik havuzlar hakkında bilgi edinmek için bkz. [*Azure SQL elastik havuzu nedir?*](sql-database-elastic-pool.md)
* Esnek işler hakkında bilgi edinmek için bkz. [*Genişletilmiş bulut veritabanlarını yönetme*](sql-database-elastic-jobs-overview.md)
* Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](saas-tenancy-app-design-patterns.md)
