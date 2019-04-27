---
title: Azure MySQL ve Azure App Service için bir Contoso Linux hizmet Masası uygulamayı yeniden düzenleme | Microsoft Docs
description: Nasıl Contoso şirket içi Linux uygulaması geçirerek yeniden düzenler öğrenmek için Azure App Service Web Katmanı ve Azure SQL veritabanı için GitHub'ı kullanma.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 4ff3f129838a43bd7684dc10e1653dab969e9c1e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60669782"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-multiple-regions-with-azure-app-service-traffic-manager-and-azure-mysql"></a>Contoso geçişi: Contoso Linux hizmet Masası uygulamayı Azure App Service, Traffic Manager ve Azure MySQL ile birden çok bölgeye yeniden düzenleme

Nasıl Contoso kendi şirket içi iki katmanlı Linux hizmet Masası uygulaması (osTicket) geçirerek yeniden düzenler Bu makale, Azure App Service ile GitHub tümleştirmesini ve Azure MySQL.

Bu belge, şirket içi kaynaklara Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

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
Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulamayı yeniden düzenleme | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Bu makalede
[11. makale: Azure DevOps hizmetlerinde TFS yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel360 uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir

Bu makalede, Contoso bir iki katmanlı Linux Apache MySQL PHP (LAMP) servis Masası Uygulaması'nı (osTicket) Azure'a geçirir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).


## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyürken ve yeni pazarlara taşıma. Bu ek müşteri hizmeti aracıları gerekir. 
- **Ölçek**: Contoso daha fazla müşteriye hizmet aracı iş ölçekler ekleyebilmeniz çözüm oluşturulmalıdır.
- **Dayanıklılığı artırmak**:  Son sorunlar sistemin yalnızca iç kullanıcılar etkilenen. Yeni iş modeli, dış kullanıcılar etkilenecek ve Contoso gerek uygulamayı çalıştırmaya her zaman.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı en iyi geçiş yöntemini belirlemek üzere sabitlenmiş:

- Uygulamanın geçerli şirket içi kapasite ve performans ölçeklendirmeniz gerekir.  Contoso Azure'nın üzerine ölçeklendirmesinden faydalanmak için uygulamanın taşınıyor.
- Contoso, uygulama kod tabanını bir sürekli teslim işlem hattı için taşımak istediğiniz.  Uygulama değişiklikleri github'a gibi Contoso bu değişiklikleri operasyon personeli için görevleri olmadan dağıtmak istediğiniz.
- Uygulama büyüme ve yük devretme özellikleri içeren dayanıklı olması gerekir. Contoso iki farklı Azure bölgelerinde uygulama dağıtma ve otomatik olarak ölçeklendirmek için ayarlamak istersiniz.
- Uygulamayı buluta taşındıktan sonra veritabanı yönetim görevleri en aza indirmek contoso istiyor.

## <a name="solution-design"></a>Çözüm tasarımı
Sonra bunların hedefleri ve gereksinimleri sabitleme, Contoso tasarlar ve bir dağıtım çözümü inceler ve geçiş için kullanılacak Azure Hizmetleri dahil olmak üzere geçiş işlemi tanımlar.


## <a name="current-architecture"></a>Geçerli mimari

- Uygulama, iki VM arasında (OSTICKETWEB ve OSTICKETMYSQL) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).

![Geçerli mimari](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Önerilen mimarisi

Önerilen mimari aşağıda verilmiştir:

- Bir Azure App Service'te iki Azure bölgesi oluşturarak OSTICKETWEB web katmanı uygulamasında geçirilecektir. PHP 7.0 Docker kapsayıcısı kullanarak Linux için Azure App Service uygulanacaktır.
- Uygulama kodu Github'a taşınır ve Azure Web uygulamasına GitHub ile sürekli teslimat için yapılandırılmış.
- Azure uygulama sunucuları, birincil (Doğu ABD 2) ve ikincil (Orta ABD) bölgeye dağıtılır.
- Traffic Manager her iki bölgede de önünde iki Azure Web Apps ayarlanır.
- Traffic Manager, Doğu ABD 2 trafiği zorlamak için öncelik modunda yapılandırılır.
- Doğu ABD 2 Azure uygulama sunucu çevrimdışı olursa, kullanıcılar başarısız Orta ABD, uygulama üzerinde erişebilir.
- Uygulama veritabanı, MySQL Workbench araçları kullanarak Azure MySQL PaaS hizmetine geçirilecektir. Şirket içi veritabanı yerel olarak yedeklenebilir ve doğrudan Azure MySQL geri yüklendi.
- Veritabanı varsayılan olarak, üretim ağı (VNET-PROD-EUS2) birincil Doğu ABD 2 bölgesinde veritabanı alt ağdaki (PROD-DB-EUS2) alacağını:
- Üretim iş yüklerinin geçiş yapıyorsanız bu yana Azure kaynakları için uygulamayı üretim kaynak grubunda yer alacağı **ContosoRG**.
- Traffic Manager kaynak Contoso'nun altyapı kaynak grubuna dağıtılacak **ContosoInfraRG**.
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


![Senaryo mimarisi](./media/contoso-migration-refactor-linux-app-service-mysql/proposed-architecture.png) 


## <a name="migration-process"></a>Geçiş işlemi

Contoso geçiş işlemi aşağıdaki gibi tamamlayın:

1. İlk adım, Azure uygulama hizmetleri sağlama, Traffic Manager'ı ayarlama ve Azure MySQL örneği sağlama dahil olmak üzere Azure altyapı Kurulumu Contoso yöneticileri ayarlayın.
2. Azure hazırlandıktan sonra bunlar MySQL Workbench kullanarak veritabanını geçirin. 
3. Sonra veritabanı, GitHub ile sürekli teslim, Azure App Service için özel depo kurma azure'da çalışıyor ve osTicket uygulamayı yükleyin.
4. Azure portalında, bunlar uygulama Github'dan Azure App Service çalışan Docker kapsayıcısına yükleyin. 
5. Bunlar, DNS ayarlarını ince ve uygulama için otomatik ölçeklendirmeyi yapılandırma.

![Geçiş işlemi](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure App Service](https://azure.microsoft.com/services/app-service/) | Hizmeti çalışır ve Web siteleri için Azure PaaS hizmeti kullanan uygulamalar ölçeklendirir.  | Fiyatlandırma örnekleri ve gerekli özellikleri boyutuna bağlıdır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | DNS, Azure veya dış Web siteleri ve Hizmetleri için doğrudan kullanıcılara kullanan bir yük dengeleyici. | Fiyatlandırma, alınan DNS sorgusu sayısı ve izlenen uç noktaların sayısını temel alır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/traffic-manager/).
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/) | Veritabanı açık kaynak MySQL Server altyapısını temel alır. Bir tam olarak yönetilen, Kurumsal kullanıma hazır, topluluk tabanlı, uygulama geliştirmeye ve dağıtıma yönelik bir hizmet olarak MySQL veritabanı sağlar. | Fiyatlandırma, işlem, depolama ve yedekleme gereksinimlerine göre. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/mysql/).

 
## <a name="prerequisites"></a>Önkoşullar

Bu senaryo çalıştırmak için Contoso gerekenler aşağıda verilmiştir.

**Gereksinimler** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Contoso, daha önce bu makale serisinde abonelik oluşturuldu. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor. 
**Azure altyapı** | Contoso bölümünde anlatıldığı gibi kendi Azure altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).



## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl tamamlanacağını garanti aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure uygulama hizmetleri sağlama**: Contoso yöneticileri birincil ve ikincil bölgede Web Apps sağlanır.
> * **2. adım: Traffic Manager'ı ayarlama**: Bunlar, Yönlendirme ve Yük Dengeleme trafiği için Web uygulamaları, önünde Traffic Manager'kurmak ayarlayın.
> * **3. adım: Sağlama MySQL**: Azure'da, bunlar Azure MySQL veritabanı örneği sağlayın.
> * **4. adım: Veritabanı geçişi**: Bunlar, MySQL Workbench kullanarak veritabanını geçirin. 
> * **5. adım: GitHub'ı ayarlama**: Bunlar, uygulama web siteleri/kodu yerel bir GitHub deposunu ayarlayın.
> * **6. adım: Web uygulaması dağıtmanın**: Bunlar, github'dan web uygulamaları dağıtın.




## <a name="step-1-provision-azure-app-services"></a>1. Adım: Azure uygulama hizmetleri sağlama

Contoso yöneticileri sağlama iki Web uygulaması (her bölgede bir tane) kullanarak Azure uygulama hizmetleri.

1. Bunlar birincil Doğu ABD 2 bölgesinde bir Web uygulaması kaynağı oluşturun (**osticket eus2**) Azure Market.
2. Üretim kaynak grubunda kaynak koyarlar **ContosoRG**.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. Bunlar birincil bölgede yeni bir App Service planı oluşturur (**uygulama SVP EUS2**), standart boyuttaki kullanarak.

     ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Bunlar, bir Docker kapsayıcı PHP 7.0 çalışma zamanı yığını ile bir Linux işletim sistemi seçin.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Bunlar ikinci bir web uygulaması oluşturma (**osticket cu**) ve App service planı Orta ABD bölgesi için.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Daha fazla yardıma mı ihtiyacınız var?**

- Hakkında bilgi edinin [Azure App Service Web apps](https://docs.microsoft.com/azure/app-service/overview).
- Hakkında bilgi edinin [Linux üzerinde Azure App Service'te](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>2. Adım: Traffic Manager'ı ayarlama

Contoso yöneticileri osTicket web katmanı üzerinde çalışan Web uygulamaları için gelen web isteklerini yönlendirmek için trafik Yöneticisi'ni ayarlayın.

1. Traffic Manager kaynak oluşturdukları (**osticket.trafficmanager.net**) Azure Market. Bunlar, Doğu ABD 2 birincil sitedir böylece öncelik yönlendirme kullanır. Bunlar, altyapı kaynak grubunda kaynak yerleştirin (**ContosoInfraRG**). Traffic Manager küreseldir ve belirli bir konuma bağlı olduğunu unutmayın

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Şimdi, Traffic Manager uç noktaları ile yapılandırın. Bunlar birincil site olarak Doğu ABD 2 Web uygulaması Ekle (**osticket eus2**) ve orta ABD uygulama ikincil olarak (**osticket cu**).

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. Uç noktaları ekledikten sonra bunları izleyebilir.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- Hakkında bilgi edinin [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Hakkında bilgi edinin [trafiği bir öncelik uç noktasına yönlendirme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>3. Adım: MySQL için Azure veritabanı sağlama

Contoso yöneticileri birincil Doğu ABD 2 bölgesinde bir MySQL veritabanı örneğinde sağlayın.

1. Azure portalında kaynak MySQL için Azure veritabanı oluştururlar. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Adı ekledikleri **contosoosticket** için Azure veritabanı. Bunlar üretim kaynak grubuna veritabanı ekleyin **ContosoRG**ve kimlik bilgilerini belirtin.
3. Şirket içi MySQL veritabanı sürümü, 5.7 olduğundan, uyumluluk için bu sürümü seçin. Bunlar, kendi veritabanı gereksinimlerine göre varsayılan değerleri kullanır.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. İçin **fazladan yedek seçenekleri**, kullanmayı seçmeleri **coğrafi olarak yedekli**. Bu seçenek bir kesinti oluşursa, kendi ikincil Orta ABD bölgesinde veritabanını geri yükleme sağlar. Veritabanı'na sağladığınızda, bu seçenek yalnızca yapılandırabilirsiniz.

    ![Yedeklilik](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Bunlar, bağlantı güvenliği ayarlayın. Veritabanında > **bağlantı güvenliği**, bunlar veritabanı, Azure hizmetlerine erişmek izin vermek için güvenlik duvarı kurallarını ayarlayın.
5. Bunlar yerel iş istasyonunda istemci IP adresi için başlangıç ve bitiş IP adreslerini ekleyin. Bu geçişi gerçekleştirme veritabanı istemci yanı sıra MySQL veritabanına erişmek Web apps sağlar.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>4. Adım: Veritabanını geçirme

Contoso yöneticileri veritabanını yedekleme ve geri yükleme ile MySQL araçlarını kullanarak geçirin. Bunlar MySQL Workbench'i yükleyin, OSTICKETMYSQL veritabanını yedekleyin ve sonra MySQL sunucusu için Azure veritabanı'na geri.

### <a name="install-mysql-workbench"></a>MySQL Workbench’i yükleme

1. Onlar iade [önkoşulları ve indirmeler MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. MySQL Workbench ile uyumlu olarak Windows için yükledikleri [yükleme yönergeleri](https://dev.mysql.com/doc/workbench/en/wb-installing.html). Makine üzerinde yükledikleri OSTICKETMYSQL VM ve Azure'a internet üzerinden erişilebilir olmalıdır.
3. MySQL Workbench içinde bir MySQL bağlantı OSTICKETMYSQL oluştururlar. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Bunlar veritabanı olarak dışarı aktarma **osticket**, yerel kendi içinde bir dosya için.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. Veritabanı yerel olarak yedeklendiğinden sonra bunlar MySQL örneği için Azure veritabanına bağlantı oluşturun.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Şimdi, (Kurtarma) Azure MySQL örneği, kendi içinde veritabanında dosyasını içeri aktarabilirsiniz. Yeni bir şema (osticket) için örneği oluşturulur.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. Verileri geri yüklendikten sonra Workbench kullanılarak sorgulanabilir ve Azure Portalı'nda görünür.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Son olarak, web apps üzerinde veritabanı bilgilerini güncelleştirmeniz gerekir. MySQL örneğinde açtıklarında **bağlantı dizeleri**. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. Dizeleri listede, bunlar Web uygulaması ayarları bulun ve bunları kopyalamak için tıklayın.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Bunlar bir Not Defteri penceresi açın ve dize yeni bir dosyaya yapıştırın ve osticket veritabanı, MySQL örneği ve kimlik bilgileri ayarları eşleşecek şekilde güncelleştirin.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Sunucu adını ve oturum açma işlemi doğrulayabilirsiniz **genel bakış** de Azure portalında MySQL örneği.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>5. Adım: GitHub'ı ayarlama

Contoso yöneticileri, Azure MySQL yeni bir özel GitHub deposu ve kümelerini osTicket veritabanına bir bağlantı oluşturun. Ardından Azure Web uygulamasında uygulama yükleyebilirsiniz.  

1.  Bunlar OsTicket yazılım genel GitHub deposuna gidin ve Contoso GitHub hesabına çatal.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. Çatal oluşturduktan sonra bunlar gidin **dahil** klasörü ve bulma **ost config.php** dosya.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. Tarayıcıda dosyayı açar ve bunlar düzenleyin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. Düzenleyici'de bunlar veritabanı ayrıntılarını, özellikle güncelleştirme **DBHOST** ve **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Daha sonra değişiklikleri işleyin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Her web uygulaması için (**osticket eus2** ve **osticket cu**), bunlar değiştirme **uygulama ayarları** Azure portalında.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Bağlantı dizesi adı ile girmeleri **osticket**ve dizeyi Not Defteri'ne kopyalayın **değer alanı**. Seçmeleri **MySQL** açılan listedeki dize yanındaki ve ayarları kaydedin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>6. Adım: Web uygulamalarını yapılandırma

Geçiş işleminin son adımı Contoso yöneticileri osTicket web siteleri ile web uygulamalarını yapılandırın.



1. Birincil web uygulamasında (**osticket eus2**) açtıklarında **dağıtım seçeneği** ve kaynağı **GitHub**.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Bunlar dağıtım seçenekleri seçin.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. Sonra yapılandırma gösterildiği gibi bekleyen Azure portalında seçeneklerini ayarlama.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. Yapılandırma güncelleştirilir ve osTicket web uygulamasını Azure App Service çalışan Docket kapsayıcıya Github'dan yüklendikten sonra site etkin olarak gösterir.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Bunlar ikincil web uygulaması için yukarıdaki adımları yineleyin (**osticket cu**).
6. Site yapılandırdıktan sonra Traffic Manager profili erişilebilir. DNS adı osTicket uygulamanın yeni bir konumdur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Contoso kolay bir DNS adı ister. Diğer ad (CNAME) kaydı oluşturdukları **osticket.contoso.com** kendi etki alanı denetleyicilerinde DNS, Traffic Manager adına işaret ettiği.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Hem yapılandırma **osticket eus2** ve **osticket cu** web uygulamaları özel ana bilgisayar adlarını izin vermek için.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Otomatik ölçeklendirmeyi ayarlayın

Son olarak, bunlar uygulama için otomatik ölçeklendirmeyi ayarlayın. Bu uygulama aracıları kullanma gibi uygulama örneği artırmak ve azaltmak iş gereksinimlerine göre sağlar. 

1. App Service'te **uygulama SRV EUS2**, açtıklarında **ölçek birimi**.
2. Bunlar, 10 dakika geçerli örneği için CPU yüzdesi % 70'in olduğunda, bir örnek sayısını artırır ve tek bir kural ile yeni bir otomatik ölçeklendirme ayarı yapılandırın.

    ![Otomatik Ölçeklendirme](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Bunlar aynı ayarı yapılandırmak **uygulama SRV cu** ikincil bölgeye üzerinden uygulama başarısız olursa aynı davranışı uygulanmasını sağlamak için. Tek fark, bu yük devretme işlemleri için yalnızca olduğundan bunlar 1 örnek sınırı kümesi olur.

   ![Otomatik Ölçeklendirme](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile bir özel GitHub deposunu kullanarak sürekli teslim ile bir Azure Web uygulamasında çalışıyor osTicket uygulama bulunanad. Uygulamanın artan esneklik için iki bölgeleri çalışıyor. OsTicket veritabanı Azure veritabanı'nda MySQL için geçişten sonra PaaS platformuna çalışıyor.

İçin Temizle, Contoso aşağıdakileri yapması gerekir: 
- VMware sanal makineleri, vCenter stok kaldırın.
- Şirket içi Vm'leri yerel yedekleme işlerden kaldırın.
- Güncelleştirme iç belgeleri, yeni konumlar ve IP adreslerini gösterir. 
- Şirket içi sanal makineleri ile etkileşim kuran tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.
- Uygulamayı çalışır durumda olduğunu izlemek için osticket trafficmanager.net URL'sinde işaret etmek için izleme ve çalışan yeniden yapılandırın.

## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Artık çalışan bir uygulamayla Contoso tam olarak çalışır hale getirme ve kendi yeni altyapısının güvenliğini sağlamak için gerekir.

### <a name="security"></a>Güvenlik

Contoso güvenlik ekibi, güvenlik sorunları belirlemek için uygulamayı gözden. Bunlar, osTicket uygulama ve MySQL veritabanı örneğinde arasındaki iletişim için SSL yapılandırılmamış belirledik. Veritabanı trafiği ele emin olmak için bunu yapmanız gerekecektir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/mysql/howto-configure-ssl).

### <a name="backups"></a>Yedeklemeler

- OsTicket web uygulamaları durum verileri içermez ve böylece yedeklenmesi gerekmez.
- Veritabanı için yedeklemeyi yapılandırma gerekmez. MySQL için Azure veritabanı sunucu yedeklemelerini otomatik olarak oluşturur ve depolar. Bunlar, dayanıklı ve üretime hazır, bu nedenle veritabanı için coğrafi yedeklilik kullanmayı seçtiniz. Yedeklemeler, sunucunuz bir-belirli bir noktaya için geri yüklemek için kullanılabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/mysql/concepts-backup).


### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- PaaS dağıtımı için hiçbir lisans sorunları vardır.
- Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında.



