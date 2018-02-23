---
title: "Kiracı başına veritabanı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs"
description: "Dağıtma ve Kiracı ve Azure SQL veritabanı kullanarak diğer SaaS desenleri başına veritabanının gösteren Wingtip biletleri SaaS çok kiracılı uygulama keşfedin."
keywords: "sql veritabanı öğreticisi"
services: sql-database
documentationcenter: 
author: MightyPen
manager: craigg
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/07/2017
ms.author: genemi
ms.openlocfilehash: cbe8a04abbf2dada7cc43e57e823c3a41bf83fe7
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="deploy-and-explore-a-multi-tenant-saas-application-that-uses-the-database-per-tenant-pattern-with-azure-sql-database"></a>Dağıtma ve Azure SQL veritabanı ile Kiracı deseni başına veritabanı kullanan bir çok kiracılı SaaS uygulaması keşfedin

Bu öğreticide, dağıtmak ve Wingtip biletleri SaaS keşfedin *veritabanı Kiracı başına* uygulama (Wingtip). Uygulama bir veritabanı Kiracı deseni başına birden çok kiracıya veri depolamak için kullanır. Uygulama, etkinleştirme SaaS senaryoları basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Etiketli mavi düğmesini tıklattıktan sonra beş dakika **Azure'a Dağıt**, bir çok kiracılı SaaS uygulamasına sahip. Uygulama Microsoft Azure bulutta çalışan bir Azure SQL veritabanı içerir. Uygulama, üç örnek kiracılar her biri kendi veritabanı ile birlikte dağıtılır. Tüm veritabanları SQL dağıtılan *esnek havuz*. Uygulama Azure aboneliğinize dağıtılır. Keşfetmek ve tek tek uygulama bileşenlerinin ile çalışmak için tam erişime sahip. Uygulama C# kaynak kodu ve yönetim komut dosyaları, kullanılabilir olan [WingtipTicketsSaaS DbPerTenant GitHub deposuna][github-wingtip-dpt].

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> - Wingtip SaaS uygulamasının nasıl.
> - Uygulama kaynak koduna ve yönetim komut dosyaları nereden.
> - Sunucuları, havuzları ve uygulaması olun veritabanları hakkında.
> - Kiracıların kendi veri ile nasıl eşlendiğini *katalog*.
> - Yeni bir kiracı sağlamasını yapma.
> - Uygulamasında Kiracı etkinliğini izlemek nasıl.

A [ilgili eğitim serileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) çeşitli SaaS tasarım ve yönetim desenlerini keşfetmek sunar. Öğreticiler ilk bu dağıtım oluşturun.
Öğreticiler kullandığınızda, sağlanan komut dosyalarını inceleyerek farklı SaaS desenleri nasıl uygulandığını görebilirsiniz.
Komut dosyalarını nasıl SQL veritabanı özelliklerini SaaS uygulamalarının geliştirilmesi basitleştirmek göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki ön koşulların karşılandığından emin olun:

* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wingtip biletleri SaaS uygulamasına dağıtmak

#### <a name="plan-the-names"></a>Adları planlama

Bu bölüm, aşağıdaki adımlarda, sağladığınız bir *kullanıcı* kaynak adları genel benzersiz olduğundan emin olmak için kullanılan değeri ve için bir ad *kaynak grubu* bir dağıtım tarafından oluşturulan tüm kaynaklar içeriyor uygulama. Adlı bir kişi için *Ann Finley*, öneririz:
- *Kullanıcı:* **af1***(kendi baş harflerini artı bir sayı.   İkinci kez uygulama dağıttığınızda farklı bir değer (örneğin af2) kullanın.)*
- *Kaynak grubu:* **wingtip dpt af1** *(wingtip dpt gösterir Kiracı başına veritabanı uygulama budur. Kullanıcı adı af1 ekleme içerdiği kaynakların adları ile kaynak grubu adı hatalarla ilintilidir.)*

Adlarınızı şimdi seçin ve not edin. 

#### <a name="steps"></a>Adımlar

1. Wingtip biletleri SaaS veritabanı Kiracı dağıtım şablonu başına Azure portalında mavi tıklatarak **Azure'a Dağıt** düğmesi.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. Şablonuna gerekli parametreler için değer girin:

    > [!IMPORTANT]
    > Gösterim amaçları doğrultusunda bazı kimlik doğrulama işlemlerinin ve sunucu güvenlik duvarlarının güvenliği kasıtlı olarak kaldırılmıştır. Öneririz, *yeni bir kaynak grubu oluşturma*. Mevcut kaynak grupları, sunucular veya havuzları kullanmayın. Bu uygulamayı, komut dosyaları veya dağıtılan tüm kaynakları üretim için kullanmayın. İlgili faturalandırmayı durdurmak için uygulamayla işiniz bittiğinde bu kaynak grubunu silin.

    - **Kaynak grubu** - seçin **Yeni Oluştur** ve benzersiz sağlamak **adı** kaynak grubu için daha önce seçtiğiniz. 
    - **Konum** - seçin bir **konumu** aşağı açılan listeden.
    - **Kullanıcı** -daha önce seçtiğiniz kullanıcı adı değerini kullanın.

1. Uygulamayı dağıtın.

    - Hüküm ve koşulları kabul için tıklatın.
    - **Satın al**’a tıklayın.

1. Dağıtım durumunu izlemek tıklayarak **bildirimleri**, arama kutusunun sağındaki zil simgesine olduğu. Wingtip biletleri SaaS uygulama dağıtma yaklaşık beş dakika sürer.

   ![dağıtım başarılı](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Karşıdan yükleme ve Wingtip biletleri yönetim komut dosyaları Engellemeyi Kaldır

Uygulama dağıtımı sırasında kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> .Zip dosyalarını bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriği (komut dosyaları, DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyaları zip dosyasından çıkarılırken, .zip dosyasını ayıklanıyor önce engellemesini kaldırmak için aşağıdaki adımları kullanın. Kaldırma komut dosyalarını çalıştırma izni sağlar.

1. Gözat [WingtipTicketsSaaS DbPerTenant GitHub deposuna][github-wingtip-dpt].
2. Tıklatın **Kopyala veya indir**.
3. Tıklatın **ZIP'i indir**ve ardından dosyayı kaydedin.
4. Sağ **WingtipTicketsSaaS DbPerTenant master.zip** dosya ve ardından **özellikleri**.
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır**ve ardından **Uygula**.
6. **Tamam**’a tıklayın.
7. Dosyaları ayıklayın.

Komut dosyaları içinde bulunur *... \\WingtipTicketsSaaS DbPerTenant ana\\öğrenme modülleri* klasör.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Bu dağıtım için kullanıcı yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce güncelleştirme *kaynak grubu* ve *kullanıcı* değerler *Kullanıcı Yapılandırması* dosya. Dağıtım sırasında kullanılan değerleri için bu değişkenleri ayarlayın.

1. İçinde **PowerShell ISE**açın... \\Modülleri öğrenme\\**UserConfig.psm1** 
2. Güncelleştirme **ResourceGroupName** ve **adı** (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin!

Bu değerler, neredeyse her komut dosyasında başvurulur.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama olayları konak görebildikleri gösterir. Birlikte salonları, jazz Sinek ve Spor Sinek salonundan türleri içerir. Wingtip anahtarların görebildikleri kiracılar kaydedilir. Bir kiracı olmak bir salonundan kolay bir yol listesi olayları ve müşterilerine bilet satabilir olanak verir. Bunların olaylarını listeler ve bilet satabilir için kişiselleştirilmiş bir web sitesine her yerini alır.

Dahili olarak uygulamada, bir SQL veritabanı bir SQL esnek havuza dağıtılan her bir kiracı alır.

Merkezi bir **olay hub'ı** sayfası, dağıtımınızdaki kiracılar bağlantıların listesini sağlar.

1. Açık *olay hub'ı* web tarayıcınızda: http://events.wingtip-dpt.&lt;kullanıcı&gt;. trafficmanager.net (yerine &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile):

    ![olay hub’ı](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. *Olay Hub’ında* **Fabrikam Caz Kulübü**’ne tıklayın.

    ![Olaylar](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Wingtip uygulamanın kullandığı [ *Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) gelen istekleri dağıtımını denetlemek için. Belirli bir kiracı için olayları sayfasına erişmek için URL şu biçimi kullanır:

- http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/fabrikamjazzclub

Önceki biçimi bölümlerini aşağıdaki tabloda açıklanmıştır.

| URL bölümü        | Açıklama       |
| :-------------- | :---------------- |
| http://events.wingtip-dpt | Wingtip uygulama olayları bölümleri.<br /><br /> *-dpt* ayırt *veritabanı Kiracı başına* diğer uygulamalardan gelen Wingtip biletlerinin uygulama. Örneğin, *tek başına* Kiracı başına uygulama (*-sa*), veya *çok Kiracı veritabanı* (*- mt*) uygulamaları. |
| .  *&lt;kullanıcı&gt;* | *af1* örneğimizde. |
| .trafficmanager.net/ | Azure trafik Yöneticisi, temel URL. |
| fabrikamjazzclub | Adlı Kiracı tanımlayan *Fabrikam Jazz kulübü*. |
| &nbsp; | &nbsp; |

1. Kiracı adı URL'den olayları uygulama tarafından ayrıştırılır.
2. Kiracı adı, bir anahtar oluşturmak için kullanılır.
3. Anahtar, kiracının veritabanının konumunu almak için kataloğu erişmek için kullanılır.
    - Kataloğu kullanılarak uygulanan *parça eşleme Yönetim*.
4. *Olay hub'ı* olayları sayfa URL listesini her bir kiracı oluşturmak için katalogda genişletilmiş meta verilerini kullanır.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturmanız [ *bir şirketin internet etki alanını işaret* ](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi DNS adı.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Uygulamanın dağıtıldığı, şimdi uygulamaya koyun!
*Demo LoadGenerator* PowerShell komut dosyasını tüm Kiracı veritabanları karşı çalışan bir iş yükünü başlatır.
Birçok SaaS uygulamalarını gerçek yük durumlarıyla ve tahmin edilemez.
Bu tür yük benzetimini yapmak için rastgele ani veya her bir kiracı faaliyete WINS'e yüküyle üreteci oluşturur.
WINS'e rastgele aralıklarda oluşur.
Çıkmaya yük düzeni için birkaç dakika sürer. Bu nedenle yükü izleme önce en az üç veya dört dakika çalıştırmak Oluşturucu izin vermek en iyisidir.

1. İçinde *PowerShell ISE*açın... \\Öğrenme modülleri\\yardımcı programları\\*Demo LoadGenerator.ps1* komut dosyası.
1. Tuşuna **F5** komut dosyasını çalıştırın ve yükleme Oluşturucu başlatın. (Varsayılan parametre değerleri için şimdi bırakın.)
1. Azure hesabınızda oturum açmak için istenir ve gerekirse, aboneliği seçmek için kullanmak istediğiniz.

Yük Oluşturucu betik katalogda her veritabanı için bir arka plan işi başlatır ve ardından durdurur.  Yük Oluşturucu betiği yeniden çalıştırın, ilk yenilerini başlatmadan önce çalışmakta herhangi bir arka plan işi durdurur.

#### <a name="monitor-the-background-jobs"></a>Arka plan işleri izleme

Denetim ve arka plan işleri izlemek istiyorsanız, aşağıdaki cmdlet'leri kullanarak şunları yapabilirsiniz:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

#### <a name="demo-loadgeneratorps1-actions"></a>Tanıtım LoadGenerator.ps1 Eylemler

*Tanıtım LoadGenerator.ps1* müşteri hareketlerinin etkin bir iş yükünü taklit eder. Aşağıdaki adımlar, eylemlerin sırasını açıklar, *Demo LoadGenerator.ps1* başlatır:

1. *Tanıtım LoadGenerator.ps1* başlatır *LoadGenerator.ps1* ön planda.
    - Bu .ps1 dosyaları her ikisi de altındaki klasörler depolanan *öğrenme modülleri\\yardımcı programları\\*.

1. *LoadGenerator.ps1* döngüler katalogdaki tüm Kiracı veritabanları ile.

1. *LoadGenerator.ps1* her Kiracı veritabanı için bir arka plan PowerShell iş başlatılır: 
    - Varsayılan olarak arka plan işleri 120 dakika boyunca çalışır.
    - Bir kiracı veritabanı üzerinde CPU tabanlı yük yürüterek her bir iş neden *sp_CpuLoadGenerator*.  Yükleme süresini ve yoğunluğu değişir bağlı olarak `$DemoScenario`. 
    - *sp_CpuLoadGenerator* yüksek bir CPU yüküne neden olan bir SQL SELECT deyimi geçici döngüler. Seçimi sorunları arasındaki zaman aralığını denetlenebilir CPU yükünü oluşturmak için parametre değerlerini göre değişir. Yük düzeylerini ve aralıklarını daha gerçekçi yükleri benzetmek için rastgele.
    - Bu .sql dosyası altında depolanır *WingtipTenantDB\\dbo\\depolanmış yordamları\\*.

1. Varsa `$OneTime = $false`, yükleme Oluşturucu arka plan işleri başlatır ve ardından çalıştırmak sağlanan yeni Kiracı için 10 saniyede izlemeye devam eder. Ayarlarsanız `$OneTime = $true`, LoadGenerator arka plan işleri başlatın ve ardından ön planda çalışmıyor. Bu öğretici için bırakın `$OneTime = $false`.

  Yük Oluşturucu yeniden başlatmak veya durdurmak isterseniz, Ctrl-C ya da durdurma işlemi Ctrl-Break kullanın. 

  Ön planda çalışmıyor yük Oluşturucu değiştirmeden bırakırsanız, diğer PowerShell betikleri çalıştırmak için başka bir PowerShell ISE örneği kullanın.

&nbsp;

Sonraki bölüme devam etmeden önce iş çağırma durumda çalışan yük Oluşturucu bırakın.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk üç örnek kiracılar oluşturur. Şimdi, dağıtılan uygulamanın etkisini görmek için başka bir kiracı oluşturursunuz. Wingtip uygulamada, yeni kiracılar sağlamak için iş akışı içinde açıklanan [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md). Bu aşamada, bir dakikadan az alan olan yeni bir kiracı oluşturun.

1. Yeni bir *PowerShell ISE*.
1. Aç... \\Modules\Provision ve Katalog öğrenme\\*Demo ProvisionAndCatalog.ps1* .
2. Betiği çalıştırmak için **F5**'e basın. (Şimdilik varsayılan değerleri bırakın.)

   > [!NOTE]
   > Birçok Wingtip SaaS komut *$PSScriptRoot* diğer komut dosyalarında işlevleri çağırmak için klasörleri gidin. Tam komut dosyası tuşlarına basarak çalıştırıldığında bu değişken yalnızca hesaplanan **F5**.  Vurgulama ve bir seçim ile çalışan **F8** hatalara yol açabilir. Bu nedenle tuşuna basarak komut dosyasını çalıştırın **F5**.

Yeni bir kiracı veritabanı şöyledir:

- Bir SQL esnek havuzu oluşturulur.
- Başlatıldı.
- Katalogda kayıtlı.

Başarılı sağlama sonra *olayları* yeni Kiracı sitesi tarayıcınızda görüntülenir:

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Yenileme *olay hub'ı* listede yeni Kiracı yapma.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Bir yük kiracılar koleksiyonu karşı çalışan başladıktan, dağıtılan kaynakların bazıları bakalım:

1. İçinde [Azure portal](http://portal.azure.com)SQL sunucuları listesine göz atın ve ardından açın **katalog-dpt -&lt;kullanıcı&gt;**  sunucu.
    - Katalog sunucusu iki veritabanlarını içeren **tenantcatalog** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan bir şablon veritabanı).

   ![veritabanları](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. SQL sunucu listesine geri dönün.

3. Açık **tenants1-dpt -&lt;kullanıcı&gt;**  Kiracı veritabanlarını barındıran sunucu.

4. Aşağıdaki öğeler bakın:
    - Her Kiracı veritabanı bir *esnek standart* 50 eDTU standart havuzdaki veritabanı.
    - *Kırmızı Akçaağaç yarış* önceden sağladığınız Kiracı veritabanı olan, veritabanı.

   ![sunucu](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Sonra *LoadGenerator.ps1* çalıştırmaları birkaç dakika için yeterli veri olmalıdır izleme bazı özellikler aramaya başlamak kullanılabilir. Bu özellikler, havuzları ve veritabanları oluşturulmuştur.

Sunucuya Gözat **tenants1-dpt -&lt;kullanıcı&gt;**, tıklatıp **Pool1** havuz için kaynak kullanımı görüntülemek için. Aşağıdaki grafikte, bir saat için yük Oluşturucu verdi.

   ![havuzu izleme](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- Etiketli ilk grafik **kaynak kullanımı**, havuz eDTU kullanımı gösterir.
- İkinci grafik eDTU kullanımı beş en etkin veritabanlarının havuzdaki gösterir.

İki grafik esnek havuzlar ve SQL veritabanı öngörülemeyen SaaS uygulama iş yükleri için uygun olduğunu gösterir.
Grafikler, her emniyeti kadar 40 ila 4 veritabanlarıdır ve henüz tüm veritabanları 50 eDTU havuzu tarafından rahatça desteklenir gösterir. 50 eDTU havuzu bile daha ağır iş yüklerini destekler.
Tek başına veritabanı olarak sağlanan varsa, her veritabanı bir S2 olması gerekir (WINS'e desteklemek için 50 DTU).
4 tek başına S2 veritabanları maliyetini yaklaşık 3 kez fiyat havuzunun olacaktır.
Gerçek dünya durumlarda, SQL veritabanı müşteriler 200 eDTU havuzu 500 veritabanlarında kadar çalıştırın.
Daha fazla bilgi için [Performans izleme öğreticisini](saas-dbpertenant-performance-monitoring.md) inceleyin.

## <a name="additional-resources"></a>Ek kaynaklar

- Ek [Kiracı uygulama başına Wingtip biletleri SaaS veritabanı üzerinde yapı öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
- Elastik havuzlar hakkında bilgi edinmek için bkz. [*Azure SQL elastik havuzu nedir?*](sql-database-elastic-pool.md)
- Esnek işler hakkında bilgi edinmek için bkz. [*Genişletilmiş bulut veritabanlarını yönetme*](sql-database-elastic-jobs-overview.md)
- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için bkz. [*Çok kiracılı SaaS uygulamaları için tasarım düzenleri*](saas-tenancy-app-design-patterns.md)


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip biletleri SaaS uygulamasına dağıtma
> - Uygulamayı oluşturan sunucular, havuzlar ve veritabanları hakkında bilgi
> - *Katalog* aracılığıyla kiracılar ve verilerinin eşleşmesi
> - Yeni kiracılar sağlama
> - Kiracı etkinliğini izlemek için havuz kullanımını görüntüleme
> - İlgili faturalandırmayı durdurmak için örnek kaynakları silme

Ardından, deneyin [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 

