---
title: Azure MySQL ve Azure App Service için bir Contoso Linux hizmet Masası uygulamayı yeniden düzenleme | Microsoft Docs
description: Nasıl Contoso şirket içi Linux uygulaması geçirerek yeniden düzenler öğrenmek için Azure App Service Web Katmanı ve Azure SQL veritabanı için GitHub'ı kullanma.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 5981c708abdaa12a662075cc5bf5aae14ccc35c2
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39002169"
---
# <a name="contoso-migration-refactor-a-contoso-linux-service-desk-app-to-the-azure-app-service-and-azure-mysql"></a>Contoso geçiş: Contoso Linux hizmet Masası uygulamayı Azure MySQL ve Azure App Service için yeniden düzenleme

Nasıl Contoso kendi şirket içi iki katmanlı Linux hizmet Masası uygulaması (osTicket) geçirerek yeniden düzenler Bu makale, Azure App Service ile GitHub tümleştirmesini ve Azure MySQL.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin onuncu değil. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Aynı altyapı tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso Wmware'de çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığını gösterir. Bunlar uygulama Vm'lerle değerlendirmek [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Rehost Azure sanal makineler ve yönetilen bir SQL örneği](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirdiğini gösterir. Uygulama web kullanarak VM'yi geçirme [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve veritabanı kullanarak uygulama [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) SQL yönetilen örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure sanal makinelerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme kendi SmartHotel Site Recovery hizmetini kullanarak Azure Iaas Vm'leri için gösterir.
[Makale 6: Azure sanal makineleri ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Bunlar, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site RECOVERY'yi kullanın. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Contoso osTicket Linux uygulamalarını Azure Site Recovery kullanarak Azure Iaas Vm'leri için nasıl geçirdiğini gösterir.
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso osTicket Linux uygulaması nasıl geçirdiğini gösterir. VM geçiş için Site Recovery ve MySQL Workbench'i Azure MySQL Server örneğine geçirmek için kullanırlar. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanı üzerinde yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulama için bir Azure kapsayıcı tabanlı web uygulaması geçirir ve uygulama veritabanı için Azure SQL Server geçirdiğini gösterir. | Kullanılabilir
Makale 10: Azure Web Apps ve Azure MySQL sunucusu üzerinde bir Linux uygulaması yeniden düzenleyin. | Contoso osTicket Linux uygulaması Azure App Service'te PHP 7.0 Docker kapsayıcısı kullanarak nasıl geçirdiğini gösterir. Dağıtım için kod tabanının Github'a geçirilir. Uygulama veritabanı için Azure MySQL geçirilir. | Bu makalede.
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir

Bu makalede, Contoso bir iki katmanlı Linux Apache MySQL PHP (LAMP) servis Masası Uygulaması'nı (osTicket) Azure'a geçirir. Bu açık kaynaklı uygulamayı kullanmak istiyorsanız, buradan indirebilirsiniz [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso büyürken ve yeni pazarlara taşıma. Gereksinim duydukları ek müşteri hizmeti aracıları. 
- **Ölçek**: Contoso daha fazla müşteriye hizmet aracı iş ölçekler ekleyebilmeniz için derlenmesi gereken çözüm.
- **Dayanıklılığı artırmak**: yalnızca iç kullanıcılar sistemle sorunlarla etkilenen. Yeni kendi iş modeli ile dış kullanıcılar etkilenir ve Contoso gerek uygulamayı çalıştırmaya her zaman.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı en iyi geçiş yöntemini belirlemek üzere sabitlenmiş:

- Uygulamanın geçerli şirket içi kapasite ve performans ölçeklendirmeniz gerekir.  Contoso Azure'nın üzerine ölçeklendirmesinden faydalanmak için uygulamanın taşınıyor.
- Contoso, uygulama kod tabanını bir sürekli teslim işlem hattı için taşımak istediğiniz.  Uygulama değişiklikleri github'a gibi bu değişiklikleri operasyon personeli için görevleri olmadan dağıtmak isterler.
- Uygulama büyüme ve yük devretme özellikleri içeren dayanıklı olması gerekir. İki farklı Azure bölgelerinde uygulama dağıtma ve otomatik olarak ölçeklendirmek için ayarlamak istersiniz.
- Uygulamayı buluta taşındıktan sonra veritabanı yönetim görevleri en aza indirmek contoso istiyor.

## <a name="solution-design"></a>Çözüm tasarımı
Kendi hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve bunların geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi tanımlayın.


## <a name="current-architecture"></a>Geçerli mimari

- Uygulama, iki VM arasında (OSTICKETWEB ve OSTICKETMYSQL) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).

![Geçerli mimari](./media/contoso-migration-refactor-linux-app-service-mysql/current-architecture.png) 


## <a name="proposed-architecture"></a>Önerilen mimarisi

Sonra geçerli mimari, hedefler ve Geçiş gereksinimleri sabitleme, Contoso bir dağıtım çözümü tasarlar ve bunların geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi, belirler.

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

1. İlk adım, Azure uygulama hizmetleri sağlama, Traffic Manager'ı ayarlama ve Azure MySQL örneği sağlama dahil olmak üzere Azure altyapı Kurulumu Contoso ayarlar.
2. Azure hazırlandıktan sonra bunlar MySQL Workbench kullanarak veritabanını geçirin. 
3. Veritabanı Azure'da çalışmaya başladıktan sonra bunların Azure App Service ile sürekli teslimat için bir GitHub özel deposu ayarlama ve osTicket uygulamayla yük.
4. Azure portalında, bunlar uygulama Github'dan Azure App Service çalışan Docker kapsayıcısına yükleyin. 
5. Bunlar, DNS ayarlarını ince ve uygulama için otomatik ölçeklendirmeyi yapılandırma.

![Geçiş işlemi](./media/contoso-migration-refactor-linux-app-service-mysql/migration-process.png) 


### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Azure App Service](https://azure.microsoft.com/services/app-service/) | Hizmeti çalışır ve Web siteleri için Azure PaaS hizmeti kullanan uygulamalar ölçeklendirir.  | Fiyatlandırma örnekleri ve gerekli özellikleri boyutuna bağlıdır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).
[Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | DNS, Azure veya dış Web siteleri ve Hizmetleri için doğrudan kullanıcılara kullanan bir yük dengeleyici. | Fiyatlandırma, alınan DNS sorgusu sayısı ve izlenen uç noktaların sayısını temel alır. | [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/traffic-manager/).
[MySQL için Azure Veritabanı](https://docs.microsoft.com/azure/mysql/) | Veritabanı açık kaynak MySQL Server altyapısını temel alır. Bir tam olarak yönetilen, Kurumsal kullanıma hazır, topluluk tabanlı, uygulama geliştirmeye ve dağıtıma yönelik bir hizmet olarak MySQL veritabanı sağlar. | Fiyatlandırma, işlem, depolama ve yedekleme gereksinimlerine göre. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/mysql/).

 
## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak istiyorsanız, işte olması gerekir.

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Erken makaleleri aboneliğinde bu dizide oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.<br/><br/> Daha ayrıntılı izinler gerekirse gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Azure altyapı** | Contoso bölümünde anlatıldığı gibi kendi Azure altyapısını ayarlamak [geçiş için Azure altyapı](contoso-migration-infrastructure.md).



## <a name="scenario-steps"></a>Senaryo adımları

Azure geçişi nasıl tamamlanacağını garanti aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Sağlama Azure uygulama hizmetleri**: Contoso Web Apps birincil ve ikincil bölgede sağlama.
> * **2. adım: Traffic Manager'ı ayarlama**: Yönlendirme ve Yük Dengeleme trafiği için Web uygulamaları, önünde Traffic Manager'kurmak ayarlayın.
> * **3. adım: Sağlama MySQL**: Azure'da Contoso Azure MySQL veritabanı örneği sağlar.
> * **4. adım: veritabanını geçirme**: Bunlar MySQL Workbench kullanarak veritabanını geçirme. 
> * **5. adım: GitHub'ı ayarlama**: Contoso uygulaması web siteleri/kod için yerel bir GitHub deposu ayarlar.
> * **6. adım: web uygulaması dağıtmanın**: Contoso web apps github'dan dağıtır.




## <a name="step-1-provision-azure-app-services"></a>1. adım: Sağlama Azure uygulama hizmetleri

Contoso sağlayan iki Web uygulaması (her bölgede bir tane) kullanarak Azure uygulama hizmetleri.

1. Bunlar birincil Doğu ABD 2 bölgesinde bir Web uygulaması kaynağı oluşturun (**osticket eus2**) Azure Market.
2. Üretim kaynak grubunda kaynak koyarlar **ContosoRG**.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app1.png) 

3. Bunlar birincil bölgede yeni bir App Service planı oluşturur (**uygulama SVP EUS2**), standart boyuttaki kullanarak.

     ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app2.png) 
    
4. Contoso PHP 7.0 bir Docker kapsayıcı çalışma zamanı yığını ile bir Linux işletim sistemi seçer.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app3.png) 

5. Bunlar ikinci bir web uygulaması oluşturma (**osticket cu**) ve App service planı Orta ABD bölgesi için.

    ![Azure uygulaması](./media/contoso-migration-refactor-linux-app-service-mysql/azure-app4.png) 


**Daha fazla yardıma mı ihtiyacınız var?**

- Hakkında bilgi edinin [Azure App Service Web apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview).
- Hakkında bilgi edinin [Linux üzerinde Azure App Service'te](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-intro).


## <a name="step-2-set-up-traffic-manager"></a>2. adım: Traffic Manager'ı ayarlama

Contoso osTicket web katmanı üzerinde çalışan Web uygulamaları için gelen web isteklerini yönlendirmek için Traffic Manager'ı ayarlar.

1. Contoso bir Traffic Manager kaynak oluşturur (**osticket.trafficmanager.net**) Azure Market. Bunlar, Doğu ABD 2 birincil sitedir böylece öncelik yönlendirme kullanır. Bunlar, altyapı kaynak grubunda kaynak yerleştirin (**ContosoInfraRG**). Traffic Manager küreseldir ve belirli bir konuma bağlı olduğunu unutmayın

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager1.png) 

2. Şimdi, Contoso yapılandırmanız Traffic Manager uç noktaları ile. Bunlar birincil site olarak Doğu ABD 2 Web uygulaması Ekle (**osticket eus2**) ve orta ABD uygulama ikincil olarak (**osticket cu**).

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager2.png) 

3. Uç noktaları ekledikten sonra bunları izleyebilir.

    ![Traffic Manager](./media/contoso-migration-refactor-linux-app-service-mysql/traffic-manager3.png)

**Daha fazla yardıma mı ihtiyacınız var?**

- Hakkında bilgi edinin [Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview).
- Hakkında bilgi edinin [trafiği bir öncelik uç noktasına yönlendirme](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-configure-priority-routing-method).
 
## <a name="step-3-provision-azure-database-for-mysql"></a>3. adım: MySQL için Azure veritabanı sağlama

Contoso hükümlerine bir MySQL örneği birincil Doğu ABD 2 bölgesinde veritabanı.

1. Azure portalında kaynak MySQL için Azure veritabanı oluştururlar. 

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-1.png)

2. Adı ekledikleri **contosoosticket** için Azure veritabanı. Bunlar üretim kaynak grubuna veritabanı ekleyin **ContosoRG**ve kimlik bilgilerini belirtin.
3. Şirket içi MySQL veritabanı sürümü, 5.7 olduğundan, uyumluluk için bu sürümü seçin. Bunlar, kendi veritabanı gereksinimlerine göre varsayılan değerleri kullanır.

     ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-2.png)

4. İçin **fazladan yedek seçenekleri**, Contoso seçer kullanılacak **coğrafi olarak yedekli**. Bu seçenek bir kesinti oluşursa, kendi ikincil Orta ABD bölgesinde veritabanını geri yükleme sağlar. Veritabanı'na sağladığınızda, bu seçenek yalnızca yapılandırabilirsiniz.

    ![Yedeklilik](./media/contoso-migration-refactor-linux-app-service-mysql/db-redundancy.png)

4. Bağlantı Güvenliği'kurmak Contoso. Veritabanında > **bağlantı güvenliği**, bunlar veritabanı, Azure hizmetlerine erişmek izin vermek için güvenlik duvarı kurallarını ayarlayın.
5. Bunlar yerel iş istasyonunda istemci IP adresi için başlangıç ve bitiş IP adreslerini ekleyin. Bu geçişi gerçekleştirme veritabanı istemci yanı sıra MySQL veritabanına erişmek Web apps sağlar.

    ![MySQL](./media/contoso-migration-refactor-linux-app-service-mysql/mysql-3.png)



## <a name="step-4-migrate-the-database"></a>4. adım: veritabanını geçirme

Contoso veritabanını yedekleme ve geri yükleme ile MySQL araçlarını kullanarak geçirir. Bunlar MySQL Workbench'i yükleyin, OSTICKETMYSQL veritabanını yedekleyin ve sonra MySQL sunucusu için Azure veritabanı'na geri.

### <a name="install-mysql-workbench"></a>MySQL Workbench’i yükleme

1. Contoso denetimleri [önkoşulları ve indirmeler MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. MySQL Workbench ile uyumlu olarak Windows için yükledikleri [yükleme yönergeleri](https://dev.mysql.com/doc/workbench/en/wb-installing.html). Makine üzerinde yükledikleri OSTICKETMYSQL VM ve Azure'a internet üzerinden erişilebilir olmalıdır.
3. MySQL Workbench içinde bir MySQL bağlantı OSTICKETMYSQL oluştururlar. 

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench1.png)

4. Bunlar veritabanı olarak dışarı aktarma **osticket**, yerel kendi içinde bir dosya için.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench2.png)

5. Veritabanı yerel olarak yedeklendiğinden sonra bunlar MySQL örneği için Azure veritabanına bağlantı oluşturun.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench3.png)

6. Şimdi, Contoso (Kurtarma) Azure MySQL örneği, kendi içinde veritabanında dosyasını içeri aktarabilirsiniz. Yeni bir şema (osticket) için örneği oluşturulur.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench4.png)

7. Verileri geri yüklendikten sonra Workbench kullanılarak sorgulanabilir ve Azure Portalı'nda görünür.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench5.png)

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench6.png)

8. Son olarak, Contoso web apps üzerinde veritabanı bilgileri güncelleştirilmesi gerekiyor. MySQL örneğinde açtıklarında **bağlantı dizeleri**. 

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench7.png)

9. Dizeleri listede, bunlar Web uygulaması ayarları bulun ve bunları kopyalamak için tıklayın.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench8.png)

10. Bunlar bir Not Defteri penceresi açın ve dize yeni bir dosyaya yapıştırın ve osticket veritabanı, MySQL örneği ve kimlik bilgileri ayarları eşleşecek şekilde güncelleştirin.

     ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench9.png)

11. Sunucu adını ve oturum açma işlemi contoso kontrol edebilirsiniz **genel bakış** de Azure portalında MySQL örneği.

    ![MySQL Workbench](./media/contoso-migration-refactor-linux-app-service-mysql/workbench10.png)


## <a name="step-5-set-up-github"></a>5. adım: GitHub'ı ayarlama

Contoso, yeni bir özel GitHub deposu oluşturur ve Azure MySQL osTicket veritabanına bir bağlantı kurar. Ardından Azure Web uygulamasında uygulama yükleyebilirsiniz.  

1.  Bunlar OsTicket yazılım genel GitHub deposuna gidin ve Contoso GitHub hesabına çatal.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github1.png)

2. Çatal oluşturduktan sonra bunlar gidin **dahil** klasörü ve bulma **ost config.php** dosya.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github2.png)


3. Tarayıcıda dosyayı açar ve bunlar düzenleyin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github3.png)

4. Veritabanı ayrıntılarını, Contoso Düzenleyicisi'nde özellikle güncelleştirmeleri **DBHOST** ve **DBUSER**. 

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github4.png)

5. Daha sonra değişiklikleri işleyin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github5.png)

6. Her web uygulaması için (**osticket eus2** ve **osticket cu**), Contoso değiştirme **uygulama ayarları** Azure portalında.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github6.png)

7. Bağlantı dizesi adı ile girmeleri **osticket**ve dizeyi Not Defteri'ne kopyalayın **değer alanı**. Seçmeleri **MySQL** açılan listedeki dize yanındaki ve ayarları kaydedin.

    ![GitHub](./media/contoso-migration-refactor-linux-app-service-mysql/github7.png)

## <a name="step-6-configure-the-web-apps"></a>6. adım: Web uygulamalarını yapılandırma

Geçiş sürecinin son adım olarak, Contoso, osTicket web siteleri ile web uygulamalarını yapılandırın.



1. Birincil web uygulamasında (**osticket eus2**) açılacağını **dağıtım seçeneği** ve kaynağı **GitHub**.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app1.png)

2. Bunlar dağıtım seçenekleri seçin.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app2.png)

3. Sonra yapılandırma gösterildiği gibi bekleyen Azure portalında seçeneklerini ayarlama.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app3.png)

4. Yapılandırma güncelleştirilir ve osTicket web uygulamasını Azure App Service çalışan Docket kapsayıcıya Github'dan yüklendikten sonra site etkin olarak gösterir.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app4.png)

5. Contoso sonra ikincil bir web uygulaması için yukarıdaki adımları yinelenir (**osticket cu**).
6. Site yapılandırdıktan sonra Traffic Manager profili erişilebilir. DNS adı osTicket uygulamanın yeni bir konumdur. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/app-service/app-service-web-tutorial-custom-domain#map-a-cname-record).

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app5.png)
    
7. Contoso kolay bir DNS adı ister. Diğer ad (CNAME) kaydı oluşturdukları **osticket.contoso.com** kendi etki alanı denetleyicilerinde DNS, Traffic Manager adına işaret ettiği.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app6.png)

8. Hem yapılandırma **osticket eus2** ve **osticket cu** web uygulamaları özel ana bilgisayar adlarını izin vermek için.

    ![Uygulama yapılandırma](./media/contoso-migration-refactor-linux-app-service-mysql/configure-app7.png)

### <a name="set-up-autoscaling"></a>Otomatik ölçeklendirmeyi ayarlayın

Son olarak, bunlar uygulama için otomatik ölçeklendirmeyi ayarlayın. Bu uygulama aracıları kullanma gibi uygulama örneği artırmak ve azaltmak iş gereksinimlerine göre sağlar. 

1. App Service'te **uygulama SRV EUS2**, Contoso açık **ölçek birimi**.
2. Bunlar, 10 dakika geçerli örneği için CPU yüzdesi % 70'in olduğunda, bir örnek sayısını artırır ve tek bir kural ile yeni bir otomatik ölçeklendirme ayarı yapılandırın.

    ![Otomatik Ölçeklendirme](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale1.png)

3. Bunlar aynı ayarı yapılandırmak **uygulama SRV cu** ikincil bölgeye üzerinden uygulama başarısız olursa aynı davranışı uygulanmasını sağlamak için. Tek fark, bu yük devretme işlemleri için yalnızca olduğundan bunlar 1 örnek sınırı kümesi olur.

   ![Otomatik Ölçeklendirme](./media/contoso-migration-refactor-linux-app-service-mysql/autoscale2.png)

##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile bir özel GitHub deposunu kullanarak sürekli teslim ile bir Azure Web uygulamasında çalışıyor osTicket uygulama bulunanad. Uygulamanın artan esneklik için iki bölgeleri çalışıyor. OsTicket veritabanı Azure veritabanı'nda MySQL için geçişten sonra PaaS platformuna çalışıyor.

Şimdi, Contoso şunları yapmanız gerekir: 
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



