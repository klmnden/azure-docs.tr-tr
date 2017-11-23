---
title: "Azure SQL veritabanı kullanan bir parçalı çok Kiracı veritabanı SaaS uygulaması dağıtma | Microsoft Docs"
description: "Dağıtma ve Azure SQL veritabanı kullanarak SaaS desenleri gösteren parçalı Wingtip biletleri SaaS çok Kiracı veritabanı uygulama, keşfedin."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: stevestein
manager: craigg
editor: billgib;MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: sstein
ms.openlocfilehash: cb55bf1f1c7eeb0fc7608aca8d70818b5e3e06c0
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı kullanan parçalı bir çok kiracılı uygulama keşfedin

Bu öğreticide dağıtın ve Wingtip biletleri adlı örnek bir SaaS çok Kiracı veritabanı uygulama keşfedin. Wingtip uygulama SaaS senaryoları uyarlamasını basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Bu uygulaması, Wingtips parçalı çok Kiracı veritabanı desen kullanır. Parçalama tarafından Kiracı tanımlayıcısıdır. Kiracı veri Kiracı tanımlayıcı değerlerini göre belirli veritabanına dağıtılır. Verilen bir veritabanı içeren kaç kiracılar olursa olsun tüm, tablo şemalarını bir kiracı tanımlayıcı dahil anlamda çok kiracılı veritabanlarıdır. 

Bu veritabanı deseni her parça veya veritabanında bir veya daha fazla Kiracı depolamanıza olanak sağlar. Birden çok kiracılar tarafından paylaşılan her veritabanı sağlayarak için en düşük maliyeti en iyi duruma getirebilirsiniz. Veya, yalıtım için yalnızca bir kiracı depolamak her veritabanı sağlayarak en iyi duruma getirebilirsiniz. En iyi duruma getirme tercih ettiğiniz bağımsız olarak her belirli bir kiracı için yapılabilir. Tercih ettiğiniz Kiracı ilk depolanan ya da daha sonra fikrinizi değiştirirseniz yapılabilir. Uygulama ya da düzgün şekilde çalışmak üzere tasarlanmıştır.

#### <a name="app-deploys-quickly"></a>Uygulamayı hızlı bir şekilde dağıtır

Aşağıdaki dağıtım bölümde **Azure'a Dağıt** düğmesi. Düğmeye basıldığında Wingtip uygulaması beş dakika sonra tam olarak dağıtılır. Wingtip uygulama Azure bulutunda çalışır ve Azure SQL veritabanı kullanır. Wingtip Azure aboneliğinize dağıtılır. Tek tek uygulama bileşenleri ile çalışmak için tam erişime sahip.

Uygulama verileri üç örnek kiracılar için dağıtılır. Kiracılar birlikte bir çok kiracılı veritabanında depolanır.

C# ve PowerShell kaynak kodu herkes için Wingtip anahtarlarından indirebilirsiniz [Github depomuzda][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Bu öğreticide öğrenin

> [!div class="checklist"]

> - Wingtip SaaS uygulamasının nasıl.
> - Uygulama kaynak koduna ve yönetim komut dosyaları nereden.
> - Sunucular ve veritabanları hakkında uygulaması olun.
> - Kiracıların kendi veri ile nasıl eşlendiğini *katalog*.
> - Yeni bir kiracı sağlamasını yapma.
> - Uygulamasında Kiracı etkinliğini izlemek nasıl.

İlgili eğitim serileri kullanılabilir bu ilk dağıtım sırasında oluşturun. Aralık, SaaS tasarım ve yönetim desenleri öğreticileri keşfedin. Şu öğreticileri çalışırken, adım farklı SaaS desenleri nasıl uygulandığını görmek için sağlanan betikler için önerilir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son Azure PowerShell yüklü. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Wingtip dağıtmak biletleri uygulama

1. Aşağıdaki **Azure'a Dağıt** düğmesi.
    - Wingtip biletleri SaaS dağıtım şablonu ile Azure Portalı'nı açar. İki parametre değerlerini şablonu gerektirir: yeni bir kaynak grubu ve bu dağıtım uygulamanın diğer dağıtımlardan ayıran bir 'user' değeri için bir ad. Sonraki adım, bu parametre değerlerini ayarlamak için Ayrıntılar sağlar.
        - Daha sonra bir yapılandırma dosyası için gerekir çünkü kullanın, tam değerleri not etmeyi unutmayın.

    [![Düğme için Azure'a dağıtın.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

2. Dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarı ayarları, tanıtım kolaylaştırmak için kasıtlı olarak güvenli değil. Seçin **yeni bir kaynak grubu oluşturma**ve mevcut kaynak grupları, sunucular veya havuzları kullanmayın. Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.

    Kaynak adları yalnızca küçük harf, sayı ve kısa çizgi kullanmak en iyisidir.

    - İçin **kaynak grubu** - seçin **Yeni Oluştur**ve ardından sağlayan bir **adı** (büyük küçük harf duyarlı) kaynak grubu için.
        - Kaynak grubu adı tüm harfler küçük harfli olması önerilir.
        - Bir rakam ile izlenen adınızın baş harflerini ve ardından bir tire append öneririz: Örneğin, *wingtip af1*.
        - Seçin bir **konumu** aşağı açılan listeden.
    - İçin **kullanıcı** -kısa seçmenizi öneririz **kullanıcı** adınızın baş harflerini artı bir rakam gibi değeri: Örneğin, *af1*.

3. **Uygulamayı dağıtın**.

    - Hüküm ve koşulları kabul için tıklatın.
    - **Satın al**’a tıklayın.

4. Dağıtım durumunu izlemek tıklayarak **bildirimleri**, arama kutusunun sağındaki zil simgesine olduğu. Wingtip uygulama dağıtma yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Karşıdan yükleme ve yönetim komut dosyaları Engellemeyi Kaldır

Uygulama dağıtımı sırasında uygulama kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken, .zip dosyasını ayıklanıyor önce engellemesini kaldırmak için aşağıdaki adımları kullanın. .Zip dosyasını engellemelerini kaldırma tarafından komut dosyalarını çalıştırmak için izin verildiğinden emin olun.

1. Gözat [WingtipTicketsSaaS MultiTenantDb github deposuna](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Tıklatın **Kopyala veya indir**.
3. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
4. Sağ **WingtipTicketsSaaS MultiTenantDb master.zip** dosya ve seçin **özellikleri**.
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**, tıklatıp **Uygula**.
6. **Tamam** düğmesine tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları bulunan *... \\WingtipTicketsSaaS MultiTenantDb ana\\öğrenme modülleri\\*  klasör.

## <a name="update-the-configuration-file-for-this-deployment"></a>Bu dağıtım için yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce ayarlayın *kaynak grubu* ve *kullanıcı* değerler **UserConfig.psm1**. Dağıtım sırasında ayarladığınız aynı değerlere bu değişkenleri ayarlayın.

1. *PowerShell ISE*’de ...\\Öğrenme Modülleri\\*UserConfig.psm1*’i açın
2. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin.

Doğru önemlidir bu dosyasında ayarlanan değerler tüm betikler tarafından kullanılır. Uygulama yeniden dağıtın, farklı bir kaynak grubu ve kullanıcı değeri seçin emin olun. Ardından UserConfig yeni değerlerle güncelleştirin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama, etkinliklerin düzenlendiği konser salonları, caz kulüpleri, spor kulüpleri gibi mekanları gösterir. Görebildikleri olayları listelemek ve bilet satabilir kolay bir yol için müşterinin (veya) Wingtip platformun kaydedin. Her mekan, etkinliklerini yönetmek ve bilet satmak için diğer kiracılardan bağımsız ve ayrı olan kişiselleştirilmiş bir web uygulaması alır. Perde arkasında parçalı bir çok kiracılı veritabanında varsayılan olarak her bir kiracının veriler depolanır.

Merkezi bir **olay hub'ı** belirli dağıtımınızdaki kiracılar bağlantıların listesini sağlar.

1. Açık *olay hub'ı* web tarayıcınızda:
    - http://Events.Wingtip-MT.&lt;kullanıcı&gt;. trafficmanager.net &nbsp; *(dağıtımınızın kullanıcı değerle değiştirin.)*

    ![olay hub’ı](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. *Olay Hub’ında* **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

Gelen istekleri, uygulamanın kullandığı dağıtımını denetlemek için [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Kiracı özgü, olayları sayfaları URL'de Kiracı adını içerir. URL'leri de belirli, kullanıcı değeri içerir ve bu biçim izleyin:

- http://Events.Wingtip-MT.&lt;kullanıcı&gt;.trafficmanager.net/*fabrikamjazzclub*
 
Olayları uygulama URL'den Kiracı adı ayrıştırır ve Kataloğu'nu kullanarak erişmek için bir anahtar oluşturmak üzere karma hale [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md). Katalog kiracının veritabanı konumu için anahtar eşler. **Olay hub'ı** ve kataloğa kayıtlı tüm kiracılar listeler. **Olay hub'ı** URL'leri oluşturmak için her eşleme ile ilişkili kiracının adını almak için katalogda genişletilmiş meta verilerini kullanır.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturacak [bir şirketin internet etki alanını işaret](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi profili için.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Uygulamanın dağıtıldığı, şimdi uygulamaya koyun! *Demo LoadGenerator* PowerShell komut dosyasını her bir kiracı için çalışan bir iş yükünü başlatır. Birçok SaaS uygulamalarını gerçek Yük genellikle durumlarıyla ve tahmin edilemez. Bu tür yük benzetimini yapmak için tüm kiracılar arasında dağıtılmış yük üreteci oluşturur. Yük rastgele aralıklarla gerçekleşen her bir kiracı üzerinde rastgele WINS'e içerir. Yük izleme önce en az üç veya dört dakika çalıştırmak Oluşturucu izin vermek en iyisidir çıkmaya, yük düzeni için birkaç dakika sürer.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\yardımcı programları\\*Demo LoadGenerator.ps1* komut dosyası.
2. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

> [!IMPORTANT]
> *Demo LoadGenerator.ps1* komut dosyası yükleme Oluşturucu çalıştığı başka bir PowerShell oturumu açar. Arka plan yük oluşturma işleri, her bir kiracı için bir tane çağıran bir ön plan görevi olarak bu oturumda yük Oluşturucu çalışır.

Ön plan görev başlatıldıktan sonra iş çağrılırken bir durumda kalır. Görev sonradan sağlanan yeni kiracılar için ek arka plan işleri başlatır.

PowerShell oturumu kapatma tüm işleri durdurur.

Farklı parametre değerleri kullanmak için yük Oluşturucu oturumu yeniden isteyebilirsiniz. Öyleyse, PowerShell oluşturma oturumu kapatın ve sonra yeniden *Demo LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Parçalı veritabanına yeni bir kiracı sağlama

İlk üç örnek kiracılar içeren *Tenants1* veritabanı. Bu dağıtılmış uygulamanın nasıl etkilediğini görmek için başka bir kiracı oluşturalım. Bu adımda, yeni bir kiracı hızla oluşturun.

1. Aç... \\Modules\ProvisionTenants öğrenme\\*Demo ProvisionTenants.ps1* içinde *PowerShell ISE*.
2. Tuşuna **F5** komut dosyasını çalıştırmak için (şu an için varsayılan değerleri bırakın).

   > [!NOTE]
   > Birçok Wingtip biletleri SaaS komut *$PSScriptRoot* klasörler, gezinti izin vermek veya başka betikleri çağrılacak veya modülleri içeri aktarmak için. Bu değişken tuşuna basarak komut dosyasını bir bütün olarak yalnızca yürütüldüğü zaman değerlendirilen **F5**.  Vurgulama ve bir seçim çalıştıran (**F8**) neden hataları, bu nedenle basın **F5** betikleri çalışırken.

Yeni kırmızı Akçaağaç yarış Kiracı eklenen *Tenants1* veritabanı ve kataloğa kayıtlı. Yeni Kiracı bilet satış *olayları* site tarayıcınızda açar:

![Yeni kiracı](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Yenileme *olay hub'ı* ve yeni Kiracı şimdi listede görüntülenir.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama

Parçalı çok kiracılı bir model, bir veritabanında yeni bir kiracı sağlamak için diğer kiracılar içerip içermediğini seçmek için kendi veritabanındaki Kiracı sağlamak için ya da sağlar. Bir kiracı kendi diğer kiracıların verileri yalıtılmış verilerini kalmaktan fayda sağlar. Yalıtım, performans bu kiracının başka kiracılar bağımsız olarak yönetmenize olanak sağlar. Ayrıca, verileri yalıtılmış Kiracı için önceki bir duruma geri yüklemek daha kolay olur. Çok Kiracı veritabanı ücretsiz deneme veya normal müşteriler ve premium müşteriler bulunan tek veritabanlarını koymak isteyebilirsiniz. Çok sayıda her yalnızca bir kiracı içeren veritabanları oluşturursanız, bunları kaynak maliyetleri en iyi duruma getirmek için bir esnek havuz tüm topluca yönetebilirsiniz.  

Şimdi biz başka bir kiracı, bu süre, kendi veritabanındaki sağlayın.

1. İçinde... \\Öğrenme modülleri\\ProvisionTenants\\*Demo ProvisionTenants.ps1*, değişiklik *$TenantName* için **Salix Salsa**,  *$VenueType* için **dance** ve *$Scenario* için **2**.

2. Tuşuna **F5** betiği yeniden çalıştırmak için.
    - Bu F5 tuşuna ayrı bir veritabanında yeni Kiracı sağlar. Veritabanı ve Kiracı katalogda kaydedilir. Ardından tarayıcı Kiracı olayları sayfası açılır.

   ![Salix Salsa olayları sayfası](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Sayfanın alt kısmına kaydırın. Var. başlığında Kiracı verilerin depolandığı veritabanı adı bakın.

3. Olay hub'ı yenileyin ve yeni kiracıları şimdi listede görünür.



## <a name="explore-the-servers-and-tenant-databases"></a>Sunucuları araştırın ve Kiracı veritabanları

Şimdi biz dağıtılan kaynakların bazıları bakın:

1. İçinde [Azure portal](http://portal.azure.com), kaynak grupları listesine göz atın. Uygulama dağıtıldığında, oluşturduğunuz kaynak grubunu açın.

   ![kaynak grubu](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Tıklatın **katalog mt&lt;kullanıcı&gt;**  sunucu. Katalog sunucusu adlı iki veritabanı içerdiğinden *tenantcatalog* ve *basetenantdb*. *Basetenantdb* veritabanı bir boş şablonu veritabanıdır. Bu yeni bir kiracı veritabanı oluşturmak için çok sayıda Kiracı veya yalnızca bir kiracı için kullanılan olup olmadığını kopyalanır.

   ![katalog sunucusu seçeneğine tıklayın](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Geri dönerek seçin ve kaynak grubu *tenants1 mt* Kiracı veritabanlarını barındıran sunucu.
    - Özgün üç kiracılar yanı sıra, eklediğiniz ilk Kiracı depolanan çok Kiracı veritabanı tenants1 veritabanıdır. 50 DTU standart bir veritabanı olarak yapılandırılır.
    - **Salixsalsa** veritabanı Salix Salsa dance salonundan yalnızca kendi Kiracı olarak tutar. Varsayılan olarak 50 Dtu'ya sahip Standard edition veritabanı olarak yapılandırılır.

   ![kiracılar sunucu](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)


## <a name="monitor-the-performance-of-the-database"></a>Veritabanının performansını izleme

Yük Oluşturucu için birkaç dakika çalışır, bazı izleme kapasiteleri portalda yerleşik veritabanı bakarak başlatmak yeterli telemetri yok.

1. Gözat **tenants1 mt&lt;kullanıcı&gt;**  sunucusu ve tıklatın **tenants1** dört kiracılar içerdiği veritabanı için kaynak kullanımını görüntülemek için. Her bir kiracı durumlarıyla ağır bir yük yük Oluşturucu gelen tabidir:

   ![İzleyici tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   DTU kullanımı grafik sorunsuz şekilde çok Kiracı veritabanı arasında çok sayıda Kiracı öngörülemeyen bir iş yükünü nasıl desteklediğini göstermektedir. Bu durumda, yük Oluşturucu yaklaşık 30 Dtu'lar durumlarıyla yükü her kiracıya uygulanıyor. Bu yükleme için % 60 kullanımı 50 DTU veritabanı karşılık gelir. % 60 aşan yükselmeleri aynı anda birden fazla Kiracı uygulanmakta yük sonucudur.

2. Gözat **tenants1 mt&lt;kullanıcı&gt;**  sunucusu ve tıklatın **salixsalsa** yalnızca bir kiracı içeren bu veritabanında kaynak kullanımı görüntülemek için veritabanı.

   ![salixsalsa veritabanı](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Yük oluşturucu bağımsız olarak hangi veritabanı her Kiracı bulunduğu her bir kiracı benzer yük uygulanıyor. Yalnızca bir kiracı ile **salixsalsa** veritabanı, veritabanı bir çok daha yüksek yük veritabanından ile birkaç kiracılar dayanabilir görebilirsiniz. 

> [!NOTE]
> Yük Oluşturucu tarafından oluşturulan yükleri yalnızca gösterim amacıyla ' dir.  Gerçek iş yükü desenleri uygulamanızda çok Kiracı ve Kiracı tek veritabanları ve bir çok kiracılı veritabanında barındırabilir kiracılar numaraları için ayrılmış kaynakları bağlıdır.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]

> - Wingtip biletleri SaaS çok Kiracı veritabanı uygulaması dağıtma
> - Sunucular ve uygulaması olun veritabanları hakkında
> - *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> - Çok Kiracı veritabanı ve tek Kiracı veritabanı yeni kiracılar sağlamasını yapma.
> - Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> - İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Şimdi deneyin [sağlama kiracılar Öğreticisi](sql-database-saas-tutorial-provision-and-catalog.md).



## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](https://docs.microsoft.com/azure/sql-database/sql-database-design-patterns-multi-tenancy-saas-applications)








<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: http://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/





<!--  Image references.

[image-deploy-to-azure-blue-48d]: http://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."

-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png

