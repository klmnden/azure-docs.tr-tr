---
title: Kiracı başına veritabanı SaaS Öğreticisi - Azure SQL veritabanı | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı'nı kullanarak Kiracı başına veritabanı düzenini ve diğer SaaS düzenlerinin gösteren Wingtip bilet SaaS çok kiracılı uygulama keşfedin.
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MightyPen
ms.author: genemi
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 14f76a716447e09299cfa18d6758245706c7b481
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60556549"
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Kiracı başına veritabanı desen ile SQL veritabanı kullanan çok kiracılı bir SaaS uygulama keşfedin ve dağıtın

Bu öğreticide, dağıtın ve Wingtip bilet SaaS Kiracı başına veritabanı uygulama (Wingtip) keşfedin. Uygulama, birden çok kiracının verileri depolamak için bir kiracı başına veritabanı düzenini kullanır. Uygulama, Azure SQL veritabanı'nın SaaS senaryolarını etkinleştirme basitleştirmek özellikleri göstermek için tasarlanmıştır.

Beş dakika sonra seçtiğiniz **azure'a Dağıt**, çok kiracılı bir SaaS uygulamasına sahip olacaksınız. Uygulama, bulutta çalışan bir SQL veritabanı içerir. Uygulama, her biri kendi veritabanına sahip üç örnek Kiracı ile dağıtılır. Tüm veritabanları, bir SQL elastik havuzuna dağıtılır. Uygulama, Azure aboneliğinize dağıtılır. Size keşfedin ve uygulama bileşenleri tek tek ile çalışmak için tam erişime sahiptir. C# kaynak kodunu ve yönetim komut dosyaları kullanılabilir [WingtipTicketsSaaS DbPerTenant GitHub deposunu][github-wingtip-dpt].

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> - Wingtip SaaS uygulaması dağıtma
> - Uygulama kaynak kodu ve yönetim komut dosyaları elde edileceği.
> - Sunucular, havuzlar ve uygulamayı oluşturan veritabanları hakkında.
> - Kiracıların kendi verilerle nasıl eşlendiğine *Kataloğu*.
> - Yeni Kiracı sağlama öğrenin.
> - Uygulamasında Kiracı etkinliğini izlemeyi öğrenin.

A [dizi ilgili öğretici](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) çeşitli SaaS tasarım ve yönetim düzenlerini keşfetmek sunar. Öğreticiler, bu ilk dağıtımı dışında oluşturun. Öğreticiler kullandığınızda, farklı SaaS düzenlerinin nasıl uygulandığını görmek için sağlanan betikleri inceleyebilirsiniz. Komut dosyaları, SQL veritabanı özellikleri, SaaS uygulamaları geliştirmeye yönelik nasıl basit hale gösterir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için Azure PowerShell'in yüklendiğinden emin olun. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wingtip bilet SaaS uygulamasını dağıtma

### <a name="plan-the-names"></a>Adları planlama

Bu bölümdeki adımlarda, kaynak adları emin olmak için kullanılan bir kullanıcı değeri genel olarak benzersiz sağlar. Ayrıca, bir uygulama dağıtımı tarafından oluşturulan tüm kaynakları içeren kaynak grubu için bir ad de sağlar. Ann Finley adlı kurgusal bir kişi için öneririz:

- **Kullanıcı**: *af1* Ann Finley'nın soyadınızın baş harfleri ile bir rakam oluşur. İkinci kez uygulama dağıttığınızda, farklı bir değer kullanın. Af2 buna bir örnektir.
- **Kaynak grubu**: *wingtip dpt af1* bunun Kiracı başına veritabanı uygulama olduğunu gösterir. Kaynak grubu adı içerdiği tüm kaynakları adları ile ilişkilendirmek için kullanıcı adı af1 ekleyin.

Artık adlarınızı seçin ve not edin.

### <a name="steps"></a>Adımlar

1. Wingtip bilet SaaS Kiracı başına veritabanı dağıtım şablonu Azure Portalı'nda açmak için seçmeniz **azure'a Dağıt**.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

1. Değerleri şablon için gerekli parametreler girin.

    > [!IMPORTANT]
    > Tanıtım amacıyla kasıtlı olarak güvenli olmayan bazı kimlik doğrulama ve sunucu güvenlik duvarı. Yeni bir kaynak grubu oluşturmanızı öneririz. Mevcut kaynak gruplarını, sunucuları veya havuzları kullanmayın. Bu uygulamayı, komut dosyaları veya dağıtılan tüm kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla tamamladığınızda, bu kaynak grubunu silin.

    - **Kaynak grubu**: Seçin **Yeni Oluştur**ve seçtiğiniz benzersiz adı daha önce kaynak grubu için belirtin.
    - **Konum**: Aşağı açılan listeden bir konum seçin.
    - **Kullanıcı**: Daha önce seçtiğiniz kullanıcı adı değerini kullanın.

1. Uygulamayı dağıtın.

    a. Hüküm ve koşulları kabul etmek için bu seçeneği seçin.

    b. **Satın al**'ı seçin.

1. Dağıtım durumunu izlemek üzere seçmek **bildirimleri** (arama kutusunun sağındaki zil simgesi). Wingtip bilet SaaS uygulamasının dağıtılması yaklaşık beş dakika sürer.

   ![Dağıtım başarılı oldu](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>İndirin ve Wingtip bilet yönetim komut dosyaları engelini kaldırma

Uygulama dağıtılır, ancak kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> .Zip dosyaları bir dış kaynaktan indirilip ayıklanan zaman yürütülebilir içeriği (betikler ve dll) Windows tarafından engelleniyor olabilir. Komut dosyalarını ayıklamak önce .zip dosyasını engelini kaldırma adımlarını izleyin. Kaldırma komut dosyalarının çalışmasına izin verilen emin olur.

1. Gözat [WingtipTicketsSaaS DbPerTenant GitHub deposunu][github-wingtip-dpt].
1. **Clone or download**'u (Kopyala veya indir) seçin.
1. Seçin **ZIP'i indir**ve ardından dosyayı kaydedin.
1. Sağ **WingtipTicketsSaaS DbPerTenant master.zip** dosya ve ardından **özellikleri**.
1. Üzerinde **genel** sekmesinde **Engellemeyi Kaldır** > **Uygula**.
1. Seçin **Tamam**ve dosyaları ayıklayın

Betikleri yerleştirilir... \\WingtipTicketsSaaS DbPerTenant ana\\öğrenme modülleri klasöründe.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Bu dağıtım için kullanıcı yapılandırma dosyasını güncelleştirme

Komut dosyalarını çalıştırmadan önce kullanıcı yapılandırma dosyasında kaynak grubu ve kullanıcı değerlerini güncelleştirin. Dağıtım sırasında kullanılan değerleri için bu değişkenleri ayarlayın.

1. PowerShell ISE'de Aç... \\Öğrenme modülleri\\**UserConfig.psm1**
1. Güncelleştirme **ResourceGroupName** ve **adı** dağıtımınız (10 ve 11 yalnızca satırlarındaki) için belirli değerlere sahip.
1. Değişiklikleri kaydedin.

Bu değerler, neredeyse tüm betikte başvurulur.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama olayları barındıran venues gösterir. Konser salonları, Caz kulüpleri ve Spor kulüpleri mekan türleri içerir. Wingtip bilet içinde venues kiracıları olarak kaydedilir. Bir kiracı olan bir mekan kolay bir yol listesi olaylara ve müşterilerine biletleri satmanın erişmesini sağlar. Her mekan, etkinliklerini ve bilet satmak için kişiselleştirilmiş bir Web sitesi alır.

Dahili olarak uygulamada, bir SQL veritabanı bir SQL elastik havuzuna dağıtılmış her bir kiracı alır.

Bir merkezi **olay hub'ı** sayfası, dağıtımınızdaki kiracılara bağlantıların listesini sağlar.

1. Olay hub'ı web tarayıcınızda açmak için URL'yi kullanın: http://events.wingtip-dpt.&lt; kullanıcı&gt;. trafficmanager.net. Yedek &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.

    ![Olay hub'ı](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Seçin **Fabrikam Caz kulübü** olay hub'ında.

    ![Olaylar](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Wingtip uygulama kullanıyorsa [*Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) gelen isteklerin dağıtımını denetlemek için. Belirli bir kiracı için olayları sayfasına erişmek için URL aşağıdaki biçimdedir:

- http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/fabrikamjazzclub

    Önceki biçimi bölümlerini aşağıdaki tabloda açıklanmıştır.

    | URL parçası        | Açıklama       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | Wingtip uygulamasının olayları bölümleri.<br /><br /> *-dpt* ayıran *Kiracı başına veritabanı* diğer uygulamalardan gelen Wingtip bilet uygulaması. Örnekler *tek* Kiracı başına uygulamayı (*-sa*) veya *çok kiracılı veritabanı* (*- mt*) uygulamaları. |
    | .  *&lt;kullanıcı&gt;* | *af1* örnekte. |
    | .trafficmanager.net/ | Traffic Manager, temel URL. |
    | fabrikamjazzclub | Fabrikam Caz kulübü adlı bir kiracıyı tanımlar. |
    | &nbsp; | &nbsp; |

- Kiracı adını URL'den olayları uygulama tarafından ayrıştırılır.
- Kiracı adı, bir anahtar oluşturmak için kullanılır.
- Anahtarı kiracının veritabanı konumunu almak için Kataloğu'na erişmek için kullanılır.
  - Kataloğu kullanılarak uygulanan *parça eşleme Yönetimi*.
- Olay hub'ı olayları listesi sayfası URL'leri için her bir kiracı oluşturmak için katalogdaki genişletilmiş meta verileri kullanır.

Bir üretim ortamında, genellikle bir CNAME DNS kaydı için oluşturduğunuz [*bir şirketin internet etki alanını işaret*](../traffic-manager/traffic-manager-point-internet-domain.md) Traffic Manager DNS adı.

> [!NOTE]
> Bu öğreticideki traffic manager kullanımı nedir hemen belirgin olmayabilir. Bu öğretici serisinde, karmaşık bir üretim ortamında ölçeğini işleyebileceği desenleri göstermek için hedefidir. Böyle bir durumda, örneğin, birden çok web uygulamaları geliştirmekten veritabanlarıyla birlikte bulunan, dağıtılmış gerekir ve bu örnekleri arasında dolaştırmak için traffic Manager'a gerekir.
Traffic manager'ın kullanımı gösteren başka bir öğretici ancak kümesidir [coğrafi geri yükleme](saas-dbpertenant-dr-geo-restore.md) ve [coğrafi çoğaltma](saas-dbpertenant-dr-geo-replication.md) öğreticiler. Aşağıdaki öğreticilerde, traffic manager, bölgesel bir kesinti durumunda kurtarma bir SaaS uygulaması örneği geçmek yardımcı olmak için kullanılır.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Şimdi uygulama dağıtıldıktan sonra çalışmasını yerleştirin.

*Tanıtım LoadGenerator* PowerShell Betiği, tüm Kiracı veritabanlarına yönelik çalışan bir iş yükü başlatır. Birçok SaaS uygulamalarına gerçek yükü ve tahmin edilemezdir. Bu tür iş yükünün benzetimini yapmak için rastgele ani ya da her Kiracı etkinliklerde ani artışlara ile bir yük Oluşturucu oluşturur. Ani artışlar rastgele aralıklarda ortaya çıkar. Ortaya çıkmaya başladı yük düzeni için birkaç dakika sürer. Çalıştırma yükü izleme önce en az üç veya dört dakika üreteci sağlar.

1. PowerShell ISE'de açın... \\Öğrenme modülleri\\yardımcı programları\\*tanıtım LoadGenerator.ps1* betiği.
2. Betiği çalıştırmak ve yük oluşturucuyu başlatmak için F5 tuşuna basın. Şimdilik varsayılan parametre değerlerini bırakın
3. Azure hesabınızda oturum açın ve gerekirse kullanmak için istediğiniz aboneliği seçin.

Yük Oluşturucu betiğine katalogda her veritabanı için bir arka plan işi başlar ve durur. Yük Oluşturucu betiğine yeniden yenilerini başlamadan önce çalışmakta olan herhangi bir arka plan işleri durdurur.

### <a name="monitor-the-background-jobs"></a>Arka plan işlerini izleme

Arka plan işleri izlemek ve denetlemek istiyorsanız, aşağıdaki cmdlet'leri kullanın:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

### <a name="demo-loadgeneratorps1-actions"></a>Tanıtım LoadGenerator.ps1 eylemleri

*Tanıtım LoadGenerator.ps1* müşteri hareketlerini etkin bir iş yükünü taklit eder. Aşağıdaki adımlar, bir dizi eylem açıklar, *tanıtım LoadGenerator.ps1* başlatır:

1. *Tanıtım LoadGenerator.ps1* başlar *LoadGenerator.ps1* ön planda.

    - Her iki .ps1 dosyaları öğrenme modülleri klasörlerinin depolanır\\yardımcı programları\\.

2. *LoadGenerator.ps1* katalogdaki tüm Kiracı veritabanlarında döner.

3. *LoadGenerator.ps1* her Kiracı veritabanı için bir arka plan PowerShell işi başlatır:

    - Varsayılan olarak, arka plan işleri için 120 dakikada bir çalıştır.
    - Her iş CPU tabanlı bir yük tek bir kiracı veritabanında yürüterek neden *sp_CpuLoadGenerator*. Yük süresi ve yoğunluk değişir bağlı olarak `$DemoScenario`.
    - *sp_CpuLoadGenerator* döngüler etrafında yüksek bir CPU yüküne neden olan bir SQL SELECT deyimi. SELECT sorunları arasındaki zaman aralığını denetlenebilir bir CPU yükü oluşturmak için parametre değerlerini göre değişir. Daha gerçekçi yükleri benzetimi yapmak için yük düzeyleri ve aralıkları rastgele.
    - Bu .sql dosyası altında depolanır *WingtipTenantDB\\dbo\\depolanmış yordamları\\*.

4. Varsa `$OneTime = $false`, yük Oluşturucu, arka plan işleri başlatır ve sonra çalışmaya devam eder. 10 saniyede bir, sağlanan tüm yeni kiracılara izler. Ayarlarsanız `$OneTime = $true`, LoadGenerator arka plan işleri başlar ve durur ön planda çalışmıyor. Bu öğreticide, bırakın `$OneTime = $false`.

   Durdurmak veya yük oluşturucuyu yeniden başlatmak istiyorsanız Ctrl-C ya da durdurma işlemi Ctrl-Break kullanın.

   Ön planda çalışan yük Oluşturucu değiştirmeden bırakırsanız, başka PowerShell betikleri çalıştırmak için başka bir PowerShell ISE örneği kullanın.

&nbsp;

Sonraki bölümde ile devam etmeden önce proje çağırma durumda çalışan yük Oluşturucu bırakın.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk dağıtım üç örnek Kiracı oluşturur. Şimdi dağıtılan uygulamayı yönelik etkisini öğrenmek için başka bir kiracıda oluşturun. Wingtip uygulamasında yeni kiracılar sağlama iş akışı içinde açıklanan [sağlama ve kataloğa kaydetme öğreticisinde](saas-dbpertenant-provision-and-catalog.md). Bu aşamada, bir dakikadan az alan olan yeni bir kiracı oluşturun.

1. Yeni bir PowerShell ISE'yi açın.
2. Aç... \\Öğrenme modülleri\sağlama ve kataloğa\\*Demo-ProvisionAndCatalog.ps1*.
3. Betiği çalıştırmak için F5 tuşuna basın. Şu an için varsayılan değerleri değiştirmeyin.

   > [!NOTE]
   > Wingtip SaaS betiklerden birçok *$PSScriptRoot* diğer betiklerde işlevleri çağırmak için klasörlere gözatmak için. Bu değişken, yalnızca tam betik, F5 tuşuna basılarak çalıştırıldığında değerlendirilir. Vurgulama ve bir seçim F8 ile çalışan, hatalara neden olabilir. Komut dosyalarını çalıştırmak için F5 tuşuna basın.

Yeni Kiracı veritabanıdır:

- Bir SQL elastik havuzunda oluşturulur.
- Başlatılmamış.
- Katalogdaki kayıtlı.

Başarılı sağlama sonra *olayları* yeni Kiracı sitesi tarayıcınızda görünür.

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Yeni Kiracı listede görünür hale getirmek için olay hub'ı yenileyin.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Bir yük koleksiyonuna yönelik karşı çalışan başladıktan sonra dağıtılan kaynaklardan bazıları göz atalım.

1. İçinde [Azure portalında](https://portal.azure.com), SQL sunucuları listesine göz atın. Açılacağını **Kataloğu-dpt -&lt;kullanıcı&gt;** sunucusu.
    - Katalog sunucusu iki veritabanı içeren **tenantcatalog** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan bir şablon veritabanı).

   ![Veritabanları](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. SQL sunucuları listesine geri dönün.

3. Açık **tenants1-dpt -&lt;kullanıcı&gt;** Kiracı veritabanlarını barındıran sunucu.

4. Aşağıdaki öğeler bakın:

    - Her Kiracı veritabanının, bir **esnek standart** 50 eDTU standart havuzda veritabanı.
    - Red Maple yarışı veritabanı, daha önce sağlanan Kiracı veritabanıdır.

   ![Sunucu veritabanları ile](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Sonra *LoadGenerator.ps1* çalıştırmalar birkaç dakika için yeterli veri olmalıdır bazı izleme özellikleri incelemeye başlamak için. Bu özellikler, havuz ve veritabanlarında oluşturulur.

Sunucuya Gözat **tenants1-dpt -&lt;kullanıcı&gt;** seçip **Pool1** havuz için kaynak kullanımını görüntülemek için. Aşağıdaki grafikte, yük Oluşturucu için bir saat çalıştı.

   ![Havuzu izleme](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- Etiketli ilk grafik **kaynak kullanımını**, havuz eDTU kullanımı gösterilmektedir.
- İkinci grafik, havuzda beş en etkin veritabanları için eDTU kullanımını gösterir.

İki grafik, elastik havuzların ve SQL veritabanı öngörülemeyen SaaS uygulaması iş yükleri için uygun olduğunu gösterir. Grafikleri Göster: her 40 Edtu'ya kadar geçiş yapabilen dört veritabanı olan ve henüz tüm veritabanlarına 50 eDTU havuz tarafından rahatça desteklenir. 50 eDTU havuz bile daha ağır iş yüklerini destekler. Her bir veritabanı tek veritabanı olarak sağladıysanız S2 olması gerekir (geçişlerini desteklemek için 50 DTU). Dört tek S2 veritabanı havuzunun fiyatı neredeyse üç kez maliyetidir. Gerçek durumlarda, SQL veritabanı müşterilerinin 200 eDTU havuzunda 500 veritabanları kadar çalıştırın. Daha fazla bilgi için [performans izleme Öğreticisi](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Ek kaynaklar

- Daha fazla bilgi için bkz. ek [Wingtip bilet SaaS Kiracı başına veritabanı uygulamayı geliştirecek öğreticilerden](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- Elastik havuzlar hakkında bilgi edinmek için [bir Azure SQL elastik havuzu nedir?](sql-database-elastic-pool.md).
- Esnek işler hakkında bilgi edinmek için [ölçeği genişletilen bulut veritabanlarını yönetme](sql-database-elastic-jobs-overview.md).
- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz: [çok kiracılı SaaS uygulamaları için Tasarım Düzenleri](saas-tenancy-app-design-patterns.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip bilet SaaS uygulamasını dağıtma
> - Sunucular, havuzlar ve uygulamayı oluşturan veritabanları hakkında.
> - Kiracıların kendi verilerle nasıl eşlendiğine *Kataloğu*.
> - Yeni kiracılar sağlama öğrenin.
> - Nasıl Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme.
> - İlgili faturalandırmayı durdurmak için örnek kaynakları silme yapma.

Ardından, deneyin [sağlama ve kataloğa kaydetme öğreticisinde](saas-dbpertenant-provision-and-catalog.md).

<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant
