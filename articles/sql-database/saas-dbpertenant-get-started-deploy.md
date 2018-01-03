---
title: "Çok kiracılı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs"
description: "Dağıtma ve Kiracı ve Azure SQL veritabanı kullanarak diğer SaaS desenleri başına veritabanının gösteren Wingtip biletleri SaaS çok kiracılı uygulama keşfedin."
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
ms.openlocfilehash: f91ddff81e51e7cc3d1561dc799013764530924b
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı ile Kiracı deseni başına veritabanı kullanan bir çok kiracılı SaaS uygulaması keşfedin

Bu öğreticide dağıtın ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfedin. Uygulama, birden çok kiracıya veri depolamak için bir kiracı başına veritabanı desen kullanır. Uygulama, etkinleştirme SaaS senaryoları basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Aşağıdaki *Azure'a Dağıt* düğmesine tıkladıktan beş dakika sonra, bulutta çalışır durumda olan ve SQL Veritabanı’nı kullanan çok kiracılı bir SaaS uygulamanız olur. Uygulama, her bir SQL esnek havuza tüm dağıtılan kendi veritabanına sahip üç örnek kiracıları ile dağıtılır. Uygulama keşfedin ve tek tek uygulama bileşenleri ile çalışmak için tam erişim vererek Azure aboneliğinize dağıtılır. Uygulama kaynak kodu ve yönetim komut dosyaları WingtipTicketsSaaS DbPerTenant GitHub depo kullanılabilir.


Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]

> * Wingtip SaaS uygulamasına dağıtma
> * Uygulama kaynak koduna ve yönetim komut dosyaları nereden
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * Kiracıların kendi veri ile nasıl eşlendiğini *Kataloğu*
> * Yeni bir kiracı sağlama
> * Uygulamasında Kiracı Etkinlik izleme

Çeşitli SaaS tasarım ve yönetim düzenlerini keşfetmek için bu ilk dağıtımı temel alan bir [dizi ilgili öğretici](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) bulabilirsiniz. Öğreticilerde ilerlerken, sağlanan betikleri ayrıntılı şekilde inceleyin ve farklı SaaS düzenlerinin nasıl uygulandığını araştırın. SaaS uygulamalarının geliştirilmesini basitleştiren birçok SQL Veritabanı özelliğinin nasıl uygulandığını ayrıntılı olarak anlamak için öğreticilerdeki betiklerde adım adım ilerleyin.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wingtip biletleri SaaS uygulamasına dağıtmak

Uygulamayı dağıtın:

1. Tıklatarak **Azure'a Dağıt** düğme Azure portalında Wingtip biletleri SaaS veritabanı Kiracı dağıtım şablonu başına açar. Şablonu iki parametre değerlerini gerektirir; Yeni bir kaynak grubu için bir ad ve bu dağıtım Kiracı uygulama başına Wingtip biletleri SaaS veritabanının diğer dağıtımlardan ayıran bir kullanıcı adı. Sonraki adım, bu değerleri ayarlamak için Ayrıntılar sağlar.

   Bunları daha sonra bir yapılandırma dosyasına girmeniz gerekir çünkü kullanın, tam değerlerini not emin olun.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Dağıtım için gerekli parametre değerlerini girin:

    > [!IMPORTANT]
    > Gösterim amaçları doğrultusunda bazı kimlik doğrulama işlemlerinin ve sunucu güvenlik duvarlarının güvenliği kasıtlı olarak kaldırılmıştır. **Yeni bir kaynak grubu oluşturma**. Mevcut kaynak grupları, sunucular veya havuzları kullanmayın. Bu uygulamayı, komut dosyaları veya dağıtılan tüm kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.

    * **Kaynak grubu** - seçin **Yeni Oluştur** ve sağlayan bir **adı** kaynak grubu için. 
    * **Konum** - seçin bir **konumu** aşağı açılan listeden.
    * **Kullanıcı** -bazı kaynaklar genel benzersiz adlar gerektirir. Benzersizlik emin olmak için dağıttığınız her uygulama sağlamasına Wingtip uygulama herhangi başka bir dağıtım tarafından oluşturulan kaynaklardan oluşturma kaynakları ayırt etmek için bir değer. Kısa kullanmak için önerilen **kullanıcı** adınızın baş harflerini artı bir sayı gibi ad (örneğin, *af1*) ve ardından kaynak grubu adı kullanan (örneğin, *wingtip af1*). **Kullanıcı** parametresi yalnızca harf, rakam ve kısa çizgi (boşluksuz) içerebilir. İlk ve son karakteri bir harf ya da sayı olmalıdır (tümünün küçük harf olması önerilir).


1. **Uygulamayı dağıtın**.

    * Hüküm ve koşulları kabul için tıklatın.
    * **Satın al**’a tıklayın.

1. **Bildirimler**’e (arama kutusunun sağındaki zil simgesi) tıklayarak dağıtım durumunu izleyin. Wingtip biletleri SaaS uygulama dağıtma yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Karşıdan yükleme ve Wingtip biletleri yönetim komut dosyaları Engellemeyi Kaldır

Uygulama dağıtımı sırasında kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken, .zip dosyasını ayıklanıyor önce engellemesini kaldırmak için aşağıdaki adımları izleyin. Bu komut dosyalarını çalıştırma izni sağlar.

1. Gözat [WingtipTicketsSaaS DbPerTenant GitHub deposuna](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant).
1. Tıklatın **Kopyala veya indir**.
1. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
1. Sağ **WingtipTicketsSaaS DbPerTenant master.zip** dosyasını bulun ve seçin **özellikleri**.
1. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**, tıklatıp **Uygula**.
1. **Tamam** düğmesine tıklayın.
1. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\WingtipTicketsSaaS DbPerTenant ana\\öğrenme modülleri* klasör.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Bu dağıtım için kullanıcı yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce güncelleştirme *kaynak grubu* ve *kullanıcı* değerler **UserConfig.psm1**. Dağıtım sırasında kullanılan değerleri için bu değişkenleri ayarlayın.

1. İçinde *PowerShell ISE*açın... \\Modülleri öğrenme\\*UserConfig.psm1* 
1. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
1. Değişiklikleri kaydedin!

Bu değerler, neredeyse her komut dosyasında başvurulur.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama birlikte salonları, jazz Sinek ve olayları barındıran Spor Sinek gibi görebildikleri hedefler. Görebildikleri olayları listelemek ve bilet satabilir kolay bir yol için müşterinin (veya) Wingtip biletlerinin kaydedin. Her salonundan yönetmek ve bunların olaylarını listelemek ve bağımsız ve diğer kiracıdan yalıtılmış bilet satabilir için kişiselleştirilmiş bir web sitesini alır. Her bir kiracı perde arkasında SQL esnek havuza dağıtılan bir SQL veritabanı alır.

Merkezi bir **olay hub'ı** sayfası, dağıtımınızdaki kiracılar bağlantıların listesini sağlar.

1. Açık _olay hub'ı_ web tarayıcınızda: http://events.wingtip-dpt.&lt;kullanıcı&gt;. trafficmanager.net (yerine &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile):

    ![olay hub’ı](media/saas-dbpertenant-get-started-deploy/events-hub.png)

1. *Olay Hub’ında* **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)


Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [ *Azure Traffic Manager*](../traffic-manager/traffic-manager-overview.md). Kiracıya özel olan etkinlikler sayfası, kiracı adlarının URL’lere dahil edilmesini gerektirir. Tüm Kiracı URL'leri özel *kullanıcı* değeri ve bu biçim izleyin: http://events.wingtipp-dpt.&lt;kullanıcı&gt;.trafficmanager.net/*fabrikamjazzclub*. Olayları uygulama URL'den Kiracı adı ayrıştırır ve kullanılarak uygulanan bir Kataloğu'na erişmek için bir anahtar oluşturmak için kullandığı [ *parça eşleme Yönetim*](sql-database-elastic-scale-shard-map-management.md). Katalog kiracının veritabanı konumu için anahtar eşler. **Olay hub'ı** URL'lerin listesini sağlamak üzere her bir veritabanı ile ilişkili kiracının adını almak için genişletilmiş meta veri Kataloğu'nda kullanır.

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

İlk üç örnek kiracılar oluşturur, ancak bu dağıtılmış uygulamanın nasıl etkilediğini görmek için başka bir kiracı oluşturalım. Wingtip biletleri SaaS sağlama kiracılar iş akışı içinde ayrıntılı [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md). Bu adımda, yeni bir kiracı hızla oluşturun.

1. İçinde *PowerShell ISE*açın... \\Modules\Provision ve Katalog öğrenme\\*Demo ProvisionAndCatalog.ps1* .
1. Tuşuna **F5** komut dosyasını çalıştırmak için (şu an için varsayılan değerleri bırakın).

   > [!NOTE]
   > Birçok Wingtip SaaS komut *$PSScriptRoot* diğer komut dosyalarında işlevleri çağırmak için klasörleri gidin. Tam komut dosyası tuşlarına basarak çalıştırıldığında bu değişken yalnızca hesaplanan **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) neden hataları, bu nedenle basın **F5** betikleri çalışırken.

Yeni bir kiracı veritabanı bir SQL esnek havuzu oluşturulur, başlatılmış ve kataloğa kayıtlı. Başarılı hazırlama, yeni kiracının sonra *olayları* sitesi tarayıcınızda görüntülenir:

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Yenileme *olay hub'ı* ve yeni Kiracı şimdi listede görüntülenir.


## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Bir yük kiracılar koleksiyonu karşı çalışan başladıktan, dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com)SQL sunucuları listesine göz atın ve Aç **katalog-dpt -&lt;kullanıcı&gt;**  sunucu. Katalog sunucusu iki veritabanlarını içeren **tenantcatalog** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan bir şablon veritabanı).

   ![veritabanları](./media/saas-dbpertenant-get-started-deploy/databases.png)

1. SQL sunucu listesine geri gidin ve açmak **tenants1-dpt -&lt;kullanıcı&gt;**  Kiracı veritabanlarını barındıran sunucu. Her Kiracı veritabanı bir _esnek standart_ 50 eDTU standart havuzdaki veritabanı. Ayrıca fark bir _kırmızı Akçaağaç yarış_ veritabanı, daha önce sağlanan Kiracı veritabanı.

   ![sunucu](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Yük oluşturucu birkaç dakikadır çalışıyorsa havuz ve veritabanlarında yerleşik olarak bulunan izleme olanaklarının bazılarını incelemeye başlamak için yeteri kadar verinin bulunması gerekir.

Sunucuya Gözat **tenants1-dpt -&lt;kullanıcı&gt;**, tıklatıp **Pool1** (yük oluşturucuyu çalışan bir saat için aşağıdaki grafiklerde) havuz için kaynak kullanımı görüntülemek için:

   ![havuzu izleme](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

Alt grafik ilk 5 veritabanları eDTU kullanımı havuzda gösterirken, havuz eDTU kullanımı, üst grafik gösterir.  Bu iki grafik, elastik havuzların ve SQL Veritabanı’nın SaaS uygulaması iş yükleri için ne kadar uygun olduğunu gösterir. Her biri 40 eDTU’ya kadar geçiş yapabilen dört veritabanı, 50 eDTU havuzunda kolayca desteklenmektedir. Tek başına veritabanı olarak sağlanan varsa, her bir S2 olması gerekiyor yaptıkları (WINS'e desteklemek için 50 DTU). 4 tek başına S2 veritabanları maliyetini yaklaşık 3 kez fiyat havuzunun olacaktır ve havuzunun hala yeterli boş alan çok daha fazla veritabanı için vardır. Gerçek dünya durumlarda, SQL veritabanı müşteriler 200 eDTU havuzu 500 veritabanlarında kadar şu anda çalışmıyor. Daha fazla bilgi için [Performans izleme öğreticisini](saas-dbpertenant-performance-monitoring.md) inceleyin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]

> * Wingtip biletleri SaaS uygulamasına dağıtma
> * Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> * *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> * Yeni kiracılar sağlama
> * Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> * İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Şimdi deneyin [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md).



## <a name="additional-resources"></a>Ek kaynaklar

* Ek [Kiracı uygulama başına Wingtip biletleri SaaS veritabanı üzerinde yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
* Elastik havuzlar hakkında bilgi edinmek için bkz. [*Azure SQL elastik havuzu nedir?*](sql-database-elastic-pool.md)
* Esnek işler hakkında bilgi edinmek için bkz. [*Genişletilmiş bulut veritabanlarını yönetme*](sql-database-elastic-jobs-overview.md)
* Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](saas-tenancy-app-design-patterns.md)
