---
title: Contoso uygulamasına geçirerek yeniden düzenleyin. Azure VM ve SQL Server AlwaysOn Kullanılabilirlik grubu için | Microsoft Docs
description: Nasıl Contoso yeniden barındırma şirket içi uygulama için bir Azure Container Web uygulaması ve bir Azure SQL Server veritabanına geçiş yaparak öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/05/2018
ms.author: raynew
ms.openlocfilehash: aeed26512be6cad5c22cdbb0ca19b1574049c0d4
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37872654"
---
# <a name="contoso-migration-refactor-an-on-premises-app-to-an-azure-web-app-and-azure-sql-database"></a>Contoso geçiş: bir şirket içi uygulamayı bir Azure Web uygulaması ve Azure SQL veritabanını yeniden düzenleme

Bu makalede, Contoso Azure SmartHotel uygulamasının nasıl yeniden düzenler gösterilmektedir. Bunlar, uygulama ön uç sanal makine bir Azure Web uygulaması ve uygulama veritabanına bir Azure SQL veritabanına geçirin.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarından bazılarını Microsoft Azure bulutuna nasıl geçirdiğini gösteren makaleler serisinin biridir. Seri arka plan bilgileri ve geçiş altyapı kurulumu, geçiş için şirket içi kaynaklarınızı değerlendirerek ve geçişleri farklı türlerde çalıştıran gösteren senaryolar içerir. Senaryoları, karmaşık hale gelmesi ve zaman içinde ek makaleleri ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
[3. makale: şirket içi kaynaklara değerlendirin](contoso-migration-assessment.md)  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği için bir uygulama barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: Azure sanal makinelerinde bir uygulamayı barındırma](contoso-migration-rehost-vm.md) | Contoso geçirme SmartHotel uygulama yalnızca Site RECOVERY'yi kullanarak VM'lerin nasıl gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve SQL Server Always On kullanılabilirlik grubu için bir uygulama barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makinelerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL sunucusu için bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
Makale 9: bir uygulamayı bir Azure Web uygulaması ve Azure SQL veritabanını yeniden düzenleme | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Bu makalede
[Makale 10: Azure Web Apps ve Azure MySQL için bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir


Bu makalede, iki katmanlı Windows Contoso geçirir. Azure'a VMware Vm'lerinde çalışan NET SmartHotel uygulaması. Bu uygulamayı kullanmak istiyorsanız, açık kaynak sağlanır ve buradan indirebileceğiniz [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, bu geçişle elde etmek istedikleri anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve şirket içi sistemler ve altyapı baskısı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.
- **Maliyetleri**: Contoso lisanslama maliyetlerini azaltmak istemektedir.

## <a name="migration-goals"></a>Geçiş hedefleri

Contoso bulut takım hedeflerini bu geçiş için aşağı sabitlenmiş. Bu hedefleri, en iyi geçiş yöntemini belirlemek için kullanıldı.

**Gereksinimleri** | **Ayrıntılar**
--- | --- 
**Uygulama** | Uygulamanızı Azure'a bugün olduğu gibi kritik olarak kalır.<br/><br/> Şu anda bir VMWare içinde olduğu gibi aynı performans özelliklerine sahip olmalıdır.<br/><br/> Uygulamada yatırım istemezsiniz. Şimdilik yalnızca buluta güvenli bir şekilde taşımak isterler.<br/><br/> Uygulama şu anda çalıştığı Windows Server 2008 R2 desteğini durduracak şekilde isterler.<br/><br/> SQL Server 2008 R2 uzağa Yönetimi gereksinimini en aza indirecek modern bir PaaS veritabanı platformu taşımak isterler.<br/><br/> Contoso istediğiniz mümkün olduğunda kendi SQL Server Lisanslama ve Yazılım Güvencesi yatırım yararlanın.<br/><br/> Ayrıca, Contoso istediğiniz tek web katmanındaki hata noktasını gidermek.
**Sınırlamalar** | Bir ASP.NET uygulaması ve aynı VM'de çalışan bir WCF hizmeti uygulaması oluşur. Bunlar bu Azure App Service kullanarak iki web uygulaması arasında bölmek isteyebilirsiniz. 
**Azure** | Uygulamayı Azure'a taşımak istediklerini söylüyor ancak Vm'lerinde çalıştırmak istemiyorsanız. Azure PaaS Hizmetleri web ve veri katmanları için yararlanmak isterler. 

## <a name="solution-design"></a>Çözüm tasarımı

Kendi hedefleri ve gereksinimleri sabitleme sonra Contoso tasarlar ve bir dağıtım çözümü gözden geçirin ve bunların geçiş için kullanacağınız Azure hizmetlerini de dahil olmak üzere geçiş işlemi belirler.

### <a name="current-app"></a>Geçerli uygulama

- SmartHotel şirket içi uygulama, iki VM arasında (WEBVM ve SQLVM) katmanlı.
- Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com** (sürüm 6.5)
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**), bir VM üzerinde çalışır.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'şirket içi etki alanı denetleyicisiyle (**contosodc1 adlı**).
- Geçiş tamamlandıktan sonra şirket içi Vm'leri Contoso veri merkezinde kullanımdan.


### <a name="proposed-solution"></a>Önerilen çözüm

- SQL Server kullanarak uygulama veritabanı katmanı için Contoso Azure SQL veritabanı karşılaştırıldığında [bu makalede](https://docs.microsoft.com/azure/sql-database/sql-database-features). Bunlar, Azure SQL veritabanı ile bazı nedenlerden dolayı karar verdim:
    - Azure SQL veritabanı bir ilişkisel veritabanı yönetilen hizmetidir. Bu, yönetime neredeyse hiç ile birden fazla hizmet düzeyinde öngörülebilir performans sunar. Hiç kapalı kalma süresi, yerleşik zeka iyileştirmesi ve global ölçeklenebilirlik ve kullanılabilirlik ile dinamik ölçeklenebilirlik sayılabilir.
    - Bunlar, basit bir Data Migration Yardımcısı (şirket içi veritabanını Azure SQL'e geçirme ve değerlendirmek için DMA) yararlanabilir.
    - Yazılım Güvencesi içeren SQL Server için Azure hibrit Avantajı'ı kullanarak bir SQL veritabanı, indirimli Fiyatlardan için mevcut lisanslarını gönderip alabilir. Bu değer % 30 tasarruf sağlayabilir.
    - SQL veritabanı, her zaman şifreli, dinamik veri maskeleme ve satır düzeyi güvenlik/tehdit algılama gibi güvenlik özellikleri sağlar.
- Uygulama web katmanı için bunlar Azure App Service kullanmaya karar verdiniz. Bu PaaS hizmeti, yalnızca birkaç yapılandırma değişikliğiyle uygulama dağıtmanıza olanak sağlar. Bunlar değişiklik yapmak için Visual Studio'yu kullanın ve iki web uygulaması dağıtın. Web sitesi için diğeri için WCF hizmeti.
  
### <a name="solution-review"></a>Çözümü gözden geçirme
Contoso, Artıları ve eksileri listesini birbirine koyarak önerilen tasarımlarına değerlendirir.

**Önemli noktalar** | **Ayrıntılar**
--- | ---
**Uzmanları** | SmartHotel uygulama kodu, Azure'a geçiş için değiştirilmesi gerekmez.<br/><br/> Bunlar, Yazılım Güvencesi'nın SQL Server ve Windows Server için Azure hibrit avantajı kullanılarak bir yatırım getirisi yararlanabilirsiniz.<br/><br/> Geçişten sonra kullanıcılar artık Windows Server 2008 R2 desteği gerekir. [Daha fazla bilgi edinin](https://support.microsoft.com/lifecycle).<br/><br/> Artık tek başarısızlık noktası değildir, bu nedenle web katmanı uygulamasının birden çok örnek ile yapılandırabilirsiniz.<br/><br/> Artık bağımlı eskime SQL Server 2008 R2 üzerinde olmaları.<br/><br/> SQL veritabanı teknik gereksinimler Contoso'nun destekler. Bunlar veritabanı geçiş Yardımcısı'nı kullanarak şirket içi veritabanı değerlendirilen ve uyumlu olduğunu belirledi.<br/><br/> SQL veritabanı, Contoso ayarlanan gerekmeyen yerleşik hata toleransı vardır. Bu veri katmanı artık tek bir yük devretme noktası olmasını sağlar.
**Simgeler** | Azure uygulama hizmetleri, her Web uygulaması için yalnızca bir uygulama dağıtımını destekler. Bu, iki Web uygulaması, sağlanan (bir Web sitesi için) ve WCF hizmeti için bir tane olması gerektiği anlamına gelir.<br/><br/> Bunlar veri geçiş Yardımcısı'nı veri geçiş hizmeti yerine kendi veritabanı için geçişi kullanırsanız, Contoso geçirme için hazır bir altyapı olmaz veritabanlarını ölçeğe. Bunlar birincil bölgeye kullanılamıyorsa yük devretme sağlamak için başka bir bölge oluşturmanız gerekir.

## <a name="proposed-architecture"></a>Önerilen mimarisi

![Senaryo mimarisi](media/contoso-migration-refactor-web-app-sql/architecture.png) 


### <a name="migration-process"></a>Geçiş işlemi

1. Contoso, bir Azure SQL örneği hazırlayacak ve SmartHotel veritabanını geçirmek.
2. Bunlar sağlama ve Web uygulamalarını yapılandırma ve SmartHotel uygulamayı dağıtırsınız.

    ![Geçiş işlemi](media/contoso-migration-refactor-web-app-sql/migration-process.png) 

### <a name="azure-services"></a>Azure hizmetleri

**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA, değerlendirmek ve bunların azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak için kullanın. DMA, SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir ve performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure SQL Veritabanı](https://azure.microsoft.com/services/sql-database/) | Akıllı, tam olarak yönetilen bir ilişkisel bulut veritabanı hizmeti. | Özellikler, aktarım hızı ve boyutuna bağlı olarak maliyet. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure uygulama Hizmetleri - Web uygulamaları](https://docs.microsoft.com/azure/app-service/app-service-web-overview) | Tam olarak yönetilen bir platform kullanarak güçlü bulut uygulamaları oluşturun | Boyut, konum ve kullanım süresine göre maliyeti. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/app-service/windows/).

## <a name="prerequisites"></a>Önkoşullar

Siz (ve Contoso) Bu senaryo çalıştırmak gerekenler şu şekildedir:

**Gereksinimleri** | **Ayrıntılar**
--- | ---
**Azure aboneliği** | Bu serideki ilk makaledeki değerlendirme gerçekleştirildiğinde aboneliği oluşturmuş olmanız. Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Mevcut bir abonelik kullanıyorsanız ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yöneticiyle birlikte çalışmanız gerekiyor.
**Azure altyapı** | [Bilgi nasıl](contoso-migration-infrastructure.md) Contoso Azure altyapısının ayarlayın.


## <a name="scenario-steps"></a>Senaryo adımları

Contoso geçişi nasıl çalışacağını aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: Azure SQL veritabanı örneğinde sağlama**: Contoso azure'da bir SQL örneği sağlar. WCF service web uygulaması, uygulama Web sitesi olan geçirdikten sonra Azure'a bu örneğine işaret edecek.
> * **2. adım: DMA veritabanıyla geçirme**: Bunlar veritabanı geçiş Yardımcısı'nı içeren uygulama veritabanını geçirme.
> * **3. adım: Sağlama Web Apps**: Bunlar iki web uygulaması sağlayın.
> * **4. adım: bağlantı dizelerini yapılandırma**: web katmanı web uygulaması, WCF service web uygulaması ve SQL örneğinin iletişim kurabilmesi için bağlantı dizelerini yapılandırırsınız.
> * **5. adım: web uygulamalarını yayımladığınızda**: son adım olarak, Contoso uygulamaları Azure'da yayımlar.


## <a name="step-1-provision-an-azure-sql-database"></a>1. adım: Azure SQL veritabanı sağlama

1. Azure'da bir SQL veritabanı oluşturmak için seçin. 

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql1.png)

2. Bunlar şirket içi VM'de çalışan veritabanıyla eşleşmesi için bir veritabanı adı belirtin (**SmartHotel.Registration**). Bunlar veritabanı ContosoRG kaynak grubuna yerleştirin. Bu, azure'daki kaynakları üretim için kullandıkları kaynak grubudur.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql2.png)

3. Yeni bir SQL Server örneğini ayarlama ayarladıkları (**sql smarthotel eus2**) birincil bölgedeki.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql3.png)

4. Fiyatlandırma katmanını sunucularına eşleştirmek için ayarladıkları ve veritabanı gerekir. Ve zaten sahip oldukları bir SQL Server lisansı için Azure hibrit avantajı ile tasarruf seçin.
5. Boyutlandırma için bunlar v çekirdek tabanlı satın alma kullanın ve beklenen gereksinimlerine için sınırları ayarlayın.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql4.png)

6. Ardından veritabanı örneği oluşturun.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql5.png)

7. Örneği oluşturulduktan sonra veritabanı ve geçiş için veritabanı geçiş Yardım kullandığınızda ihtiyaç duydukları ayrıntılarını not edin.

    ![SQL sağlama](media/contoso-migration-refactor-web-app-sql/provision-sql6.png)


**Daha fazla yardıma mı ihtiyacınız var?**

- [Yardım](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) bir SQL veritabanı sağlama.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) sanal çekirdek kaynak sınırları.


## <a name="step-2-migrate-the-database-with-dma"></a>2. adım: DMA veritabanını geçirme

Contoso DMA kullanarak SmartHotel veritabanı geçirir.

### <a name="install-dma"></a>DMA'yı yükleme

1. Bunlar aracı indirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595) şirket içi SQL Server VM (**SQLVM**).
2. Bunlar Kurulum (DownloadMigrationAssistant.msi) sanal makinede çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

### <a name="migrate-the-database-with-dma"></a>DMA veritabanını geçirme

1. Yeni bir proje DMA'da oluşturun (**SmartHotelDB**) seçip **geçiş** 
2. Kaynak sunucu türü seçmeleri **SQL Server**ve hedef olarak **Azure SQL veritabanı**. 

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-1.png)

3. Geçiş ayrıntıları ekledikleri **SQLVM** kaynak sunucu ve **SmartHotel.Registration** veritabanı. 

     ![DMA](media/contoso-migration-refactor-web-app-sql/dma-2.png)

4. Bunlar hangi kimlik doğrulaması ile ilişkili görünüyor bir hata alırsınız. Ancak araştırdıktan sonra veritabanı adı nokta (.) sorunudur. Geçici çözüm olarak, bunlar adını kullanarak yeni bir SQL veritabanını sağlamak karar **SmartHotel kayıt**, sorunu çözmek için. DMA'yı yeniden çalıştırın, bunlar seçebilir **SmartHotel kayıt**ve sihirbazıyla devam edin.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-3.png)

5. İçinde **nesneleri seçin**, veritabanı tablolarını seçin ve bir SQL betiği oluşturun.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-4.png)

6. DMS komut dosyasını oluşturduktan sonra'ı tıklatın **şemayı Dağıt**.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-5.png)

7. DMA, dağıtımın başarılı olduğunu onaylar.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-6.png)

8. Şimdi geçişi başlatın.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-7.png)

9. Geçiş tamamlandıktan sonra Contoso veritabanı Azure SQL örneğinde çalıştığını doğrulayabilirsiniz.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-8.png)

10. Bunlar ek SQL veritabanını silin **SmartHotel.Registration** Azure portalında.

    ![DMA](media/contoso-migration-refactor-web-app-sql/dma-9.png)


## <a name="step-3-provision-web-apps"></a>3. adım: Sağlama Web uygulamaları

Geçişi veritabanı ile iki web uygulamalarını Contoso can now sağlama.

1. Seçmeleri **Web uygulaması** portalında.

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app1.png)

2. Bunlar bir uygulama adı sağlayın (**SHWEB EUS2**), Windows üzerinde çalıştırma ve üretim kaynakları grubu kaldırma yerleştirin **ContosoRG**. Yeni app service ve planı oluştururlar.

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app2.png)

3. Web uygulaması sağlandıktan sonra WCF hizmeti için bir web uygulaması oluşturma işlemini yineleyin (**SHWCF EUS2**)

    ![Web uygulaması](media/contoso-migration-refactor-web-app-sql/web-app3.png)

4. İşiniz bittiğinde sonra başarıyla oluşturulan denetlemek için uygulamaları adresine göz atın.

## <a name="step-4-configure-connection-strings"></a>4. adım: bağlantı dizelerini yapılandırma

Contoso web apps emin olmak gerek duyduğu ve tüm veritabanı iletişim kurabilir. Bunu yapmak için bağlantı dizeleri kod ve web apps'te yapılandırın.

1. WCF hizmeti için web App'te (**SHWCF EUS2**) > **ayarları** > **uygulama ayarları**, bunlar adlı yeni bir bağlantı dizesi Ekle  **DefaultConnection**.
2. Bağlantı dizesi çekilen **SmartHotel kayıt** veritabanı ve doğru kimlik bilgileriyle güncelleştirilmelidir.

    ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/string1.png)

3. Visual Studio kullanarak, Contoso çözüm dosyasından açılır **SmartHotel360-iç-kayıt-apps** klasör. **ConnectionStrings** SmartHotel.Registration.Wcf bağlantı dizesi ile güncelleştirilmesi gerektiğini WCF hizmeti için web.config dosyasının bölümü.

     ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/string2.png)

4. **İstemci** SmartHotel.Registration.Web için web.config dosyasının WCF hizmeti yeni konumunu gösterecek şekilde değiştirilmesi. Hizmet uç noktasını barındıran WCF web uygulamasının URL'sini budur.

    ![Bağlantı dizesi](media/contoso-migration-refactor-web-app-sql/strings3.png)


## <a name="step-5-publish-web-apps"></a>5. adım: web uygulamaları yayımlama

Son adım olarak, Contoso web uygulamalarını Azure'da yayımlar.

1. Visual Studio'da, bunlar SmartHotel.REgistration.Wcf projeye sağ tıklayın > **Yayımla**.

    ![Yayımlama](media/contoso-migration-refactor-web-app-sql/publish-web1.png)

2. Bunlar, yayımlama başlatın.

    ![Yayımlama](media/contoso-migration-refactor-web-app-sql/publish-web2.png)

3. Web uygulaması oluşturmuş çünkü bunlar mevcut bir App Service seçin.

    ![Yayımlama](media/contoso-migration-refactor-web-app-sql/publish-web3.png)

4. Visual Studio, WCF uygulamayı seçin, sonra da dağıtır.

    ![Yayımlama](media/contoso-migration-refactor-web-app-sql/publish-web4.png)

5. Bunlar, ardından web uygulaması - SmartHotel.Registration.Web yayımlamak için işlemi tekrarlayın.

    ![Yayımlama](media/contoso-migration-refactor-web-app-sql/publish-web5.png)


Bu noktada, uygulamayı Azure'a başarıyla geçirildi.

## <a name="clean-up-after-migration"></a>Geçişten sonra Temizleme

Geçişten sonra Contoso temizleme adımları tamamlaması gerekir:  

- Şirket içi sanal makineleri, vCenter stok kaldırın.
- Sanal makinelerin yerel yedekleme işlerden kaldırın.
- SmartHotel uygulama için yeni konumlar göstermek için iç belgeleri güncelleştirin. Veritabanını Azure SQL veritabanı ve ön uç iki web uygulaması çalışıyor olarak çalışıyor olarak göster.
- Yetkisi alınan Vm'leri ile etkileşimde bulunan tüm kaynakları gözden geçirin ve herhangi bir ilgili ayarları veya belgeleri yeni yapılandırmayı yansıtacak şekilde güncelleştirin.


## <a name="review-the-deployment"></a>Dağıtım gözden geçirin

Azure'da geçirilen kaynakları ile tam olarak çalışır hale getirme ve yeni altyapılarını güvenli Contoso gerekir.

### <a name="security"></a>Güvenlik

- Contoso gereksinim emin olmak, yeni **SmartHotel kayıt** veritabanı güvenlidir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- Özellikle, web uygulamaları, SSL sertifikaları kullanacak şekilde güncelleştirmeniz gerekir.

### <a name="backups"></a>Yedeklemeler

- Contoso, Azure SQL veritabanı için yedekleme gereksinimlerini gözden geçirme gerekiyor. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Bunlar veritabanı için bölgesel yük devretme sağlamak için yük devretme grupları düşünmelisiniz. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Contoso Web uygulama ana Doğu ABD 2 ve orta ABD bölgesinde esnekliği için dağıtmayı göz önünde bulundurun gerekir. Bunlar, Traffic Manager'ın yük devretme bölgesel kesintiler durumunda emin olmak için yapılandırabilirsiniz.

### <a name="licensing-and-cost-optimization"></a>Lisanslama ve maliyet iyileştirme

- Tüm kaynaklar dağıtıldıktan sonra Contoso temel alan Azure etiketler atamasını kendi [Altyapı planlama](contoso-migration-infrastructure.md#set-up-tagging).
- Tüm lisans Contoso tüketiyor PaaS hizmetlerinin maliyetini içinde yerleşik olarak bulunur. Bu Kurumsal Anlaşma ' düşülür.
1. Contoso, Microsoft'un yan kuruluşu olan Cloudyn tarafından lisanslanan Azure maliyet Yönetimi olanak sağlar. Bu Azure ve diğer bulut kaynaklarını yönetmenize yardımcı olacak bir çoklu bulut maliyet yönetimi çözümüdür.  [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/cost-management/overview) Azure maliyet yönetimi hakkında. 

## <a name="conclusion"></a>Sonuç

Bu makalede, Contoso uygulama ön uç VM'nin iki Azure Web Apps'e geçiş yaparak Azure SmartHotel uygulamada yeniden düzenlenen. Uygulama veritabanı için Azure SQL veritabanına geçirilmesi.






