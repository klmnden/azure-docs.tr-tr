---
title: Team Foundation Server dağıtımı için Visual Studio Team Services (VSTS) azure'da yeniden düzenleme | Microsoft Docs
description: Nasıl Contoso, şirket içi TFS dağıtımı geçirerek yeniden düzenler öğrenmek için Visual Studio Team Services (VSTS) azure'da bu.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: raynew
ms.openlocfilehash: 6b2067556cb42a1d40b3a8ba2bc681fbd602ab8d
ms.sourcegitcommit: 3d0295a939c07bf9f0b38ebd37ac8461af8d461f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2018
ms.locfileid: "43842646"
---
# <a name="contoso-migration--refactor-a-team-foundation-server-deployment-to-visual-studio-team-services-vsts"></a>Contoso geçiş: Team Foundation Server dağıtımı Visual Studio Team Services (VSTS) için yeniden düzenleme

Nasıl Contoso şirket içi Team Foundation Server (TFS) dağıtımı geçiş yaparak düzenlediği olduğundan bu makale, için Visual Studio Team Services (VSTS) azure'da. Contoso'nun geliştirme ekibi kullanmış TFS takım işbirliği ve kaynak denetimi için son beş yıldır. Artık, takım için bulut tabanlı bir çözüm kaynak denetimi ve geliştirme ve test iş için taşımak istediğiniz. Takımın bir DevOps modeline taşıma ve yeni yerel bulut uygulamaları geliştirin, VSTS bir rol oynayacak.

Bu belge, şirket içi kaynaklara Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.


**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Altyapıyı, serideki tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme](contoso-migration-assessment.md)  | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Kullanılabilir
[4. makale: bir uygulamayı bir Azure VM ve SQL veritabanı yönetilen örneği yeniden barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Contoso geçirir uygulama ön uç VM kullanarak [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso geçirir uygulama veritabanını kullanarak bir Azure SQL veritabanı yönetilen örneği [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir   
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel360 uygulamaya geçirir. Contoso, uygulama sanal makinelerini geçirmek için Site Recovery kullanır. Veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Azure Site Recovery kullanarak Azure vm'lerine Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini contoso tamamlar | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Azure Site Recovery kullanarak Azure Vm'leri için Linux osTicket uygulaması geçirir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso SmartHotel360 uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir 
Makale 11: TFS VSTS üzerinde yeniden düzenleyin. | Contoso, Visual Studio Team Services azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Bu makalede.
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel360 uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel360 uygulaması oluşturur. | Kullanılabilir



## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, gelecekteki hedeflerini tanımlamak için iş ortaklarıyla yakından çalışmıştır. İş ortakları aşırı geliştirme araç ve teknolojileri ile ilgili değildir, ancak şu noktaları yakalanan:

- **Yazılım**: temel faaliyet bağımsız olarak tüm şirketlerin Contoso dahil olmak üzere, yazılım şirketleri sunulmuştur. İş liderlik nasıl ilgileniyor BT kullanıcılar için yeni çalışan uygulamalar ve deneyimler müşterileri için şirket ortaya çıkmasına yardımcı olabilir.
- **Verimliliği**: Contoso gereken kolaylaştırabilir ve gereksiz yordamları geliştiriciler ve kullanıcılar için kaldırmak. Bu şirketin müşteri gereksinimlerine daha verimli bir şekilde sunmanıza olanak tanır. Hızlı zaman ya da para için iş gereksinimlerini BT.
- **Çeviklik**: Contoso BT gereken iş gereksinimlerine yanıt ve Market genel ekonomi başarılı etkinleştirmek için daha hızlı tepki verin. BT iş için bir pencere engelleyicisi olmamalıdır.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini vsts'ye geçiş için aşağı sabitlenmiş:

- Takımın verileri buluta geçirmek için bir aracı gerekir. Birkaç el ile gerçekleştirilen işlemleri gerekli.
- İş öğesi verileri ve geçen yıl için geçmiş geçirilmelidir.
- Takım, yeni kullanıcı adları ve parolalar ayarlamak istememektedir. Tüm geçerli sistem atamaları korunmalıdır.
- Takım, kaynak denetimi için Team Foundation sürüm denetimi (TFVC) uzak Git'e taşımak istiyor.
- Tam geçişi Git için kaynak kodun yalnızca en son sürümünü içe aktaran bir ipucu "geçişi" olacaktır. Tüm iş codebase kaydırmalar durdurulacaktır olduğunda, bir kesinti süreleri içinde gerçekleşir. Takım, yalnızca geçerli ana dal geçmişini taşıma sonrasında kullanılabilir olacağını farkındadır.
- Takımın değişiklik hakkında endişe ve tam bir taşıma işleminden önce test etmek istediğiniz. Takım, VSTS Git sonra bile TFS erişimi korumak istiyorsunuz.
- Contoso, birden çok koleksiyonu vardır ve süreci daha iyi anlamak için sadece birkaç projeleri içeren bir başlamak istiyorsanız.
- Takım, TFS koleksiyonlarını birden fazla URL ile VSTS hesabı ile bire bir ilişki olduğunu biliyoruz. Ancak, bu ayrımı kod tabanları ve projeleri için geçerli modelin eşleşir.


## <a name="proposed-architecture"></a>Önerilen mimarisi

- Contoso TFS projelerindeki buluta taşımak yanı sıra artık kendi projeleri veya kaynak denetimi şirket içi barındırma.
- TFS'yi VSTS'ye geçirilecektir.
- Contoso adlı bir TFS koleksiyonu şu anda sahip **ContosoDev**, hangi geçirilecek adlı bir VSTS hesabı **contosodevmigration.visualstudio.com**.
- VSTS projeleri, iş öğelerini, hataları ve yinelemeler geçen seneden geçirilecektir.
- Contoso Contoso dağıtımı sırasında ayarladığınız Azure Active Directory, yararlanarak [Azure altyapı](contoso-migration-infrastructure.md) geçiş planlama başında. 


![Senaryo mimarisi](./media/contoso-migration-tfs-vsts/architecture.png) 


## <a name="migration-process"></a>Geçiş işlemi

Contoso geçiş işlemi aşağıdaki gibi tamamlayın:

1. İlgili çok fazla hazırlık yoktur. İlk adım, Contoso, TFS geliştirdikleri desteklenen bir düzeye yükseltmeniz gerekir. Contoso TFS 2017 güncelleştirme 3 çalışıyor, ancak veritabanı geçişi kullanmak için desteklenen bir 2018 sürüm yapılan son güncelleştirmelerle birlikte çalışması için gerekli.
2. Yükselttikten sonra Contoso TFS geçiş aracını çalıştırın ve koleksiyonu doğrulayın.
3. Contoso hazırlık dosyaları kümesini oluşturmak ve test etmek için bir geçiş prova gerçekleştirin.
4. Contoso başka bir geçiş çalıştırılır, bu kez, iş öğelerini, hataları içeren tam bir geçiş eklenebilmesidir ve kod.
5. Geçişten sonra Contoso kodlarını TFVC'den Git'e taşınır.

![Geçiş işlemi](./media/contoso-migration-tfs-vsts/migration-process.png) 


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl tamamlanacağını garanti aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure depolama hesabı oluşturma**: Bu depolama hesabı geçiş işlemi sırasında kullanılacaktır.
> * **2. adım: Yükseltme TFS**: Contoso kendi dağıtım TFS 2018'den yükseltme 2'ye yükseltme. 
> * **3. adım: Koleksiyon doğrulama**: Contoso TFS koleksiyonu geçiş için hazırlık doğrulama.
> * **4. adım: Derleme hazırlama dosyası**: Contoso TFS geçiş aracını kullanarak geçiş dosyaları oluşturur. 


## <a name="step-1-create-a-storage-account"></a>1. adım: depolama hesabı oluşturma

1. Azure portalında Contoso Yöneticiler depolama hesabı oluşturma (**contosodevmigration**).
2. Bunlar hesabı, ikincil bölgeye yük devretme için - Orta ABD kullandıkları yerleştirin. Yerel olarak yedek depolama ile genel amaçlı bir standart hesabını kullanırlar.

    ![Depolama hesabı](./media/contoso-migration-tfs-vsts/storage1.png) 


**Daha fazla yardıma mı ihtiyacınız var?**

- [Azure depolamaya giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction).
- [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).


## <a name="step-2-upgrade-tfs"></a>2. adım: Yükseltme TFS

Contoso yöneticileri TFS sunucusu TFS 2018 güncelleştirme 2'ye yükseltin. Başlamadan önce:

- Sefer indirdiklerinde [TFS 2018 güncelleştirme 2](https://visualstudio.microsoft.com/downloads/)
- Bunlar doğrulama [donanım gereksinimleri](https://docs.microsoft.com/tfs/server/requirements)ve okuma aracılığıyla [sürüm notları](https://docs.microsoft.com/visualstudio/releasenotes/tfs2018-relnotes) ve [yükseltme tuzakları](https://docs.microsoft.com/tfs/server/upgrade/get-started#before-you-upgrade-to-tfs-2018).

Bunlar aşağıdaki gibi yükseltin:

1. Başlatmak için (bir VMware vM üzerinden çalışan), TFS sunucusu yedekleme ve VMware anlık görüntüsünü alın.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade1.png) 

2. TFS yükleyici başlar, ve bunlar yükleme konumu seçin. Yükleyici, internet erişimi gerektirir.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade2.png) 

3. Yükleme tamamlandıktan sonra Sunucu Yapılandırma Sihirbazı'nı başlatır.

    ![TFS](./media/contoso-migration-tfs-vsts/upgrade3.png) 

4. Doğrulamadan sonra Yükseltme Sihirbazı tamamlar.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade4.png) 

5. Bunlar, TFS yüklemesi projeleri, iş öğeleri ve kod gözden geçirerek doğrulayın.

     ![TFS](./media/contoso-migration-tfs-vsts/upgrade5.png) 

> [!NOTE]
> Yükseltme tamamlandıktan sonra Özellikleri Yapılandırma Sihirbazı'nı çalıştırmak bazı TFS yükseltmeleri gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/vsts/work/customize/configure-features-after-upgrade?utm_source=ms&utm_medium=guide&utm_campaign=vstsdataimportguide&view=vsts).

**Daha fazla yardıma mı ihtiyacınız var?**

Hakkında bilgi edinin [TFS'ye yükseltme](https://docs.microsoft.com/tfs/server/upgrade/get-started).

## <a name="step-3-validate-the-tfs-collection"></a>3. adım: TFS koleksiyonu doğrulama

Contoso yöneticileri TFS geçiş aracı, geçiş işleminden önce doğrulamak için ContosoDev koleksiyon veritabanında çalıştırın.

1. İndirin ve açın [TFS geçiş aracı](https://www.microsoft.com/download/details.aspx?id=54274). Çalışmakta olan TFS güncelleştirme sürümü indirmek önemlidir. Sürümü Yönetici konsolunda denetlenebilir.

    ![TFS](./media/contoso-migration-tfs-vsts/collection1.png)

2. Bunlar, takım projesi koleksiyonunun URL'sini belirterek doğrulama gerçekleştirmek için aracı çalıştırın:

        **TfsMigrator validate /collection:http://contosotfs:8080/tfs/ContosoDev**


3. Aracı bir hata gösterir.

    ![TFS](./media/contoso-migration-tfs-vsts/collection2.png)

5. Bunlar bulunan günlük dosyaları bulunur **günlükleri** aracı konumu hemen önce bir klasör. Her bir başlıca doğrulama için bir günlük dosyası oluşturulur. **TfsMigration.log** ana bilgileri tutar.

    ![TFS](./media/contoso-migration-tfs-vsts/collection3.png)

4. Bunlar, kimliğiyle ilgili, bu girdiyi bulun.

    ![TFS](./media/contoso-migration-tfs-vsts/collection4.png)

5. Çalışan **TfsMigration doğrula/Help** komut satırında ve görüp komutu **/tenantDomainName** kimliklerini doğrulamak için gerekli görünüyor.

     ![TFS](./media/contoso-migration-tfs-vsts/collection5.png)

6. Doğrulama komutu yeniden çalıştırın ve Azure AD adlarının yanı sıra bu değeri dahil et: **TfsMigrator doğrula/Collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com**.

    ![TFS](./media/contoso-migration-tfs-vsts/collection7.png)

7. Bir Azure AD oturum açma ekranı görüntülenir ve bunlar bir genel yönetici kullanıcı kimlik bilgilerini girin.

     ![TFS](./media/contoso-migration-tfs-vsts/collection8.png)

8. Doğrulama geçirir ve araç tarafından onaylanır.

    ![TFS](./media/contoso-migration-tfs-vsts/collection9.png)



## <a name="step-4-create-the-migration-files"></a>4. adım: geçiş dosyaları oluşturma

Doğrulama tamamlandı Contoso yöneticileri TFS geçiş aracı geçiş dosyaları oluşturmak için kullanabilirsiniz.

1. Bunlar, hazırlama adımı araçta çalıştırın.

    **TfsMigrator hazırlama/Collection:http://contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus**

     ![Hazırlama](./media/contoso-migration-tfs-vsts/prep1.png)

    Hazırlama şunları yapar:
    - Tüm kullanıcıların listesini bulmak için koleksiyon tarar ve tanımla Haritası günlük doldurur (**IdentityMapLog.csv**])
    - Her bir kimliği için bir eşleştirme bulmak üzere Azure Active Directory bağlantısı hazırlar.
    - Contoso zaten Azure AD dağıtılan ve hazırlama eşleşen kimlikler bulabilir ve bunları etkin olarak işaretleyin bu nedenle, AD Connect kullanarak eşitlenir.

2. Bir Azure AD oturum açma ekranı görüntülenir ve bunlar genel yönetici kimlik bilgilerini girin

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep2.png)

3. Hazırlama tamamlandıktan ve aracı dosyalarını içeri aktarın başarıyla oluşturulduğunu bildiriyor.

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep3.png)

4. Bunlar artık IdentityMapLog.csv hem import.json dosyanın yeni bir klasöre oluşturulduğunu görebilirsiniz.

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep4.png)

5. İmport.json dosyası içeri aktarma ayarları sağlar. Bu depolama hesabı bilgileri ve istenen hesap adı gibi bilgileri içerir. Alanların çoğu, otomatik olarak doldurulur. Bazı alanları kullanıcı girişi gerekli. Dosyasını açın ve oluşturulması için VSTS hesabı adı ekler: **contosodevmigration**. Bu ada sahip, kendi VSTS URL'sidir **contosodevmigration.visualstudio.com**.

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > Hesap geçiş işleminden önce oluşturulmalıdır, geçiş tamamlandıktan sonra değiştirilebilir.

6. Bunlar, VSTS'de içeri aktarma sırasında kapsama dahil edilecektir hesaplarını gösterir kimlik günlük eşleme dosyasını inceleyin. 

    - VSTS kullanıcıları içeri aktarma işleminden sonra olacak kimlikleri etkin kimlikleri bakın.
    - VSTS, bu kimlikleri lisanslanacağını ve geçişten sonra bir kullanıcı hesabı olarak gösterilir.
    - Bu kimlikleri olarak işaretlenmiş **etkin** içinde **beklenen içeri aktarma durumu** dosyasındaki sütun.

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep6.png)



## <a name="step-5-migrate-to-vsts"></a>5. adım: VSTS'ye geçirme

Yerinde hazırlıkla Contoso yöneticileri artık geçiş odaklanabilirsiniz. Geçiş çalıştırdıktan sonra sürüm denetimi için TFVC, Git kullanarak geçiş yaparsınız.

Başlamadan önce yöneticilerin koleksiyonu geçiş için çevrimdışı olması için geliştirme ekibi ile kapalı kalma süresi zamanlayın. Geçiş işlemi için adımlar şunlardır:

1. **Koleksiyon ayırma**: Bunlar koleksiyon için kimlik verilerini bulunduğu TFS sunucusu yapılandırma veritabanında koleksiyon eklenmiş ve çevrimiçi durumdayken. Koleksiyonu TFS sunucusundan ayrıldığında, bu kimlik verilerinin bir kopyasını alır ve taşıma için koleksiyonuyla paketler. Bu veriler olmadan kimlik bölümü alma yürütülemez. İçeri aktarma işlemi tamamlanana kadar içeri aktarma sırasında oluşan değişiklikleri almak için hiçbir yolu olmadığından koleksiyon ayrılmış kalın önerilir.
2. **Bir yedekleme oluşturmak**: bunları VSTS aktarılabilen bir yedekleme oluşturmak sonraki adım geçiş işleminin içindir. Veri katmanı uygulaması bileşen paketleri (DACPAC), veritabanı değişiklikleri tek bir dosya halinde paketlenmiş ve dağıtılan diğer SQL örneklerine izin veren bir SQL Server özelliğidir. Ayrıca doğrudan VSTS'ye geri yüklenebilir ve bu nedenle buluta koleksiyon verisi almak için paketleme yöntemi olarak kullanılır. Contoso, DACPAC oluşturmak için SqlPackage.exe Aracı'nı kullanır. Bu araç, SQL Server veri Araçları'nda bulunur.
3. **Karşıya yükleme, depolama alanına**: DACPAC sonra oluşturulur, bunlar Azure Storage'a yükler. Karşıya yüklendikten sonra bunlar bir paylaşılan erişim imzası (SAS) depolama TFS geçiş aracı erişmesine izin vermek için alın.
4. **Alma dolgu**: Bunlar ardından DACPAC ayarı dahil olmak üzere içe aktarma dosyasındaki eksik alanları doldurabilirsiniz. Bunlar yapmak istediğiniz başlamak belirtirsiniz bir **prova** her şeyin tam geçişten önce düzgün şekilde çalıştığını denetlemek için içeri aktarma.
5. **Bir prova yapmak**: prova içeri aktarmalar yardımcı koleksiyon geçişi test edin. Kuru çalıştırma ömrünü sınırlı ve üretim geçiş çalışmadan önce silindi. Otomatik olarak ayarlanmış bir süre sonra silinen. Başarı e-postada içeri aktarma tamamlandıktan sonra alınan prova zaman silinecek hakkında bir not dahildir. Not alın ve buna göre planlayın.
6. **Üretim Geçişi tamamlamak**: import.json güncelleştiriliyor ve içeri aktarma çalıştırarak yeniden Contoso yöneticileri prova geçişi tamamlandı son geçiş yapın.



### <a name="detach-the-collection"></a>Koleksiyon ayırma

Başlamadan önce Contoso yöneticilerinin bir yerel SQL Server Yedekleme ve TFS sunucusu VMware anlık görüntüsünü kullanımdan çıkarmadan önce yararlanın.

1.  TFS yönetim konsolunda, bunlar istedikleri ayırmak için koleksiyonu seçin (**ContosoDev**).

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate1.png)

2. İçinde **genel**, seçtikleri **koleksiyonunu Ayır**

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate2.png)

3. Takım projesi koleksiyonu ayırma Sihirbazı'nda > **hizmet mesajı**, koleksiyondaki projelere bağlanmayı deneyebilecek kullanıcılar için bir ileti sağlarlar.

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate3.png)

4. İçinde **ayırma ilerleme**, ilerleme durumunu izlemek ve tıklayın **sonraki** işlemi tamamlandığında.

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate4.png)

5. İçinde **hazırlık denetimleri**, denetimleri tamamladığınızda, tıklayın **ayırma**.

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate5.png)

6. Simgeye **Kapat** Kurulumu tamamlamak.

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate6.png)

6. Koleksiyon artık TFS yönetici konsolundan başvuruluyor.

    ![Geçiş](./media/contoso-migration-tfs-vsts/migrate7.png)


### <a name="generate-a-dacpac"></a>DACPAC oluştur

Contoso yöneticileri, VSTS'na aktarma için bir yedekleme (DACPAC) oluşturun.

- SQL Server veri Araçları'nda SqlPackage.exe DACPAC oluşturmak için kullanılır. Birden çok 120 ve 130 140 gibi adları olan klasörler altında bulunan SQL Server veri araçları ile birlikte yüklenen SqlPackage.exe sürümü vardır. DACPAC hazırlamak için doğru sürüm kullanılması önemlidir.
- TFS 2018'den içeri aktarmalar, 140 klasöründen veya üzeri SqlPackage.exe kullanmanız gerekebilir.  CONTOSOTFS için bu dosyayı klasörde yer alır: **C:\Program Files (x86) \Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140**.


Contoso yöneticileri DACPAC şu şekilde oluşturun:

1. Bunlar, bir komut istemi açın ve SQLPackage.exe konumuna gidin. Bunlar, DACPAC oluşturmak için bu aşağıdaki komutu yazın:

    **SqlPackage.exe /sourceconnectionstring: "veri kaynağı SQLSERVERNAME\INSTANCENAME; = Initial Catalog = Tfs_ContosoDev; Integrated Security = True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData = true /p: miyim gnoreUserLoginMappings true /p:IgnorePermissions = true /p:Storage = bellek =** 

    ![Backup](./media/contoso-migration-tfs-vsts/backup1.png)

2. Komut çalıştıktan sonra aşağıdaki ileti görüntülenir.

    ![Backup](./media/contoso-migration-tfs-vsts/backup2.png)

3. Bunlar DACPACfile özelliklerini doğrulayın

    ![Backup](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="update-the-file-to-storage"></a>Dosya depolama için güncelleştirme

Contoso DACPAC oluşturulduktan sonra Azure Depolama'ya yükler.

1. Bunlar [yükleyip](https://azure.microsoft.com/features/storage-explorer/) Azure Depolama Gezgini.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup5.png)

4. Aboneliğine bağlanın ve geçiş için oluşturduğu depolama hesabını bulun (**contosodevmigration**). Yeni bir blob kapsayıcısı, oluşturdukları **vstsmigration**.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup6.png)

5. Bunlar, blok blobu olarak karşıya yükleme için DACPAC dosyası belirtin.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup7.png)
    
7. Dosya karşıya sonra bunların dosya adını tıklayın > **Generate SAS**. Bunlar depolama hesabı altında blob kapsayıcıları genişletin, içeri aktarma dosyalarıyla kapsayıcıyı seçin ve tıklayın **paylaşılan erişim imzası Al**.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup8.png)

8. Varsayılanları kabul edin ve tıklayın **Oluştur**. Bu, 24 saat boyunca erişim sağlar.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup9.png)

9. Böylece TFS geçiş araç tarafından kullanılabilmesi için paylaşılan erişim imzası URL'sini kopyalayın.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup10.png)

> [!NOTE]
> İzin verilen sürede penceresi veya izinleri dolmadan geçiş gerçekleştirilmesi gerekir.
> Azure portalından bir SAS anahtarı oluşturma. Şu şekilde oluşturulan anahtarları hesabı kapsamlı ve içeri aktarma ile çalışmaz.

### <a name="fill-in-the-import-settings"></a>İçeri aktarma ayarlarını doldurun

Daha önce Contoso yöneticileri kısmen içeri aktarma belirtim dosyasının (import.json) doldurulur. Artık, kalan ayarları eklemeniz gerekir.

İmport.json dosyasını açın ve aşağıdaki alanları doldurun: • konumu: yukarıda oluşturulan SAS anahtarını konumu.
• Dacpac: depolama hesabına yüklediğiniz DACPAC dosyasının adı ayarlayın. ".Dacpac" uzantısına dahil etme.
• ImportType: şu an için prova için ayarlayın.


![İçeri aktarma ayarları](./media/contoso-migration-tfs-vsts/import1.png)


### <a name="do-a-dry-run-migration"></a>Prova geçiş yapın

Contoso yöneticileri bir prova geçişle her şeyin beklendiği gibi çalıştığından emin olmak için başlatın.

1. Bir komut istemi açın ve TfsMigration konumuna (C:\TFSMigrator) bulun.
2. İlk adım olarak içeri aktarma dosyası doğrularlar. Emin olmak için dosyanın düzgün biçimlendirildiğinden ve SAS anahtarını çalışıp çalışmadığını isterler.

    **TfsMigrator /importFile:C:\TFSMigrator\import.json /validateonly içeri aktarma**

3. Doğrulama SAS anahtarı daha uzun bir süre sonu zamanı gerekiyor. bir hata döndürür.

    ![Prova](./media/contoso-migration-tfs-vsts/test1.png)

3. Süre sonu yedi gün olarak ayarlanmış olan yeni bir SAS anahtarı oluşturmak için Azure Depolama Gezgini kullanırlar.

    ![Prova](./media/contoso-migration-tfs-vsts/test2.png)

3. Bunlar import.json dosyasını güncelleştirin ve doğrulamayı tekrar çalıştırın. Bu kez başarılı bir şekilde tamamlar.

    **TfsMigrator /importFile:C:\TFSMigrator\import.json /validateonly içeri aktarma**

    ![Prova](./media/contoso-migration-tfs-vsts/test3.png)
    
7. Bunlar prova başlatın:

    **TfsMigrator alma /importFile:C:\TFSMigrator\import.json**

8. Geçişi onaylamak için bir ileti görüntülenir. Sonra prova hazırlanmış verinin korunur süreyi not edin.

    ![Prova](./media/contoso-migration-tfs-vsts/test4.png)

9. Azure AD oturum açma görünür ve Contoso Yöneticisi oturum açma ile tamamlanıyor.

    ![Prova](./media/contoso-migration-tfs-vsts/test5.png)

10. Bir ileti alma hakkında bilgi gösterir.

    ![Prova](./media/contoso-migration-tfs-vsts/test6.png)

11. 15 dakika veya bunu sonra bunlar URL'sine gidin ve aşağıdaki bilgilere bakın:

     ![Prova](./media/contoso-migration-tfs-vsts/test7.png)

12. Geçiş tamamlandıktan sonra bir Contoso geliştirme müşteri adayları prova düzgün şekilde çalıştığını denetlemek için VSTS imzalar. Kimlik doğrulamasından sonra VSTS hesabı onaylamak için birkaç ayrıntı gerekir.

    ![Prova](./media/contoso-migration-tfs-vsts/test8.png)

13. VSTS'de Geliştirme lideri projeleri VSTS'ye geçirildiğini görebilirsiniz. Hesap 15 gün içinde silinecek bir bildirim yoktur.

    ![Prova](./media/contoso-migration-tfs-vsts/test9.png)

14. Geliştirme lideri projelerden birine ve açar **iş öğelerini** > **Bana atanan**. Bu iş öğesi verileri, kimlikle birlikte geçirildiğini gösterir.

    ![Prova](./media/contoso-migration-tfs-vsts/test10.png)

15. Müşteri adayı, başka proje ve kod, kaynak kodu ve geçmiş geçirildi olduğunu onaylamak için de denetler.

    ![Prova](./media/contoso-migration-tfs-vsts/test11.png)


### <a name="run-the-production-migration"></a>Üretim geçiş çalıştırma

Prova ile tam, Contoso yöneticileri için üretim geçiş geçin. Bunlar prova silmek, içeri aktarma ayarlarını güncelleştirmek ve içeri aktarma yeniden çalıştırın.

1. VSTS Portalı'nda, bunlar prova hesabının silin.
2. Ayarlanacak import.json dosyanın güncelleştirilmesi **ImportType** için **ProductionRun**.

    ![Üretim](./media/contoso-migration-tfs-vsts/full1.png)

3. Geçiş için prova gibi başlatmaları: **TfsMigrator alma /importFile:C:\TFSMigrator\import.json**.
4. Bir ileti geçişi onaylamak için gösterir ve verileri bir hazırlama alanı olarak güvenli bir konumda yedi güne kadar tutulabilirsiniz, sizi uyarır.

    ![Üretim](./media/contoso-migration-tfs-vsts/full2.png)

5. Azure AD oturum içinde bunlar bir Contoso Yöneticisi oturum açma belirtin.

    ![Üretim](./media/contoso-migration-tfs-vsts/full3.png)

6. Bir ileti alma hakkında bilgi gösterir.

    ![Üretim](./media/contoso-migration-tfs-vsts/full4.png)

7. Yaklaşık 15 dakika sonra bunlar URL'sine göz atın ve aşağıdaki bilgileri görür:

    ![Üretim](./media/contoso-migration-tfs-vsts/full5.png)

8. Contoso geliştirme müşteri adayları, kontrol etmek için VSTS oturum geçiş tamamlandıktan sonra geçiş düzgün bir şekilde çalıştı. Oturum açtıktan sonra müşteri adayı projeleri geçirildiğini görebilirsiniz.

    ![Üretim](./media/contoso-migration-tfs-vsts/full6.png)

8. Geliştirme lideri projelerden birine ve açar **iş öğelerini** > **Bana atanan**. Bu iş öğesi verileri, kimlikle birlikte geçirildiğini gösterir.

    ![Üretim](./media/contoso-migration-tfs-vsts/full7.png)

9. Müşteri adayı onaylamak için diğer iş öğesi verileri denetler.

    ![Üretim](./media/contoso-migration-tfs-vsts/full8.png)

15. Müşteri adayı, başka proje ve kod, kaynak kodu ve geçmiş geçirildi olduğunu onaylamak için de denetler.

    ![Üretim](./media/contoso-migration-tfs-vsts/full9.png)


### <a name="move-source-control-from-tfvc-to-git"></a>Kaynak denetimi TFVC'den GİT'e taşıma

Geçiş ile tamamlandı, Contoso için kaynak kodu Yönetimi TFVC'den Git'e taşımak istiyor. Kaynak kodunda şu anda VSTS hesabı, aynı hesaptaki Git depoları olarak içeri aktarmak contoso yöneticilerinin gerekir.

1. VSTS Portalı'nda, TFVC depoları birini açın (**$/ PolicyConnect**) ve gözden geçirin.

    ![Git](./media/contoso-migration-tfs-vsts/git1.png)

2. Simgeye **kaynak** açılan > **alma**.

    ![Git](./media/contoso-migration-tfs-vsts/git2.png)

3. İçinde **kaynak türünü** seçmeleri **TFVC**ve depo yolunu belirtin. Geçmişi geçirmek istemiyorsanız verdiyseniz.

    ![Git](./media/contoso-migration-tfs-vsts/git3.png)

    > [!NOTE]
    > TFVC ve Git sürüm denetim bilgileri nasıl depolamak farklar nedeniyle, Contoso geçmişi geçişi yapamayacağınızı açıklığa öneririz. Windows ve diğer ürünler merkezi sürüm denetiminden Git'e geçiş, Microsoft geçen bir yaklaşım budur.

4. İçeri aktarma sonrasında Yöneticiler kodu gözden geçirin.

    ![Git](./media/contoso-migration-tfs-vsts/git4.png)

5. Bunlar ikinci depo için işlemi tekrarlayın (**$/ SmartHotelContainer**).

    ![Git](./media/contoso-migration-tfs-vsts/git5.png)

6. Kaynak gözden geçirdikten sonra geliştirme geliyor vsts'ye geçiş yapılır kabul etmiş olursunuz. VSTS, artık geçiş ekiplerin içindeki tüm geliştirme için kaynağı haline gelir.

    ![Git](./media/contoso-migration-tfs-vsts/git6.png)



**Daha fazla yardıma mı ihtiyacınız var?**

[Daha fazla bilgi edinin](https://docs.microsoft.com/vsts/git/import-from-tfvc?view=vsts) TFVC'den içeri aktarma hakkında.

##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile Contoso şunları yapmanız gerekir:

- Gözden geçirme [sonrası içeri aktarma](https://docs.microsoft.com/vsts/articles/migration-post-import?view=vsts) makale ek alma etkinlikleri hakkında bilgi için.
- TFVC depoları silmek ya da bunlar salt okunur moduna alın. Kod tabanlarında gerekmez kullanılmıştır, ancak bunların geçmişini başvurulabilir.

## <a name="next-steps"></a>Sonraki adımlar

Contoso ilgili takım üyeleri için VSTS ve Git eğitimi sağlamanız gerekir.



