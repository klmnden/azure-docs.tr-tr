---
title: Azure SQL Database coğrafi olarak yedekli yedeklemeler kullanarak SaaS uygulamaları için olağanüstü durum kurtarma | Microsoft Docs
description: Bir çok kiracılı SaaS uygulaması bir kesinti durumunda kurtarmayı Azure SQL Database coğrafi olarak yedekli yedeklemeleri kullanmayı öğrenin
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: a677e6eb583e293f83df824804aa4cd6f8f5d778
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="recover-a-multi-tenant-saas-application-using-geo-restore-from-database-backups"></a>Coğrafi geri yükleme veritabanı yedeklerden kullanarak çok kiracılı SaaS uygulamasına Kurtar

Bu öğreticide, Kiracı başına veritabanı modeli kullanılarak uygulanan bir çok kiracılı SaaS uygulaması için bir tam olağanüstü durum kurtarma senaryosunda keşfedin. Kullandığınız [ _coğrafi geri yükleme_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-recovery-using-backups) katalog ve Kiracı veritabanları otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden bir alternatif kurtarma bölgeye kurtarmak için. Kesinti giderildikten sonra kullandığınız [ _coğrafi çoğaltma_ ](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview) özgün bölgelerini değiştirilen veritabanlarına repatriate için.

![coğrafi geri yükleme mimarisi](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Coğrafi geri yükleme SQL veritabanı için düşük maliyetli olağanüstü durum kurtarma çözümüdür.  Bununla birlikte, coğrafi olarak yedekli yedeklerden geri yükleme bir saat veri kaybına neden olabilir ve her veritabanı boyutuna bağlı olarak uzun bir süre alabilir. **En düşük olası RPO ve RTO uygulamalarla kurtarmak için coğrafi çoğaltma coğrafi geri yükleme yerine kullanın**.

Bu öğreticinin geri yükleme ve repatriation iş akışları araştırır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]

>* Kiracı kataloğuna veritabanı ve esnek havuzu yapılandırma bilgilerini eşitleme
>* Uygulama, sunucuları ve havuzları kapsayan bir yansıma ortamını bir 'Kurtarma' bölgede ayarlama    
>* Kullanarak katalog ve Kiracı veritabanlarını kurtarmak _coğrafi geri yükleme_
>* Değiştirilen Kiracı veritabanları kullanma ve Kiracı katalog repatriate _coğrafi çoğaltma_ kesinti çözümlendikten sonra
>* Her bir veritabanına geri (repatriated ya da gibi) güncelleştirme Kataloğu her bir kiracının veritabanının Etkin kopyanın geçerli konumunu izlemek için
>* Uygulama ve Kiracı veritabanı her zaman birlikte bulunur gecikme süresini azaltmak için aynı Azure bölgesinde emin olun  
 

Bu öğreticiye başlamadan önce aşağıdaki önkoşulların tamamlandığından emin olun:
* Wingtip biletleri SaaS veritabanı Kiracı uygulama başına dağıtılır. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfedin.](saas-dbpertenant-get-started-deploy.md)  
* Azure PowerShell’in yüklendiğinden. Ayrıntılar için bkz. [Azure PowerShell’i kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps)

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Coğrafi geri yükleme kurtarma düzeni giriş

Olağanüstü Durum Kurtarma (DR) önemli bir pek çok uygulama uyumluluk nedenleriyle veya iş sürekliliği için konudur. Uzun süren hizmet kesintisi olması, iyi hazırlıklı DR planı iş kesintiyi en aza indirebilirsiniz. Coğrafi geri yükleme üzerinde dayalı olarak bir kurtarma planı çeşitli hedeflere ulaşmak gerekir:
 * Kiracı veritabanlarını geri yüklemek kullanılabilir olduğundan emin olmak için mümkün olan en kısa sürede yedek seçilen kurtarma bölgedeki tüm gerekli kapasite.
 * Özgün havuz ve veritabanı yapılandırmasını yansıtan bir yansıma kurtarma ortamını oluşturma 
 * Özgün bölge tekrar çevrimiçi olursa Orta yürütülen geri yükleme işlemi iptal etmek mümkün olması gerekir.
 * Yeni Kiracı ekleme mümkün olan en kısa sürede yeniden için hızlı sağlama Kiracı etkinleştir  
 * Kiracılar öncelik sırasına geri yüklemek için en iyileştirilmiş
 * Çevrimiçi kiracılar mümkün olan en kısa sürede pratik burada paralel adımları yaparak almak için en iyileştirilmiş
 * Hata, yeniden başlatılabilir ve ıdempotent dayanıklı olmasını
 * Kesinti giderildikten sonra özgün bölgelerini veritabanlarına kiracılar için en az etkiyle repatriate.  

> [!Note]
> Uygulama içine kurtarılan _eşleştirilmiş bölge_ uygulamanın dağıtıldığı bölge. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions).   



Bu öğreticide, Azure SQL Database ve Azure platformu özelliklerini kullanarak bu zorluklar ele alınmıştır:

* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), tüm gerekli kapasite mümkün olan en kısa sürede ayırmak için. Azure Resource Manager şablonları bir yansıma kurtarma bölgede esnek havuzlar ve özgün sunucuları sağlamak için kullanılır. Yeni kiracılar sağlamak için ayrı bir sunucu ve havuzu da oluşturulur. 
* [Esnek veritabanı istemci Kitaplığı](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-elastic-database-client-library) oluşturmak ve bir kiracı veritabanı kataloğunu korumak için (EDCL).  Kataloğu düzenli aralıklarla yenilendiğinden havuzunu ve veritabanı yapılandırma bilgilerini içerecek şekilde genişletilir.
* [Parça yönetim kurtarma özellikleri](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) kurtarma ve repatriation sırasında veritabanı konum girdilerini kataloğunda korumak için EDCL biri.  
* [Coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden katalog ve Kiracı veritabanlarını kurtarmak için. 
* [Zaman uyumsuz geri yükleme işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) sistem tarafından her havuzu için sıraya ve havuzu aşırı toplu olarak işlenir Kiracı öncelik sırasına göre gönderilir. Bu işlemler önce veya gerekirse yürütme sırasında iptal edilebilir.    
* [Coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)kesinti sonra özgün bölge veritabanlarına repatriate için. Coğrafi çoğaltma'yı kullanarak, hiçbir veri kaybı ve Kiracı üzerinde en az etki sağlar.
* [SQL server DNS diğer adları](https://docs.microsoft.com/azure/sql-database/dns-alias-overview) konumuna bakılmaksızın etkin Kataloğu'na bağlanmak Katalog eşitleme işlemi izin vermek için.  

## <a name="get-the-disaster-recovery--scripts"></a>Olağanüstü durum kurtarma komut dosyalarını almak 

Bu öğreticide kullanılan DR komut kullanılabilir [Wingtip biletleri SaaS veritabanı GitHub deposunu Kiracı başına](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri yönetim komut dosyaları engellemesini kaldırmak.
> [!IMPORTANT]
> Tüm Wingtip biletleri yönetim komut dosyaları gibi DR betikleri örnek kalite ve üretimde kullanılmayacak.   

## <a name="review-the-healthy-state-of-the-application"></a>Uygulama sağlıklı durumunu gözden geçirin
Kurtarma işlemine başlamadan önce uygulamaya normal sağlıklı durumunu gözden geçirin.
1. Web tarayıcınızda Wingtip biletleri olay hub'ı açın (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net - Değiştir &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).
    * Sayfanın alt kısmına kaydırın ve Katalog sunucu adını ve konumunu altbilgisindeki dikkat edin.  Konum, uygulamayı dağıttığınız bölgedir.    
        İpucu: fare görüntü büyütmek için konum gelin.

    ![Olay hub'ı sağlam durumda özgün bölge](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

1. Contoso birlikte Hall Kiracı'tıklayın ve kendi olay sayfası açın.
    * Altbilgisinde kiracılar sunucu adı dikkat edin. Konum katalog sunucusunun konumu ile aynı olacaktır.

    ![Contoso birlikte Hall özgün bölge](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png)    
1. İçinde [Azure portal](https://portal.azure.com), gözden geçirin ve uygulama dağıttığınız kaynak grubunu açın.
    * Kaynakları ve SQL veritabanı sunucuları ve uygulama hizmet bileşenleri dağıtılan bölge dikkat edin.

## <a name="sync-tenant-configuration-into-catalog"></a>Eşitleme Kiracı yapılandırması kataloğunda

Bu görevde, sunucuların, esnek havuzlar ve veritabanlarını yapılandırma Kiracı kataloğunda eşitlemek için bir işlem başlatın.  Bu bilgiler daha sonra kurtarma bölgede bir yansıma ortamını yapılandırmak için kullanılır.

> [!IMPORTANT]
> Basitlik için eşitleme işlemini ve diğer uzun süren kurtarma ve repatriation işlemler bu örnekleri yerel Powershell işleri veya istemci kullanıcı oturum açma altında Çalıştır oturumları olarak uygulanır. Oturum açma birkaç saat sonra süresi dolacak ve işleri sonra başarısız olacak verilen kimlik doğrulama belirteçleri. Bir üretim senaryosunda, uzun süre çalışan işlemleri, bir hizmet sorumlusu altında çalışan bir tür, güvenilir Azure Hizmetleri olarak uygulanmalıdır. Bkz: [bir sertifika ile bir hizmet sorumlusu oluşturmak için kullanım Azure PowerShell](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. İçinde _PowerShell ISE_, ...\Learning Modules\UserConfig.psm1 dosyasını açın. Değiştir `<resourcegroup>` ve `<user>` satırlarında 10 ve 11 uygulama dağıtıldığında kullanılan değerine sahip.  Dosyayı kaydedin!

2. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasını açın.
    *  Bu öğretici sırasında her çalıştıracağınız bu PowerShell senaryolarda, betik böylece tutmak bu dosyayı açın.

3. Aşağıdakileri ayarlayın:
    * **$DemoScenario = 1**, Kiracı sunucu eşitlenen bir arka plan işi başlatmak ve yapılandırma bilgileri kataloğuna havuzu

3. Tuşuna **F5** eşitleme komut dosyasını çalıştırmak için. 
    *  Bu bilgiler daha sonra kurtarma kurtarma bölgede sunucuları, havuzları ve veritabanı yansıtma görüntüsü oluşturduğundan emin olmak için kullanılır.  
![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Arka planda çalışan PowerShell penceresini bırakın ve bu öğreticinin geri kalanını ile devam edin.

> [!Note]
> Eşitleme işlemi katalog bir DNS diğer adı üzerinden bağlanır. Diğer geri yükleme ve etkin kataloğa işaret edecek şekilde repatriation sırasında değiştirilir. Eşitleme işlemi katalog güncel kurtarma bölgede veritabanı veya havuzu yapılandırma değişiklikleri ile tutar.  Repatriation sırasında bu değişiklikleri özgün bölgede denk kaynaklarına uygulanır.

## <a name="geo-restore-recovery-process-overview"></a>Coğrafi geri yükleme kurtarma işlemine genel bakış

Coğrafi geri yükleme kurtarma işlemi uygulama dağıtır ve veritabanları kurtarma bölgeye yedeklerden geri yükler.

Kurtarma işlemi aşağıdakileri yapar:

1. Trafik Yöneticisi uç noktası özgün bölgede web uygulaması için devre dışı bırakır. Uç noktası devre dışı bırakma kullanıcılar geçersiz bir durumda uygulama için özgün bölge Kurtarma sırasında çevrimiçi olması bağlanmasını önler.

1. Katalog veritabanı bir kurtarma katalog sunucusu kurtarma bölgede coğrafi geri yüklemeler sağlar ve güncelleştirmeleri _activecatalog_ geri yüklenen katalog sunucusuna işaret etmek için diğer ad.  
    * Katalog diğer değiştirme Katalog eşitleme işlemi her zaman etkin kataloğa eşitlenir sağlar.

1. Bunlar geri önce Kiracı veritabanları için erişimi engellemek için tüm var olan kiracılar kurtarma kataloğunda çevrimdışı olarak işaretler.

1. Kurtarma bölgede uygulama örneğini sağlar ve bu bölgede geri yüklenen katalog kullanacak şekilde yapılandırır.
    * Minimum gecikme tutmak için böylece her zaman aynı bölgede Kiracı veritabanına bağlanan örnek uygulaması tasarlanmıştır.

1. Yeni kiracılar sağlanacak sunucu ve esnek bir havuz sağlar. Bu kaynakları oluşturmak, yeni kiracılar sağlama var olan kiracılar Kurtarma ile engellemez sağlar.

1. Yeni Kiracı veritabanları kurtarma bölgede sunucusunu işaret edecek şekilde Kiracı yeni diğer ad güncelleştirir. Bu diğer adı değiştirmek için yeni Kiracı veritabanları kurtarma bölgede sağlanır sağlar.
        
1. Sunucuları ve esnek havuzlar Kiracı veritabanlarını geri yüklemek için kurtarma bölgede sağlar. Bu sunucuları ve havuzları yansıma özgün bölgede yapılandırmasının var.  Havuzları önceden sağlama tüm veritabanlarını geri yüklemek için gereken kapasiteyi ayırır.
    * Bir bölgede bir kesinti eşleştirilmiş bölgede kullanılabilir kaynaklara önemli baskısı yerleştirebilirsiniz.  DR için coğrafi geri yükleme üzerinde güveniyorsanız kaynaklarını hızla ayırma önerilir. Uygulamanın belirli bir bölgede kurtarılması gereken kritik öneme sahipse coğrafi çoğaltma kullanmayı düşünün. 

1. Kurtarma bölgede Web uygulaması için trafik Yöneticisi uç noktası sağlar. Bu uç noktayı etkinleştirme, yeni kiracılar sağlamak uygulama sağlar. Bu aşamada, var olan kiracılar hala çevrimdışı.

1. Toplu isteklerinin öncelik sırasına veritabanlarını geri gönderir. 
    * Böylece tüm havuzlardaki veritabanları paralel olarak geri toplu düzenlenir.  
    * Geri yükleme istekleri gönderildiğinde zaman uyumsuz olarak hızlı bir şekilde gönderildi ve her havuzda yürütme için kuyruğa alındı.
    * Geri yükleme istekleri tüm havuzlardaki paralel olarak işlenir çünkü birçok havuzlardaki önemli kiracılar dağıtmak daha iyidir. 

1. Veritabanları zaman geri belirlemek için SQL veritabanı hizmetinin izler. Bir kiracı veritabanı geri yüklendikten sonra çevrimiçi katalog olarak işaretlenmiş ve Kiracı veritabanı için rowversion toplam kaydedilir. 
    * Katalogda çevrimiçi işaretlenmiş hemen sonra Kiracı veritabanları uygulama tarafından erişilebilir.
    * Kiracı veritabanında rowversion değerlerinin toplamı Kataloğu'nda depolanır. Bu toplam veritabanı kurtarma bölgede güncelleştirilmiş olup olmadığını belirlemek repatriation işlem sağlayan parmak izi gibi davranır.      

## <a name="run-the-recovery-script"></a>Kurtarma komut dosyasını çalıştır

> [!IMPORTANT]
> Bu öğretici veritabanları coğrafi olarak yedekli yedeklerden geri yükler. Bu yedeklemeler 10 dakika içinde genellikle kullanılabilir, ancak bunlar kullanılabilir olmadan önce bir saat sürebilir. Komut dosyası, kullanılabilir oluncaya kadar duraklatılır.  Bir kahve almak için zamanı!

Şimdi bölge uygulamanın dağıtıldığı ve kurtarma komut dosyasını çalıştırmak kesinti olduğunu düşünün:

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyası, aşağıdaki değerleri ayarlayın:
    * **$DemoScenario = 2**, coğrafi olarak yedekli yedeklerden geri yükleyerek kurtarma bölgesine uygulama Kurtar

1. Betiği çalıştırmak için **F5**'e basın.  
    * Komut dosyasını yeni bir PowerShell penceresi açar ve ardından paralel olarak çalışan PowerShell işleri kümesi başlatır.  Bu işleri kurtarma bölgesine sunucuları, havuzları ve veritabanlarını geri yükleyin. 
    * Kurtarma bölgedir _eşleştirilmiş bölge_ uygulama dağıtılan Azure bölgesiyle ilişkilendirilmiş. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/en-us/azure/best-practices-availability-paired-regions). 

1. PowerShell penceresinde kurtarma işleminin durumunu izleyin.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

>Kurtarma işleri için kod keşfetmek için ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\RecoveryJobs klasöründeki PowerShell komut dosyalarını gözden geçirin.

## <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirin
Uygulama uç noktasını Traffic Manager'da devre dışı olsa da, uygulama kullanılamıyor. Katalog geri ve tüm kiracılar çevrimdışı olarak işaretlenir.  Kurtarma bölge uygulama uç sonra etkin ve uygulamayı yeniden çevrimiçi. Uygulama kullanılabilir olsa da, kiracılar kendi veritabanlarını geri yüklenene kadar olay hub'ı çevrimdışı görünür. Çevrimdışı Kiracı veritabanlarını işlemek için uygulamanızı tasarlayın önemlidir.

1. Katalog veritabanı kurtarıldıktan sonra ancak kiracılar tekrar çevrimiçi önce web tarayıcınızda Wingtip biletleri olay hub'ı yenileyin.
    * Altbilgisinde katalog sunucusu adı şimdi olduğuna dikkat edin bir _-kurtarma_ sonek ve kurtarma bölgede yer alır.
    * Henüz geri yüklenmemiş kiracılar çevrimdışı olarak işaretlenir ve seçilemeyen dikkat edin.   
 
    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Bir kiracının olayları sayfası doğrudan Kiracı çevrimdışı durumdayken açarsanız, sayfa 'Çevrimdışı Kiracı' bir bildirim görüntüler. Contoso birlikte Hall çevrimdışıysa, örneğin, açmaya http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall ![kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgede yeni bir kiracı sağlama
Kiracı veritabanları bile geri önce yeni kiracılar kurtarma bölgede sağlayabilirsiniz. Yeni Kiracı veritabanları kurtarma bölgede sağlanan kurtarılan veritabanları ile daha sonra repatriated.   

1. İçinde *PowerShell ISE*, aşağıdaki özelliği ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betik ayarlayın:
    * **$DemoScenario = 3**, Kurtarma bölgede yeni bir kiracı sağlama

1. Betiği çalıştırmak için **F5**'e basın. 

1. Sağlama tamamlandıktan sonra Hawthorn Hall Etkinlikler sayfasını tarayıcıda açılır. 
    * Hawthorn Hall veritabanı kurtarma bölgede bulunduğunu dikkat edin.
    ![Hawthorn kurtarma bölgede sağlanan Hall](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

1. Tarayıcıda Hawthorn Hall dahil görmek için Wingtip biletleri olay hub'ı sayfayı yenileyin. 
    * Diğer kiracılar geri yüklemek beklemeden Hawthorn Hall sağlanan değilse, diğer kiracılar çevrimdışı olabilir.

## <a name="review-the-recovered-state-of-the-application"></a>Uygulamasını kurtarılan durumunu gözden geçirin

Kurtarma işlemi tamamlandığında, uygulama ve tüm kiracılar kurtarma bölgede tam işlevlidir. 

1. Tüm kiracılar kurtarılan PowerShell konsol penceresinde görünen gösterir sonra olay hub'ı yenileyin.  Kiracılar tüm yeni Kiracı Hawthorn Hall dahil olmak üzere çevrimiçi görünür.

    ![Olay hub'ı kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

1. Contoso birlikte Hall üzerinde tıklayın ve kendi olayları sayfası açın.
    * Veritabanı kurtarma bölgede bulunan kurtarma sunucusunda bulunan altbilgisinde dikkat edin.

    ![Kurtarma bölgede contoso](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

1. İçinde [Azure portal](https://portal.azure.com), kaynak gruplarının listesini açın.  
    * Kurtarma kaynak grubu ile dağıttığınız kaynak grubunu fark _-kurtarma_ soneki.  Kurtarma kaynak grubu, tüm kurtarma işlemi sırasında oluşturulan kaynakların yanı sıra kesinti sırasında oluşturulan yeni kaynaklar içeriyor.  

1. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri dikkat edin:
    * Katalog ve tenants1 sunucularının kurtarma sürümleri ile _-kurtarma_ soneki.  Tüm bu sunuculara geri yüklenen katalog ve Kiracı veritabanları özgün bölgede kullanılan adlara sahip.

    * _Tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Bu sunucu, yeni kiracılar sırasında kesinti sağlamak için kullanılır.
    *   Uygulama hizmeti adlı, _olayları-wingtip-dpt -&lt;recoveryregion&gt;-&lt;kullanıcı & gt_; olayları Uygulama Kurtarma örneğini olduğu. 

    ![Kurtarma bölgede contoso](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png)   
    
1. Açık _tenants2-dpt -&lt;kullanıcı&gt;-kurtarma_ SQL server.  Veritabanı içerdiği bildirimi _hawthornhall_ ve esnek havuz _Pool1_.  _Hawthornhall_ veritabanını bir esnek veritabanı olarak yapılandırılmış _Pool1_ esnek havuz.

## <a name="change-tenant-data"></a>Kiracı verilerini değiştir 
Bu görevde, geri yüklenen Kiracı veritabanlarından birini güncelleştirin. Repatriation işlem özgün bölgesine değiştirilmiş geri yüklenen veritabanları kopyalayacak. 

1. Tarayıcınızda, olaylar listesinde Contoso birlikte Hall için bulma aracılığıyla olaylarını'e gidin ve son olay Not _ciddi Strauss_.

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 betik aşağıdaki değeri ayarlayın:
    * **$DemoScenario = 4**, bir olay kurtarma bölgede Kiracı silin
1. Tuşuna **F5** betik yürütmek için.
1. Contoso birlikte Hall olayları sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall) ve ciddi Strauss, olay eksik olduğuna dikkat edin.

Bu noktada öğreticide kurtarma bölgede şu anda çalışıyor uygulama kurtardı.  Kurtarma bölgede yeni bir kiracı hazırladıktan ve veri geri yüklenen kiracılar birinin değiştirdi.  

> [!Note]
> Örnekteki diğer öğreticileri Kurtarma durumu uygulamada çalıştırmak için tasarlanmamıştır. Diğer öğreticiler keşfetmek uygulamanın ilk repatriate emin olun.

## <a name="repatriation-process-overview"></a>Repatriation işlemine genel bakış

Bir kesinti giderildikten sonra repatriation işlem uygulama ve veritabanlarını orijinal bölgeye döner.

![Coğrafi geri yükleme repatriation](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 


İşlem:

1. Devam eden geri yükleme etkinlik durdurur ve herhangi bir bekleyen veya yürütülen veritabanı geri yükleme isteğini iptal eder.

1. Kesinti değiştirilmedi özgün bölge Kiracı veritabanları yeniden etkinleştirir.  Bu, henüz kurtarılamaz veritabanları ve kurtarılan ancak daha sonra değiştirilmedi veritabanları içerir. Yeniden etkinleştirilen veritabanları tam olarak son, kiracılar tarafından erişilir.

1. Yeni kiracılar sunucu ve esnek havuzu özgün bölgede bulunan bir yansıtma görüntüsü sağlar. Bu eylem tamamlandıktan sonra yeni Kiracı diğer adı bu sunucuya işaret edecek şekilde güncelleştirilir. Diğer ad güncelleştirme yeni Kiracı ekleme kurtarma bölge yerine özgün bölgede oluşmasına neden olur.

1. Coğrafi çoğaltma'yı kullanarak, Katalog Kurtarma bölgesinden özgün bölgesine taşır.

1. Özgün bölgede havuzu yapılandırması sırasında kesinti kurtarma bölgede yapılan değişiklikler ile tutarlı olacak şekilde güncelleştirir.

1. Gerekli sunucular ve kesinti sırasında oluşturulan tüm yeni veritabanlarını barındırmak için havuzlarını oluşturur.

1. Coğrafi çoğaltma'yı kullanarak, repatriates güncelleştirilip güncelleştirilmediğini veritabanlarını geri yükleme sonrası Kiracı ve kesinti sırasında sağlanan tüm yeni Kiracı veritabanları geri. 

1. Geri yükleme işlemi sırasında kurtarma bölgede oluşturulan kaynakları siler.

Repatriated gereken Kiracı veritabanları sayısını sınırlamak için adım 1-3 derhal yapılır.  

4. adım, Kurtarma bölge kataloğunda sırasında kesinti değiştirilirse yalnızca yapılır. Katalog yeni kiracılar oluşturduysanız veya herhangi bir veritabanı veya havuzu yapılandırma kurtarma bölgede değiştirildiğinde güncelleştirilir.

Adım 7 kiracılar için en az kesintiye neden olur ve veri kaybı olmamasına önemlidir. Bu hedefe ulaşmak için işlemin kullandığı _coğrafi çoğaltma_.

Coğrafi olarak çoğaltılmış her veritabanı önce özgün bölgede karşılık gelen veritabanı silinir. Kurtarma bölgede sonra özgün bölgede bir ikincil çoğaltma oluşturma coğrafi çoğaltılmış, veritabanıdır. Kiracı, çoğaltma tamamlandıktan sonra veritabanına herhangi bir bağlantısı kurtarma bölgede kıran kataloğunda çevrimdışı olarak işaretlenir. Veritabanı sonra üzerinden, işlemleri veri kaybı olmamasına şekilde ikincil işlenmeyi bekleyen herhangi bir neden başarısız oldu. Yük devretme, veritabanı rolleri ters çevrilir.  Özgün bölgede ikincil birincil okuma-yazma veritabanı haline gelir ve kurtarma bölgede veritabanı salt okunur bir ikincil olur. Katalog Kiracı girişi özgün bölgede veritabanı başvuracak şekilde güncelleştirilmez ve Kiracı çevrimiçi olarak işaretlenir.  Bu noktada, veritabanının repatriation tamamlanır. 

Uygulamaları bağlantıları bozuk olduğunda bunlar otomatik olarak yeniden emin olmak için yeniden deneme mantığı ile yazılması gerekir.  Bağlantı Aracısı için Kataloğu'nu kullanarak yeniden bağlandığında, özgün bölgede repatriated veritabanı bağlayın. Kısa bağlantıyı kes fark genelde rağmen iş saatleri dışında veritabanları repatriate seçebilirsiniz. 

Bir veritabanı repatriated sonra ikincil veritabanı kurtarma bölgede silinebilir. Özgün bölgede veritabanı sonra coğrafi geri yükleme için DR koruma yeniden kullanır.

8. adımda kaynak havuzları ve kurtarma sunucuları dahil olmak üzere Kurtarma bölgede silinir.

## <a name="run-the-repatriation-script"></a>Repatriation komut dosyasını çalıştır
Şimdi şimdi kesinti çözümlendi ve repatriation komut dosyasını çalıştırmak düşünün.

Öğretici izlediyseniz değişmeden olduklarından betik hemen Fabrikam Jazz kulübü ve özgün bölgede kızılcık Dojo yeniden etkinleştirir. Ardından, değiştirildiği için yeni Kiracı Hawthorn Hall ve Contoso birlikte Hall repatriates. Komut ayrıca Hawthorn Hall hazırlandığında güncelleştirildi katalog repatriates.
  
1. İçinde *PowerShell ISE*...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyası.

1. Katalog eşitleme işlemi, PowerShell örneğinde hala çalıştığından emin olun.  Gerekirse, ayarlayarak yeniden başlatın:
    * **$DemoScenario = 1**, Kiracı sunucu, havuzu ve veritabanı yapılandırma bilgilerini kataloğuna Eşitlemeyi Başlat
    * Betiği çalıştırmak için **F5**'e basın.
1.  Ardından repatriation işlemini başlatmak üzere ayarlayın:
    * **$DemoScenario = 5**, uygulamanın kendi özgün bölgeye repatriate
    * Tuşuna **F5** yeni bir PowerShell penceresi kurtarma komut dosyasını çalıştırmak için.  Repatriation birkaç dakika sürer ve PowerShell penceresinde izlenebilir.
1. Komut dosyası çalışırken, olay hub'ı sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net)
    * Tüm kiracılar çevrimiçi ve erişilebilir bu işlemi boyunca olduğuna dikkat edin.
1. Açmak için Fabrikam Jazz kulübü üzerinde'ı tıklatın. Bu Kiracı değiştirmezseniz altbilginin sunucu zaten özgün sunucuya geri dikkat edin.
1. Açın veya Contoso birlikte Hall olayları sayfayı yenileyin ve altbilginin veritabanı hala olduğunu fark _-kurtarma_ sunucu başlangıçta.  
1. Repatriation işlem tamamlandığında Contoso birlikte Hall olayları sayfayı yenileyin ve veritabanı artık özgün bölgenizde olduğuna dikkat edin.
1. Olay hub'ı tekrar yenileyin ve Hawthorn Hall açın ve veritabanını da özgün bölgede bulunan dikkat edin. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Repatriation sonra kurtarma bölge kaynakları temizlemek
Repatriation tamamlandıktan sonra kurtarma bölgedeki kaynakları silmek güvenlidir. 

> [!IMPORTANT]
> Bunlar için tüm faturalama hemen durdurmak için bu kaynakları silin.

Geri yükleme işlemi bir kurtarma kaynak grubunda tüm kurtarma kaynaklar oluşturur. Temizleme işlemi, bu kaynak grubunu silmek ve Kataloğu'ndan kaynaklarına yönelik tüm başvuruları kaldırın. 

1. İçinde *PowerShell ISE*, ...\Learning Modules\Business sürekliliği ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyası, ayarla:
    * **$DemoScenario = 6**, artık kullanılmayan kaynaklar kurtarma bölgesinden Sil

1. Betiği çalıştırmak için **F5**'e basın.

Komut dosyalarını temizleme işleminden sonra geri başlattığınız yerde uygulamasıdır.  Komut dosyasını yeniden çalıştırın veya diğer öğreticileri bu noktada deneyin.

## <a name="designing-the-application-to-ensure-app-and-database-are-colocated"></a>Uygulama ve veritabanı emin olmak için uygulama tasarlama birlikte 
Uygulama, her zaman Kiracı veritabanı ile aynı bölgede örneğinden bağlamasına şekilde tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikme süresini azaltır. Bu iyileştirme uygulama veritabanı etkileşimini kullanıcı uygulama etkileşim chattier olduğunu varsayar.  

Kiracı veritabanları kurtarma ve özgün bölgeler arasında repatriation sırasında süredir yayılır.  Her veritabanı için veritabanı Kiracı sunucu adı bir DNS araması yaparak bulunur bölge uygulama arar. SQL veritabanı'nda bir diğer ad sunucu adıdır. Diğer sunucu adı, bölge adını içerir. Uygulama, veritabanı ile aynı bölgede değilse, veritabanı sunucusu ile aynı bölgede örneğine yönlendirir.  Veritabanı ile aynı bölgede örneğine yönlendirerek uygulama ve veritabanı arasındaki gecikme süresi en aza indirir.  

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]

>* Bir yansıma Kurtarma Ortamı'başka bir bölgede oluşturulacak düzenli aralıklarla yenilendiğinden yapılandırma bilgilerini tutmak için Kiracı Kataloğu'nu kullanma
>* Coğrafi geri yükleme kurtarma bölgeye Azure SQL veritabanlarını kurtarmak için kullanın
>* Geri yüklenen Kiracı veritabanı konumlarını gösterecek şekilde Kiracı katalog güncelleştirmesi  
>* Yeniden yapılandırma olmadan Kiracı kataloğun bağlanmak bir uygulama etkinleştirmek için bir DNS diğer adı kullanın
>* Bir kesinti çözümlendikten sonra özgün bölgelerini kurtarılan veritabanlarını repatriate için coğrafi çoğaltma kullanın

Şimdi deneyin [database coğrafi çoğaltma kullanarak çok kiracılı SaaS uygulaması için olağanüstü durum kurtarma](saas-dbpertenant-dr-geo-replication.md) coğrafi çoğaltma kullanarak büyük ölçekli bir çok kiracılı uygulama kurtarmak için gereken zamanı önemli ölçüde azaltmak nasıl öğrenin.

## <a name="additional-resources"></a>Ek kaynaklar

* [Wingtip SaaS uygulamasına yapı ek öğreticileri](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
