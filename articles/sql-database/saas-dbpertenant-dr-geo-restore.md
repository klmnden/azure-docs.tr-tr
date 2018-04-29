---
title: SaaS uygulamaları olağanüstü durum kurtarma için Azure SQL Database coğrafi olarak yedekli yedekleri kullanın | Microsoft Docs
description: Çok kiracılı bir SaaS uygulaması bir kesinti durumunda kurtarmayı Azure SQL Database coğrafi olarak yedekli yedeklemeleri kullanmayı öğrenin
keywords: sql veritabanı öğreticisi
services: sql-database
author: stevestein
manager: craigg
ms.service: sql-database
ms.custom: saas apps
ms.topic: article
ms.date: 04/16/2018
ms.author: ayolubek
ms.openlocfilehash: 8fd25e13f6796b8be99ad3efd425bcde7bca3905
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="use-geo-restore-to-recover-a-multitenant-saas-application-from-database-backups"></a>Çok kiracılı bir SaaS uygulaması veritabanı yedeklemeleri kurtarmak için coğrafi geri yükleme kullanın

Bu öğretici, her Kiracı model veritabanı ile uygulanan çok kiracılı bir SaaS uygulaması için bir tam olağanüstü durum kurtarma senaryosunda araştırır. Kullandığınız [coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-recovery-using-backups) katalog ve Kiracı veritabanları otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden bir alternatif kurtarma bölgeye kurtarmak için. Kesinti giderildikten sonra kullandığınız [coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview) özgün bölgelerini değiştirilen veritabanlarına repatriate için.

![coğrafi geri yükleme mimarisi](media/saas-dbpertenant-dr-geo-restore/geo-restore-architecture.png)

Coğrafi geri yükleme Azure SQL veritabanı için düşük maliyetli olağanüstü durum kurtarma çözümüdür. Bununla birlikte, coğrafi olarak yedekli yedeklerden geri yükleme, bir saat veri kaybına neden olabilir. Her veritabanı boyutuna bağlı olarak uzun zaman alabilir. 

> [!NOTE]
> En düşük olası RPO ve RTO uygulamalarla coğrafi geri yükleme yerine coğrafi çoğaltma kullanarak kurtarın.

Bu öğreticinin geri yükleme ve repatriation iş akışları araştırır. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:
> [!div class="checklist"]

>* Veritabanı ve esnek havuzu yapılandırma bilgilerini Kiracı kataloğuna eşitleyin.
>* Uygulama, sunucuları ve havuzları içeren bir kurtarma bölgede bir yansıma ortamı ayarlayın.   
>* Katalog ve Kiracı veritabanları coğrafi geri yükleme kullanarak kurtarın.
>* Değiştirilen Kiracı veritabanları ve Kiracı katalog kesinti çözümlendikten sonra repatriate için coğrafi çoğaltma kullanın.
>* Her bir veritabanına geri (repatriated ya da gibi) güncelleştirme Kataloğu her bir kiracının veritabanının Etkin kopyanın geçerli konumunu izlemek için.
>* Uygulama ve Kiracı veritabanı her zaman gecikmesini azaltmak için aynı Azure bölgesinde birlikte bulunduğundan emin olun. 
 

Bu öğretici başlamadan önce aşağıdaki önkoşulları tamamlayın:
* Wingtip biletleri SaaS veritabanı Kiracı uygulama başına dağıtın. Beş dakikadan daha kısa bir süre içinde dağıtmak için bkz: [dağıtma ve Kiracı uygulama başına Wingtip biletleri SaaS veritabanı keşfetme](saas-dbpertenant-get-started-deploy.md). 
* Azure PowerShell'i yükleyin. Ayrıntılar için bkz [Azure PowerShell ile çalışmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-the-geo-restore-recovery-pattern"></a>Coğrafi geri yükleme kurtarma düzeni giriş

Olağanüstü Durum Kurtarma (DR) önemli bir pek çok uygulama uyumluluk nedenleriyle veya iş sürekliliği için konudur. Uzun süren hizmet kesintisi ise, iyi hazırlıklı DR planı iş kesintiyi en aza indirebilirsiniz. Coğrafi geri yükleme üzerinde dayalı olarak bir kurtarma planı çeşitli hedeflere ulaşmak gerekir:
 * Yedek seçilen kurtarma bölgedeki tüm gerekli kapasite Kiracı veritabanlarını geri yüklemek kullanılabilir olduğundan emin olmak için mümkün olan en kısa sürede.
 * Özgün havuz ve veritabanı yapılandırmasını yansıtan bir yansıma kurtarma ortamı oluşturun. 
 * Özgün bölge tekrar çevrimiçi olursa Orta yürütülen geri yükleme işlemi iptal edilmesine izin verin.
 * Yeni Kiracı ekleme mümkün olan en kısa sürede yeniden için hızlı sağlama Kiracı etkinleştirin.
 * Kiracılar öncelik sırasına geri yüklemek için en iyi duruma getirilmiş.
 * Pratik burada paralel adımları yaparak çevrimiçi kiracılar hemen almak için en iyi duruma getirilmiş.
 * Hata, yeniden başlatılabilir ve ıdempotent dayanıklı olması.
 * Kesinti giderildikten sonra özgün bölgelerini veritabanlarına kiracılar için en az etkiyle repatriate.  

> [!NOTE]
> Uygulama, uygulamanın dağıtıldığı bölge eşleştirilmiş bölgeye kurtarılır. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).   

Bu öğretici Azure SQL Database ve Azure platformu özelliklerini bu güçlükleri kullanır:

* [Azure Resource Manager şablonları](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template), tüm gerekli kapasite mümkün olan en kısa sürede ayırmak için. Azure Resource Manager şablonları bir yansıma kurtarma bölgede esnek havuzlar ve özgün sunucuları sağlamak için kullanılır. Yeni kiracılar sağlamak için ayrı bir sunucu ve havuzu da oluşturulur.
* [Esnek veritabanı istemci Kitaplığı](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-client-library) (oluşturmak ve bir kiracı veritabanı kataloğunu korumak için EDCL),. Genişletilmiş Kataloğu düzenli aralıklarla yenilendiğinden havuzunu ve veritabanı yapılandırma bilgilerini içerir.
* [Parça yönetim kurtarma özellikleri](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-database-recovery-manager) kurtarma ve repatriation sırasında veritabanı konum girdilerini kataloğunda korumak için EDCL biri.  
* [Coğrafi geri yükleme](https://docs.microsoft.com/azure/sql-database/sql-database-disaster-recovery), otomatik olarak tutulan coğrafi olarak yedekli yedeklemelerden katalog ve Kiracı veritabanlarını kurtarmak için. 
* [Zaman uyumsuz geri yükleme işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations), Kiracı öncelik sırasına göre gönderilir, sistem tarafından her havuzu için sıraya ve havuzu aşırı yüklü değilse toplu olarak işlenir. Bu işlemler önce veya gerekirse yürütme sırasında iptal edilebilir.   
* [Coğrafi çoğaltma](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview)kesinti sonra özgün bölge veritabanlarına repatriate için. Hiçbir veri kaybı ve Kiracı üzerinde en az etki coğrafi çoğaltma kullandığınız zaman değildir.
* [SQL server DNS diğer adları](https://docs.microsoft.com/azure/sql-database/dns-alias-overview)konumuna bakılmaksızın etkin Kataloğu'na bağlanmak Katalog eşitleme işlemi izin vermek için.  

## <a name="get-the-disaster-recovery-scripts"></a>Olağanüstü durum kurtarma komut dosyalarını almak

Bu öğreticide kullanılan DR komut kullanılabilir [Wingtip biletleri SaaS veritabanı GitHub deposunu Kiracı başına](https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant). Kullanıma [genel rehberlik](saas-tenancy-wingtip-app-guidance-tips.md) adımların indirin ve Wingtip biletleri yönetim komut dosyaları engellemesini kaldırmak.

> [!IMPORTANT]
> Tüm Wingtip biletleri yönetim komut dosyaları gibi DR betikleri örnek kalite ve üretimde kullanılmayacak.

## <a name="review-the-healthy-state-of-the-application"></a>Uygulama sağlıklı durumunu gözden geçirin
Kurtarma işlemine başlamadan önce uygulamaya normal sağlıklı durumunu gözden geçirin.

1. Web tarayıcınızda Wingtip biletleri olay hub'ı açın (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net, yerine &lt;kullanıcı&gt; dağıtımınızın kullanıcı değeri ile).
    
   Sayfanın alt kısmına kaydırın ve Katalog sunucu adını ve konumunu altbilgisindeki dikkat edin. Konum, uygulamayı dağıttığınız bölgedir.    

   > [!TIP]
   > Fare görüntü büyütmek için konum gelin.

   ![Olay hub'ı sağlam durumda özgün bölge](media/saas-dbpertenant-dr-geo-restore/events-hub-original-region.png)

2. Contoso birlikte Hall Kiracı seçin ve kendi olay sayfası açın.

   Altbilgisinde kiracının sunucu adı dikkat edin. Konum katalog sunucusunun konumu ile aynı değil.

   ![Contoso birlikte Hall özgün bölge](media/saas-dbpertenant-dr-geo-restore/contoso-original-location.png) 

3. İçinde [Azure portal](https://portal.azure.com), gözden geçirin ve uygulama dağıttığınız kaynak grubunu açın.

   Kaynakları ve SQL veritabanı sunucuları ve uygulama hizmet bileşenleri dağıtılan bölge dikkat edin.

## <a name="sync-the-tenant-configuration-into-the-catalog"></a>Eşitleme kataloğunda Kiracı yapılandırma

Bu görevde, sunucuların, esnek havuzlar ve veritabanlarını yapılandırma Kiracı kataloğunda eşitlemek için bir işlem başlatın. Bu bilgiler daha sonra kurtarma bölgede bir yansıma ortamını yapılandırmak için kullanılır.

> [!IMPORTANT]
> Basitlik için eşitleme işlemini ve diğer uzun süre çalışan kurtarma ve repatriation işlemler bu örnekleri yerel PowerShell işleri veya istemci kullanıcı oturum açma altında Çalıştır oturumları olarak uygulanır. Oturum açtığınızda verilen kimlik doğrulama belirteçleri birkaç saat sonra sona ve işleri sonra başarısız olacak. Bir üretim senaryosunda, uzun süre çalışan işlemleri, bir hizmet sorumlusu altında çalışan bir tür, güvenilir Azure Hizmetleri olarak uygulanmalıdır. Bkz: [bir sertifika ile bir hizmet sorumlusu oluşturmak için kullanım Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-authenticate-service-principal). 

1. PowerShell ISE ...\Learning Modules\UserConfig.psm1 dosyasını açın. Değiştir `<resourcegroup>` ve `<user>` satırlarında 10 ve 11 uygulama dağıtıldığında kullanılan değerine sahip. Dosyayı kaydedin.

2. PowerShell ISE ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasını açın.

    Bu öğreticide senaryoların her biri bu PowerShell komut dosyasını çalıştırın, bu nedenle bu dosyayı açık tutun.

3. Aşağıdakileri ayarlayın:

    $DemoScenario = 1: Kiracı sunucu ve havuzu yapılandırma bilgilerini kataloğuna eşitlenen bir arka plan işi başlatmak.

4. Eşitleme komut dosyasını çalıştırmak için F5'i seçin. 

    Bu bilgiler daha sonra kurtarma kurtarma bölgede sunucuları, havuzları ve veritabanı yansıtma görüntüsü oluşturduğundan emin olmak için kullanılır.  

    ![Eşitleme işlemi](media/saas-dbpertenant-dr-geo-restore/sync-process.png)

Arka planda çalışan PowerShell penceresini bırakın ve bu öğreticinin geri kalanını ile devam edin.

> [!NOTE]
> Eşitleme işlemi katalog bir DNS diğer adı üzerinden bağlanır. Diğer geri yükleme ve etkin kataloğa işaret edecek şekilde repatriation sırasında değiştirilir. Eşitleme işlemi Katalog Kurtarma bölgede veritabanı veya havuzu yapılandırma değişiklikleri ile güncel tutar. Repatriation sırasında bu değişiklikleri özgün bölgede denk kaynaklarına uygulanır.

## <a name="geo-restore-recovery-process-overview"></a>Coğrafi geri yükleme kurtarma işlemine genel bakış

Coğrafi geri yükleme kurtarma işlemi uygulama dağıtır ve veritabanları kurtarma bölgeye yedeklerden geri yükler.

Kurtarma işlemi aşağıdakileri yapar:

1. Özgün bölgede web uygulaması için Azure trafik Yöneticisi uç noktası devre dışı bırakır. Uç noktası devre dışı bırakma kullanıcılar geçersiz bir durumda uygulama için özgün bölge Kurtarma sırasında çevrimiçi olması bağlanmasını önler.

2. Hükümler Kurtarma bir katalog veritabanı kurtarma bölgede coğrafi geri yüklemeler sunucu katalog ve geri yüklenen katalog sunucuya işaret edecek şekilde activecatalog diğer güncelleştirir. Katalog diğer değiştirme Katalog eşitleme işlemi her zaman etkin kataloğa eşitlenir sağlar.

3. Bunlar geri önce Kiracı veritabanları için erişimi engellemek için tüm var olan kiracılar kurtarma kataloğunda çevrimdışı olarak işaretler.

4. Kurtarma bölgede uygulama örneğini sağlar ve bu bölgede geri yüklenen katalog kullanacak şekilde yapılandırır. Minimum gecikme tutmak için örnek uygulamayı her zaman aynı bölgede Kiracı veritabanına bağlanmak için tasarlanmıştır.

5. Yeni kiracılar sağlanan sunucusu ve esnek bir havuz sağlar. Bu kaynakları oluşturmak, yeni kiracılar sağlama var olan kiracılar Kurtarma ile engellemez sağlar.

6. Yeni Kiracı veritabanları kurtarma bölgede sunucusunu işaret edecek şekilde yeni Kiracı diğer güncelleştirir. Bu diğer adı değiştirmek için yeni Kiracı veritabanları kurtarma bölgede sağlanır sağlar.
        
7. Sunucuları ve esnek havuzlar Kiracı veritabanlarını geri yüklemek için kurtarma bölgede sağlar. Bu sunucuları ve havuzları yansıma özgün bölgede yapılandırmasının var. Önden havuzları sağlama tüm veritabanlarını geri yüklemek için gereken kapasiteyi ayırır.

    Bir bölgede bir kesinti önemli baskısı eşleştirilmiş bölgedeki kaynakları yerleştirin. DR için coğrafi geri yükleme üzerinde güveniyorsanız kaynaklarını hızla ayırma önerilir. Coğrafi çoğaltma kritik öneme sahipse uygulamanın belirli bir bölgede kurtarılır göz önünde bulundurun. 

8. Kurtarma bölgede web uygulaması için trafik Yöneticisi uç noktası sağlar. Bu uç noktayı etkinleştirme, yeni kiracılar sağlamak uygulama sağlar. Bu aşamada, var olan kiracılar hala çevrimdışı.

9. Toplu isteklerinin öncelik sırasına veritabanlarını geri gönderir. 

    * Böylece tüm havuzlardaki veritabanları paralel olarak geri toplu düzenlenir.  

    * Geri yükleme istekleri gönderildiğinde zaman uyumsuz olarak hızlı bir şekilde gönderildi ve her havuzda yürütme için kuyruğa alındı.

    * Geri yükleme istekleri tüm havuzlardaki paralel olarak işlenir çünkü birçok havuzlardaki önemli kiracılar dağıtmak daha iyidir. 

10. Veritabanları zaman geri belirlemek için SQL veritabanı hizmetinin izler. Bir kiracı veritabanı geri yüklendikten sonra çevrimiçi katalog olarak işaretlenmiş ve Kiracı veritabanı için rowversion toplam kaydedilir. 

    * Katalogda çevrimiçi işaretlenmiş hemen sonra Kiracı veritabanları uygulama tarafından erişilebilir.

    * Kiracı veritabanında rowversion değerlerinin toplamı Kataloğu'nda depolanır. Bu toplam veritabanı bir kurtarma bölgede güncelleştirildi belirlemek repatriation işlem sağlayan parmak izi gibi davranır.       

## <a name="run-the-recovery-script"></a>Kurtarma komut dosyasını çalıştır

> [!IMPORTANT]
> Bu öğretici veritabanları coğrafi olarak yedekli yedeklerden geri yükler. Bu yedeklemeler 10 dakika içinde genellikle kullanılabilir, ancak bir saat sürebilir. Komut dosyası, kullanılabilir oluncaya kadar duraklar.

Bölge, uygulamanın dağıtıldığı ve kurtarma komut dosyasını çalıştırmak kesinti olduğunu düşünün:

1. PowerShell ISE ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında, aşağıdaki değeri ayarlayın:

    $DemoScenario = 2: coğrafi olarak yedekli yedeklerden geri yükleyerek kurtarma bölgesine uygulama kurtarma.

2. Komut dosyasını çalıştırmak için F5'i seçin.  

    * Komut dosyasını yeni bir PowerShell penceresi açar ve ardından paralel olarak çalışan PowerShell işleri kümesi başlatır. Bu işleri kurtarma bölgesine sunucuları, havuzları ve veritabanlarını geri yükleyin.

    * Kurtarma, uygulamayı dağıttığınız Azure bölgesiyle ilişkili eşleştirilmiş bölge bölgedir. Daha fazla bilgi için bkz: [Azure bölgeleri eşleştirilmiş](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). 

3. PowerShell penceresinde kurtarma işleminin durumunu izleyin.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress.png)

> [!NOTE]
> Kurtarma işleri için kod keşfetmek için ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\RecoveryJobs klasöründeki PowerShell komut dosyalarını gözden geçirin.

## <a name="review-the-application-state-during-recovery"></a>Kurtarma sırasında uygulama durumunu gözden geçirin
Uygulama uç noktasını Traffic Manager'da devre dışı olsa da, uygulama kullanılamıyor. Katalog geri ve tüm kiracılar çevrimdışı olarak işaretlenir. Kurtarma bölge uygulama uç sonra etkinleştirilir ve tekrar çevrimiçi bir uygulamadır. Uygulama kullanılabilir olsa da, kiracılar kendi veritabanlarını geri yüklenene kadar olay hub'ı çevrimdışı görünür. Çevrimdışı Kiracı veritabanlarını işlemek için uygulamanızı tasarlayın önemlidir.

* Katalog veritabanı kurtarıldıktan sonra ancak kiracılar tekrar çevrimiçi önce web tarayıcınızda Wingtip biletleri olay hub'ı yenileyin.

    * Altbilgisinde katalog sunucusu adı şimdi olduğuna dikkat edin - kurtarma sonek ve kurtarma bölgede yer alır.

    * Henüz geri yüklenmemiş kiracılar çevrimdışı olarak işaretlenir ve seçilemeyen dikkat edin.   
 
    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/events-hub-tenants-offline-in-recovery-region.png)    

    * Bir kiracının olayları sayfası doğrudan Kiracı çevrimdışı durumdayken açarsanız, sayfa bir kiracı çevrimdışı bildirim görüntüler. Contoso birlikte Hall çevrimdışıysa, örneğin, açmaya http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall.

    ![Kurtarma işlemi](media/saas-dbpertenant-dr-geo-restore/dr-in-progress-offline-contosoconcerthall.png)

## <a name="provision-a-new-tenant-in-the-recovery-region"></a>Kurtarma bölgede yeni bir kiracı sağlama
Kiracı veritabanları bile geri önce yeni kiracılar kurtarma bölgede sağlayabilirsiniz. Yeni Kiracı veritabanları kurtarma bölgede sağlanan kurtarılan veritabanları ile daha sonra repatriated.   

1. PowerShell ISE ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında, aşağıdaki özelliği ayarlayın:

    $DemoScenario = 3: Kurtarma bölgede yeni bir kiracı sağlamak.

2. Komut dosyasını çalıştırmak için F5'i seçin.

3. Hawthorn Hall olayları sayfası sonlandığında sağlamada tarayıcısında açar. 

    Hawthorn Hall veritabanı kurtarma bölgede bulunduğunu dikkat edin.

    ![Hawthorn kurtarma bölgede sağlanan Hall](media/saas-dbpertenant-dr-geo-restore/hawthorn-hall-provisioned-in-recovery-region.png)

4. Tarayıcıda Hawthorn Hall dahil görmek için Wingtip biletleri olayları hub sayfayı yenileyin. 

    Diğer kiracılar geri yüklemek beklemeden Hawthorn Hall sağlanan olursa, diğer kiracılar çevrimdışı olabilir.

## <a name="review-the-recovered-state-of-the-application"></a>Uygulamasını kurtarılan durumunu gözden geçirin

Kurtarma işlemi bittiğinde, uygulama ve tüm kiracılar kurtarma bölgede tam işlevlidir. 

1. Tüm kiracılar kurtarılan PowerShell konsol penceresinde görünen gösterir sonra olay hub'ı yenileyin. 

    Tüm kiracılar yeni Kiracı Hawthorn Hall dahil olmak üzere, çevrimiçi olarak görünür.

    ![Olay hub'ı kurtarılan ve yeni kiracılar](media/saas-dbpertenant-dr-geo-restore/events-hub-with-hawthorn-hall.png)

2. Contoso birlikte Hall üzerinde tıklayın ve kendi olayları sayfası açın. 

    Veritabanı kurtarma bölgede bulunan kurtarma sunucusunda bulunur altbilgisinde dikkat edin.

    ![Kurtarma bölgede contoso](media/saas-dbpertenant-dr-geo-restore/contoso-recovery-location.png)

3. İçinde [Azure portal](https://portal.azure.com), kaynak gruplarının listesini açın.  

    Dağıttığınız artı kurtarma kaynak grubu, Kurtarma sonekiyle kaynak grubunu dikkat edin. Kurtarma kaynak grubu, tüm kurtarma işlemi sırasında oluşturulan kaynakların yanı sıra kesinti sırasında oluşturulan yeni kaynaklar içeriyor. 

4. Kurtarma kaynak grubunu açın ve aşağıdaki öğeleri dikkat edin:

    * Katalog ve tenants1 sunucularının kurtarma sürümleri - kurtarma soneki. Tüm bu sunuculara geri yüklenen katalog ve Kiracı veritabanları özgün bölgede kullanılan adlara sahip.

    * Tenants2-dpt -&lt;kullanıcı&gt;-kurtarma SQL server. Bu sunucu, yeni kiracılar sırasında kesinti sağlamak için kullanılır.

    * Uygulama hizmeti adlı olayları-wingtip-dpt -&lt;recoveryregion&gt;-&lt;kullanıcı&gt;, olayları Uygulama Kurtarma örneğini olduğu.

    ![Kurtarma bölgede contoso kaynakları](media/saas-dbpertenant-dr-geo-restore/resources-in-recovery-region.png) 
    
5. Tenants2 açmak-dpt -&lt;kullanıcı&gt;-kurtarma SQL server. Veritabanı hawthornhall ve esnek havuz Pool1 içerdiğine dikkat edin. Hawthornhall veritabanı Pool1 esnek havuzdaki esnek bir veritabanı olarak yapılandırılır.

## <a name="change-the-tenant-data"></a>Kiracı verileri değiştirme 
Bu görevde, geri yüklenen Kiracı veritabanlarından birini güncelleştirin. Repatriation işlem kopyaları özgün bölgesine değiştirilmiş veritabanları geri. 

1. Tarayıcınızda, Contoso birlikte Hall için olaylar listesini bulmak, aracılığıyla olaylarını kaydırın ve son olay ciddi Strauss dikkat edin.

2. PowerShell ISE ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyasında, aşağıdaki değeri ayarlayın:

    $DemoScenario = 4: olay kurtarma bölgede Kiracı silin.

3. Komut dosyasını çalıştırmak için F5'i seçin.

4. Contoso birlikte Hall olayları sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;.trafficmanager.net/contosoconcerthall) ve olay ciddi Strauss eksik olduğuna dikkat edin.

Bu noktada öğreticide kurtarma bölgede şu anda çalışıyor uygulama kurtardı. Kurtarma bölgede yeni bir kiracı hazırladıktan ve veri geri yüklenen kiracılar birinin değiştirdi.  

> [!NOTE]
> Örnekteki diğer öğreticileri Kurtarma durumu uygulamada çalıştırmak için tasarlanmamıştır. Diğer öğreticiler keşfetmek uygulamanın ilk repatriate emin olun.

## <a name="repatriation-process-overview"></a>Repatriation işlemine genel bakış

Bir kesinti giderildikten sonra repatriation işlem uygulama ve veritabanlarını orijinal bölgeye döner.

![Coğrafi geri yükleme repatriation](media/saas-dbpertenant-dr-geo-restore/geo-restore-repatriation.png) 

İşlem:

1. Devam eden geri yükleme etkinlik durdurur ve herhangi bir bekleyen veya yürütülen veritabanı geri yükleme isteğini iptal eder.

2. Kesinti değiştirilmedi özgün bölge Kiracı veritabanları yeniden etkinleştirir. Bu veritabanları olanlar henüz kurtarılamaz ve bu kurtarılan ancak daha sonra değiştirilmez içerir. Yeniden etkinleştirilen veritabanları tam olarak son, kiracılar tarafından erişilir.

3. Yansıma özgün bölgede yeni bir kiracının sunucusu ve esnek havuzun sağlar. Bu eylem tamamlandıktan sonra yeni Kiracı diğer bu sunucuya işaret edecek şekilde güncelleştirilir. Diğer ad güncelleştirme yeni Kiracı ekleme kurtarma bölge yerine özgün bölgede oluşmasına neden olur.

3. Katalog Kurtarma bölgesinden özgün bölgesine taşımak için coğrafi çoğaltma kullanır.

4. Kurtarma bölgede kesinti sırasında yapılan değişiklikler ile tutarlı olacak şekilde havuzu yapılandırması özgün bölgede güncelleştirir.

5. Gerekli sunucular ve kesinti sırasında oluşturulan tüm yeni veritabanlarını barındırmak için havuzları oluşturur.

6. Güncelleştirilmiş veritabanlarını geri yükleme sonrası geri repatriate Kiracı ve kesinti sırasında sağlanan tüm yeni Kiracı veritabanları için coğrafi çoğaltma kullanır. 

7. Geri yükleme işlemi sırasında kurtarma bölgede oluşturulan kaynakları siler.

Repatriated gereken Kiracı veritabanları sayısını sınırlamak için 1-3 adımları derhal yapılır.  

4. adım, Kurtarma bölge kataloğunda sırasında kesinti değiştirilirse yalnızca yapılır. Katalog yeni kiracılar oluşturduysanız veya herhangi bir veritabanı veya havuzu yapılandırma kurtarma bölgede değiştirildiğinde güncelleştirilir.

Adım 7 kiracılar için en az kesintiye neden olur ve veri kaybı olmamasına önemlidir. Bu hedefe ulaşmak için coğrafi çoğaltma işlemi kullanır.

Coğrafi olarak çoğaltılmış her veritabanı önce özgün bölgede karşılık gelen veritabanı silinir. Kurtarma bölgede sonra özgün bölgede bir ikincil çoğaltma oluşturma coğrafi çoğaltılmış, veritabanıdır. Kiracı, çoğaltma tamamlandıktan sonra veritabanına herhangi bir bağlantısı kurtarma bölgede kıran kataloğunda çevrimdışı olarak işaretlenir. Veritabanı, üzerinde başarısız oldu, beklemede olan işlemleri ikincil dolayısıyla verileri işlemek için neden kaybolur. 

Yük devretme, veritabanı rolleri ters çevrilir. Özgün bölgede ikincil birincil okuma-yazma veritabanı haline gelir ve kurtarma bölgede veritabanı salt okunur bir ikincil olur. Katalog Kiracı girişi özgün bölgede veritabanı başvuracak şekilde güncelleştirilmez ve Kiracı çevrimiçi olarak işaretlenir. Bu noktada, veritabanının repatriation tamamlanır. 

Bunlar otomatik olarak bağlantı bozuk olduğunda yeniden emin olmak için yeniden deneme mantığı ile uygulamaları yazılması gerekir. Yeniden bağlanma aracısı için katalog kullanılırken, özgün bölgede repatriated veritabanı bağlayın. Kısa bağlantıyı kes fark genelde rağmen iş saatleri dışında veritabanları repatriate seçebilirsiniz.

Bir veritabanı repatriated sonra ikincil veritabanı kurtarma bölgede silinebilir. Özgün bölgede veritabanı daha sonra yeniden coğrafi geri yükleme için DR koruma kullanır.

8. adımda kaynak havuzları ve kurtarma sunucuları dahil olmak üzere Kurtarma bölgede silinir.

## <a name="run-the-repatriation-script"></a>Repatriation komut dosyasını çalıştır
Şimdi kesinti çözümlendi ve repatriation komut dosyasını çalıştırmak düşünün.

Öğretici uyguladıysanız, değişmeden oldukları için komut dosyası hemen Fabrikam Jazz kulübü ve özgün bölgede kızılcık Dojo yeniden etkinleştirir. Ardından, değiştirildiği için yeni Kiracı Hawthorn Hall ve Contoso birlikte Hall repatriates. Komut ayrıca Hawthorn Hall hazırlandığında güncelleştirildi katalog repatriates.
  
1. PowerShell ISE'de ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 komut dosyası, Katalog eşitleme işlemi, PowerShell örneğinde hala çalıştığından emin olun. Gerekirse, ayarlayarak yeniden başlatın:

    $DemoScenario = 1: Kiracı sunucu, havuzu ve veritabanı yapılandırma bilgilerini kataloğuna Eşitlemeyi Başlat.

    Komut dosyasını çalıştırmak için F5'i seçin.

2.  Ardından repatriation işlemini başlatmak üzere ayarlayın:

    $DemoScenario = 5: uygulama özgün bölgeye repatriate.

    Yeni bir PowerShell penceresi kurtarma komut dosyasını çalıştırmak için F5'i seçin. Repatriation birkaç dakika sürer ve PowerShell penceresinde izlenebilir.

3. Komut dosyası çalışırken, olayları hub sayfayı yenileyin (http://events.wingtip-dpt.&lt; Kullanıcı&gt;. trafficmanager.net).

    Tüm kiracılar çevrimiçi ve erişilebilir bu işlemi boyunca olduğuna dikkat edin.

4. Açmak için Fabrikam Jazz kulübü seçin. Bu Kiracı değiştirirseniz kaydetmedi altbilginin sunucu zaten özgün sunucuya geri dikkat edin.

5. Açın veya Contoso birlikte Hall olayları sayfayı yenileyin. Altbilginin başlangıçta, veritabanı hala kurtarma sunucusunda, dikkat edin. 

6. Yenileyin repatriation işlemi bittiğinde Contoso birlikte Hall olayları sayfası ve veritabanı artık özgün bölgenizde olduğuna dikkat edin.

7. Olay hub'ı tekrar yenileyin ve Hawthorn Hall açın. Veritabanını özgün bölgede bulunuyor dikkat edin. 

## <a name="clean-up-recovery-region-resources-after-repatriation"></a>Repatriation sonra kurtarma bölge kaynakları temizlemek
Repatriation tamamlandıktan sonra kurtarma bölgedeki kaynakları silmek güvenlidir. 

> [!IMPORTANT]
> Bunlar için tüm faturalama hemen durdurmak için bu kaynakları silin.

Geri yükleme işlemi bir kurtarma kaynak grubunda tüm kurtarma kaynaklar oluşturur. Temizleme işlemi, bu kaynak grubu siler ve Katalog kaynakları tüm başvuruları kaldırır. 

1. PowerShell ISE ...\Learning Modules\Business devamlılığı ve olağanüstü durum Recovery\DR-RestoreFromBackup\Demo-RestoreFromBackup.ps1 kodda ayarlayın:
    
    $DemoScenario = 6: Kurtarma bölgesinden eski kaynakları silin.

2. Komut dosyasını çalıştırmak için F5'i seçin.

Komut dosyalarını temizleme işleminden sonra geri başlattığınız yerde uygulamasıdır. Bu noktada, komut dosyasını yeniden çalıştırın veya diğer öğreticileri deneyin.

## <a name="designing-the-application-to-ensure-that-the-app-and-the-database-are-co-located"></a>Uygulama ve veritabanı birlikte bulunan olduğundan emin olmak için uygulama tasarlama 
Uygulama her zaman kiracının veritabanı ile aynı bölgede bir örneğine bağlanmak için tasarlanmıştır. Bu tasarım, uygulama ve veritabanı arasındaki gecikme süresini azaltır. Bu iyileştirme uygulama veritabanı etkileşimini kullanıcı uygulama etkileşim chattier olduğunu varsayar.  

Kiracı veritabanları kurtarma ve özgün bölgeler arasında repatriation sırasında süredir yayılıyor olabilir. Her veritabanı için veritabanı Kiracı sunucu adı bir DNS araması yaparak bulunur bölge uygulama arar. SQL veritabanı'nda bir diğer ad sunucu adıdır. Diğer sunucu adı, bölge adını içerir. Uygulama, veritabanı ile aynı bölgede değilse, veritabanı sunucusu ile aynı bölgede örneğine yönlendirir. Veritabanı ile aynı bölgede örneğine yönlendirerek uygulama ve veritabanı arasındaki gecikme süresi en aza indirir.  

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:
> [!div class="checklist"]

>* Kiracı katalog bir yansıma Kurtarma Ortamı'başka bir bölgede oluşturulmasına izin veren düzenli aralıklarla yenilendiğinden yapılandırma bilgilerini tutmak için kullanın.
>* Azure SQL veritabanları, Kurtarma bölgeye coğrafi geri yükleme kullanarak kurtarın.
>* Kiracı katalog geri yüklenen Kiracı veritabanı konumlarını yansıtacak şekilde güncelleştirin. 
>* Yeniden yapılandırma olmadan Kiracı kataloğun bağlanmak bir uygulama etkinleştirmek için bir DNS diğer adı kullanın.
>* Bir kesinti çözümlendikten sonra özgün bölgelerini kurtarılan veritabanlarını repatriate için coğrafi çoğaltma kullanın.

Deneyin [database coğrafi çoğaltma kullanarak çok kiracılı bir SaaS uygulaması için olağanüstü durum kurtarma](saas-dbpertenant-dr-geo-replication.md) öğretici coğrafi çoğaltma büyük ölçekli bir çok kiracılı uygulama kurtarmak için gereken zamanı önemli ölçüde azaltmak için nasıl kullanılacağını öğrenin.

## <a name="additional-resources"></a>Ek kaynaklar

[Wingtip SaaS uygulamasına yapı ek öğreticileri](https://docs.microsoft.com/azure/sql-database/sql-database-wtp-overview#sql-database-wingtip-saas-tutorials)
