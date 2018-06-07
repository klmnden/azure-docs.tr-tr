---
title: Azure SQL veritabanı kullanan bir parçalı çok Kiracı veritabanı SaaS uygulaması dağıtma | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı kullanarak SaaS desenleri gösteren parçalı Wingtip biletleri SaaS çok Kiracı veritabanı uygulama, keşfedin.
keywords: sql veritabanı öğreticisi
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.workload: data-management
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: genemi
ms.openlocfilehash: ac53443140b792d01147cdf22b81d0e6658fa429
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34646465"
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application-that-uses-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı kullanan parçalı bir çok kiracılı uygulama keşfedin

Bu öğreticide dağıtın ve Wingtip biletleri adlı örnek bir çok kiracılı SaaS uygulama keşfedin. Wingtip biletleri uygulama SaaS senaryoları uyarlamasını basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Bu uygulama Wingtip biletleri uygulamanın parçalı çok Kiracı veritabanı desen kullanır. Parçalama tarafından Kiracı tanımlayıcısıdır. Kiracı veri Kiracı tanımlayıcı değerlerini göre belirli bir veritabanına dağıtılır. 

Bu veritabanı deseni her parça veya veritabanında bir veya daha fazla Kiracı depolamanıza olanak sağlar. Birden çok kiracılar tarafından paylaşılan her veritabanı sağlayarak için en düşük maliyeti en iyi duruma getirebilirsiniz. Veya, yalıtım için yalnızca bir kiracı depolamak her veritabanı sağlayarak en iyi duruma getirebilirsiniz. En iyi duruma getirme tercih ettiğiniz bağımsız olarak her belirli bir kiracı için yapılabilir. Tercih ettiğiniz Kiracı ilk depolanan ya da daha sonra fikrinizi değiştirirseniz yapılabilir. Uygulama ya da düzgün şekilde çalışmak üzere tasarlanmıştır.

#### <a name="app-deploys-quickly"></a>Uygulamayı hızlı bir şekilde dağıtır

Uygulama Azure bulutunda çalışır ve Azure SQL veritabanı kullanır. Aşağıdaki dağıtım mavi bölümde **Azure'a Dağıt** düğmesi. Düğmeye basıldığında uygulaması Azure aboneliğinize beş dakika içinde tam olarak dağıtılır. Tek tek uygulama bileşenleri ile çalışmak için tam erişime sahip.

Uygulama verileri üç örnek kiracılar için dağıtılır. Kiracılar birlikte bir çok kiracılı veritabanında depolanır.

C# ve PowerShell kaynak kodu herkes için Wingtip anahtarlarından indirebilirsiniz [GitHub deposunu][link-github-wingtip-multitenantdb-55g].

#### <a name="learn-in-this-tutorial"></a>Bu öğreticide öğrenin

> [!div class="checklist"]
> - Wingtip biletleri SaaS uygulamasının nasıl.
> - Uygulama kaynak koduna ve yönetim komut dosyaları nereden.
> - Sunucular ve veritabanları hakkında uygulaması olun.
> - Kiracıların kendi veri ile nasıl eşlendiğini *katalog*.
> - Yeni bir kiracı sağlamasını yapma.
> - Uygulamasında Kiracı etkinliğini izlemek nasıl.

İlgili eğitim serileri kullanılabilir bu ilk dağıtım sırasında oluşturun. Aralık, SaaS tasarım ve yönetim desenleri öğreticileri keşfedin. Şu öğreticileri çalışırken, adım farklı SaaS desenleri nasıl uygulandığını görmek için sağlanan betikler için önerilir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son Azure PowerShell yüklü. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Wingtip dağıtmak biletleri uygulama

#### <a name="plan-the-names"></a>Adları planlama

Bu bölüm, aşağıdaki adımlarda, sağladığınız bir *kullanıcı* kaynak adları genel benzersiz olduğundan emin olmak için kullanılan değeri ve için bir ad *kaynak grubu* bir dağıtım tarafından oluşturulan tüm kaynaklar içeriyor uygulama. Adlı bir kişi için *Ann Finley*, öneririz:
- *Kullanıcı:* **af1***(kendi baş harflerini artı bir sayı. İkinci kez uygulama dağıttığınızda farklı bir değer (örneğin af2) kullanın.)*
- *Kaynak grubu:* **wingtip mt af1** *(wingtip mt gösterir parçalı çok kiracılı uygulama budur. Kullanıcı adı af1 ekleme içerdiği kaynakların adları ile kaynak grubu adı hatalarla ilintilidir.)*

Adlarınızı şimdi seçin ve not edin. 

#### <a name="steps"></a>Adımlar

1. Aşağıdaki mavi tıklatın **Azure'a Dağıt** düğmesi.
    - Wingtip biletleri SaaS dağıtım şablonu ile Azure Portalı'nı açar.

    [![Düğme için Azure'a dağıtın.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

1. Dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bu Tanıtım için tüm önceden var olan kaynak grupları, sunucular veya havuzları kullanmayın. Bunun yerine, seçin **yeni bir kaynak grubu oluşturma**. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.
    > Bu uygulama ya da oluşturur, herhangi bir kaynağa üretim için kullanmayın. Kimlik doğrulama ve sunucu güvenlik duvarı ayarları, bazı yönlerini tanıtım kolaylaştırmak için uygulamada bilerek güvenli değil.

    - İçin **kaynak grubu** - seçin **Yeni Oluştur**ve ardından sağlayan bir **adı** (büyük küçük harf duyarlı) kaynak grubu için.
        - Seçin bir **konumu** aşağı açılan listeden.
    - İçin **kullanıcı** -kısa seçmenizi öneririz **kullanıcı** değeri.

1. **Uygulamayı dağıtın**.

    - Hüküm ve koşulları kabul için tıklatın.
    - **Satın al**’a tıklayın.

1. Dağıtım durumunu izlemek tıklayarak **bildirimleri**, arama kutusunun sağındaki zil simgesine olduğu. Wingtip uygulama dağıtma yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Karşıdan yükleme ve yönetim komut dosyaları Engellemeyi Kaldır

Uygulama dağıtımı sırasında uygulama kaynak kodu ve yönetim komut dosyaları indirin.

> [!NOTE]
> ZIP dosyaları bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken, .zip dosyasını ayıklanıyor önce engellemesini kaldırmak için aşağıdaki adımları kullanın. .Zip dosyasını kaldırma tarafından komut dosyalarını çalıştırmak için izin verildiğinden emin olun.

1. Gözat [WingtipTicketsSaaS MultiTenantDb GitHub deposuna](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Tıklatın **Kopyala veya indir**.
3. Tıklatın **ZIP'i indir** ve dosyayı kaydedin.
4. Sağ **WingtipTicketsSaaS MultiTenantDb master.zip** dosya ve seçin **özellikleri**.
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**, tıklatıp **Uygula**.
6. **Tamam**’a tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları bulunan *... \\WingtipTicketsSaaS MultiTenantDb ana\\öğrenme modülleri\\*  klasör.

## <a name="update-the-configuration-file-for-this-deployment"></a>Bu dağıtım için yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce ayarlayın *kaynak grubu* ve *kullanıcı* değerler **UserConfig.psm1**. Dağıtım sırasında ayarladığınız aynı değerlere bu değişkenleri ayarlayın.

1. Aç... \\Modülleri öğrenme\\*UserConfig.psm1* içinde *PowerShell ISE*.
2. Güncelleştirme *ResourceGroupName* ve *adı* (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin.

Doğru önemlidir bu dosyasında ayarlanan değerler tüm betikler tarafından kullanılır. Uygulama yeniden dağıtın, kullanıcı ve kaynak grubu için farklı değerler seçmeniz gerekir. Ardından UserConfig.psm1 dosyasını yeni değerlerle yeniden güncelleştirin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Wingtip uygulamada, kiracılar görebildikleri ' dir. Bir salonundan birlikte hall, spor kulübü veya olayları barındıran herhangi bir yere olabilir. Görebildikleri Wingtip içinde müşteri olarak kaydetmek ve her salonundan için bir kiracı tanımlayıcı oluşturulur. Ortak olaylar bilet satın alabileceğiniz şekilde her salonundan Wingtip, kendi Yaklaşan olayları listeler.

Bunların olaylarını listelemek ve bilet satabilir için kişiselleştirilmiş web uygulaması her yerini alır. Her web uygulaması bağımsız ve diğer kiracıdan yalıtılmış ' dir. Dahili olarak Azure SQL veritabanı'nda, her veri her Kiracı için varsayılan olarak parçalı bir çok kiracılı veritabanına depolanır. Tüm verileri Kiracı tanımlayıcısı ile etiketlenir.

Merkezi bir **olay hub'ı** Web sayfası kendi dağıtımınıza içindeki kiracıların bağlantıların listesini sağlar. Denemek için aşağıdaki adımları kullanın **olay hub'ı** Web sayfası ve tek tek web uygulaması:

1. Açık **olay hub'ı** web tarayıcınızda:
    - http://events.wingtip-mt.&lt; Kullanıcı&gt;. trafficmanager.net &nbsp; *(Değiştir &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.)*

    ![olay hub’ı](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. **Olay Hub’ında** **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Gelen istekleri dağıtımını denetlemek için Wingtip uygulamanın kullandığı [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Her bir kiracı için olayları sayfası, URL'de Kiracı adını içerir. Her URL Ayrıca belirli, kullanıcı değeri içerir. Her URL, aşağıdaki adımları kullanarak gösterilen biçimde obeys:

- http://events.wingtip-mt.&lt; Kullanıcı&gt;.trafficmanager.net/*fabrikamjazzclub*

1. Olayları uygulama URL'den Kiracı adı ayrıştırır. Kiracı adı *fabrikamjazzclub* önceki örnek URL.
2. Uygulama Kataloğu'nu kullanarak erişmek için bir anahtar oluşturmak için Kiracı adı sonra karmaları [parça eşleme Yönetim](sql-database-elastic-scale-shard-map-management.md).
3. Uygulama Kataloğu'nda anahtar bulur ve karşılık gelen kiracının veritabanının konumunu alır.
4. Uygulama konum bilgilerini bulmak ve Kiracı için tüm verileri içeren bir veritabanına erişmek için kullanır.

#### <a name="events-hub"></a>Olay hub'ı

1. **Olay hub'ı** kataloğu ve bunların görebildikleri kayıtlı tüm kiracılar listeler.
2. **Olay hub'ı** URL'leri oluşturmak için her eşleme ile ilişkili kiracının adını almak için katalogda genişletilmiş meta verilerini kullanır.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturduğunuz [bir şirketin internet etki alanını işaret](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi profili için.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Uygulamanın dağıtıldığı, şimdi uygulamaya koyun! *Demo LoadGenerator* PowerShell komut dosyasını her bir kiracı için çalışan bir iş yükünü başlatır. Birçok SaaS uygulamalarını gerçek Yük genellikle durumlarıyla ve tahmin edilemez. Bu tür yük benzetimini yapmak için tüm kiracılar arasında dağıtılmış yük üreteci oluşturur. Yük rastgele aralıklarla gerçekleşen her bir kiracı üzerinde rastgele WINS'e içerir. Yük izleme önce en az üç veya dört dakika çalıştırmak Oluşturucu izin vermek en iyisidir çıkmaya, yük düzeni için birkaç dakika sürer.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\yardımcı programları\\*Demo LoadGenerator.ps1* komut dosyası.
2. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

*Demo LoadGenerator.ps1* komut dosyası yükleme Oluşturucu çalıştığı başka bir PowerShell oturumu açar. Arka plan yük oluşturma işleri, her bir kiracı için bir tane çağıran bir ön plan görevi olarak bu oturumda yük Oluşturucu çalışır.

Ön plan görev başlatıldıktan sonra iş çağrılırken bir durumda kalır. Görev sonradan sağlanan yeni kiracılar için ek arka plan işleri başlatır.

PowerShell oturumu kapatma tüm işleri durdurur.

Farklı parametre değerleri kullanmak için yük Oluşturucu oturumu yeniden isteyebilirsiniz. Öyleyse, PowerShell oluşturma oturumu kapatın ve sonra yeniden *Demo LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Parçalı veritabanına yeni bir kiracı sağlama

İlk üç örnek kiracılar içeren *Tenants1* veritabanı. Şirketinizdeki başka bir kiracı oluşturmak ve dağıtılmış uygulamanın kendi etkileri gözlemleyin. Bu adımda, yeni bir kiracı oluşturmak için bir tuşa basın:

1. Aç... \\Modülleri öğrenme\\sağlamak ve Katalog\\*Demo ProvisionTenants.ps1* içinde *PowerShell ISE*.
2. Tuşuna **F5** (değil **F8**) komut dosyasını çalıştırmak için (şu an için varsayılan değerleri bırakın).

   > [!NOTE]
   > Yalnızca tuşlarına basarak PowerShell komut dosyalarını çalıştırması gereken **F5** basarak değil anahtar **F8** komut dosyası, seçilen bir parçası çalıştırmak için. Sorun **F8** olan *$PSScriptRoot* değişkeni değerlendirilmez. Bu değişken klasörleri gitmek için birçok komut dosyaları tarafından başka betikleri çağırma ya da modülleri içe gereklidir.

Yeni kırmızı Akçaağaç yarış Kiracı eklenen *Tenants1* veritabanı ve kataloğa kayıtlı. Yeni Kiracı bilet satış **olayları** site tarayıcınızda açar:

![Yeni kiracı](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Yenileme **olay hub'ı**, ve yeni Kiracı şimdi listede görüntülenir.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama

Parçalı çok kiracılı bir model diğer kiracılar içeren bir veritabanına ya da kendi bir veritabanına yeni bir kiracı sağlayacak seçmesine izin verir. Kendi veritabanında yalıtılmış Kiracı aşağıdaki yararları özgürlüğüne:
- Kiracının veritabanının performansını gereksinimlerine göre diğer kiracılar bozmasına gerek kalmadan yönetilebilir.
- Diğer bir kiracılar etkilenecek çünkü gerekiyorsa, veritabanını önceki bir noktaya, zaman içindeki geri yüklenebilir.

Ücretsiz deneme müşteriler veya ekonomi müşteriler, çok kiracılı veritabanlarına koymak isteyebilirsiniz. Her premium Kiracı kendi adanmış veritabanı koyabilirsiniz. Çok sayıda yalnızca bir kiracı içeren veritabanları oluşturursanız, bunları kaynak maliyetleri en iyi duruma getirmek için bir esnek havuz tüm topluca yönetebilirsiniz.

Ardından, biz başka bir kiracı, bu süre, kendi veritabanındaki sağlayın:

1. İçinde... \\Öğrenme modülleri\\sağlamak ve Katalog\\*Demo ProvisionTenants.ps1*, değişiklik *$TenantName* için **Salix Salsa**, *$VenueType* için **dance** ve *$Scenario* için **2**.

2. Tuşuna **F5** betiği yeniden çalıştırmak için.
    - Bu **F5** basın ayrı bir veritabanında yeni Kiracı sağlar. Veritabanı ve Kiracı katalogda kaydedilir. Ardından tarayıcı Kiracı olayları sayfası açılır.

   ![Salix Salsa olayları sayfası](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Sayfanın alt kısmına kaydırın. Var. başlığında Kiracı verilerin depolandığı veritabanı adı bakın.

3. Yenileme **olay hub'ı** ve iki yeni kiracılar şimdi listede görünür.

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

Yük Oluşturucu için birkaç dakika çalışır, izleme kapasiteleri Azure portalda yerleşik veritabanı bakmak yeterli telemetri yok.

1. Gözat **tenants1 mt&lt;kullanıcı&gt;**  sunucusu ve tıklatın **tenants1** dört kiracılar içerdiği veritabanı için kaynak kullanımını görüntülemek için. Her bir kiracı durumlarıyla ağır bir yük yük Oluşturucu gelen tabidir:

   ![İzleyici tenants1](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   DTU kullanımı grafik sorunsuz şekilde çok Kiracı veritabanı arasında çok sayıda Kiracı öngörülemeyen bir iş yükünü nasıl desteklediğini göstermektedir. Bu durumda, yük Oluşturucu yaklaşık 30 Dtu'lar durumlarıyla yükü her kiracıya uygulanıyor. Bu yükleme için % 60 kullanımı 50 DTU veritabanı karşılık gelir. % 60 aşan yükselmeleri aynı anda birden fazla Kiracı uygulanmakta yük sonucudur.

2. Gözat **tenants1 mt&lt;kullanıcı&gt;**  sunucusu ve tıklatın **salixsalsa** veritabanı. Kaynak kullanımı yalnızca bir kiracı içeren bu veritabanında görebilirsiniz.

   ![salixsalsa veritabanı](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Yük oluşturucu bağımsız olarak hangi veritabanı her Kiracı bulunduğu her bir kiracı benzer yük uygulanıyor. Yalnızca bir kiracı ile **salixsalsa** veritabanı, veritabanı bir çok daha yüksek yük veritabanından ile birkaç kiracılar dayanabilir görebilirsiniz. 

#### <a name="resource-allocations-vary-by-workload"></a>Kaynak ayırma iş yüküne göre farklılık gösterir.

Bazen çok Kiracı veritabanı tek Kiracı veritabanı, daha iyi performans için daha fazla kaynak gerektiren, ancak her zaman. En iyi Tahsis edilen kaynakların sisteminizde kiracılar için belirli bir iş yükünü özelliklerine bağlıdır.

Yük üreteci komut dosyası tarafından oluşturulan iş yükleri, yalnızca gösterme amaçlıdır.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [tasarım desenleri çok kiracılı SaaS uygulamaları için](saas-tenancy-app-design-patterns.md).

- Esnek havuzları hakkında bilgi için bkz:
    - [Esnek havuz yönetmek ve birden çok Azure SQL veritabanı ölçekleme Yardım](sql-database-elastic-pool.md)
    - [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip biletleri SaaS çok Kiracı veritabanı uygulamasının nasıl.
> - Sunucular ve veritabanları hakkında uygulaması olun.
> - Kiracıların kendi verilerle eşlendiğinden *katalog*.
> - Çok Kiracı veritabanı ve tek Kiracı veritabanı yeni kiracılar sağlamasını yapma.
> - Kiracı etkinliğini izlemek için havuzu kullanımı görüntülemek nasıl.
> - Nasıl ilgili faturalama durdurmak için örnek kaynaklar silinir.

Şimdi deneyin [sağlamak ve kataloğu Öğreticisi](sql-database-saas-tutorial-provision-and-catalog.md).


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

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Azure'a dağıtmak için düğmesi."

