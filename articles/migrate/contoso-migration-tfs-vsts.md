---
title: Azure DevOps Azure hizmetlerinde Team Foundation Server dağıtımına yeniden düzenleme | Microsoft Docs
description: Nasıl Contoso, şirket içi TFS dağıtımı geçirerek yeniden düzenler öğrenin, azure'da Azure DevOps hizmetlerine.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 21396a10543d388b6ac360f426272f1841b2f510
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60674586"
---
# <a name="contoso-migration--refactor-a-team-foundation-server-deployment-to-azure-devops-services"></a>Contoso geçişi:  Azure DevOps Services’a bir Team Foundation Server dağıtımını yeniden düzenleme

Nasıl Contoso yeniden düzenleme, şirket içi Team Foundation Server (TFS) dağıtımı geçiş yaparak bu makale, Azure, Azure DevOps hizmetlerine. Contoso'nun geliştirme ekibi kullanmış TFS takım işbirliği ve kaynak denetimi için son beş yıldır. Artık, bulut tabanlı bir çözüm için kaynak denetimi ve geliştirme ve test iş için taşımak istiyorum. Bir Azure DevOps modeline taşıma ve bulutta çalışan yeni uygulamalar geliştirebilir, azure DevOps hizmetleriyle bir rol oynayacak.

Bu belge, şirket içi kaynaklara Contoso adlı kurgusal şirketin Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapısını kurma ve farklı türde geçiş çalıştırmak nasıl çalışılacağını senaryolar içerir. Senaryoları, karmaşık hale gelmesi. Zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: Bir Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Aynı altyapı tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
[3. makale: Şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso Wmware'de çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığını gösterir. Bunlar uygulama Vm'lerle değerlendirmek [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği için yeniden barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirdiğini gösterir. Uygulama web kullanarak VM'yi geçirme [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve veritabanı kullanarak uygulama [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) SQL yönetilen örneğine geçirmek için hizmet. | Kullanılabilir
[Makale 5: Azure Vm'lerine yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme kendi SmartHotel Site Recovery hizmetini kullanarak Azure Iaas Vm'leri için gösterir.
[Makale 6: Azure sanal makineleri ve SQL Server kullanılabilirlik gruplarını yeniden barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Bunlar, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site RECOVERY'yi kullanın. | Kullanılabilir
[Makale 7: Azure sanal makinelerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Contoso osTicket Linux uygulamalarını Azure Site Recovery kullanarak Azure Iaas Vm'leri için nasıl geçirdiğini gösterir.
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu için bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso osTicket Linux uygulaması nasıl geçirdiğini gösterir. VM geçiş için Site Recovery ve MySQL Workbench'i Azure MySQL Server örneğine geçirmek için kullanırlar. | Kullanılabilir
[Makale 9: Bir Azure Web uygulaması ve Azure SQL veritabanı için bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulama için bir Azure kapsayıcı tabanlı web uygulaması geçirir ve uygulama veritabanı için Azure SQL Server geçirdiğini gösterir. | Kullanılabilir
[Makale 10: Bir Linux uygulaması Azure App Service ve Azure MySQL sunucusu için yeniden düzenleme](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso osTicket Linux uygulaması Azure App Service'te PHP 7.0 Docker kapsayıcısı kullanarak nasıl geçirdiğini gösterir. Dağıtım için kod tabanının Github'a geçirilir. Uygulama veritabanı için Azure MySQL geçirilir. | Kullanılabilir
11. makale: Azure DevOps Hizmetleri'nde TFS dağıtımını yeniden düzenleyin | Azure DevOps Azure hizmetlerinde geliştirme uygulama TFS geçirme | Bu makalede
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir


## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, gelecekteki hedeflerini tanımlamak için iş ortaklarıyla yakından çalışmıştır. İş ortakları aşırı geliştirme araç ve teknolojileri ile ilgili değildir, ancak şu noktaları yakalanan:

- **Yazılım**: Temel faaliyet bağımsız olarak tüm şirketlerin Contoso dahil olmak üzere, yazılım şirketleri sunulmuştur. İş liderlik nasıl ilgileniyor BT kullanıcılar için yeni çalışan uygulamalar ve deneyimler müşterileri için şirket ortaya çıkmasına yardımcı olabilir.
- **Verimliliği**: Contoso kolaylaştırabilir ve gereksiz yordamları geliştiriciler ve kullanıcılar için kaldırmak gerekir. Bu şirketin müşteri gereksinimlerine daha verimli bir şekilde sunmanıza olanak tanır. Hızlı zaman ya da para için iş gereksinimlerini BT.
- **Çeviklik**:  Contoso BT iş gereksinimlerine yanıt ve Market genel ekonomi başarılı etkinleştirmek için daha hızlı tepki gerekir. BT iş için bir pencere engelleyicisi olmamalıdır.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini geçiş Azure DevOps hizmetler için aşağı sabitlenmiş:

- Takımın verileri buluta geçirmek için bir aracı gerekir. Birkaç el ile gerçekleştirilen işlemleri gerekli.
- İş öğesi verileri ve geçen yıl için geçmiş geçirilmelidir.
- Yeni kullanıcı adları ve parolalar ayarlamak istemezsiniz. Tüm geçerli sistem atamaları korunmalıdır.
- Kaynak denetimi için Team Foundation sürüm denetimi (TFVC) uzak Git'e taşımak istiyor.
- Tam geçişi Git için kaynak kodun yalnızca en son sürümünü içe aktaran bir ipucu "geçişi" olacaktır. Tüm iş codebase kaydırmalar durdurulacaktır olduğunda, bir kesinti süreleri içinde gerçekleşir. Bunlar, yalnızca geçerli ana dal geçmişini taşıma sonrasında kullanılabilir olacağını anlayın.
- Bunlar, değişiklik hakkında endişeleriniz ve tam bir taşıma işleminden önce test etmek istediğiniz. Azure DevOps hizmetlerine taşıdıktan sonra bile TFS erişimi korumak isterler.
- Birden çok koleksiyon sahip ve süreci daha iyi anlamak için sadece birkaç projeleri içeren bir başlamak istiyorsunuz.
- Bunlar, birden fazla URL sahip şekilde TFS koleksiyonlarını Azure DevOps Hizmetleri kuruluşlar ile bire bir ilişki olduğunu biliyoruz. Ancak, bu ayırma kod tabanları ve projeleri için geçerli model eşleşir.


## <a name="proposed-architecture"></a>Önerilen mimarisi

- Contoso TFS projelerindeki buluta taşımak yanı sıra artık kendi projeleri veya kaynak denetimi şirket içi barındırma.
- TFS, Azure DevOps hizmetlerine geçirilecektir.
- Contoso adlı bir TFS koleksiyonu şu anda sahip **ContosoDev**, hangi geçirilecek adlı bir Azure DevOps Hizmetleri kuruluş **contosodevmigration.visualstudio.com**.
- Azure DevOps Hizmetleri projeleri, iş öğelerini, hataları ve yinelemeler geçen seneden geçirilecektir.
- Contoso yararlanarak kendi Azure Active ne zaman ayarlanan dizininde, bunlar [Azure altyapılarını dağıtılan](contoso-migration-infrastructure.md) bunların geçiş planlama başında. 


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
> * **1. adım: Bir Azure depolama hesabı oluşturma**: Bu depolama hesabına geçiş işlemi sırasında kullanılacaktır.
> * **2. adım: TFS yükseltme**: Contoso dağıtımı, TFS 2018'den yükseltme 2'ye yükseltir. 
> * **3. adım: Koleksiyon doğrulama**: Contoso, TFS koleksiyonu geçiş için hazırlık doğrulayacaktır.
> * **4. adım: Hazırlık dosyası derleme**: Contoso TFS geçiş aracını kullanarak geçiş dosyaları oluşturur. 


## <a name="step-1-create-a-storage-account"></a>1. Adım: Depolama hesabı oluşturma

1. Azure portalında Contoso Yöneticiler depolama hesabı oluşturma (**contosodevmigration**).
2. Bunlar hesabı, ikincil bölgeye yük devretme için - Orta ABD kullandıkları yerleştirin. Yerel olarak yedekli depolama ile genel amaçlı bir standart hesabını kullanırlar.

    ![Depolama hesabı](./media/contoso-migration-tfs-vsts/storage1.png) 


**Daha fazla yardıma mı ihtiyacınız var?**

- [Azure depolamaya giriş](https://docs.microsoft.com/azure/storage/common/storage-introduction).
- [Depolama hesabı oluşturma](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).


## <a name="step-2-upgrade-tfs"></a>2. Adım: TFS yükseltme

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
> Yükseltme tamamlandıktan sonra Özellikleri Yapılandırma Sihirbazı'nı çalıştırmak bazı TFS yükseltmeleri gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/devops/reference/configure-features-after-upgrade?utm_source=ms&utm_medium=guide&utm_campaign=vstsdataimportguide&view=vsts).

**Daha fazla yardıma mı ihtiyacınız var?**

Hakkında bilgi edinin [TFS'ye yükseltme](https://docs.microsoft.com/tfs/server/upgrade/get-started).

## <a name="step-3-validate-the-tfs-collection"></a>3. Adım: TFS koleksiyonu doğrula

Contoso yöneticileri TFS geçiş aracı, geçiş işleminden önce doğrulamak için ContosoDev koleksiyon veritabanında çalıştırın.

1. İndirin ve açın [TFS geçiş aracı](https://www.microsoft.com/download/details.aspx?id=54274). Çalışmakta olan TFS güncelleştirme sürümü indirmek önemlidir. Sürümü Yönetici konsolunda denetlenebilir.

    ![TFS](./media/contoso-migration-tfs-vsts/collection1.png)

2. Bunlar, proje koleksiyonunun URL'sini belirterek doğrulama gerçekleştirmek için aracı çalıştırın:

   **TfsMigrator doğrula/Collection: http:\//contosotfs:8080/tfs/ContosoDev**


3. Aracı bir hata gösterir.

    ![TFS](./media/contoso-migration-tfs-vsts/collection2.png)

5. Bunlar bulunan günlük dosyaları bulunur **günlükleri** aracı konumu hemen önce bir klasör. Her bir başlıca doğrulama için bir günlük dosyası oluşturulur. **TfsMigration.log** ana bilgileri tutar.

    ![TFS](./media/contoso-migration-tfs-vsts/collection3.png)

4. Bunlar, kimliğiyle ilgili, bu girdiyi bulun.

    ![TFS](./media/contoso-migration-tfs-vsts/collection4.png)

5. Çalışan **TfsMigration doğrula/Help** komut satırında ve görüp komutu **/tenantDomainName** kimliklerini doğrulamak için gerekli görünüyor.

     ![TFS](./media/contoso-migration-tfs-vsts/collection5.png)

6. Bunlar doğrulama komutunu yeniden çalıştırın ve Azure AD adlarının yanı sıra bu değeri içerir: **TfsMigrator doğrula/Collection: http:\//contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com**.

    ![TFS](./media/contoso-migration-tfs-vsts/collection7.png)

7. Bir Azure AD oturum açma ekranı görüntülenir ve bunlar bir genel yönetici kullanıcı kimlik bilgilerini girin.

     ![TFS](./media/contoso-migration-tfs-vsts/collection8.png)

8. Doğrulama geçirir ve araç tarafından onaylanır.

    ![TFS](./media/contoso-migration-tfs-vsts/collection9.png)



## <a name="step-4-create-the-migration-files"></a>4. Adım: Geçiş dosyaları oluşturma

Doğrulama tamamlandı Contoso yöneticileri TFS geçiş aracı geçiş dosyaları oluşturmak için kullanabilirsiniz.

1. Bunlar, hazırlama adımı araçta çalıştırın.

    **TfsMigrator hazırlama/Collection: http:\//contosotfs:8080/tfs/ContosoDev /tenantDomainName:contosomigration.onmicrosoft.com /accountRegion:cus**

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

5. İmport.json dosyası içeri aktarma ayarları sağlar. Bu depolama hesabı bilgileri ve istenen kuruluş adı gibi bilgileri içerir. Alanların çoğu, otomatik olarak doldurulur. Bazı alanları kullanıcı girişi gerekli. Contoso dosyayı açar ve oluşturulacak Azure DevOps Hizmetleri kuruluş adını ekler: **contosodevmigration**. Bu ada sahip, Azure DevOps Hizmetleri URL'si olacaktır **contosodevmigration.visualstudio.com**.

    ![Hazırlama](./media/contoso-migration-tfs-vsts/prep5.png)

    > [!NOTE]
    > Kuruluş geçiş işleminden önce oluşturulmalıdır, geçiş tamamlandıktan sonra değiştirilebilir.

6. Bunlar, Azure DevOps hizmetlerine içeri aktarma sırasında kapsama dahil edilecektir hesaplarını gösterir kimlik günlük eşleme dosyasını inceleyin. 

   - Azure DevOps Hizmetleri kullanıcıları içeri aktarma işleminden sonra olacak kimlikleri etkin kimlikleri bakın.
   - Azure DevOps Hizmetleri bu kimlikleri lisanslanacağını ve geçişten sonra kuruluşunuzdaki bir kullanıcı olarak gösterilir.
   - Bu kimlikleri olarak işaretlenmiş **etkin** içinde **beklenen içeri aktarma durumu** dosyasındaki sütun.

     ![Hazırlama](./media/contoso-migration-tfs-vsts/prep6.png)



## <a name="step-5-migrate-to-azure-devops-services"></a>5. Adım: Azure DevOps Services’a geçiş

Yerinde hazırlıkla Contoso yöneticileri artık geçiş odaklanabilirsiniz. Geçiş çalıştırdıktan sonra sürüm denetimi için TFVC, Git kullanarak geçiş yaparsınız.

Başlamadan önce yöneticilerin koleksiyonu geçiş için çevrimdışı olması için geliştirme ekibi ile kapalı kalma süresi zamanlayın. Geçiş işlemi için adımlar şunlardır:

1. **Koleksiyon ayırma**: Koleksiyon eklenmiş ve çevrimiçi durumdayken kimlik verileri koleksiyonu için TFS sunucusu yapılandırma veritabanında yer alıyor. Koleksiyonu TFS sunucusundan ayrıldığında, bu kimlik verilerinin bir kopyasını alır ve taşıma için koleksiyonuyla paketler. Bu veriler olmadan kimlik bölümü alma yürütülemez. İçeri aktarma işlemi tamamlanana kadar içeri aktarma sırasında oluşan değişiklikleri almak için hiçbir yolu olmadığından koleksiyon ayrılmış kalın önerilir.
2. **Bir yedekleme oluşturmak**: Sonraki adım geçiş işlemi, Azure DevOps Hizmetleri içine aktarılabilen bir yedekleme oluşturmaktır. Veri katmanı uygulaması bileşen paketleri (DACPAC), veritabanı değişiklikleri tek bir dosya halinde paketlenmiş ve dağıtılan diğer SQL örneklerine izin veren bir SQL Server özelliğidir. Azure DevOps Hizmetleri doğrudan da geri yüklenebilir ve bu nedenle buluta koleksiyon verisi almak için paketleme yöntemi olarak kullanılır. Contoso, DACPAC oluşturmak için SqlPackage.exe Aracı'nı kullanır. Bu araç, SQL Server veri Araçları'nda bulunur.
3. **Depolama alanına yükleme**: DACPAC oluşturulduktan sonra kullanıcılar, Azure Depolama'ya yükler. Karşıya yüklendikten sonra bunlar bir paylaşılan erişim imzası (SAS) depolama TFS geçiş aracı erişmesine izin vermek için alın.
4. **Alma dolgu**: Ardından contoso DACPAC ayarı dahil olmak üzere içe aktarma dosyasındaki eksik alanları doldurabilirsiniz. Bunlar yapmak istediğiniz başlamak belirtirsiniz bir **prova** her şeyin tam geçişten önce düzgün şekilde çalıştığını denetlemek için içeri aktarma.
5. **Bir prova yapmak**: Yardım test koleksiyon geçişi çalıştırma kuru alır. Kuru çalıştırma ömrünü sınırlı ve üretim geçiş çalışmadan önce silindi. Otomatik olarak ayarlanmış bir süre sonra silinen. Başarı e-postada içeri aktarma tamamlandıktan sonra alınan prova zaman silinecek hakkında bir not dahildir. Not alın ve buna göre planlayın.
6. **Üretim Geçişi tamamlamak**: Prova geçişi tamamlandı Contoso yöneticileri import.json güncelleştiriliyor ve içeri aktarma yeniden çalıştırarak son geçiş yapın.



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

Contoso, Azure DevOps Hizmetleri içine almak için bir yedek (DACPAC) oluşturur.

- SQL Server veri Araçları'nda SqlPackage.exe DACPAC oluşturmak için kullanılır. Birden çok 120 ve 130 140 gibi adları olan klasörler altında bulunan SQL Server veri araçları ile birlikte yüklenen SqlPackage.exe sürümü vardır. DACPAC hazırlamak için doğru sürüm kullanılması önemlidir.
- TFS 2018'den içeri aktarmalar, 140 klasöründen veya üzeri SqlPackage.exe kullanmanız gerekebilir.  CONTOSOTFS için bu dosyayı klasöründe bulunur: **C:\Program Files (x86)\Microsoft Visual Studio\2017\Enterprise\Common7\IDE\Extensions\Microsoft\SQLDB\DAC\140**.


Contoso yöneticileri DACPAC şu şekilde oluşturun:

1. Bunlar, bir komut istemi açın ve SQLPackage.exe konumuna gidin. Bunlar, DACPAC oluşturmak için bu aşağıdaki komutu yazın:

    **SqlPackage.exe /sourceconnectionstring: "veri kaynağı SQLSERVERNAME\INSTANCENAME; = Initial Catalog = Tfs_ContosoDev; Integrated Security = True" /targetFile:C:\TFSMigrator\Tfs_ContosoDev.dacpac /action:extract /p:ExtractAllTableData = true /p: miyim gnoreUserLoginMappings true /p:IgnorePermissions = true /p:Storage = bellek =** 

    ![Backup](./media/contoso-migration-tfs-vsts/backup1.png)

2. Komut çalıştıktan sonra aşağıdaki ileti görüntülenir.

    ![Backup](./media/contoso-migration-tfs-vsts/backup2.png)

3. Bunlar DACPAC dosyasının özelliklerini doğrulayın

    ![Backup](./media/contoso-migration-tfs-vsts/backup3.png)

### <a name="update-the-file-to-storage"></a>Dosya depolama için güncelleştirme

Contoso DACPAC oluşturulduktan sonra Azure Depolama'ya yükler.

1. Bunlar [yükleyip](https://azure.microsoft.com/features/storage-explorer/) Azure Depolama Gezgini.

    ![Karşıya Yükle](./media/contoso-migration-tfs-vsts/backup5.png)

4. Aboneliğine bağlanın ve geçiş için oluşturduğu depolama hesabını bulun (**contosodevmigration**). Yeni bir blob kapsayıcısı, oluşturdukları **azuredevopsmigration**.

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

İmport.json dosyasını açın ve aşağıdaki alanları doldurun: • konumu: Yukarıda oluşturulan SAS anahtarını konumu.
• Dacpac: Depolama hesabına yüklediğiniz DACPAC dosyasının adı ayarlayın. ".Dacpac" uzantısına dahil etme.
• ImportType: Şimdilik prova için ayarlayın.


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

12. Azure DevOps hizmetlerine denetlemek için Contoso geliştirme liderleri işaretleri geçiş tamamlandıktan sonra prova düzgün bir şekilde çalıştı. Kimlik doğrulamasından sonra Azure DevOps Hizmetleri kuruluş onaylamak için birkaç ayrıntı gerekir.

    ![Prova](./media/contoso-migration-tfs-vsts/test8.png)

13. Azure DevOps Hizmetleri'nde Geliştirme lideri, Azure DevOps Hizmetleri projeleri geçirildiğini görebilirsiniz. Kuruluş 15 gün içinde silinecek bir bildirim yoktur.

    ![Prova](./media/contoso-migration-tfs-vsts/test9.png)

14. Geliştirme lideri projelerden birine ve açar **iş öğelerini** > **Bana atanan**. Bu iş öğesi verileri, kimlikle birlikte geçirildiğini gösterir.

    ![Prova](./media/contoso-migration-tfs-vsts/test10.png)

15. Müşteri adayı, başka proje ve kod, kaynak kodu ve geçmiş geçirildi olduğunu onaylamak için de denetler.

    ![Prova](./media/contoso-migration-tfs-vsts/test11.png)


### <a name="run-the-production-migration"></a>Üretim geçiş çalıştırma

Prova ile tam, Contoso yöneticileri için üretim geçiş geçin. Bunlar prova silmek, içeri aktarma ayarlarını güncelleştirmek ve içeri aktarma yeniden çalıştırın.

1. Azure DevOps Services Portalı'nda, bunlar prova kuruluş silin.
2. Ayarlanacak import.json dosyanın güncelleştirilmesi **ImportType** için **ProductionRun**.

    ![Üretim](./media/contoso-migration-tfs-vsts/full1.png)

3. Bunlar prova için yaptığınız gibi bunlar geçişi başlatın: **TfsMigrator alma /importFile:C:\TFSMigrator\import.json**.
4. Bir ileti geçişi onaylamak için gösterir ve verileri bir hazırlama alanı olarak güvenli bir konumda yedi güne kadar tutulabilirsiniz, sizi uyarır.

    ![Üretim](./media/contoso-migration-tfs-vsts/full2.png)

5. Azure AD oturum içinde bunlar bir Contoso Yöneticisi oturum açma belirtin.

    ![Üretim](./media/contoso-migration-tfs-vsts/full3.png)

6. Bir ileti alma hakkında bilgi gösterir.

    ![Üretim](./media/contoso-migration-tfs-vsts/full4.png)

7. Yaklaşık 15 dakika sonra bunlar URL'sine göz atın ve aşağıdaki bilgileri görür:

    ![Üretim](./media/contoso-migration-tfs-vsts/full5.png)

8. Geçiş tamamlandıktan sonra Contoso geliştirme neden geçiş düzgün şekilde çalıştığını denetlemek için Azure DevOps Hizmetleri'ni günlüğe kaydeder. Oturum açtıktan sonra projeler geçirildiğini görebilir.

    ![Üretim](./media/contoso-migration-tfs-vsts/full6.png)

8. Geliştirme lideri projelerden birine ve açar **iş öğelerini** > **Bana atanan**. Bu iş öğesi verileri, kimlikle birlikte geçirildiğini gösterir.

    ![Üretim](./media/contoso-migration-tfs-vsts/full7.png)

9. Müşteri adayı onaylamak için diğer iş öğesi verileri denetler.

    ![Üretim](./media/contoso-migration-tfs-vsts/full8.png)

15. Müşteri adayı, başka proje ve kod, kaynak kodu ve geçmiş geçirildi olduğunu onaylamak için de denetler.

    ![Üretim](./media/contoso-migration-tfs-vsts/full9.png)


### <a name="move-source-control-from-tfvc-to-git"></a>Kaynak denetimi TFVC'den GİT'e taşıma

Geçiş ile tamamlandı, Contoso için kaynak kodu Yönetimi TFVC'den Git'e taşımak istiyor. Bunlar, kaynak kodu şu anda Azure DevOps Hizmetleri kuruluş olarak Git depoları aynı kuruluşta aktarmanız gerekir.

1. Azure DevOps Services Portalı'nda, TFVC depoları birini açın (**$/ PolicyConnect**) ve gözden geçirin.

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

6. Kaynak gözden geçirdikten sonra müşteri adayları geliştirme Azure DevOps hizmetlere geçiş yapıldığını kabul etmiş olursunuz. Azure DevOps Hizmetleri artık geçişin ekiplerin içindeki tüm geliştirme kaynağı haline gelir.

    ![Git](./media/contoso-migration-tfs-vsts/git6.png)



**Daha fazla yardıma mı ihtiyacınız var?**

[Daha fazla bilgi edinin](https://docs.microsoft.com/azure/devops/repos/git/import-from-TFVC?view=vsts) TFVC'den içeri aktarma hakkında.

##  <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Tam geçişi ile Contoso şunları yapmanız gerekir:

- Gözden geçirme [sonrası içeri aktarma](https://docs.microsoft.com/azure/devops/articles/migration-post-import?view=vsts) makale ek alma etkinlikleri hakkında bilgi için.
- TFVC depoları silmek ya da bunlar salt okunur moduna alın. Kod tabanlarında gerekmez kullanılmıştır, ancak bunların geçmişini başvurulabilir.

## <a name="next-steps"></a>Sonraki adımlar

Contoso ilgili takım üyeleri için Azure DevOps Services ve Git eğitimi sağlamanız gerekir.



