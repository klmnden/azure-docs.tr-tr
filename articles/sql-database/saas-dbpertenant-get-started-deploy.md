---
title: Kiracı başına veritabanı SaaS öğretici - Azure SQL veritabanı | Microsoft Docs
description: Dağıtma ve Azure SQL veritabanı kullanarak veritabanı başına Kiracı düzeni ve diğer SaaS desenleri gösteren Wingtip biletleri SaaS çok müşterili uygulama keşfedin.
keywords: sql veritabanı öğreticisi
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.custom: scale out apps
ms.topic: article
ms.date: 12/07/2017
ms.author: genemi
ms.openlocfilehash: c62817b6bb60d99a4762e433510cc54d15add35a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="deploy-and-explore-a-multitenant-saas-app-that-uses-the-database-per-tenant-pattern-with-sql-database"></a>Dağıtma ve Kiracı başına veritabanı desen ile SQL veritabanı kullanan çok kiracılı bir SaaS uygulama keşfedin

Bu öğreticide dağıtmak ve Wingtip biletleri SaaS Kiracı başına veritabanı uygulama (Wingtip) inceleyin. Uygulama, birden çok kiracıya veri depolamak için bir kiracı başına veritabanı desen kullanır. Uygulama, SaaS senaryoları etkinleştirmek nasıl basitleştirmek Azure SQL veritabanı özelliklerini göstermek için tasarlanmıştır.

Seçtiğiniz sonra beş dakika **Azure'a Dağıt**, çok kiracılı bir SaaS uygulamasına sahip. Uygulama bulutta çalışan bir SQL veritabanı içerir. Uygulama, üç örnek kiracılar her biri kendi veritabanı ile birlikte dağıtılır. Tüm veritabanları SQL esnek havuza dağıtılır. Uygulama Azure aboneliğinize dağıtılır. Keşfetmek ve tek tek uygulama bileşenlerinin ile çalışmak için tam erişime sahip. Uygulama C# kaynak kodu ve yönetim komut dosyaları kullanılabilir olan [WingtipTicketsSaaS DbPerTenant GitHub deposuna][github-wingtip-dpt].

Bu öğreticide şunları öğrenirsiniz:

> [!div class="checklist"]
> - Wingtip SaaS uygulamasının nasıl.
> - Uygulama kaynağı kod ve yönetim komut dosyaları alınacağı.
> - Sunucuları, havuzları ve uygulaması olun veritabanları hakkında.
> - Kiracıların kendi veri ile nasıl eşlendiğini *katalog*.
> - Yeni bir kiracı sağlamasını yapma.
> - Uygulamasında Kiracı etkinliğini izlemek nasıl.

A [ilgili eğitim serileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials) çeşitli SaaS tasarım ve yönetim desenlerini keşfetmek sunar. Öğreticiler ilk bu dağıtım oluşturun. Öğreticiler kullandığınızda, farklı SaaS desenleri nasıl uygulandığını görmek için sağlanan komut dosyalarını inceleyebilirsiniz. Komut dosyalarını nasıl SQL veritabanı özelliklerini SaaS uygulamalarının geliştirilmesi basitleştirmek göstermektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için Azure PowerShell yüklü olduğundan emin olun. Daha fazla bilgi için bkz: [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="deploy-the-wingtip-tickets-saas-application"></a>Wingtip biletleri SaaS uygulamasına dağıtmak

#### <a name="plan-the-names"></a>Adları planlama

Bu bölümdeki adımları emin kaynak adları yapmak için kullanılan bir kullanıcı değerini genel benzersiz sağlar. Ayrıca uygulama dağıtımı tarafından oluşturulan tüm kaynaklarını içeren kaynak grubu için bir ad sağlayın. Ann Finley adlı kurgusal bir kişi için öneririz:

- **Kullanıcı**: *af1* Ann Finley'nın baş harflerini artı bir rakam oluşur. İkinci kez uygulamayı dağıtırsanız, farklı bir değer kullanın. Af2 örneğidir.
- **Kaynak grubu**: *wingtip dpt af1* bu Kiracı başına veritabanı uygulamayı gösterir. Kaynak grubu adı içerdiği kaynakların adları ile ilişkilendirmek için kullanıcı adı af1 ekleyin.

Adlarınızı şimdi seçin ve not edin. 

#### <a name="steps"></a>Adımlar

1. Azure portalında Wingtip biletleri SaaS Kiracı başına veritabanı dağıtım şablonunu açmak için seçin **Azure'a Dağıt**.

   <a href="https://aka.ms/deploywingtipdpt" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

2. Değerleri şablonda için gerekli parametreler girin.

    > [!IMPORTANT]
    > Bazı kimlik doğrulama ve sunucu güvenlik duvarları Tanıtım amaçlı bilerek güvenli. Yeni bir kaynak grubu oluşturmanızı öneririz. Mevcut kaynak grupları, sunucular veya havuzları kullanmayın. Bu uygulamayı, komut dosyaları veya dağıtılan tüm kaynakları üretim için kullanmayın. İlgili faturalama durdurmak için uygulama ile tamamladığınızda bu kaynak grubunu silin.

    - **Kaynak grubu**: seçin **Yeni Oluştur**ve kaynak grubu için daha önce seçtiğiniz benzersiz bir ad sağlayın. 
    - **Konum**: aşağı açılan listeden bir konum seçin.
    - **Kullanıcı**: daha önce seçtiğiniz kullanıcı adı değerini kullanın.

3. Uygulamayı dağıtın.

    a. Hüküm ve koşulları kabul için seçin.

    b. Seçin **satın alma**.

4. Dağıtım durumunu izlemek üzere seçmek **bildirimleri** (zil simgesine arama kutusunun sağındaki). Wingtip biletleri SaaS uygulama dağıtma yaklaşık beş dakika sürer.

   ![Dağıtım başarılı oldu](media/saas-dbpertenant-get-started-deploy/succeeded.png)

## <a name="download-and-unblock-the-wingtip-tickets-management-scripts"></a>Karşıdan yükleme ve Wingtip biletleri yönetim komut dosyaları Engellemeyi Kaldır

Uygulamayı dağıtır olsa da, kaynak kodu ve yönetim komut dosyaları indirin.

> [!IMPORTANT]
> .Zip dosyalarını bir dış kaynaktan yüklediğiniz ve açtığınız zaman yürütülebilir içeriğini (komut dosyaları ve DLL'ler) Windows tarafından engellenmiş olabilir. Komut dosyalarını ayıklayın önce .zip dosyası engelini kaldırma adımlarını izleyin. Kaldırma komut dosyalarının çalışmasına izin verilen emin olur.

1. Gözat [WingtipTicketsSaaS DbPerTenant GitHub deposuna][github-wingtip-dpt].
2. Seçin **Kopyala veya indir**.
3. Seçin **ZIP'i indir**ve ardından dosyayı kaydedin.
4. Sağ **WingtipTicketsSaaS DbPerTenant master.zip** dosya ve ardından **özellikleri**.
5. Üzerinde **genel** sekmesine **Engellemeyi Kaldır** > **Uygula**.
6. Seçin **Tamam**ve dosyaları ayıklayın

Komut dosyaları içinde bulunur... \\WingtipTicketsSaaS DbPerTenant ana\\öğrenme modülleri klasör.

## <a name="update-the-user-configuration-file-for-this-deployment"></a>Bu dağıtım için kullanıcı yapılandırma dosyasını güncelleştir

Herhangi bir betiği çalıştırmadan önce kullanıcı yapılandırma dosyası kaynak grubu ve kullanıcı değerlerini güncelleştirin. Dağıtım sırasında kullanılan değerleri için bu değişkenleri ayarlayın.

1. PowerShell ISE Aç... \\Modülleri öğrenme\\**UserConfig.psm1** 
2. Güncelleştirme **ResourceGroupName** ve **adı** (10 ve 11 yalnızca satırlarındaki) dağıtımınız belirli değerleri içeren.
3. Değişiklikleri kaydedin.

Bu değerler, neredeyse her komut dosyasında başvurulur.

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Uygulama olayları konak görebildikleri gösterir. Birlikte salonları, jazz Sinek ve Spor Sinek salonundan türleri içerir. Wingtip anahtarların görebildikleri kiracılar kaydedilir. Bir kiracı olmak bir salonundan kolay bir yol listesi olayları ve müşterilerine bilet satabilir olanak verir. Bunların olaylarını listelemek için ve bilet satabilir için kişiselleştirilmiş bir Web sitesi her yerini alır.

Dahili olarak uygulamada, her bir kiracı SQL esnek havuza dağıtılan bir SQL veritabanı alır.

Merkezi bir **olay hub'ı** sayfası, dağıtımınızdaki kiracılar bağlantıların listesini sağlar.

1. Web tarayıcınızda olay hub'ı açmak için URL'yi kullanın: http://events.wingtip-dpt.&lt;kullanıcı&gt;. trafficmanager.net. Yedek &lt;kullanıcı&gt; dağıtımınızın kullanıcı değerine sahip.

    ![Olay hub'ı](media/saas-dbpertenant-get-started-deploy/events-hub.png)

2. Seçin **Fabrikam Jazz kulübü** olay Hub'ında.

    ![Olaylar](./media/saas-dbpertenant-get-started-deploy/fabrikam.png)

#### <a name="azure-traffic-manager"></a>Azure Traffic Manager

Wingtip uygulamanın kullandığı [ *Azure Traffic Manager* ](../traffic-manager/traffic-manager-overview.md) gelen istekleri dağıtımını denetlemek için. Belirli bir kiracı için olayları sayfasına erişmek için URL şu biçimi kullanır:

- http://events.wingtip-dpt.&lt;user&gt;.trafficmanager.net/fabrikamjazzclub

    Önceki biçimi bölümlerini aşağıdaki tabloda açıklanmıştır.

    | URL bölümü        | Açıklama       |
    | :-------------- | :---------------- |
    | http://events.wingtip-dpt | Wingtip uygulama olayları bölümleri.<br /><br /> *-dpt* ayırt *Kiracı başına veritabanı* diğer uygulamalardan gelen Wingtip biletlerinin uygulama. Örnekler *tek başına* Kiracı başına uygulama (*-sa*) veya *çok müşterili veritabanı* (*- mt*) uygulamaları. |
    | .  *&lt;kullanıcı&gt;* | *af1* örnekte. |
    | .trafficmanager.net/ | Trafik Yöneticisi, temel URL. |
    | fabrikamjazzclub | Fabrikam Jazz kulübü adlı Kiracı tanımlar. |
    | &nbsp; | &nbsp; |

* Kiracı adı URL'den olayları uygulama tarafından ayrıştırılır.
* Kiracı adı, bir anahtar oluşturmak için kullanılır.
* Anahtar, kiracının veritabanının konumunu almak için Kataloğu'na erişmek için kullanılır.
    - Kataloğu kullanılarak uygulanır *parça eşleme Yönetim*.
* Olay hub'ı genişletilmiş meta veri Kataloğu'nda her bir kiracı için olaylar listesi sayfası URL'lerini oluşturmak için kullanır.

Bir üretim ortamında genellikle bir CNAME DNS kaydı oluşturmanız [ *bir şirketin internet etki alanını işaret* ](../traffic-manager/traffic-manager-point-internet-domain.md) trafik Yöneticisi DNS adı.

## <a name="start-generating-load-on-the-tenant-databases"></a>Kiracı veritabanları üzerinde yük oluşturmaya başlama

Uygulamanın dağıtıldığı, şimdi uygulamaya koyun.

*Demo LoadGenerator* PowerShell komut dosyasını tüm Kiracı veritabanları karşı çalışan bir iş yükü başlatır. Birçok SaaS uygulamalarını gerçek yük durumlarıyla ve tahmin edilemez. Bu tür yük benzetimini yapmak için rastgele ani veya her bir kiracı faaliyete WINS'e yüküyle üreteci oluşturur. WINS'e rastgele aralıklarda oluşur. Çıkmaya yük düzeni için birkaç dakika sürer. Yük izlemek önce en az üç veya dört dakika çalıştırmak üreteci sağlar.

1. PowerShell ISE Aç... \\Öğrenme modülleri\\yardımcı programları\\*Demo LoadGenerator.ps1* komut dosyası.
2. Komut dosyasını çalıştırın ve yükleme Oluşturucu başlatmak için F5 tuşuna basın. Varsayılan parametre değerleri için şimdi bırakın.
3. Azure hesabınızda oturum açın ve gerekirse kullanmak için istediğiniz aboneliği seçin.

Yük Oluşturucu betik katalogda her veritabanı için bir arka plan işi başlatır ve ardından durdurur. Yük Oluşturucu betiği yeniden yenilerini başlamadan önce çalışmakta olan herhangi bir arka plan işi durdurur.

#### <a name="monitor-the-background-jobs"></a>Arka plan işleri izleme

Denetim ve arka plan işleri izlemek istiyorsanız, aşağıdaki cmdlet'leri kullanın:

- `Get-Job`
- `Receive-Job`
- `Stop-Job`

#### <a name="demo-loadgeneratorps1-actions"></a>Tanıtım LoadGenerator.ps1 Eylemler

*Tanıtım LoadGenerator.ps1* müşteri hareketlerinin etkin bir iş yükünü taklit eder. Aşağıdaki adımlar, eylemlerin sırasını açıklar, *Demo LoadGenerator.ps1* başlatır:

1. *Tanıtım LoadGenerator.ps1* başlatır *LoadGenerator.ps1* ön planda.

    - Her iki .ps1 dosyaları öğrenme modülleri klasörleri altında depolanır\\yardımcı programları\\.

2. *LoadGenerator.ps1* döngüler katalogdaki tüm Kiracı veritabanları ile.

3. *LoadGenerator.ps1* her Kiracı veritabanı için bir arka plan PowerShell iş başlatılır:

    - Varsayılan olarak, arka plan işleri 120 dakikadır çalıştırın.
    - Bir kiracı veritabanı üzerinde CPU tabanlı yük yürüterek her bir iş neden *sp_CpuLoadGenerator*. Yükleme süresini ve yoğunluğu değişir bağlı olarak `$DemoScenario`. 
    - *sp_CpuLoadGenerator* yüksek bir CPU yüküne neden olan bir SQL SELECT deyimi geçici döngüler. Seçimi sorunları arasındaki zaman aralığını denetlenebilir CPU yükünü oluşturmak için parametre değerlerini göre değişir. Yük düzeylerini ve aralıklarını daha gerçekçi yükleri benzetmek için rastgele.
    - Bu .sql dosyası altında depolanır *WingtipTenantDB\\dbo\\depolanmış yordamları\\*.

4. Varsa `$OneTime = $false`, yükleme Oluşturucu arka plan işleri başlar ve çalışmaya devam eder. 10 saniyede sağlanan yeni kiracılar için izler. Ayarlarsanız `$OneTime = $true`, LoadGenerator arka plan işleri başlatır ve ardından ön planda çalışması durur. Bu öğretici için bırakın `$OneTime = $false`.

  Yük Oluşturucu yeniden başlatmak veya durdurmak isterseniz, Ctrl-C ya da durdurma işlemi Ctrl-Break kullanın. 

  Ön planda çalışmıyor yük Oluşturucu değiştirmeden bırakırsanız, diğer PowerShell betikleri çalıştırmak için başka bir PowerShell ISE örneği kullanın.

&nbsp;

Sonraki bölümde ile devam etmeden önce iş çağırma durumda çalışan yük Oluşturucu bırakın.

## <a name="provision-a-new-tenant"></a>Yeni bir kiracı sağlama

İlk üç örnek kiracılar oluşturur. Şimdi, dağıtılan uygulamanın etkisini görmek için başka bir kiracı oluşturursunuz. Wingtip uygulamada, yeni kiracılar sağlamak için iş akışı içinde açıklanan [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md). Bu aşamada, bir dakikadan az alan olan yeni bir kiracı oluşturun.

1. Yeni bir PowerShell ISE açın.
2. Aç... \\Modules\Provision ve Katalog öğrenme\\*Demo ProvisionAndCatalog.ps1*.
3. Komut dosyasını çalıştırmak için F5 tuşuna basın. Şu an için varsayılan değerleri bırakın.

   > [!NOTE]
   > Birçok Wingtip SaaS komut *$PSScriptRoot* diğer komut dosyalarında işlevleri çağırmak için klasörlere göz atmak için. Bu değişken, yalnızca tam komut dosyası, F5 tuşuna basarak yürütüldüğünde değerlendirilir. Vurgulama ve bir seçim ile F8 çalıştıran hatalara yol açabilir. Komut dosyalarını çalıştırmak için F5 tuşuna basın.

Yeni bir kiracı veritabanı şöyledir:

- Bir SQL esnek havuzu oluşturulur.
- Başlatıldı.
- Katalogda kayıtlı.

Başarılı sağlama sonra *olayları* yeni Kiracı sitesi tarayıcınızda görüntülenir.

![Yeni kiracı](./media/saas-dbpertenant-get-started-deploy/red-maple-racing.png)

Listede yeni Kiracı yapmak için olay hub'ı yenileyin.

## <a name="explore-the-servers-pools-and-tenant-databases"></a>Sunucuları, havuzları ve kiracı veritabanlarını öğrenme

Bir yük kiracılar koleksiyonu karşı çalışan başladıktan, dağıtılan kaynakların bazılarına göz atalım.

1. İçinde [Azure portal](http://portal.azure.com), SQL sunucuları listesine göz atın. Ardından açın **katalog-dpt -&lt;kullanıcı&gt;**  sunucu.
    - Katalog sunucusu iki veritabanlarını içeren **tenantcatalog** ve **basetenantdb** (yeni kiracılar oluşturmak için kopyalanan bir şablon veritabanı).

   ![Veritabanları](./media/saas-dbpertenant-get-started-deploy/databases.png)

2. SQL sunucu listesine geri dönün.

3. Açık **tenants1-dpt -&lt;kullanıcı&gt;**  Kiracı veritabanlarını barındıran sunucu.

4. Aşağıdaki öğeler bakın:

    - Her Kiracı veritabanı bir **esnek standart** 50 eDTU standart havuzdaki veritabanı.
    - Kırmızı Akçaağaç yarış veritabanı, daha önce sağlanan Kiracı veritabanıdır.

   ![Server veritabanları ile](./media/saas-dbpertenant-get-started-deploy/server.png)

## <a name="monitor-the-pool"></a>Havuzu izleme

Sonra *LoadGenerator.ps1* çalıştırmaları birkaç dakika için yeterli veri olmalıdır izleme bazı özellikler aramaya başlamak kullanılabilir. Bu özellikler, havuzları ve veritabanları oluşturulmuştur.

Sunucuya Gözat **tenants1-dpt -&lt;kullanıcı&gt;**seçip **Pool1** havuz için kaynak kullanımı görüntülemek için. Aşağıdaki grafikte, bir saat için yük Oluşturucu verdi.

   ![İzleyici havuzu](./media/saas-dbpertenant-get-started-deploy/monitor-pool.png)

- Etiketli ilk grafik **kaynak kullanımı**, havuz eDTU kullanımı gösterir.
- İkinci grafik eDTU kullanımı beş en etkin veritabanlarının havuzdaki gösterir.

İki grafik esnek havuzlar ve SQL veritabanı öngörülemeyen SaaS uygulama iş yükleri için uygun olduğunu gösterir. Grafikler, her emniyeti kadar 40 ila dört veritabanlarıdır ve henüz tüm veritabanları 50 eDTU havuzu tarafından rahatça desteklenir gösterir. 50 eDTU havuzu bile daha ağır iş yüklerini destekler. Veritabanları bağımsız veritabanı olarak sağlanır, her biri bir S2 olması gerekir (WINS'e desteklemek için 50 DTU). Dört tek başına S2 veritabanları havuzu bedelinin neredeyse üç kez maliyetidir. Gerçek dünya durumlarda, SQL veritabanı müşteriler 200 eDTU havuzu 500 veritabanlarında kadar çalıştırın. Daha fazla bilgi için bkz: [performans izleme Öğreticisi](saas-dbpertenant-performance-monitoring.md).

## <a name="additional-resources"></a>Ek kaynaklar

- Daha fazla bilgi için ek bkz [Wingtip biletleri SaaS Kiracı başına veritabanı uygulamayı derleme öğreticileri](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials).
- Esnek havuzları hakkında bilgi edinmek için [bir Azure SQL esnek havuzu nedir?](sql-database-elastic-pool.md).
- Esnek iş hakkında bilgi edinmek için [Yönet ölçeklendirilmiş bulut veritabanları](sql-database-elastic-jobs-overview.md).
- Çok kiracılı SaaS uygulamaları hakkında bilgi edinmek için [tasarım desenleri çok kiracılı SaaS uygulamaları için](saas-tenancy-app-design-patterns.md).


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunları öğrendiniz:

> [!div class="checklist"]
> - Wingtip biletleri SaaS uygulamasının nasıl.
> - Sunucuları, havuzları ve uygulaması olun veritabanları hakkında.
> - Kiracıların kendi veri ile nasıl eşlendiğini *katalog*.
> - Yeni kiracılar sağlamasını yapma.
> - Kiracı etkinliğini izlemek için havuzu kullanımı görüntülemek nasıl.
> - Nasıl ilgili faturalama durdurmak için örnek kaynaklar silinir.

Ardından, deneyin [sağlamak ve kataloğu Öğreticisi](saas-dbpertenant-provision-and-catalog.md).



<!-- Link references. -->

[github-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant 

