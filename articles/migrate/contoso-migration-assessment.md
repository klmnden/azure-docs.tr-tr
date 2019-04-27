---
title: Contoso Azure'a geçiş için şirket içi iş yüklerini değerlendirme | Microsoft Docs
description: Azure geçişi ve Data Migration Yardımcısı'nı kullanarak, şirket içi makinelerin Azure'a geçiş için Contoso nasıl değerlendirdiğini öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 01/30/2019
ms.author: raynew
ms.openlocfilehash: 4739308d301291bf88e8ae547ba85f9648339c4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681105"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Contoso geçişi: Şirket içi iş yüklerini, Azure’a geçişe yönelik olarak değerlendirme

Bu makalede, Contoso, şirket içi SmartHotel360 uygulamayı Azure'a geçiş için değerlendirir.

Bu makalede, Contoso adlı kurgusal şirketin şirket içi kaynaklarını Microsoft Azure bulutuna nasıl geçirdiğini belgeleri bir serinin bir parçasıdır. Seri arka plan bilgileri içerir ve bir geçiş altyapısını kurma nasıl çalışılacağını ayrıntılı dağıtım senaryoları geçiş için şirket içi kaynaklara uygunluğunu değerlendirmek ve farklı türde geçiş çalıştırın. Senaryoları, karmaşık hale gelmesi. Makaleler, zaman içinde serinin eklenir.

Makale | Ayrıntılar | Durum
--- | --- | ---
[1. makale: Genel bakış](contoso-migration-overview.md) | Makale serisi, Contoso'nun geçiş stratejisi ve dizisinde kullanılan örnek uygulamalar genel bakış. | Kullanılabilir
[2. makale: Bir Azure altyapısı dağıtma](contoso-migration-infrastructure.md) | Contoso şirket içi altyapısını ve Azure altyapısını geçiş için hazırlar. Aynı altyapı serisi içinde tüm makaleler için kullanılır. | Kullanılabilir
3. makale: Şirket içi kaynaklarınızı Azure'a geçiş için değerlendirme | Contoso, Vmware'de çalıştırılan şirket içi SmartHotel360 uygulamasının bir değerlendirme çalışır. Contoso Azure geçişi hizmeti ve veri geçiş Yardımcısı'nı kullanarak uygulama SQL Server veritabanı kullanarak uygulama Vm'leri değerlendirir. | Bu makalede
[4. makale: Bir Azure VM ve SQL veritabanı yönetilen örneği bir uygulamada barındırma](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso, Azure'a lift-and-shift ile taşıma geçiş için kendi şirket içi SmartHotel360 uygulaması çalışır. Bu, Azure Site Recovery hizmetini kullanarak ön uç uygulama geçirir. Bu uygulama veritabanını Azure SQL veritabanı yönetilen Azure veritabanı geçiş hizmeti örneğine geçirir. | Kullanılabilir
[Makale 5: Bir uygulamayı Azure vm'lerinde yeniden barındırma](contoso-migration-rehost-vm.md) | Contoso, SmartHotel360 uygulama sanal makinelerini Azure Site Recovery hizmetini kullanarak sanal makineleri geçirir. | Kullanılabilir
[Makale 6: Azure sanal makinelerinde ve SQL Server AlwaysOn Kullanılabilirlik grubuna bir uygulamayı barındırma](contoso-migration-rehost-vm-sql-ag.md) | Contoso uygulaması Vm'leri ve veritabanı geçiş hizmeti uygulama veritabanı AlwaysOn Kullanılabilirlik grubu tarafından korunan bir SQL Server kümesine geçirmek için geçirmek için Site RECOVERY'yi kullanarak SmartHotel360 uygulama geçirir. | Kullanılabilir
[Makale 7: Azure vm'lerinde Linux uygulaması barındırma](contoso-migration-rehost-linux-vm.md) | Contoso Azure vm'lerine, Site Recovery hizmetini kullanarak kendi Linux osTicket uygulamasının lift-and-shift ile taşıma geçiş tamamlanır. | Kullanılabilir
[Makale 8: MySQL için Azure sanal makineler ve Azure veritabanı üzerinde bir Linux uygulaması barındırma](contoso-migration-rehost-linux-vm-mysql.md) | Contoso, Linux osTicket uygulaması için Azure Site RECOVERY'yi kullanarak VM'lerin geçirir. Bu uygulama veritabanı MySQL Workbench kullanarak MySQL için Azure veritabanı'na geçirir. | Kullanılabilir
[Makale 9: Bir Azure web uygulaması ve Azure SQL veritabanı içinde bir uygulamayı yeniden düzenleme](contoso-migration-refactor-web-app-sql.md) | Contoso, SmartHotel360 uygulama için bir Azure web uygulaması geçirir ve uygulama veritabanı için veritabanı geçiş Yardımcısı'nı kullanarak bir Azure SQL Server örneği geçirir. | Kullanılabilir
[Makale 10: MySQL için bir Azure web uygulaması ve Azure veritabanı'nda bir Linux uygulama yeniden düzenleyin](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso, bir Azure web uygulamasına GitHub ile sürekli teslim için tümleşik Azure Traffic Manager'ı kullanarak birden fazla Azure bölgesini üzerinde kendi Linux osTicket uygulaması geçirir. Contoso uygulaması veritabanı örneği MySQL için Azure veritabanı geçirir. | Kullanılabilir
[11. makale: Azure DevOps Hizmetleri Team Foundation Server'da yeniden düzenleyin](contoso-migration-tfs-vsts.md) | Contoso, Azure DevOps Hizmetleri azure'da, şirket içi Team Foundation Server dağıtımı geçirir. | Kullanılabilir
[Makale 12: Bir uygulamayı Azure kapsayıcıları ve Azure SQL veritabanı yeniden oluşturma](contoso-migration-rearchitect-container-sql.md) | Contoso, SmartHotel uygulamayı Azure'a geçirir. Ardından, Azure Service Fabric ve Azure SQL veritabanı ile veritabanı çalıştıran bir Windows kapsayıcısı olarak app web katmanından rearchitects. | Kullanılabilir
[Makale 13: Uygulamanızı Azure'a yeniden oluşturun](contoso-migration-rebuild.md) | Contoso Azure özellikleri ve Hizmetleri, Azure App Service, Azure Kubernetes Service (AKS), Azure işlevleri, Azure Bilişsel hizmetler ve Azure Cosmos DB dahil olmak üzere çeşitli kullanarak kendi SmartHotel uygulaması oluşturur. | Kullanılabilir
[Makale 14: Azure'a geçiş ölçeklendirin](contoso-migration-scale.md) | Geçiş birleşimleri denedikten sonra Contoso Azure tam geçişi ölçeklendirilebilecek şekilde hazırlar. | Kullanılabilir


## <a name="overview"></a>Genel Bakış

Azure'a geçiş contoso göz önünde bulundurur gibi şirket, şirket içi iş yüklerini buluta geçiş için uygun olup olmadığını belirlemek için teknik ve finansal değerlendirme çalıştırmak istiyor. Özellikle, geçiş için makine ve veritabanı uyumluluğunu değerlendirmek Contoso takım istiyor. Kapasite ve Azure'da Contoso'nun kaynaklarını çalıştırma maliyetlerini tahmin etmek istemektedir.

Kullanmaya başlayın ve söz konusu teknolojileri daha iyi anlamak için iki kendi şirket içi Contoso değerlendirir uygulamalar, aşağıdaki tabloda özetlenmiştir. Şirket, barındırma ve yeniden düzenleme uygulamaları geçiş için geçiş senaryoları için değerlendirir. Yeniden barındırma ve içinde yeniden düzenleme hakkında daha fazla bilgi [Contoso geçişine genel bakış](contoso-migration-overview.md).

Uygulama adı | Platform | Uygulama katmanları | Ayrıntılar
--- | --- | --- | ---
SmartHotel360<br/><br/> (Contoso seyahat gereksinimlerini yönetir) | Bir SQL Server veritabanı ile Windows üzerinde çalışır | İki katmanlı uygulama. Ön uç ASP.NET Web sitesi bir VM üzerinde çalışan (**WEBVM**) ve SQL Server başka bir VM üzerinde çalışan (**SQLVM**). | VMware vCenter Server tarafından yönetilen bir ESXi ana bilgisayarı üzerinde çalışan, vm'leridir.<br/><br/> Örnek uygulama indirebilirsiniz [GitHub](https://github.com/Microsoft/SmartHotel360).
osTicket<br/><br/> (Contoso hizmet Masası uygulaması) | Linux/Apache, MySQL PHP (LAMP) ile çalışır | İki katmanlı uygulama. Ön uç bir PHP Web sitesi bir VM üzerinde çalışan (**OSTICKETWEB**) ve MySQL veritabanı başka bir VM üzerinde çalışan (**OSTICKETMYSQL**). | Uygulama tarafından müşteri hizmeti uygulamalarını çalışanları ve dış müşteriler için sorunları izlemek için kullanılır.<br/><br/> İçinden örneği karşıdan yükleyebilirsiniz [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Geçerli mimari

Geçerli Contoso şirket içi altyapısı bu diyagramda gösterilmektedir:

![Geçerli Contoso mimarisi](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso bir ana veri merkezinde sahiptir. Veri Merkezi içinde Şehir, İstanbul, Doğu ABD bulunur.
- Contoso, Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptir.
- Ana veri merkezinde bir fiber Metro Ethernet bağlantısı (500 MB/sn) ile İnternet'e bağlı.
- Her dal, ana merkezine IPSec VPN tüneli ile işletme sınıfı bağlantıları kullanarak internet'e yerel olarak bağlıdır. Kurulum, Contoso'nun kalıcı olarak bağlı tüm ağ sağlar ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezinin VMware ile tam olarak sanallaştırılır. Contoso, vCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma konaklarını sahiptir.
- Contoso, kimlik yönetimi için Active Directory kullanır. Contoso DNS sunucuları iç ağda kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.

## <a name="business-drivers"></a>İş sürücüleri

Contoso'nun BT liderlik ekibindeki çalıştığınız yakından iş bu geçişle elde etmek istediği anlamak için şirketin iş ortakları:

- **Adres büyütmeye**: Contoso artmaktadır. Sonuç olarak, şirketin şirket içi sistemler ve altyapı baskısı arttı.
- **Verimliliği artırmak**: Contoso gereksiz yordamları kaldırıp, geliştiriciler ve kullanıcılar için işlemleri daha verimli hale getirin gerekiyor. Hızlı ve değil atık zaman veya para şirket kadar olacak şekilde iş gereksinimlerini BT müşteri gereksinimlerine daha hızlı sunabilirsiniz.
- **Çevikliği artırın**:  Contoso BT işletme ihtiyaçlarını daha duyarlı olmamız gerekir. Market'te genel ekonomisine başarılı olması şirket için gerçekleşen değişiklikleri, daha hızlı tepki vermek mümkün olması gerekir. Contoso BT gereken şekilde alınamadı veya bir iş engelleyici haline gelir.
- **Ölçek**: Şirketin başarıyla büyüdükçe, Contoso BT sistemlerinin aynı hızda büyüyebilir sağlamanız gerekir.

## <a name="assessment-goals"></a>Değerlendirme amaçları

Contoso bulut ekibi, geçiş değerlendirmesi için hedefleri belirledi:

- Geçişten sonra azure'da uygulama uygulamaları Contoso'nun şirket içi VMWare ortamınızda bugün sahip aynı performans özelliklerine sahip olmalıdır. Buluta geçiş, uygulama performansını daha az kritik olduğundan gelmez.
- Contoso, uygulamaları ve veritabanlarının uyumluluk Azure gereksinimleriyle anlamak gerekir. Aynı zamanda contoso Azure barındırma seçeneklerini anlamak gerekir.
- Contoso'nun veritabanı yönetim uygulamaları buluta taşıma işlemi sonrasında küçültülmesine.  
- Contoso, yalnızca kendi geçiş seçenekleri yanı sıra buluta taşındıktan sonra altyapı ile ilişkili maliyetleri anlamak istiyor.

## <a name="assessment-tools"></a>Değerlendirme araçları

Contoso, Microsoft araçları, geçiş değerlendirmesi için kullanır. Araçları, şirketin hedefleriyle aynı doğrultuda ilerlemesini ve Contoso, gerekli tüm bilgileri sağlamanız gerekir.

Teknoloji | Açıklama | Maliyet
--- | --- | ---
[Veri Geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Contoso değerlendirmek ve azure'daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını algılamak için veri geçiş Yardımcısı'nı kullanır. Data Migration Yardımcısı SQL kaynaklar ve hedefler arasında özellik eşliği değerlendirir. Bu, performans ve güvenilirlik iyileştirmeleri önerir. | Data Migration Yardımcısı bir ücretsiz ve indirilebilir bir araçtır.
[Azure Geçişi](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso, VMware Vm'lerini değerlendirmek için Azure geçişi hizmeti kullanır. Azure geçişi, makinelerin geçiş uygunluğunu değerlendirir. Bu, Azure'da çalıştırmak için boyutlandırma ve maliyet tahminleri sağlar.  | Mayıs 2018'den itibaren Azure geçişi ücretsiz bir hizmettir.
[Hizmet Eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure geçişi, şirket geçmek istediği makineler arasındaki bağımlılıkları göstermek için hizmet eşlemesini kullanır. | Hizmet eşlemesi, Azure İzleyici günlüklerine bir parçasıdır. Şu anda, Contoso, hizmet eşlemesi 180 gün için kullanabilirsiniz.

Bu senaryoda Contoso indirir ve kendi seyahat uygulaması için şirket içi SQL Server veritabanını değerlendirmek için veri geçiş Yardımcısı çalıştırılır. Contoso Azure Geçişi ' % s'uygulama sanal makinelerini azure'a geçiş işleminden önce değerlendirmek için bağımlılık eşlemesi ile kullanır.

## <a name="assessment-architecture"></a>Değerlendirmesi mimarisi

![Geçiş değerlendirmesi mimarisi](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso tipik kurumsal kuruluşu temsil eden bir kurgusal bir addır.
- Contoso olan bir şirket içi veri merkezi (**contoso-datacenter**) ve şirket içi etki alanı denetleyicileri (**contosodc1 adlı**, **CONTOSODC2**).
- VMware Vm'lerini 6.5 sürümünü çalıştıran VMware ESXi ana bilgisayarına bulunur (**contosohost1**, **contosohost2**).
- VMware ortamı vCenter Server 6.5 tarafından yönetilir (**vcenter.contoso.com**, bir VM'de çalışan).
- SmartHotel360 seyahat uygulaması şu özelliklere sahiptir:
    - Uygulama, iki VMware Vm'sinde katmanlı (**WEBVM** ve **SQLVM**).
    - Vm'leri, VMware ESXi ana bilgisayarında bulunan **contosohost1.contoso.com**.
    - SP1 ile Windows Server 2008 R2 Datacenter çalıştıran Vm'leri.
- VMware ortamı bir sanal makine üzerinde çalıştırılan vCenter Server (**vcenter.contoso.com**) tarafından yönetilir.
- OsTicket hizmet Masası Uygulaması:
    - Uygulama, iki VM arasında katmanlı (**OSTICKETWEB** ve **OSTICKETMYSQL**).
    - Ubuntu Linux Server 16.04 LTS çalıştıran VM'ler.
    - **OSTICKETWEB** Apache 2 ve PHP 7.0 çalışıyor.
    - **OSTICKETMYSQL** MySQL 5.7.22 çalışıyor.

## <a name="prerequisites"></a>Önkoşullar

Contoso ve diğer kullanıcıların değerlendirmesi için aşağıdaki önkoşulları karşılaması gerekir:

- Sahibi veya katkıda bulunan izinleri Azure aboneliği veya Azure aboneliğindeki bir kaynak grubu.
- 6.5, 6.0 veya 5.5 sürümü çalıştıran şirket içi vCenter Server örneği.
- Vcenter Server'daki salt okunur bir hesap veya hesap oluşturma izinleri.
- .Ova şablonu kullanarak bir VM vCenter Server örneğinde oluşturma izinleri.
- 5.5 veya sonraki sürümünü çalıştıran en az bir ESXi ana bilgisayarı.
- Biri, SQL Server veritabanı çalıştıran en az iki şirket içi VMware sanal makinesi.
- Her sanal makinede Azure geçişi aracılarını yüklemek için izinler.
- Sanal makinelerin doğrudan İnternet bağlantısı olmalıdır.  
    - İnternet erişimini kısıtlayabilirsiniz [gerekli URL'ler](https://docs.microsoft.com/azure/migrate/concepts-collector).  
    - İnternet bağlantısı, Azure Vm'lerinizi yoksa [Log Analytics Gateway](../azure-monitor/platform/gateway.md) bunlar üzerinde yüklü olmalıdır ve aracı trafiğini üzerinden yönlendirilir.
- Veritabanı değerlendirmesi için SQL Server örneğini çalıştıran sanal makinenin FQDN’si.
- Windows SQL Server sanal makinesinde çalışan güvenlik duvarının TCP bağlantı noktası 1433'ü (varsayılan) noktasında harici bağlantılara izin vermelidir. Bu kurulum bağlanmak Data Migration Yardımcısı sağlar.

## <a name="assessment-overview"></a>Değerlendirme genel bakış

Contoso, değerlendirme performansını şu şekildedir:

> [!div class="checklist"]
> * **1. adım: Veri geçiş Yardımcısı'nı yüklemek ve indirmek**: Contoso Data Migration Yardımcısı'nı şirket içi SQL Server veritabanının değerlendirmesi için hazırlar.
> * **2. adım: Veri geçiş Yardımcısı'nı kullanarak veritabanını değerlendirme**: Contoso çalışır ve veritabanı değerlendirmesini analiz eder.
> * **3. adım: Azure Geçişi'ni kullanarak VM değerlendirmesi için hazırlama**: Contoso şirket içi hesapları ayarlar ve VMware ayarlarını değiştirir.
> * **4. adım: Azure Geçişi'ni kullanarak şirket içi Vm'leri bulmak**: Contoso, bir Azure geçişi toplayıcısı VM oluşturur. Ardından, Contoso için değerlendirme Vm'leri bulmak için toplayıcıyı çalışır.
> * **5. adım: Azure Geçişi'ni kullanarak bağımlılık Analizine hazırlanma**: Şirket VM'ler arasındaki bağımlılık eşlemesini görebilmeniz için Contoso sanal makinelere Azure geçişi aracılarını yükler.
> * **6. adım: Azure Geçişi'ni kullanarak sanal makineleri değerlendirme**: Contoso, bağımlılıkları, grupları sanal makinelerin denetler ve değerlendirme çalıştırır. Değerlendirme hazır olduğunda, Contoso geçiş hazırlığında değerlendirmeyi analiz eder.

## <a name="step-1-download-and-install-data-migration-assistant"></a>1. Adım: İndirme ve veri geçiş Yardımcısı'nı yükleme

1. Contoso veri geçiş Yardımcısı'ndan indirir [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - Veri geçiş Yardımcısı SQL Server örneğine bağlanabilen herhangi bir makineye yüklenebilir. Contoso, SQL Server makinesinde çalıştırmak gerektirmez.
    - Data Migration Yardımcısı'nı SQL Server ana bilgisayar makinesinde çalıştırmamalısınız.
2. Contoso indirilen kurulum dosyasına (DownloadMigrationAssistant.msi) yüklemeye çalışır.
3. Üzerinde **son** sayfası, Contoso seçer **başlatma Microsoft Data Migration Yardımcısı** Sihirbazı tamamlamadan önce.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel360"></a>2. Adım: Çalıştırma ve SmartHotel360 için'veritabanı değerlendirmesini analiz etme

Artık, Contoso SmartHotel360 uygulama için şirket içi SQL Server veritabanını çözümlemek için bir değerlendirme çalıştırabilirsiniz.

1. Veri geçiş Yardımcısı'nda, Contoso seçer **yeni** > **değerlendirme**ve ardından değerlendirme bir proje adı sağlar.
2. İçin **kaynak sunucu türü**, Contoso seçer **Azure Virtual Machines'de SQL Server**.

    ![Data Migration Yardımcısı - kaynak seçin](./media/contoso-migration-assessment/dma-assessment-1.png)

   > [!NOTE]
   >    Şu anda, Data Migration Yardımcısı değerlendirmesi bir Azure SQL veritabanı yönetilen örneğine geçirmek için desteklememektedir. Geçici bir çözüm olarak, Contoso SQL Server Azure sanal makinesinde beklenen hedefi değerlendirme için kullanır.

3. İçinde **hedef sürümü seçin**, Contoso, SQL Server 2017 hedef sürüm olarak seçer. SQL veritabanı yönetilen örneği tarafından kullanılan sürümü olduğundan, bu sürümü seçmek contoso gerekir.
4. Contoso yardımcı uyumluluğu ve yeni özellikler hakkında bilgi bulmak için raporları seçer:
   - **Uyumluluk sorunlarını** geçişten veya geçişten önce küçük bir ayarlama gerektiren değişiklikleri unutmayın. Bu rapor, kullanım dışı olan özellikleri şu anda kullanımda haberdar Contoso tutar. Sorunlar, uyumluluk düzeyine göre düzenlenir.
   - **Yeni özellikler önerisi** geçişten sonra veritabanı için kullanılabilecek hedef SQL Server platformundaki yeni özellikleri notlar. Yeni bir özellik önerisi, başlıklar altında düzenlenir **performans**, **güvenlik**, ve **depolama**.

     ![Data Migration Yardımcısı - uyumluluk sorunları ve yeni özellikleri](./media/contoso-migration-assessment/dma-assessment-2.png)

2. İçinde **sunucuya Bağlan**, Contoso, veritabanı ve ona erişmek için kimlik bilgilerini çalışan ve VM adını girer. Contoso seçer **sunucu sertifikasına güven** VM, SQL Server erişebilir emin olmak için. Ardından, Contoso seçer **Connect**.

    ![Data Migration Yardımcısı -, sunucuya bağlanma](./media/contoso-migration-assessment/dma-assessment-3.png)

3. İçinde **kaynağı Ekle**, Contoso ekler istediği değerlendirmek için veritabanı ve ardından seçer **sonraki** değerlendirmeyi başlatmak için.
4. Değerlendirme oluşturulur.

    ![Data Migration Yardımcısı - değerlendirme oluşturun](./media/contoso-migration-assessment/dma-assessment-4.png)

5. İçinde **gözden geçirme sonuçlarının**, Contoso değerlendirme sonuçlarını görüntüler.

### <a name="analyze-the-database-assessment"></a>Veritabanı değerlendirmesini analiz etme

Bunlara erişilebilir hemen sonra sonuçlar görüntülenir. Contoso sorunları giderir, seçmelisiniz **değerlendirmeyi yeniden Başlat** değerlendirmeyi yeniden çalıştırmak için.

1. İçinde **uyumluluk sorunlarını** raporunda, Contoso her uyumluluk düzeyinde herhangi bir sorun olup olmadığını denetler. Uyumluluk düzeyleri, SQL Server sürümleriyle aşağıdaki şekilde eşlenir:

   - 100: SQL Server 2008/Azure SQL veritabanı
   - 110: SQL Server 2012/Azure SQL veritabanı
   - 120: SQL Server 2014/Azure SQL veritabanı
   - 130: SQL Server 2016/Azure SQL veritabanı
   - 140: SQL Server 2017/Azure SQL veritabanı

     ![Data Migration Yardımcısı - uyumluluk raporu verir.](./media/contoso-migration-assessment/dma-assessment-5.png)

2. İçinde **özellik önerileri** raporu için Contoso, değerlendirmenin geçişten sonra önerdiği performans, güvenlik ve depolama özellikleri görüntüler. Çeşitli özellikler önerilir, bellek içi OLTP, columnstore dizinleri, Stretch Database, Always Encrypted, dinamik veri maskeleme ve saydam veri şifrelemesi gibi.

    ![Data Migration Yardımcısı - özellik önerileri raporu](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Contoso gereken [saydam veri şifrelemesini etkinleştirme](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) tüm SQL Server veritabanları için. Bu, bir veritabanı bulutta, şirket içinde barındırılan daha olduğunda daha da önemlidir. Saydam veri şifrelemesi yalnızca geçişten sonra etkinleştirilmelidir. Saydam veri şifrelemesi zaten etkin değilse, Contoso sertifika veya asimetrik anahtar hedef sunucunun ana veritabanına taşımanız gerekir. Bilgi edinmek için nasıl [saydam veri şifrelemesi ile korunan veritabanını başka bir SQL Server örneğine taşıma](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Contoso değerlendirme JSON veya CSV biçiminde dışarı aktarabilirsiniz.

> [!NOTE]
> Büyük ölçekli değerlendirmeleri için:
> - Değerlendirmelerin durumunu görüntüleyin ve aynı anda birden fazla değerlendirme çalıştırılması **tüm değerlendirmeler** sayfası.
> - Değerlendirmeleri birleştirebilirsiniz bir [SQL Server veritabanı](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017).
> - Değerlendirmeleri birleştirebilirsiniz bir [Power BI raporu](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).

## <a name="step-3-prepare-for-vm-assessment-by-using-azure-migrate"></a>3. Adım: Azure Geçişi'ni kullanarak VM değerlendirmesi için hazırlama

Contoso sanal makineleri değerlendirme için otomatik olarak bulmak üzere Azure geçişi kullanabileceğiniz bir VMware hesabı oluşturmak gereken bir VM oluşturmak için açılması gereken bağlantı noktalarını not edin ve istatistik ayarları düzeyini ayarlayın hakları doğrulayın.

### <a name="set-up-a-vmware-account"></a>VMware hesabı ayarlama

VM bulma aşağıdaki özelliklere sahip sunucu vcenter'da salt okunur bir hesap gerektirir:

- **Kullanıcı türü**: En az bir salt okunur kullanıcı.
- **İzinleri**: Veri Merkezi nesnesi için seçin **alt nesneleri yay** onay kutusu. İçin **rol**seçin **salt okunur**.
- **Ayrıntılar**: Veri merkezi düzeyinde merkezindeki tüm nesnelere erişimi olan kullanıcı atanır.
- Erişimi kısıtlamak için Ata **erişemez** rolüyle **alt yay** nesne alt nesnelere (vSphere konakları, veri depoları, VM'ler ve ağlar).

### <a name="verify-permissions-to-create-a-vm"></a>Sanal makine oluşturma izinlerini doğrulama

Contoso, bir dosyayı .ova biçiminde içeri aktararak VM oluşturma iznine sahip olduğunu doğrular. Bilgi edinmek için nasıl [ayrıcalıklarına sahip bir rol oluşturup](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Bağlantı noktalarını doğrulama

Contoso değerlendirme bağımlılık eşlemesini kullanır. Bağımlılık eşlemesi aracıyı değerlendirilecek Vm'lerde yüklü olmasını gerektirir. Aracısının her VM'de, 443 numaralı TCP bağlantı noktasından Azure'a bağlanmak çözümleyebilmesi gerekir. Hakkında bilgi edinin [bağlantı gereksinimleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).

### <a name="set-statistics-settings"></a>İstatistik ayarlarını yapma

Contoso dağıtım başlamadan önce vCenter Server için istatistik ayarları düzeyini 3 ayarlamanız gerekir.

> [!NOTE]
> - Düzeyi ayarladıktan sonra Contoso en az değerlendirme çalıştırılmadan önce bir gün beklemeniz gerekir. Aksi takdirde, değerlendirme beklendiği gibi çalışmayabilir.
> - Düzey 3'ten yüksekse değerlendirme çalışır, ancak:
>    - Diskler ve ağ iletişimi için performans verileri toplanmaz.
>    - Depolama için Azure Geçişi, Azure’da şirket içi diskle aynı boyutta standart bir disk önerir.
>    - Her bir şirket içi ağ bağdaştırıcısı için ağ iletişimi için Azure'da bir ağ bağdaştırıcısı önerilir.
>    - İşlem, Azure Geçişi sanal makine çekirdeklerine ve bellek boyutuna bakar ve aynı yapılandırmaya sahip bir Azure sanal makinesi önerir. Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.
> - Düzey 3 kullanarak boyutlandırma hakkında daha fazla bilgi için bkz. [boyutlandırma](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Düzeyini ayarlamak için:

1. VSphere Web istemcisi Contoso vCenter sunucu örneğini açar.
2. Contoso seçer **Yönet** > **ayarları** > **genel** > **Düzenle**.
3. İçinde **istatistikleri**, Contoso ayarlar istatistik düzeyi ayarları **3. düzey**.

    ![vCenter Server istatistik düzeyini](./media/contoso-migration-assessment/vcenter-statistics-level.png)

## <a name="step-4-discover-vms"></a>4. Adım: VM'leri bulma

Vm'leri bulmak için Azure geçişi projesini Contoso oluşturur. Contoso indirir ve Toplayıcı sanal Makinesini ayarlar. Ardından, Contoso, şirket içi Vm'leri bulmak için toplayıcıyı çalışır.

### <a name="create-a-project"></a>Proje oluşturma

1. İçinde [Azure portalında](https://portal.azure.com), Contoso arar **Azure geçişi**. Ardından, Contoso bir proje oluşturur.
2. Contoso bir proje adını belirtir (**ContosoMigration**) ve Azure aboneliği. Yeni bir Azure kaynak grubu oluşturur (**ContosoFailoverRG**).
    > [!NOTE]
    > - Azure geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz.
    > - Herhangi bir hedef konumdan geçiş planlayabilirsiniz.
    > - Proje konumu yalnızca şirket içi Vm'lerden toplanan meta verileri depolamak için kullanılır.

    ![Azure geçişi - geçiş projesi oluşturma](./media/contoso-migration-assessment/project-1.png)

### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure geçişi olarak bilinen bir şirket içi VM oluşturur *Toplayıcı gerecini*. VM, şirket içi VMware Vm'lerini bulur ve sanal makineleri ile ilgili meta verileri Azure geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için Contoso OVA şablon indirir ve VM'yi oluşturmak için şirket içi vCenter sunucusu örneğine içeri aktarır.

1. Azure geçişi projesinde Contoso seçer **Başlarken** > **Keşif ve değerlendirme** > **makineleri Bul**. Contoso OVA şablon dosyası indirir.
2. Contoso proje kimliği ve anahtarı kopyalar. Proje ve kimliği Toplayıcı yapılandırmak için gereklidir.

    ![Azure geçişi - OVA dosyasını indirme](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Sanal Makineyi dağıtmadan önce Contoso OVA dosyasını güvenli olup olmadığını denetler:

1. Contoso, dosyayı karşıdan yüklendiği makinede bir yönetici komut istemi penceresi açılır.
2. Contoso OVA dosyasını karma oluşturmak için şu komutu çalıştırır:

    ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

    **Örnek**

    ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma listelenen karma değerler eşleşmelidir [burada](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#continuous-discovery).

### <a name="create-the-collector-appliance"></a>Toplayıcı gereci oluşturma

Şimdi, Contoso, indirilen dosyayı vCenter sunucusu örneğine içeri aktarabilir ve Toplayıcı gerecini VM sağlama:

1. VSphere Client konsolunda Contoso seçer **dosya** > **OVF şablonu Dağıt**.

    ![vSphere Web istemcisi - dağıtma OVF şablonu](./media/contoso-migration-assessment/vcenter-wizard.png)

2. OVF şablonu Dağıtma Sihirbazı içinde Contoso seçer **kaynak**ve ardından OVA dosyasının konumunu belirtir.
3. İçinde **adını ve konumunu**, Contoso Toplayıcı VM için bir görünen ad belirtir. Ardından, sanal Makinenin barındırılacağı Envanter konumunu seçer. Contoso, ayrıca konak veya küme Toplayıcı gerecini çalıştırılacağı belirtir.
4. İçinde **depolama**, Contoso depolama konumunu belirtir. İçinde **Disk biçimi**, Contoso seçer nasıl depolama sağlamak istiyor.
5. İçinde **ağ eşlemesi**, Contoso, Toplayıcı VM bağlanmak bir ağ belirtir. Ağ meta verileri Azure'a göndermek için internet bağlantısı gerekir.
6. Contoso ayarlarını inceler ve ardından seçer **dağıtımdan sonra Aç** > **son**. Gereç oluşturulduğunda başarıyla tamamlandığını bildiren bir ileti görüntülenir.

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Artık, Contoso Vm'leri bulmak için toplayıcıyı çalışır. Şu anda Toplayıcı şu anda yalnızca destekler **İngilizce (Amerika Birleşik Devletleri)** işletim sistemi dili ve Toplayıcı arabirimi dili olarak.

1. VSphere Client konsolunda Contoso seçer **konsolu aç**. Contoso, dil, saat dilimi ve parola tercihlerini Toplayıcı VM için belirtir.
2. Masaüstünde, Contoso seçer **Toplayıcı Çalıştır** kısayol.

    ![vSphere istemci Konsolu - Toplayıcı kısayolu](./media/contoso-migration-assessment/collector-shortcut.png)

3. Azure geçişi Toplayıcısı'nda Contoso seçer **önkoşulları ayarlama**. Contoso lisans koşullarını kabul eder ve üçüncü taraf bilgilerini okur.
4. Toplayıcı, VM'nin internet erişimi, zaman eşitlenir ve Toplayıcı hizmetinin çalışıp çalışmadığını olduğunu denetler. (Toplayıcı hizmeti VM üzerinde varsayılan olarak yüklenir.) Contoso, VMware Powerclı'yi de yükler.

    > [!NOTE]
    > Proxy kullanmadan sanal Makinenin doğrudan internet erişimi olduğunu kabul edilir.

    ![Azure geçişi toplayıcısı - önkoşulları doğrulayın](./media/contoso-migration-assessment/collector-verify-prereqs.png)

5. İçinde **vCenter Server ayrıntılarını belirtin**, Contoso adı (FQDN) veya vCenter sunucusu örneğinin IP adresini girer ve salt okunur kimlik bilgilerini, bulma için kullanılır.
6. Contoso, VM bulma için bir kapsam seçer. Toplayıcı yalnızca belirtilen kapsam içindeki Vm'leri bulabilir. Kapsamı belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam 1500'den fazla VM içermelidir.

    ![VCenter Server ayrıntılarını belirtin](./media/contoso-migration-assessment/collector-connect-vcenter.png)

7. İçinde **geçişi projesini belirtin**, Contoso girer Azure geçişi proje Kimliğini ve Portalı'ndan kopyalanan anahtarı. Proje kimliği ve anahtarı almak için Contoso projesine gidebilirsiniz **genel bakış** sayfası > **makineleri Bul**.  

    ![Geçiş projesi belirtin](./media/contoso-migration-assessment/collector-connect-azure.png)

8. İçinde **toplama durumunu görüntüle**, Contoso bulma işlemini izleyin ve Vm'lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

    ![Toplama ilerleme durumunu görüntüleme](./media/contoso-migration-assessment/collector-collection-process.png)

### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Koleksiyon bittiği zaman Contoso sanal makinelerin portalda görüntülenip görüntülenmediğini denetler:

1. Azure geçişi projesinde Contoso seçer **Yönet** > **makineler**. Contoso bulunacak istediği Vm'leri gösterilir denetler.

    ![Azure geçişi - bulunan makineler](./media/contoso-migration-assessment/discovery-complete.png)

2. Şu anda, makineleri Azure geçişi aracılarının yüklü yok. Contoso bağımlılıkları görüntülemek için aracıları yüklemeniz gerekir.

    ![Azure geçişi - aracı yüklemesi gerekli değildir](./media/contoso-migration-assessment/machines-no-agent.png)

## <a name="step-5-prepare-for-dependency-analysis"></a>5. Adım: Bağımlılık Analizine hazırlanma

Değerlendirmek için istediği sanal makineler arasındaki bağımlılıkları görüntülemek için Contoso indirir ve uygulama sanal makinelerini aracıları yükler. Contoso, tüm sanal makineler, uygulamaları, Windows ve Linux için hem de aracıları yükler.

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Değiştirmeden önce sanal makinelerin bir kopyasını tutmak için Contoso aracıları yüklemeden önce anlık görüntüsünü alır.

![Makine anlık görüntüsü](./media/contoso-migration-assessment/snapshot-vm.png)

### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme

1. İçinde **makineler**, Contoso makine seçer. İçinde **bağımlılıkları** sütunu, Contoso seçer **yüklemesi gerektirir**.
2. İçinde **makineleri Bul** bölmesinde, Contoso:
    - Microsoft İzleme Aracısı (MMA) ve her bir Windows VM için bağımlılık aracısını yükler.
    - MMA ve bağımlılık aracısını her bir Linux VM için indirir.
3. Contoso çalışma alanı kimliği ve anahtarı kopyalar. MMA'yı yüklerken Contoso çalışma alanı kimliği ve anahtarı gerekir.

    ![Aracıyı indirme](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Windows sanal makinelerinde aracıları yükleyin

Contoso yükleme her sanal makinede çalıştırır.

#### <a name="install-the-mma-on-windows-vms"></a>Windows Vm'lerinde MMA'yı yükleme

1. Contoso indirilen aracıya çift tıklamaları birbirinden ayırma.
2. İçinde **hedef klasör**, Contoso varsayılan yükleme klasörünü tutar ve ardından seçer **sonraki**.
3. İçinde **Aracı Kurulum Seçenekleri**, Contoso seçer **aracıyı Azure Log Analytics'e bağlama** > **sonraki**.

    ![Microsoft Monitoring Agent Kurulumu - Aracı Kurulum Seçenekleri](./media/contoso-migration-assessment/mma-install.png)

4. İçinde **Azure Log Analytics**, Contoso yapıştırır portaldan kopyaladığınız anahtarını ve çalışma alanı kimliği.

    ![Microsoft Monitoring Agent Kurulumu - Azure günlük analizi](./media/contoso-migration-assessment/mma-install2.png)

5. İçinde **yüklemeye hazır**, Contoso MMA'yı yükler.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Windows Vm'lerinde bağımlılık aracısını yükleme

1. Contoso indirilen bağımlılık aracısına çift tıklamaları birbirinden ayırma.
2. Contoso lisans koşullarını kabul eder ve yüklemenin tamamlanmasını bekler.

    ![Bağımlılık Aracısı Kurulumu - yükleme](./media/contoso-migration-assessment/dependency-agent.png)

### <a name="install-the-agents-on-linux-vms"></a>Linux Vm'lerinde aracıları yükleyin

Contoso yükleme her sanal makinede çalıştırır.

#### <a name="install-the-mma-on-linux-vms"></a>Linux Vm'lerinde MMA'yı yükleme

1. Contoso, aşağıdaki komutu kullanarak, her VM'de Python ctypes kitaplığı yükler:

    `sudo apt-get install python-ctypeslib`
2. Contoso kök olarak MMA aracısını yüklemek için komutu çalıştırmanız gerekir. Kök olmak için Contoso şu komutu çalıştırır ve ardından kök parolasını girer:

    `sudo -i`
3. Contoso MMA'yı yükler:
   - Contoso çalışma alanı kimliği ve anahtarı komutu girer.
   - 64-bit komutlardır.
   - Çalışma alanı kimliği ve birincil anahtar, Azure portalında Log Analytics çalışma alanında bulunur. Seçin **ayarları**ve ardından **bağlı kaynaklar** sekmesi.
   - Log Analytics Aracısı'nı indirme, sağlama toplamını doğrulama ve yüklemek için aşağıdaki komutları çalıştırın ve ekleme aracısı:

     ```
     wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
     ```

#### <a name="install-the-dependency-agent-on-linux-vms"></a>Linux Vm'lerinde bağımlılık aracısını yükleme

MMA'yı yükledikten sonra Contoso Linux Vm'lerinde bağımlılık aracısını yükler:

1. Bağımlılık Aracısı'nı InstallDependencyAgent Linux64.bin, kendi kendine ayıklanan bir ikili içeren bir kabuk betiği'ni kullanarak Linux bilgisayarlara yüklenir. Contoso çalıştırmaları sh kullanarak veya, bir dosyayı ekler dosyaya izinleri yürütün.

2. Contoso kök olarak Linux bağımlılık aracısını yükler:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```

## <a name="step-6-run-and-analyze-the-vm-assessment"></a>6. Adım: Çalıştırma ve sanal makine değerlendirmesini analiz etme

Contoso, artık makine bağımlılıklarını doğrulayın ve bir grup oluşturun. Ardından, grup için değerlendirme çalışır.

### <a name="verify-dependencies-and-create-a-group"></a>Bağımlılıkları doğrulayın ve bir grup oluşturun

1. Analiz etmek için hangi makineleri belirlemek için Contoso seçer **bağımlılıklarını görüntüleme**.

    ![Azure geçişi - makine bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. SQLVM için bağımlılık eşlemi aşağıdaki ayrıntıları gösterir:

    - İşlem gruplarının veya belirtilen süre (varsayılan olarak, bir saat) boyunca SQLVM'de çalıştırılan etkin ağ bağlantınız işlemler.
    - Tüm bağımlı makinelere/makinelerden gelen (istemci) ve giden (sunucu) TCP bağlantıları.
    - Azure geçişi aracılarının yüklü olan bağımlı makineler, ayrı kutular olarak gösterilir.
    - Aracıların yüklü olmayan makineler bağlantı noktası ve IP adresi bilgilerini gösterir.

3. Aracı (WEBVM) yüklü olan makineler için daha fazla bilgi görüntülemek için makine kutusuna Contoso seçer. FQDN, işletim sistemi ve MAC adresi bilgileri içerir.

    ![Azure geçişi - Grup bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Contoso (SQLVM ve WEBVM) grubuna eklemek için Vm'leri seçer. Contoso, birden çok VM seçmek için CTRL tuşuna basıp tıklayarak kullanır.
5. Contoso seçer **Grup Oluştur**ve ardından bir adını girer (**smarthotelapp**).

    > [!NOTE]
    > Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını genişletebilirsiniz. Belirli bir süre seçin veya başlangıç ve bitiş tarihleri seçin.

### <a name="run-an-assessment"></a>Değerlendirme çalıştırma

1. İçinde **grupları**, Contoso grubu açar (**smarthotelapp**) ve ardından seçer **Değerlendirme Oluştur**.

    ![Azure geçişi - değerlendirme oluşturma](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Değerlendirme görüntülemek üzere Contoso seçer **Yönet** > **değerlendirmeleri**.

Contoso varsayılan değerlendirme ayarlarını kullanır ancak yapabilecekleriniz [ayarlarını özelleştirme](how-to-modify-assessment.md).

### <a name="analyze-the-vm-assessment"></a>Sanal makine değerlendirmesini analiz etme

Azure geçişi değerlendirmesi bilgilerdir hakkında Azure ile uyumluluk şirket içi boyutlandırmaya Azure VM için önerilen ve tahmini aylık Azure maliyetleri.

![Azure geçişi - değerlendirme raporu](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

![Azure geçişi - değerlendirme görüntüsü](./media/contoso-migration-assessment/assessment-display.png)

Bir güvenilirlik derecelendirmesi 1 yıldız dan 5 yıldız için değerlendirme sahiptir (1 yıldız en düşük ve 5 yıldız en yüksek).
- Güvenilirlik derecelendirmesi değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği üzerinde temelinde bir değerlendirmeye atanır.
- Derecelendirme, Azure geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Güvenilirlik derecelendirmesi yaptığınızda kullanışlıdır *performansa dayalı boyutlandırma*. Azure geçişi, kullanım tabanlı boyutlandırma için yeterli veri noktası olmayabilir. İçin *şirket içi olarak* Azure geçişi VM'yi boyutlandırmak için ihtiyaç duyduğu tüm veri noktalarına sahip boyutlandırma, güvenilirlik derecelendirmesi her zaman 5 yıldız olmasıdır.
- Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için güvenilirlik derecelendirmesi sağlanır:

   Veri noktalarının kullanılabilirliği | Güvenilirlik derecelendirmesi
   --- | ---
   %0-%20 | 1 yıldız
   %21-%40 | 2 yıldız
   %41-%60 | 3 yıldız
   %61-%80 | 4 yıldız
   %81-%100 | 5 yıldız

#### <a name="verify-azure-readiness"></a>Azure için hazır olma doğrulayın

![Azure geçişi - hazır olma durumu değerlendirmesi](./media/contoso-migration-assessment/azure-readiness.png)  

Değerlendirme raporu tabloda özetlenen bilgileri gösterir. Performans tabanlı boyutlandırmayı göstermek için Azure geçişi aşağıdaki bilgiler gerekiyor. Bilgiler toplanamıyorsa, boyutlandırma değerlendirmesi doğru olmayabilir.

- CPU ve bellek kullanımı verileri.
- Sanal makineye bağlı her disk için okuma/yazma IOPS ve aktarım hızı.
- Sanal makineye bağlı her ağ bağdaştırıcısı için ağa gelen/ağdan giden bilgiler.

Ayar | Belirtme | Ayrıntılar
--- | --- | ---
**Azure sanal makinesinin hazır olma durumu** | VM geçiş için hazır olup olmadığını belirtir. | Olası durumlar:</br><br/>- Azure için hazır<br/><br/>- Koşullarla hazır <br/><br/>- Azure için hazır değil<br/><br/>- Hazır olma durumu bilinmiyor<br/><br/> Azure geçişi, bir sanal Makinenin hazır olmaması durumunda uygulanacak bazı düzeltme adımlarını gösterir.
**Azure sanal makinesi boyutu** | Hazır sanal makineler için Azure geçişi, bir Azure VM boyutu öneri sunar. | Boyutlandırma önerisi, değerlendirme özelliklerine bağlıdır:<br/><br/>- Performans tabanlı boyutlandırma kullandıysanız boyutlandırma, sanal makinelerin performans geçmişini dikkate alır.<br/><br/>-Kullandıysanız *şirket içi olarak*boyutlandırma, şirket içi VM boyutuna dayalıdır ve kullanım verileri kullanılmaz.
**Önerilen araç** | Azure makineleri aracıları çalıştırdığından Azure geçişi makinenin içinde çalışan işlemlere bakar. Bu makinenin bir veritabanı makinesi olup olmadığını tanımlar.
**Sanal makine bilgileri** | Bu rapor, işletim sistemi, önyükleme türü ve disk ve Depolama bilgileri gibi şirket içi sanal makine için ayarları gösterir.

#### <a name="review-monthly-cost-estimates"></a>Aylık maliyet tahminlerini gözden geçirme

Bu görünüm, Azure'da çalışan VM'lerin toplam işlem ve depolama maliyetini gösterir. Ayrıca, her makine ayrıntılarını gösterir.

![Azure geçişi - Azure maliyetleri](./media/contoso-migration-assessment/azure-costs.png)

- Maliyet tahminleri, bir makine için boyut önerileri kullanılarak hesaplanır.
- İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir.

## <a name="clean-up-after-assessment"></a>Değerlendirme sonrasında Temizle

- Değerlendirme sona erdiğinde, Contoso değerlendirmeleri gelecekte kullanmak için Azure geçişi Gereci korur.
- Contoso VMware VM devre dışı bırakır. Ek sanal makineler değerlendirirken, Contoso yeniden kullanır.
- Contoso tutar **Contoso geçiş** azure'da proje. Proje şu anda dağıtılmış **ContosoFailoverRG** kaynak grubunda Doğu ABD Azure bölgesi.
-  Toplayıcı sanal makinesi bir 180 günlük değerlendirme lisansına sahiptir. Bu sınırı süresi dolarsa, Contoso Toplayıcı indirin ve yeniden kurmanız gerekecektir.

## <a name="conclusion"></a>Sonuç

Bu senaryoda, veri geçiş değerlendirmesi aracını kullanarak, SmartHotel360 uygulama veritabanını Contoso değerlendirir. Şirket içi sanal makineleri, Azure geçişi hizmetini kullanarak değerlendirir. Contoso, şirket kaynaklarına Azure'a geçiş için hazır olduğundan emin olmak için değerlendirmeleri inceler.

## <a name="next-steps"></a>Sonraki adımlar

Contoso, serideki sonraki makalede, SmartHotel360 uygulamanızı Azure'a lift-and-shift ile taşıma geçişini kullanarak rehosts. Contoso, Azure Site Recovery kullanarak uygulama için ön uç WEBVM geçirir. Bu uygulama veritabanını Azure SQL veritabanı yönetilen örneği için veritabanı geçiş hizmetini kullanarak geçirir. [Başlama](contoso-migration-rehost-vm-sql-managed-instance.md) bu dağıtımla.
