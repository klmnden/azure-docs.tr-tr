---
title: 'SaaS uygulamaları için: Azure SQL veritabanı coğrafi olarak yedekli yedeklemeleri için olağanüstü durum kurtarma | Microsoft Docs'
description: Çok kiracılı bir SaaS uygulama kesinti durumunda kurtarmayı coğrafi olarak yedekli Azure SQL veritabanı yedeklemeleri kullanmayı öğrenin
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
ms.date: 01/14/2019
ms.openlocfilehash: c96f2dc2b44ea2118d9f0dd6c988017efcba5800
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60557078"
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Coğrafi geri yükleme, çok kiracılı bir SaaS uygulaması, veritabanı yedeklerinden kurtarma için kullanın

Bu öğretici, bir uygulanan kiracılı model başına veritabanı ile çok kiracılı bir SaaS uygulaması için eksiksiz olağanüstü durum kurtarma senaryosuna açıklar. Kullandığınız [coğrafi geri yükleme](sql-database-recovery-using-backups.md) katalog ve Kiracı veritabanları, bir alternatif kurtarma bölgeye otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden kurtarmak için. Kesinti giderildikten sonra kullandığınız [coğrafi çoğaltma](sql-database-geo-replication-overview.md) repatriate kendi özgün bölgeye değişen veritabanları için.

![Coğrafi geri yükleme mimarisi](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Coğrafi geri yükleme, Azure SQL veritabanı için düşük maliyetli olağanüstü durum kurtarma çözümüdür. Ancak, coğrafi olarak yedekli yedeklemelerden geri yükleme, bir saate kadar veri kaybına neden olabilir. Her veritabanı boyutuna bağlı olarak uzun zaman alabilir. 

> [!NOTE]
> Coğrafi geri yükleme yerine coğrafi çoğaltma ile mümkün olan en düşük RPO ve RTO uygulamalarla kurtarın.

Bu öğretici, geri yükleme hem repatriation iş akışları açıklar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]
> 
> * Veritabanı ve elastik havuzu yapılandırma bilgilerini Kiracı kataloğa eşitleyin.
> * Uygulama, sunucuları ve havuzları içeren bir kurtarma bölgesinde bir Ayna görüntüsünü ortamı ayarlayın.   
> * Coğrafi geri yükleme ile katalog ve Kiracı veritabanlarını kurtarın.
> * Coğrafi çoğaltma, kesinti giderildikten sonra değiştirilen Kiracı veritabanları ve Kiracı Kataloğu repatriate için kullanın.
> * Her bir veritabanına geri (repatriated ya da gibi) güncelleştirme Kataloğu da Etkin kopyanın her bir kiracının veritabanının geçerli konumunu izlemek için.
> * Uygulama ve Kiracı veritabanı her zaman gecikmesini azaltmak için aynı Azure bölgesinde birlikte bulunduğundan emin olun. 
 

Bu öğreticiye başlamadan önce aşağıdaki önkoşulları tamamlayın:
* Wingtip bilet SaaS veritabanı başına Kiracı uygulama dağıtın. Beş dakikadan kısa bir süre içinde dağıtmak için bkz. [Dağıt ve Kiracı uygulama başına Wingtip bilet SaaS veritabanı keşfedin](saas-dbpertenant-get-started-deploy.md). 
* Azure PowerShell'i yükleyin. Ayrıntılar için bkz [Azure PowerShell'i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Coğrafi geri yükleme kurtarma düzeniyle giriş

Olağanüstü Durum Kurtarma (DR), uyumluluk nedenleriyle veya iş sürekliliği için birçok uygulama için önemli bir konu olmasına. Süren hizmet kesintisi ise iyi hazırlanmış bir kurtarma planı iş kesintileri en aza indirebilirsiniz. Bir kurtarma planı üzerinde coğrafi geri yükleme tabanlı çeşitli hedeflere ulaşmak gerekir:
 * Seçilen kurtarma bölgesindeki tüm gerekli kapasite Kiracı veritabanlarını geri yüklemek kullanılabilir olduğundan emin olmak için mümkün olan en kısa sürede saklı tutarız.
 * Özgün havuz ve veritabanı yapılandırmasını yansıtan bir Ayna görüntüsünü kurtarma ortamı oluşturun. 
 * Özgün bölge tekrar çevrimiçi olursa geri yükleme işleminin ortasında uçuştaki iptaline izin verin.
 * Yeni Kiracı ekleme mümkün olan en kısa sürede yeniden için hızlı sağlama Kiracı etkinleştirin.
 * Kiracılar öncelik sırasına geri yüklemek için en iyi duruma getirilmiş.
 * Pratik olduğunda paralel adımları uygulayarak çevrimiçi kiracılar'ı olabildiğince çabuk almak için en iyi duruma getirilmiş.
 * Hata, yeniden başlatılabilir ve etkili dayanıklı olması.
 * Kesinti giderildikten sonra özgün bölgelerini veritabanlarına kiracılar için minimum etkiyle repatriate.  

> [!NOTE]
> Uygulamayı, uygulamanın dağıtıldığı bölge eşleştirilmiş bölgeye kurtarılır. Daha fazla bilgi için [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

Bu öğretici, bu sorunları gidermek üzere Azure SQL veritabanı ve Azure platformunun özelliklerini kullanır:

* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), tüm gerekli kapasite mümkün olan en kısa sürede ayırmak için. Azure Resource Manager şablonları, bir Ayna görüntüsünü özgün sunucuları ve kurtarma bölgedeki elastik havuzların sağlamak için kullanılır. Yeni kiracılar sağlama için ayrı bir sunucu ve havuzu da oluşturulur.
* [Elastik veritabanı istemci Kitaplığı](sql-database-elastic-database-client-library.md) (oluşturmak ve bir kiracı veritabanı Kataloğu korumak için EDCL),. Genişletilmiş Kataloğu düzenli aralıklarla Yenilenen havuz ve veritabanı yapılandırma bilgilerini içerir.
* [Parça yönetim kurtarma özellikleri](sql-database-elastic-database-recovery-manager.md) Katalog veritabanı konumu girişlerini kurtarma ve repatriation sırasında korumak için EDCL.  
* [Coğrafi geri yükleme](sql-database-disaster-recovery.md), otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden katalog ve Kiracı veritabanlarını kurtarmak için. 
* [Zaman uyumsuz geri yükleme işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), Kiracı öncelik sırasına gönderilen, sistem tarafından her havuzu için sıraya alındı ve havuz aşırı yüklü değilse toplu olarak işlenir. Bu işlemler, önce veya gerekirse yürütme sırasında iptal edilebilir.   
* [Coğrafi çoğaltma](sql-database-geo-replication-overview.md)kesinti sonra veritabanları özgün bölgeye repatriate için. Yapıldığında hiçbir veri kaybı ve Kiracı üzerinde en az etki coğrafi çoğaltma kullanın.
* [SQL sunucu DNS diğer adları](dns-alias-overview.md)konumuna bakılmaksızın etkin Kataloğu'na bağlanmak kataloğun eşitleme işlemini izin vermek için.  

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma betiklerini alma

Bu öğreticide kullanılan DR betikler kullanılabilir [GitHub deposundan Kiracı başına veritabanı Wingtip bilet SaaS](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımları indirin ve Wingtip bilet yönetim komut dosyaları engelini kaldırmak için.

> [!IMPORTANT]
> Tüm Wingtip bilet yönetim komut dosyaları gibi DR betikleri örnek kalitesi ve üretim ortamında kullanılmamalıdır.

## <a name="review-the-healthy-state-of-the-application"></a>Uygulamanın sistem durumunu gözden geçirin
Kurtarma işlemine başlamadan önce uygulama normal sağlıklı durumunu gözden geçirin.

1. Wingtip bilet olay hub'ı web tarayıcınızda açın (http://events.wingtip-dpt.&lt; kullanıcı&gt;. trafficmanager.net değiştirin &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).
    
   Sayfanın en altına gidin ve kataloğu sunucu adını ve konumunu altbilgisindeki dikkat edin. Uygulamanın dağıtıldığı bölge konumdur.    

   > [!TIP]
   > Fare görünen büyütmek için konumu üzerinde gelin.

   ![Olay hub'ı sağlıklı duruma özgün bölgede](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Contoso Konser Salonu kiracısı seçin ve kendi olay sayfasını açın.

   Alt bilgisinde kiracının sunucu adına dikkat edin. Konumun Kataloğu sunucunun konumu ile aynıdır.

   ![Contoso Konser Salonu özgün bölge](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. İçinde [Azure portalında](https://portal.azure.com)gözden geçirip uygulamayı dağıttığınız kaynak grubunu açın.

   Kaynaklar ve SQL veritabanı sunucularını ve uygulama hizmet bileşenleri dağıtılan bölge dikkat edin.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>Kataloğa Kiracı yapılandırmayı eşitleyemedi

Bu görevde, sunucuları, elastik havuzları ve veritabanlarını yapılandırmasını Kiracı kataloğa eşitlemek için bir işlem başlatın. Bu bilgiler daha sonra kurtarma bölgesinde bir Ayna görüntüsünü ortamını yapılandırmak için kullanılır.

> [!IMPORTANT]
> Kolaylık olması için yerel PowerShell işleri veya istemci kullanıcı oturum açma bilgilerinizi altında çalışan oturumları olarak bu örnekler, eşitleme işlemini ve diğer uzun süreli kurtarma ve repatriation işlemler uygulanır. Oturum açtığınızda verilen kimlik doğrulama belirteçlerinizi birkaç saat sonra süresi dolacak ve ardından işleri başarısız olur. Bir üretim senaryosunda, uzun süre çalışan işlemler güvenilir Azure Hizmetleri çalıştıran bir hizmet sorumlusu altında tür olarak uygulanmalıdır. Bkz: [bir sertifika ile hizmet sorumlusu oluşturmak için Azure PowerShell kullanarak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. PowerShell ISE'de ...\Learning Modules\UserConfig.psm1 dosyasını açın. Değiştirin `<resourcegroup>` ve `<user>` satırlarında 10 ve 11 uygulamasını dağıtırken kullandığınız değerine sahip. Dosyayı kaydedin.

2. PowerShell ISE'de betik Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 ...\Learning açın.

    Bu öğreticide, senaryoların her biri bu PowerShell Betiği çalıştırın, böylece bu dosya açık tutun.

3. Aşağıdakileri ayarlayın:

    $DemoScenario = 1: Kiracı sunucusuna ve havuzu yapılandırma bilgilerini kataloğa eşitlenen bir arka plan işi başlatın.

4. Eşitleme betiği çalıştırmak için F5'i seçin. 

    Bu bilgiler daha sonra kurtarma kurtarma bölgesinde bir Ayna görüntüsünü sunucuları, havuzları ve veritabanlarını oluşturduğundan emin olmak için kullanılır.  

    ![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Arka planda çalışan PowerShell penceresine bırakın ve bu öğreticinin geri kalanını ile devam edin.

> [!NOTE]
> Eşitleme işlemi katalog bir DNS diğer adı aracılığıyla bağlanır. Geri yükleme ve etkin Kataloğu'na işaret edecek şekilde repatriation sırasında diğer ad değiştirildi. Eşitleme işlemi Katalog Kurtarma bölgesinde veritabanı veya havuz yapılandırma değişiklikleri ile güncel tutar. Repatriation sırasında bu değişiklikleri özgün bölgede eşdeğer kaynaklara uygulanır.

## <a name="geo-restore-recovery-process-overview"></a>Coğrafi geri yükleme kurtarma işlemine genel bakış

Coğrafi geri yükleme kurtarma işlemi, bir uygulama dağıtır ve veritabanları kurtarma bölgeye yedeklerden geri yükler.

Kurtarma işlemi şunları yapar:

1. Azure Traffic Manager uç noktası web uygulamasının özgün bölgede devre dışı bırakır. Uç noktanın devre dışı kullanıcılar geçersiz bir durumda uygulama için özgün bölge Kurtarma sırasında çevrimiçi olması bağlanmasını engeller.

2. Hükümleri Kurtarma bir katalog veritabanı sunucusu kurtarma bölgede coğrafi geri yükleme işlemleri katalog ve geri yüklenen katalog sunucusuna işaret edecek şekilde activecatalog diğer güncelleştirir. Katalog diğer adının değiştirilmesi, kataloğu eşitleme işlemini her zaman etkin kataloğa eşitler sağlar.

3. Bunlar geri yüklenmeden önce Kiracı veritabanlarına erişimi engellemek için tüm mevcut kiracıda kurtarma Kataloğu çevrimdışı olarak işaretler.

4. Uygulama Kurtarma bölgesinde bir örneğini sağlar ve bu bölgede geri yüklenen Kataloğu kullanacak şekilde yapılandırır. En az gecikme süresini tutmak için örnek uygulamayı her zaman aynı bölgede Kiracı veritabanına bağlanmak için tasarlanmıştır.

5. Yeni kiracılar sağlanan sunucusu ve esnek bir havuz sağlar. Bu kaynakları oluşturma, yeni kiracılar sağlama var olan kiracılar kurtarılmasını müdahale etmediği sağlar.

6. Yeni Kiracı veritabanları kurtarma bölgesindeki sunucuya işaret edecek şekilde yeni Kiracı diğer güncelleştirir. Bu diğer adı değiştirme, tüm yeni kiracılara veritabanları kurtarma bölgesinde sağlanmasını önler.
        
7. Sunucuları ve Kiracı veritabanlarını geri yüklemek için kurtarma bölgedeki elastik havuzlar sağlar. Bu sunucular ve havuzları bir Ayna görüntüsünü özgün bölgede yapılandırmasının var. Önden havuzları sağlama tüm veritabanlarını geri yüklemek için gereken kapasite ayırır.

    Bir bölgede kesinti önemli baskısı eşlenen bölgesi içinde kullanılabilir kaynaklara yerleştirebilirsiniz. DR için coğrafi geri yükleme üzerinde güveniyorsanız, kaynaklar'ı hızlı bir şekilde ayırma önerilir. Coğrafi çoğaltma kritik ise bir uygulama belirli bir bölgede kurtarılmadan göz önünde bulundurun. 

8. Kurtarma bölgesindeki web uygulaması için Traffic Manager uç nokta sağlar. Bu uç noktayı etkinleştirme, yeni kiracılar sağlama uygulaması sağlar. Bu aşamada, var olan kiracılar hala çevrimdışı.

9. Toplu işleri öncelik sırasına göre veritabanlarını geri yüklemek için istek gönderir. 

    * Tüm havuzlar arasında veritabanlarını paralel olarak geri yüklenir, böylece toplu işler halinde düzenlenir.  

    * Geri yükleme isteği gönderildiğinde zaman uyumsuz olarak hızlı bir şekilde gönderildi ve her havuzda yürütme için sıraya alındı.

    * Geri yükleme istekleri tüm havuzlardaki paralel olarak işlenir, çünkü önemli kiracılar birçok havuzlar arasında dağıtmak daha iyidir. 

10. Ne zaman veritabanlarını geri belirlemek için SQL veritabanı hizmeti izler. Bir kiracı veritabanı geri yüklendikten sonra çevrimiçi katalogda işaretlenir ve rowversion toplam Kiracı veritabanı için kaydedilir. 

    * Katalogda çevrimiçi işaretlenmiş hemen sonra Kiracı veritabanlarını uygulama tarafından erişilebilir.

    * Rowversion değerler Kiracı veritabanında bir toplamı Kataloğu'nda depolanır. Bu toplam veritabanı bir kurtarma bölgesinde güncelleştirildiği belirlenemiyor repatriation işlem veren parmak izi görür.       

## <a name="run-the-recovery-script"></a>Kurtarma betiği çalıştırın

> [!IMPORTANT]
> Bu öğreticide, coğrafi olarak yedekli yedeklemelerden veritabanlarını geri yükler. Bu yedeklemeler 10 dakika içinde genellikle kullanılabilir, ancak bir saat sürebilir. Betiği, kullanılabilir oluncaya kadar duraklatılır.

İçinde uygulama dağıtılan ve kurtarma betiği çalıştırmak bir bölgede kesinti olduğunu düşünün:

1. PowerShell ISE'de ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betikte aşağıdaki değeri ayarlayın:

    $DemoScenario = 2: Uygulama, coğrafi olarak yedekli yedeklemelerden geri yükleyerek bir kurtarma bölgeye kurtarın.

2. Betiği çalıştırmak için F5'i seçin.  

    * Betik yeni bir PowerShell penceresi açar ve bir dizi paralel olarak çalışan PowerShell işler başlatır. Bu işleri kurtarma bölgeye sunucuları, havuzları ve veritabanlarını geri yükleyin.

    * Uygulamanın dağıtıldığı Azure bölgesi ile ilişkili eşleştirilmiş bölge kurtarma bölgedir. Daha fazla bilgi için [Azure eşleştirilmiş bölgeleri](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Kodu kurtarma işleri için keşfetmek için PowerShell betikleri ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\RecoveryJobs klasöründe gözden geçirin.

## <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirin
Uygulama, uygulama uç noktasını Traffic Manager'da devre dışı bırakıldığında kullanılamaz. Katalog geri ve tüm kiracılar çevrimdışı olarak işaretlenir. Uygulama uç noktasını kurtarma bölgesindeki sonra etkinleştirilir ve uygulama yeniden çevrimiçi olacak. Uygulama kullanılabilir olsa da, kiracıların kendi veritabanlarını geri yüklenene kadar olay hub'ında çevrimdışı görünür. Çevrimdışı Kiracı veritabanlarını işlemek için uygulamanızı tasarlamak önemlidir.

* Katalog veritabanı kurtarıldıktan sonra ancak kiracılar tekrar çevrimiçi hale gelebilmesi web tarayıcınızda Wingtip bilet olay hub'ı yenileyin.

  * Alt bilgisinde katalog sunucusu adı artık olduğuna dikkat edin - kurtarma sonek ve kurtarma bölgede bulunuyor.

  * Henüz geri yüklenmemiş kiracılar çevrimdışı olarak işaretlenir ve seçilemeyen dikkat edin.   
 
    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

  * Bir kiracının olayları sayfası doğrudan Kiracı çevrimdışı durumdayken açarsanız, sayfanın Kiracı çevrimdışı bir bildirim görüntüler. Contoso Konser Salonu çevrimdışı olduğunda, örneğin, açmaya http://events.wingtip-dpt.&lt; kullanıcı&gt;.trafficmanager.net/contosoconcerthall.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgesinde yeni bir kiracı sağlama
Hatta Kiracı veritabanlarını geri yüklenmeden önce kurtarma bölgesinde yeni kiracılar sağlayabilirsiniz. Yeni Kiracı veritabanları kurtarma bölgesinde sağlanan, kurtarılan veritabanları ile daha sonra repatriated.   

1. PowerShell ISE'de ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betikte aşağıdaki özelliği ayarlayın:

    $DemoScenario = 3: Kurtarma bölgesinde yeni bir kiracı sağlayın.

2. Betiği çalıştırmak için F5'i seçin.

3. Tamamlandığında sağlanırken, Hawthorn Hall olayları sayfası tarayıcıda açılır. 

    Hawthorn Hall veritabanı kurtarma bölgesinde bulunduğunu dikkat edin.

    ![Hawthorn kurtarma bölgede sağlanan Hall](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. Tarayıcıda, Hawthorn Hall dahil görmek için Wingtip bilet olay hub'ı sayfayı yenileyin. 

    Geri yüklemek diğer kiracılar için beklemenize gerek kalmadan Hawthorn Hall sağladıysanız, diğer kiracılardan çevrimdışı olabilir.

## <a name="review-the-recovered-state-of-the-application"></a>Uygulamasını kurtarılan durumunu gözden geçirin

Kurtarma işlemi tamamlandığında, uygulama ve tüm kiracılar kurtarma bölgesinde tam işlevlidir. 

1. Bir PowerShell konsol penceresi içinde görünen tüm kiracılar kurtarıldığı gösterdikten sonra olay hub'ı yenileyin. 

    Tüm kiracılar yeni Kiracı, Hawthorn Hall dahil olmak üzere, çevrimiçi olarak görünür.

    ![Olay hub'ı kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Contoso Konser Salonu üzerinde tıklayın ve kendi olayları sayfasını açın. 

    Alt bilgisinde, veritabanı kurtarma bölgesinde bulunan kurtarma sunucusunda bulunur dikkat edin.

    ![Kurtarma bölgesindeki contoso](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. İçinde [Azure portalında](https://portal.azure.com), kaynak grupları listesini açın.  

    Kurtarma sonekiyle dağıttığınız kaynak grubunu yanı sıra, Kurtarma kaynak grubunda dikkat edin. Kurtarma kaynak grubunun tüm kurtarma işlemi sırasında oluşturulan kaynakları kesinti sırasında oluşturulan yeni kaynakları içerir. 

4. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri dikkat edin:

   * Katalog ve tenants1 sunucularının kurtarma sürümler kurtarma sonekine sahip. Tüm bu sunucularda geri yüklenen katalog ve Kiracı veritabanlarını orijinal bölgede kullanılan adları vardır.

   * Tenants2-dpt -&lt;kullanıcı&gt;-kurtarma SQL server. Bu sunucu, kesinti sırasında yeni kiracılar sağlama için kullanılır.

   * App service adlı olayları-wingtip-dpt -&lt;recoveryregion&gt;-&lt;kullanıcı&gt;, etkinlikler uygulaması kurtarma örneği olduğu.

     ![Kurtarma bölgesindeki contoso kaynakları](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Tenants2 açın-dpt -&lt;kullanıcı&gt;-kurtarma SQL server. Veritabanı hawthornhall ve elastik havuz Pool1 içerdiğine dikkat edin. Hawthornhall veritabanı Pool1 elastik havuzdaki esnek bir veritabanı olarak yapılandırılır.

## <a name="change-the-tenant-data"></a>Kiracı verilerini değiştirme 
Bu görevde, geri yüklenen Kiracı veritabanlarından birini güncelleştirin. Repatriation işlem kopyalar özgün bölgeye değişmiş olan veritabanlarının geri. 

1. Tarayıcınızda, olaylar listesini bulmak için Contoso Konser Salonu, olayları kaydırın ve son olayın, ciddi Strauss dikkat edin.

2. PowerShell ISE'de ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betikte aşağıdaki değeri ayarlayın:

    $DemoScenario = 4: Kurtarma bölgesinde bir kiracıdan gelen bir olay silin.

3. Betiği çalıştırmak için F5'i seçin.

4. Contoso Konser Salonu olayları sayfayı yenileyin (http://events.wingtip-dpt.&lt; kullanıcı&gt;.trafficmanager.net/contosoconcerthall) ve ciddi Strauss olay eksik olduğuna dikkat edin.

Bu öğreticide kurtarma bölgesinde artık çalıştıran uygulama kurtardı. Kurtarma bölgesinde yeni bir kiracı hazırladıktan ve geri yüklenen kiracılar birinin veri değiştirilmiş.  

> [!NOTE]
> Diğer öğreticileri örnek, Kurtarma durumu uygulamayı çalıştırmak için tasarlanmamıştır. Diğer öğreticileri keşfedin istiyorsanız, önce uygulama repatriate emin olun.

## <a name="repatriation-process-overview"></a>Repatriation işlemine genel bakış

Kesinti çözümlendikten sonra uygulama ve onun özgün bölgeye veritabanlarında repatriation işlem döner.

![Coğrafi geri yükleme repatriation](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

İşlem:

1. Tüm devam eden geri yükleme etkinlikleri durdurur ve herhangi bir bekleyen veya uçuşan veritabanı geri yükleme isteğini iptal eder.

2. Kesinti beri değiştirilmedi özgün bölge Kiracı veritabanlarındaki yeniden etkinleştirir. Bu veritabanları, henüz kurtarılamaz ve kurtarılan ancak sonradan değişmemiş içerir. Yeniden etkinleştirilen veritabanları tam olarak son, kiracılar tarafından erişilir.

3. Bir Ayna görüntüsünü özgün bölgede yeni kiracının sunucusu ve elastik havuzu sağlar. Bu işlem tamamlandıktan sonra yeni Kiracı diğer bu sunucuya işaret edecek şekilde güncelleştirilir. Diğer güncelleştirme, yeni Kiracı ekleme kurtarma bölgesindeki yerine özgün bölgede oluşmasına neden olur.

3. Katalog özgün bölgeye kurtarma bölgeden taşımak için coğrafi çoğaltma kullanır.

4. Havuz yapılandırmasını özgün bölgede kesinti sırasında kurtarma bölgesinde yapılan değişiklikler ile tutarlı olacak şekilde güncelleştirir.

5. Gerekli sunucular ve kesinti sırasında oluşturulan tüm yeni veritabanlarını barındırmak için havuzları oluşturur.

6. Coğrafi çoğaltma geri repatriate Kiracı güncelleştirilip güncelleştirilmediğini veritabanlarını geri yükleme sonrası ve kesinti sırasında sağlanan tüm yeni Kiracı veritabanlarını kullanır. 

7. Geri yükleme işlemi sırasında kurtarma bölgesinde oluşturulan kaynakları temizler.

Repatriated gereken Kiracı veritabanlarının sayısını sınırlamak için 1-3 adımları en kısa sürede gerçekleştirilir.  

4. adım, yalnızca Katalog Kurtarma bölgesinde kesinti sırasında değiştirildiyse gerçekleştirilir. Kataloğa yeni Kiracı oluşturduysanız veya herhangi bir veritabanı veya havuz yapılandırma kurtarma bölgesinde değiştirilirse güncelleştirilir.

7. adım, kiracılar için en az neden olur ve veri kaybı olmamasına önemlidir. Bu hedefe ulaşmak için coğrafi çoğaltma işlemi kullanır.

Her veritabanı, coğrafi olarak çoğaltılmış önce özgün bölgede karşılık gelen veritabanı silinir. Veritabanı kurtarma bölgede coğrafi çoğaltmalı ikincil bir çoğaltmaya özgün bölgede oluşturma, ise. Çoğaltma tamamlandıktan sonra Kiracı veritabanına herhangi bir bağlantı kurtarma bölgesinde keser kataloğunda çevrimdışı olarak işaretlenmiş. Veritabanı sonra Yük devretme, işlemleri ikincil kadar hiçbir veriyi işlemek için bekleyen neden kaybolur. 

Yük devretmede, veritabanı rolleri alınır. İkincil bölgedeki özgün birincil okuma / yazma veritabanı haline gelir ve salt okunur bir ikincil veritabanı kurtarma bölgesindeki olur. Özgün bölgede veritabanı başvurmak için Kiracı giriş kataloğunda güncelleştirilir ve Kiracı çevrimiçi olarak işaretlenir. Bu noktada, veritabanının repatriation tamamlanmıştır. 

Bunlar otomatik olarak bağlantılar bozuk olduğunda yeniden emin olmak için yeniden deneme mantığı ile uygulamaların yeniden yazılması gerekir. Aracı yeniden bağlanmak için kataloğun kullandıkları, özgün bölgede repatriated veritabanına bağlanın. Kısa Kes fark genelde olsa da, çalışma saatleri dışında veritabanları repatriate seçebilirsiniz.

Bir veritabanı repatriated sonra ikincil veritabanı kurtarma bölgesindeki silinebilir. Veritabanını özgün bölgede daha sonra yeniden coğrafi geri yükleme için DR koruma kullanır.

Adım 8'de, Kurtarma bölgesinde kurtarma sunucuları ve havuzları da dahil olmak üzere kaynaklar silinir.

## <a name="run-the-repatriation-script"></a>Repatriation betiği çalıştırın
Şimdi kesinti çözümlendi ve repatriation betiğini çalıştırmak düşünün.

Öğreticiyi takip, değiştirilmemiş olduğundan betik hemen Fabrikam Caz kulübü ve özgün bölgede kızılcık Dojo yeniden etkinleştirir. Ardından, değiştirilmiş olduğundan yeni Kiracı Hawthorn Hall ve Contoso Konser Salonu repatriates. Betik ayrıca Hawthorn Hall sağlanırken güncelleştirildi Kataloğu repatriates.
  
1. PowerShell ISE'de Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betik ...\Learning Katalog eşitleme işlemi, PowerShell örneğinde hala çalıştığından emin olun. Gerekirse, ayarlayarak yeniden başlatın:

    $DemoScenario = 1: Kiracı sunucu, havuz ve veritabanı yapılandırma bilgilerini kataloğa eşitleme başlatın.

    Betiği çalıştırmak için F5'i seçin.

2.  Ardından repatriation işlemini başlatmak için aşağıdakileri ayarlayın:

    $DemoScenario = 5: Uygulama, kendi özgün bölgeye repatriate.

    Yeni bir PowerShell penceresi kurtarma betiği çalıştırmak için F5'i seçin. Repatriation birkaç dakika sürer ve PowerShell penceresinde izlenebilir.

3. Komut dosyası çalıştırılırken, olay hub'ı sayfayı yenileyin (http://events.wingtip-dpt.&lt; kullanıcı&gt;. trafficmanager.net).

    Tüm kiracılar çevrimiçi ve erişilebilir bu süreci boyunca olduğuna dikkat edin.

4. Fabrikam Caz kulübü açmak için seçin. Bu Kiracı değiştirirseniz yaramadı zaten sunucusudur altbilginin bildirimi özgün sunucuya geri döndürüldü.

5. Contoso Konser Salonu olayları sayfayı yenileyin veya açın. Alt bilgisinden başlangıçta veritabanı hala kurtarma sunucusunda, dikkat edin. 

6. Repatriation işlem tamamlandığında Contoso Konser Salonu olayları sayfası ve veritabanı artık özgün bölgenizde olduğuna dikkat edin yenileyin.

7. Olay hub'ı tekrar yenileyin ve Hawthorn Hall açın. Veritabanını özgün bölgede de bulunuyor dikkat edin. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Kurtarma bölgesi kaynakları repatriation sonra Temizleme
Repatriation tamamlandıktan sonra kurtarma bölgesinde kaynakları silmek güvenlidir. 

> [!IMPORTANT]
> En kısa sürede onlar için tüm faturalandırma işlemlerini durdurmak için şu kaynakları silin.

Geri yükleme işlemi bir kurtarma kaynak grubundaki tüm kurtarma kaynakları oluşturur. Temizleme işlemi, bu kaynak grubunu siler ve tüm kaynaklara başvurular Kataloğu'ndan kaldırır. 

1. PowerShell ISE'de ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betikte ayarlayın:
    
    $DemoScenario = 6: Kurtarma bölgesi artık kullanılmayan kaynakları silin.

2. Betiği çalıştırmak için F5'i seçin.

Komut dosyalarını temizlendikten sonra geri başlattığınız yerde uygulamasıdır. Bu noktada, betiği yeniden çalıştırın veya diğer öğreticileri deneyin.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Uygulama ve veritabanı birlikte bulunan olmasını sağlamak için uygulama tasarlama 
Uygulama, kiracının veritabanı ile aynı bölgede örneğinden her zaman bağlanmak için tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikme süresini azaltır. Bu iyileştirme, uygulama veritabanı etkileşimi uygulama için kullanıcı etkileşimi chattier olduğunu varsayar.  

Kiracı veritabanlarını kurtarma ve özgün bölgeler arasında bir süre boyunca repatriation yayılıyor olabilir. Her veritabanı için veritabanı Kiracı sunucu adı bir DNS araması yaparak bulunur bölge uygulama arar. SQL veritabanı'nda bir diğer ad sunucu adıdır. Diğer adlı sunucu adını, bölge adını içerir. Uygulama, veritabanı ile aynı bölgede değilse, veritabanı sunucusu ile aynı bölgede örneğine yeniden yönlendirir. Aynı bölgede veritabanı örneğine yönlendirerek uygulama ve veritabanı arasındaki gecikme süresini en aza indirir.  

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]
> 
> * Başka bir bölgede oluşturulacak bir Ayna görüntüsünü kurtarma ortamı sağlayan düzenli aralıklarla yenilenen yapılandırma bilgilerini saklamak için Kiracı Kataloğu kullanın.
> * Azure SQL veritabanı coğrafi geri yükleme ile kurtarma bölgeye kurtarın.
> * Kiracı kataloğa geri yüklenen Kiracı veritabanı konumları yansıtacak şekilde güncelleştirin. 
> * Yeniden yapılandırma olmadan Kiracı Kataloğu'na bağlanmak bir uygulama için bir DNS diğer adı kullanın.
> * Coğrafi çoğaltma, kesinti giderildikten sonra kurtarılan veritabanları kendi özgün bölgeye repatriate için kullanın.

Deneyin [coğrafi veri tabanı çoğaltmayı kullanarak çok kiracılı bir SaaS uygulaması için olağanüstü durum kurtarma](saas-dbpertenant-dr-geo-replication.md) coğrafi çoğaltma büyük ölçekli, çok kiracılı bir uygulama kurtarmak için gereken zamanı önemli ölçüde azaltmak için nasıl kullanılacağını öğrenmek için öğreticiye.

## <a name="additional-resources"></a>Ek kaynaklar

[Wingtip SaaS uygulamasına geliştirecek ek öğreticilerden](saas-dbpertenant-wingtip-app-overview.md#sql-database-wingtip-saas-tutorials)
