---
title: Bir Azure kapsayıcı ve Azure SQL veritabanı içinde bir Contoso uygulaması yeniden oluşturma | Microsoft Docs
description: Contoso Azure Windows kapsayıcıları ve Azure SQL veritabanı bir uygulamada nasıl yeniden oluşturma hakkında bilgi edinin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: cb984bcbe79b69c0614579d66a3b853cd38a7e12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60690263"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Contoso geçişi: Bir Azure kapsayıcı ve Azure SQL veritabanı için bir şirket içi uygulamayı yeniden oluşturma

Bu makalede, Contoso nasıl geçirir ve kendi SmartHotel360 uygulamayı azure'da yeniden oluşturma gösterilmektedir. Contoso uygulaması ön uç sanal makine bir Azure Windows kapsayıcısı ve bir Azure SQL veritabanı uygulama veritabanına geçirir.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklara Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri eklenir.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir 
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: Azure Web Apps ve Azure SQL veritabanında bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulamayı yeniden düzenleme](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir 
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Bu makalede
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir 
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir

Bu makalede, iki katmanlı Windows WPF XAML forms SmartHotel360 uygulaması Azure'a VMware Vm'lerinde çalışan Contoso geçirir. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

Contoso BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyüyor ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereksiz yordamları kaldırın ve geliştiriciler ve kullanıcılar için işlemleri daha verimli hale getirin gerekiyor.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**:  Contoso BT işletme ihtiyaçlarını daha duyarlı olmamız gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: İş başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisans maliyetleri en aza indirmek istiyor.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanıldı.

**Hedefleri** | **Ayrıntılar**
--- | --- 
**Uygulama reqs** | Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır.<br/><br/> Şu anda bir VMWare içinde olduğu gibi aynı performans özelliklerine sahip olmalıdır.<br/><br/> Contoso, üzerinde uygulama şu anda çalışır ve uygulamada yatırım yapmaya hazır olduğunuz Windows Server 2008 R2, desteğini durduracak ister.<br/><br/> Contoso Yönetimi gereksinimini en aza indirecek bir modern PaaS veritabanı platform, SQL Server 2008 R2 uzağa taşımak istiyor.<br/><br/> Contoso istediğiniz mümkün olduğunda kendi SQL Server Lisanslama ve Yazılım Güvencesi yatırım yararlanın.<br/><br/> Contoso uygulaması web katmanını ölçeklendirmek atabilmek istiyor.
**Sınırlamalar** | Bir ASP.NET uygulaması ve aynı VM'de çalışan bir WCF hizmeti uygulaması oluşur. Contoso, bunu Azure App Service kullanarak iki web uygulaması arasında bölmek istiyor. 
**Azure reqs** | Contoso, uygulamayı Azure'a taşıyabilir ve uygulama yaşam genişletmek için bir kapsayıcıda çalıştırmak istiyor. Tamamen Azure'da uygulama sıfırdan başlamak istememektedir. 
**DevOps** | Azure DevOps Hizmetleri için kod oluşturur bir DevOps kullanarak modelinize taşıyın ve yayın işlem contoso istiyor.

## <a name="solution-design"></a>Çözüm tasarımı

Hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve Contoso geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-app"></a>Geçerli uygulama

- Smarthotel360'ı şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-architecture"></a>Önerilen mimarisi

- SQL Server kullanarak uygulama veritabanı katmanı için Contoso Azure SQL veritabanı karşılaştırıldığında [bu makalede](https://docs.microsoft.com/azure/sql-database/sql-database-features). Bunun birkaç nedeni Azure SQL veritabanı ile karar verdim:
    - Azure SQL veritabanı bir ilişkisel veritabanı yönetilen hizmetidir. Bu, yönetime neredeyse hiç ile birden fazla hizmet düzeyinde öngörülebilir performans sunar. Hiç kapalı kalma süresi, yerleşik zeka iyileştirmesi ve global ölçeklenebilirlik ve kullanılabilirlik ile dinamik ölçeklenebilirlik sayılabilir.
    - Contoso basit Data Migration Yardımcısı'nı (değerlendirmek ve şirket içi veritabanını Azure SQL'e geçirme için DMA) yararlanır.
    - Yazılım Güvencesi içeren SQL Server için Azure hibrit Avantajı'ı kullanarak bir SQL veritabanı, indirimli Fiyatlardan için var olan lisansları Contoso değiştirebilir. Bu değer % 30 tasarruf sağlayabilir.
    - SQL veritabanı, her zaman şifreli, dinamik veri maskeleme ve satır düzeyi güvenlik/tehdit algılama gibi güvenlik özellikleri sağlar.
- Uygulama web katmanı için Contoso verdi, Azure DevOps Hizmetleri'ni kullanarak Windows kapsayıcısına dönüştürme.
    - Contoso Azure Service Fabric kullanarak uygulamayı dağıtma ve Azure Container Registry (ACR) gelen Windows kapsayıcı görüntüsü çekin.
    - Bir prototip uygulamasının yaklaşım analizi de içerecek şekilde genişletmek için Service Fabric, Cosmos DB'ye bağlı başka bir hizmet olarak uygulanacaktır.  Bu bilgileri Tweetleri okuyun ve üzerinde uygulamayı görüntüleyin.
- DevOps işlem hattı uygulamak için Contoso Azure DevOps kaynak kodu Yönetimi (SCM), Git depoları için kullanır.  Otomatik derleme ve yayınlar kod oluşturmak için kullanılan ve Azure Service Fabric ve Azure Container Registry'yi dağıtma.

    ![Senaryo mimarisi](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Çözümü gözden geçirme
Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarım değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | SmartHotel360 uygulama kodu, Azure Service fabric'e geçiş için değiştirilmesi gerekir. Ancak, değişiklikler için Service Fabric SDK araçları kullanarak, minimal istenmektedir.<br/><br/> Service Fabric için taşıma ile hızlı bir şekilde, özgün kod tabanının riski olmadan zaman içinde uygulama eklemek için mikro hizmet geliştirin Contoso başlayabilirsiniz.<br/><br/> Windows kapsayıcıları, genel kapsayıcılarıyla aynı avantajları sunar. Bunlar, çevikliği, taşınabilirlik ve denetimi artırın.<br/><br/> Contoso, Yazılım Güvencesi'nın SQL Server ve Windows Server için Azure hibrit teklifi kullanarak kendi yatırım yararlanabilirsiniz.<br/><br/> Geçişten sonra Windows Server 2008 R2 desteklemek artık gerekir. [Daha fazla bilgi edinin](https://support.microsoft.com/lifecycle).<br/><br/> Artık tek başarısızlık noktası değildir, Contoso web katmanı uygulamasının birden çok örnek ile yapılandırabilirsiniz.<br/><br/> Artık bağımlı eskime SQL Server 2008 R2 üzerinde olacaktır.<br/><br/> SQL veritabanı teknik gereksinimler Contoso'nun destekler. Contoso yöneticileri veritabanı geçiş Yardımcısı'nı kullanarak şirket içi veritabanı değerlendirilen ve uyumlu bulunamadı.<br/><br/> SQL veritabanı, Contoso ayarlanan gerekmez yerleşik hata toleransı vardır. Bu veri katmanı artık tek bir yük devretme noktası olmasını sağlar.
**Simgeler** | Diğer geçiş seçenekleri daha karmaşık kapsayıcılardır. Öğrenme eğrisini kapsayıcılarına, Contoso için bir sorun olabilir.  Bunlar, yeni bir eğri artma değerli birçok sağlayan bir karmaşıklık düzeyi sunar.<br/><br/> Contoso'da operasyon ekibinin, anlamak ve Azure, kapsayıcılar ve mikro hizmetler uygulama için destek başlamasını gerekir.<br/><br/> Contoso Data Migration Yardımcısı yerine veri geçiş hizmeti veritabanını geçirmek için kullanıyorsa, geçiş için hazır bir altyapı olmaz veritabanlarını ölçeğe.



### <a name="migration-process"></a>Geçiş işlemi

1. Contoso için Windows Azure service fabric kümesi sağlar.
2. Bir Azure SQL örneğine hazırlar ve SmartHotel360 veritabanı için geçirir.
3. Contoso Web Katmanı VM Service Fabric SDK araçları kullanarak bir Docker kapsayıcısı dönüştürür.
4. Service fabric kümesi ve ACR bağlanır ve Azure service fabric kullanarak uygulamayı dağıtır.

    ![Geçiş işlemi](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak ve değerlendirir. DMA, SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir ve performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | Bir akıllı, tam olarak yönetilen bir ilişkisel bulut veritabanı hizmeti sağlar. | Özellikler, aktarım hızı ve boyutuna bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Tüm kapsayıcı dağıtımı türlerinin görüntülerini depolar. | Özellikler, depolama ve kullanım süresi göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Yapıları ve her zaman açık, ölçeklenebilir ve dağıtılmış uygulamaları çalıştırma | Boyut, konum ve işlem düğümlerinin süresine bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/service-fabric/).
[Azure DevOps](https://docs.microsoft.com/azure/azure-portal/tutorial-azureportal-devops) | Sürekli tümleştirme ve sürekli dağıtım (CI/CD) işlem hattı için uygulama geliştirme sağlar. İşlem hattı uygulama kodu, paketleri ve diğer derleme yapıtlarının üretmek için bir yapı sistemi ve değişiklikleri geliştirme, test ve üretim ortamlarına dağıtmak için bir Release Management sisteminden yönetmek için bir Git deposu başlar.

## <a name="prerequisites"></a>Önkoşullar

Bu senaryo çalıştırmak için Contoso gerekenler şu şekildedir:

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso, daha önce bu makale serisinde abonelik oluşturuldu. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso daha önce bir Azure altyapıyı ayarlayın.
**Geliştirici önkoşulları** | Contoso, geliştirici iş istasyonunda aşağıdaki araçları gerekir:<br/><br/> - [Visual Studio 2017 Community Edition: Sürüm 15.5](https://www.visualstudio.com/)<br/><br/> -.NET iş yükünün etkinleştirilmiş.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [Service Fabric SDK'sı v 3.0 veya üstü](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) ya da Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcıları kullanacak şekilde ayarlayın.



## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalıştığını şu şekildedir:

> [!div class="checklist"]
> * **1. adım: Azure SQL veritabanı örneğinde sağlama**: Contoso, azure'da bir SQL örneği sağlar. Ön uç web VM için bir Azure kapsayıcı geçirildikten sonra uygulamanın web ön ucu ile kapsayıcı örneği bu veritabanına işaret etmesini sağlayacaksınız.
> * **2. adım: Azure Container Registry (ACR) oluşturma**: Contoso bir kurumsal kapsayıcı kayıt defteri docker kapsayıcı görüntüleri için hazırlar.
> * **3. adım: Azure Service Fabric'e sağlama**: Bu, Service Fabric kümesi sağlar.
> * **4. adım: Service fabric sertifikaları yönetme**: Azure DevOps Hizmetleri erişim küme için sertifikaları contoso ayarlar.
> * **5. adım: DMA veritabanıyla geçirme**: Bu veritabanı geçiş Yardımcısı'nı uygulama veritabanıyla geçirir.
> * **6. adım: Azure DevOps Hizmetleri'ni ayarlama**: Contoso, Azure DevOps Hizmetleri'ndeki yeni bir proje ayarlar ve kod Git deposuna içeri aktarır.
> * **7. adım: Uygulamayı dönüştürmek**: Contoso uygulaması, Azure DevOps ve SDK araçlarını kullanarak bir kapsayıcıya dönüştürür.
> * **8. adım: Derleme ve yayın ayarlama**: Contoso oluşturmak ve uygulamayı Service Fabric kümesi ve ACR yayımlamak için derleme ve yayın işlem hatları ayarlar.
> * **9. adım: Uygulamayı genişletmek**: Uygulama genel sonra Contoso Azure özelliklerinden yararlanmak için genişletir ve bu işlem hattını kullanarak Azure'a yeniden yayımlar.



## <a name="step-1-provision-an-azure-sql-database"></a>1. Adım: Bir Azure SQL veritabanı sağlama

Contoso yöneticileri, bir Azure SQL veritabanı sağlama.

1. Oluşturmak için seçtikleri bir **SQL veritabanı** azure'da. 

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Bunlar şirket içi VM'de çalışan veritabanıyla eşleşmesi için bir veritabanı adı belirtin (**SmartHotel.Registration**). Bunlar veritabanı ContosoRG kaynak grubuna yerleştirin. Bu, azure'daki kaynakları üretim için kullandıkları kaynak grubudur.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Yeni bir SQL Server örneğini ayarlama ayarladıkları (**sql smarthotel eus2**) birincil bölgedeki.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Bunlar, sunucu ve veritabanı ihtiyaçlarını karşılamak için fiyatlandırma katmanını ayarlayın. Ve zaten sahip oldukları bir SQL Server lisansı için Azure hibrit avantajı ile tasarruf seçin.
5. Boyutlandırma için bunlar v çekirdek tabanlı satın alma kullanın ve beklenen gereksinimleri için sınırları ayarlayın.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. Ardından veritabanı örneği oluşturun.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Örneği oluşturulduktan sonra veritabanı ve geçiş için veritabanı geçiş Yardım kullandığınızda ihtiyaç duydukları ayrıntılarını not edin.

    ![SQL sağlama](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- [Yardım](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bir SQL veritabanı sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) sanal çekirdek kaynak sınırları.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>2. Adım: Bir ACR oluşturma ve bir Azure Container sağlama

Azure container Web VM'den dışa aktarılan dosyaları kullanılarak oluşturulur. Kapsayıcıyı Azure Container Registry (ACR) barındırılır.


1. Contoso yöneticileri Azure portalında kapsayıcı kayıt defteri oluşturun.

     ![Container Kayıt Defteri](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Kullanıcılar kayıt için bir ad sağlayın (**contosoacreus2**) ve altyapı kaynaklarını için kullandıkları kaynak grubunda birincil bölgeye yerleştirin. Bunlar yönetici kullanıcılar için erişimi etkinleştirmek ve coğrafi çoğaltma yararlanabilir, böylece bir premium SKU ' ayarlayın.

    ![Container Kayıt Defteri](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>3. Adım: Azure Service Fabric'e sağlama

SmartHotel360 kapsayıcı, Azure Service Fabric kümesinde çalıştırılır. Contoso yöneticileri, Service Fabric kümesi gibi oluşturun:

1. Azure Marketi'nden bir Service Fabric kaynak oluştur

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. İçinde **Temelleri**, küme için benzersiz bir DS ad ve şirket içi VM erişim için kimlik bilgilerini sağlayın. Bunlar üretim kaynak grubunda kaynak yerleştirin (**ContosoRG**) birincil Doğu ABD 2 bölgesinde.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. İçinde **düğüm türü yapılandırması**, düğüm türü adını, dayanıklılık ayarları, VM boyutu ve uygulama uç noktalarını girin.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. İçinde **anahtar kasası oluşturma**, sertifika barındırmak için kendi altyapısı kaynak grubunda yeni bir anahtar kasası oluştururlar.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. İçinde **erişim ilkeleri** anahtar kasası dağıtmak için sanal makinelere tanırlar.

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


## <a name="step-4-manage-service-fabric-certificates"></a>4. Adım: Service Fabric sertifikaları yönetme

Contoso küme Azure DevOps Hizmetleri erişmesine izin vermek için küme sertifikası gerekir. Bunu ayarlamanız contoso yöneticileri.

1. Bunlar, Azure portalını açın ve anahtar kasası için göz atın.
2. Bunlar, sertifikalar ve sağlama işlemi sırasında oluşturulan sertifikanın parmak izini kopyalayın.

    ![Parmak izini kopyalayın](./media/contoso-migration-rearchitect-container-sql/cert1.png)
 
3. Bunlar daha sonra başvurmak için bir metin dosyasına kopyalayın.
4. Şimdi küme üzerinde bir yönetici istemci sertifikası olacak bir istemci sertifikası ekleyin. Bu uygulama dağıtım yayın ardışık düzeni kümeye bağlanmak, Azure DevOps hizmetleri sağlar. Yapmak için bunlar, bunlar KeyVault portalda açın ve seçin **sertifikaları** > **Oluştur/içeri aktarma**.

    ![İstemci sertifikası oluşturma](./media/contoso-migration-rearchitect-container-sql/cert2.png)

5. Sertifika adını girin ve bir X.509 ayırt edici adı belirtin **konu**.

     ![Sertifika adı](./media/contoso-migration-rearchitect-container-sql/cert3.png)

6. Sertifika oluşturulduktan sonra bunu yerel olarak PFX biçiminde indirin.

     ![Sertifikayı indir](./media/contoso-migration-rearchitect-container-sql/cert4.png)

7. Şimdi, bunlar KeyVault sertifikaları listesine geri dönün ve yeni oluşturduğunuz istemci sertifikasının parmak izini kopyalayın. Bunlar metin dosyasına kaydedin.

     ![İstemci sertifikası parmak izi](./media/contoso-migration-rearchitect-container-sql/cert5.png)

8. Azure DevOps Hizmetleri dağıtımı için Sertifikayı Base64 değerini belirlemek ihtiyaç duydukları. PowerShell kullanarak yerel geliştirici iş istasyonu bunu. Bunlar, çıktı daha sonra kullanmak için bir metin dosyasına yapıştırın.

    ```powershell
        [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes("C:\path\to\certificate.pfx")) 
    ```

     ![Base64 değeri](./media/contoso-migration-rearchitect-container-sql/cert6.png)

9. Son olarak, bunlar yeni sertifikayı Service Fabric kümesine ekleyin. Kümeyi açın ve'a tıklayın portalda Bunu yapmak için **güvenlik**.

     ![İstemci sertifikası Ekle](./media/contoso-migration-rearchitect-container-sql/cert7.png)

10. Simgeye **Ekle** > **yönetici istemci**ve yeni bir istemci sertifikası parmak izini yapıştırın. Bunlar ardından **Ekle**. Bu işlem, 15 dakika kadar sürebilir.

     ![İstemci sertifikası Ekle](./media/contoso-migration-rearchitect-container-sql/cert8.png)

## <a name="step-5-migrate-the-database-with-dma"></a>5. Adım: DMA veritabanını geçirme

Contoso yöneticileri artık DMA kullanarak SmartHotel360 veritabanı geçirebilirsiniz.

### <a name="install-dma"></a>DMA'yı yükleme

1. Bunlar aracı indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar Kurulum (DownloadMigrationAssistant.msi) sanal makinede çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

### <a name="configure-the-firewall"></a>Güvenlik duvarını yapılandırma

Azure SQL veritabanı'na bağlanmak için erişim izni vermek için bir güvenlik duvarı kuralı Contoso yöneticileri ayarlayın.

1. İçinde **güvenlik duvarı ve sanal ağlar** özellikleri, veritabanı için Azure hizmetlerine erişime izin ver ve şirket içi SQL Server VM istemci IP adresi için bir kural ekleyin.
2. Sunucu düzeyinde güvenlik duvarı kuralı oluşturulur.

    ![Güvenlik duvarı](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

Daha fazla yardıma mı ihtiyacınız var?

[Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) oluşturma ve Azure SQL veritabanı için güvenlik duvarı kurallarını yönetme.

### <a name="migrate"></a>Geçiş

Contoso yöneticileri artık veritabanına geçirin.

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


## <a name="step-6-set-up-azure-devops-services"></a>6. Adım: Azure DevOps Services'ı ayarlama

Contoso uygulaması için işlem hatları ve DevOps altyapı oluşturmak gerekir.  Bunu yapmak için Contoso yöneticileri yeni bir Azure DevOps projesi oluşturun, kendi kodlarını alma ve ardından derleme ve yayın işlem hatları.

1.   Bunlar Contoso Azure DevOps hesabında yeni bir proje oluşturun (**ContosoSmartHotelRearchitect**) seçip **Git** sürüm denetimi.
![Yeni Proje](./media/contoso-migration-rearchitect-container-sql/vsts1.png)

2. Bunlar, şu anda uygulama kodlarını tutan Git deposunu içeri aktarma. İçinde bir [genel deponun](https://github.com/Microsoft/SmartHotel360-internal-booking-apps) ve indirebilirsiniz.

    ![Uygulama kodu indirin](./media/contoso-migration-rearchitect-container-sql/vsts2.png)

3. Kod içeri aktardıktan sonra bunlar Visual Studio depoya bağlanın ve Takım Gezgini kullanarak kodu kopyalayın.

4. Deponun bir geliştirici makinesinde kopyasını sonra uygulama için çözüm dosyasını açın. Her web app ve wcf hizmeti projesi dosyası içinde ayırın.

    ![Çözüm dosyası](./media/contoso-migration-rearchitect-container-sql/vsts4.png)

## <a name="step-7-convert-the-app-to-a-container"></a>7. Adım: Uygulamayı bir kapsayıcısına dönüştürme

Şirket içi uygulama, geleneksel bir üç katmanlı bir uygulama olur:

- Bu, WebForms ve SQL Server'a bağlanırken bir WCF hizmeti içerir.
- Entity Framework, SQL veritabanındaki bir WCF hizmeti aracılığıyla gösterme verileri ile tümleştirmek için kullanır.
- WebForms uygulaması, WCF Hizmeti ile etkileşime girer.

Contoso yöneticileri uygulamayı Visual Studio ve SDK Tools kullanarak, aşağıdaki gibi bir kapsayıcıya dönüştürür:


1. Visual Studio kullanarak, bunlar açık çözüm dosyası (SmartHotel.Registration.sln) gözden geçirin **SmartHotel360-iç-kayıt-apps\src\Registration** Yerel Depodaki dizin.  İki uygulama gösterilmektedir. Web ön ucu SmartHotel.Registration.Web ve WCF hizmet uygulaması SmartHotel.Registration.WCF.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container2.png)


2. Bunlar web uygulamasına sağ tıklayın > **Ekle** > **kapsayıcı Düzenleyicisi desteği**.
3. İçinde **kapsayıcı Orkestra desteği ekleme**, seçtikleri **Service Fabric**.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container3.png)
    
4. Bunlar, SmartHotel.Registration.WCF uygulama için işlemi tekrarlayın.
5. Şimdi, bunların çözümü nasıl değiştiğini kontrol edin.

   - Yeni uygulama **SmartHotel.RegistrationApplication/**
   - Bu iki hizmet de içerir: **SmartHotel.Registration.WCF** ve **SmartHotel.Registration.Web**.

     ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container4.png)

6. Visual Studio Docker dosyası oluşturulur ve gerekli görüntüleri yerel olarak Geliştirici makinesinde çekilir.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container5.png)

7. Bir bildirim dosyası (**ServiceManifest.xml**) oluşturulur ve Visual Studio tarafından açılmış. Bu dosya, Service Fabric Azure'a dağıtıldığında kapsayıcının nasıl yapılandırılacağını söyler.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container6.png)

8. Başka bir bildirim dosyası (** ApplicationManifest.xml) kapsayıcıları için yapılandırma uygulamaları içerir.

    ![Kapsayıcı](./media/contoso-migration-rearchitect-container-sql/container7.png)

9. Açtıklarında **ApplicationParameters/Cloud.xml** dosya ve uygulama Azure SQL veritabanına bağlanmak için bağlantı dizesini güncelleştirin. Bağlantı dizesini Azure portalında veritabanında yer alabilir.

    ![Bağlantı dizesi](./media/contoso-migration-rearchitect-container-sql/container8.png)

10. Güncelleştirilen kodun işlenmesinden ya Azure DevOps Services'a iletin.

    ![İşleme](./media/contoso-migration-rearchitect-container-sql/container9.png)

## <a name="step-8-build-and-release-pipelines-in-azure-devops-services"></a>8. adım: Derleme ve yayın işlem hatları Azure DevOps Hizmetleri'nde

Contoso yöneticileri artık yapı gerçekleştirin ve eylem işlemine DevOps uygulamalarını serbest bırakmak için Azure DevOps Hizmetleri yapılandırın.

1. Azure DevOps Hizmetleri'nde bunlar tıklayın **derleme ve yayın** > **yeni işlem hattı**.

    ![Yeni ardışık düzen](./media/contoso-migration-rearchitect-container-sql/pipeline1.png)

2. Seçmeleri **Azure DevOps Hizmetleri Git** ve ilgili depo.

    ![Git ve depo](./media/contoso-migration-rearchitect-container-sql/pipeline2.png)

3. İçinde **bir şablon seçin**, bunlar seçin doku ile Docker desteği.

     ![Docker ve yapısı](./media/contoso-migration-rearchitect-container-sql/pipeline3.png)
    
4. Eylem etiketi görüntülerin değiştirdiğinden **bir görüntü oluşturun**ve görev sağlanan ACR kullanmak için yapılandırın.

     ![Kayıt Defteri](./media/contoso-migration-rearchitect-container-sql/pipeline4.png)

5. İçinde **görüntüleri itme** görev, bunlar görüntüyü ACR'ye itilecek yapılandırın ve en son etiket eklemek için seçin.
6. İçinde **Tetikleyicileri**, sürekli tümleştirme çözümünü ve ana dal ekleyin.

    ![Tetikleyiciler](./media/contoso-migration-rearchitect-container-sql/pipeline5.png)

7. Simgeye **Kaydet ve kuyruğa al** derlemeyi başlatmak için.
8. Derleme başarılı olduktan sonra yayın işlem hattı taşıyın. Azure DevOps bunlar Hizmetleri **yayınlar** > **yeni işlem hattı**.

    ![Yayın ardışık düzeni](./media/contoso-migration-rearchitect-container-sql/pipeline6.png)    

9. Seçmeleri **Azure Service Fabric dağıtımı** şablonu ve adını aşama (**SmartHotelSF**).

    ![Ortam](./media/contoso-migration-rearchitect-container-sql/pipeline7.png)

10. Bunlar bir işlem hattı adı sağlayın (**ContosoSmartHotel360Rearchitect**). Aşama için bunlar tıklayın **1 iş, 1 görev** Service Fabric dağıtımını yapılandırmak için.

    ![Aşama ve görev](./media/contoso-migration-rearchitect-container-sql/pipeline8.png)

11. Şimdi, tıklayın **yeni** yeni küme bağlantısı eklemek için.

    ![Yeni bağlantı](./media/contoso-migration-rearchitect-container-sql/pipeline9.png)

12. İçinde **ekleme Service Fabric hizmeti bağlantısı**, bağlantı ve uygulama dağıtmak için Azure DevOps Hizmetleri tarafından kullanılacak kimlik doğrulama ayarlarını yapılandırın. Küme uç noktasına, Azure portalında bulunabilir ve bunlar ekleme **tcp: / /** öneki olarak.
13. Topladıkları sertifika bilgileri de giriş **sunucu sertifikası parmak izi** ve **istemci sertifikası**.

    ![Sertifika](./media/contoso-migration-rearchitect-container-sql/pipeline10.png)

13. Bunlar işlem hattına tıklayın > **bir yapıt ekleme**.

     ![Yapay Nesne](./media/contoso-migration-rearchitect-container-sql/pipeline11.png)

14. Bunlar, projeyi seçin ve en son sürümünü kullanarak bir işlem hattı, derleme.

     ![Oluşturma](./media/contoso-migration-rearchitect-container-sql/pipeline12.png)

15. Yapıt üzerinde Şimşek denetlenir unutmayın.

     ![Yapı durumu](./media/contoso-migration-rearchitect-container-sql/pipeline13.png)

16. Ayrıca, sürekli dağıtım tetikleyicisi etkin olduğunu unutmayın.

    ![Sürekli dağıtım etkin](./media/contoso-migration-rearchitect-container-sql/pipeline14.png) 

17. Simgeye **Kaydet** > **yayınlamaya**.

    ![Yayınla](./media/contoso-migration-rearchitect-container-sql/pipeline15.png)

18. Dağıtım tamamlandıktan sonra SmartHotel360 artık Service Fabric çalıştırırsınız.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish4.png)

19. Uygulamaya bağlanmak için bunlar trafiği Service Fabric düğümleri önündeki Azure yük dengeleyici genel IP adresine yönlendirir.

    ![Yayımlama](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-9-extend-the-app-and-republish"></a>9. adım: Uygulamayı genişletmek ve yeniden yayımlama

SmartHotel360 uygulaması ve veritabanı Azure'da çalıştırıldıktan sonra Contoso uygulamayı genişletmek istiyor.

- Contoso'nun geliştiricilerin, prototip oluşturma, Service Fabric kümesinde çalışacak yeni bir .NET Core uygulaması önerilir.
- Uygulama, CosmosDB yaklaşım veri çekmek için kullanılır.
- Bu veriler, sunucusuz bir Azure işlevi ve Bilişsel hizmetler metin analizi API'sini kullanarak işlenir Tweetleri biçiminde olacaktır.

### <a name="provision-azure-cosmos-db"></a>Azure Cosmos DB sağlama

İlk adım, bir Azure Cosmos veritabanı Contoso yöneticileri sağlayın.

1. Bunlar, Azure Marketi'nden bir Azure Cosmos DB kaynağı oluşturun.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Bunlar bir veritabanı adı sağlayın (**contososmarthotel**), SQL API'yi seçin ve kaynak üretim kaynak grubunda birincil Doğu ABD 2 bölgesinde yerleştirin.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. İçinde **Başlarken**, seçtikleri **Veri Gezgini**ve yeni bir koleksiyon ekleyin.
4. İçinde **koleksiyon Ekle** kimlikleri sağlamak ve depolama kapasitesi ve aktarım hızı ayarlama.

    ![Genişletme](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. Portalda yeni bir veritabanı açtıklarında > **koleksiyon** > **belgeleri** tıklatıp **yeni belge**.
6. Bunlar aşağıdaki JSON kodunu belge penceresine yapıştırın. Örnek verileri tek bir tweet biçiminde budur.

    ```json
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel360",
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

Sağlanan Cosmos DB ile uygulama bağlanmak için Contoso Yöneticiler yapılandırabilir.

1. Visual Studio'da, Çözüm Gezgini'nde ApplicationModern\ApplicationParameters\cloud.xml dosya açın.

    ![Yaklaşım uygulaması](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Bunlar, aşağıdaki iki parametrelerini doldurun:

   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```xml
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Yaklaşım uygulaması](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Uygulamayı yeniden yayımlaması

Uygulama genişletildikten sonra Contoso yöneticileri, işlem hattını kullanarak Azure'a yeniden yayımlayın.

1. Bunlar, yürütme ve kodlarını Azure DevOps Services'a iletin. Bu, derleme ve yayın işlem hattını başlatıyor.

2. Derleme ve dağıtım bittikten sonra SmartHotel360 artık Service Fabric çalıştırırsınız. Service Fabric Yönetim Konsolu, artık üç hizmeti de gösterir.

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish3.png)

3. Artık SentimentIntegration uygulamayı çalışır durumda olduğunu görmek için hizmetleriyle de tıklayabilirsiniz

    ![Yeniden yayımlama](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Sanal makinelerin yerel yedekleme işlerden kaldırın.
- SmartHotel360 uygulama için yeni konumlar göstermek için iç belgeleri güncelleştirin. Veritabanını Azure SQL veritabanı ve Service Fabric'te çalışan olarak ön uç çalışıyor olarak göster.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.


## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

- Contoso yöneticilerinin gerekir emin olmak, yeni **SmartHotel kayıt** veritabanı güvenlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Özellikle, kapsayıcı SSL sertifikalarıyla kullanacak şekilde güncelleştirmeniz gerekir.
- Bunlar, Service Fabric uygulamaları için gizli dizilerini korumak için KeyVault kullanarak düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups"></a>Yedeklemeler

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso yöneticileri için veritabanı bölgesel yük devretme sağlamak için yük devretme grupları kullanmayı düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Bunlar, ACR premium SKU için coğrafi çoğaltmayı yararlanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Contoso, kapsayıcılar için Web App kullanıma sunulduğunda ana Doğu ABD 2 ve orta ABD bölgesi Web uygulamasında dağıtmayı göz önünde bulundurun gerekir. Contoso yöneticileri, Traffic Manager'ın yük devretme bölgesel kesintiler durumunda emin olmak için yapılandırabilirsiniz.
- Cosmos DB otomatik olarak yedekler. Contoso [okuyun](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore) daha fazla bilgi için bu işlemi.

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

## <a name="conclusion"></a>Sonuç

Bu makalede, azure'da SmartHotel360 app Service Fabric'e uygulama ön uç sanal makine geçiş yaparak Contoso yeniden düzenlenen. Uygulama veritabanı için bir Azure SQL veritabanı olarak geçirildi.





