---
title: Contoso şirket içi uygulamasını azure'a yeniden | Microsoft Docs
description: Contoso bir uygulamayı Azure App Services, Kubernetes hizmeti, CosmosDB, Azure işlevleri ve Bilişsel hizmetler kullanarak azure'a nasıl oluşturur? öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 74c33d73f15c4edf63a02ea5c9a0cdcad88bb68c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59049754"
---
# <a name="contoso-migration-rebuild-an-on-premises-app-to-azure"></a>Contoso geçişi: Azure'da şirket içi uygulama yeniden oluşturun

Bu makalede, Contoso nasıl geçirir ve azure'da SmartHotel360 uygulaması oluşturur gösterilmektedir. Contoso, Azure App Services Web uygulamaları için uygulamanın ön uç sanal makine geçirir. Azure Kubernetes Service (AKS) tarafından yönetilen kapsayıcıları dağıtılmış mikro hizmetler kullanarak uygulama arka ucu oluşturulur. Site evcil hayvan fotoğraf işlevselliği sağlamak için Azure işlevleri ile etkileşime girer. 

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklara Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel Bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynakları değerlendirme](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel360 uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş Azure'a SmartHotel360 uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: Bir uygulamayı Azure VM’lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso geçirme SmartHotel360 uygulama yalnızca Site RECOVERY'yi kullanarak VM'lerin nasıl gösterir. | Kullanılabilir
[Makale 6: Azure Vm'leri ve SQL Server Always On kullanılabilirlik grubu için uygulamayı yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: Azure Web Apps ve Azure SQL veritabanında bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Bir Linux uygulamasına Azure Web Apps ve Azure MySQL yeniden düzenleyin](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Nasıl geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımı geçirdiğini gösterir, azure'da Azure DevOps hizmetlerine. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
Makale 13: Bir uygulamayı Azure'da yeniden oluşturun | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Bu makalede
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir

Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel360 uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360-Backend).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyüyor ve Contoso Web siteleri kullanan müşteriler için fark yaratan deneyimler sağlamak istiyor.
- **Çeviklik**: Contoso genel ekonomik, başarı sağlamak Pazar, değişiklikleri daha hızlı tepki vermek mümkün olması gerekir. 
- **Ölçek**: İş başarıyla büyüdükçe, Contoso BT Ekibi aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisans maliyetleri en aza indirmek istiyor.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut ekibi, uygulama gereksinimleri bu geçiş için aşağı sabitlenmiş. Bu gereksinimler, en iyi geçiş yöntemini belirlemek için kullanılıyordu:
 - Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır. İyi performans göstermelerini ve ölçeği kolayca gerekir.
 - Uygulama Iaas bileşenleri kullanmamanız gerekir. Her şeyi PaaS veya sunucusuz Hizmetleri oluşturulmalıdır.
 - Uygulama yapıları bulut hizmetlerindeki çalışması gerektiğini ve bulutta özel Kurumsal Çapta kapsayıcı kayıt defterindeki kapsayıcı bulunmalıdır.
 - Uygulama tarafından alınan kararları, Oteller kabul gerekir bu yana evcil hayvan fotoğraflar için kullanılan API hizmeti doğru ve güvenilir, gerçek dünyadaki olmalıdır. Herhangi bir evcil hayvan Oteller kalmak için erişime izin verildi.
 - DevOps işlem hattı gereksinimlerini karşılamak üzere Contoso için kaynak kodu Yönetimi (SCM), Git depoları ile Azure DevOps kullanır.  Otomatik derleme ve yayınlar kod oluşturmak için kullanılan ve Azure Web Apps, Azure işlevleri ve AKS dağıtma.
 - Farklı bir CI/CD işlem hatları, ön uç web sitesinde yanı sıra, arka uçtaki mikro hizmetler için gereklidir.
 - Arka uç Hizmetleri ön uç web uygulamasını döngüsü farklı bir sürüm var.  Bu gerekliliği karşılamak için bunlar iki farklı DevOps işlem hatları dağıtır.
 - Contoso, tüm ön uç Web sitesi dağıtımı için yönetim onay gerekir ve CI/CD işlem hattı bu sağlamanız gerekir.


## <a name="solution-design"></a>Çözüm tasarımı

Hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve geçiş için kullanılacak Azure Hizmetleri dahil olmak üzere geçiş işlemi tanımlar.

### <a name="current-app"></a>Geçerli uygulama

- Smarthotel360'ı şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-architecture"></a>Önerilen mimarisi

- Ön uç uygulaması, birincil bir Azure bölgesinde bir Azure App Services Web uygulaması olarak dağıtılır.
- Evcil hayvan fotoğrafları karşıya yükleme bir Azure işlevi sağlar ve site bu işlevselliği ile etkileşime geçer.
- Evcil hayvan fotoğraf işlevi, Bilişsel hizmetler görüntü işleme API'si ve CosmosDB yararlanır.
- Sitenin arka uç, mikro hizmetler kullanılarak oluşturulmuştur. Bu yönetilen Azure Kubernetes Service'i (AKS) kapsayıcıları dağıtılır.
- Kapsayıcıları Azure DevOps kullanılarak oluşturulan ve Azure Container Registry (ACR) gönderdiniz.
- Şimdilik, Contoso Web app ve işlev kodunuzu Visual Studio kullanarak el ile dağıtım
- Mikro hizmetler, Kubernetes komut satırı araçları çağıran bir PowerShell Betiği kullanılarak dağıtılır.

    ![Senaryo mimarisi](./media/contoso-migration-rebuild/architecture.png) 

  
### <a name="solution-review"></a>Çözümü gözden geçirme

Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarım değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | PaaS ve sunucusuz çözümler için uçtan uca dağıtım önemli ölçüde kullanarak Contoso sağlamalısınız Yönetimi zamanı azaltır.<br/><br/> Mikro hizmet mimarisi için taşıma çözümü zaman içinde kolayca uzatmanın Contoso sağlar.<br/><br/> Yeni işlevsellik mevcut çözümleri kod tabanlarında bozmadan çevrimiçi getirilebilir.<br/><br/> Hiçbir tek hata noktası ile birden çok örneği ile Web uygulaması yapılandırılacaktır.<br/><br/> Otomatik ölçeklendirme, uygulama trafiği hacimlere işleyebilmeniz etkinleştirilecektir.<br/><br/> Contoso taşıma PaaS Hizmetleri ile Windows Server 2008 R2 işletim sisteminde çalışan güncel çözümlerini devre dışı bırakabilirsiniz.<br/><br/> CosmosDB, Contoso tarafından hiçbir yapılandırma gerektiren, yerleşik hata toleransı vardır. Bu, veri katmanı artık tek bir yük devretme noktası olduğu anlamına gelir.
**Simgeler** | Diğer geçiş seçenekleri daha karmaşık kapsayıcılardır. Öğrenme eğrisini, Contoso için bir sorun olabilir.  Bunlar, yeni bir eğri artma değerli birçok sağlayan bir karmaşıklık düzeyi sunar.<br/><br/> Contoso'da operasyon ekibinin anlamak ve kapsayıcılar ve mikro hizmet uygulaması için Azure desteği için artırma gerekir.<br/><br/> Contoso DevOps için tüm çözüm tam olarak uygulanan edilmemiş. Contoso AKS, İşlevler ve uygulama hizmetleri Hizmetleri dağıtımı için hakkında düşünmeniz gerekir.



### <a name="migration-process"></a>Geçiş işlemi

1. Contoso, ACR, AKS ve CosmosDB sağlayın.
2. Bunlar, Azure Web uygulaması, depolama hesabı, işlevi ve API dahil olmak üzere dağıtımı için altyapı sağlayın. 
3. Altyapısı yerleştirildikten sonra mikro hizmetler kullanarak bunları ACR'ye gönderim Azure DevOps, kapsayıcı görüntüleri oluşturacaksınız.
4. Contoso Bu mikro hizmetler için bir PowerShell betiğini kullanarak ASK dağıtır.
5. Son olarak, bunlar Azure işlevi ve Web uygulaması dağıtacaksınız.

    ![Geçiş işlemi](./media/contoso-migration-rebuild/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[AKS](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Kubernetes yönetim, dağıtımı ve işlemleri basitleştirir. Tam olarak yönetilen bir Kubernetes kapsayıcı düzenleme hizmeti sağlar.  | AKS ücretsiz bir hizmettir.  Yalnızca sanal makineler ve ilişkili depolama ve kullanılan ağ kaynakları için ödeme yaparsınız. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/kubernetes-service/).
[Azure İşlevleri](https://azure.microsoft.com/services/functions/) | Bir olay odaklı, sunucusuz bilgi işlem deneyimiyle geliştirme hızlandırır. İsteğe bağlı olarak ölçeklendirin.  | Yalnızca tüketilen kaynaklar için ödeme yaparsınız. Planı, saniye başına kaynak tüketimi ve yürütme göre faturalandırılır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/functions/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Tüm kapsayıcı dağıtımı türlerinin görüntülerini depolar. | Özellikler, depolama ve kullanım süresi göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure App Service](https://azure.microsoft.com/services/app-service/containers/) | Herhangi bir platformda çalışan kurumsal sınıf web, mobil ve API uygulamalarını hızlı bir şekilde oluşturun, dağıtın ve ölçeklendirin. | App Service planları, saniyelik olarak faturalandırılır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Önkoşullar

Bu senaryoda Contoso gerekenler şu şekildedir:

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso abonelikleri daha önceki bir makalede sırasında oluşturuldu. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.
**Geliştirici önkoşulları** | Contoso, geliştirici iş istasyonunda aşağıdaki araçları gerekir:<br/><br/> - [Visual Studio 2017 Community Edition: Sürüm 15.5](https://www.visualstudio.com/)<br/><br/> .NET iş yükünün etkinleştirilmiş.<br/><br/> [Git](https://git-scm.com/)<br/><br/> [Azure PowerShell](https://azure.microsoft.com/downloads/)<br/><br/> [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)<br/><br/> [Docker CE (Windows 10) ya da Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) Windows kapsayıcıları kullanacak şekilde ayarlayın.



## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: AKS ve ACR sağlama**: Contoso yönetilen AKS kümesi ve PowerShell kullanarak Azure kapsayıcı kayıt defteri sağlar.
> * **2. adım: Docker kapsayıcıları derleme**: Bunlar Azure DevOps kullanarak Docker kapsayıcılar için CI ' ayarlayın ve bunları ACR'ye gönderin.
> * **3. adım: Arka uç mikro hizmetlerin dağıtımı**: Bunlar, arka uç mikro hizmetler tarafından kullanılan altyapı geri kalanını dağıtın.
> * **4. adım: Ön uç altyapıyı**: Bunlar, evcil hayvan telefonlar, Cosmos DB ve görüntü işleme API'si için blob depolama da dahil olmak üzere, ön uç altyapısı dağıtın.
> * **5. adım: Arka uç geçirme**: Bunlar, mikro hizmetlerin dağıtımı ve arka uç geçirmek için AKS üzerinde çalıştırın.
> * **6. adım: Ön uç yayımlama**: Bunlar, Azure App service ve evcil hayvan hizmet tarafından çağrılacak işlev uygulaması için SmartHotel360 uygulamayı yayımlayın.



## <a name="step-1-provision-back-end-resources"></a>1. Adım: Arka uç kaynakları sağlayın

Contoso yöneticileri AKS ve Azure Container Registry (ACR) kullanarak yönetilen bir Kubernetes kümesi oluşturmak için bir dağıtım betiğini çalıştırın.

- Yönergeler için bu bölümdeki **SmartHotel360-Azure-backend** depo.
- **SmartHotel360-Azure-backend** GitHub deposunu tüm bu dağıtımın parçası yazılımı içerir.

### <a name="prerequisites"></a>Önkoşullar

1. Başlamadan önce tüm önkoşul yazılım dağıtımı için kullanmakta olduğunuz geliştirme makinesindeki yüklü Contoso yöneticileri emin olun.
2. Bunlar Git kullanarak geliştirme makinede yerel depoyu kopyalama: **git kopyalama https://github.com/Microsoft/SmartHotel360-Azure-backend.git**


### <a name="provision-aks-and-acr"></a>AKS ve ACR sağlayın

Contoso yöneticileri gibi sağlayın:

1.  Visual Studio Code kullanarak klasörü açın ve taşır **/dağıtım/k8s** betiğin bulunduğu dizine **gen aks env.ps1**.
2. Bunlar, AKS ACR ile yönetilen Kubernetes kümesi oluşturmak için betiği çalıştırın.

    ![AKS](./media/contoso-migration-rebuild/aks1.png)
 
3.  Dosya açık $location parametresi güncelleştirilmesi **eastus2**ve dosyayı kaydedin.

    ![AKS](./media/contoso-migration-rebuild/aks2.png)

4. Simgeye **görünümü** > **tümleşik Terminal** kodda tümleşik Terminalini açmak için.

    ![AKS](./media/contoso-migration-rebuild/aks3.png)

5. PowerShell Tümleşik terminalde, bunlar Connect AzAccount komutunu kullanarak Azure'da oturum açın. [Daha fazla bilgi edinin](https://docs.microsoft.com/powershell/azure/get-started-azureps) PowerShell ile çalışmaya başlama hakkında.

    ![AKS](./media/contoso-migration-rebuild/aks4.png)

6. Azure CLI çalıştırarak kimlik doğrulaması **az login** komutunu ve aşağıdaki web tarayıcıları kullanarak kimlik doğrulaması yapmak için yönergeleri. [Daha fazla bilgi edinin](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) hakkında günlük kaydı Azure CLI ile oturum açın.

    ![AKS](./media/contoso-migration-rebuild/aks5.png)

7. Bunlar, kaynak grubu adı ContosoRG, AKS kümesi smarthotel-aks-eus2 adını ve yeni kayıt defteri adını geçirerek aşağıdaki komutu çalıştırın.

    ```
    .\gen-aks-env.ps1  -resourceGroupName ContosoRg -orchestratorName smarthotelakseus2 -registryName smarthotelacreus2
    ```
    ![AKS](./media/contoso-migration-rebuild/aks6.png)

8. Azure kaynakları için AKS kümesi içeren başka bir kaynak grubu oluşturur.

    ![AKS](./media/contoso-migration-rebuild/aks7.png)

9. Dağıtım tamamlandıktan sonra yükledikleri **kubectl** komut satırı aracı. Azure CloudShell üzerinde aracı zaten yüklü.

    **az aks yükleme-CLI**

10. Bunlar çalıştırarak küme bağlantıyı doğrulama **kubectl alma düğümleri** komutu. VM otomatik olarak oluşturulan kaynak grubunda aynı ada düğümüdür.

    ![AKS](./media/contoso-migration-rebuild/aks8.png)

11. Bunlar, Kubernetes panosunu başlatmak için aşağıdaki komutu çalıştırın: 

    **az aks Gözat--resource-group ContosoRG--ad smarthotelakseus2**

12. Panoya bir tarayıcı sekmesi açılır. Azure CLI kullanarak bir tünel bağlantısı budur. 

    ![AKS](./media/contoso-migration-rebuild/aks9.png)




## <a name="step-2-configure-the-back-end-pipeline"></a>2. Adım: Arka uç ardışık düzenini yapılandırın

### <a name="create-an-azure-devops-project-and-build"></a>Azure DevOps projesi oluşturma ve derleme

Contoso bir Azure DevOps projesi oluşturur ve kapsayıcı oluşturmak için bir CI yapı yapılandırır ve ardından ACR'ye iter. Bu yönergeleri bölümdeki [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) repository.r

1. Yeni bir kuruluş, VisualStudio.com oluşturdukları (**contosodevops360.visualstudio.com**) ve Git kullanacak şekilde yapılandırın.

2. Bunlar yeni bir proje oluşturun (**SmartHotelBackend**) Git sürüm denetimi ve Çevik iş akışını kullanarak.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts1.png) 


3. Bunlar içeri aktarma [GitHub deposunu](https://github.com/Microsoft/SmartHotel360-Backend).

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts2.png)
    
4. İçinde **işlem hatları**, simgeye **derleme**ve Azure depoları Git deposundan bir kaynak olarak kullanarak bir işlem hattı oluşturacaksınız. 

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts3.png)

6. Boş bir işlemle başlatmak için seçin.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts4.png)  

7. Seçmeleri **barındırılan Linux Önizleme** derleme işlem hattı için.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts5.png) 
 
8. İçinde **1. Aşama**, ekledikleri bir **Docker Compose** görev. Bu görev yapılar Docker compose.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts6.png) 

9. Tekrarlayın ve başka bir **Docker Compose** görev. Bu, ACR kapsayıcıları iter.

     ![Azure DevOps](./media/contoso-migration-rebuild/vsts7.png) 

8. Bunlar ilk görevi (oluşturmak için) seçin ve yapı Azure aboneliği, yetkilendirme ve ACR ile yapılandırın. 

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts8.png)

9. Bunlar yolunu belirtin **docker compose.yaml** dosyasındaki **src** deposunun klasör. Hizmet görüntülerinizi oluşturmak ve son etiket eklemek için seçin. Eylem değiştiğinde **derleme hizmeti görüntüleri**, Azure DevOps görev adını değişiklikleri **derleme hizmetleri otomatik olarak**

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts9.png)

10. Şimdi, (göndermek için) ikinci Docker görev yapılandırın. Bunlar aboneliği seçin ve **smarthotelacreus2** ACR. 

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts10.png)

11. Yeniden, bunların dosya docker compose.yaml dosyasına girin ve seçin **hizmet görüntüleri itme** ve son etiket içerir. Eylem değiştiğinde **hizmet görüntüleri itme**, Azure DevOps görev adını değişiklikleri **anında iletme hizmetlerini otomatik olarak**

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts11.png)

12. Azure DevOps görevlerle yapılandırılmış Contoso derleme işlem hattı kaydeder ve yapı işlemini başlatır.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts12.png)

13. Bunlar üzerinde derleme işinin ilerleme durumunu denetlemek için tıklayın.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts13.png)

14. Derleme tamamlandıktan sonra ACR mikro hizmetler tarafından kullanılan kapsayıcıları ile doldurulmuş yeni depoları gösterir.

    ![Azure DevOps](./media/contoso-migration-rebuild/vsts14.png)


### <a name="deploy-the-back-end-infrastructure"></a>Arka uç altyapısı dağıtma

Oluşturulan bir AKS kümesi ve yerleşik Docker görüntüleri, Contoso yöneticileri artık arka uç mikro hizmetler tarafından kullanılan altyapı geri kalanını dağıtın.

- Bölüm kullanma yönergeleri [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) depo.
- İçinde **/deploy/k8s/arm** klasörü, tüm öğeleri oluşturmak için tek bir komut yoktur. 

Bunlar gibi dağıtın:

1. Bunlar, bir geliştirici komut istemi açın ve komut az oturum açma için Azure aboneliği kullanın.
2. Bunlar aşağıdaki komutu yazarak ContosoRG kaynak grubu ve EUS2 bölgede, Azure kaynakları dağıtmak için deploy.cmd dosyasını kullanın:

    **.\deploy.cmd azuredeploy ContosoRG -c eastus2**

    ![Arka ucu dağıtın](./media/contoso-migration-rebuild/backend1.png)

2. Azure portalında, bunlar daha sonra kullanılmak üzere her veritabanı için bağlantı dizesini yakalayın.

    ![Arka ucu dağıtın](./media/contoso-migration-rebuild/backend2.png)

### <a name="create-the-back-end-release-pipeline"></a>Arka uç yayın işlem hattı oluşturma

Şimdi, Contoso yöneticileri aşağıdakileri yapın:

- Hizmetlere gelen trafiğe izin veren NGINX giriş denetleyicisine dağıtın.
- Mikro hizmetler için AKS kümesi dağıtın.
- İlk adım olarak, Azure DevOps kullanarak mikro hizmetler için bağlantı dizelerini güncelleştirin. Bunlar daha sonra mikro hizmetlerin dağıtımı için yeni Azure DevOps yayın işlem hattı yapılandırın.
- Bu yönergeleri bölümdeki [SmartHotel360-Azure-Backend](https://github.com/Microsoft/SmartHotel360-Azure-backend) depo.
- Bazı yapılandırma ayarları (örneğin Active Directory B2C) bu makalenin kapsamında olmayan unutmayın. Bu depo ayarları hakkında daha fazla bilgi edinin.

Bunlar, işlem hattı oluşturun:

1. Güncelleştirme, Visual Studio kullanarak **/deploy/k8s/config_local.yml** not almak için daha önce bunlar veritabanı bağlantı bilgilerini içeren dosya.

    ![DB bağlantıları](./media/contoso-migration-rebuild/back-pipe1.png)

2. Azure DevOps açın ve SmartHotel360 proje, **yayınlar**, simgeye **+ yeni işlem hattı**.

    ![Yeni ardışık düzen](./media/contoso-migration-rebuild/back-pipe2.png)

3. Simgeye **boş iş** şablon olmadan işlem hattı başlatmak için.
4. Bunlar, aşama ve işlem hattı adları sağlar.

      ![Aşama adı](./media/contoso-migration-rebuild/back-pipe4.png)

      ![İşlem hattı adı](./media/contoso-migration-rebuild/back-pipe5.png)

5. Bunlar, bir yapıt ekleyin.

     ![Yapıt ekleme](./media/contoso-migration-rebuild/back-pipe6.png)

6. Seçmeleri **Git** kaynağı olarak yazın ve proje, kaynak ve SmartHotel360 uygulaması için ana dalı belirtin.

    ![Yapı ayarları](./media/contoso-migration-rebuild/back-pipe7.png)

7. Bunlar, görev bağlantıya tıklayın.

    ![Görev bağlantısı](./media/contoso-migration-rebuild/back-pipe8.png)

8. Bir PowerShell Betiği bir Azure ortamında çalıştırabilirsiniz, yeni bir Azure PowerShell görev ekleyin.

    ![Azure PowerShell](./media/contoso-migration-rebuild/back-pipe9.png)

9. Görev için Azure aboneliğini seçin ve seçin **deploy.ps1** Git deposundan kod.

    ![Betiği çalıştırın](./media/contoso-migration-rebuild/back-pipe10.png)


10. Bunlar bağımsız değişkenleri komut dosyasına ekleyin. betik, tüm küme içeriği siler (dışında **giriş** ve **giriş denetleyicisine**) ve mikro Hizmetleri dağıtın.

    ![Betik bağımsız değişkenleri](./media/contoso-migration-rebuild/back-pipe11.png)

11. Bunlar tercih edilen Azure PowerShell sürümünü en son sürüme ayarlayın ve işlem hattı kaydedin.
12. Bunlar geri gitme **yayın** sayfasında ve el ile yeni bir yayın oluşturun.

    ![Yeni yayın](./media/contoso-migration-rebuild/back-pipe12.png)

13. Yayın oluşturduktan sonra ve de tıklayın **eylemleri**, simgeye **Dağıt**.

      ![Yayını Dağıt](./media/contoso-migration-rebuild/back-pipe13.png)  

14. Dağıtım tamamlandığında, hizmetler, Azure Cloud Shell kullanma durumunu denetlemek için aşağıdaki komutu çalıştırın: **kubectl alma hizmetleri**.


## <a name="step-3-provision-front-end-services"></a>3. Adım: Ön uç hizmetleri sağlama

Contoso yöneticileri, ön uç uygulamaları tarafından kullanılan altyapı dağıtmanız gerekebilir. Bunlar, evcil hayvan görüntülerini depolamak için blob depolama kapsayıcısı oluşturur; evcil hayvan bilgilerle belgeleri depolamak için Cosmos veritabanı; ve Web sitesi için görüntü işleme API'si. 

Yönergeler için bu bölümdeki [SmartHotel360 genel web](https://github.com/Microsoft/SmartHotel360-public-web) depo.

### <a name="create-blob-storage-containers"></a>Blob depolama kapsayıcıları oluşturma

1.  Azure portalında depolama hesabı oluşturuldu ve tıklattığında açtıklarında **Blobları**.
2.  Bunlar yeni bir kapsayıcı oluşturur (**Evcil Hayvanlar**) kapsayıcıya genel erişim düzeyi ile ayarlayın. Kullanıcılar, bu kapsayıcı için kendi evcil hayvan fotoğrafları yükleyeceksiniz.

    ![Depolama blob'u](./media/contoso-migration-rebuild/blob1.png)

3. Adlı ikinci yeni bir kapsayıcı oluşturdukları **ayarları**. Tüm ön uç uygulama ayarlarını içeren bir dosya bu kapsayıcıda yer alır.

    ![Depolama blob'u](./media/contoso-migration-rebuild/blob2.png)

4. Bunlar, gelecekte başvurmak için bir metin dosyasında depolama hesabı için erişim ayrıntılarını yakalayın.

    ![Depolama blob'u](./media/contoso-migration-rebuild/blob2.png)

### <a name="provision-a-cosmos-database"></a>Bir Cosmos veritabanı sağlama

Contoso yöneticileri evcil hayvan bilgileri için kullanılacak bir Cosmos veritabanı sağlayın.

1. Oluşturdukları bir **Azure Cosmos DB** Azure Marketi'nde.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos1.png)

2. Bunlar bir ad belirtin (**contosomarthotel**), SQL API'yi seçin ve üretim kaynak grubunun ana Doğu ABD 2 bölgesinde ContosoRG yerleştirin.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos2.png)

3. Bunlar yeni bir koleksiyon veritabanı için varsayılan kapasitesi ve aktarım hızı ile ekleyin.

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos3.png)


4. Bunlar, gelecekte başvurmak için bir veritabanı için bağlantı bilgilerini unutmayın. 

    ![Cosmos DB](./media/contoso-migration-rebuild/cosmos4.png)


### <a name="provision-computer-vision"></a>Sağlama görüntü işleme

Contoso yöneticileri, görüntü işleme API'si sağlayın. API, kullanıcılar tarafından karşıya yüklenen resim değerlendirmek için işlev tarafından çağrılır.

1. Oluşturdukları bir **görüntü işleme** Azure Marketi'nde örnek.

     ![Görüntü İşleme](./media/contoso-migration-rebuild/vision1.png)

2. Bunlar API sağlama (**smarthotelpets**) ana Doğu ABD 2 bölgesinde ContosoRG üretim kaynak grubu.

    ![Görüntü İşleme](./media/contoso-migration-rebuild/vision2.png)

3. Bunlar, API için bağlantı ayarlarını daha sonra başvurmak için bir metin dosyasına kaydedin.

     ![Görüntü İşleme](./media/contoso-migration-rebuild/vision3.png)


### <a name="provision-the-azure-web-app"></a>Azure Web uygulaması sağlama

Contoso yöneticileri, Azure portalını kullanarak web uygulaması sağlayın.


1. Seçmeleri **Web uygulaması** portalında.

    ![Web uygulaması](media/contoso-migration-rebuild/web-app1.png)

2. Bunlar bir uygulama adı sağlayın (**smarthotelcontoso**), Windows üzerinde çalıştırma ve üretim kaynak grubuna yerleştirmek **ContosoRG**. Bunlar, uygulamanın izlenmesi için yeni bir Application Insights örneği oluştur...

    ![Web uygulaması adı](media/contoso-migration-rebuild/web-app2.png)

3. İşiniz bittiğinde sonra başarıyla oluşturulduktan denetlemek için uygulamanın adresine göz atın.

4. Şimdi, Azure portalında bunlar kodu için bir hazırlama yuvası oluşturur. işlem hattı, bu yuva için içeriden dağıtır. Bu, bir yayın yöneticileri gerçekleştirene kadar kod üretime koymak değil sağlar.

    ![Web uygulaması hazırlama yuvası](media/contoso-migration-rebuild/web-app3.png)



### <a name="provision-the-azure-function-app"></a>Azure işlevi uygulaması sağlama

Azure portalında işlev uygulaması Contoso yöneticileri sağlayın.

1. Seçmeleri **işlev uygulaması**.

    ![İşlev uygulaması oluşturma](./media/contoso-migration-rebuild/function-app1.png)

2. Bunlar bir uygulama adı sağlayın (**smarthotelpetchecker**). Kullanıcılar uygulamayı üretim kaynak grubuna yerleştirin **ContosoRG**. Barındırma yerde ayarlamak **tüketim planı**ve Doğu ABD 2 bölgesinde uygulama yerleştirin. İzleme için Application ınsights'ı örneği ile birlikte yeni bir depolama hesabı oluşturulur.

    ![İşlev uygulaması ayarları](./media/contoso-migration-rebuild/function-app2.png)


3. Uygulama dağıtıldıktan sonra başarıyla oluşturulmuş denetlemek için uygulama adresine göz atın.


## <a name="step-4-set-up-the-front-end-pipeline"></a>4. Adım: Ön uç işlem hattı ayarlayın

Contoso yöneticileri, ön uç sitesinin iki farklı projeler oluşturun. 

1. Azure DevOps, proje oluşturma **SmartHotelFrontend**.

    ![Ön uç projesi](./media/contoso-migration-rebuild/function-app1.png)

2. Bunlar içeri aktarma [SmartHotel360 ön uç](https://github.com/Microsoft/SmartHotel360-public-web.git) Git deposuna yeni bir proje.
3. İşlev uygulaması için bunlar başka bir Azure DevOps projesi (SmartHotelPetChecker) oluşturma ve içeri aktarma [PetChecker](https://github.com/Microsoft/SmartHotel360-PetCheckerFunction ) bu projeye Git deposu.

### <a name="configure-the-web-app"></a>Web uygulamasını yapılandırma

Artık Contoso yöneticileri Contoso kaynakları kullanmak için Web uygulaması yapılandırırsınız.

1. Bunlar Azure DevOps projesi için bağlanın ve yerel olarak geliştirme makinesini deposunu kopyalayın.
2. Visual Studio'da, depodaki tüm dosyaları göstermek için klasörü açın.

    ![Depoya dosyaları](./media/contoso-migration-rebuild/configure-webapp1.png)

3. Bunlar, yapılandırma değişikliklerinin gerektiği gibi güncelleştirin.

    - Web uygulaması başlatıldığında arar **SettingsUrl** uygulama ayarı.
    - Bu değişken, bir yapılandırma dosyasına işaret eden bir URL içermelidir.
    - Varsayılan olarak kullanılan genel bir uç nokta ayardır.

4.  Bunlar /config-sample.json/sample.json dosyasını güncelleştirin.

    - Web yapılandırma dosyası genel bir uç nokta kullanılırken budur.
    - Düzenleyip **URL'leri** ve **pets_config** AKS API uç noktaları, depolama hesapları ve Cosmos veritabanı için değerlerle bölümler.
    - URL'lere Contoso oluşturduğunuz yeni web uygulamasına DNS adı eşleşmelidir.
    - Contoso için bu, **smarthotelcontoso.eastus2.cloudapp.azure.com**.

    ![JSON ayarları](./media/contoso-migration-rebuild/configure-webapp2.png)

5. Dosya güncelleştirildikten sonra yeniden adlandırmak **smarthotelsettingsurl**ve bunlar oluşturduğunuz blob depolama alanına yükleyin.

    ![Yeniden adlandırma ve karşıya yükleme](./media/contoso-migration-rebuild/configure-webapp3.png)

6. Bunlar, dosyanın URL'si almak için tıklayın. URL yapılandırma dosyalarını çeker uygulama tarafından kullanılır.

    ![Uygulama URL'si](./media/contoso-migration-rebuild/configure-webapp4.png)

7. İçinde **appsettings. Production.JSON** dosyasını, bunlar güncelleştirme **SettingsURL** yeni dosyanın URL'si.

    ![URL'yi güncelleştir](./media/contoso-migration-rebuild/configure-webapp5.png)

### <a name="deploy-the-website-to-the-azure-app-service"></a>Web sitesini Azure App Service'e dağıtma

Contoso yöneticileri Web sitesi artık yayımlayabilirsiniz.


1. Azure DevOps açtıklarında ve **SmartHotelFrontend** içinde proje **derleme ve yayınlar**, simgeye **+ yeni işlem hattı**.
2. Seçmeleri **Azure DevOps Git** kaynağı olarak.
3. Seçmeleri **ASP.NET Core** şablonu.
4. İşlem hattı gözden geçirin ve bu maddeyi **Web projeleri yayımlamak** ve **yayınlanmış projelerine Zip** seçilir.

    ![Ardışık düzen ayarları](./media/contoso-migration-rebuild/vsts-publishfront2.png)

5. İçinde **Tetikleyicileri**, sürekli tümleştirme çözümünü ve ana dal ekleyin. Bu çözüm süresini ana dalına işlendi yeni kodu her derleme işlem hattı başladığını sağlar.

    ![Sürekli tümleştirme](./media/contoso-migration-rebuild/vsts-publishfront3.png)

6. Simgeye **Kaydet ve kuyruğa** derlemeyi başlatmak için.
7. Derleme tamamlandıktan sonra bunlar kullanarak sürüm işlem hattınızdan yapılandırma **Azure uygulama hizmeti dağıtımının**.
8. Aşama adı sağladıkları **hazırlama**.

    ![Ortam adı](./media/contoso-migration-rebuild/vsts-publishfront4.png)

9. Bunlar bir yapıt ekleyin ve yalnızca yapılandırılmış derlemeyi seçin.

     ![Yapıt ekleme](./media/contoso-migration-rebuild/vsts-publishfront5.png)

10. Bunlar, yapıt ışık Şimşek simgesine tıklayın ve sürekli dağıtımı etkinleştirme.

    ![Sürekli dağıtım](./media/contoso-migration-rebuild/vsts-publishfront6.png)
11. İçinde **ortam**, simgeye **1 iş, 1 görev** altında **hazırlama**.
12. Abonelik ve uygulama adı'nı seçtikten sonra bunlar açık **Azure App Service'e dağıtma** görev. Dağıtımı kullanmak üzere yapılandırılmış **hazırlama** dağıtım yuvası. Bu kod inceleme ve onaya bu yuvadaki otomatik olarak oluşturur.

     ![Yuva](./media/contoso-migration-rebuild/vsts-publishfront7.png)

13. İçinde **işlem hattı**, bunlar yeni aşama ekleyin.

    ![Yeni ortam](./media/contoso-migration-rebuild/vsts-publishfront8.png)

14. Seçmeleri **yuva ile Azure App Service dağıtımı**ve ortam adını **Prod**.
15. Bunlar tıklayarak **1 iş, 2 görev**, abonelik, uygulama hizmeti adı seçin ve **hazırlama** yuvası.

    ![Ortam adı](./media/contoso-migration-rebuild/vsts-publishfront10.png)

16. Bunlar kaldırmak **yuvası Azure App Service'e dağıtma** işlem hattından. Ayrıca, önceki adımları tarafından var. yerleştirildi.

    ![Komut zincirinden Kaldır](./media/contoso-migration-rebuild/vsts-publishfront11.png)

13. Bunlar, işlem hattı kaydedin. İşlem hattını, bunlar tıklayın **dağıtım sonrası koşulları**.

    ![Dağıtım sonrası](./media/contoso-migration-rebuild/vsts-publishfront12.png)

14. Tanırlar **dağıtım sonrası onayları**, ve geliştirme müşteri adayı onaylayan olarak ekleyin.

    ![Dağıtım sonrası onayı](./media/contoso-migration-rebuild/vsts-publishfront13.png)

15. Derleme işlem hattı, bunlar elle bir yapı tetiklersiniz. Bu sitenin hazırlama yuvasını dağıtır yeni yayın işlem hattını tetikler. Contoso için yuva URL'sidir **https://smarthotelcontoso-staging.azurewebsites.net/**.
16. Yapı tamamlanana ve yayın yuvaya dağıtır sonra Azure DevOps Geliştirme lideri onay e-posta gönderir.
17. Geliştirme lideri tıklama **görüntülemek onay**ve onaylama veya reddetme DevOps Azure Portalı'nda istek.

    ![Onay e-postası](./media/contoso-migration-rebuild/vsts-publishfront14.png)

16. Müşteri adayı açıklamasını yapar ve onaylar. Bu, takas başlatır **hazırlama** ve **prod** yuvası ve yapı üretime taşır.

    ![Onaylama ve değiştirme](./media/contoso-migration-rebuild/vsts-publishfront15.png)

17. İşlem hattını değiştirme tamamlar.

    ![Değiştirmeyi tamamla](./media/contoso-migration-rebuild/vsts-publishfront16.png)

18. Takım denetimleri **prod** web uygulaması üretimde olduğunu doğrulamak için yuva **https://smarthotelcontoso.azurewebsites.net/**.


### <a name="deploy-the-petchecker-function-app"></a>PetChecker işlev uygulamasını dağıtma

Contoso yöneticileri gibi uygulamayı dağıtın.

1. Bunlar, Azure DevOps projesi için bağlanarak deposunu yerel olarak geliştirme makinesini kopyalayın.
2. Visual Studio'da, depodaki tüm dosyaları göstermek için klasörü açın.
3. Açtıklarında **src/PetCheckerFunction/local.settings.json** dosya ve depolama, Cosmos veritabanı ve görüntü işleme API'si için uygulama ayarları ekleyin.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function5.png)

4. Bunlar, kod tamamlama ve geri Azure yaptıkları değişiklikleri gönderme DevOps için eşitleme.
5. Yeni bir derleme işlem hattı ekleyin ve seçin **Azure DevOps Git** kaynağı için.
6. Seçmeleri **ASP.NET Core (.NET Framework)** şablonu.
7. Bunlar, şablon için Varsayılanları kabul edin.
8. İçinde **Tetikleyicileri**seçin sonra **sürekli tümleştirmeyi etkinleştir**, tıklatıp **Kaydet ve kuyruğa** derlemeyi başlatmak için.
9. Derleme başarılı olduktan sonra bir yayın ardışık düzeni oluşturdukları ekleme **yuva ile Azure App Service dağıtımı**.
10. Bunlar ortam adı **Prod**ve aboneliği seçin. Ayarladıkları **uygulama türü** için **işlev uygulaması**ve app service adı olarak **smarthotelpetchecker**.

    ![İşlev uygulaması](./media/contoso-migration-rebuild/petchecker2.png)

11. Bunlar bir yapıt ekleme **yapı**.

    ![Yapay Nesne](./media/contoso-migration-rebuild/petchecker3.png)

12. Tanırlar **sürekli dağıtım tetikleyicisi**, tıklatıp **Kaydet**.
13. Simgeye **yeni derlemeyi kuyruğa al** eksiksiz bir CI/CD işlem hattı çalıştırılacak.
14. İşlev dağıtıldıktan sonra Azure portalında ile göründüğü **çalıştıran** durumu.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function6.png)


7. Evcil hayvan denetleyicisi uygulamanın, beklenen şekilde çalışıp çalışmadığını test etmek için uygulamaya göz atın [ http://smarthotel360public.azurewebsites.net/Pets ](http://smarthotel360public.azurewebsites.net/Pets).
8. Bunlar, bir resim karşıya yüklenecek avatar tıklayın.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function7.png)

9. Küçük bir köpek kontrol etmek için istedikleri ilk fotoğraf var.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function8.png)

10. Uygulama kabul bir ileti döndürür.

    ![İşlev dağıtma](./media/contoso-migration-rebuild/function9.png)






## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapısının güvenliğini sağlamak Contoso artık gerekir.

### <a name="security"></a>Güvenlik

- Yeni veritabanlarını güvenli olmasını sağlamak contoso gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Uygulama SSL sertifikalarıyla kullanacak şekilde güncelleştirilmesi gerekir. 443 numaralı yanıtlamak için kapsayıcı örneğini yeniden dağıtılması.
- Contoso, gizli dizileri için Service Fabric uygulamalarını korumak için KeyVault kullanmayı düşünmeniz gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups-and-disaster-recovery"></a>Yedekleme ve olağanüstü durum kurtarma

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Contoso SQL yük devretme grupları veritabanı için bölgesel yük devretme sağlamak için kullanmayı düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso, ACR premium SKU için coğrafi çoğaltmayı yararlanabilirsiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Cosmos DB otomatik olarak yedekler. Contoso için [daha fazla bilgi edinin](https://docs.microsoft.com/azure/cosmos-db/online-backup-and-restore) bu işlem hakkında.

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını kendi [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso azure'da SmartHotel360 uygulaması oluşturur. Azure App Services Web Apps için ön uç VM'si yeniden şirket içi uygulama. Azure Kubernetes Service (AKS) tarafından yönetilen kapsayıcıları dağıtılmış mikro hizmetler kullanarak uygulama arka ucu oluşturulur. Contoso evcil hayvan fotoğraf uygulaması ile işlevini iyileştirdik.




