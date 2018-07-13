---
title: Contoso Azure'a geçiş için şirket içi iş yüklerini değerlendirme | Microsoft Docs
description: Contoso, şirket içi makineleri Azure geçişi ve veritabanı yükseltme ile Azure'a geçiş için nasıl değerlendirdiğini öğrenin
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: fa6fb4ffe1eea98392b2199f379431b0dffc6774
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006575"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Contoso geçiş: şirket içi iş yüklerini, Azure'a geçiş için değerlendirme

Bu makalede, Contoso, şirket içi SmartHotel uygulamasında, Azure'a geçiş için hazırlık nasıl değerlendirdiğini gösterilmektedir.

Bu belge, Contoso adlı kurgusal şirketin şirket içi kaynaklarını Microsoft Azure bulutuna nasıl geçirdiğini belge makaleler serisinin üçüncü değil. Seri arka plan bilgileri içerir ve bir geçiş altyapısını kurma nasıl çalışılacağını dağıtım senaryolarında bir dizi geçiş için şirket içi kaynaklara uygunluğunu değerlendirmek ve farklı türde geçiş çalıştırın. Senaryoları, karmaşık hale gelmesi ve diğer makaleler zamanla ekleyeceğiz.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
[Makale 1: genel bakış](contoso-migration-overview.md) | Contoso'nun geçiş stratejisi, makale dizisini ve kullandığımız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
[2. makale: bir Azure altyapısını dağıtma](contoso-migration-infrastructure.md) | Açıklayan nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar. Altyapıyı, tüm geçiş makaleleri için kullanılır. | Kullanılabilir
3. makale: şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme  | Contoso değerlendirme Wmware'de çalışan bir şirket içi iki katmanlı SmartHotel uygulamanın nasıl çalıştığını gösterir. Contoso uygulaması Vm'lerle değerlendirir [Azure geçişi](migrate-overview.md) hizmet ve uygulama SQL Server veritabanıyla [veritabanı geçiş Yardımcısı'nı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Bu makalede.
[4. makale: Azure sanal makineler ve yönetilen bir SQL örneği üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso lift-and-shift ile taşıma geçiş için Azure SmartHotel şirket içi uygulama için nasıl çalıştığını gösterir. Contoso VM ön uç uygulamasını kullanarak geçirir [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)ve SQL yönetilen örneği, uygulama veritabanını kullanarak [Azure veritabanı geçiş hizmeti](https://docs.microsoft.com/azure/dms/dms-overview). | Kullanılabilir
[Makale 5: bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Nasıl Contoso geçirme SmartHotel uygulama sanal makinelerini Site Recovery hizmetini kullanarak Azure Vm'leri için gösterir. | Kullanılabilir
[Makale 6: Azure sanal makineleri ve Always On kullanılabilirlik grubu SQL Server üzerinde bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso SmartHotel uygulamayı nasıl geçirdiğini gösterir. Contoso, uygulama sanal makinelerini ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site Recovery kullanır. | Kullanılabilir
[Makale 7: Azure sanal makineler'de Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Nasıl Contoso Linux osTicket uygulamayı lift-and-shift ile taşıma geçişini Azure Vm'leri için Site RECOVERY'yi kullanarak yaptığını gösterir. | Kullanılabilir
[Makale 8: Azure sanal makineler ve Azure MySQL üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin nasıl geçirdiğini gösterir ve uygulama veritabanı, MySQL Workbench kullanarak Azure MySQL Server örneğine geçirir. | Kullanılabilir
[Makale 9: bir uygulamayı Azure Web Apps ve Azure SQL veritabanında yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Nasıl Contoso SmartHotel uygulamayı bir Azure Web uygulamasına geçirir ve uygulama veritabanının Azure SQL Server örneğine geçirir gösterir | Kullanılabilir
[Makale 10: Azure Web Apps ve Azure MySQL üzerinde bir Linux uygulaması yeniden düzenleyin.](contoso-migration-refactor-linux-app-service-mysql.md) | Linux osTicket uygulaması Contoso birden çok sitede, GitHub ile sürekli teslim için tümleşik Azure Web Apps'e nasıl geçirdiğini gösterir. Bunlar, Azure MySQL örneğine uygulama veritabanına geçirin. | Kullanılabilir
[Makale 11: TFS VSTS üzerinde yeniden düzenleyin.](contoso-migration-tfs-vsts.md) | Geçiş yaparak Contoso şirket içi Team Foundation Server (TFS) dağıtımının nasıl geçirdiğini gösterir. Bunun için Visual Studio Team Services (VSTS) azure'da. | Kullanılabilir
[Makale 12: bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso geçirir ve Azure SmartHotel uygulamasının rearchitects nasıl gösterir. Bunlar, bir Windows kapsayıcısı ve bir Azure SQL veritabanı'nda uygulama veritabanı uygulama web katmanla yeniden oluşturma. | Kullanılabilir
[Makale 13: uygulamanızı Azure'a yeniden oluşturun.](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, uygulama hizmetleri, Azure Kubernetes, Azure işlevleri, Bilişsel hizmetler ve Cosmos DB dahil olmak üzere çeşitli kullanarak SmartHotel uygulamasının nasıl yeniden gösterir. | Kullanılabilir


## <a name="overview"></a>Genel Bakış

Azure'a geçiş olarak, Contoso şirketi, şirket içi iş yüklerini buluta geçiş için uygun olup olmadığını anlamak için teknik ve finansal değerlendirme çalıştırmak istiyor. Özellikle, Contoso takım geçiş için makine ve veritabanı uyumluluğunu değerlendirmek ve kapasite ve Azure'da kaynaklarını çalıştırma maliyetlerini tahmin etmek istersiniz.

Geçiriyor ve, iki şirket içi uygulamalarda değerlendirmek için ilerlediklerini, söz konusu teknolojileri daha iyi anlamak için aşağıdaki tabloda özetlenmiştir. Bunlar, yeniden barındırma ve yeniden düzenleme uygulamaları geçiş için geçiş senaryoları için değerlendirdiğiniz unutmayın. Yeniden barındırma ve içinde yeniden düzenleme hakkında daha fazla bilgi [Contoso geçişine genel bakış](contoso-migration-overview.md).

**Uygulama adı** | **Platform** | **Uygulama katmanları** | **Ayrıntılar**
--- | --- | --- | ---
SmartHotel<br/><br/> Contoso seyahat gereksinimlerini yönetir | Windows üzerinde çalışan SQL Server veritabanı ile | Bir VM (WEBVM) ve başka bir VM (SQLVM üzerinde) çalışan SQL Server üzerinde çalışan ön uç ASP.NET Web sitesi ile iki katmanlı uygulama | VMware vCenter sunucusu tarafından yönetilen bir ESXi ana bilgisayarı üzerinde çalışan, vm'leridir.<br/><br/> Örnek uygulamayı indirilebileceğini [GitHub](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Contoso hizmet Masası uygulaması | Linux/Apache, MySQL PHP (LAMP) ile çalışıyor. | Ön uç PHP Web sitesi üzerinde bir VM (OSTICKETWEB) ve başka bir VM (OSTICKETMYSQL üzerinde) çalışan bir MySQL veritabanı ile iki katmanlı uygulama | Uygulama tarafından müşteri hizmeti uygulamalarını çalışanları ve dış müşteriler için sorunları izlemek için kullanılır.<br/><br/> Örnek uygulamayı indirilebileceğini [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Geçerli mimari


İşte geçerli Contoso şirket içi altyapı gösteren diyagram.

![Contoso mimarisi](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso, şehir, New York'ta olarak Doğu ABD'de bulunan bir ana veri merkezinde sahiptir.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezinin fiber metro ethernet bağlantı (500 MB/sn) ile İnternet'e bağlı.
- Her dal, yerel olarak IPSec VPN tünelleri ana merkezine iş sınıfı bağlantıları kullanarak İnternet'e bağlı. Böylece, kalıcı olarak bağlanması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezinin VMware ile tam olarak sanallaştırılır. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma konaklarını sahiptirler.
- Contoso, kimlik yönetimi ve iç ağdaki DNS sunucuları için Active Directory kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.





## <a name="business-drivers"></a>İş sürücüleri

BT yönetim takımı, iş ile bu geçiş yapmanın istediği anlamak için iş ortaklarıyla yakından çalıştı:

- **Adres büyütmeye**: Contoso giderek ve sonuç olarak, şirket içi sistemler ve altyapı Basıncı yoktur.
- **Verimliliği artırmak**: Contoso gereken gereksiz yordamları kaldırıp, geliştiriciler ve kullanıcılar için süreçlerini kolaylaştırabilirsiniz.  Hızlı olmasını iş ihtiyaçlarınıza BT ve değil atık zaman ve para, bu nedenle müşteri gereksinimlerine daha hızlı teslim.
- **Çevikliği artırın**: Contoso BT işletme ihtiyaçlarını daha hızlı olması gerekir. Market'te genel ekonomi de başarılı etkinleştirmek için değişiklikleri daha hızlı tepki vermek mümkün olması gerekir.  Ulaşın veya bir iş engelleyici hale gerekmez.
- **Ölçek**: başarıyla büyüdükçe, Contoso BT aynı yükselmeye mümkün sistemlerini sağlamanız gerekir.

## <a name="assessment-goals"></a>Değerlendirme amaçları

Contoso bulut Takım hedeflerine, geçiş değerlendirmesi için aşağı sabitlenmiş:

- Bugün, şirket içi VMWare ortamlarında olduğu gibi geçişten sonra azure'da uygulama aynı performans özelliklerine sahip olmalıdır.  Buluta geçiş, uygulama performansını daha az kritik olduğundan gelmez.
- Contoso uygulamalarını ve veritabanlarını uyumluluğunu Azure barındırma seçeneklerini yanı sıra Azure gereksinimleri anlamak gerekir.
- Uygulamaları buluta taşıdıktan sonra Contoso'nun veritabanı yönetim en aza.  
- Contoso yalnızca geçiş seçenekleri yanı sıra buluta taşındıktan sonra altyapı ile ilişkili maliyetleri anlamak istiyor.

## <a name="assessment-tools"></a>Değerlendirme araçları
Contoso, Microsoft araçları değerlendirme için kullanıyor. Bu araçlar, kendi hedefleriyle aynı doğrultuda ilerlemesini ve bunları ihtiyaç duydukları tüm gerekli bilgileri sağlamanız gerekir.

**Teknoloji** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA, değerlendirmek ve bunların azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak için kullanın. DMA, SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir ve performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure Geçişi](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso, VMware Vm'lerini değerlendirmek için bu hizmeti kullanır. Makinelerin geçiş uygunluğunu değerlendirir ve Azure’da çalıştırmaya ilişkin boyutlandırma ve maliyet tahminleri sağlar.  | Yok 2018 için ücret kullanmakta bu hizmeti.
[Hizmet Eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Geçişi, geçirmek istediğiniz makineler arasındaki bağımlılıkları göstermek için Hizmet Eşlemesini kullanır. |  Hizmet Eşlemesi, Azure Log Analytics’in bir parçasıdır. Şu an ücretsiz olarak 180 gün kullanılabilir.

Bu senaryoda Contoso indirir ve seyahat uygulamalarına yönelik şirket içi SQL Server veritabanını değerlendirmek için DMA'yı çalıştırır. Azure kullandıkları uygulama sanal makinelerini azure'a geçiş işleminden önce değerlendirmek için bağımlılık eşlemesi ile geçirin.



## <a name="assessment-architecture"></a>Değerlendirmesi mimarisi


![Geçiş değerlendirmesi mimarisi](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso, tipik kurumsal kuruluş gösteren kurgusal bir addır.
- Contoso olan bir şirket içi veri merkezi (**contoso-datacenter**), şirket içi etki alanı denetleyicileriyle (contosodc1 adlı, CONTOSODC2).
- VMware Vm'lerini bir VMware ESXI üzerinde bulunan 6.5 sürümünü çalıştıran konaklar. Konaklar: **contosohost1**, **contosohost2**
- VMware ortamı 6.5 vCenter sunucusu tarafından yönetilir (**venter**bir VM üzerinde çalışıyor.
- SmartHotel seyahat uygulaması:
    - Uygulama, iki VMware Vm'sinde katmanlı **WEBVM** ve **SQLVM**.
    - Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com**.
    - SP1 ile Windows Server 2008 R2 Datacenter çalıştıran Vm'leri.
- VMware ortamı bir sanal makine üzerinde çalıştırılan vCenter Server (**vcenter.contoso.com**) tarafından yönetilir.
- OSTicket hizmet Masası Uygulaması:
    - Uygulama, iki VM arasında katmanlı **OSTICKETWEB** ve **OSTICKETMYSQL**.
    - VM'ler, Ubuntu Linux Server 16.04-LTS üzerinde çalışıyor.
    - Apache 2 ve PHP 7.0 OSTICKETWEB VM çalışıyor.
    - MySQL 5.7.22 OSTICKETMYSQL VM çalışıyor.

![Mimari](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Önkoşullar

Değerlendirme için Contoso (ve,) gerekenler şu şekildedir:

- Sahibi veya katkıda bulunan erişimi için Azure aboneliği veya Azure aboneliğindeki bir kaynak grubu.
- 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir şirket içi vCenter sunucusu.
- vCenter sunucusunda salt okunur bir hesap veya hesap oluşturma izinleri.
- .OVA şablonu kullanarak, vCenter sunucusu üzerinde sanal makine oluşturma izni.
- 5.0 veya üzeri bir sürümü çalıştıran en az bir ESXi ana bilgisayarı.
- Biri, SQL Server veritabanı çalıştıran en az iki şirket içi VMware sanal makinesi.
- Her sanal makinede Azure geçişi aracılarını yüklemek için izinler.
- Sanal makinelerin doğrudan İnternet bağlantısı olmalıdır.
        - İnternet erişimini, [gerekli URL’ler](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) ile sınırlayabilirsiniz.
        -Eğer internet bağlantısı olmayan makineleriniz [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) üzerlerinde yüklü olması gerekir.
- Veritabanı değerlendirmesi için SQL Server örneğini çalıştıran sanal makinenin FQDN’si.
- DMA’nın bağlanabilmesi için SQL Server sanal makinesinde çalışan Windows Güvenlik Duvarı, 1433 numaralı TCP bağlantı noktasında harici bağlantılara izin vermelidir.


## <a name="assessment-overview"></a>Değerlendirme genel bakış

Burada'nın değerlendirme yapmak için Contoso nasıl kolaylaştıracağını:


> [!div class="checklist"]
> * **1. adım: DMA'yı yükleyip**: şirket içi SQL Server veritabanının değerlendirmesi için DMA hazırlayın.
> * **2. adım: DMA veritabanıyla değerlendirme**: veritabanı değerlendirmesini analiz edin ve çalıştırın.
> * **3. adım: Azure geçişi ile sanal makine değerlendirmesine hazırlanma**: şirket içi hesapları ve VMware ayarlarını ayarlayın.
> * **4. adım: Azure geçişi ile şirket içi Vm'leri bulma**: bir Azure geçişi toplayıcısı sanal makine oluşturun. Daha sonra değerlendirmesi için Vm'leri bulmak için toplayıcıyı çalıştırın.
> * **5. adım: Azure geçişi ile bağımlılık Analizine hazırlanma**: Azure geçişi aracılarını yükleyin, VM'lerin VM'ler arasındaki bağımlılık eşlemesini görebilmeniz için.
> * **6. adım: Azure geçişi ile sanal makineleri değerlendirme**: bağımlılıkları denetleyin, sanal makineleri gruplayın ve değerlendirmeyi çalıştırın. Değerlendirme hazır olduktan sonra geçiş için hazırlık analiz edin.


## <a name="step-1-download-and-install-the-dma"></a>1. adım: DMA'yı yükleyip

1. Contoso nden DMA'yı yükler [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - Yardımcı bir SQL örneğine bağlanabilen herhangi bir makineye yüklenebilir. SQL Server makinesinde çalıştırmanız gerekmez.
    - Bu SQL Server ana bilgisayar makinesinde çalıştırmamalısınız.
2. Bunlar, indirilen kurulum dosyasına (yüklemeyi başlatmak için DownloadMigrationAssistant.msi), çalıştırın.
3. Üzerinde **son** seçtikleri sayfa **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>2. adım: Çalıştırma ve SmartHotel için veritabanı değerlendirmesini analiz etme

Artık Contoso SmartHotel uygulama için kendi şirket içi SQL Server analiz etmek için bir değerlendirme çalıştırabilirsiniz.

1. Veritabanı geçiş Yardımcısı'nda, bunlar tıklayın **yeni**seçin **değerlendirme**ve bir proje adı - değerlendirme verin **SmartHotel**.
2. Seçmeleri **kaynak sunucu türü** olarak **Azure Virtual Machines'de SQL Server**.

    ![Kaynak seçme](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Şu anda DMA, SQL Yönetilen Örneği’ne geçiş için değerlendirmeyi desteklemez. Geçici bir çözüm olarak, Contoso SQL Server Azure sanal makinesinde beklenen hedef olarak değerlendirme için kullanıyor.

3. İçinde **hedef sürümü seçin**, bunların SQL Server 2017 hedef sürümü seçin. Bunlar, SQL yönetilen örneği tarafından kullanılan sürümü olduğundan bu seçmeniz gerekir.
4. Uyumluluk ve yeni özellikler hakkında bilgi bulmak için seçin:
    - **Uyumluluk sorunlarını** geçişten veya geçişten önce küçük bir ayarlama gerektiren değişiklikleri unutmayın. Kullanımdan Kaldırılan olan özellikleri şu anda kullanımda haberdar kalmasını sağlar. Sorunlar, uyumluluk düzeyine göre düzenlenir.
    - **Yeni özellikler önerisi** geçişten sonra veritabanı için kullanılabilecek hedef SQL Server platformundaki yeni özellikleri notlar. Bunlar, Performansa, Güvenliğe ve Depolamaya göre düzenlenir.

    ![Hedef seçin](./media/contoso-migration-assessment/dma-assessment-2.png)

2. İçinde **sunucuya Bağlan**, bunlar erişmek için veritabanı ve kimlik bilgilerini çalıştıran VM adını girin. Etkinleştirmek ihtiyaç duydukları **sunucu sertifikasına güven** bunlar SQL Server'a alabilirsiniz emin olmak için. Bunlar ardından **Connect**.

    ![Hedef seçin](./media/contoso-migration-assessment/dma-assessment-3.png)

3. İçinde **kaynağı Ekle**, istedikleri değerlendirmek ve veritabanı ekledikleri **sonraki** değerlendirmeyi başlatmak için.
4. Değerlendirme oluşturulur.

    ![Değerlendirme oluşturma](./media/contoso-migration-assessment/dma-assessment-4.png)

5. İçinde **gözden geçirme sonuçlarının**, değerlendirme sonuçları görebilir.


### <a name="analyze-the-database-assessment"></a>Veritabanı değerlendirmesini analiz etme

Bunlara erişilebilir hemen sonra sonuçlar görüntülenir. Bunlar sorunları giderin, bunlar'ye tıklamanız **değerlendirmeyi yeniden Başlat** değerlendirmeyi yeniden çalıştırmak için.

1. İçinde **uyumluluk sorunlarını** raporu, her bir uyumluluk düzeyinde herhangi bir sorunu kontrol edin. Uyumluluk düzeyleri, SQL Server sürümleriyle aşağıdaki şekilde eşlenir:

    - 100: SQL Server 2008/Azure SQL Veritabanı
    - 110: SQL Server 2012/Azure SQL Veritabanı
    - 120: SQL Server 2014/Azure SQL Veritabanı
    - 130: SQL Server 2016/Azure SQL Veritabanı
    - 140: SQL Server 2017/Azure SQL Veritabanı

    ![Uyumluluk sorunları](./media/contoso-migration-assessment/dma-assessment-5.png)

2. İçinde **özellik önerileri** raporu için Contoso, değerlendirmenin geçişten sonra önerdiği performans, güvenlik ve depolama özelliklerini görüntüleyebilirsiniz. Çeşitli özellikler önerilir, bellek içi OLTP ve Columnstore, Stretch Database, Always Encrypted, dinamik veri maskeleme ve saydam veri şifrelemesi (TDE) dahil olmak üzere.

    ![Özellik önerileri](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Öneririz Contoso [TDE sağlayan](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) tüm SQL Server veritabanları ve bu olduğunda daha da kritik bulutta veritabanlarıdır. TDE, yalnızca geçişten sonra etkinleştirilmelidir. TDE zaten etkin değilse, sertifika veya asimetrik anahtar hedef sunucunun ana veritabanına taşımanız gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Bunlar, değerlendirme JSON veya CSV biçiminde dışarı aktarabilirsiniz.

Büyük ölçekli bir değerlendirme çalıştırıyorsanız, erişebildiğiniz dikkat edin:

- Aynı anda birden fazla değerlendirme çalıştırılması ve açarak değerlendirmelerin durumunu görüntülemek **tüm değerlendirmeler** sayfası.
- [Değerlendirmeleri bir SQL Server veritabanında birleştirebilirsiniz](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Değerlendirmeleri bir Powerbı raporunda birleştirebilirsiniz](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>3. adım: Azure geçişi ile sanal makine değerlendirmesine hazırlanma

Contoso Azure geçişi kullanan otomatik olarak sanal makineleri değerlendirme için keşfetmek, bir VM oluşturmak için izinleri doğrulamak için bir VMware hesabı oluşturmak için açılması gereken bağlantı noktalarını not edin ve istatistik ayarlarının düzeyini ayarlayın.

### <a name="set-up-a-vmware-account"></a>VMware hesabı ayarlama

 VM bulma aşağıdaki özelliklerle vcenter'da salt okunur bir hesap gerektirir:

- Kullanıcı türü: En azından salt okunur bir kullanıcı.
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur.
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

### <a name="verify-permissions-to-create-a-vm"></a>Sanal makine oluşturma izinlerini doğrulama

Contoso, bir dosyada içeri aktararak VM oluşturma iznine sahip doğrular. OVA biçimi. [Daha fazla bilgi edinin](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Bağlantı noktalarını doğrulama

Contoso değerlendirme bağımlılık eşlemesini kullanır. Bu özellik, değerlendirmek istediğiniz Vm'lere yüklü bir Aracısı gerektirir. Aracının her VM'de, 443 numaralı TCP bağlantı noktasından Azure'a bağlanmanız gerekir. Bağlantı gereksinimleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).


### <a name="set-statistics-settings"></a>İstatistik ayarlarını yapma

Contoso, dağıtıma başlamadan önce vCenter Server için istatistik ayarları düzeyini 3 ayarlamanız gerekir. Şunlara dikkat edin:

- Düzeyi ayarladıktan sonra en az bir gün değerlendirmeyi çalıştırmadan önce beklemeniz gerekir. Aksi takdirde beklendiği gibi çalışmayabilir.
- Düzey, 3’ten yüksekse değerlendirme çalışır ancak:
    - Diskler ve ağ iletişimi için performans verileri toplanmaz.
    - Depolama için Azure Geçişi, Azure’da şirket içi diskle aynı boyutta standart bir disk önerir.
    - Ağ iletişimi için, her şirket içi ağ bağdaştırıcısına yönelik olarak Azure’da bir ağ bağdaştırıcısı önerilir.
    - İşlem için Azure Geçişi, sanal makine çekirdeklerine ve bellek boyutuna bakar ve aynı yapılandırmaya sahip bir Azure sanal makinesi önerir. Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.
- Düzey 3 ile boyutlandırma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Bunlar düzeyi şu şekilde ayarlayın:

1. VSphere Web istemcisi, vCenter sunucu örneğini açın.
2. İçinde **Yönet** > **ayarları** > **genel**, simgeye **Düzenle**.
3. İçinde **istatistikleri**, bunlar istatistik düzeyi ayarları ayarlamak **3. düzey**.

    ![vCenter istatistikleri düzeyi](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>4. adım: Vm'leri bulma

Vm'leri bulmak için Azure geçişi projesini Contoso oluşturur. Bunlar indirin ve Toplayıcı sanal Makinesini ayarlama ve şirket içi Vm'leri bulmak için toplayıcıyı çalıştırın.

### <a name="create-a-project"></a>Proje oluşturma

1. İçinde [Azure portalı](https://portal.azure.com), bunlar için arama **Azure geçişi**ve (ContosoMigration) bir proje oluşturun.
2. Azure aboneliği, bir proje adı belirtin ve yeni bir Azure kaynak grubu oluşturma **ContosoFailoverRG**. Şunlara dikkat edin:

    - Bir Azure Geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz.
    - Herhangi bir hedef konumdan geçiş planlayabilirsiniz.
    - Proje konumu yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır.

    ![Azure Geçişi](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için Contoso indiren bir. OVA şablon ve VM'yi oluşturmak için şirket içi vCenter sunucusuna aktarır.

1. Azure geçişi projesinde > **Başlarken** > **Keşif ve değerlendirme** > **makineleri Bul**, bunlar indirin. OVA şablon dosyası.
2. Bunlar, proje Kimliğini ve anahtarını kopyalayın. Bunlar, Toplayıcı yapılandırmak için gereklidir.

    ![.ova dosyasını indir](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Sanal Makineyi dağıtmadan önce Contoso denetler. OVA dosyasını güvenlidir.

1. Dosyayı indirdikten makine üzerinde bir yönetici komut penceresi açın.
2. Bunlar OVA'ın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara (sürüm 1.0.9.12) uygun olmalıdır

**Algoritma** | **Karma değeri**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

### <a name="create-the-collector-appliance"></a>Toplayıcı gereci oluşturma

Artık Contoso indirilen dosyayı vCenter Server'a aktarın ve yapılandırma sunucusu sanal makine sağlayın.

1. VSphere Client konsolunda bunların tıklayın **dosya** > **OVF şablonu Dağıt**.

    ![OVF dağıtma](./media/contoso-migration-assessment/vcenter-wizard.png)

2. OVF şablonu Dağıtma Sihirbazı > **kaynak**, bunlar konumunu belirtin. OVA dosyasını.
3. İçinde **adını ve konumunu**, Toplayıcı VM için bir kolay ad ve hangi VM'nin barındırılacağı Envanter konumunu belirtin. Ayrıca konak veya küme Toplayıcı gerecini çalışacağı belirler.
5. İçinde **depolama**, bunlar depolama konumu belirtin ve **Disk biçimi**, depolama sağlamak istedikleri nasıl.
7. İçinde **ağ eşlemesi**, bunlar Toplayıcı VM'nin bağlanacağı ağı belirtin. Meta verileri Azure’a göndermek için ağ, İnternet bağlantısına sahip olmalıdır.
8. Ayarları gözden geçirin ve seçin **dağıtımdan sonra Aç**> **son**. Gereç oluşturulduktan sonra tamamlamanın başarılı olduğunu onaylayan bir ileti görüntülenir.

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Şimdi Vm'leri bulmak için toplayıcıyı çalıştırın. Toplayıcı şu anda yalnızca "İngilizce (ABD)" işletim sistemi dili ve Toplayıcı arabirimi dili desteklediğini unutmayın.

1. VSphere istemcisi Konsolu > **konsolu aç**, dil, saat dilimi ve parola tercihlerini Toplayıcı VM için belirtin.
2. Bunlar masaüstünde **Toplayıcı Çalıştır** kısayol.

    ![Toplayıcı kısayolu](./media/contoso-migration-assessment/collector-shortcut.png)

4. Azure geçişi Toplayıcısı'nda > **önkoşulları ayarlama**, lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
5. Toplayıcı VM'nin internet erişimi, zaman eşitlenir ve Toplayıcı hizmetinin çalışıp çalışmadığını olduğunu denetler. (Bu VM üzerinde varsayılan olarak yüklenir). VMWare Powerclı'yi de yükler.

    > [!NOTE]
    > Sanal makinenin, ara sunucu olmadan İnternet’e doğrudan erişiminin olduğunu varsayıyoruz.

    ![Önkoşulları doğrulama](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. İçinde **vCenter Server ayrıntılarını belirtin**adını (FQDN) veya vCenter sunucusunun IP adresini belirtin ve salt okunur kimlik bilgilerini, bulma için kullanılır.
7. Bunlar, VM bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam en fazla 1500 VM’yi içermelidir.

    ![vCenter’a bağlanma](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. İçinde **geçişi projesini belirtin**, Azure geçişi proje Kimliğini ve portalından kopyalandığından anahtarı belirtin. Yeniden projede edinebilirsiniz **genel bakış** sayfası > **makineleri Bul**.  

    ![Azure'a Bağlanma](./media/contoso-migration-assessment/collector-connect-azure.png)

7. İçinde **toplama durumunu görüntüle** Contoso bulma işlemini izleyin ve Vm'lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

    ![Koleksiyon işlemi sürüyor](./media/contoso-migration-assessment/collector-collection-process.png)



### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Koleksiyon tamamlandıktan sonra Contoso sanal makinelerin portalda görüntülenip görüntülenmediğini denetler.

1. Azure geçişi projesinde > **Yönet** > **makineler**, bulmak istediğiniz VM'lerin görüntülenip görüntülenmediğini denetleyin.

    ![Bulunan makineler](./media/contoso-migration-assessment/discovery-complete.png)

3. Makinelerde şu anda Azure Geçişi aracılarının yüklü olmadığını unutmayın. Contoso bunları görüntülemek için bağımlılıkları yüklemeniz gerekir.

    ![Bulunan makineler](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>5. adım: bağımlılık Analizine hazırlanma

Contoso erişmek istediğiniz sanal makineler arasındaki bağımlılıkları görüntülemek için indirme ve uygulama sanal makinelerini aracıları yükleyin. Contoso tüm sanal makineler hem Windows hem de Linux uygulamaları için bunu yapar.

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Bunlar, aracıları yüklemeden önce anlık görüntüsünü alarak değiştirmeden önce sanal makinenin bir kopyasını tutun.

![Makine anlık görüntüsü](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme

1. Üzerinde **makineler** sayfasında, bunlar makine seçin ve ardından **yüklemesi gerektirir** içinde **bağımlılıkları** sütun.
2. Üzerinde **makineleri Bul** sayfasında, şunları yapın:
    - Her bir Windows VM için MMA ve bağımlılık Aracısı'nı indirme
    - Her bir Linux VM için MMA ve bağımlılık Aracısı'nı indirme
3. Artık, çalışma alanı kimliği ve anahtarını kopyalayın. Bunlar bu MMA'yı yüklerken gerekir.

    ![Aracıyı indirme](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Windows sanal makinelerinde aracıları yükleyin

Bunlar, her sanal makinede yüklemeyi çalıştırın.

#### <a name="install-the-mma-on-windows-vms"></a>Windows Vm'lerinde MMA'yı yükleme

1. Bunlar, indirilen aracıya çift tıklayın.
2. İçinde **hedef klasör**, bunlar varsayılan yükleme klasörünü tutun > **sonraki**.
2. İçinde **Aracı Kurulum Seçenekleri**, seçtikleri **aracıyı Azure Log Analytics'e bağlama** > **sonraki**.

    ![MMA yüklemesi](./media/contoso-migration-assessment/mma-install.png)

5. İçinde **Azure Log Analytics**, bunlar çalışma alanı kimliği ve portaldan kopyaladığınız anahtarını yapıştırın.

    ![MMA yüklemesi](./media/contoso-migration-assessment/mma-install2.png)

6. İçinde **yüklemeye hazır**, şimdi MMA'yı yükleyebilirsiniz.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Windows Vm'lerinde bağımlılık aracısını yükleme

1. Bunlar, indirilen bağımlılık aracısına çift tıklayın.
2. Bunlar, lisans koşullarını kabul edin ve yüklemenin bitmesini bekleyin.

    ![Bağımlılık aracısı](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Linux Vm'lerinde aracıları yükleyin

Bunlar, her sanal makinede yüklemeyi çalıştırın.

#### <a name="install-the-mma-on-linux-vms"></a>Linux Vm'lerinde MMA'yı yükleme

1. Her VM kullanarak python ctypes kitaplığı yükledikleri: **sudo apt-get yükleme python ctypeslib**.
2. Bunlar, kök olarak MMA aracısını yüklemek için komutu çalıştırmanız gerekir.  Kök çalışma olmak için aşağıdaki komutu ve kök parolayı girin: **sudo -i**.
3. Artık MMA aracısını yükleyin.
    - Komutu, doğru çalışma alanı kimliği ve anahtarı ekleyin.
    - 64-bit komutlardır.
    - **Çalışma alanı kimliği** ve **birincil anahtar** içinde OMS portalında bulunabilir > **ayarları**, **bağlı kaynaklar** sekmesi.
    - OMS Aracısı'nı indirme, sağlama ve yükleme/ekleme Aracısı doğrulamak için aşağıdaki komutları çalıştırın.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```



#### <a name="install-the-dependency-agent-on-linux-vms"></a>Linux Vm'lerinde bağımlılık aracısını yükleme

MMA'yı yükledikten sonra Contoso Linux Vm'lerinde bağımlılık Aracısı'nı yükleyebilirsiniz.

1. Bağımlılık Aracısı'nı InstallDependencyAgent Linux64.bin, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği kullanarak Linux bilgisayarlarda yüklü. Bunlar sh kullanarak dosyayı çalıştırın veya ekleme dosyaya izinleri yürütün.

2. Bunlar, kök olarak Linux bağımlılık Aracısı'nı yükleyin:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>6. adım: Çalıştırma ve sanal makine değerlendirmesini analiz etme

Contoso, artık makine bağımlılıklarını doğrulayın ve bir grup oluşturun. Daha sonra grup için değerlendirme çalıştırın.

### <a name="verify-dependencies-and-create-a-group"></a>Bağımlılıkları doğrulayın ve bir grup oluşturun


1. Bunlar makineleri analiz etmek tıklayın **bağımlılıklarını görüntüleme**.

    ![Makine bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. SQLVM için bağımlılık eşlemi, aşağıdaki ayrıntıları gösterir:

    - Belirtilen süre (varsayılan olarak 1 saat) boyunca SQLVM’de çalıştırılan etkin ağ bağlantılarıyla grupları/işlemleri işleme
    - Tüm bağımlı makinelere/makinelerden gelen (istemci) ve giden (sunucu) TCP bağlantıları.
    - Azure Geçişi aracılarının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - Aracıların yüklü olmadığı makineler, bağlantı noktası ve IP adresi bilgilerini gösterir.

3. Aracısının (WEBVM) yüklü olduğu makineler için FQDN, işletim sistemi ve MAC adresi gibi daha fazla bilgi görüntülemek için makine kutusuna tıklayın.

    ![Grup bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Şimdi (SQLVM ve WEBVM) grubuna eklemek için sanal makineleri seçin.  CTRL tuşunu basılı tutup birden çok VM seçmek için tıklayın.
5. Simgeye **Grup Oluştur**ve bir ad (smarthotelapp) belirtin.

> [!NOTE]
    > Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını genişletebilirsiniz. Belirli bir süre veya başlangıç ve bitiş tarihleri seçebilirsiniz.


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma


1. Üzerinde **grupları** sayfasında grubu (smarthotelapp) açın ve tıklayın **Değerlendirme Oluştur**.

    ![Değerlendirme oluşturma](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Değerlendirme, **Yönet** > **Değerlendirmeler** sayfasında görüntülenir.

Contoso varsayılan değerlendirme ayarlarını kullanılan, ancak bu ayarları özelleştirebilirsiniz. [Daha fazla bilgi edinin](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>Sanal makine değerlendirmesini analiz etme

Azure geçişi değerlendirmesi bilgiler içeren Azure için şirket içi Vm'leri uyumluluğu hakkında doğru boyutlandırma Azure VM için önerilen ve tahmini aylık Azure maliyetleri.

![Değerlendirme raporu](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

![Değerlendirme görüntüsü](./media/contoso-migration-assessment/assessment-display.png)

Değerlendirme güvenilirlik derecelendirmesi, 1 yıldızdan 5 Yıldıza kadar (1 yıldız en düşük ve en yüksek olacak 5 yıldız) alır.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Derecelendirme, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi, yaptığınızda kullanışlıdır *performansa dayalı boyutlandırma* Azure geçişi kullanım tabanlı boyutlandırma gerçekleştirmek için yeterli veri noktası sahip. Azure Geçişi, VM’yi boyutlandırmak için ihtiyaç duyduğu tüm veri noktalarına sahip olduğundan, *şirket içi boyutlandırma* için güvenilirlik derecesi her zaman 5 yıldızdır.
- Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

#### <a name="verify-readiness"></a>Hazır olma durumu doğrulaması

![Hazır olma durumu değerlendirmesi](./media/contoso-migration-assessment/azure-readiness.png)  

Değerlendirme raporu, tabloda özetlenen bilgileri gösterir. Performans tabanlı boyutlandırmayı göstermek için Azure Geçişi’nin aşağıdaki bilgilere ihtiyaç duyduğunu unutmayın. Bu bilgiler toplanamıyorsa, boyutlandırma değerlendirmesi doğru olmayabilir.

- CPU ve bellek kullanımı verileri.
- Sanal makineye bağlı her disk için okuma/yazma IOPS ve aktarım hızı.
- Sanal makineye bağlı her ağ bağdaştırıcısı için ağa gelen/ağdan giden bilgiler.


**Ayar** | **Gösterge** | **Ayrıntılar**
--- | --- | ---
**Azure sanal makinesinin hazır olma durumu** | Sanal makinenin geçiş için hazır olup olmadığını belirtir | Olası durumlar:</br><br/>- Azure için hazır<br/><br/>- Koşullarla hazır <br/><br/>- Azure için hazır değil<br/><br/>- Hazır olma durumu bilinmiyor<br/><br/> Bir sanal makinenin hazır olmaması durumunda uygulanacak bazı düzeltme adımlarını göstereceğiz.
**Azure sanal makinesi boyutu** | Hazır sanal makineler için bir Azure sanal makinesi boyutu öneririz. | Boyutlandırma önerisi, değerlendirme özelliklerine bağlıdır:<br/><br/>- Performans tabanlı boyutlandırma kullandıysanız boyutlandırma, sanal makinelerin performans geçmişini dikkate alır.<br/><br/>- 'Şirket içi olarak' seçeneğini kullandıysanız boyutlandırma, şirket içi sanal makine boyutunu temel alır ve kullanım verileri kullanılmaz.
**Önerilen araç** | Makinelerimiz aracıları çalıştırdığından Azure Geçişi, makinenin içinde çalışan işlemlere bakar ve makinenin bir veritabanı makinesi olup olmadığını belirler.
**Sanal makine bilgileri** | Raporda, işletim sistemi, önyükleme türü, disk ve depolama bilgileri gibi şirket içi sanal makine ayarları gösterilir.


#### <a name="review-monthly-cost-estimates"></a>Aylık maliyet tahminlerini gözden geçirme

Bu görünümde, Azure’da çalışan sanal makinelerin toplam işlem ve depolama maliyetinin yanı sıra her makineye ilişkin ayrıntılar da görüntülenir.

![Hazır olma durumu değerlendirmesi](./media/contoso-migration-assessment/azure-costs.png)

- Maliyet tahminleri, makine için boyut önerileri kullanılarak hesaplanır.
- İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir.


## <a name="clean-up-after-assessment"></a>Değerlendirme sonrasında Temizle

- Değerlendirme tamamlandıktan sonra Azure geçişi gereç gelecekteki değerlendirmeleri için Contoso korur.
- Bunlar, VM VMware devre dışı bırakın. Bunlar ek VM'ler değerlendirirken, yeniden başlayacağız.
- Bunlar, Contoso geçiş projesinin'ı Azure'da saklayacağız.  Şu anda Doğu ABD bölgesinde Azure ContosoFailoverRG kaynak grubundaki dağıtılır.
-  Toplayıcı sanal makinesi 180 günlük değerlendirme lisansa sahip. Bu sınırı süresi dolarsa indirip Toplayıcı yeniden kurmanız gerekir.


## <a name="conclusion"></a>Sonuç

Bu senaryoda Contoso DMA Aracı'nı ve Azure geçişi hizmetini kullanarak şirket içi Vm'leri kullanarak SmartHotel uygulama veritabanını değerlendirdik. Bunlar daha sonra şirket içi kaynaklarınızı Azure'a geçiş için hazır olduğundan emin olmak için değerlendirmeleri gözden geçirdik.

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makalede, Azure SmartHotel, uygulamada lift-and-shift ile taşıma geçiş Contoso rehosts. Contoso, Azure Site Recovery ve bir Azure SQL veritabanı geçiş hizmetini kullanarak yönetilen örneğe, uygulama veritabanını kullanarak bir uygulama için ön uç WEBVM geçirir. [Başlama](contoso-migration-rehost-vm-sql-managed-instance.md) bu dağıtımla.
