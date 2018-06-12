---
title: Contoso geçiş Azure için şirket içi iş yüklerini değerlendirmek | Microsoft Docs
description: Contoso şirket içi makinelerinin geçiş Azure Azure geçiş ve veritabanını yükseltme için nasıl değerlendirir öğrenin
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 8568668032a97e574a85758080818311839a9caa
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35301142"
---
# <a name="contoso-migration-assess-on-premises-workloads-for-migration-to-azure"></a>Contoso geçişi: geçiş Azure için şirket içi iş yüklerini değerlendirin

Bu makalede, Azure, geçiş için Hazırlanmakta kendi şirket içi SmartHotel uygulama Contoso nasıl değerlendirir gösterir.

Bu belge nasıl Contoso adlı kurgusal şirket için Microsoft Azure bulut şirket kaynaklarını geçirir belge makaleleri dizi üçüncü değil. Seri arka plan bilgileri içerir ve geçiş altyapısını kurma nasıl çalışılacağını dağıtım senaryolarında bir dizi geçiş için şirket içi kaynaklar uygunluğunu değerlendirmek ve farklı türde geçişler çalıştırın. Karmaşık senaryolar büyümesine ve biz diğer makaleler zamanla eklenmesi.

**Makale** | **Ayrıntılar** | **Durum**
--- | --- | ---
Makale 1: genel bakış | Contoso'nun geçiş stratejisi, makale serisi ve kullanırız örnek uygulamaları genel bir bakış sağlar. | Kullanılabilir
Makale 2: bir Azure altyapısı dağıtın | Nasıl kendi şirket içi ve Azure altyapı Contoso bu geçiş için hazırlar açıklar. Aynı alt tüm Contoso geçiş senaryoları için kullanılır. | Kullanılabilir
Makale 3: şirket içi kaynaklara (Bu makalede) değerlendirin | Contoso VMware üzerinden çalışan kendi şirket içi iki katmanlı SmartHotel uygulamasının bir değerlendirme nasıl çalıştığı gösterilmektedir. Uygulama VM'ler ile değerlendirmek [Azure geçirmek](migrate-overview.md) hizmet ve uygulama SQL Server veritabanı ile [Azure veritabanı geçiş Yardımcısı](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Kullanılabilir
Makale 4: Azure Vm'leri ve yönetilen bir SQL örneği düzenleme (yükseltme ve shift) | Contoso SmartHotel uygulamayı Azure'a nasıl geçirir gösterir. VM ön uç uygulamasını kullanarak geçirmek [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), uygulamayı ve veritabanı kullanma [Azure veritabanı geçiş](https://docs.microsoft.com/azure/dms/dms-overview) yönetilen bir SQL örneğine geçirmek için hizmet. | Kullanılabilir
Makale 5: Azure VM'ler düzenleme (yükseltme ve shift) | Contoso SmartHotel uygulama yalnızca Site Recovery kullanarak sanal makineleri geçirmek nasıl gösterir.
Makale 6: (yükseltme ve shift) Azure Vm'leri ve SQL Server kullanılabilirlik gruplarını yeniden Düzenle | Contoso SmartHotel uygulama nasıl geçirir gösterir. Sanal makineleri uygulama ve veritabanı geçiş hizmeti uygulama veritabanı için SQL Server kullanılabilirlik grubu geçirmek için geçirmek için Site Recovery kullanırlar. | Kullanılabilir
Makale 7: Azure Vm'leri ve Azure MySQL sunucusu düzenleme (yükseltme ve shift) | Contoso geçirmek için Site Recovery ve MySQL çalışma ekranı kullanarak SmartHotel app sanal makineleri (yedekleme ve geri yükleme) Azure MySQL Server örneğine nasıl geçirir gösterir. | Kullanılabilir

Bu makalede kullanılan örnek uygulaması kullanmak istediğiniz, açık kaynak olarak sağlanır ve buradan indirebilirsiniz [github](https://github.com/Microsoft/SmartHotel360).

## <a name="overview"></a>Genel Bakış

Azure geçirmeyi düşünün gibi Contoso şirket, şirket içi iş yüklerini buluta geçiş için uygun olup olmadığını anlamak için teknik ve finansal değerlendirmesini çalıştırmak istiyor. Özellikle, Contoso ekip geçiş için makine ve veritabanı uyumluluğu değerlendirmek ve kapasite ve kaynaklarını Azure'da çalışan maliyetlerini tahmin etmek istiyorsunuz.

Kendi ayak ıslak almak ve, iki kendi şirket içi uygulamaların değerlendirmek için oluşturacağız yer alan teknolojiler daha iyi anlamak için aşağıdaki tabloda özetlenmiştir. Bunlar için geçiş senaryoları bu geçiş için yeniden barındırma ve düzenleme uygulamaları değerlendiriliyor olduğunu unutmayın. Yeniden barındırma ve içinde yeniden düzenleme hakkında daha fazla bilgi [Contoso geçişine genel bakış](contoso-migration-overview.md).

**Uygulama adı** | **Platform** | **Uygulama katmanları** | **Ayrıntılar**
--- | --- | --- | ---
SmartHotel<br/><br/> Contoso seyahat gereksinimlerini yönetir | SQL Server veritabanı ile Windows üzerinde çalışan | Bir VM'yi (WEBVM) ve başka bir VM (SQLVM üzerinde) çalışan SQL Server üzerinde çalışan ön uç ASP.NET Web sitesi ile iki katmanlı uygulama | VM VMware vCenter sunucusu tarafından yönetilen bir ESXi ana bilgisayar üzerinde çalışan, ' dir.<br/><br/> Örnek uygulaması yüklenebilir [github](https://github.com/Microsoft/SmartHotel360).
OSTicket<br/><br/> Contoso hizmet Masası uygulaması | Linux/Apache, MySQL PHP (AMPUL) ile çalışıyor. | Bir VM'yi (OSTICKETWEB) ve MySQL veritabanı başka bir VM (OSTICKETMYSQL üzerinde) çalıştıran bir ön uç PHP Web sitesi ile iki katmanlı uygulama | Uygulama, iç çalışanlar ve dış müşterileri için sorunları izlemek için müşteri hizmeti uygulamalar tarafından kullanılır.<br/><br/> Örnek uygulaması yüklenebilir [GitHub](https://github.com/osTicket/osTicket).

## <a name="current-architecture"></a>Geçerli mimari


Geçerli Contoso şirket içi altyapı gösteren bir diyagram aşağıdadır.

![Contoso mimarisi](./media/contoso-migration-assessment/contoso-architecture.png)  

- Contoso içinde Şehir, New York, Doğu Amerika Birleşik Devletleri bulunan bir ana veri merkezi vardır.
- Amerika Birleşik Devletleri arasında üç ek yerel dalları sahiptirler.
- Ana veri merkezi bir fiber metro ethernet bağlantısı (500 MB/sn) ile Internet'e bağlı.
- Her dal Internet'e IPSec VPN tünelleri dön ana veri merkezi ile iş sınıfı bağlantıları kullanarak yerel olarak bağlı. Böylece, kalıcı olarak bağlı olması, tüm ağ ve internet bağlantısı en iyi duruma getirir.
- Ana veri merkezi, VMware ile tam olarak sanallaştırılmış. VCenter Server 6.5 tarafından yönetilen iki ESXi 6.5 sanallaştırma ana bilgisayarı sahiptirler.
- Contoso Active Directory kimlik yönetimi ve iç ağdaki DNS sunucuları için kullanır.
- Etki alanı denetleyicileri veri merkezindeki VMware VM'ler üzerinde çalıştırın. Yerel dalları etki alanı denetleyicilerde fiziksel sunucularda çalıştırın.





## <a name="business-drivers"></a>İş sürücüleri

BT liderlik ekibindeki yakından iş ile bu geçiş elde etmek istediği anlamak için iş ortaklarıyla çalışmıştır:

- **Adres iş büyümesi**: Contoso artmaktadır ve sonuç olarak şirket içi sistemlerini ve altyapı Basıncı yoktur.
- **Verimliliği artırma**: Contoso gereken gereksiz yordamları kaldırmak ve işlemler, geliştiriciler ve kullanıcılar için verimli hale getirmek.  Hızlı olarak iş gereksinimlerini BT ve değil atık saat veya para, böylece daha hızlı müşteri gereksinimlerine gönderiliyor.
- **Çeviklik artırmak**: Contoso BT işletme ihtiyaçlarını daha iyi yanıt olması gerekir. Market'te başarılı şekilde bir genel ekonomi etkinleştirmek için değişiklikleri daha hızlı tepki mümkün olması gerekir.  Bu dpm'nin biçiminde alın veya bir iş engelleyici haline gelir.
- **Ölçek**: işi başarıyla büyüdükçe, Contoso BT aynı yükselmeye sorgulayabilmesi sistemleri sağlamanız gerekir.

## <a name="assessment-goals"></a>Değerlendirme amaçları

Contoso bulut takım kendi geçiş değerlendirmeleri hedeflerini aşağı sabitlenmiş:

- Bugün şirket içi VMWare ortamlarında olduğu gibi geçişten sonra aynı performans özellikleri azure'daki uygulamalara sahip olmalıdır.  Buluta geçiş uygulama performansı daha az önemli olduğu anlamına gelmez.
- Contoso uyumluluğunu kullanıcıların uygulamalara ve veritabanlarını kendi barındırma seçenekleri azure'da yanı sıra Azure gereksinimleri anlamak gerekir.
- Uygulamaları buluta taşıdıktan sonra Contoso'nun veritabanı yönetim en aza.  
- Contoso değil yalnızca geçiş seçenekleri, aynı zamanda buluta taşındıktan sonra altyapısı ile ilişkili maliyetleri anlamak istiyor.

## <a name="assessment-tools"></a>Değerlendirme araçları
Contoso Microsoft araçları değerlendirmesi için kullanıyor. Bu araçlar hedeflerine ile hizalayın ve bunları ihtiyaç duydukları tüm bilgilerle sağlamalıdır.

**Teknoloji** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı geçiş Yardımcısı (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA değerlendirmek ve veritabanı işlevleri Azure etkileyebilecek uyumluluk sorunları algılamak için kullandıkları. DMA SQL kaynakları ve hedefleri arasında özellik eşliği değerlendirir ve performans ve güvenilirlik geliştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır.
[Azure Geçişi](https://docs.microsoft.com/azure/migrate/migrate-overview) | Contoso VMware Vm'leri değerlendirmek için bu hizmeti kullanır. Makinelerin geçiş uygunluğunu değerlendirir ve Azure’da çalıştırmaya ilişkin boyutlandırma ve maliyet tahminleri sağlar.  | Yok 2018 için ücret ödemeden kullanmakta bu hizmet.
[Hizmet Eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Geçişi, geçirmek istediğiniz makineler arasındaki bağımlılıkları göstermek için Hizmet Eşlemesini kullanır. |  Hizmet Eşlemesi, Azure Log Analytics’in bir parçasıdır. Şu an ücretsiz olarak 180 gün kullanılabilir.

Bu senaryoda, Contoso indirir ve şirket içi SQL Server veritabanı, seyahat uygulama için değerlendirmek için DMA çalıştırır. Azure kullandıkları Azure geçişten önce VM'ler, uygulama değerlendirmek için bağımlılık eşleme ile geçirin.



## <a name="assessment-architecture"></a>Değerlendirme mimarisi


![Geçiş değerlendirmesi mimarisi](./media/contoso-migration-assessment/migration-assessment-architecture.png)

- Contoso, tipik kurumsal kuruluşu temsil eden kurgusal bir addır. 
- Contoso olan bir şirket içi veri merkezi (**contoso datacenter**), şirket içi etki alanı denetleyicileriyle (contosodc1 adlı, CONTOSODC2).
- VMware Vm'leri VMware ESXI üzerinde bulunan sürüm 6.5 çalıştıran konaklar. Ana: **contosohost1**, **contosohost2**
- VMware ortamı 6.5 vCenter server tarafından yönetilen (**venter**, bir VM üzerinde çalışır.
- SmartHotel seyahat uygulama:
    - İki VMware VM uygulama katmanlı **WEBVM** ve **SQLVM**.
    - Sanal makineleri VMware ESXi ana bilgisayarda bulunan **contosohost1.contoso.com**.
    - Sanal makineleri Windows Server 2008 R2 Datacenter SP1 ile çalışıyor.
- VMware ortamı bir sanal makine üzerinde çalıştırılan vCenter Server (**vcenter.contoso.com**) tarafından yönetilir.
- OSTicket hizmet Masası uygulama:
    - Uygulama iki VM'ler üzerindeki katmanlı **OSTICKETWEB** ve **OSTICKETMYSQL**.
    - Sanal makineleri Ubuntu Linux Server 16.04-LTS üzerinde çalışıyor.
    - OSTICKETWEB VM Apache 2 ve PHP 7.0 çalışıyor.
    - OSTICKETMYSQL VM MySQL 5.7.22 çalışıyor.

![Mimari](./media/contoso-migration-assessment/architecture.png)


## <a name="prerequisites"></a>Önkoşullar

Contoso (ve) değerlendirmesi için ihtiyaç duyduğu aşağıda verilmiştir:

- Sahibi veya katkıda erişim Azure aboneliği için ya da Azure Abonelikteki bir kaynak grubu için.
- 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir şirket içi vCenter sunucusu.
- vCenter sunucusunda salt okunur bir hesap veya hesap oluşturma izinleri.
- .OVA şablonu kullanarak, vCenter sunucusu üzerinde sanal makine oluşturma izni.
- 5.0 veya üzeri bir sürümü çalıştıran en az bir ESXi ana bilgisayarı.
- Biri, SQL Server veritabanı çalıştıran en az iki şirket içi VMware sanal makinesi.
- Her VM Azure geçirmek aracıları yüklemek için izinler.
- Sanal makinelerin doğrudan İnternet bağlantısı olmalıdır.
        - İnternet erişimini, [gerekli URL’ler](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) ile sınırlayabilirsiniz.
        -İf internet bağlantısı, makinelerle [OMS ağ geçidi](../log-analytics/log-analytics-oms-gateway.md) bunlar üzerinde yüklü olması gerekir.
- Veritabanı değerlendirmesi için SQL Server örneğini çalıştıran sanal makinenin FQDN’si.
- DMA’nın bağlanabilmesi için SQL Server sanal makinesinde çalışan Windows Güvenlik Duvarı, 1433 numaralı TCP bağlantı noktasında harici bağlantılara izin vermelidir.


## <a name="assessment-overview"></a>Değerlendirme genel bakış

Burada'nın nasıl Contoso giderek değerlendirme yapmak için:


> [!div class="checklist"]
> * **1. adım: DMA yükleyip**: DMA şirket içi SQL Server veritabanının değerlendirmesi için hazırlayın.
> * **2. adım: DMA veritabanıyla değerlendirmek**: çalıştırın ve veritabanı değerlendirme analiz edin.
> * **3. adım: Azure geçirme ile VM değerlendirmesi için hazırlama**: şirket içi hesaplarınızı ve VMware ince ayar ayarları ayarlayın.
> * **4. adım: Azure geçirme ile şirket içi sanal makineleri Bul**: bir Azure geçirmek toplayıcısı VM oluşturun. Daha sonra değerlendirmesi için sanal makineleri bulmak için toplayıcı çalıştırın.
> * **5. adım: Azure geçirme ile bağımlılık çözümlemesini hazırlama**: Azure geçirmek yükleme aracıları VM'ler VMs arasındaki bağımlılık eşlemeyi görebilmeniz için.
> * **6. adım: Azure geçirmek VM'lerin değerlendirmek**: denetleyin bağımlılıkları, sanal makineleri grup ve değerlendirme çalıştırın. Değerlendirme hazır olduktan sonra geçiş için Hazırlanmakta analiz edin.


## <a name="step-1-download-and-install-the-dma"></a>1. adım: DMA yükleyip

1. Contoso indirmeleri gelen DMA [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=53595).
    - Yardımcısı, bir SQL örneğine bağlanan herhangi bir makinede yüklenebilir. SQL Server makinesinde çalıştırmanız gerekmez.
    - SQL Server ana makinede çalıştırılması gerekir.
2. Bunlar (yüklemeyi başlatmak için DownloadMigrationAssistant.msi), indirilen kurulum dosyasını çalıştırın.
3. Üzerinde **son** sayfasında, seçtikleri **başlatma Microsoft veri geçiş Yardımcısı** Sihirbazı tamamlamadan önce.

## <a name="step-2-run-and-analyze-the-database-assessment-for-smarthotel"></a>2. adım: Çalıştırın ve SmartHotel veritabanı değerlendirme analiz edin

Şimdi Contoso SmartHotel uygulama için kendi şirket içi SQL Server analiz etmek için bir değerlendirme çalıştırabilirsiniz.

1. Veritabanı geçiş Yardımcısı'nda, bunlar tıklatın **yeni**seçin **değerlendirme**ve bir proje adı - değerlendirme verin **SmartHotel**.
2. Seçtikleri **kaynak sunucu türünü** olarak **Azure Virtual Machines'de SQL Server**. 

    ![Kaynak seçme](./media/contoso-migration-assessment/dma-assessment-1.png)

    > [!NOTE]
      Şu anda DMA, SQL Yönetilen Örneği’ne geçiş için değerlendirmeyi desteklemez. Geçici bir çözüm olarak, Contoso SQL Server üzerinde Azure VM beklenen hedef olarak değerlendirmesi için kullanıyor.

3. İçinde **seçin hedef sürüm**, SQL Server 2017 hedef sürüm olarak seçerler. Bunlar, SQL yönetilen örneği tarafından kullanılan sürüm olduğundan bu seçmeniz gerekir.
4. Uyumluluk ve yeni özellikler hakkında bilgi bulmak için seçin:
    - **Uyumluluk sorunları** geçiş bölün veya geçiş öncesinde ikincil bir düzeltme gerektiren değişiklikler unutmayın. Kullanım dışı bırakılan tüm özellikleri hakkında şu anda kullanımda haberdar olmasını sağlar. Sorunlar, uyumluluk düzeyine göre düzenlenir.
    - **Yeni özellikler öneri** geçişten sonra veritabanı için kullanılabilir bir hedef SQL Server platformuna yeni özellikler belirtilmiştir. Bunlar, Performansa, Güvenliğe ve Depolamaya göre düzenlenir.

    ![Hedef seçin](./media/contoso-migration-assessment/dma-assessment-2.png)

2. İçinde **bir sunucuya Bağlan**, erişmek için veritabanı ve kimlik bilgilerini çalışan VM adını girin. Etkinleştirmek ihtiyaç duydukları **güven sunucu sertifikası** SQL Server'a elde emin olmak için. Bunlar ardından **Bağlan**.

    ![Hedef seçin](./media/contoso-migration-assessment/dma-assessment-3.png)

3. İçinde **Ekle kaynak**, istedikleri değerlendirmek ve veritabanı Ekle **sonraki** değerlendirme başlatmak için.
4. Değerlendirme oluşturulur.
    
    ![Değerlendirme oluşturma](./media/contoso-migration-assessment/dma-assessment-4.png)

5. İçinde **İnceleme sonuçları**, değerlendirme sonuçlarını görebilirsiniz.


### <a name="analyze-the-database-assessment"></a>Veritabanı değerlendirmesini analiz etme

Kullanılabilir hemen sonuçları görüntülenir. Sorunları giderin bunlar tıklatın gerek **yeniden değerlendirme** değerlendirme yeniden çalıştırmak için.

1. İçinde **uyumluluk sorunları** rapor, her uyumluluk düzeyine sahip herhangi bir sorunu kontrol edin. Uyumluluk düzeyleri, SQL Server sürümleriyle aşağıdaki şekilde eşlenir:

    - 100: SQL Server 2008/Azure SQL Veritabanı
    - 110: SQL Server 2012/Azure SQL Veritabanı
    - 120: SQL Server 2014/Azure SQL Veritabanı
    - 130: SQL Server 2016/Azure SQL Veritabanı
    - 140: SQL Server 2017/Azure SQL Veritabanı

    ![Uyumluluk sorunları](./media/contoso-migration-assessment/dma-assessment-5.png)

2. İçinde **özellik önerileri** raporu, Contoso geçişten sonra değerlendirme önerir performans, güvenlik ve depolama özelliklerini görüntüleyebilirsiniz. Çeşitli özellikler önerilir, bellek içi OLTP ve Columnstore, Esnetme veritabanı, her zaman şifreli, dinamik veri maskeleme ve saydam veri şifreleme (TDE) dahil olmak üzere.

    ![Özellik önerileri](./media/contoso-migration-assessment/dma-assessment-6.png)

    > [!NOTE]
    > Öneririz Contoso [TDE sağlayan](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) için tüm SQL Server veritabanları ve bu yapıldığında daha kritik bulutta veritabanlarıdır. TDE yalnızca geçişten sonra etkinleştirilmelidir. TDE zaten etkin değilse sertifika veya asimetrik anahtar hedef sunucunun ana veritabanına taşımanız gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).

2. Bunlar, değerlendirme JSON veya CSV biçiminde dışarı aktarabilirsiniz.

Büyük ölçek değerlendirme çalıştırıyorsanız yapabilecekleriniz dikkat edin:

- Birden çok değerlendirmeleri aynı anda çalıştırmak ve açarak değerlendirmeleri durumunu görüntülemek **tüm değerlendirmeleri** sayfası.
- [Bir SQL Server veritabanına değerlendirmeleri birleştirmek](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Powerbı raporun değerlendirmeleri birleştirmek](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-3-prepare-for-vm-assessment-with-azure-migrate"></a>3. adım: Azure geçirme ile VM değerlendirmesi için hazırlama

Contoso Azure geçirmek kullanacağı otomatik olarak Vm'leri için değerlendirmesi bulma, bir VM oluşturmak için izinleri doğrulamak için bir VMware hesabı oluşturmak için açılması gereken bağlantı noktalarını unutmayın ve istatistikleri ayarları düzeyini ayarlayın.

### <a name="set-up-a-vmware-account"></a>VMware hesabı ayarlama

 VM bulma aşağıdaki özelliklere sahip vCenter salt okunur bir hesapta gerektirir: 

- Kullanıcı türü: En azından salt okunur bir kullanıcı.
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur.
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

### <a name="verify-permissions-to-create-a-vm"></a>Sanal makine oluşturma izinlerini doğrulama

Contoso, bir dosyada içeri aktararak bir VM oluşturmak için izinlere sahip olduğunu doğrulayın. OVA biçimi. [Daha fazla bilgi edinin](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Bağlantı noktalarını doğrulama

Contoso değerlendirme bağımlılık eşleme kullanır. Bu özellik, değerlendirmek istediğiniz Vm'lere yüklü bir aracı gerektirir. Aracıyı Azure için TCP 443 numaralı bağlantı noktası her VM bağlanması gerekir. Bağlantı gereksinimleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).


### <a name="set-statistics-settings"></a>İstatistik ayarlarını yapma

Dağıtıma başlamadan önce Contoso düzeyi 3 vCenter sunucusu istatistikleri ayarlarını ayarlamanız gerekir. Şunlara dikkat edin:

- Düzeyi ayarladıktan sonra en az bir gün değerlendirme çalıştırmadan önce beklemeniz gerekir. Aksi takdirde beklendiği gibi çalışmayabilir.
- Düzey, 3’ten yüksekse değerlendirme çalışır ancak:
    - Diskler ve ağ iletişimi için performans verileri toplanmaz.
    - Depolama için Azure Geçişi, Azure’da şirket içi diskle aynı boyutta standart bir disk önerir.
    - Ağ iletişimi için, her şirket içi ağ bağdaştırıcısına yönelik olarak Azure’da bir ağ bağdaştırıcısı önerilir.
    - İşlem için Azure Geçişi, sanal makine çekirdeklerine ve bellek boyutuna bakar ve aynı yapılandırmaya sahip bir Azure sanal makinesi önerir. Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.
- Düzey 3 ile boyutlandırma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Bunlar düzeyini aşağıdaki gibi ayarlayın:

1. VSphere Web istemcisi bunlar vCenter server örneği açın.
2. İçinde **Yönet** > **ayarları** > **genel**, bunlar tıklatın **Düzenle**.
3. İçinde **istatistikleri**, bunlar istatistiği düzeyi ayarları ayarlamak **Düzey 3**.

    ![vCenter istatistikleri düzeyi](./media/contoso-migration-assessment/vcenter-statistics-level.png)



## <a name="step-4-discover-vms"></a>4. adım: Sanal makineleri Bul

Sanal makineleri bulmak için Azure geçirmek projesinde Contoso oluşturur. Bunlar indirin ve VM Toplayıcı ve şirket içi Vm'leri bulmak için toplayıcı çalıştırın.

### <a name="create-a-project"></a>Proje oluşturma

1. İçinde [Azure portal](https://portal.azure.com), için arama yapın **Azure geçirmek**ve bir proje (ContosoMigration) oluşturun.
2. Azure aboneliği bir proje adı belirtin ve yeni bir Azure kaynak grubu oluşturma **ContosoFailoverRG**. Şunlara dikkat edin:

    - Bir Azure Geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz.
    - Herhangi bir hedef konumdan geçiş planlayabilirsiniz.
    - Proje konumu yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır.

    ![Azure Geçişi](./media/contoso-migration-assessment/project-1.png)


### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı Gereci ayarlamak için Contoso indirmeleri bir. OVA şablon ve VM oluşturmak için şirket içi vCenter sunucusuna aktarır.

1. Azure geçirmek projesinde > **Başlarken** > **bulma & değerlendirin** > **Bul makineler**, indirirler +. OVA şablon dosyası.
2. Bunlar, proje kimliği ve anahtarı kopyalayın. Bu, Toplayıcı yapılandırmak için gereklidir.

    ![.ova dosyasını indir](./media/contoso-migration-assessment/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

VM dağıtmadan önce Contoso olduğunu denetler. OVA dosyası güvenlidir.

1. Üzerinde dosya karşıdan makinede oldukları bir yönetici komut penceresi açın.
2. Bunlar için OVA karma oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara uygun olmalıdır (sürüm 1.0.9.7)

    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c


### <a name="create-the-collector-appliance"></a>Toplayıcı gereci oluşturma

Şimdi Contoso vCenter sunucusuna indirilen dosya alma ve yapılandırma sunucusu VM sağlayın.

1. Bunlar vSphere istemci konsolunda tıklatın **dosya** > **OVF şablon dağıtma**.

    ![OVF dağıtma](./media/contoso-migration-assessment/vcenter-wizard.png)

2. OVF şablon Dağıtma Sihirbazı'nda > **kaynak**, konumunu belirtin. OVA dosyası.
3. İçinde **ad ve konum**, Toplayıcı VM için bir kolay ad ve hangi VM barındırılacağı stok konumunu belirtin. Bunlar ayrıca konağa veya kümeye Toplayıcı Gereci çalışacağı belirtin.
5. İçinde **depolama**, depolama konumu belirtin ve **Disk biçimi**, depolama sağlamak istedikleri nasıl.
7. İçinde **ağ eşlemesi**, VM Toplayıcı bağlanacağı ağ belirtin. Meta verileri Azure’a göndermek için ağ, İnternet bağlantısına sahip olmalıdır.
8. Ayarları gözden geçirin ve seçin **dağıtımdan sonra güç üzerinde**> **son**. Gereç oluşturulduktan sonra tamamlamanın başarılı olduğunu onaylayan bir ileti görüntülenir.

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Şimdi sanal makineleri bulmak için toplayıcı çalıştırın. Toplayıcı şu anda yalnızca "İngilizce (ABD)" işletim sistemi dilini ve Toplayıcı arabirimi desteklediğini unutmayın.

1. VSphere istemci Konsolu > **açık Konsolu**, dil, saat dilimi ve VM Toplayıcı parola tercihlerini belirtin.
2. Bunlar masaüstünde **Toplayıcı çalıştırmak** kısayol.

    ![Toplayıcı kısayolu](./media/contoso-migration-assessment/collector-shortcut.png)

4. Azure geçirmek Toplayıcı içinde > **önkoşulları ayarlama**, lisans koşullarını kabul edin ve üçüncü taraf bilgileri okuyun.
5. Toplayıcı VM Internet erişimi, zaman eşitlenir ve Toplayıcı hizmetinin çalıştığından emin olup olmadığını denetler (bunu VM üzerinde varsayılan olarak yüklenir). Ayrıca, VMWare Powerclı yükler.

    > [!NOTE]
    > Sanal makinenin, ara sunucu olmadan İnternet’e doğrudan erişiminin olduğunu varsayıyoruz.

    ![Önkoşulları doğrulama](./media/contoso-migration-assessment/collector-verify-prereqs.png)


5. İçinde **vCenter sunucusu ayrıntılarını belirtin**adı (FQDN) veya vCenter sunucusunun IP adresi belirtin ve salt okunur kimlik bilgilerinin bulma için kullanılır.
7. Bunlar, VM keşfi için kapsamı seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam en fazla 1500 VM’yi içermelidir.

    ![vCenter’a bağlanma](./media/contoso-migration-assessment/collector-connect-vcenter.png)

6. İçinde **belirt geçiş proje**, Azure geçirmek proje kimliği ve portalından kopyalandığından anahtarı belirtin. Yeniden projede edinebilirsiniz **genel bakış** sayfa > **Bul makineler**.  

    ![Azure'a Bağlanma](./media/contoso-migration-assessment/collector-connect-azure.png)

7. İçinde **koleksiyonu ilerlemeyi görüntüleme** Contoso izleme bulma ve VM'lerin toplanan meta verilerin kapsamında olduğunu denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

    ![Koleksiyon işlemi sürüyor](./media/contoso-migration-assessment/collector-collection-process.png) 



### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Koleksiyon tamamlandıktan sonra sanal makineleri portalda görünür Contoso denetler.

1. Azure geçirmek Proje > **Yönet** > **makineler**, bulmak istediğiniz sanal makineleri olup olmadığını denetleyin.

    ![Bulunan makineler](./media/contoso-migration-assessment/discovery-complete.png)

3. Makinelerde şu anda Azure Geçişi aracılarının yüklü olmadığını unutmayın. Contoso görüntülemek için bağımlılıklar yüklemeniz gerekir.

    ![Bulunan makineler](./media/contoso-migration-assessment/machines-no-agent.png)



## <a name="step-5-prepare-for-dependency-analysis"></a>5. adım: bağımlılık analiz için hazırlama

Erişmek istediğiniz Contoso VMs arasındaki bağımlılıkları görüntülemek için indirin ve aracıları uygulamasını VM'ler yükleyin. Contoso tüm vm'lerde uygulamalarını, Windows ve Linux için bunu yapar.

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Bunlar, aracıları yüklenmeden önce bir anlık görüntü uygulayarak değiştirmeden önce VM kopyasını tutun.

![Makine anlık görüntüsü](./media/contoso-migration-assessment/snapshot-vm.png)


### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme

1. Üzerinde **makineler** sayfasında, makine seçin ve ardından **yükleme gerektirir** içinde **bağımlılıkları** sütun.
2. Üzerinde **Bul makineler** sayfasında, aşağıdakileri yapın:
    - Her bir Windows VM MMA ve bağımlılık Aracısı'nı indirme
    - Her bir Linux VM MMA ve bağımlılık Aracısı'nı indirme
3. Şimdi bunların çalışma alanı kimliği ve anahtarı kopyalayın. Bunlar MMA yüklerken ihtiyaç duyar.

    ![Aracıyı indirme](./media/contoso-migration-assessment/download-agents.png)

### <a name="install-the-agents-on-windows-vms"></a>Windows sanal makinelerin aracılarını yükleme

Bunlar, her VM yüklemeyi çalıştırın.

#### <a name="install-the-mma-on-windows-vms"></a>Windows Vm'lerinde MMA yükleyin

1. Bunlar, indirilen Aracısı'nı çift tıklatın.
2. İçinde **hedef klasörü**, varsayılan yükleme klasörü tutmak > **sonraki**.
2. İçinde **aracı Kur Seçenekleri**, seçtikleri **aracıyı Azure günlük Analizi'ne bağlayın** > **sonraki**.

    ![MMA yüklemesi](./media/contoso-migration-assessment/mma-install.png)
    
5. İçinde **Azure günlük analizi**, bunlar portaldan kopyaladığınız anahtar ve çalışma alanı kimliği yapıştırın. 

    ![MMA yüklemesi](./media/contoso-migration-assessment/mma-install2.png)

6. İçinde **yüklemeye hazır**, MMA şimdi yükleyebilirsiniz.

#### <a name="install-the-dependency-agent-on-windows-vms"></a>Windows sanal makinelerin bağımlılık Aracısı'nı yükleme

1. Bunlar, indirilen bağımlılık Aracısı'nı çift tıklatın.
2. Bunlar, lisans koşullarını kabul edin ve yüklemesinin tamamlanması için bekleyin.

    ![Bağımlılık aracısı](./media/contoso-migration-assessment/dependency-agent.png)


### <a name="install-the-agents-on-linux-vms"></a>Linux VM'ler aracılarını yükleme

Bunlar, her VM yüklemeyi çalıştırın.

#### <a name="install-the-mma-on-linux-vms"></a>Linux VM'ler üzerinde MMA yükleyin

1. Her VM kullanarak python ctypes kitaplığı yükledikleri: **apt sudo get yüklemek python ctypeslib**.
2. Bunlar, kök olarak MMA Aracısı'nı yüklemek için komutu çalıştırmanız gerekir.  Kök Çalıştır olmasını, aşağıdaki komut ve kök parolayı girin: **sudo -i**.
3. Şimdi MMA Aracısı'nı yükleyin.
    - Doğru çalışma alanı kimliği ve anahtarı komuta ekleyin.
    - 64 bit için komutlardır.
    - **Çalışma alanı kimliği** ve **birincil anahtar** OMS portalı içinde bulunabilir > **ayarları**, **bağlı kaynakları** sekmesi.
    - OMS Aracısı'nı indirme, sağlama toplamı ve yükleme/yerleşik aracı doğrulamak için aşağıdaki komutları çalıştırın.

    ```
    wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w 6b7fcaff-7efb-4356-ae06-516cacf5e25d -s k7gAMAw5Bk8pFVUTZKmk2lG4eUciswzWfYLDTxGcD8pcyc4oT8c6ZRgsMy3MmsQSHuSOcmBUsCjoRiG2x9A8Mg==
    ```
 


#### <a name="install-the-dependency-agent-on-linux-vms"></a>Linux VM'ler üzerinde bağımlılık Aracısı'nı yükleme

MMA yüklendikten sonra Contoso Linux sanal makinelerin bağımlılık Aracısı'nı yükleyebilirsiniz.

1. Bağımlılık Aracısı'nı InstallDependencyAgent Linux64.bin, kendiliğinden açılan bir ikili içeren bir kabuk betiği kullanarak Linux bilgisayarlara yüklenir. Bunlar Paylaş kullanarak dosyasını çalıştırın veya ekleme yürütme dosya izinlerinin.

2. Bunlar, kök olarak Linux bağımlılık Aracısı'nı yükleyin:

    ```
    wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin && sudo sh InstallDependencyAgent-Linux64.bin -s
    ```


## <a name="step-6-run-and-analyze-the-vm-assessment"></a>6. adım: Çalıştırın ve VM değerlendirme analiz edin

Contoso şimdi makine bağımlılıkları doğrulayın ve bir grup oluşturun. Ardından, grubu için değerlendirme çalıştırın.

### <a name="verify-dependencies-and-create-a-group"></a>Bir grup oluşturun ve bağımlılıkları doğrulayın


1. Makineler çözümlemek için bunlar tıklatın **bağımlılıklarını görüntüleme**.

    ![Makine bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/view-machine-dependencies.png)

2. SQLVM için bağımlılık eşlemi, aşağıdaki ayrıntıları gösterir:

    - Belirtilen süre (varsayılan olarak 1 saat) boyunca SQLVM’de çalıştırılan etkin ağ bağlantılarıyla grupları/işlemleri işleme
    - Tüm bağımlı makinelere/makinelerden gelen (istemci) ve giden (sunucu) TCP bağlantıları.
    - Azure Geçişi aracılarının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - Aracıların yüklü olmadığı makineler, bağlantı noktası ve IP adresi bilgilerini gösterir.

3. Aracısının (WEBVM) yüklü olduğu makineler için bunlar makine kutusunda FQDN, işletim sistemi ve MAC adresi dahil daha fazla bilgi görüntülemek için tıklatın.

    ![Grup bağımlılıklarını görüntüleme](./media/contoso-migration-assessment/sqlvm-dependencies.png)

4. Şimdi (SQLVM ve WEBVM) grubuna eklemek için sanal makineleri seçin.  CTRL kullanın + birden çok VM seçmek için tıklatın.
5. Bunlar tıklatın **Grup Oluştur**ve bir ad (smarthotelapp) belirtin.

> [!NOTE]
    > Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını genişletebilirsiniz. Belirli bir süre veya başlangıç ve bitiş tarihleri seçebilirsiniz.


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma


1. Üzerinde **grupları** sayfasında, grubun (smarthotelapp) açın ve tıklatın **değerlendirmesi oluşturma**.

    ![Değerlendirme oluşturma](./media/contoso-migration-assessment/run-vm-assessment.png)

2. Değerlendirme, **Yönet** > **Değerlendirmeler** sayfasında görüntülenir.

Contoso varsayılan değerlendirme ayarları kullanılır, ancak ayarlarını özelleştirebilirsiniz. [Daha fazla bilgi edinin](how-to-modify-assessment.md).



### <a name="analyze-the-vm-assessment"></a>Sanal makine değerlendirmesini analiz etme

Bir Azure geçirmek değerlendirme bilgilerdir Azure için şirket içi sanal makineleri uyumluluğu hakkında boyutlandırmaya Azure VM için önerilen ve tahmini aylık Azure maliyetleri.

![Değerlendirme raporu](./media/contoso-migration-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

![Değerlendirme görüntüsü](./media/contoso-migration-assessment/assessment-display.png)

Bir değerlendirme güvenirlik derecesi 1 yıldızdan 5 yıldız (en düşük olan 1 yıldız ve 5 yıldız en yüksek olan) alır.
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Derecelendirme, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- GÜVENİRLİK derecelendirme olduğunda yararlı yapmakta olduğunuz *performans tabanlı boyutlandırma* gibi Azure geçirmek kullanım tabanlı boyutlandırma yapmak için yeterli veri noktası olmayabilir. Azure Geçişi, VM’yi boyutlandırmak için ihtiyaç duyduğu tüm veri noktalarına sahip olduğundan, *şirket içi boyutlandırma* için güvenilirlik derecesi her zaman 5 yıldızdır.
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


## <a name="clean-up-after-assessment"></a>Değerlendirme temizleme

- Değerlendirme tamamlandıktan sonra Contoso Azure geçiş Gereci gelecekteki değerlendirmeleri için korur.
- Bunlar, VM VMware devre dışı bırakın. İlave VM'ler değerlendirirken, yeniden başlayacağız.
- Bunlar Contoso geçiş projeyi Azure'da tutarsınız.  Şu anda ContosoFailoverRG kaynak grubu, Doğu ABD Azure bölgesindeki dağıtılır.
-  VM Toplayıcı 180 günlük değerlendirme lisansına sahip. Bu sınırı süresi dolarsa indirip yeniden Toplayıcı gerekir.


## <a name="conclusion"></a>Sonuç

Bu senaryoda, Contoso DMA Aracı'nı ve Azure geçiş hizmetini kullanarak şirket içi sanal makineleri kullanarak SmartHotel uygulama veritabanını uygunluk. Bunlar şirket içi kaynakları geçiş Azure için hazır olduğundan emin olmak için Değerlendirmeler gözden.

## <a name="next-steps"></a>Sonraki adımlar

Bu serideki sonraki makalede Contoso Azure kendi SmartHotel uygulamada yükseltme shift geçiş ile rehosts. Contoso Azure Site Recovery ve bir Azure SQL örneğine yönetilen veritabanı geçiş hizmetini kullanarak, uygulama veritabanını kullanarak uygulama için ön uç WEBVM geçirir. [Başlama](contoso-migration-rehost-vm-sql-managed-instance.md) bu dağıtım ile.
