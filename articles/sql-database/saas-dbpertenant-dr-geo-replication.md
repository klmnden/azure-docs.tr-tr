---
title: Azure SQL veritabanı coğrafi çoğaltmayı kullanarak SaaS uygulamaları için olağanüstü durum kurtarma | Microsoft Docs
description: Azure SQL veritabanı coğrafi çoğaltmalar bir çok kiracılı SaaS uygulaması bir kesinti durumunda kurtarmayı kullanmayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: AyoOlubeko
ms.author: ayolubek
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: b6f0d25f621768f79e8262f38617152e91692a23
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62129865"
---
# <a name="disaster-recovery-for-a-multi-tenant-saas-application-using-database-geo-replication"></a>Coğrafi çoğaltma veritabanı kullanan çok kiracılı SaaS uygulaması için olağanüstü durum kurtarma

Bu öğreticide, bir kiracı başına veritabanı modeli kullanılarak uygulanan bir çok kiracılı SaaS uygulaması için eksiksiz olağanüstü durum kurtarma senaryosuna keşfedin. Uygulama kesintiden korumak için kullandığınız [ _coğrafi çoğaltma_ ](sql-database-geo-replication-overview.md) bir alternatif kurtarma bölgesinde yinelemeler için katalog ve Kiracı veritabanları oluşturmak için. Bir kesinti oluşursa, hızlı bir şekilde normal iş işlemlerini sürdürmek için bu çoğaltmaların yük devri. Yük devretmede, özgün bölgede veritabanları veritabanlarının kurtarma bölgesindeki ikincil çoğaltma olur. Bu çoğaltmaların yeniden çevrimiçi duruma geldikten sonra bunlar otomatik olarak veritabanlarının kurtarma bölgesindeki durumuna flow'unu yakalayın. Kesinti giderildikten sonra geri özgün üretim bölgede veritabanları için başarısız.

Bu öğretici, yük devretme ve yeniden çalışma iş akışları açıklar. Şunları öğrenirsiniz:
> [!div class="checklist"]
> 
> * Kiracı kataloğa veritabanı ve elastik havuzu yapılandırma bilgilerini eşitleme
> * Uygulama, sunucuları ve havuzları kapsayan bir kurtarma ortamı farklı bir bölgede ayarlayın
> * Kullanım _coğrafi çoğaltma_ katalog ve Kiracı veritabanlarını kurtarma bölgeye çoğaltmak için
> * Uygulama ve Katalog ve Kiracı veritabanları kurtarma bölgeye yük devretme 
> * Daha sonra uygulamanın yükünü devretme, kesinti giderildikten sonra özgün bölgesiyle katalog ve Kiracı veritabanlarını yedekleme
> * Her bir kiracının veritabanının birincil konumu izlemek için üzerinden her bir kiracı veritabanı başarısız olarak güncelleştirme Kataloğu
> * Uygulama ve birincil Kiracı veritabanı gecikme süresini azaltmak için aynı Azure bölgesinde birlikte her zaman emin olun.  
 

Bu öğreticiye başlamadan önce aşağıdaki önkoşulların karşılandığından emin olun:
* Wingtip bilet SaaS veritabanı başına Kiracı uygulama dağıtılır. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Kiracı uygulama başına Wingtip bilet SaaS veritabanı keşfedin](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-replication-recovery-pattern"></a>Coğrafi çoğaltma kurtarma düzeniyle giriş

![Recovery mimarisi](media/saas-dbpertenant-dr-geo-replication/recovery-architecture.png)
 
Olağanüstü Durum Kurtarma (DR), uyumluluk nedenleriyle veya iş sürekliliği için birçok uygulama için önemli bir konu olmasına. Süren hizmet kesintisi olması, iyi hazırlanmış bir kurtarma planı iş kesintileri en aza indirebilirsiniz. Coğrafi çoğaltmayı kullanarak en düşük RPO ve RTO yük kısa fark etmeye devredilemeyen bir kurtarma bölgedeki veritabanı çoğaltmalarına sağlar.

Coğrafi çoğaltma üzerinde dayalı olarak bir kurtarma planı, üç farklı bölümden oluşur:
* Kurulum-oluşturulmasının ve bakımının Kurtarma Ortamı'nın
* Kurtarma - uygulama ve veritabanlarının bir kesinti oluşursa kurtarma ortamına yük devretme
* Repatriation - yük devretme uygulama ve veritabanlarının geri özgün bölgesiyle uygulama giderildikten sonra 

Tüm bölümleri dikkatlice olarak değerlendirilmesi uygun ölçekte özellikle işletim varsa. Genel olarak, plan çeşitli hedeflere ulaşmak gerekir:

* Kurulum
    * Kurmak ve kurtarma bölgesinde bir Ayna görüntüsünü ortamı güncelleştirin. Elastik havuzlar oluşturma ve bu kurtarma ortamda herhangi bir veritabanına çoğaltma kurtarma bölgesindeki kapasite ayırır. Bu ortamı bulundurmak, bunlar sağlanırken yeni Kiracı veritabanları çoğaltmayı içerir.  
* Kurtarma
    * Günlük maliyetleri azaltmak için ölçeği aşağı kurtarma ortamı kullanıldığı havuzları ve veritabanlarını kurtarma bölgesindeki tam işlem kapasitesi edinmek için ölçeği gerekir
    * Yeni Kiracı kurtarma bölgesinde olabildiğince çabuk sağlamayı etkinleştir  
    * Kiracılar öncelik sırasına geri yüklemek için iyileştirilmiş
    * Kiracılar çevrimiçi mümkün olduğunca hızlı pratik olduğunda paralel adımları uygulayarak almak için iyileştirilmiş
    * Hata, yeniden başlatılabilir ve etkili dayanıklı olması
    * Özgün bölge tekrar çevrimiçi olursa Orta uçuştaki işlemi iptal etmek mümkün olabilir.
* Repatriation 
    * Kiracılar için en az etki ile özgün bölgede kopyaya kurtarma bölgeden veritabanları yük devretme: hiçbir veri kaybı ve en düşük süresi Kiracı başına çevrimdışı.   

Bu öğreticide, Azure SQL veritabanı ve Azure platformunun özelliklerini kullanarak bu sorunları ele alınır:

* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), tüm gerekli kapasite mümkün olan en kısa sürede ayırmak için. Azure Resource Manager şablonları, bir Ayna görüntüsünü üretim sunucuları ve kurtarma bölgedeki elastik havuzların sağlamak için kullanılır.
* [Coğrafi çoğaltma](sql-database-geo-replication-overview.md), tüm veritabanları için zaman uyumsuz olarak çoğaltılan salt okunur olan ikincil veritabanı oluşturmayı. Kesinti sırasında kurtarma bölgedeki çoğaltmalarına yük devri.  Kesinti giderildikten sonra veri kaybı olmadan özgün bölgede veritabanları için geri başarısız.
* [Zaman uyumsuz](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) yük devretme işlemleri için çok sayıda veritabanıyla yük devretme süresini en aza indirmek için Kiracı öncelik sırasıyla gönderilir.
* [Parça yönetim kurtarma özellikleri](sql-database-elastic-database-recovery-manager.md), kurtarma ve repatriation sırasında Katalog veritabanı girişlerini değiştirmek için. Bu özellikler uygulama yapılandırmadan, Kiracı veritabanlarına konumdan bağımsız olarak bağlanmasına olanak sağlar.
* [SQL sunucu DNS diğer adları](dns-alias-overview.md)sorunsuz hangi bölge ne olursa olsun uygulama işletim yeni kiracılar sağlama etkinleştirmek için. DNS diğer adları konumuna bakılmaksızın etkin Kataloğu'na bağlanmak kataloğun eşitleme işlemini izin vermek için de kullanılır.

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma betiklerini alma 

> [!IMPORTANT]
> Tüm Wingtip bilet yönetim komut dosyaları gibi DR betikleri örnek kalitesi ve üretim ortamında kullanılmamalıdır. 

Bu öğretici ve Wingtip uygulama kaynak kodunu de kullanılan betikler kullanılabilir kurtarma [GitHub deposundan Kiracı başına veritabanı Wingtip bilet SaaS](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant/). Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet yönetim komut dosyaları engelini kaldırmak için.

## <a name="tutorial-overview"></a>Öğreticiye genel bakış
Bu öğreticide, ilk coğrafi çoğaltma Wingtip bilet uygulaması veritabanlarını ve çoğaltmaları farklı bir bölgede oluşturmak için kullanın. Ardından, bir kesintiden kurtarma benzetimini yapmak için bu bölgeye yük devri. Tamamlandığında, uygulama kurtarma bölgesinde tamamen çalışır durumdadır.

Daha sonra bir ayrı repatriation adımda, özgün bölgeye kurtarma bölgesindeki katalog ve Kiracı veritabanları yük devretme. Uygulama ve veritabanlarının repatriation kullanılabilir kalır. Tamamlandığında, uygulama özgün bölgede tamamen çalışır durumdadır.

> [!Note]
> Uygulamanın içine kurtarılmadan _eşleştirilmiş bölge_ uygulamanın dağıtıldığı bölge. Daha fazla bilgi için [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

## <a name="review-the-healthy-state-of-the-application"></a>Uygulamanın sistem durumunu gözden geçirin

Kurtarma işlemine başlamadan önce uygulama normal sağlıklı durumunu gözden geçirin.
1. Wingtip biletleri olay hub'ı web tarayıcınızda açın (http://events.wingtip-dpt.&lt ; kullanıcı&gt;. trafficmanager.net - değiştirin &lt; kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).
    * Sayfanın en altına gidin ve kataloğu sunucu adını ve konumunu altbilgisindeki dikkat edin. Uygulamanın dağıtıldığı bölge konumdur.
    *İPUCU: Fare görünen büyütmek için konumu üzerinde gelin. * 
     ![Olay hub'ı sağlıklı duruma özgün bölgede](media/saas-dbpertenant-dr-geo-replication/events-hub-original-region.png)

2. Contoso Konser Salonu kiracıda'a tıklayın ve kendi olay sayfasını açın.
    * Alt bilgisinde Kiracı sunucu adına dikkat edin. Konum Kataloğu server'ın konumu ile aynı olacaktır.

3. İçinde [Azure portalında](https://portal.azure.com), uygulamanın dağıtıldığı kaynak grubunu açın
    * Sunucuları dağıtıldığı bölgeyi dikkat edin. 

## <a name="sync-tenant-configuration-into-catalog"></a>Kataloğu eşitleme Kiracı yapılandırma

Bu görevde, sunucuları, elastik havuzları ve veritabanlarını yapılandırmasını Kiracı kataloğa eşitlenen bir işlem başlatın. İşlem, bu bilgileri kataloğunda güncel kalmasını sağlar.  İşlem, özgün bölgesinde olup olmadığını veya kurtarma bölgesinde etkin Kataloğu ile çalışır. Yapılandırma bilgilerini özgün ortamı ile tutarlı kurtarma ortamı sağlamak için kurtarma işleminin bir parçası olduğundan ve ardından emin olmak için daha sonra sırasında repatriation özgün bölge içinde yapılan değişiklikler ile tutarlı hale getirilene olarak kullanılır Kurtarma Ortamı. Katalog ayrıca Kiracı kaynaklarını kurtarma durumunu izlemek için kullanılır

> [!IMPORTANT]
> Kolaylık olması için yerel PowerShell işleri veya istemci kullanıcı oturum açma bilgilerinizi altında çalışan oturumları olarak bu öğreticileri, eşitleme işlemi ve uzun süre çalışan diğer kurtarma ve repatriation işlemler uygulanır. Oturum birkaç saat sonra sona erer ve ardından işleri başarısız olur, verilen kimlik doğrulama belirteçleri. Bir üretim senaryosunda, uzun süre çalışan işlemler güvenilir Azure Hizmetleri çalıştıran bir hizmet sorumlusu altında tür olarak uygulanmalıdır. Bkz: [bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal).

1. İçinde _PowerShell ISE_, ...\Learning Modules\UserConfig.psm1 dosyasını açın. Değiştirin `<resourcegroup>` ve `<user>` satırlarında 10 ve 11 uygulamasını dağıtırken kullandığınız değerine sahip.  Dosya Kaydet!

2. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve ayarlayın:
    * **$DemoScenario = 1**, Kiracı sunucusuna eşitlenen bir arka plan işi başlatın ve yapılandırma bilgileri kataloğa havuzu

3. Tuşuna **F5** eşitleme betiği çalıştırmak için. Kiracı kaynaklarını yapılandırmasını eşitlemek için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/sync-process.png)

Arka planda çalışan PowerShell penceresine bırakın ve bu öğreticinin geri kalanını ile devam edin. 

> [!Note]
> Eşitleme işlemi katalog bir DNS diğer adı aracılığıyla bağlanır. Geri yükleme ve etkin Kataloğu'na işaret edecek şekilde repatriation sırasında bu diğer ad değiştirildi. Eşitleme işlemi katalog güncel kurtarma bölgesinde veritabanı veya havuz yapılandırma değişiklikleri ile tutar.  Repatriation sırasında bu değişiklikleri özgün bölgede eşdeğer kaynaklara uygulanır.

## <a name="create-secondary-database-replicas-in-the-recovery-region"></a>İkincil veritabanı çoğaltmalarını kurtarma bölgesinde oluşturun.

Bu görevde, yinelenen uygulama örneği dağıtır ve Katalog ve tüm Kiracı veritabanlarında bir kurtarma bölgeye çoğaltır bir işlem başlatın.

> [!Note]
> Bu öğreticide, Wingtip bilet örnek uygulamaya coğrafi çoğaltma koruması ekler. Coğrafi çoğaltma kullanan bir uygulamayı bir üretim senaryosunda, her Kiracı gizliliğe coğrafi olarak yedekli bir veritabanından ile sağlanması. Bkz: [Azure SQL veritabanı ile yüksek oranda kullanılabilir hizmetler tasarlama](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)

1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 2**yansıma kurtarma ortamı oluşturun ve çoğaltmak katalog ve Kiracı veritabanları

2. Betiği çalıştırmak için **F5**'e basın. Çoğaltmaları oluşturmak için yeni bir PowerShell oturumu açılır.
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-replication/replication-process.png)  

## <a name="review-the-normal-application-state"></a>Normal uygulama durumunu gözden geçirin

Bu noktada uygulama normal olarak özgün bölgede çalışan ve coğrafi çoğaltma tarafından artık korunur.  İkincil çoğaltmaların salt okunur mevcut tüm veritabanları için kurtarma bölgesindeki. 

1. Azure portalında kaynak grupları ve bir kaynak grubu - ile oluşturulan Not bakmak kurtarma bölgesindeki kurtarma soneki. 

2. Kurtarma kaynak grubundaki kaynakları keşfedin.  

3. Contoso Konser Salonu veritabanında tıklayın _tenants1-dpt -&lt;kullanıcı&gt;-kurtarma_ sunucusu.  Coğrafi çoğaltma üzerinde sol taraftaki tıklayın. 

    ![Contoso Konser coğrafi çoğaltma bağlantısı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication.png) 

Azure bölgeleri Haritası özgün bölgede birincil ve ikincil kurtarma bölgesindeki arasındaki coğrafi çoğaltma bağlantı unutmayın.  

## <a name="fail-over-the-application-into-the-recovery-region"></a>Kurtarma bölgesi uygulamasına yük devretme

### <a name="geo-replication-recovery-process-overview"></a>Coğrafi çoğaltma kurtarma işlemine genel bakış

Kurtarma betiği aşağıdaki görevleri gerçekleştirir:

1. Traffic Manager uç noktası web uygulamasının özgün bölgede devre dışı bırakır. Uç noktanın devre dışı kullanıcılar geçersiz bir durumda uygulama için özgün bölge Kurtarma sırasında çevrimiçi olması bağlanmasını engeller.

1. Katalog veritabanı zorla yük devretme, birincil veritabanı olmak için kurtarma bölgesinde kullanır ve güncelleştirmeleri _activecatalog_ kurtarma katalog sunucusuna işaret etmek için diğer ad.

1. Güncelleştirmeleri _newtenant_ kurtarma bölgede Kiracı sunucusuna işaret etmek için diğer ad. Bu diğer adı değiştirme, tüm yeni kiracılara veritabanları kurtarma bölgesinde sağlanmasını önler. 

1. Üzerinden başarısız olmadan önce Kiracı veritabanlarına erişimi engellemek için tüm mevcut kiracıda kurtarma Kataloğu çevrimdışı olarak işaretler.

1. Tüm elastik havuzlar ve özgün bölge yapılandırmalarında yansıtmak üzere Kurtarma bölgede çoğaltılan tek veritabanları yapılandırmasını güncelleştirir. (Havuzlar veya Kurtarma Ortamı ' çoğaltılmış veritabanlarında maliyetlerini azaltmak için normal işlemler sırasında ölçeklenirse bu görevi yalnızca gereklidir).

1. Kurtarma bölgesindeki web uygulaması için Traffic Manager uç nokta sağlar. Bu uç noktayı etkinleştirme, yeni kiracılar sağlama uygulaması sağlar. Bu aşamada, var olan kiracılar hala çevrimdışı.

1. Toplu istekleri zorlamak için öncelik sırasına veritabanları yük devretme gönderir.
    * Böylece veritabanlarını paralel olarak tüm havuzlar arasında yük devredildi toplu işler halinde düzenlenir.
    * Yük devretme isteklerini zaman uyumsuz işlemleri kullanarak hızlı bir şekilde gönderildi ve birden çok istek eşzamanlı olarak işlenebilecek gönderilir.

   > [!Note]
   > Bir kesinti senaryosunda birincil özgün bölgesinde çevrimdışı veritabanlarıdır.  Zorla yük devretme üzerinde ikincil sonları birincil bağlantı kalan tüm kuyruğa alınan işlemler uygulama çalışırken. Bu öğreticide gibi bir DR tatbikatı senaryoda, yük devretme sırasında herhangi bir güncelleştirme etkinliği ise olabilir bazı veri kaybı. Kurtarma bölgeye özgün bölgesindeki veritabanları yük devretme sonrasında, repatriation sırasında normal bir yük devretme hiçbir veri kaybı sağlamak için kullanılır.

1. Ne zaman veritabanları yük Devredilmiş belirlemek için SQL veritabanı hizmeti izler. Bir kiracı veritabanı yük devretme sonra Kiracı veritabanının kurtarma durumunu kaydetmek ve Kiracı çevrimiçi olarak işaretlemek için katalog güncelleştirir.
    * Katalogda çevrimiçi işaretlenmiş hemen sonra Kiracı veritabanlarını uygulama tarafından erişilebilir.
    * Rowversion değerler Kiracı veritabanında bir toplamı Kataloğu'nda depolanır. Bu değer, veritabanı kurtarma bölgesinde güncelleştirilmiş olup olmadığını belirlemek repatriation işlem veren parmak izi olarak görev yapar.

### <a name="run-the-script-to-fail-over-to-the-recovery-region"></a>Kurtarma bölgeye yük devretmek için betiği çalıştırın

Artık uygulama dağıtılan ve kurtarma betiği çalıştırmak bir bölgede kesinti olduğunu düşünün:

1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 3**, uygulama içinde bir kurtarma bölgeye göre çoğaltmalarına yük devretme kurtarma

2. Betiği çalıştırmak için **F5**'e basın.  
    * Betik, yeni bir PowerShell penceresi açar ve ardından bir dizi paralel olarak çalışan PowerShell işler başlatır. Bu işlerin Kiracı veritabanlarına yönelik kurtarma bölgeyi devredin.
    * Kurtarma bölgesi _eşleştirilmiş bölge_ uygulamayı dağıttığınız Azure bölgesi ile ilişkili. Daha fazla bilgi için [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.
    ![Yük devretme işlemi](media/saas-dbpertenant-dr-geo-replication/failover-process.png)

> [!Note]
> Kodu kurtarma işleri için keşfetmek için PowerShell betikleri ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\RecoveryJobs klasöründe gözden geçirin.

### <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirin

Uygulama, uygulama uç noktasını Traffic Manager'da devre dışı bırakıldığında kullanılamaz. Katalog Kurtarma bölgeye devredilen ve tüm kiracılar çevrimdışı olarak işaretlenmiş sonra uygulamayı tekrar çevrimiçi olana. Uygulama kullanılabilir olsa da, her Kiracı veritabanına yük devretme kadar olay hub'ında çevrimdışı görüntülenir. Çevrimdışı Kiracı veritabanlarını işlemek için uygulamanızı tasarlamak önemlidir.

1. Katalog veritabanı kurtarıldıktan sonra en kısa sürede web tarayıcınızda Wingtip biletleri olay hub'ı yenileyin.
   * Alt bilgisinde katalog sunucusu adı artık olduğuna dikkat edin. bir _-kurtarma_ sonek ve kurtarma bölgede bulunuyor.
   * Henüz geri yüklenmiyor, kiracılar çevrimdışı olarak işaretlenir ve seçilemeyen dikkat edin.  

     > [!Note]
     > Kurtarmak için yalnızca birkaç veritabanlarıyla kurtarma tamamlanmadan önce çevrimdışı durumdayken kiracılar göremeyebilirsiniz için tarayıcıyı yenilemeniz mümkün olmayabilir. 
 
     ![Olay hub'ı çevrimdışı](media/saas-dbpertenant-dr-geo-replication/events-hub-offlinemode.png) 

   * Çevrimdışı bir kiracının olayları sayfası doğrudan açarsanız 'Çevrimdışı Kiracı' bir bildirim görüntüler. Contoso Konser Salonu çevrimdışı olduğunda, örneğin, açmaya http://events.wingtip-dpt.&lt ; kullanıcı&gt;.trafficmanager.net/contosoconcerthall ![ Contoso çevrimdışı sayfası](media/saas-dbpertenant-dr-geo-replication/dr-in-progress-offline-contosoconcerthall.png) 

### <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgesinde yeni bir kiracı sağlama
Üzerinde var olan tüm Kiracı veritabanlarında bile başarısız olmuş önce kurtarma bölgesinde yeni kiracılar sağlayabilirsiniz.  

1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 komut dosyasını açın ve aşağıdaki özelliği ayarlayın:
    * **$DemoScenario = 4**, Kurtarma bölgesinde yeni bir kiracı sağlama

2. Tuşuna **F5** betiği çalıştırın ve yeni Kiracı sağlama. 

3. Tamamlandığında Hawthorn Hall olayları sayfası tarayıcıda açılır. Alt bilgisinden Hawthorn Hall veritabanı kurtarma bölgesinde sağlandığında unutmayın.
    ![Hawthorn Hall olayları sayfası](media/saas-dbpertenant-dr-geo-replication/hawthornhallevents.png) 

4. Tarayıcıda Hawthorn Hall dahil görmek için Wingtip biletleri olay hub'ı sayfayı yenileyin. 
    * Geri yüklemek diğer kiracılar için beklemenize gerek kalmadan Hawthorn Hall sağladıysanız, diğer kiracılardan çevrimdışı olabilir.


## <a name="review-the-recovered-state-of-the-application"></a>Uygulamasını kurtarılan durumunu gözden geçirin

Kurtarma işlemi tamamlandığında, uygulama ve tüm kiracılar kurtarma bölgesinde tam işlevlidir. 

1. Bir PowerShell konsol penceresi içinde görünen tüm kiracılar kurtarıldığı gösterir. sonra olay hub'ı yenileyin.  Kiracılar tüm yeni Kiracı, Hawthorn Hall dahil olmak üzere çevrimiçi görünür.

    ![Olay hub'ı kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-replication/events-hub-with-hawthorn-hall.png)

2. İçinde [Azure portalında](https://portal.azure.com), kaynak grupları listesini açın.  
    * Dikkat edin, Kurtarma kaynak grubunun yanı sıra ile dağıttığınız kaynak grubunu _-kurtarma_ soneki.  Kurtarma kaynak grubunun tüm kurtarma işlemi sırasında oluşturulan kaynakları kesinti sırasında oluşturulan yeni kaynakları içerir.  

3. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri dikkat edin:
   * Katalog ve tenants1 sunucuları kurtarma sürümleri ile _-kurtarma_ soneki.  Tüm bu sunucularda geri yüklenen katalog ve Kiracı veritabanlarını orijinal bölgede kullanılan adları vardır.

   * _Tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Bu sunucu, kesinti sırasında yeni kiracılar sağlama için kullanılır.
   * App Service adlı _olayları-wingtip-dpt -&lt;recoveryregion&gt;-&lt;kullanıcıya & gt_; olayları Uygulama Kurtarma örneği olduğu. 

     ![Azure kurtarma kaynakları](media/saas-dbpertenant-dr-geo-replication/resources-in-recovery-region.png) 
    
4. Açık _tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Veritabanı içerdiği bildirimi _hawthornhall_ ve elastik havuz _Pool1_.  _Hawthornhall_ veritabanı içinde bir elastik veritabanı olarak yapılandırıldığında _Pool1_ elastik havuz.

5. Kaynak grubuna gidin ve Contoso Konser Salonu veritabanında tıklayarak _tenants1-dpt -&lt;kullanıcı&gt;-kurtarma_ sunucusu. Coğrafi çoğaltma üzerinde sol taraftaki tıklayın.
    
    ![Yük devretmenin ardından contoso veritabanı](media/saas-dbpertenant-dr-geo-replication/contoso-geo-replication-after-failover.png)

## <a name="change-tenant-data"></a>Kiracı verilerini değiştir 
Bu görevde, Kiracı veritabanlarını birini güncelleştirin. 

1. Tarayıcınızda, olaylar listesinde için Contoso Konser Salonu bulun ve son olay adını not edin.
2. İçinde *PowerShell ISE*, ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 betik aşağıdaki değeri ayarlayın:
    * **$DemoScenario = 5** kurtarma bölgesinde bir kiracıdan gelen bir olay Sil
3. Tuşuna **F5** komut dosyası yürütme
4. Contoso Konser Salonu olayları sayfayı yenileyin (http://events.wingtip-dpt.&lt ; kullanıcı&gt;.trafficmanager.net/contosoconcerthall - yerine &lt; kullanıcı&gt; dağıtımınızın kullanıcı değeri ile) ve son olayın silindiğini dikkat edin.

## <a name="repatriate-the-application-to-its-original-production-region"></a>Uygulama, özgün üretim bölgeye repatriate

Bu görevi, uygulama kendi özgün bölgeye repatriates. Gerçek bir senaryoda, kesinti giderildiğinde repatriation başlatmak.

### <a name="repatriation-process-overview"></a>Repatriation işlemine genel bakış

![Repatriation mimarisi](media/saas-dbpertenant-dr-geo-replication/repatriation-architecture.png)

Repatriation işlemi:
1. Herhangi bir bekleyen veya uçuşan veritabanı geri yükleme isteğini iptal eder.
2. Güncelleştirmeleri _newtenant_ kaynak bölgede Kiracı sunucusuna işaret etmek için diğer ad. Bu diğer adı değiştirme, tüm yeni kiracılara veritabanları artık kaynak bölgede sağlanacak sağlar.
3. Değiştirilen Kiracı verilerde özgün bölgeye çekirdeğini
4. Kiracı veritabanlarını öncelik sırasına devreder.

Yük devretme, veritabanını özgün bölgesiyle etkili bir şekilde taşır. Veritabanı yük devrettiğinde, tüm açık bağlantılar bırakılır ve veritabanı birkaç saniye boyunca kullanılamaz. Uygulamaları yeniden bağlandıklarından emin olmak için yeniden deneme mantığı ile yazılmış olması gerekir.  Bu kısa bağlantı kesme fark genelde olsa da, çalışma saatleri dışında veritabanları repatriate tercih edebilirsiniz. 


### <a name="run-the-repatriation-script"></a>Repatriation betiği çalıştırın
Şimdi kesinti çözümlendi ve repatriation betiğini çalıştırmak şimdi düşünün.

1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-FailoverToReplica\Demo-FailoverToReplica.ps1 betiği.

2. Katalog eşitleme işlemi, PowerShell örneğinde hala çalışmakta olduğunu doğrulayın.  Gerekirse, ayarlayarak yeniden başlatın:
    * **$DemoScenario = 1**, kataloğa Kiracı sunucu, havuz ve veritabanı yapılandırma bilgilerini Eşitlemeyi Başlat
    * Betiği çalıştırmak için **F5**'e basın.

3.  Ardından repatriation işlemini başlatmak için aşağıdakileri ayarlayın:
    * **$DemoScenario = 6**, uygulamanın kendi özgün bölgeye repatriate
    * Tuşuna **F5** yeni bir PowerShell penceresi kurtarma betiği çalıştırmak için.  Repatriation birkaç dakika sürer ve PowerShell penceresinde izlenebilir.
    ![Repatriation işlemi](media/saas-dbpertenant-dr-geo-replication/repatriation-process.png)

4. Komut dosyası çalıştırılırken, olay hub'ı sayfayı yenileyin (http://events.wingtip-dpt.&lt ; kullanıcı&gt;. trafficmanager.net)
    * Tüm kiracılar çevrimiçi ve erişilebilir bu süreci boyunca olduğuna dikkat edin.

5. Repatriation tamamlandıktan sonra olay hub'ı yenileyin ve Hawthorn Hall olayları sayfasını açın. Bu veritabanı özgün bölgeye repatriated, dikkat edin.
    ![Olay hub'ı repatriated](media/saas-dbpertenant-dr-geo-replication/events-hub-repatriated.png)


## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Uygulama ve veritabanı emin olmak için uygulama tasarlama birlikte 
Uygulama, her zaman aynı bölgede Kiracı veritabanı örneğinden bağlamasına şekilde tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikme süresini azaltır. Bu iyileştirme, uygulama veritabanı etkileşimi uygulama için kullanıcı etkileşimi chattier olduğunu varsayar.  

Kiracı veritabanlarını kurtarma ve özgün bölgeler arasında bir süre boyunca repatriation yayılmış. Her veritabanı için veritabanı Kiracı sunucu adı bir DNS araması yaparak bulunur bölge uygulama arar. SQL veritabanı'nda bir diğer ad sunucu adıdır. Diğer adlı sunucu adını, bölge adını içerir. Uygulama, veritabanı ile aynı bölgede değilse, veritabanı sunucusu ile aynı bölgede örneğine yeniden yönlendirir.  Aynı bölgede veritabanı örneğine yönlendirerek uygulama ve veritabanı arasındaki gecikme süresini en aza indirir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> 
> * Kiracı kataloğa veritabanı ve elastik havuzu yapılandırma bilgilerini eşitleme
> * Uygulama, sunucuları ve havuzları kapsayan bir kurtarma ortamı farklı bir bölgede ayarlayın
> * Kullanım _coğrafi çoğaltma_ katalog ve Kiracı veritabanlarını kurtarma bölgeye çoğaltmak için
> * Uygulama ve Katalog ve Kiracı veritabanları kurtarma bölgeye yük devretme 
> * Uygulama, katalog ve Kiracı veritabanlarını orijinal bölgeye kesinti giderildikten sonra yeniden çalışma

Azure SQL veritabanı sağlar, iş sürekliliği sağlamak teknolojileri hakkında daha fazla bilgi [iş Sürekliliğine genel bakış](sql-database-business-continuity.md) belgeleri.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulamasına geliştirecek ek öğreticilerden](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
