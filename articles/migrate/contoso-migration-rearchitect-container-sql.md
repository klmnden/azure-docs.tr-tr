---
title: Bir Azure kapsayıcı ve Azure SQL veritabanı içinde bir Contoso uygulaması yeniden oluşturma | Microsoft Docs
description: Contoso Azure Windows kapsayıcıları ve Azure SQL veritabanı bir uygulamada nasıl yeniden oluşturma hakkında bilgi edinin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 733a93d0fc80d86d28f13a9e1d32108b58893bf0
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2018
ms.locfileid: "42060891"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Contoso geçiş: bir Azure kapsayıcı ve Azure SQL veritabanı için bir şirket içi uygulamayı yeniden oluşturma

Bu makalede, Contoso nasıl geçirir ve bunların Azure SmartHotel uygulamayı yeniden oluşturma gösterilmektedir. Bunlar, uygulama ön uç sanal makine bir Azure Windows kapsayıcısı ve uygulama veritabanına bir Azure SQL veritabanına geçirin.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarından bazılarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği için bir uygulama barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: Azure sanal makinelerinde bir uygulamayı barındırma](contoso-migration-rehost-vm.md) | Contoso geçirme SmartHotel uygulama yalnızca Site RECOVERY'yi kullanarak VM'lerin nasıl gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve SQL Server Always On kullanılabilirlik grubu için bir uygulama barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makinelerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu için bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı bir Azure Web uygulaması ve Azure SQL veritabanını yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL için bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da.
Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Bu makalede
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir

Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisanslama maliyetlerini azaltmak istemektedir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanıldı.

**Hedefleri** | **Ayrıntılar**
--- | --- 
**Uygulama reqs** | Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır.<br/><br/> Şu anda bir VMWare içinde olduğu gibi aynı performans özelliklerine sahip olmalıdır.<br/><br/> Üzerinde uygulama şu anda çalışır ve uygulamada yatırım yapmaya hazır olduğunuz Windows Server 2008 R2, desteğini durduracak şekilde isterler.<br/><br/> SQL Server 2008 R2 uzağa Yönetimi gereksinimini en aza indirecek modern bir PaaS veritabanı platformu taşımak isterler.<br/><br/> Contoso istediğiniz mümkün olduğunda kendi SQL Server Lisanslama ve Yazılım Güvencesi yatırım yararlanın.<br/><br/> Uygulama web katmanını ölçeklendirmek mümkün olmasını isterler.
**Sınırlamalar** | Bir ASP.NET uygulaması ve aynı VM'de çalışan bir WCF hizmeti uygulaması oluşur. Bunlar bu Azure App Service kullanarak iki web uygulaması arasında bölmek isteyebilirsiniz. 
**Azure reqs** | Uygulamayı Azure'a taşıyabilir ve uygulama yaşam genişletmek için bir kapsayıcıda çalıştırmak isterler. Tamamen Azure'da uygulama sıfırdan başlatmak istemezsiniz. 

## <a name="solution-design"></a>Çözüm tasarımı

Kendi hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve bunların geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi belirler.

### <a name="current-app"></a>Geçerli uygulama

- SmartHotel şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-architecture"></a>Önerilen mimarisi

- SQL Server kullanarak uygulama veritabanı katmanı için Contoso Azure SQL veritabanı karşılaştırıldığında [bu makalede](https://docs.microsoft.com/azure/sql-database/sql-database-features). Bunlar, Azure SQL veritabanı ile bazı nedenlerden dolayı karar verdim:
    - Azure SQL veritabanı bir ilişkisel veritabanı yönetilen hizmetidir. Bu, yönetime neredeyse hiç ile birden fazla hizmet düzeyinde öngörülebilir performans sunar. Hiç kapalı kalma süresi, yerleşik zeka iyileştirmesi ve global ölçeklenebilirlik ve kullanılabilirlik ile dinamik ölçeklenebilirlik sayılabilir.
    - Bunlar, basit bir Data Migration Yardımcısı (şirket içi veritabanını Azure SQL'e geçirme ve değerlendirmek için DMA) yararlanabilir.
    - Yazılım Güvencesi içeren SQL Server için Azure hibrit Avantajı'ı kullanarak bir SQL veritabanı, indirimli Fiyatlardan için mevcut lisanslarını gönderip alabilir. Bu değer % 30 tasarruf sağlayabilir.
    - SQL veritabanı, her zaman şifreli, dinamik veri maskeleme ve satır düzeyi güvenlik/tehdit algılama gibi güvenlik özellikleri sağlar.
- Bunlar uygulama web katmanı için karar Visual Studio kullanarak Windows kapsayıcısına Dönüştür.
    - Bunlar Azure Service Fabric kullanarak uygulamayı dağıtma ve Windows kapsayıcı görüntüsü Azure Container Registry (ACR) öğesinden çekme.
    - Bir prototip uygulamasının yaklaşım analizi de içerecek şekilde genişletmek için Service Fabric, Cosmos DB'ye bağlı başka bir hizmet olarak uygulanacaktır.  Bu bilgileri Tweetleri okuyun ve üzerinde uygulamayı görüntüleyin.

    ![Senaryo mimarisi](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Çözümü gözden geçirme
Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarımlarına değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | SmartHotel uygulama kodu, Azure Service fabric'e geçiş için değiştirilmesi gerekir. Ancak, değişiklikler için Service Fabric SDK araçları kullanarak, minimal istenmektedir.<br/><br/> Service Fabric için taşıma ile bunlar, özgün kod tabanının riski olmadan zaman içinde hızlı bir şekilde uygulamaya eklemek için mikro hizmet geliştirin başlayabilirsiniz.<br/><br/> Windows kapsayıcıları, genel kapsayıcılarıyla aynı avantajları sunar. Bunlar, çevikliği, taşınabilirlik ve denetimi artırın.<br/><br/> Bunlar, Yazılım Güvencesi'nın SQL Server ve Windows Server için Azure hibrit avantajı kullanılarak bir yatırım getirisi yararlanabilirsiniz.<br/><br/> Geçişten sonra kullanıcılar artık Windows Server 2008 R2 desteği gerekir. [Daha fazla bilgi edinin](https://support.microsoft.com/lifecycle).<br/><br/> Artık tek başarısızlık noktası değildir, bu nedenle web katmanı uygulamasının birden çok örnek ile yapılandırabilirsiniz.<br/><br/> Artık bağımlı eskime SQL Server 2008 R2 üzerinde olmaları.<br/><br/> SQL veritabanı teknik gereksinimler Contoso'nun destekler. Bunlar veritabanı geçiş Yardımcısı'nı kullanarak şirket içi veritabanı değerlendirilen ve uyumlu olduğunu belirledi.<br/><br/> SQL veritabanı, Contoso ayarlanan gerekmeyen yerleşik hata toleransı vardır. Bu veri katmanı artık tek bir yük devretme noktası olmasını sağlar.
**Simgeler** | Diğer geçiş seçenekleri daha karmaşık kapsayıcılardır. Öğrenme eğrisini kapsayıcılarına, Contoso için bir sorun olabilir.  Bunlar, yeni bir eğri artma değerli birçok sağlayan bir karmaşıklık düzeyi sunar.<br/><br/> Contoso'da operasyon ekibinin, anlamak ve Azure, kapsayıcılar ve mikro hizmetler uygulama için destek başlamasını gerekir.<br/><br/> Bunlar veri geçiş Yardımcısı'nı veri geçiş hizmeti yerine kendi veritabanı için geçişi kullanırsanız, Contoso geçirme için hazır bir altyapı olmaz veritabanlarını ölçeğe.



### <a name="migration-process"></a>Geçiş işlemi

1. Contoso için Windows Azure service fabric kümesi sağlayın.
2. Bunlar, bir Azure SQL örneği sağlaması ve SmartHotel veritabanını geçirmek.
3. Bunlar, Service Fabric SDK araçları kullanarak bir Docker kapsayıcısı Web Katmanı VM dönüştürün.
4. Bunlar service fabric kümesi ve ACR bağlamak ve Azure service fabric kullanarak uygulamayı dağıtın.

    ![Geçiş işlemi](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA, değerlendirmek ve bunların azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak için kullanın. DMA, SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir ve performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | Akıllı, tam olarak yönetilen bir ilişkisel bulut veritabanı hizmeti. | Özellikler, aktarım hızı ve boyutuna bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/) | Tüm kapsayıcı dağıtımı türlerinin görüntülerini Store. | Özellikler, depolama ve kullanım süresi göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Her zaman açık, ölçeklenebilir ve dağıtılmış uygulamalar oluşturun ve çalıştırın | Boyut, konum ve işlem düğümlerinin süresine bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/service-fabric/).

## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak gerekenler şu şekildedir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.
**Geliştirici önkoşulları** | Contoso, geliştirici iş istasyonunda aşağıdaki araçları gerekir:<br/><br/> - [Visual Studio 2017 Community Edition: Sürüm 15.5](https://www.visualstudio.com/)<br/><br/> -.NET iş yükünün etkinleştirilmiş.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [Service Fabric SDK'sı v 3.0 veya üstü](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) ya da Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcıları kullanacak şekilde ayarlayın.



## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure SQL veritabanı örneğinde sağlama**: Contoso azure'da bir SQL örneği sağlar. Ön uç web VM için bir Azure kapsayıcı geçirildikten sonra uygulamanın web ön ucu ile kapsayıcı örneği bu veritabanına işaret etmesini sağlayacaksınız.
> * **2. adım: Sağlama Azure Service Fabric**: Bunlar, Service Fabric kümesi sağlayın.
> * **4. adım: DMA veritabanıyla geçirme**: Bunlar veritabanı geçiş Yardımcısı'nı içeren uygulama veritabanını geçirme.
> * **5. adım: uygulamayı bir kapsayıcıya dönüştürmek**: Kullanıcılar uygulamayı Visual Studio ve SDK Tools kullanarak bir kapsayıcıya dönüştürmek.
> * **6. adım: uygulamayı yayımlama**: app Service Fabric kümesi ve ACR yayımlayın.
> * **7. adım: uygulamayı genişletmek**: uygulama genel olduktan sonra bunlar Azure özelliklerinden yararlanın ve bunu Azure'a yeniden yayımlamanız gerektiğinde genişletin.



## <a name="step-1-provision-an-azure-sql-database"></a>1. adım: Azure SQL veritabanı sağlama

1. Azure'da bir SQL veritabanı oluşturmak için seçin. 

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Bunlar şirket içi VM'de çalışan veritabanıyla eşleşmesi için bir veritabanı adı belirtin (**SmartHotel.Registration**). Bunlar veritabanı ContosoRG kaynak grubuna yerleştirin. Bu, azure'daki kaynakları üretim için kullandıkları kaynak grubudur.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Yeni bir SQL Server örneğini ayarlama ayarladıkları (**sql smarthotel eus2**) birincil bölgedeki.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Fiyatlandırma katmanını sunucularına eşleştirmek için ayarladıkları ve veritabanı gerekir. Ve zaten sahip oldukları bir SQL Server lisansı için Azure hibrit avantajı ile tasarruf seçin.
5. Boyutlandırma için bunlar v çekirdek tabanlı satın alma kullanın ve beklenen gereksinimlerine için sınırları ayarlayın.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Ardından veritabanı örneği oluşturun.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Örneği oluşturulduktan sonra veritabanı ve geçiş için veritabanı geçiş Yardım kullandığınızda ihtiyaç duydukları ayrıntılarını not edin.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- [Yardım](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bir SQL veritabanı sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) sanal çekirdek kaynak sınırları.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>2. adım: bir ACR oluşturma ve bir Azure Container sağlama

Azure container Web VM'den dışa aktarılan dosyaları kullanılarak oluşturulur. Kapsayıcıyı Azure Container Registry (ACR) barındırılır.


1. Contoso, Azure portalında kapsayıcı kayıt defteri oluşturur.

     ![Container Kayıt Defteri](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Kullanıcılar kayıt için bir ad sağlayın (**contosoacreus2**) ve altyapı kaynaklarını için kullandıkları kaynak grubunda birincil bölgeye yerleştirin. Bunlar yönetici kullanıcılar için erişimi etkinleştirmek ve coğrafi çoğaltma yararlanabilir, böylece bir premium SKU ' ayarlayın.

    ![Container Kayıt Defteri](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>3. adım: Sağlama Azure Service Fabric

SmartHotel kapsayıcı, Azure Service Fabric Sluster içinde çalışır. Contoso şu şekilde Service Fabric kümesi oluşturur:

1. Azure Marketi'nden bir Service Fabric kaynak oluştur

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. İçinde **temel**, küme için benzersiz bir DS ad ve şirket içi VM erişim için kimlik bilgilerini sağlayın. Bunlar üretim kaynak grubunda kaynak yerleştirin (**ContosoRG**) birincil Doğu ABD 2 bölgesinde.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. İçinde **düğüm türü yapılandırması**, düğüm türü adını, dayanıklılık ayarları, VM boyutu ve uygulama uç noktalarını girin.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. İçinde **anahtar kasası oluşturma**, sertifika barındırmak için kendi altyapısı kaynak grubunda yeni bir anahtar kasası oluştururlar.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. İçinde **erişim ilkeleri** , anahtar kasası dağıtmak için sanal makinelere eanble erişim.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png) 

6. Bunlar, sertifika için bir ad belirtin.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png) 

7. Özet sayfasında, sertifikayı indirmek için kullanılan bağlantıyı kopyalayın. Service Fabric kümesine bağlanmak için ihtiyaç duydukları.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png) 

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png) 

8. Doğrulama denetimini geçtikten bunlar kümesi sağlayın.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png) 

9. Sertifika Alma Sihirbazı'nda, geliştirme makinelere yüklenen sertifika alın. Sertifika, küme kimlik doğrulaması için kullanılır.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png) 

10. Küme oluşturulduktan sonra bunlar için Service Fabric kümesi Gezgini bağlanın.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png) 

11. Bunlar, doğru sertifikayı seçmeniz gerekir.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png) 

12. Küme, Service Fabric Explorer yükler ve Contoso yönetici yönetebilir.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png) 


## <a name="step-3-migrate-the-database-with-dma"></a>3. adım: DMA veritabanını geçirme

Contoso DMA kullanarak SmartHotel veritabanı geçirir.

### <a name="install-dma"></a>DMA'yı yükleme

1. Bunlar aracı indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar Kurulum (DownloadMigrationAssistant.msi) sanal makinede çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

### <a name="configure-the-firewall"></a>Güvenlik duvarını yapılandırma

Azure SQL veritabanı'na bağlanmak için bir güvenlik duvarı kuralı gereklidir.

1. İçinde **güvenlik duvarı ve sanal ağlar** özellikleri, veritabanı için Azure hizmetlerine erişime izin ver ve şirket içi SQL Server VM istemci IP adresi için bir kural ekleyin.
2. Sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

    ![Güvenlik duvarı](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Daha fazla yardıma mı ihtiyacınız var?

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#creating-and-managing-firewall-rules) oluşturma ve Azure SQL veritabanı için güvenlik duvarı kurallarını yönetme.

### <a name="migrate"></a>Geçiş

1. Yeni bir proje DMA'da oluşturun (**SmartHotelDB**) seçip **geçiş** 
2. Kaynak sunucu türü seçmeleri **SQL Server**ve hedef olarak **Azure SQL veritabanı**. 

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

3. Geçiş ayrıntıları ekledikleri **SQLVM** kaynak sunucu ve **SmartHotel.Registration** veritabanı. 

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

4. Bunlar hangi kimlik doğrulaması ile ilişkili görünüyor bir hata alırsınız. Ancak araştırdıktan sonra veritabanı adı nokta (.) sorunudur. Geçici çözüm olarak, bunlar adını kullanarak yeni bir SQL veritabanını sağlamak karar **SmartHotel kayıt**, sorunu çözmek için. DMA'yı yeniden çalıştırın, bunlar seçebilir **SmartHotel kayıt**ve sihirbazıyla devam edin.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

5. İçinde **nesneleri seçin**, veritabanı tablolarını seçin ve bir SQL betiği oluşturun.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

6. DMS komut dosyasını oluşturduktan sonra'ı tıklatın **şemayı Dağıt**.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

7. DMA, dağıtımın başarılı olduğunu onaylar.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

8. Şimdi geçişi başlatın.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

9. Geçiş tamamlandıktan sonra Contoso veritabanı Azure SQL örneğinde çalıştığını doğrulayabilirsiniz.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. Bunlar ek SQL veritabanını silin **SmartHotel.Registration** Azure portalında.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)



## <a name="step-4-convert-the-app-to-a-container"></a>4. adım: uygulamayı bir kapsayıcıya dönüştürmek.

Şirket içi uygulama, geleneksel bir üç katmanlı bir uygulama olur:

- Bu, WebForms ve SQL Server'a bağlanırken bir WCF hizmeti içerir.
- Entity Framework, SQL veritabanındaki bir WCF hizmeti aracılığıyla gösterme verileri ile tümleştirmek için kullanır.
- WebForms uygulaması, WCF Hizmeti ile etkileşime girer.

Contoso uygulaması kullanarak Visual Studio ve SDK Tools aşağıdaki gibi bir kapsayıcıya dönüştürür:

1. Bunlar, bir geliştirici makinesinde yerel olarak deposunu kopyalayın:

    **Git kopya https://github.com/Microsoft/SmartHotel360-internal-booking-apps.git**

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container1.png)

2. Visual Studio kullanarak, çözüm dosyası (SmartHotel.Registration.sln) içinde açtıklarında **SmartHotel360-iç-kayıt-apps\src\Registration** Yerel Depodaki dizin.  İki uygulama gösterilmektedir. Web ön ucu SmartHotel.Registration.Web nad SmartHotel.Registration.WCF WCF hizmeti uygulaması.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container2.png)


3. Bunlar web uygulamasına sağ tıklayın > **Ekle** > **kapsayıcı Düzenleyicisi desteği**.
4. İçinde **kapsayıcı Orkestra desteği ekleme**, seçtikleri **Service Fabric**.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container3.png)

5. Contoso SmartHotel.Registration.WCF uygulama için işlem yinelenir.
6. Şimdi, Contoso çözümü nasıl değiştiğini denetler.

    - Yeni uygulama **SmartHotel.RegistrationApplication/**
    - İki hizmet içerir: **SmartHotel.Registration.WCF** ve **SmartHotel.Registration.Web**.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container4.png)

7. Visual Studio Docker dosyası oluşturulur ve gerekli görüntüleri yerel olarak Geliştirici makinesinde çekilir.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container5.png)

8. Bir bildirim dosyası (**ServiceManifest.xml**) oluşturulur ve Visual Studio tarafından açılmış. Bu dosya, Service Fabric Azure'a dağıtıldığında kapsayıcının nasıl yapılandırılacağını söyler.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container6.png)

9. Başka bir bildirim dosyası (** ApplicationManifest.xml) kapsayıcıları için yapılandırma uygulamaları içerir.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container7.png)

## <a name="step-5-publish-the-app"></a>5. adım: uygulamayı yayımlama


Son olarak, Contoso uygulama Service Fabric kümesi ve ACR yayımlayabilirsiniz.

> [!NOTE]
> SmartHotel uygulamayı Service Fabric kümesine ilgili bazı değişiklikler yapıldı. Hem özgün hem Modernleştirilmiş uygulama kodundan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360-internal-booking-apps). Değiştirilen dosya **AppliationModern/ApplicationParameters/Cloud.xml**.


1. Visual Studio'da uygulamayı Azure SQL veritabanı'na bağlanmak için bağlantı dizesini güncelleştirme. Bağlantı dizesini Azure portalında veritabanı'nda bulunabilir.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish1.png)

2. Contoso Visual Studio kullanarak Service Fabric için uygulamanın yayınlar. Bunlar üzerinde Service Fabric uygulamaya sağ tıklayın > **Yayımla**.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish2.png)

3. Abonelik, bağlantı uç noktası ve ACR seçerler. Ardından **Yayımla**.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish3.png)

4. Dağıtım tamamlandıktan sonra SmartHotel artık Service Fabric çalıştırırsınız.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish4.png)

5. Contoso uygulamaya bağlanmak için Service Fabric düğümlerinin en genel IP adresini Azure load balancer, trafiği yönlendirir.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-6-extend-the-app-and-republish"></a>6. adım: uygulamayı genişletmek ve yeniden yayımlayın

Contoso Azure SmartHotel uygulaması ve veritabanı çalıştırıldıktan sonra uygulamayı genişletmek istiyor.

- Contoso'nun geliştiricilerin, prototip oluşturma, Service Fabric kümesinde çalışacak yeni bir .NET Core uygulaması önerilir.
- Uygulama, CosmosDB yaklaşım veri çekmek için kullanılır.
- Bu veriler, sunucusuz bir Azure işlevi ve Bilişsel hizmetler metin analizi API'sini kullanarak işlenir Tweetleri biçiminde olacaktır.

### <a name="provision-azure-cosmos-db"></a>Azure Cosmos DB sağlama

Contoso, ilk adım, bir Azure Cosmos veritabanı sağlama.

1. Bunlar, Azure Marketi'nden bir Azure Cosmos DB kaynağı oluşturun.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Bunlar bir veritabanı adı sağlayın (**contososmarthotel**), SQL API'yi seçin ve kaynak üretim kaynak grubunda birincil Doğu ABD 2 bölgesinde yerleştirin.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. İçinde **Başlarken**, seçtikleri **Veri Gezgini**ve yeni bir koleksiyon ekleyin.
4. İçinde **koleksiyon Ekle** kimlikleri sağlamak ve depolama kapasitesi ve aktarım hızı ayarlama.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. Portalda yeni bir veritabanı açtıklarında > **koleksiyon** > **belgeleri** tıklatıp **yeni belge**.
6. Bunlar aşağıdaki JSON kodunu belge penceresine yapıştırın. Örnek verileri tek bir tweet biçiminde budur.

    ```
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500, 
            "hashtags": [
                ""
            ]
    }
    ```

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend4.png)

7. Bunlar, Cosmos DB uç noktası ve kimlik doğrulama anahtarını bulun. Bunlar uygulamada koleksiyonuna bağlanmak için kullanılır. Veritabanında bunlar tıklayın **anahtarları**, URI ve birincil anahtarı Not Defteri'ne kopyalayın.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>Yaklaşım uygulamayı güncelleştirme

Sağlanan Cosmos DB ile Contoso bağlanmak için uygulamayı yapılandırabilirsiniz.

1. Visual Studio'da, Çözüm Gezgini'nde ApplicationModern\ApplicationParameters\cloud.xml dosya açın.

    ![Yaklaşım uygulaması](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Bunlar, aşağıdaki iki parametrelerini doldurun:

   ```
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Yaklaşım uygulaması](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Uygulamayı yeniden yayımlaması

Uygulama genişletildikten sonra Contoso, Azure'a yeniden yayımlar.

1. Portalda, bunlar üzerinde Service Fabric uygulaması sağ tıklayın > **Yayımla**.

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish1.png)

2. Abonelik, bağlantı uç noktası ve ACR seçerler. Ardından **Yayımla**.

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish2.png)

4. Dağıtım tamamlandıktan sonra SmartHotel artık Service Fabric çalıştırırsınız. Listeleme doku Yönetimi konsolunda, artık üç hizmetler gösterilmektedir.

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish3.png)

5. Contoso SentimentIntegration uygulamayı çalışır durumda olduğunu görmek için hizmetler yoluyla tıklayabilirsiniz

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Sanal makinelerin yerel yedekleme işlerden kaldırın.
- SmartHotel uygulama için yeni konumlar göstermek için iç belgeleri güncelleştirin. Veritabanını Azure SQL veritabanı ve Service Fabric'te çalışan olarak ön uç çalışıyor olarak göster.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.


## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

- Contoso gereksinim emin olmak, yeni **SmartHotel kayıt** veritabanı güvenlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Özellikle, kapsayıcı SSL sertifikalarıyla kullanacak şekilde güncelleştirmeniz gerekir.
- Bunlar, Service Fabric uygulamaları için gizli dizilerini korumak için KeyVault kullanarak düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups"></a>Yedeklemeler

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Bunlar veritabanı için bölgesel yük devretme sağlamak için yük devretme grupları düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Bunlar, ACR premium SKU için coğrafi çoğaltmayı yararlanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Contoso, kapsayıcılar için Web App kullanıma sunulduğunda ana Doğu ABD 2 ve orta ABD bölgesi Web uygulamasında dağıtmayı göz önünde bulundurun gerekir. Bunlar, Traffic Manager'ın yük devretme bölgesel kesintiler durumunda emin olmak için yapılandırabilirsiniz.

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını kendi [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
1. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso için Service Fabric uygulama ön uç sanal makine geçiş yaparak Azure SmartHotel uygulamada yeniden düzenlenen. Uygulama veritabanı için Azure SQL veritabanına geçirilmesi.





