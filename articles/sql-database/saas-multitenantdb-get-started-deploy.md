---
title: Azure SQL veritabanı kullanan çok kiracılı veritabanı parçalı SaaS uygulamasını dağıtma | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı'nı kullanarak, SaaS düzenlerinin gösteren parçalı Wingtip bilet SaaS çok kiracılı veritabanı uygulama keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: billgib, stein
manager: craigg
ms.date: 10/16/2018
ms.openlocfilehash: 350e67f5a1e7e1eab7abe27a6ca851ed2420af84
ms.sourcegitcommit: 59fd8dc19fab17e846db5b9e262a25e1530e96f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65978537"
---
# <a name="deploy-and-explore-a-sharded-multi-tenant-application"></a>Parçalı bir çok kiracılı uygulamasını dağıtma ve keşfetme

Bu öğreticide, dağıtın ve Wingtip bilet adlandırılmış bir örnek çok kiracılı SaaS uygulaması keşfedin. Wingtip bilet uygulaması, Azure SQL veritabanı'nın SaaS senaryolarını uygulamasını basitleştirmek özellikleri göstermek için tasarlanmıştır.

Wingtip bilet uygulaması, bu uygulaması, parçalı çok kiracılı veritabanı deseni kullanır. Parçalama Kiracı tarafından tanımlayıcısıdır. Kiracı verilerini Kiracı tanımlayıcısı değerlere göre belirli bir veritabanına dağıtılır. 

Bu veritabanı Düzen her parça ya da veritabanı veya daha fazla Kiracı depolamanıza olanak tanır. Birden fazla Kiracı tarafından paylaşılacak her bir veritabanı sağlayarak en düşük maliyeti en iyi duruma getirebilirsiniz. Veya her bir veritabanı yalnızca tek bir kiracı depolamak sağlayarak yalıtımı için iyileştirebilirsiniz. En iyi duruma getirme seçtiğiniz belirli her Kiracı için bağımsız olarak yapılabilir. Seçtiğiniz Kiracı ilk depolanan ya da daha sonra fikrinizi değiştirirseniz yapılabilir. Uygulama ya da düzgün şekilde çalışmak üzere tasarlanmıştır.

## <a name="app-deploys-quickly"></a>Uygulama hızlı bir şekilde dağıtır

Uygulama, Azure bulutunda çalışır ve Azure SQL veritabanı kullanır. Aşağıdaki dağıtım bölümde mavi sağlar **azure'a Dağıt** düğmesi. Düğmeye basıldığında, uygulama tam olarak beş dakika içinde Azure aboneliğinize dağıtılır. Tek tek uygulama bileşenleri ile çalışmak için tam erişime sahiptir.

Uygulama verileri için üç örnek Kiracı dağıtılır. Kiracılar, birlikte bir çok kiracılı veritabanında depolanır.

Herkes için Wingtip bilet ' C# ve PowerShell kaynak kodunu indirebilirsiniz [kendi GitHub deposu][link-github-wingtip-multitenantdb-55g].

## <a name="learn-in-this-tutorial"></a>Bu öğreticide bilgi edinin

> [!div class="checklist"]
> - Wingtip bilet SaaS uygulamasını dağıtma
> - Uygulama kaynak kodunu ve yönetim komut dosyaları elde edileceği.
> - Sunucuları ve uygulamayı oluşturan veritabanları hakkında.
> - Kiracıların kendi verilerle nasıl eşlendiğine *Kataloğu*.
> - Yeni Kiracı sağlama öğrenin.
> - Uygulamasında Kiracı etkinliğini izlemeyi öğrenin.

Bir dizi ilgili öğretici kullanılabilir bu ilk dağıtımı temel oluşturun. Aralık, SaaS tasarım ve yönetim düzenlerini öğreticileri keşfedin. Öğreticilerde ilerlerken çalışırken, farklı SaaS düzenlerinin nasıl uygulandığını görmek için sağlanan betikleri adımlayın teşvik edilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

- En son Azure PowerShell yüklü. Ayrıntılar için bkz [Azure PowerShell'i kullanmaya başlama][link-azure-get-started-powershell-41q].

## <a name="deploy-the-wingtip-tickets-app"></a>Dağıtma Wingtip bilet uygulaması

### <a name="plan-the-names"></a>Adları planlama

Bu bölümdeki adımlarda, sağladığınız bir *kullanıcı* Kaynak adlarının benzersiz olduğundan emin olmak için kullanılan değer ve için bir ad *kaynak grubu* bir dağıtım tarafından oluşturulan tüm kaynakları içerir uygulamanın. Adlı bir kişi için *Ann Finley*, öneririz:
- *Kullanıcı:* **af1***(baş harflerini yanı sıra bir sayı.   İkinci kez uygulamayı dağıtma (örneğin af2) farklı bir değer kullanın.)*
- *Kaynak grubu:* **wingtip mt af1** *(wingtip-mt gösterir parçalı çok kiracılı uygulama budur. Kullanıcı adı af1 ekleme adlarıyla içerdiği tüm kaynakları kaynak grubu adını ilişkilendirir.)*

Artık adlarınızı seçin ve not edin. 

### <a name="steps"></a>Adımlar

1. Aşağıdaki mavi tıklayın **azure'a Dağıt** düğmesi.
   - Azure portalı ile Wingtip bilet SaaS dağıtım şablonu açar.

     [![Düğme için Azure'a dağıtın.][image-deploy-to-azure-blue-48d]][link-aka-ms-deploywtp-mtapp-52k]

1. Dağıtım için gerekli parametre değerlerini girin.

    > [!IMPORTANT]
    > Bu Tanıtım için tüm önceden var olan kaynak grupları, sunucuları veya havuzları kullanmayın. Bunun yerine, seçin **yeni bir kaynak grubu oluşturma**. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.
    > Bu uygulamayı ya da oluşturduğu kaynakları üretim için kullanmayın. Kimlik doğrulaması ve sunucu güvenlik duvarı ayarları, bazı yönlerini tanıtım kolaylaştırmak için uygulamada kasıtlı olarak güvenli değil.

    - İçin **kaynak grubu** - seçin **Yeni Oluştur**ve ardından bir **adı** kaynak grubunun (büyük/küçük harfe duyarlı).
        - Seçin bir **konumu** aşağı açılan listeden.
    - İçin **kullanıcı** -kısa seçmenizi öneririz **kullanıcı** değeri.

1. **Uygulamayı dağıtın**.

    - Hüküm ve koşulları kabul etmek için tıklayın.
    - **Satın al**’a tıklayın.

1. Tıklayarak dağıtım durumunu izlemek **bildirimleri**, arama kutusunun sağındaki zil simgesine olduğu. Wingtip uygulamasının dağıtılması yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-multitenantdb-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-management-scripts"></a>Karşıdan yükleme ve yönetim komut dosyaları engelini kaldırma

Uygulama dağıtılırken, uygulama kaynak kodu ve yönetim komut dosyaları indirin.

> [!NOTE]
> Zip dosyaları bir dış kaynaktan indirilip ayıklanan zaman yürütülebilir içeriği (betikler, DLL'ler) Windows tarafından engelleniyor olabilir. Komut dosyaları zip dosyasından ayıklanırken, .zip dosyasını ayıklamadan önce engelini kaldırmak için aşağıdaki adımları kullanın. .Zip dosyasını engellemesinin kaldırılması betikleri çalıştırmak için izin verildiğinden emin olun.

1. Gözat [WingtipTicketsSaaS MultiTenantDb GitHub deposunu](https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb).
2. Tıklayın **Kopyala veya indir**.
3. Tıklayın **ZIP'i indir** ve dosyayı kaydedin.
4. Sağ **WingtipTicketsSaaS MultiTenantDb master.zip** seçin ve dosya **özellikleri**.
5. Üzerinde **genel** sekmesinde **Engellemeyi Kaldır**, tıklatıp **Uygula**.
6. **Tamam**'ı tıklatın.
7. Dosyaları ayıklayın.

Betikleri bulunan *... \\WingtipTicketsSaaS MultiTenantDb ana\\öğrenme modülleri\\*  klasör.

## <a name="update-the-configuration-file-for-this-deployment"></a>Bu dağıtım için yapılandırma dosyasını güncelleştirme

Komut dosyalarını çalıştırmadan önce ayarlayın *kaynak grubu* ve *kullanıcı* değerler **UserConfig.psm1**. Bu değişkenler, dağıtım sırasında ayarladığınız aynı değerlere ayarlayın.

1. Aç... \\Öğrenme modülleri\\*UserConfig.psm1* içinde *PowerShell ISE*.
2. Güncelleştirme *ResourceGroupName* ve *adı* dağıtımınız (10 ve 11 yalnızca satırlarındaki) için belirli değerlere sahip.
3. Değişiklikleri kaydedin.

Ayarların doğruluğunu önemli, bu nedenle bu dosyasında ayarlanan değerlerle tüm betikler tarafından kullanılır. Uygulamayı yeniden, kullanıcı ve kaynak grubu için farklı değerler seçmeniz gerekir. Ardından UserConfig.psm1 dosyasını yeni değerlerle yeniden güncelleştirin.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Wingtip uygulamasında kiracılar venues ' dir. Bir mekan Konser Salonu, bir spor kulübü veya olayları barındıran herhangi bir konumda olabilir. Mekanlar Wingtip müşteriler olarak kaydedin ve Kiracı tanımlayıcısı, her mekanın için oluşturulur. Genel olaylar bilet satın alabilirsiniz, böylece her mekan kendi Wingtip, gelecekteki olayları listeler.

Her mekan, etkinliklerini ve bilet satmak için kişiselleştirilmiş web uygulaması alır. Her web uygulaması, bağımsız ve diğer kiracıdan yalıtılmış ' dir. Dahili olarak Azure SQL veritabanı'nda, her her kiracının verileri varsayılan olarak parçalı bir çok kiracılı veritabanında depolanır. Tüm veri Kiracı tanımlayıcısı ile etiketlenir.

Bir merkezi **olay hub'ı** Web sayfası, belirli dağıtım içindeki kiracıların bağlantıların listesini sağlar. Denemek için aşağıdaki adımları kullanın **olay hub'ı** Web sayfası ve tek tek web uygulaması:

1. Açık **olay hub'ı** web tarayıcınızda:
   - http://events.wingtip-mt.&lt; Kullanıcı&gt;. trafficmanager.net &nbsp; *(Değiştir &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.)*

     ![olay hub’ı](media/saas-multitenantdb-get-started-deploy/events-hub.png)

2. **Olay Hub’ında** **Fabrikam Caz Kulübü**’ne tıklayın.

   ![Olaylar](./media/saas-multitenantdb-get-started-deploy/fabrikam.png)

### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Gelen isteklerin dağıtımını denetlemek için Wingtip uygulama kullanan [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md). Her Kiracı için olayları sayfası, URL'de Kiracı adını içerir. Her URL, ayrıca, belirli bir kullanıcı değerini içerir. Her URL, aşağıdaki adımları kullanarak, gösterilen biçimi ilişkiden:

- http://events.wingtip-mt.&lt;user&gt;.trafficmanager.net/*fabrikamjazzclub*

1. Etkinlikler uygulaması, Kiracı adını URL'den ayrıştırır. Kiracı adı *fabrikamjazzclub* önceki örnek URL.
2. Uygulamayı kullanarak bir kataloğa erişim sağlayacak anahtar oluşturmak üzere Kiracı adından sonra karma [parça eşleme Yönetimi](sql-database-elastic-scale-shard-map-management.md).
3. Uygulama Kataloğu'nda anahtar bulur ve karşılık gelen kiracının veritabanı konumunu alır.
4. Uygulama konum bilgilerini bulmak ve Kiracı için tüm verileri içeren bir veritabanına erişmek için kullanır.

### <a name="events-hub"></a>Olay hub'ı

1. **Olay hub'ı** katalog ve bunların venues kayıtlı olan tüm kiracılar listeler.
2. **Olay hub'ı** URL'leri oluşturmak için her eşleme ile ilişkili kiracının adını almak için katalogdaki genişletilmiş meta verileri kullanır.

Bir üretim ortamında, genellikle bir CNAME DNS kaydı için oluşturduğunuz [bir şirketin internet etki alanını işaret](../traffic-manager/traffic-manager-point-internet-domain.md) için traffic manager profili.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Şimdi uygulama dağıtıldıktan sonra bu çalışmaya başlayabiliriz! *Tanıtım LoadGenerator* PowerShell komut dosyasını çalıştıran her Kiracı için bir iş yükü başlatır. Birçok SaaS uygulamalarına gerçek Yük genellikle düzensiz ve tahmin edilemezdir. Bu tür iş yükünün benzetimini yapmak için tüm kiracılar genelinde dağıtılmış bir yük Oluşturucu oluşturur. Yük rastgele aralıklarla gerçekleşen her bir kiracı üzerinde rastgele artışları içerir. Yükü izleme önce en az üç veya dört dakika çalıştırın oluşturucunun en iyisidir çıkması yük düzeni için birkaç dakika sürer.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\yardımcı programları\\*tanıtım LoadGenerator.ps1* betiği.
2. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için **F5**’e basın (şimdilik varsayılan parametre değerlerini bırakın).

*Tanıtım LoadGenerator.ps1* betik yük Oluşturucu çalıştığı başka bir PowerShell oturumu açar. Yük Oluşturucu, arka plan yük oluşturma işleri, her Kiracı için bir çağıran bir ön plan görevi olarak bu oturumda çalıştırır.

Ön plan görev başlatıldıktan sonra bir işlemi çağırma durumda kalır. Görev, daha sonra sağlanan yeni kiracılar için ek arka plan işleri başlatır.

PowerShell oturumu kapatma tüm işleri durdurur.

Farklı parametre değerleri kullanılacak yük Oluşturucu oturumlarını yeniden başlatmanız isteyebilirsiniz. Bu durumda, PowerShell oluşturma oturumu kapatın ve yeniden *tanıtım LoadGenerator.ps1*.

## <a name="provision-a-new-tenant-into-the-sharded-database"></a>Parçalı veritabanına yeni bir kiracı sağlama

İlk dağıtım üç örnek Kiracı içerir *Tenants1* veritabanı. Şimdi başka bir kiracı oluşturur ve dağıtılan uygulama üzerindeki etkileri gözlemleyin. Bu adımda, yeni bir kiracı oluşturmak için bir tuşa basın:

1. Aç... \\Öğrenme modülleri\\sağlama ve kataloğa kaydetme\\*tanıtım ProvisionTenants.ps1* içinde *PowerShell ISE*.
2. Tuşuna **F5** (değil **F8**) betiği çalıştırmak için (şimdilik varsayılan değerlerini bırakın).

   > [!NOTE]
   > Yalnızca tuşlarına basarak PowerShell betikleri çalıştırmanız gerekir **F5** basarak değil, anahtar **F8** seçilen bir kod parçası çalıştırmak için. Sorun **F8** olan *$PSScriptRoot* değişken değerlendirilmez. Bu değişken klasörleri gitmek için birçok betikler tarafından diğer betikleri çağırma veya modülleri içeri aktarmak için gerekli.

Yeni Kiracı Red Maple yarışı eklenir *Tenants1* veritabanı ve kataloğa kayıtlı. Yeni kiracının bilet satış **olayları** sitesi, tarayıcınızda açılır:

![Yeni kiracı](./media/saas-multitenantdb-get-started-deploy/red-maple-racing.png)

Yenileme **olay hub'ı**, ve yeni kiracının listede görünür.

## <a name="provision-a-new-tenant-in-its-own-database"></a>Kendi veritabanında yeni bir kiracı sağlama

Parçalı çok kiracılı model, diğer kiracılar içeren bir veritabanına veya kendi veritabanına yeni bir kiracı sağlama konusundaki kararı olanak tanır. Kendi veritabanında yalıtılmış Kiracı, aşağıdaki faydaları ölçeklenebilme:

- Diğer kiracıların ihtiyaçlarını tehlikeye gerek kalmadan kiracının veritabanının performansını yönetilebilir.
- Diğer bir kiracı etkilenebileceğini çünkü gerekirse, veritabanı önceki bir noktaya zaman içinde geri yüklenebilir.

Ücretsiz deneme müşterileri veya ekonomi müşterileri, çok kiracılı veritabanlarına koymak isteyebilirsiniz. Her premium kiracıya adanmış kendi veritabanında yerleştirebilirsiniz. Yalnızca tek bir kiracı içeren veritabanları çok sayıda oluşturursanız, bunları kaynak maliyetleri iyileştirmek için bir elastik havuz tüm topluca yönetebilirsiniz.

Ardından, biz başka bir kiracı, bu süre içinde kendi veritabanı sağlayın:

1. İçinde... \\Öğrenme modülleri\\sağlama ve kataloğa kaydetme\\*tanıtım ProvisionTenants.ps1*, değişiklik *$TenantName* için **Salix Salsa**, *$VenueType* için **dansını** ve *$Scenario* için **2**.

2. Tuşuna **F5** betiğini yeniden çalıştırmak için.
    - Bu **F5** basın, yeni kiracıya ayrı bir veritabanı sağlar. Veritabanı ve Kiracı kataloğa kaydedilir. Ardından tarayıcı kiracının olayları sayfası açılır.

   ![Salix Salsa olayları sayfası](./media/saas-multitenantdb-get-started-deploy/salix-salsa.png)

   - Sayfanın alt kısmına kaydırın. Orada başlığında Kiracı verilerini depolandığı veritabanı adı bakın.

3. Yenileme **olay hub'ı** ve iki yeni kiracılar listesinde görünür.

## <a name="explore-the-servers-and-tenant-databases"></a>Sunucular ve Kiracı veritabanlarını öğrenme

Artık dağıtılan kaynaklardan bazıları bakacağız:

1. İçinde [Azure portalında](https://portal.azure.com), kaynak grupları, listeye göz atın. Uygulama dağıtıldığında, oluşturduğunuz kaynak grubunu açın.

   ![kaynak grubu](./media/saas-multitenantdb-get-started-deploy/resource-group.png)

2. Tıklayın **Kataloğu-mt&lt;kullanıcı&gt;**  sunucusu. Katalog sunucusu adlı iki veritabanı içeren *tenantcatalog* ve *basetenantdb*. *Basetenantdb* boş şablonu veritabanı veritabanıdır. Yeni bir kiracı veritabanı oluşturmak için birçok kiracının veya tek bir kiracı için kullanılıp kopyalanır.

   ![katalog sunucusu seçeneğine tıklayın](./media/saas-multitenantdb-get-started-deploy/catalog-server.png)

3. Geri Git seçin ve kaynak grubu *tenants1-mt* Kiracı veritabanlarını barındıran sunucu.
    - Tenants1 veritabanı, özgün üç Kiracı yanı sıra, eklediğiniz ilk Kiracı depolandığı çok kiracılı bir veritabanıdır. 50 DTU standart veritabanı olarak yapılandırılmış.
    - **Salixsalsa** Salix Salsa dansını mekan olarak kendi tek kiracılı veritabanı tutar. Varsayılan olarak 50 Dtu içeren standart sürüm veritabanı olarak yapılandırılır.

   ![kiracılar sunucusu](./media/saas-multitenantdb-get-started-deploy/tenants-server.png)

## <a name="monitor-the-performance-of-the-database"></a>Veritabanı performansını izleme

Yük Oluşturucu birkaç dakikadır çalışıyorsa, yeterli telemetri İzleme özelliklerini Azure portalında oluşturulan veritabanı bakmak kullanılabilir.

1. Gözat **tenants1-mt&lt;kullanıcı&gt;**  sunucusu seçeneğine tıklayıp **tenants1** dört kiracılar olduğu veritabanı için kaynak kullanımını görüntülemek için. Kullanımın ağır bir yük gelen yük Oluşturucu her Kiracı tabidir:

   ![tenants1 izleyin](./media/saas-multitenantdb-get-started-deploy/monitor-tenants1.png)

   DTU kullanımı grafiği düzgün şekilde, çok kiracılı veritabanı birçok kiracılar genelinde öngörülemeyen bir iş yüküne nasıl desteklediğini göstermektedir. Bu durumda, yük Oluşturucu her Kiracı için yaklaşık 30 Dtu'ların ara sıra bir yük uyguluyor. Bu yük 50 DTU veritabanının % 60 kullanımına karşılık gelmektedir. % 60'ı aşan en yüksek sayılar birden fazla Kiracı için aynı anda uygulanan yük sonucudur.

2. Gözat **tenants1-mt&lt;kullanıcı&gt;**  sunucusu seçeneğine tıklayıp **salixsalsa** veritabanı. Kaynak kullanımını tek bir kiracı içeren bu veritabanında görebilirsiniz.

   ![salixsalsa veritabanı](./media/saas-multitenantdb-get-started-deploy/monitor-salix.png)

Yük Oluşturucu her Kiracı bulunduğu veritabanı bağımsız olarak her bir kiracı benzer bir yük uygulanıyor. Tek bir kiracıda ile **salixsalsa** veritabanı, veritabanı veritabanından çok daha yüksek yük ile birden çok kiracıyı dayanabilir görebilirsiniz. 

### <a name="resource-allocations-vary-by-workload"></a>Kaynak ayırmalar iş yüküne göre farklılık gösterir.

Bazen bir çok kiracılı veritabanı bir tek kiracılı veritabanı daha iyi performans için daha fazla kaynak gerektiren ama her zaman kullanılmaz. En uygun kaynakların ayrılması, sisteminizdeki kiracılar için belirli iş yükü özelliklerine bağlıdır.

Yük Oluşturucu betiği tarafından oluşturulan iş yükleri, yalnızca gösterme amaçlıdır.

## <a name="additional-resources"></a>Ek kaynaklar

- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [çok kiracılı SaaS uygulamaları için Tasarım Düzenleri](saas-tenancy-app-design-patterns.md).

- Elastik havuzlar hakkında bilgi edinmek için bkz:

  - [Elastik havuzlar, yönetmenize ve birden çok Azure SQL veritabanını ölçeklendirme Yardım](sql-database-elastic-pool.md)
  - [Azure SQL Veritabanı ile ölçek genişletme](sql-database-elastic-scale-introduction.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip bilet SaaS çok kiracılı veritabanı uygulama dağıtma
> - Sunucuları ve uygulamayı oluşturan veritabanları hakkında.
> - Kiracılar ve verilerinin ile *Kataloğu*.
> - Yeni kiracılar, çok kiracılı veritabanı ile tek kiracılı veritabanı sağlamasını yapma.
> - Nasıl Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme.
> - İlgili faturalandırmayı durdurmak için örnek kaynakları silme yapma.

Şimdi deneyin [sağlama ve kataloğa kaydetme öğreticisinde](sql-database-saas-tutorial-provision-and-catalog.md).


<!--  Link references.

A [series of related tutorials] is available that build upon this initial deployment.
[link-wtp-overivew-jumpto-saas-tutorials-97j]: saas-multitenantdb-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials

-->

[link-aka-ms-deploywtp-mtapp-52k]: https://aka.ms/deploywtp-mtapp


[link-azure-get-started-powershell-41q]: https://docs.microsoft.com/powershell/azure/get-started-azureps

[link-github-wingtip-multitenantdb-55g]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDB/



<!--  Image references.

[image-deploy-to-azure-blue-48d]: https://aka.ms/deploywtp-mtapp "Button for Deploy to Azure."
-->

[image-deploy-to-azure-blue-48d]: media/saas-multitenantdb-get-started-deploy/deploy.png "Azure'a dağıtmaya düğmesi."

