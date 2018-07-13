---
title: Contoso şirket içi uygulamasını azure'a yeniden | Microsoft Docs
description: Contoso bir uygulamayı Azure App Services, Kubernetes hizmeti, CosmosDB, Azure işlevleri ve Bilişsel hizmetler kullanarak azure'a nasıl oluşturur? öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/09/2018
ms.author: raynew
ms.openlocfilehash: 0d195d5fbede3100c0474ae9614a880cfb3acb19
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39005008"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Contoso geçiş: şirket içi bir uygulamayı Azure'da yeniden oluşturun

Bu makalede, Contoso nasıl geçirir ve bunların Azure SmartHotel uygulamada oluşturur gösterilmektedir. Bunlar, Azure App Services Web uygulamaları için uygulamanın ön uç VM geçirin. Azure Kubernetes Service (AKS) tarafından yönetilen kapsayıcıları dağıtılmış mikro hizmetler kullanarak uygulama arka ucu oluşturulur. Site evcil hayvan fotoğraf işlevsellik sağlayan Azure işlevleri ile etkileşime girer. 

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarından bazılarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso geçirme SmartHotel uygulama yalnızca Site RECOVERY'yi kullanarak VM'lerin nasıl gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve SQL Server Always On kullanılabilirlik grubu için bir uygulama barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL için bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Nasıl geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımı geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
Makale 13: bir uygulamayı Azure'da yeniden oluşturun. | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Bu makalede.

Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek. Bunlar, müşterilerine kendi Web sitelerinde için fark yaratan deneyimler sunmak istiyorsunuz.
- **Çeviklik**: Contoso sağlayabilmelidir Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki verin. 
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisanslama maliyetlerini azaltmak istemektedir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekibi, uygulama gereksinimleri bu geçiş için aşağı sabitlenmiş. Bu gereksinimler, en iyi geçiş yöntemini belirlemek için kullanılıyordu:
 - Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır. İyi performans göstermelerini ve ölçeği kolayca gerekir.
 - Uygulama Iaas bileşenleri kullanmamanız gerekir. Her şeyi PaaS veya sunucusuz Hizmetleri oluşturulmalıdır.
 - Uygulama yapıları bulut hizmetlerindeki çalışması gerektiğini ve bulutta özel Kurumsal Çapta kapsayıcı kayıt defterindeki kapsayıcı bulunmalıdır.
 - Uygulama tarafından alınan kararları, Oteller kabul gerekir bu yana evcil hayvan fotoğraflar için kullanılan API hizmeti doğru ve güvenilir, gerçek dünyadaki olmalıdır. Herhangi bir evcil hayvan Oteller kalmak için erişime izin verildi.

## <a name="solution-design"></a>Çözüm tasarımı

Kendi hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve bunların geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi belirler.

### <a name="current-app"></a>Geçerli uygulama

- SmartHotel şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-architecture"></a>Önerilen mimarisi

- Ön uç uygulaması, kullanıcıların birincil bölgede bir Azure App Services Web uygulaması olarak dağıtılır.
- Evcil hayvan fotoğrafları karşıya yükleme bir Azure işlevi sağlar ve site bu işlevsellikle etkileşimde bulunacak.
- Evcil hayvan fotoğraf işlevi, Bilişsel hizmetler görüntü işleme API'si ve CosmosDB yararlanır.
- Sitenin arka uç, mikro hizmetler kullanılarak oluşturulmuştur. Bu yönetilen Azure Kubernetes Service'i (AKS) kapsayıcıları dağıtılır.
- Kapsayıcılar VSTS kullanılarak oluşturulan ve Azure Container Registry (ACR) için gönderildi.
- Şimdilik, Contoso Web app ve işlev kodunuzu Visual Studio kullanarak el ile dağıtır.
- Mikro hizmetler, Kubernetes komut satırı araçları çağıran bir PowerShell Betiği kullanılarak dağıtılır.

    ![Senaryo mimarisi](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Çözümü gözden geçirme
Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarımlarına değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | PaaS ve sunucusuz çözümler için uçtan uca dağıtım önemli ölçüde kullanarak Contoso sağlamalısınız Yönetimi zamanı azaltır.<br/><br/> Mikro hizmet mimarisi için taşıma kendi çözümü zaman içinde kolayca uzatmanın Contoso sağlar.<br/><br/> Yeni işlevsellik mevcut çözümleri kod tabanlarında bozmadan çevrimiçi getirilebilir.<br/><br/> Hiçbir tek hata noktası ile birden çok örneği ile Web uygulaması yapılandırılacaktır.<br/><br/> Otomatik ölçeklendirme, uygulama trafiği hacimlere işleyebilmeniz etkinleştirilecektir.<br/><br/> Contoso taşıma PaaS Hizmetleri ile Windows Server 2008 R2 işletim sisteminde çalışan güncel çözümlerini devre dışı bırakabilirsiniz.<br/><br/> CosmosDB, Contoso tarafından hiçbir yapılandırma gerektiren, yerleşik hata toleransı vardır. Bu, veri katmanı artık tek bir yük devretme noktası olduğu anlamına gelir.
**Simgeler** | Diğer geçiş seçenekleri daha karmaşık kapsayıcılardır. Öğrenme eğrisini, Contoso için bir sorun olabilir.  Bunlar, yeni bir eğri artma değerli birçok sağlayan bir karmaşıklık düzeyi sunar.<br/><br/> Contoso'da operasyon ekibinin, anlamak ve Azure, kapsayıcılar ve mikro hizmetler uygulama için destek başlamasını gerekir.<br/><br/> Contoso DevOps için tüm çözüm tam olarak uygulanan edilmemiş. Bunlar, hizmetlerin AKS, İşlevler ve uygulama Hizmetleri dağıtım için düşünmeniz gerekir.



### <a name="migration-process"></a>Geçiş işlemi

1. Contoso, ACR, AKS ve CosmosDB sağlayın.
2. Bunlar, Azure Web uygulaması, depolama hesabı, işlevi ve API dahil olmak üzere dağıtımı için altyapı sağlayın. 
3. Altyapısı yerleştirildikten sonra mikro hizmetler kullanarak bunları ACR'ye gönderim VSTS, kapsayıcı görüntüleri oluşturacaksınız.
4. Contoso Bu mikro hizmetler için bir PowerShell betiğini kullanarak ASK dağıtır.
5. Son olarak, bunlar Azure işlevi ve Web uygulaması dağıtacaksınız.

    ![Geçiş işlemi](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Kubernetes yönetim, dağıtımı ve işlemleri basitleştirir. Tam olarak yönetilen bir Kubernetes kapsayıcı düzenleme hizmeti sağlar.  | AKS ücretsiz bir hizmettir.  Yalnızca sanal makineler ve ilişkili depolama ve kullanılan ağ kaynakları için ödeme yaparsınız. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Azure İşlevleri](https://azure.microsoft.com/services/functions/) | Bir olay odaklı, sunucusuz bilgi işlem deneyimiyle geliştirme hızlandırır. İsteğe bağlı olarak ölçeklendirin.  | Yalnızca tüketilen kaynaklar için ödeme yaparsınız. Planı, saniye başına kaynak tüketimi ve yürütme göre faturalandırılır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/functions/).
[Azure kapsayıcı kayıt defteri](https://azure.microsoft.com/services/container-registry/) | Tüm kapsayıcı dağıtımı türlerinin görüntülerini depolar. | Özellikler, depolama ve kullanım süresi göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Herhangi bir platformda çalışan kurumsal sınıf web, mobil ve API uygulamalarını hızlı bir şekilde oluşturun, dağıtın ve ölçeklendirin. | App Service planları, saniyelik olarak faturalandırılır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak gerekenler şu şekildedir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.
**Geliştirici önkoşulları** | Contoso, geliştirici iş istasyonunda aşağıdaki araçları gerekir:<br/><br/> - [Visual Studio 2017 Community Edition: Sürüm 15.5](https://www.visualstudio.com/)<br/><br/> .NET iş yükünün etkinleştirilmiş.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) ya da Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcıları kullanacak şekilde ayarlayın.



## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Sağlama AKS ve ACR**: Contoso yönetilen AKS kümesi ve PowerShell kullanarak Azure kapsayıcı kayıt defteri sağlar
> * **2. adım: Docker kapsayıcıları oluşturma**: VSTS kullanarak Docker kapsayıcılar için CI ' ayarlama ve bunları ACR'ye gönderin.
> * **3. adım: arka uç mikro Hizmetleri dağıtma**: arka uç mikro hizmetler tarafından kullanılan altyapı geri kalanını dağıttıkları.
> * **4. adım: ön uç altyapıyı**: inlcuding evcil hayvan telefonlar, Cosmos DB ve görüntü işleme API'si için blob depolama, ön uç altyapısı dağıttıkları.
> * **5. adım: arka uç geçirme**: mikro Hizmetleri dağıtın ve arka uç geçirmek için AKS üzerinde çalıştırın.
> * **6. adım: ön uç yayımlama**: Bunlar Azure App service ve evcil hayvan hizmet tarafından çağrılacak işlev uygulaması için SmartHotel uygulamayı yayımlayın.



## <a name="step-1-provision-an-aks-cluster-and-acr"></a>1. adım: bir AKS kümesi ve ACR sağlama

Contoso AKS ve Azure Container Registry'yi kullanarak yönetilen bir Kubernetes kümesi oluşturmak için bir dağıtım betiği çalıştırır.

- Yönergeler için bu bölümdeki **SmartHotel360-Azure-backend** depo.
- **SmartHotel360-Azure-backend** GitHub deposunu tüm bu dağıtımın parçası yazılımı içerir.

Bunlar aşağıdaki gibi sağlayın:

1. Başlamadan önce tüm önkoşul yazılımlarının yüklü olduğunu kullanmakta olduğunuz geliştirme makinenizde Contoso sağlar. 
2. Bunlar yerel olarak geliştirme makinesini kullanarak Git deposunu kopyalayın.

    **Git kopya https://github.com/Microsoft/SmartHotel360-Azure-backend.git**

3.  Contoso, Visual Studio Code kullanarak klasör açılır ve taşır **/dağıtım/k8s** betiğin bulunduğu dizine **gen aks env.ps1**.
4. Bunlar, AKS ile kapsayıcı kayıt defteri yönetilen Kubernetes kümesi oluşturmak için betiği çalıştırın.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
5.  Dosya açık $location parametresi güncelleştirilmesi **eastus2**ve dosyayı kaydedin.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

6. Simgeye **görünümü** > **tümleşik Terminal** kodda tümleşik Terminalini açmak için.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

7. PowerShell Tümleşik terminalde, bunlar Connect-AzureRmAccount'komutunu kullanarak Azure'da oturum açın. [Daha fazla bilgi edinin](https://docs.microsoft.com/powershell/azure/get-started-azureps) PowerShell ile çalışmaya başlama hakkında.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

8. Azure CLI çalıştırarak kimlik doğrulaması **az login** komutunu ve aşağıdaki web tarayıcıları kullanarak kimlik doğrulaması yapmak için yönergeleri. [Daha fazla bilgi edinin](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) hakkında günlük kaydı Azure CLI ile oturum açın.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

9. Bunlar, kaynak grubu adı ContosoRG, AKS kümesi smarthotel-aks-eus2 adını ve yeni kayıt defteri adını geçirerek aşağıdaki komutu çalıştırın.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```

    ![AKS](./media/contoso-migration-rebuild/aks6.png)

10. Azure kaynakları için AKS kümesi içeren başka bir kaynak grubu oluşturur.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

11. Dağıtım tamamlandıktan sonra Contoso yükleyin **kubectl** komut satırı aracı. Azure CloudShell üzerinde aracı zaten yüklü.

    **az aks install-cli**

12. Bunlar çalıştırarak küme bağlantıyı doğrulama **kubectl alma düğümleri** komutu. VM otomatik olarak oluşturulan kaynak grubunda aynı ada düğümüdür.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

13. Bunlar, Kubernetes panosunu başlatmak için aşağıdaki komutu çalıştırın: 

    **az aks Gözat--resource-group ContosoRG--ad smarthotelakseus2**

14. Panoya bir tarayıcı sekmesi açılır. Azure CLI kullanarak bir tünel bağlantısı budur. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-build-a-docker-container"></a>2. adım: bir Docker kapsayıcısı oluşturma

### <a name="create-a-vsts-and-build"></a>Bir VSTS oluşturun ve yapılandırın

Contoso bir VSTS projesi oluşturur ve kapsayıcı oluşturmak için bir CI yapı yapılandırır ve ardından ACR'ye iter. Bu yönergeleri bölümdeki [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) depo.

1. Bunlar, VisualStudio.com yeni bir hesap oluşturun (**contosodevops360.visualstudio.com**) ve Git kullanacak şekilde yapılandırın.

2. Bunlar yeni bir proje oluşturun (**smarthotelrefactor**) Git sürüm denetimi ve Çevik iş akışını kullanarak.

    ![VSTS](./media/contoso-migration-rebuild/vsts1.png) 


3. Bunlar, GitHub deposunu içeri aktarın.

    ![VSTS](./media/contoso-migration-rebuild/vsts2.png)
    
4. İçinde **derleme ve yayın**, bir kaynaktan alınan olarak VSTS Gıt kullanarak yeni bir tanımı oluşturdukları **smarthotel** depo. 

    ![VSTS](./media/contoso-migration-rebuild/vsts3.png)

6. Boş bir işlem hattı ile başlatmak için seçin.

    ![VSTS](./media/contoso-migration-rebuild/vsts4.png)  

7. Seçmeleri **barındırılan Linux Önizleme** derleme tanımı için.

    ![VSTS](./media/contoso-migration-rebuild/vsts5.png) 
 
8. İçinde **1. Aşama**, ekledikleri bir **Docker Compose** görev. Bu görev yapılar Docker compose.

    ![VSTS](./media/contoso-migration-rebuild/vsts6.png) 

9. Tekrarlayın ve başka bir **Docker Compose** görev. Bu, ACR kapsayıcıları iter.

     ![VSTS](./media/contoso-migration-rebuild/vsts7.png) 

8. Bunlar ilk görevi (oluşturmak için) seçin ve yapı Azure aboneliği, yetkilendirme ve ACR ile yapılandırın. 

    ![VSTS](./media/contoso-migration-rebuild/vsts8.png)

9. Bunlar yolunu belirtin **docket compose.yaml** dosyasındaki **src** deposunun klasör. Hizmet görüntülerinizi oluşturmak ve son etiket eklemek için seçin. Eylemi değiştiğinde **derleme hizmeti görüntüleri**, VSTS görev adı değişiklikleri **hizmetleri otomatik olarak oluşturun**

    ![VSTS](./media/contoso-migration-rebuild/vsts9.png)

10. Şimdi, Contoso (göndermek için) ikinci Docker görev yapılandırır. Bunlar aboneliği seçin ve **smarthotelacreus2** ACR. 

    ![VSTS](./media/contoso-migration-rebuild/vsts10.png)

11. Yeniden, bunların dosya docker compose.yaml dosyasına girin ve seçin **hizmet görüntüleri itme** ve son etiket içerir. Eylem değiştiğinde **hizmet görüntüleri itme**, VSTS görev adını değişiklikleri **anında iletme hizmetlerini otomatik olarak**

    ![VSTS](./media/contoso-migration-rebuild/vsts11.png)

12. Yapılandırılmış VSTS görevleri ile Contoso derleme tanımı kaydeder ve yapı işlemini başlatır.

    ![VSTS](./media/contoso-migration-rebuild/vsts12.png)

13. Bunlar üzerinde derleme işinin ilerleme durumunu denetlemek için tıklayın.

    ![VSTS](./media/contoso-migration-rebuild/vsts13.png)

14. Derleme tamamlandıktan sonra ACR mikro hizmetler tarafından kullanılan kapsayıcıları ile doldurulmuş yeni depoları gösterir.

    ![VSTS](./media/contoso-migration-rebuild/vsts14.png)


## <a name="step-3-deploy-back-end-microservices"></a>3. adım: arka uç mikro hizmet dağıtma

Oluşturulan bir AKS kümesi ve Docker görüntülerini derleme, Contoso artık arka uç mikro hizmetler tarafından kullanılan altyapı geri kalanını dağıtır.

- Bölüm kullanma yönergeleri [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) depo.
- İçinde **/deploy/k8s/arm** klasörü, tüm öğeleri oluşturmak için tek bir komut yoktur. 

Bunlar gibi dağıtın:

1. Contoso, aşağıdaki komutu yazarak ContosoRG kaynak grubu ve EUS2 bölgede, Azure kaynakları dağıtmak için deploy.cmd dosyasını kullanır:

    **. \deploy azuredeploy - c eastus2 ContosoRG**

    ![Arka ucu dağıtın](./media/contoso-migration-rebuild/backend1.png)

2. Bunlar daha sonra kullanılmak üzere her veritabanı için bağlantı dizesini yakalayın.

    ![Arka ucu dağıtın](./media/contoso-migration-rebuild/backend2.png)

## <a name="step-4-deploy-front-end-infrastructure"></a>4. adım: ön uç altyapısı dağıtma

Contoso, ön uç uygulamaları tarafından kullanılan altyapı dağıtmak gerekmez. Bunlar, evcil hayvan görüntülerini depolamak için blob depolama kapsayıcısı oluşturur; evcil hayvan bilgilerle belgeleri depolamak için Cosmos veritabanı, ve Web sitesi için görüntü işleme API'si. 

Yönergeler için bu bölümdeki [SmartHotel genel web](https://github.com/Microsoft/SmartHotel360-public-web) depo.

### <a name="create-storage-containers"></a>Depolama kapsayıcıları oluşturma

1.  Azure portalında depolama hesabı oluşturuldu ve tıklattığında Contoso açılır **Blobları**.
2.  Bunlar yeni bir kapsayıcı oluşturur (**Evcil Hayvanlar**) kapsayıcıya genel erişim düzeyi ile ayarlayın. Kullanıcılar, bu kapsayıcı için kendi evcil hayvan fotoğrafları yükleyeceksiniz.

    ![Depolama blobu](./media/contoso-migration-rebuild/blob1.png)

3. Adlı ikinci yeni bir kapsayıcı oluşturdukları **ayarları**. Tüm ön uç uygulama ayarlarını içeren bir dosya bu kapsayıcıda yer alır.

    ![Depolama blobu](./media/contoso-migration-rebuild/blob2.png)

4. Bunlar, gelecekte başvurmak için bir metin dosyasında depolama hesabı için erişim ayrıntılarını yakalayın.

    ![Depolama blobu](./media/contoso-migration-rebuild/blob2.png)

## <a name="provision-a-cosmos-database"></a>Bir Cosmos veritabanı sağlama

Contoso evcil hayvan bilgileri için kullanılacak bir Cosmos veritabanı sağlar.

1. Oluşturdukları bir **Azure Cosmos DB** Azure Marketi'nde.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Bunlar bir ad belirtin (**contosomarthotel**), SQL API'yi seçin ve üretim kaynak grubunun ana Doğu ABD 2 bölgesinde ContosoRG yerleştirin.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Bunlar yeni bir koleksiyon veritabanı için varsayılan kapasitesi ve aktarım hızı ile ekleyin.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Bunlar, gelecekte başvurmak için bir veritabanı için bağlantı bilgilerini unutmayın. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


## <a name="provision-computer-vision"></a>Sağlama görüntü işleme

Contoso görüntü işleme API'sini sağlar. API, kullanıcılar tarafından karşıya yüklenen resim değerlendirmek için işlev tarafından çağrılır.

1. Oluşturdukları bir **görüntü işleme** Azure Marketi'nde örnek.

     ![Görüntü İşleme](./media/contoso-migration-rebuild/vision1.png)

2. Bunlar API sağlama (**smarthotelpets**) ana Doğu ABD 2 bölgesinde ContosoRG üretim kaynak grubu.

    ![Görüntü İşleme](./media/contoso-migration-rebuild/vision2.png)

3. Bunlar, API için bağlantı ayarlarını daha sonra başvurmak için bir metin dosyasına kaydedin.

     ![Görüntü İşleme](./media/contoso-migration-rebuild/vision3.png)

## <a name="step-5-deploy-the-back-end-services-in-azure"></a>5. adım: arka uç hizmetlerini azure'da dağıtma

Şimdi, Contoso gerekir hizmetlere gelen trafiğe izin vermek ve mikro hizmetler için AKS kümesi dağıtırsınız NGINX giriş denetleyicisine dağıtılacak.

Bu yönergeleri bölümdeki [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) depo.



1. Visual Studio Code güncelleştirmek için kullandıkları **/deploy/k8s/config_loal.yml** dosya. Bunlar, çeşitli veritabanı bağlantıları bunlar daha önce not ettiğiniz bilgileri güncelleştirin.

     ![Mikro hizmetlerin dağıtımı](./media/contoso-migration-rebuild/deploy-micro.png)

    > [!NOTE]
    > Bazı yapılandırma ayarları (örneğin Active Directory B2C), bu makalede ele değildir. Bu depo ayarları hakkında daha fazla bilgi yok.

2. Deploy.ps1 dosyasını (giriş ve giriş denetleyicisine) hariç tüm küme içeriğini siler ve mikro hizmetler dağıtır. Bunlar, aşağıdaki gibi çalıştırın:

  ```
  .\deploy.ps1 -configFile .\conf_local.yml -dockerUser smarthotelacreus2  -dockerPassword [passwordhere] -registry smarthotelacreus2.azurecr.io -imageTag latest -deployInfrastructure $false -buildImages $false -pushImages $false
  ```

3. Bunlar, hizmetlerin durumunu denetlemek için aşağıdaki komutu çalıştırın:

    ```
    kubectl get services
    ```
4. Bunlar dağıtım gözden geçirmek için Kubernetes panosunu açın:

    ```
    az aks browse --resource-group ContosoRG --name smarthotelakseus2
    ```


## <a name="step-6-publish-the-frontend"></a>6. adım: ön uç yayımlama

Son adım olarak, Contoso SmartHotel uygulamasını Azure App Service ve evcil hayvan hizmeti tarafından çağrılan işlev uygulaması için yayımlar.

Bu yönergeleri bölümdeki [SmartHotel genel web depo.](https://github.com/Microsoft/SmartHotel360-public-web) Depo.

### <a name="set-up-web-app-to-use-contoso-resources"></a>Contoso kaynakları kullanmak için Web uygulamasını ayarlama

1. Contoso makineye Git kullanarak yerel geliştirme deposu klonlar:

    **Git kopya https://github.com/Microsoft/SmartHotel360-public-web.git**

2. Visual Studio'da, depodaki tüm dosyaları göstermek için klasörü açın.

    ![Ön uç yayımlama](./media/contoso-migration-rebuild/front-publish1.png)

3. Bunlar, varsayılan ayar olarak gerekli yapılandırma değişikliklerini yapın.

    - Web uygulaması başlatıldığında arar **SettingsUrl** uygulama ayarı.
    - Bu değişken, bir yapılandırma dosyası için bir URL içermelidir.
    - Varsayılan olarak kullanılan genel bir uç nokta ayardır.

4. Bunlar güncelleştirme **/config-sample.json/sample.json** dosya. Web yapılandırma dosyası genel bir uç nokta kullanılırken budur.

    - Her ikisi de Düzenle **URL'leri** ve **pets_config** bölümlerle AKS API uç noktaları, depolama hesapları ve Cosmos veritabanı için değerleri. 
    - URL'lere Contoso oluşturduğunuz yeni web uygulamasına DNS adı eşleşmelidir.
    - Contoso için bu, **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![Ön uç yayımlama](./media/contoso-migration-rebuild/front-publish2.png)

5. Dosya güncelleştirildikten sonra bunlar yeniden adlandırın **smarthotelsettingsurl**ve bunlar oluşturduğunuz depolama blobu karşıya yükleyin.

     ![Ön uç yayımlama](./media/contoso-migration-rebuild/front-publish3.png)

6. Bunlar, dosyanın URL'si almak için tıklayın. Yapılandırma dosyasını çekmek başladığında bu URL'yi ve uygulama tarafından kullanılır.

    ![Ön uç yayımlama](./media/contoso-migration-rebuild/front-publish4.png)

7. Bunlar güncelleştirme **SettingsUrl** içinde **appsettings. Production.JSON** dosyalarını, yeni dosyanın URL'si.

    ![Ön uç yayımlama](./media/contoso-migration-rebuild/front-publish5.png)


### <a name="deploy-the-website-to-the-azure-app-service"></a>Web sitesini Azure App Service'e dağıtma

Artık Contoso Web sitesi yayımlayabilirsiniz.


1. Bunlar, Visual Studio 2017'de Application Insights izlemeyi etkinleştirin. Bunu yapmak için seçtikleri **PublicWeb** için arama ve Çözüm Gezgini'nde proje **Application Insights Ekle**. Bunlar, uygulama Altyapısı'na dağıttığınızda oluşturulan Application Insight örneği ile kaydedin.

    ![Web sitesi yayımlama](./media/contoso-migration-rebuild/deploy-website1.png)

2. Visual Studio'da bunlar PublicWeb Proje uygulama anlayışları'na bağlanmak ve yapılandırıldığı gösterecek şekilde güncelleştirin.
 
    ![Web sitesi yayımlama](./media/contoso-migration-rebuild/deploy-website2.png)

3. Visual Studio'da oluşturma ve kendi web uygulama yayımlama.

    ![Web sitesi yayımlama](./media/contoso-migration-rebuild/deploy-website3.png)

5. Uygulama adı belirtin ve üretim kaynak grubuna yerleştirin **ContosoRG**, ana Doğu ABD 2 bölgesinde.

    ![Web sitesi yayımlama](./media/contoso-migration-rebuild/deploy-website4.png)

### <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

1. Bunlar, oluşturma ve işlev yayımlamak için Visual Studio'yu kullanın. Bunu yapmak için sağ **PetCheckerFunction** > **Yayımla**. Daha sonra yeni bir App Service işlev oluşturun.

     ![İşlev dağıtma](./media/contoso-migration-rebuild/function1.png)

2. Adı belirlediği **smarthotelpetchecker**, ContosoRG kaynak grubunu ve yeni bir app service planı içinde yerleştirin.

     ![İşlev dağıtma](./media/contoso-migration-rebuild/function2.png)

3. Depolama hesabı seçip bir işlev oluşturun.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function3.png)

4. Dağıtımı, Azure işlev uygulaması sağlama ile başlar. İçinde **Yayımla**, Contoso, depolama, Cosmos veritabanı ve görüntü işleme API'si için uygulama ayarları ekler.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function4.png)

5. İlk işlev yerel olarak çalıştırmak için bunlar ayarlarında güncelleştirme **PetCheckerFunction/local.settings.json**.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function5.png)

6. İşlev dağıtıldıktan sonra Azure portalında ile göründüğü **çalıştıran** durumu.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function6.png)


7. Evcil hayvan denetleyicisi yapay ZEKA, beklenen şekilde çalışıp çalışmadığını test etmek için uygulamaya göz atın [ http://smarthotel360public.azurewebsites.net/Pets ](http://smarthotel360public.azurewebsites.net/Pets).
8. Bunlar, bir resim karşıya yüklenecek avatar tıklayın.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function7.png)

9. Küçük bir köpek kontrol etmek için istedikleri ilk fotoğraf var.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function8.png)

10. Uygulama kabul bir ileti döndürür.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function9.png)




## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

- Contoso, kendi yeni veritabanlarını güvenli olmasını sağlamak gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Uygulama SSL sertifikalarıyla kullanacak şekilde güncelleştirilmesi gerekir. 443 numaralı yanıtlamak için kapsayıcı örneğini yeniden dağıtılması.
- Bunlar, Service Fabric uygulamaları için gizli dizilerini korumak için KeyVault kullanarak düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Bunlar veritabanı için bölgesel yük devretme sağlamak için SQL yük devretme grupları düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Bunlar, ACR premium SKU için coğrafi çoğaltmayı yararlanabilirsiniz. [Daha fazla bilgi](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication)

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını kendi [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso Azure SmartHotel uygulamada yeniden oluşturun. Bunlar, şirket içi uygulama yeniden Azure App Services Web uygulamaları için ön uç VM'si. Bunlar, Azure Kubernetes Service (AKS) tarafından yönetilen kapsayıcıları dağıtılmış mikro hizmetler kullanarak uygulama arka ucu üzerine kurulmuştur. Bunlar işlevini evcil hayvan fotoğraf uygulaması ile geliştirilmiştir.




