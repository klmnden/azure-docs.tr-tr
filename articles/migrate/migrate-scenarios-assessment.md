---
title: DMA ve Azure Geçişi ile Azure’a geçiş için şirket içi iş yüklerini değerlendirme | Microsoft Docs
description: Data Migration Yardımcısı (DMA) ve Azure Geçişi hizmetini kullanarak şirket içi makinelerin geçişi için Azure’ın nasıl hazırlanacağını öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 04/16/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 33e31c47a6125ac363410a9a78e9c9310c74d51e
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="scenario-1-assess-on-premises-workloads-for-migration-to-azure"></a>Senaryo 1: Azure’a geçiş için şirket içi iş yüklerini değerlendirme

Azure’a geçiş ile ilgili olarak Contoso şirketi, şirket içi iş yüklerinin buluta geçiş için uygun olup olmadığını öğrenmek amacıyla teknik ve finansal iç değerlendirme çalıştırmak istiyor. Özellikle geçiş için makine ve veritabanı uyumluluğunu değerlendirmek, Azure’da kaynaklarını çalıştırma maliyetlerini ve kapasitesini tahmin etmek istiyor.

Söz konusu teknolojileri ilk kez kullanmak ve daha iyi anlamak için küçük bir şirket içi seyahat uygulamasını değerlendirip geçiriyor. Bu, bir web uygulamasının bir sanal makinede ve bir SQL Server veritabanının ikinci bir sanal makinede çalıştığı, iki katmanlı bir uygulamadır. Uygulama, VMware içinde dağıtılır ve ortam, vCenter Server tarafından yönetilir. Data Migration Yardımcısı (DMA) ve Azure Geçişi hizmetini kullanarak değerlendirmeyi gerçekleştirir.

**Teknoloji** | **Açıklama** | **Maliyet**
--- | --- | ---
[DMA](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | DMA, Azure’daki veritabanı işlevselliğini etkileyebilecek uyumluluk sorunlarını değerlendirir ve algılar. Ayrıca değerlendirme yapıp SQL Server kaynağınız ile hedefiniz arasında eşleme gerçekleştirir ve hedef ortamınız için performans ve güvenilirlik iyileştirmeleri önerir. | Ücretsiz olarak indirilebilir bir araçtır. 
[Azure Geçişi](https://docs.microsoft.com/azure/migrate/migrate-overview) | Hizmet, Azure’a geçiş için şirket içi makineleri değerlendirmenize yardımcı olur. Makinelerin geçiş uygunluğunu değerlendirir ve Azure’da çalıştırmaya ilişkin boyutlandırma ve maliyet tahminleri sağlar. Şu anda, Azure Geçişi hizmeti, şirket içi VMware sanal makinelerini Azure’a geçiş için değerlendirebilir. | Şimdilik (Nisan 2018) bu hizmet ücretsiz kullanılabilir.
[Hizmet Eşlemesi](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-service-map) | Azure Geçişi, geçirmek istediğiniz makineler arasındaki bağımlılıkları göstermek için Hizmet Eşlemesini kullanır. |  Hizmet Eşlemesi, Azure Log Analytics’in bir parçasıdır. Şu an ücretsiz olarak 180 gün kullanılabilir. 

Bu senaryoda, seyahat uygulamamıza ilişkin şirket içi SQL Server veritabanını değerlendirmek için DMA’yı indirip çalıştıracağız. Uygulama sanal makinelerini Azure’a geçirmeden önce değerlendirmek için bağımlılık eşlemesi ile Azure geçişini kullanacağız.

> [!NOTE]
> Bu senaryo için, veritabanına yönelik değerlendirme hedefimiz, "Azure sanal makinesinde SQL Server" olacaktır. Ancak sonraki senaryo makalemizde, Azure SQL Yönetilen Örneğine geçişi çalıştıracağız. DMA şu anda bir Azure SQL Yönetilen Örneği hedefine yönelik değerlendirmeyi desteklemediğinden bu yaklaşımı kullanıyoruz.

## <a name="architecture"></a>Mimari

Bu senaryoda ayarlama yapacağız 

 ![Geçiş değerlendirmesi mimarisi](./media/migrate-scenarios-assessment/migration-assessment-architecture.png)

Bu senaryoda:
- Contoso, şirket içi etki alanı denetleyicisi ( **contosodc1**) ile bir şirket içi veri merkezi (**contoso-datacenter**) içeriyor.
- Dahili seyahat uygulaması, **WEBVM** ve **SQLVM** adlı iki sanal makinede katmanlanmış olup **contosohost1.contoso.com** adresindeki VMware ESXi ana bilgisayarında bulunur.
- VMware ortamı bir sanal makine üzerinde çalıştırılan vCenter Server (**vcenter.contoso.com**) tarafından yönetilir.




## <a name="prerequisites"></a>Ön koşullar

Bu senaryoyu dağıtmak için ihtiyacınız olanlar aşağıda verilmiştir:

- 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir şirket içi vCenter sunucusu.
- vCenter sunucusunda salt okunur bir hesap veya hesap oluşturma izinleri. 
- .OVA şablonu kullanarak, vCenter sunucusu üzerinde sanal makine oluşturma izni.
- 5.0 veya üzeri bir sürümü çalıştıran en az bir ESXi ana bilgisayarı.
- Biri, SQL Server veritabanı çalıştıran en az iki şirket içi VMware sanal makinesi.
- Her sanal makinede Azure Geçişi aracılarını yüklemek için izniniz olmalıdır.
- Sanal makinelerin doğrudan İnternet bağlantısı olmalıdır.
        - İnternet erişimini, [gerekli URL’ler](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-pre-requisites) ile sınırlayabilirsiniz.
        - İnternet bağlantısı olmayan makineleriniz varsa, bu makinelere [OMS ağ geçidini](../log-analytics/log-analytics-oms-gateway.md) indirip yüklemeniz gerekir.
- Veritabanı değerlendirmesi için SQL Server örneğini çalıştıran sanal makinenin FQDN’si.
- DMA’nın bağlanabilmesi için SQL Server sanal makinesinde çalışan Windows Güvenlik Duvarı, 1433 numaralı TCP bağlantı noktasında harici bağlantılara izin vermelidir.


## <a name="scenario-overview"></a>Senaryoya genel bakış

Yapacaklarımız aşağıda verilmiştir:


> [!div class="checklist"]
> * **1. Adım: Azure’ı hazırlama**. Bir Azure aboneliğine sahip olmamız gerekir.
> * **2. Adım: DMA’yı indirip yükleme**: Şirket içi SQL Server veritabanının değerlendirmesi için DMA’yı hazırlayın.
> * **3. Adım: Veritabanını değerlendirme**: Veritabanı değerlendirmesini çalıştırıp analiz edin.
> * **4. Adım: Azure Geçişi ile sanal makine değerlendirmesine hazırlanma**: Şirket içi hesapları ayarlayın ve VMware ayarlarını yapın.
> * **5. Adım: Şirket içi sanal makineleri bulma**: Azure Geçişi toplayıcısı sanal makinesi oluşturun. Daha sonra değerlendirme amacıyla sanal makineleri bulmak için toplayıcıyı çalıştırın.
> * **6. Adım: Bağımlılık analizine hazırlanma**: Sanal makineler arasındaki bağımlılık eşlemesini görebilmemiz için sanal makinelere Azure Geçişi aracılarını yükleyin.
> * **7. Adım: Sanal makineleri değerlendirme**: Bağımlılıkları denetleyin, sanal makineleri gruplayın ve değerlendirmeyi çalıştırın. Değerlendirme hazır olduktan sonra geçiş hazırlığında değerlendirmeyi analiz edin.


## <a name="step-1-prepare-azure"></a>1. Adım: Azure’ı hazırlama

Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

- Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.
- Var olan bir abonelik kullanıyorsanız ve yönetici değilseniz, abonelik için veya bu senaryoda kullandığınız kaynak grubu için size Sahip veya Katkıda Bulunan izinleri ataması amacıyla yöneticiyle birlikte çalışmanız gerekir.


## <a name="step-2-download-and-install-the-dma"></a>2. Adım: DMA’yı indirip yükleme

1. [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=53595)'nden DMA’yı indirin.
    - SQL örneğine bağlanabilen herhangi bir makineye Yardımcı’yı yükleyebilirsiniz. SQL Server makinesinde çalıştırmanız gerekmez.
    - Bunu SQL Server ana bilgisayar makinesinde çalıştırmamalısınız.
2. Yüklemeyi başlatmak için indirilen kurulum dosyasına (DownloadMigrationAssistant.msi) çift tıklayın.
3. **Son** sayfasında **Microsoft Data Migration Yardımcısı’nı Başlat** seçeneğinin belirlendiğinden emin olun ve **Son**’a tıklayın.

## <a name="step-3-run-and-analyze-the-database-assessment"></a>3. Adım: Veritabanı değerlendirmesini çalıştırma ve analiz etme

Kaynak SQL Server örneğinizi, belirtilen bir hedefe karşı analiz etmek için bir değerlendirme çalıştırın.

1. **Yeni** bölümünde **Değerlendirme**’yi seçin ve değerlendirmeye bir proje adı verin.
2. **Kaynak sunucu türü** bölümünde **SQL Server**’ı seçin. **Hedef sunucu türü** bölümünde **Azure Sanal Makineler'de SQL Server**’ı seçin.

    ![Kaynak seçme](./media/migrate-scenarios-assessment/dma-assessment-1.png)

    > [!NOTE]
      Şu anda DMA, SQL Yönetilen Örneği’ne geçiş için değerlendirmeyi desteklemez. Geçici bir çözüm olarak, değerlendirme için beklenen hedefimiz olarak Azure Sanal Makineler'de SQL Server’ı kullanıyoruz.

1.  **Hedef Sürümü Seçin** bölümünde, Azure’da çalıştırmak istediğiniz SQL Server’ın hedef sürümünü ve değerlendirmede neyi bulmak istediğinizi belirtin:
    - **Uyumluluk Sorunları**, size geçişi bölebilecek veya geçişten önce düşük ölçekli ayarlama gerektiren değişiklikleri bildirir. Ayrıca, şu anda kullanmakta olduğunuz, ancak kullanımdan kaldırılmış olan özellikleri de size bildirir. Sorunlar, uyumluluk düzeyine göre düzenlenir. 
    - **Yeni özellikler önerisi**, geçişten sonra veritabanınız için kullanılabilecek hedef SQL Server platformundaki yeni özellikleri size bildirir. Bunlar, Performansa, Güvenliğe ve Depolamaya göre düzenlenir. 

    ![Hedef seçin](./media/migrate-scenarios-assessment/dma-assessment-2.png)

2. **Sunucuya bağlan** bölümünde, SQL Server örneğini çalıştıran makinenin adını, kimlik doğrulama türünü ve bağlantı ayrıntılarını belirtin. Ardından **Bağlan**’a tıklayın.

    ![Hedef seçin](./media/migrate-scenarios-assessment/dma-assessment-3.png)
    
3. **Kaynak ekle** bölümünde, değerlendirmek istediğiniz veritabanını seçin ve **Ekle**’ye tıklayın.
4. Belirttiğiniz ada sahip bir değerlendirme oluşturulur.

    ![Değerlendirme oluşturma](./media/migrate-scenarios-assessment/dma-assessment-4.png)

5.  Değerlendirmeyi başlatmak için **İleri**’ye tıklayın.
6. **Sonuçları Gözden Geçir** bölümünde, etkinleştirdiğiniz değerlendirme testlerinin sonuçlarını göreceksiniz.


### <a name="analyze-the-database-assessment"></a>Veritabanı değerlendirmesini analiz etme

Sonuçlar, kullanılabilir olduğu anda Yardımcı’da görüntülenir. 

1. **Uyumluluk Sorunları** raporunda, veritabanınızın her bir uyumluluk düzeyi için sorun içerip içermediğini ve içeriyorsa nasıl düzeltileceğini denetleyin. Uyumluluk düzeyleri, SQL Server sürümleriyle aşağıdaki şekilde eşlenir:
    - 100: SQL Server 2008/Azure SQL Veritabanı
    - 110: SQL Server 2012/Azure SQL Veritabanı
    - 120: SQL Server 2014/Azure SQL Veritabanı
    - 130: SQL Server 2016/Azure SQL Veritabanı
    - 140: SQL Server 2017/Azure SQL Veritabanı

    ![Uyumluluk sorunları](./media/migrate-scenarios-assessment/dma-assessment-5.png)

2. **Özellik önerileri** raporunda, değerlendirmenin geçişten sonra yapılandırmanızı önerdiği performans, güvenlik ve depolama özelliklerini görüntüleyin. Bellek İçi OLTP ve Columnstore, Esnetme Veritabanı, Always Encrypted, Dinamik Veri Maskeleme ve Saydam Veri Şifrelemesi gibi çeşitli özellikler önerilir.

    ![Özellik önerileri](./media/migrate-scenarios-assessment/dma-assessment-6.png)

3. Herhangi bir sorunu düzeltirseniz, **Değerlendirmeyi Yeniden Başlat**’a tıklayarak yeniden çalıştırın. 
4. **Raporu dışarı aktar**’a tıklayarak değerlendirme raporunu JSON veya CSV biçiminde alın.

Büyük ölçekli bir değerlendirme çalıştırıyorsanız:

- Eşzamanlı olarak birden fazla değerlendirme çalıştırabilir ve **Tüm Değerlendirmeler** sayfasını açarak değerlendirmelerin durumunu görüntüleyebilirsiniz.
- [Değerlendirmeleri bir SQL Server veritabanında birleştirebilirsiniz](https://docs.microsoft.com/sql/dma/dma-consolidatereports?view=ssdt-18vs2017#import-assessment-results-into-a-sql-server-database).
- [Değerlendirmeleri bir PowerBI raporunda birleştirebilirsiniz](https://docs.microsoft.com/sql/dma/dma-powerbiassesreport?view=ssdt-18vs2017).


## <a name="step-4-prepare-for-vm-assessment-with-azure-migrate"></a>4. Adım: Azure Geçişi ile sanal makine değerlendirmesine hazırlanma

Azure Geçişi’nin değerlendirme için sanal makineleri otomatik olarak bulmak amacıyla kullanacağı bir VMware hesabı oluşturun, sanal makine oluşturma iznine sahip olduğunuzu doğrulayın, açık olması gereken bağlantı noktalarını not edin ve istatistik ayarlarının düzeyini ayarlayın.

### <a name="set-up-a-vmware-account"></a>VMware hesabı ayarlama

 vCenter’da salt okunur bir hesabınız olması gerekir. Yoksa, aşağıdaki özelliklere sahip bir VMware hesabı oluşturun:

- Kullanıcı türü: En azından salt okunur bir kullanıcı.
- İzinler: Veri Merkezi nesnesi –> Alt Nesneye Yay, rol=Salt okunur.
- Ayrıntılar: Veri merkezi düzeyinde atanmış ve veri merkezindeki tüm nesnelere erişimi olan kullanıcı.
- Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

### <a name="verify-permissions-to-create-a-vm"></a>Sanal makine oluşturma izinlerini doğrulama

.OVA biçiminde bir dosyayı içeri aktararak sanal makine oluşturma iznine sahip olup olmadığınızı denetleyin. [Daha fazla bilgi edinin](https://kb.vmware.com/s/article/1023189?other.KM_Utility.getArticleLanguage=1&r=2&other.KM_Utility.getArticleData=1&other.KM_Utility.getArticle=1&ui-comm-runtime-components-aura-components-siteforce-qb.Quarterback.validateRoute=1&other.KM_Utility.getGUser=1).

### <a name="verify-ports"></a>Bağlantı noktalarını doğrulama

Bu senaryoda, bağımlılık eşlemesini yapılandıracağız. Bu özellik, değerlendirdiğiniz sanal makinelere bir aracının yüklenmesini gerektirir. Bu aracının her sanal makinede 443 numaralı TCP bağlantı noktasından Azure’a bağlanabilmesi gerekir. Bağlantı gereksinimleri hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid).


### <a name="set-statistics-settings"></a>İstatistik ayarlarını yapma

Dağıtımı başlatmadan önce vCenter Server için istatistik ayarları, 3 düzeyine ayarlanmalıdır. Şunlara dikkat edin:
- Düzeyi ayarladıktan sonra, değerlendirmeyi çalıştırmadan önce en az bir gün beklemeniz gerekir. Aksi takdirde beklendiği gibi çalışmayabilir.
- Düzey, 3’ten yüksekse değerlendirme çalışır ancak:
    - Diskler ve ağ iletişimi için performans verileri toplanmaz.
    - Depolama için Azure Geçişi, Azure’da şirket içi diskle aynı boyutta standart bir disk önerir.
    - Ağ iletişimi için, her şirket içi ağ bağdaştırıcısına yönelik olarak Azure’da bir ağ bağdaştırıcısı önerilir.
    - İşlem için Azure Geçişi, sanal makine çekirdeklerine ve bellek boyutuna bakar ve aynı yapılandırmaya sahip bir Azure sanal makinesi önerir. Birden fazla uygun Azure sanal makinesi boyutu varsa, en düşük maliyetli olan önerilir.
   
    
Düzey 3 ile boyutlandırma hakkında [daha fazla bilgi edinin](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

Düzeyi aşağıdaki gibi ayarlayın:

1. vSphere Web İstemcisi’nde vCenter sunucu örneğini açın.
2. **Yönet** sekmesini seçin ve **Ayarlar** bölümünde **Genel**’e tıklayın.
3. **Düzenle**’ye tıklayın ve **İstatistikler** bölümünde, istatistik düzeyi ayarlarını **Düzey 3** olarak belirleyin.

    ![vCenter istatistikleri düzeyi](./media/migrate-scenarios-assessment/vcenter-statistics-level.png)



## <a name="step-5-discover-vms"></a>5. Adım: Sanal makineleri bulma

Bir Azure Geçişi projesi oluşturun, toplayıcı sanal makinesini indirip ayarlayın. Sonra sanal makinelerinizi bulmak için toplayıcıyı çalıştırın.

### <a name="create-a-project"></a>Proje oluşturma

1. [Azure portalında](https://portal.azure.com) oturum açın ve **Kaynak oluştur**’a tıklayın.
2. **Azure Geçişi**’ni arayın. Arama sonuçlarında **Azure Geçişi**’ni seçin ve **Oluştur**’a tıklayın.
3. Bir proje adı ve Azure aboneliğini belirtin.
4. Yeni bir kaynak grubu oluşturun.
5. Projenin konumunu belirtin ve sonra **Oluştur**’a tıklayın.

    - Bir Azure Geçişi projesini yalnızca Orta Batı ABD veya Doğu ABD bölgesinde oluşturabilirsiniz.
    - Herhangi bir hedef konumdan geçiş planlayabilirsiniz.
    - Proje konumu yalnızca şirket içi sanal makinelerden toplanan meta verileri depolamak için kullanılır.

    ![Azure Geçişi](./media/migrate-scenarios-assessment/project-1.png)


    

### <a name="download-the-collector-appliance"></a>Toplayıcı gerecini indirin

Azure Geçişi, toplayıcı gereci olarak bilinen bir şirket içi VM oluşturur. Bu VM, şirket içi VMware VM’lerini bulur ve bunlara ilişkin meta verileri Azure Geçişi hizmetine gönderir. Toplayıcı gerecini ayarlamak için, bir .OVA dosyası indirin ve VM’yi oluşturmak için dosyayı şirket içi vCenter sunucusuna aktarın.

1. Azure Geçişi projesinde **Kullanmaya Başlama** > **Bul ve Değerlendir** > **Makineleri Keşfet**’ye tıklayın.
2. **Makineleri keşfet** bölümünde .OVA dosyasını indirmek için **İndir**’e tıklayın.
3. **Proje kimlik bilgilerini kopyala** bölümünde proje kimliğini ve anahtarı kopyalayın. Toplayıcıyı yapılandırırken bu bilgilere ihtiyaç duyarsınız.

    ![.ova dosyasını indir](./media/migrate-scenarios-assessment/download-ova.png) 

### <a name="verify-the-collector-appliance"></a>Toplayıcı gereci doğrulama

Dağıtmadan önce .OVA dosyasının güvenilir olup olmadığını kontrol edin.

1. Dosyayı indirdiğiniz makinede yönetici komut penceresi açın.
2. OVA’nın karmasını oluşturmak için aşağıdaki komutu çalıştırın:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Örnek kullanım: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Oluşturulan karma bu ayarlara uygun olmalıdır (sürüm 1.0.9.7)
    
    **Algoritma** | **Karma değeri**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c
    

### <a name="create-the-collector-appliance"></a>Toplayıcı gereci oluşturma

İndirilen dosyayı vCenter Server’a aktarın.

1. vSphere Client konsolunda **Dosya** > **OVF Şablonu Dağıt**’a tıklayın.

    ![OVF dağıtma](./media/migrate-scenarios-assessment/vcenter-wizard.png) 

2. OVF Şablonu Dağıtma Sihirbazı > **Kaynak** bölümünde .OVA dosyasının konumunu belirtin ve **İleri**’ye tıklayın.
3. **OVF Şablonu Ayrıntıları** bölümünde **İleri**’ye tıklayın. **Son Kullanıcı Lisans Sözleşmesi** sayfasında **Kabul Et**’e tıklayarak sözleşmeyi kabul edin ve **İleri**’ye tıklayın.
4. **Ad ve Konum** bölümünde, toplayıcı sanal makinesi için bir kolay ad ve sanal makinenin barındırılacağı envanter konumunu belirtip **İleri**’ye tıklayın. Toplayıcı gerecinin çalıştırılacağı ana bilgisayarı veya kümeyi belirtin.
5. **Depolama** bölümünde, gereç için dosyaları depolamak istediğiniz yeri belirtin ve **İleri**’ye tıklayın.
6. **Disk Biçimi** bölümünde, depolamayı nasıl sağlamak istediğinizi belirtin.
7. **Ağ Eşleme** bölümünde toplayıcı VM’nin bağlanacağı ağı belirtin. Meta verileri Azure’a göndermek için ağ, İnternet bağlantısına sahip olmalıdır. 
8. **Tamamlamaya Hazır** bölümünde ayarları gözden geçirin, **Dağıtımdan sonra güç aç**’ı seçin ve **Son**’a tıklayın.

Gereç oluşturulduktan sonra tamamlamanın başarılı olduğunu onaylayan bir ileti görüntülenir.

### <a name="run-the-collector-to-discover-vms"></a>VM’leri bulmak için toplayıcıyı çalıştırma

Başlamadan önce, toplayıcının şu anda işletim sistemi dili ve toplayıcı arabirim dili olarak yalnızca "İngilizce (ABD)" dilini desteklediğini unutmayın.

1. vSphere Client konsolunda VM’ye sağ tıklayın ve **Konsolu Aç** seçeneğini belirleyin.
2. Gereç için dil, saat dilimi ve parola tercihlerini belirtin.
3. Masaüstünde **Toplayıcı çalıştır** kısayoluna tıklayın.

    ![Toplayıcı kısayolu](./media/migrate-scenarios-assessment/collector-shortcut.png) 
    
4. Azure Geçişi Toplayıcısı’nda **Önkoşulları ayarla** seçeneğini açın.
    - Lisans koşullarını kabul edin ve üçüncü taraf bilgilerini okuyun.
    - Toplayıcı, sanal makinenin İnternet erişimi olup olmadığını, saatin eşitlenmiş olup olmadığını ve toplayıcı hizmetinin çalışmakta (varsayılan olarak sanal makineye yüklenmiş) olup olmadığını denetler. VMWare PowerCLI’yi de yükler. 
    
    > [!NOTE]
    > Sanal makinenin, ara sunucu olmadan İnternet’e doğrudan erişiminin olduğunu varsayıyoruz.

    ![Önkoşulları doğrulama](./media/migrate-scenarios-assessment/collector-verify-prereqs.png)
    

5. **vCenter Server bilgilerini belirtin** bölümünde şunları yapın:
    - vCenter sunucusunun adını (FQDN) veya IP adresini belirtin.
    - **Kullanıcı Adı** ve **Parola** bölümünde, toplayıcının vCenter sunucusundaki sanal makineleri bulmak için kullanacağı salt okunur hesabın kimlik bilgilerini belirtin.
    - **Kapsam seçin** bölümünde, sanal makine bulma için bir kapsam seçin. Toplayıcı yalnızca belirtilen kapsam içindeki VM’leri bulabilir. Kapsam belirli bir klasör, veri merkezi veya küme olarak ayarlanabilir. Kapsam en fazla 1500 VM’yi içermelidir. 

    ![vCenter’a bağlanma](./media/migrate-scenarios-assessment/collector-connect-vcenter.png)

6. **Geçişi projesini belirtin** bölümünde portaldan kopyaladığınız Azure Geçişi proje kimliğini ve anahtarını belirtin. Bu bilgileri kopyalamadıysanız toplayıcı VM’den Azure portalını açın. Projenin **Genel Bakış** sayfasında **Makineleri Bul**’a tıklayın ve değerleri kopyalayın.  

    ![Azure'a Bağlanma](./media/migrate-scenarios-assessment/collector-connect-azure.png)

7. **Toplama durumunu görüntüle** bölümünde bulma işlemini izleyin ve VM’lerden toplanan meta verilerin kapsam içinde olup olmadığını denetleyin. Toplayıcı, yaklaşık bir bulma süresi sağlar.

    ![Koleksiyon işlemi sürüyor](./media/migrate-scenarios-assessment/collector-collection-process.png)
   


### <a name="verify-vms-in-the-portal"></a>VM’lerin portalda olup olmadığını doğrulama

Koleksiyon tamamlandıktan sonra sanal makinelerin portalda görüntülenip görüntülenmediğini denetleyin.

1. Azure Geçişi projesinde **Yönet** > **Makineler** seçeneklerine tıklayın.
2. Bulmak istediğiniz sanal makinelerin sayfada görüntülenip görüntülenmediğini denetleyin.

    ![Bulunan makineler](./media/migrate-scenarios-assessment/discovery-complete.png)

3. Makinelerde şu anda Azure Geçişi aracılarının yüklü olmadığını unutmayın. Bağımlılıkları görüntüleyebilmemiz için bunları yüklememiz gerekir.
    
    ![Bulunan makineler](./media/migrate-scenarios-assessment/machines-no-agent.png)



## <a name="step-6-prepare-for-dependency-analysis"></a>6. Adım: Bağımlılık analizine hazırlanma

Değerlendirmek istediğimiz sanal makineler arasındaki bağımlılıkları görüntülemek için WEBVM ve SQLVM adlı web uygulaması sanal makinelerinde aracıları indirip yükleriz.

### <a name="take-a-snapshot"></a>Anlık görüntü alma

Sanal makinenizi değiştirmeden önce bir kopyasını istiyorsanız, aracıları yüklemeden önce anlık görüntü alın.

![Makine anlık görüntüsü](./media/migrate-scenarios-assessment/snapshot-vm.png) 


### <a name="download-and-install-the-vm-agents"></a>Sanal makine aracılarını indirip yükleme

1.  Azure Geçişi projesinde > **Makineler** sayfasında makineyi seçin ve **Bağımlılıklar** sütununda **Yükleme gerektirir**’e tıklayın.
2.  **Makineleri Bul** sayfasında, her bir sanal makine için Microsoft Monitoring Agent (MMA) ve Bağımlılık aracısını indirip yükleyin.
3.  Çalışma alanı kimliğini ve anahtarını kopyalayın. MMA’yı yüklediğinizde bunlar gerekir.

    ![Aracıyı indirme](./media/migrate-scenarios-assessment/download-agents.png) 



#### <a name="install-the-mma"></a>MMA’yı yükleme

1. İndirilen aracıya çift tıklayın.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın. **Lisans Koşulları** sayfasında **Kabul Ediyorum**’a tıklayarak lisansı kabul edin.
3. **Hedef Klasör** bölümünde varsayılan yükleme klasörünü tutun > **İleri**’ye tıklayın. 
4. **Aracı Kurulum Seçenekleri** bölümünde **Aracıyı Azure Log Analytics’e bağla** > **İleri** seçeneklerini belirleyin. 

    ![MMA yüklemesi](./media/migrate-scenarios-assessment/mma-install.png) 
5. **Azure Log Analytics**’te, portaldan kopyaladığınız çalışma alanı kimliğini ve anahtarını yapıştırın. **İleri**’ye tıklayın.
    ![MMA yüklemesi](./media/migrate-scenarios-assessment/mma-install2.png) 

6. **Yüklemeye Hazır** bölümünde MMA’yı yükleyin.



#### <a name="install-the-dependency-agent"></a>Bağımlılık aracısını yükleme

1.  İndirilen Bağımlılık aracısına çift tıklayın.
2.  **Lisans Koşulları** sayfasında **Lisansı kabul ediyorum**’a tıklayın.
3.  **Yükleniyor** bölümünde, yüklemenin tamamlanmasını bekleyin. Ardından **İleri**'ye tıklayın.

    ![Bağımlılık aracısı](./media/migrate-scenarios-assessment/dependency-agent.png) 


       
## <a name="step-7-run-and-analyze-the-vm-assessment"></a>7. Adım: Sanal makine değerlendirmesini çalıştırma ve analiz etme

Makine bağımlılıklarını doğrulayın ve bir grup oluşturun. Daha sonra değerlendirmeyi çalıştırın.

### <a name="verify-dependencies-and-create-a-group"></a>Bağımlılıkları doğrulayın ve bir grup oluşturun.

1.  **Makineler** sayfasında, analiz etmek istediğiniz sanal makineler için **Bağımlılıkları Görüntüle**’ye tıklayın.

    ![Makine bağımlılıklarını görüntüleme](./media/migrate-scenarios-assessment/view-machine-dependencies.png) 

2. SQLVM için bağımlılık eşlemi, aşağıdaki ayrıntıları gösterir:

    - Belirtilen süre (varsayılan olarak 1 saat) boyunca SQLVM’de çalıştırılan etkin ağ bağlantılarıyla grupları/işlemleri işleme
    - Tüm bağımlı makinelere/makinelerden gelen (istemci) ve giden (sunucu) TCP bağlantıları.
    - Azure Geçişi aracılarının yüklü olduğu bağımlı makineler, ayrı kutular olarak gösterilir
    - Aracıların yüklü olmadığı makineler, bağlantı noktası ve IP adresi bilgilerini gösterir.
    
 3. Aracının yüklü olduğu makineler için (WEBVM), FQDN, işletim sistemi ve MAC adresi gibi daha fazla bilgiyi görüntülemek için makine kutusuna tıklayın. 

    ![Grup bağımlılıklarını görüntüleme](./media/migrate-scenarios-assessment/sqlvm-dependencies.png)

4. Şimdi, gruba eklemek istediğiniz sanal makineleri seçin (SQLVM ve WEBVM).  CTRL tuşunu basılı tutup tıklayarak birden fazla sanal makine seçin.
5. **Grup Oluştur**’a tıklayın ve bir ad (smarthotelapp) belirtin.

> [!NOTE]
    > Daha ayrıntılı bağımlılıkları görüntülemek için zaman aralığını genişletebilirsiniz. Belirli bir süre veya başlangıç ve bitiş tarihleri seçebilirsiniz. 


### <a name="run-an-assessment"></a>Değerlendirme çalıştırma


1. **Gruplar** sayfasında grubu (smarthotelapp) açın
2. **Değerlendirme oluştur**’a tıklayın.

    ![Değerlendirme oluşturma](./media/migrate-scenarios-assessment/run-vm-assessment.png)

3. Değerlendirme, **Yönet** > **Değerlendirmeler** sayfasında görüntülenir.


### <a name="modify-assessment-settings"></a>Değerlendirme ayarlarını değiştirme

Bu öğretici için varsayılan değerlendirme ayarlarını kullandık, ancak aşağıdaki gibi ayarları özelleştirebilirsiniz:

1. Geçiş projesinin **Değerlendirmeler** sayfasında değerlendirmeyi seçin ve **Özellikleri düzenle**’ye tıklayın.
2. Özellikleri aşağıdaki tabloya uygun şekilde değiştirin:

    **Ayar** | **Ayrıntılar** | **Varsayılan**
    --- | --- | ---
    **Hedef konum** | İçine geçişi yapmak istediğiniz Azure konumu | Varsayılan yoktur.
    **Depolama yedekliliği** | Azure sanal makinelerinin geçişten sonra kullanacağı depolama yedekliliği türü. | Varsayılan değer, [Yerel olarak yedekli depolama (LRS)](../storage/common/storage-redundancy-lrs.md) değeridir. Azure Geçişi yalnızca yönetilen diskleri temel alan değerlendirmeleri destekler ve yönetilen diskler yalnızca LRS’yi, bu nedenle de LRS seçeneğini destekler. 
    **Boyutlandırma ölçütü** | Azure için sanal makineleri doğru şekilde boyutlandırmak üzere Azure Geçişi tarafından kullanılacak ölçüt. Performans geçmişini dikkate almadan, *performans tabanlı* boyutlandırma yapabilir veya sanal makineleri *şirket içi olarak* boyutlandırabilirsiniz. | Varsayılan seçenek, performans tabanlı boyutlandırmadır.
    **Performans geçmişi** | Sanal makinelerin performansını değerlendirmek için dikkate alınacak süre. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir. | Varsayılan bir gündür.
    **Yüzdebirlik kullanımı** | Doğru boyutlandırma için dikkate alınacak performans örnek kümesinin yüzdebirlik değeri. Bu özellik yalnızca boyutlandırma ölçütü *performans tabanlı boyutlandırma* olduğunda geçerlidir.  | Varsayılan, 95. yüzdebirliktir.
    **Fiyatlandırma katmanı** | Hedef Azure sanal makineleri için [fiyatlandırma katmanını (Temel/Standart)](../virtual-machines/windows/sizes-general.md) belirtebilirsiniz. Örneğin, bir üretim ortamına geçiş yapmayı planlıyorsanız, daha düşük gecikme süresi ile sanal makineler sağlayan, ancak daha fazla maliyetli olabilecek Standart katmanını göz önünde bulundurmak istersiniz. Öte yandan, bir Geliştirme ve Test ortamınız varsa, daha yüksek gecikme süresi ve daha düşük maliyetle sanal makineler içeren Temel katmanını göz önünde bulundurmak isteyebilirsiniz. | Varsayılan olarak [Standart](../virtual-machines/windows/sizes-general.md) katmanı kullanılır.
    **Konfor katsayısı** | Azure Geçişi, değerlendirme sırasında bir tamponu (konfor katsayısı) göz önünde bulundurur. Bu tampon, VM’lerin makine kullanım verilerinin (CPU, bellek, disk ve ağ) üzerine uygulanır. Konfor katsayısı; sezona özgü kullanım, kısa performans geçmişi ve gelecek kullanımlarda oluşabilecek artışlar gibi konuları hesaba katar.<br/><br/> Örneğin, %20 kullanıma sahip 10 çekirdekli bir VM normalde 2 çekirdekli VM ile sonuçlanır. Ancak, 2.0x konfor katsayısı ile sonuç 4 çekirdekli VM olur. | Varsayılan ayar, 1.3x değeridir.
    **Teklif** | Kaydolduğunuz [Azure Teklifi](https://azure.microsoft.com/support/legal/offer-details/). | Varsayılan, [Kullandıkça öde](https://azure.microsoft.com/offers/ms-azr-0003p/)’dir.
    **Para Birimi** | Fatura para birimi. | Varsayılan, ABD Doları’dır.
    **İndirim (%)** | Azure teklifinin yanı sıra aldığınız, aboneliğe özgü indirim. | Varsayılan ayar, %0’dır.
    **Azure Hibrit Avantajı** | Yazılım güvencesine sahip olup olmadığınızı ve [Azure Hibrit Avantajı](https://azure.microsoft.com/pricing/hybrid-use-benefit/) için uygun olup olmadığınızı belirtin. Evet olarak ayarlanırsa, Windows sanal makineleri için Windows olmayan Azure fiyatları dikkate alınır. | Varsayılan değer Evet’tir.

3. Değerlendirme ayarlarını güncelleştirmek için **Kaydet**’e tıklayın.


### <a name="analyze-the-vm-assessment"></a>Sanal makine değerlendirmesini analiz etme

Azure Geçişi değerlendirmesi, şirket içi sanal makinelerin Azure için uyumlu olup olmadığı, Azure sanal makinesi için önerilen doğru boyutlandırmanın olduğu ve tahmini aylık Azure maliyetleri hakkında bilgileri içerir. 

![Değerlendirme raporu](./media/migrate-scenarios-assessment/assessment-overview.png)

#### <a name="review-confidence-rating"></a>Güvenilirlik derecelendirmesini gözden geçirme

![Değerlendirme görüntüsü](./media/migrate-scenarios-assessment/assessment-display.png)

Değerlendirmeniz, 1 yıldızdan 5 yıldıza kadar bir güvenilirlik derecelendirmesi alır (1 yıldız en düşük, 5 yıldız en yüksektir).
- Güvenilirlik derecelendirmesi, değerlendirmeyi hesaplamak için gereken veri noktalarının kullanılabilirliği temelinde bir değerlendirmeye atanır.
- Derecelendirme, Azure Geçişi tarafından sağlanan boyut önerilerinin güvenilirliğini tahmin etmenize yardımcı olur.
- Azure Geçişi, kullanım tabanlı boyutlandırma gerçekleştirmek için yeterli miktarda veri noktasına sahip olmayabileceğinden güvenilirlik derecesi, *performans tabanlı boyutlandırmada* size fayda sağlar. Azure Geçişi, VM’yi boyutlandırmak için ihtiyaç duyduğu tüm veri noktalarına sahip olduğundan, *şirket içi boyutlandırma* için güvenilirlik derecesi her zaman 5 yıldızdır.
- Kullanılabilir veri noktalarının yüzdesine bağlı olarak değerlendirme için güvenilirlik derecelendirmesi sağlanır:

   **Veri noktalarının kullanılabilirliği** | **Güvenilirlik derecelendirmesi**
   --- | ---
   %0-%20 | 1 Yıldız
   %21-%40 | 2 Yıldız
   %41-%60 | 3 Yıldız
   %61-%80 | 4 Yıldız
   %81-%100 | 5 Yıldız

#### <a name="verify-readiness"></a>Hazır olma durumu doğrulaması

![Hazır olma durumu değerlendirmesi](./media/migrate-scenarios-assessment/azure-readiness.png)  

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

![Hazır olma durumu değerlendirmesi](./media/migrate-scenarios-assessment/azure-costs.png) 

- Maliyet tahminleri, makine için boyut önerileri kullanılarak hesaplanır.
- İşlem ve depolama için tahmini aylık maliyetler gruptaki tüm VM’ler için birleştirilir. 


## <a name="conclusion"></a>Sonuç

Bu senaryoda yaptıklarımız:

> [!div class="checklist"]
> * DMA aracıyla şirket içi veritabanımızı değerlendirdik.
> * Azure Geçişi hizmetiyle bağımlılık eşlemesini kullanarak şirket içi sanal makinelerimizi değerlendirdik.
> * Şirket içi kaynaklarımızın Azure’a geçiş için hazır olduğundan emin olmak için değerlendirmeleri gözden geçirdik.

## <a name="next-steps"></a>Sonraki adımlar

Şirket içi sanal makineleri Azure’a lift-and-shift ile taşımak için sonraki senaryoyla devam edelim.



