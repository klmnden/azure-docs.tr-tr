---
title: Azure SQL veritabanı coğrafi çoğaltma'yı kullanarak SaaS uygulamaları için olağanüstü durum kurtarma | Microsoft Docs
description: Bir çok kiracılı SaaS uygulaması bir kesinti durumunda kurtarmayı Azure SQL Database coğrafi çoğaltmalar kullanmayı öğrenin
keywords: sql veritabanı öğreticisi
services: sql-database
author: AyoOlubeko
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/09/2018
ms.author: ayolubek
ms.openlocfilehash: c6f3da52643caa9aa1172db5b884c5336c409715
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Veritabanı coğrafi çoğaltma kullanarak çok kiracılı SaaS uygulaması için olağanüstü durum kurtarma

Bu öğreticide, Kiracı başına veritabanı modeli kullanılarak uygulanan bir çok kiracılı SaaS uygulaması için bir tam olağanüstü durum kurtarma senaryosunda keşfedin. Uygulama bir kesintisi korumak için kullandığınız [ _coğrafi çoğaltma_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) çoğaltmaları katalog ve Kiracı veritabanları için bir alternatif kurtarma bölgede oluşturmak için. Bir kesinti oluşursa, hızlı bir şekilde normal iş işlemlerini sürdürmek için bu çoğaltmalar için yük devri. Yük devretme, özgün bölge veritabanlarında kurtarma bölgede ve veritabanlarını ikincil çoğaltmaların haline gelir. Bu çoğaltmalar yeniden çevrimiçi duruma geldikten sonra bunlar otomatik olarak kurtarma bölge veritabanlarında durumunu Yakala. Kesinti giderildikten sonra özgün üretim bölge veritabanlarında dön başarısız.

Bu öğretici yük devretme ve yeniden çalışma iş akışları araştırır. Bilgi edineceksiniz nasıl yapılır:
> [!div classs="checklist"]

>* Kiracı kataloğuna veritabanı ve esnek havuzu yapılandırma bilgilerini eşitleme
>* Uygulama, sunucuları ve havuzları kapsayan bir kurtarma ortamını alternatif bir bölgede ayarlama
>* Kullanım _coğrafi çoğaltma_ katalog ve Kiracı veritabanları için kurtarma bölge çoğaltmak için
>* Uygulama ve Katalog ve Kiracı veritabanları kurtarma bölgeye yük devri 
>* Daha sonra uygulama başarısız, kesinti çözümlendikten sonra özgün bölgesine katalog ve Kiracı veritabanlarını yedeklemek
>* Her bir kiracının veritabanının birincil konumunu izlemek için üzerinden her bir kiracı veritabanı başarısız olarak güncelleştirme Kataloğu
>* Uygulama ve birincil Kiracı veritabanı her zaman gecikmesini azaltmak için aynı Azure bölgesinde birlikte bulunan emin olun  
 

Bu öğreticiye başlamadan önce aşağıdaki önkoşulların tamamlandığından emin olun:
* Wingtip biletleri SaaS veritabanı Kiracı uygulama başına dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfedin.](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Coğrafi çoğaltma kurtarma düzeni giriş

![Kurtarma mimarisi](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Olağanüstü Durum Kurtarma (DR) önemli bir pek çok uygulama uyumluluk nedenleriyle veya iş sürekliliği için konudur. Uzun süren hizmet kesintisi olması, iyi hazırlıklı DR planı iş kesintiyi en aza indirebilirsiniz. Coğrafi çoğaltma'yı kullanarak düşük RPO'ya ve RTO yük kısa fark edilecek Devredilebilen bir kurtarma bölgede veritabanı çoğaltmaları tutarak sağlar.

Coğrafi çoğaltma üzerinde dayalı olarak bir kurtarma planı üç farklı bölümden oluşur:
* Kurulum-oluşturulmasını ve bakımını Kurtarma Ortamı'nın
* Kurtarma - uygulama ve veritabanları bir kesinti oluşursa kurtarma ortamına yük devretme
* Repatriation - yük devretme uygulamanın ve veritabanlarını geri özgün bölgesine uygulama giderildikten sonra 

Tüm bölümleri dikkatlice olarak kabul edilmesi özellikle ölçekte işletim varsa. Genel olarak, plan çeşitli hedeflere ulaşmak gerekir:

* Kurulum
    * Kurmak ve kurtarma bölge yansıma ortamında sürdürün. Esnek havuzu oluşturmayı ve bu kurtarma ortamında herhangi bir tek başına veritabanlarının çoğaltılması kurtarma bölgede kapasite saklı tutar. Bu ortam koruma sağlanmış gibi yeni Kiracı veritabanı çoğaltma içerir.  
* Kurtarma
    * Ölçeklendirilmiş aşağı kurtarma ortamı günlük maliyetleri en aza indirmek için kullanıldığı havuzları ve tek başına veritabanları kurtarma bölgede tam işlem kapasitesi edinmeye ölçeklendirilmesi gerekir
    * Yeni Kiracı kurtarma bölgede mümkün olan en kısa sürede sağlama etkinleştir  
    * Kiracılar öncelik sırasına geri yüklemek için en iyileştirilmiş
    * Çevrimiçi kiracılar mümkün olduğunca hızlı pratik burada paralel adımları yaparak almak için en iyileştirilmiş
    * Hata, yeniden başlatılabilir ve ıdempotent dayanıklı olmasını
    * Özgün bölge tekrar çevrimiçi olursa Orta yürütülen işlemi iptal etmek mümkün.
* Repatriation 
    * Kiracılar için en az etkiyle özgün bölgede çoğaltmaları kurtarma bölgesinden veritabanlarına yük devri: hiçbir veri kaybı ve minimum süre Kiracı başına çevrimdışı.   

Bu öğreticide, Azure SQL Database ve Azure platformu özelliklerini kullanarak bu zorluklar ele alınmıştır:

* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), tüm gerekli kapasite mümkün olan en kısa sürede ayırmak için. Azure Resource Manager şablonları bir yansıma kurtarma bölgede esnek havuzlar ve üretim sunucuları sağlamak için kullanılır.
* [Coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview), tüm veritabanları için zaman uyumsuz olarak çoğaltılmış salt okunur olan ikincil kopyalar oluşturun. Bir kesinti sırasında kurtarma bölgede çoğaltmaları yük.  Kesinti giderildikten sonra özgün bölge veri kaybı olmadan veritabanlarında dön başarısız.
* [Zaman uyumsuz](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) yük devretme işlemlerini çok sayıda veritabanları için yük devretme süresini en aza indirmek için Kiracı öncelik sırasıyla gönderilir.
* [Parça yönetim kurtarma özellikleri](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager), kurtarma ve repatriation sırasında veritabanı girişlerini kataloğunda değiştirmek için. Bu özellikler uygulama yapılandırmadan Kiracı veritabanı konumu bağımsız olarak bağlanmak için uygulamasını sağlar.
* [SQL server DNS diğer adları](https://docs.microsoft.com/azure/sql-database/dns-alias-overview), hangi bölgede bağımsız olarak uygulama işletim yeni kiracıya kesintisiz sağlamayı etkinleştirmek için. DNS diğer adları konumuna bakılmaksızın etkin Kataloğu'na bağlanmak Katalog eşitleme işlemi izin vermek için de kullanılır.

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma komut dosyalarını almak 

> [!IMPORTANT]
> Tüm Wingtip biletleri yönetim komut dosyaları gibi DR betikleri örnek kalite ve üretimde kullanılmayacak. 

Bu öğretici ve Wingtip uygulama kaynak koduna kullanılan komut kullanılabilir kurtarma [Wingtip biletleri SaaS veritabanı GitHub deposunu Kiracı başına](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/). Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri yönetim komut dosyaları engellemesini kaldırmak.

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu öğreticide, ilk coğrafi çoğaltma Wingtip biletleri uygulama veritabanlarını ve çoğaltmaları farklı bir bölgede oluşturmak için kullanın. Ardından, bir kesintisinden kurtarma benzetimini yapmak için bu bölgeye yük. Tamamlandığında, uygulama kurtarma bölgede tamamen çalışır durumdadır.

Daha sonra bir ayrı repatriation adımında, özgün bölge kurtarma bölgede katalog ve Kiracı veritabanları yük devri. Uygulama ve veritabanları repatriation kullanılabilir kalır. Tamamlandığında, uygulama özgün bölgede tamamen çalışır durumdadır.

> [!Note]
> Uygulama içine kurtarılan _eşleştirilmiş bölge_ uygulamanın dağıtıldığı bölge. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).

## <a name="review-the-healthy-state-of-the-application"></a>Uygulama sağlıklı durumunu gözden geçirin

Kurtarma işlemine başlamadan önce uygulamaya normal sağlıklı durumunu gözden geçirin.
1. Web tarayıcınızda Wingtip biletleri olay hub'ı açın (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net - Değiştir &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).
    * Sayfanın alt kısmına kaydırın ve Katalog sunucu adını ve konumunu altbilgisindeki dikkat edin. Konum, uygulamayı dağıttığınız bölgedir.
    *İpucu: fare görüntü büyütmek için konum gelin. * 
     ![Olayları hub sağlam durumda özgün bölge](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Contoso birlikte Hall Kiracı'tıklayın ve kendi olay sayfası açın.
    * Altbilgisinde Kiracı sunucu adı dikkat edin. Konum katalog sunucusunun konumu ile aynı olacaktır.

3. İçinde [Azure portal](https://portal.azure.com), uygulamanın dağıtıldığı kaynak grubunu açın
    * Sunucuları dağıtılmış olduğundan bölge dikkat edin. 

## <a name="sync-tenant-configuration-into-catalog"></a>Eşitleme Kiracı yapılandırması kataloğunda

Bu görevde, sunucuların, esnek havuzlar ve veritabanlarını yapılandırma Kiracı kataloğunda eşitlenen bir işlem başlatın. İşlem, bu bilgileri kataloğunda güncel kalmasını sağlar.  İşlem, özgün bölgede olup olmadığını veya kurtarma bölgede etkin Kataloğu ile çalışır. Yapılandırma bilgilerini özgün ortamıyla tutarlı kurtarma ortamı sağlamak için kurtarma işleminin bir parçası olduğundan ve ardından emin olmak için daha sonra sırasında repatriation özgün bölge içinde yapılan değişiklikler ile tutarlı hale gelir olarak kullanılır Kurtarma Ortamı. Katalog ayrıca Kiracı kaynaklarına kurtarma durumunu izlemek için kullanılır

> [!IMPORTANT]
> Basitlik için eşitleme işlemini ve diğer uzun süren kurtarma ve repatriation işlemler bu öğreticileri yerel Powershell işleri veya istemci kullanıcı oturum açma altında Çalıştır oturumları olarak uygulanır. Oturum açma birkaç saat sonra sona erecek ve işleri sonra başarısız olacak verilen kimlik doğrulama belirteçleri. Bir üretim senaryosunda, uzun süre çalışan işlemleri, bir hizmet sorumlusu altında çalışan bir tür, güvenilir Azure Hizmetleri olarak uygulanmalıdır. Bkz: [bir sertifika ile bir hizmet sorumlusu oluşturmak için kullanım Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal).

1. İçinde _PowerShell ISE_, ...\Learning Modules\UserConfig.psm1 dosyasını açın. Değiştir `<resourcegroup>` ve `<user>` satırlarında 10 ve 11 uygulama dağıtıldığında kullanılan değerine sahip.  Dosyayı kaydedin!

2. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve ayarlayın:
    * **$DemoScenario = 1**, Kiracı sunucu eşitlenen bir arka plan işi başlatmak ve yapılandırma bilgileri kataloğuna havuzu

3. Tuşuna **F5** eşitleme komut dosyasını çalıştırmak için. Kiracı kaynaklarına yapılandırmasını eşitlemek için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

Arka planda çalışan PowerShell penceresini bırakın ve öğreticinin geri kalanını ile devam edin. 

> [!Note]
> Eşitleme işlemi katalog bir DNS diğer adı üzerinden bağlanır. Bu diğer adı, geri yükleme ve etkin kataloğa işaret edecek şekilde repatriation sırasında değiştirilir. Eşitleme işlemi katalog güncel kurtarma bölgede veritabanı veya havuzu yapılandırma değişiklikleri ile tutar.  Repatriation sırasında bu değişiklikleri özgün bölgede denk kaynaklarına uygulanır.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>İkincil veritabanı çoğaltmalarını kurtarma bölgede oluşturma

Bu görevde, yinelenen uygulama örneği dağıtır ve Katalog ve tüm Kiracı veritabanı bir kurtarma bölgeye çoğaltan bir işlem başlatın.

> [!Note]
> Bu öğretici Wingtip biletleri örnek uygulamaya coğrafi çoğaltma koruması ekler. Coğrafi çoğaltma kullanan bir uygulamayı bir üretim senaryosunda, her bir kiracı outset coğrafi olarak çoğaltılmış bir veritabanından ile sağlanması. Bkz: [Azure SQL veritabanı kullanarak yüksek oranda kullanılabilir hizmetler tasarlama](https://docs.microsoft.com/azure/sql-database/sql-database-designing-cloud-solutions-for-disaster-recovery#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 2**, yansıma kurtarma ortamı oluşturmak ve çoğaltma katalog ve Kiracı veritabanları

2. Betiği çalıştırmak için **F5**'e basın. Çoğaltmaları oluşturmak için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Normal uygulama durumunu gözden geçirin
Bu noktada, uygulama özgün bölgede normal çalışıyor ve şimdi coğrafi çoğaltma tarafından korunur.  İkincil çoğaltmaların salt okunur kayıtlı tüm veritabanları için kurtarma bölgede. 
1. Kaynak grupları ve bir kaynak grubu ile - oluşturulan Not Azure portalında bakmak kurtarma bölgede kurtarma soneki. 

1. Kurtarma kaynak grubundaki kaynakları araştırın.  

1. Contoso birlikte Hall veritabanında tıklatın _tenants1-dpt -&lt;kullanıcı&gt;-kurtarma_ sunucu.  Coğrafi çoğaltma üzerinde sol tarafta'ı tıklatın. 

    ![Contoso birlikte coğrafi çoğaltma bağlantısı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

Azure bölgeleri eşlemesinde özgün bölgede birincil ve ikincil kurtarma bölgede arasında coğrafi çoğaltma bağlantı unutmayın.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Kurtarma bölge uygulamasına yük devri

### <a name="geo-replication-recovery-process-overview"></a>Coğrafi çoğaltma kurtarma işlemine genel bakış

Kurtarma komut dosyası, aşağıdaki görevleri gerçekleştirir:

1. Trafik Yöneticisi uç noktası özgün bölgede web uygulaması için devre dışı bırakır. Uç noktası devre dışı bırakma kullanıcılar geçersiz bir durumda uygulama için özgün bölge Kurtarma sırasında çevrimiçi olması bağlanmasını önler.

1. Zorla yük devretme katalog veritabanının birincil veritabanı olmak için kurtarma bölgede kullanır ve güncelleştirmeleri _activecatalog_ kurtarma katalog sunucusuna işaret etmek için diğer ad.

1. Güncelleştirmeleri _newtenant_ kurtarma bölgede Kiracı sunucusuna işaret etmek için diğer ad. Bu diğer adı değiştirme yeni Kiracı için veritabanlarını kurtarma bölgede sağlanır sağlar. 

1. Üzerinden başarısız olmadan önce Kiracı veritabanları için erişimi engellemek için tüm var olan kiracılar kurtarma kataloğunda çevrimdışı olarak işaretler.

1. Tüm esnek havuzlar ve çoğaltılmış bağımsız veritabanları özgün bölge yapılandırmalarında yansıtmak üzere Kurtarma bölgede yapılandırmasını güncelleştirir. (Maliyetlerini azaltmak için normal işlemler sırasında havuzları veya kurtarma ortamı çoğaltılmış veritabanlarında ölçeklenir bu görevi yalnızca gerekli değildir).

1. Kurtarma bölgede web uygulaması için trafik Yöneticisi uç noktası sağlar. Bu uç noktayı etkinleştirme, yeni kiracılar sağlamak uygulama sağlar. Bu aşamada, var olan kiracılar hala çevrimdışı.

1. Öncelik sırasına veritabanları zorlamak için istekleri toplu devri gönderir.
    * Böylece veritabanlarının paralel olarak tüm havuzlardaki devredildi toplu düzenlenir.
    * Yük devretme isteklerini zaman uyumsuz işlemleri kullanarak hızlı bir şekilde gönderildi ve birden çok isteği aynı anda işlenebilir gönderilir.

   > [!Note]
   > Bir kesinti senaryosunda birincil özgün bölgede çevrimdışı veritabanlarıdır.  Zorla başarısız üzerinden sayfasındaki ikincil sonlarını birincil bağlantı fazlalık sıraya alınan işlemler uygulamak çalışmadan. Bu öğretici gibi bir DR ayrıntıya senaryoda, yük devretme sırasında herhangi bir güncelleştirme etkinliği ise olabilir bazı veri kaybı. Kurtarma bölgeye özgün bölge veritabanlarında üzerinden başarısız olduğunda daha sonra repatriation sırasında normal bir yük devretme veri kaybı olduğundan emin olmak için kullanılır.

1. Veritabanları üzerinden başarısız olduğunda belirlemek için SQL veritabanı hizmetinin izler. Bir kiracı veritabanı yük devrettikten sonra Kiracı veritabanı kurtarma durumunu kaydetmek ve Kiracı çevrimiçi olarak işaretlemek için katalog güncelleştirir.
    * Katalogda çevrimiçi işaretlenmiş hemen sonra Kiracı veritabanları uygulama tarafından erişilebilir.
    * Kiracı veritabanında rowversion değerlerinin toplamı Kataloğu'nda depolanır. Bu değer, veritabanı kurtarma bölgede güncelleştirilmiş olup olmadığını belirlemek repatriation işlem sağlayan parmak izi gibi davranır.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Kurtarma bölgesine üzerinden vermesine komut dosyasını çalıştır

Şimdi bölge uygulamanın dağıtıldığı ve kurtarma komut dosyasını çalıştırmak kesinti olduğunu düşünün:

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 3**, Kurtarma bölgeye yapabilmesini kopyalara göre içine uygulama Kurtar

2. Betiği çalıştırmak için **F5**'e basın.  
    * Komut dosyasını yeni bir PowerShell penceresi açar ve ardından bir dizi paralel olarak çalışan PowerShell işleri başlatır. Bu işleri Kiracı veritabanları kurtarma bölgeye yük devri.
    * Kurtarma bölgedir _eşleştirilmiş bölge_ uygulama dağıtılan Azure bölgesiyle ilişkilendirilmiş. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.
    ![Yük devretme işlemi](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Kurtarma işleri için kod keşfetmek için ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\RecoveryJobs klasöründeki PowerShell komut dosyalarını gözden geçirin.

### <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirin
Uygulama uç noktasını Traffic Manager'da devre dışı olsa da, uygulama kullanılamıyor. Katalog Kurtarma bölgesine devredilen ve tüm kiracılar çevrimdışı olarak işaretlenmiş sonra uygulama tekrar çevrimiçi duruma getirilir. Uygulama kullanılabilir olsa da, veritabanını devredilen kadar her bir kiracı olay hub'ı çevrimdışı görüntülenir. Çevrimdışı Kiracı veritabanlarını işlemek için uygulamanızı tasarlayın önemlidir.

1. Katalog veritabanı kurtarıldıktan sonra derhal web tarayıcınızda Wingtip biletleri olay hub'ı yenileyin.
    * Altbilgisinde katalog sunucusu adı şimdi olduğuna dikkat edin bir _-kurtarma_ sonek ve kurtarma bölgede yer alır.
    * Henüz geri yüklenmez, kiracılar çevrimdışı olarak işaretlenir ve seçilemeyen dikkat edin.  

    > [!Note]
    > Kurtarmak için yalnızca birkaç olan veritabanları, Kurtarma tamamlanmadan önce çevrimdışı durumdayken kiracılar göremeyebilirsiniz için tarayıcıyı yenileyin mümkün olmayabilir. 
 
    ![Olay hub'ı çevrimdışı](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

    * Çevrimdışı bir kiracının olayları sayfası doğrudan açarsanız, 'Çevrimdışı Kiracı' bir bildirim görüntülenir. Contoso birlikte Hall çevrimdışıysa, örneğin, açmaya http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall ![çevrimdışı Contoso sayfası](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgede yeni bir kiracı sağlama
Varolan tüm Kiracı veritabanları üzerinden bile başarısız olmuş önce yeni kiracılar kurtarma bölgede sağlayabilirsiniz.  

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki özelliği ayarlayın:
    * **$DemoScenario = 4**, Kurtarma bölgede yeni bir kiracı sağlama

2. Tuşuna **F5** komut dosyasını çalıştırın ve yeni Kiracı sağlama. 

3. Tamamlandığında Hawthorn Hall Etkinlikler sayfasını tarayıcıda açılır. Altbilginin Hawthorn Hall veritabanı kurtarma bölgede sağlandığında unutmayın.
    ![Hawthorn Hall olayları sayfası](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. Tarayıcıda Hawthorn Hall dahil görmek için Wingtip biletleri olay hub'ı sayfayı yenileyin. 
    * Diğer kiracılar geri yüklemek beklemeden Hawthorn Hall sağlanan değilse, diğer kiracılar çevrimdışı olabilir.


## <a name="review-the-recovered-state-of-the-application"></a>Uygulamasını kurtarılan durumunu gözden geçirin

Kurtarma işlemi tamamlandığında, uygulama ve tüm kiracılar kurtarma bölgede tam işlevlidir. 

1. Tüm kiracılar kurtarılan PowerShell konsol penceresinde görünen gösterir sonra olay hub'ı yenileyin.  Kiracılar tüm yeni Kiracı Hawthorn Hall dahil olmak üzere çevrimiçi görünür.

    ![Olay hub'ı kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. İçinde [Azure portal](https://portal.azure.com), kaynak gruplarının listesini açın.  
    * Kurtarma kaynak grubu ile dağıttığınız kaynak grubunu fark _-kurtarma_ soneki.  Kurtarma kaynak grubu, tüm kurtarma işlemi sırasında oluşturulan kaynakların yanı sıra kesinti sırasında oluşturulan yeni kaynaklar içeriyor.  

3. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri dikkat edin:
    * Katalog ve tenants1 sunucularının kurtarma sürümleri ile _-kurtarma_ soneki.  Tüm bu sunuculara geri yüklenen katalog ve Kiracı veritabanları özgün bölgede kullanılan adlara sahip.

    * _Tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Bu sunucu, yeni kiracılar sırasında kesinti sağlamak için kullanılır.
    *   Uygulama hizmeti adlı, _olayları-wingtip-dpt -&lt;recoveryregion&gt;-&lt;kullanıcı & gt_; olayları Uygulama Kurtarma örneğini olduğu. 

    ![Azure kurtarma kaynakları ](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png)    
    
4. Açık _tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Veritabanı içerdiği bildirimi _hawthornhall_ ve esnek havuz _Pool1_.  _Hawthornhall_ veritabanını bir esnek veritabanı olarak yapılandırılmış _Pool1_ esnek havuz.

5. Kaynak grubuna geri gidin ve Contoso birlikte Hall veritabanında tıklayın _tenants1-dpt -&lt;kullanıcı&gt;-kurtarma_ sunucu. Coğrafi çoğaltma üzerinde sol tarafta'ı tıklatın.
    
    ![Yük devretme sonrasında contoso veritabanı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Kiracı verilerini değiştir 
Bu görevde, Kiracı veritabanlarından birini güncelleştirin. 

1. Tarayıcınızda, Contoso birlikte Hall için olaylar listesini bulun ve son olay adını not edin.
2. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 betik aşağıdaki değeri ayarlayın:
    * **$DemoScenario = 5** kurtarma bölgede bir kiracıdan gelen olay silme
3. Tuşuna **F5** komut dosyası yürütme
4. Contoso birlikte Hall olayları sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall - yerine &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile) ve son olay silinip silinmediğini dikkat edin.

## <a name="repatriate-the-application-to-its-original-production-region"></a>Uygulama kendi özgün üretim bölgeye repatriate

Bu görev, özgün bölge uygulamaya repatriates. Gerçek bir senaryoda, kesinti giderildikten sonra repatriation başlatması.

### <a name="repatriation-process-overview"></a>Repatriation işlemine genel bakış

![Repatriation mimarisi](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

Repatriation işlemi:
1. Tüm bekleyen veya yürütülen veritabanı geri yükleme isteği iptal eder.
2. Güncelleştirmeleri _newtenant_ kaynak bölgede kiracılar sunucusuna işaret etmek için diğer ad. Bu diğer adı değiştirme kaynağı bölgede şimdi sağlanacak veritabanlarını yeni Kiracı için sağlar.
3. Değiştirilen Kiracı verilerde özgün bölgeye çekirdeğini oluşturur
4. Kiracı veritabanları öncelik sırasına yöneltilir.

Yük devretme veritabanı özgün bölgesine etkili bir şekilde taşır. Veritabanı üzerinden başarısız olduğunda, açık olan bağlantıları bırakılır ve veritabanı birkaç saniye kullanılamaz. Uygulamaları yeniden bağlandıklarında emin olmak için yeniden deneme mantığı ile yazılması gerekir.  Bu kısa bağlantıyı kes fark genelde rağmen iş saatleri dışında veritabanları repatriate seçebilirsiniz. 


### <a name="run-the-repatriation-script"></a>Repatriation komut dosyasını çalıştır
Şimdi şimdi kesinti çözümlendi ve repatriation komut dosyasını çalıştırmak düşünün.

1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyası.

2. Katalog eşitleme işlemi, PowerShell örneğinde hala çalıştığından emin olun.  Gerekirse, ayarlayarak yeniden başlatın:
    * **$DemoScenario = 1**, Kiracı sunucu, havuzu ve veritabanı yapılandırma bilgilerini kataloğuna Eşitlemeyi Başlat
    * Betiği çalıştırmak için **F5**'e basın.

3.  Ardından repatriation işlemini başlatmak üzere ayarlayın:
    * **$DemoScenario = 6**, uygulamanın kendi özgün bölgeye repatriate
    * Tuşuna **F5** yeni bir PowerShell penceresi kurtarma komut dosyasını çalıştırmak için.  Repatriation birkaç dakika sürer ve PowerShell penceresinde izlenebilir.
    ![Repatriation işlemi](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Komut dosyası çalışırken, olay hub'ı sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net)
    * Tüm kiracılar çevrimiçi ve erişilebilir bu işlemi boyunca olduğuna dikkat edin.

5. Repatriation tamamlandıktan sonra olay hub'ı yenileyin ve Hawthorn Hall olayları sayfasını açın. Bu veritabanı için özgün bölge repatriated olduğunu dikkat edin.
    ![Olay hub'ı repatriated](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Uygulama ve veritabanı emin olmak için uygulama tasarlama birlikte 
Uygulama, her zaman Kiracı veritabanı ile aynı bölgede örneğinden bağlamasına şekilde tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikme süresini azaltır. Bu iyileştirme uygulama veritabanı etkileşimini kullanıcı uygulama etkileşim chattier olduğunu varsayar.  

Kiracı veritabanları kurtarma ve özgün bölgeler arasında repatriation sırasında süredir yayılır. Her veritabanı için veritabanı Kiracı sunucu adı bir DNS araması yaparak bulunur bölge uygulama arar. SQL veritabanı'nda bir diğer ad sunucu adıdır. Diğer sunucu adı, bölge adını içerir. Uygulama, veritabanı ile aynı bölgede değilse, veritabanı sunucusu ile aynı bölgede örneğine yönlendirir.  Veritabanı ile aynı bölgede örneğine yönlendirerek uygulama ve veritabanı arasındaki gecikme süresi en aza indirir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div classs="checklist"]

>* Kiracı kataloğuna veritabanı ve esnek havuzu yapılandırma bilgilerini eşitleme
>* Uygulama, sunucuları ve havuzları kapsayan bir kurtarma ortamını alternatif bir bölgede ayarlama
>* Kullanım _coğrafi çoğaltma_ katalog ve Kiracı veritabanları için kurtarma bölge çoğaltmak için
>* Uygulama ve Katalog ve Kiracı veritabanları kurtarma bölgeye yük devri 
>* Kesinti çözümlendikten sonra özgün bölge uygulama, katalog ve Kiracı veritabanlarını geri başarısız

Azure SQL veritabanı sağlar iş sürekliliği sağlamak için teknolojileri hakkında daha fazla bilgiyi [iş Sürekliliğine genel bakış](sql-database-business-continuity.md) belgeleri.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulamasına yapı ek öğreticileri](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
