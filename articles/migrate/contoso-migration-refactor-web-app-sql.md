---
title: Contoso uygulamasına Azure Web uygulaması için ve Azure SQL veritabanı geçiş yaparak yeniden düzenleyin. | Microsoft Docs
description: Nasıl geçiş yaparak Contoso şirket içi uygulama rehosts öğrenmek için bir Azure Web uygulaması ve Azure SQL Server veritabanı.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/03/2018
ms.author: raynew
ms.openlocfilehash: 56937a6ad5c63e662c5e9ba9a1fd05900c3790d4
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43699150"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Contoso geçiş: bir şirket içi uygulamayı bir Azure Web uygulaması ve Azure SQL veritabanını yeniden düzenleme

Bu makalede, Contoso Azure SmartHotel uygulamasının nasıl yeniden düzenler gösterilmektedir. Bunlar, uygulama ön uç sanal makine bir Azure Web uygulaması ve uygulama veritabanına bir Azure SQL veritabanına geçirin.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarından bazılarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: bir uygulamayı bir Azure VM ve SQL veritabanı yönetilen örneği yeniden barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir  
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve SQL Server Always On kullanılabilirlik grubu için bir uygulama barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
Makale 9: bir uygulamayı bir Azure Web uygulaması ve Azure SQL veritabanında yeniden düzenleme | Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Bu makalede
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir 
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Contoso, Visual Studio Team Services azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir

Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisanslama maliyetlerini azaltmak istemektedir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanıldı.

**Gereksinimleri** | **Ayrıntılar**
--- | --- 
**Uygulama** | Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır.<br/><br/> Şu anda bir VMWare içinde olduğu gibi aynı performans özelliklerine sahip olmalıdır.<br/><br/> Takım, uygulamada yatırım yapmaya istememektedir. Şimdilik, yöneticileri yalnızca uygulama güvenli bir şekilde buluta taşır.<br/><br/> Takım, uygulama şu anda çalıştığı Windows Server 2008 R2 desteğini durduracak istiyorsunuz.<br/><br/> Ayrıca takım Yönetimi gereksinimini en aza indirecek bir modern PaaS veritabanı platform, SQL Server 2008 R2 uzağa gitme istemektedir.<br/><br/> Contoso istediğiniz mümkün olduğunda kendi SQL Server Lisanslama ve Yazılım Güvencesi yatırım yararlanın.<br/><br/> Ayrıca, Contoso tek web katmanındaki hata noktasını gidermek istiyor.
**Sınırlamalar** | Bir ASP.NET uygulaması ve aynı VM'de çalışan bir WCF hizmeti uygulaması oluşur. Bunlar bu Azure App Service kullanarak iki web uygulaması arasında bölmek isteyebilirsiniz. 
**Azure** | Contoso, uygulamayı Azure'a taşımak istiyor, ancak Vm'lerinde çalıştırmak istememektedir. Contoso Azure PaaS Hizmetleri web ve veri katmanları için yararlanmak istiyor. 
**DevOps** | Contoso, kendi derlemeleri için Visual Studio Team Services (VSTS) kullanarak bir DevOps modeline taşıma ve yayın işlem hatları ister.

## <a name="solution-design"></a>Çözüm tasarımı

Hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve geçiş için kullanılacak Azure Hizmetleri dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-app"></a>Geçerli uygulama

- SmartHotel şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-solution"></a>Önerilen çözüm

- SQL Server kullanarak uygulama veritabanı katmanı için Contoso Azure SQL veritabanı karşılaştırıldığında [bu makalede](https://docs.microsoft.com/azure/sql-database/sql-database-features). Contoso: Azure SQL veritabanı ile bazı nedenlerden dolayı karar verdim
    - Azure SQL veritabanı bir ilişkisel veritabanı yönetilen hizmetidir. Bu, yönetime neredeyse hiç ile birden fazla hizmet düzeyinde öngörülebilir performans sunar. Hiç kapalı kalma süresi, yerleşik zeka iyileştirmesi ve global ölçeklenebilirlik ve kullanılabilirlik ile dinamik ölçeklenebilirlik sayılabilir.
    - Contoso Data Migration Yardımcısı (şirket içi veritabanını Azure SQL'e geçirme ve değerlendirmek için DMA) basit yararlanabilirsiniz.
    - Yazılım Güvencesi içeren SQL Server için Azure hibrit Avantajı'ı kullanarak bir SQL veritabanı, indirimli Fiyatlardan için var olan lisans Contoso değiştirebilir. Bu değer % 30 tasarruf sağlayabilir.
    - SQL veritabanı, her zaman şifreli, dinamik veri maskeleme ve satır düzeyi güvenlik/tehdit algılama gibi güvenlik özellikleri sağlar.
- Uygulama web katmanı için Contoso Azure App Service kullanmaya karar vermiştir. Bu PaaS hizmeti, yalnızca birkaç yapılandırma değişikliğiyle uygulama dağıtmanıza olanak sağlar. Contoso değişiklik yapmak için Visual Studio'yu kullanın ve iki web uygulaması dağıtın. Web sitesi için diğeri için WCF hizmeti.
- DevOps işlem hattı gereksinimlerini karşılamak üzere Contoso VSTS kullanmayı seçti. Git depoları ile kaynak kodu Yönetimi (SCM), VSTS dağıtacaksınız. Otomatik derleme ve yayın Kodu derlemek için kullanılan ve Azure Web Apps'e dağıtın.
  
### <a name="solution-review"></a>Çözümü gözden geçirme
Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarımlarına değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | SmartHotel uygulama kodu, Azure'a geçiş için değiştirilmesi gerekmez.<br/><br/> Contoso, Yazılım Güvencesi'nın SQL Server ve Windows Server için Azure hibrit avantajı kullanılarak bir yatırım getirisi yararlanabilirsiniz.<br/><br/> Geçişten sonra Windows Server 2008 R2 desteklenmesi gerekmez. [Daha fazla bilgi edinin](https://support.microsoft.com/lifecycle).<br/><br/> Artık tek başarısızlık noktası değildir, Contoso web katmanı uygulamasının birden çok örnek ile yapılandırabilirsiniz.<br/><br/> Veritabanı artık bağımlı eskime SQL Server 2008 R2 üzerinde olacaktır.<br/><br/> SQL veritabanı teknik gereksinimlerini destekler. Contoso veritabanı geçiş Yardımcısı'nı kullanarak şirket içi veritabanı değerlendirilen ve uyumlu olduğunu belirledi.<br/><br/> SQL veritabanı, Contoso ayarlanan gerekmeyen yerleşik hata toleransı vardır. Bu veri katmanı artık tek bir yük devretme noktası olmasını sağlar.
**Simgeler** | Azure uygulama hizmetleri, her Web uygulaması için yalnızca bir uygulama dağıtımını destekler. Bu, iki Web uygulaması, sağlanan (bir Web sitesi için) ve WCF hizmeti için bir tane olması gerektiği anlamına gelir.<br/><br/> Contoso Data Migration Yardımcısı yerine veri geçiş hizmeti, veritabanını taşımak için kullanıyorsa, geçiş için hazır bir altyapı olmaz veritabanlarını ölçeğe. Contoso, birincil bölgeye kullanılamıyorsa yük devretme sağlamak için başka bir bölge oluşturmak için gereklidir.

## <a name="proposed-architecture"></a>Önerilen mimarisi

![Senaryo mimarisi](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Geçiş işlemi

1. Contoso bir Azure SQL örneğine hazırlar ve SmartHotel veritabanı için geçirir.
2. Contoso sağlar ve Web uygulamalarını yapılandırır ve bunları SmartHotel uygulamayı dağıtır.

    ![Geçiş işlemi](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso, azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak ve değerlendirmek için DMA'yı kullanın. DMA, SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir ve performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | Akıllı, tam olarak yönetilen bir ilişkisel bulut veritabanı hizmeti. | Özellikler, aktarım hızı ve boyutuna bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure uygulama Hizmetleri - Web uygulamaları](https://docs.microsoft.com/azure/app-service/app-service-web-overview) | Tam olarak yönetilen bir platform kullanarak güçlü bulut uygulamaları oluşturun | Boyut, konum ve kullanım süresine göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Önkoşullar

Bu senaryo çalıştırmak için Contoso gereksinimleri aşağıda verilmiştir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso abonelikleri erken bir makale sırasında oluşturuldu. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure SQL veritabanı örneğinde sağlama**: Contoso azure'da bir SQL örneği sağlar. WCF service web uygulaması, uygulama Web sitesi olan geçirdikten sonra Azure'a bu örneğine işaret edecek.
> * **2. adım: DMA veritabanıyla geçirme**: Contoso uygulaması veritabanıyla veritabanı geçiş Yardımcısı'nı geçirir.
> * **3. adım: Sağlama Web Apps**: Contoso hükümlerine iki web uygulamaları.
> * **4. adım: VSTS'yi ayarlama**: Contoso yeni VSTS projesi oluşturur ve Git deposunu içeri aktarır.
> * **5. adım: bağlantı dizelerini yapılandırma**: Contoso web katmanı web uygulaması, WCF service web uygulaması ve SQL örneğinin iletişim kurabilmesi için bağlantı dizelerini yapılandırır.
> * **6. adım: Yapı ayarlayın ve yayın VSTS'yi hatlarında**: son adım olarak, Contoso yapıyı ayarlar ve sürüm uygulamayı oluşturmak için işlem hatları ve bunları iki ayrı Azure Web uygulaması dağıtır.


## <a name="step-1-provision-an-azure-sql-database"></a>1. adım: Azure SQL veritabanı sağlama

1. Azure'da bir SQL veritabanı oluşturmak için contoso yöneticileri'ni seçin. 

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Bunlar şirket içi VM'de çalışan veritabanıyla eşleşmesi için bir veritabanı adı belirtin (**SmartHotel.Registration**). Bunlar veritabanı ContosoRG kaynak grubuna yerleştirin. Bu, azure'daki kaynakları üretim için kullandıkları kaynak grubudur.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Yeni bir SQL Server örneğini ayarlama ayarladıkları (**sql smarthotel eus2**) birincil bölgedeki.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Fiyatlandırma katmanını sunucularına eşleştirmek için ayarladıkları ve veritabanı gerekir. Ve zaten sahip oldukları bir SQL Server lisansı için Azure hibrit avantajı ile tasarruf seçin.
5. Boyutlandırma için bunlar v çekirdek tabanlı satın alma kullanın ve beklenen gereksinimlerine için sınırları ayarlayın.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Ardından veritabanı örneği oluşturun.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Örneği oluşturulduktan sonra veritabanı ve geçiş için veritabanı geçiş Yardım kullandığınızda ihtiyaç duydukları ayrıntılarını not edin.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- [Yardım](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bir SQL veritabanı sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) sanal çekirdek kaynak sınırları.


## <a name="step-2-migrate-the-database-with-dma"></a>2. adım: DMA veritabanını geçirme

Contoso yöneticileri DMA kullanarak SmartHotel veritabanı geçirir.

### <a name="install-dma"></a>DMA'yı yükleme

1. Bunlar aracı indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar Kurulum (DownloadMigrationAssistant.msi) sanal makinede çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

### <a name="migrate-the-database-with-dma"></a>DMA veritabanını geçirme

1. Yeni bir proje DMA'da oluşturdukları (**SmartHotelDB**) seçip **geçiş** 
2. Kaynak sunucu türü seçmeleri **SQL Server**ve hedef olarak **Azure SQL veritabanı**. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. Geçiş ayrıntıları ekledikleri **SQLVM** kaynak sunucu ve **SmartHotel.Registration** veritabanı. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Bunlar hangi kimlik doğrulaması ile ilişkili görünüyor bir hata alırsınız. Ancak araştırdıktan sonra veritabanı adı nokta (.) sorunudur. Geçici çözüm olarak, bunlar adını kullanarak yeni bir SQL veritabanını sağlamak karar **SmartHotel kayıt**, sorunu çözmek için. DMA'yı yeniden çalıştırın, bunlar seçebilir **SmartHotel kayıt**ve sihirbazıyla devam edin.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. İçinde **nesneleri seçin**, veritabanı tablolarını seçin ve bir SQL betiği oluşturun.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. DMS komut dosyasını oluşturduktan sonra'ı tıklatın **şemayı Dağıt**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. DMA, dağıtımın başarılı olduğunu onaylar.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. Şimdi geçişi başlatın.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Geçiş tamamlandıktan sonra Contoso yöneticileri Azure SQL örneğinde veritabanı çalıştığını doğrulayabilirsiniz.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. Bunlar ek SQL veritabanını silin **SmartHotel.Registration** Azure portalında.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>3. adım: Sağlama Web uygulamaları

Veritabanı geçişi, Contoso yöneticileri artık iki web uygulaması sağlayabilirsiniz.

1. Seçmeleri **Web uygulaması** portalında.

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Bunlar bir uygulama adı sağlayın (**SHWEB EUS2**), Windows üzerinde çalıştırma ve üretim kaynakları grubu kaldırma yerleştirin **ContosoRG**. Yeni app service ve planı oluştururlar.

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Web uygulaması sağlandıktan sonra WCF hizmeti için bir web uygulaması oluşturma işlemini yineleyin (**SHWCF EUS2**)

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. İşiniz bittiğinde sonra başarıyla oluşturulan denetlemek için uygulamaları adresine göz atın.


## <a name="step-4-set-up-vsts"></a>4. adım: VSTS ayarlama


Contoso uygulaması için işlem hatları ve DevOps altyapı oluşturmak gerekir.  Bunu yapmak için Contoso yöneticileri yeni VSTS projesi oluşturma, kod alın ve ardından derlemeyi Ayarla ve yayın işlem hatları.

1.   Bunlar Contoso VSTS hesabında yeni bir proje oluşturun (**ContosoSmartHotelRefactor**) seçip **Git** sürüm denetimi.

    ![Yeni proje](./media/contoso-migration-refactor-web-app-sql/vsts1.png)

2. Bunlar, şu anda uygulama kodlarını tutan Git deposunu içeri aktarma. İçinde bir [genel deponun](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) ve indirebilirsiniz.

    ![Uygulama kodu indirin](./media/contoso-migration-refactor-web-app-sql/vsts2.png)

3. Kod içeri aktardıktan sonra bunlar Visual Studio depoya bağlanın ve Takım Gezgini kullanarak kodu kopyalayın.

    ![Depoya bağlanın](./media/contoso-migration-refactor-web-app-sql/vsts3.png)

4. Deponun bir geliştirici makinesinde kopyasını sonra uygulama için çözüm dosyasını açın. Her web app ve wcf hizmeti projesi dosyası içinde ayırın.

    ![Çözüm dosyası](./media/contoso-migration-refactor-web-app-sql/vsts4.png)

## <a name="step-5-configure-connection-strings"></a>5. adım: bağlantı dizelerini yapılandırma

Web apps emin olmak contoso yöneticilerinin gerekir ve tüm veritabanı iletişim kurabilir. Bunu yapmak için bağlantı dizeleri kod ve web apps'te yapılandırın.

1. WCF hizmeti için web App'te (**SHWCF EUS2**) > **ayarları** > **uygulama ayarları**, bunlar adlı yeni bir bağlantı dizesi Ekle  **DefaultConnection**.
2. Bağlantı dizesi çekilen **SmartHotel kayıt** veritabanı ve doğru kimlik bilgileriyle güncelleştirilmelidir.

    ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Visual Studio kullanarak, bunlar açık **SmartHotel.Registration.wcf** çözüm dosyasını projeden. **ConnectionStrings** SmartHotel.Registration.Wcf bağlantı dizesi ile güncelleştirilmesi gerektiğini WCF hizmeti için web.config dosyasının bölümü.

     ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/string2.png)

4. **İstemci** SmartHotel.Registration.Web için web.config dosyasının WCF hizmeti yeni konumunu gösterecek şekilde değiştirilmesi. Hizmet uç noktasını barındıran WCF web uygulamasının URL'sini budur.

    ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/strings3.png)

5. Kodda değişiklik olduktan sonra değişiklikleri kaydetmek yöneticilerinin gerekir. Visual Studio'da Takım Gezgini'ni kullanarak bunlar commmit ve eşitleme.


## <a name="step-6-set-up-build-and-release-pipelines-in-vsts"></a>6. adım: Yapı ayarlayın ve VSTS'yi hatlarında yayın

Contoso yöneticileri artık VSTS derleme gerçekleştirmek ve eylem işlemine DevOps uygulamalarını serbest bırakmak için yapılandırın.

1. Bunlar, VSTS'de tıklayın **derleme ve yayın** > **yeni işlem hattı**.

    ![Yeni ardışık düzen](./media/contoso-migration-refactor-web-app-sql/pipeline1.png)

2. Seçmeleri **VSTS Gıt** ve ilgili depo.

    ![Git ve depo](./media/contoso-migration-refactor-web-app-sql/pipeline2.png)

3. İçinde **bir şablon seçin**, bunlar derlemesi ASP.NET şablonunu seçin.

     ![ASP.NET şablon](./media/contoso-migration-refactor-web-app-sql/pipeline3.png)
    
4. Derleme ContosoSmartHotelRefactor ASP.NET CI adını belirtin ve tıklayın **Kaydet ve kuyruğa**.

     ![Kaydet ve kuyruğa](./media/contoso-migration-refactor-web-app-sql/pipeline4.png)

5. Bu ilk derlemesi başlatıyor. Bunlar işlemini izlemek için yapı sayıya tıklayın. Tamamlandıktan sonra işlem geri bildirim görebilir.

    ![Geri Bildirim](./media/contoso-migration-refactor-web-app-sql/pipeline5.png)

6. Başarılı bir derleme, ardından derleme açın ve'a tıklayın, sonra'ı tıklatın **Yapıtları**. Bu klasör, yapı sonuçlarını içerir.

    - İki zip dosyaları uygulamaları içeren paketlerdir.
    - Bu dosyalar, yayın işlem hattı, Azure Web Apps'e dağıtım için kullanılır

     ![Yapıt](./media/contoso-migration-refactor-web-app-sql/pipeline6.png)

7. Simgeye **yayınlar** > **+ yeni işlem hattı**.

    ![Yeni ardışık düzen](./media/contoso-migration-refactor-web-app-sql/pipeline7.png)

8. Bunlar, Azure App Service dağıtım şablonu seçin.

    ![Azure App Service şablonu](./media/contoso-migration-refactor-web-app-sql/pipeline8.png)

9. Bunlar, yayın işlem hattı adı **ContosoSmartHotelRefactor**ve ortam adı (EUS2 SHWCF) WCF web uygulamasının adını belirtin.

    ![Ortam](./media/contoso-migration-refactor-web-app-sql/pipeline9.png)

10. Bir ortamda bunlar tıklayın **1. Aşama, 1 görev** WCF Hizmeti dağıtımını yapılandırmak için.

    ![WCF dağıtımı](./media/contoso-migration-refactor-web-app-sql/pipeline10.png)

11. Abonelik, seçili ve yetkili ve select doğrulayın **uygulama hizmeti adı**.

     ![App service'ı seçin](./media/contoso-migration-refactor-web-app-sql/pipeline11.png)

12. İçinde **Yapıtları**, seçtikleri **+ bir yapıt ekleme**ve ile oluşturmak için Seç **ContosoSmarthotelRefactor ASP.NET CI** işlem hattı.

     ![Oluşturma](./media/contoso-migration-refactor-web-app-sql/pipeline12.png)

15. Bunlar Şimşek tıklayın yapıt denetlenir., sürekli dağıtım tetikleyicisini etkinleştirin.

     ![Şimşek](./media/contoso-migration-refactor-web-app-sql/pipeline13.png)

16. Ayrıca, sürekli dağıtım tetikleyicisi ayarlanmış olması gerekir, Not **etkin**.

   ![Sürekli dağıtım etkin](./media/contoso-migration-refactor-web-app-sql/pipeline14.png) 

17. Şimdi, bunlar için tıklatın **Azure App Service'e dağıtma**.

    ![App service dağıtma](./media/contoso-migration-refactor-web-app-sql/pipeline15.png)

18. İçinde **bir dosya veya klasör seçin**, bunlar bulun **SmartHotel.Registration.Wcf.zip** derleme ve clilck sırasında oluştururken dosya **Kaydet**.-sql

    ![WCF Kaydet](./media/contoso-migration-refactor-web-app-sql/pipeline16.png)

19. Simgeye **işlem hattı** >**+ Ekle**eklemek için bir ortam için **SHWEB EUS2**, başka bir Azure App Service dağıtımı seçme.

    ![Ortam Ekle](./media/contoso-migration-refactor-web-app-sql/pipeline17.png)

20. Bunlar web uygulamasını yayımlama için işlemi tekrarlayın (**SmartHotel.Registration.Web.zip**) doğru web uygulamasına dosya.

    ![Web uygulaması yayımlama](./media/contoso-migration-refactor-web-app-sql/pipeline18.png)

21. Yayın işlem hattı, kaydedildikten sonra şu şekilde gösterilir.

     ![Yayın işlem hattı özeti](./media/contoso-migration-refactor-web-app-sql/pipeline19.png)

22. Bunlar geri gitme **derleme**, tıklatıp **Tetikleyicileri** > **sürekli tümleştirmeyi etkinleştir**. Bu işlem hattı, koda geçildiğinde değişiklikler yapılır ve tam derleme ve yayın gerçekleşir sağlar.

    ![sürekli tümleştirmeyi etkinleştir](./media/contoso-migration-refactor-web-app-sql/pipeline20.png)

23. Simgeye **Kaydet ve kuyruğa** tam işlem hattını çalıştırmak için. Yeni bir derleme, Azure App Service uygulamasının ilk sürüm sırayla oluşturan tetiklenir.

    ![İşlem hattı Kaydet](./media/contoso-migration-refactor-web-app-sql/pipeline21.png)

24. Contoso yöneticileri, yapı izleyin ve işlem hattı işlemden VSTS yayın. Yayın derleme tamamlandıktan sonra başlar.

    ![Derleme ve yayın uygulama](./media/contoso-migration-refactor-web-app-sql/pipeline22.png)

25. İşlem hattı bittikten sonra her iki site dağıtılan ve çalışan çevrimiçi uygulamasıdır.

    ![Son sürüm](./media/contoso-migration-refactor-web-app-sql/pipeline23.png)

Bu noktada, uygulamayı Azure'a başarıyla geçirildi.



## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Sanal makinelerin yerel yedekleme işlerden kaldırın.
- SmartHotel uygulama için yeni konumlar göstermek için iç belgeleri güncelleştirin. Veritabanını Azure SQL veritabanı ve ön uç iki web uygulaması çalışıyor olarak çalışıyor olarak göster.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.


## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

- Contoso gereken emin olmak, yeni **SmartHotel kayıt** veritabanı güvenlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Özellikle, Contoso web uygulamaları, SSL sertifikaları kullanacak şekilde güncelleştirmeniz gerekir.

### <a name="backups"></a>Yedeklemeler

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso SQL veritabanı yedeklemelerini yönetme hakkında daha fazla bilgi gerekiyor ve geri yükler. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups) otomatik yedekler hakkında.
- Contoso için veritabanı bölgesel yük devretme sağlamak için yük devretme grupları kullanmayı düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso Web uygulama ana Doğu ABD 2 ve orta ABD bölgesinde esnekliği için dağıtmayı göz önünde bulundurun gerekir. Contoso, Traffic Manager'ın yük devretme bölgesel kesintiler durumunda emin olmak için yapılandırabilirsiniz.

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını kendi [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama ön uç VM'nin iki Azure Web Apps'e geçiş yaparak Azure SmartHotel uygulamada yeniden düzenlenen. Uygulama veritabanı için bir Azure SQL veritabanı olarak geçirildi.






