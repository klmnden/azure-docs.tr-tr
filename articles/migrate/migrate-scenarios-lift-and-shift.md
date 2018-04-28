---
title: Şirket içi iş yüklerini Azure DMS ve Site Recovery ile azure'a geçirme | Microsoft Docs
description: Azure Azure veritabanı geçiş hizmeti, Azure Site Recovery hizmeti ile şirket içi makineler geçişi için hazırlanmasında öğrenin.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: d6fc1af3c5a587ab62b86dcd4189165d96e6e3bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="scenario-2-lift-and-shift-migration-to-azure"></a>Senaryo 2: Yükseltme ve shift geçiş Azure

Contoso şirketi Azure için geçişe. Bu sorunu anlamak denemek için değerlendirmek ve küçük şirket içi seyahat uygulama Azure'a geçirmek isterler. Bir VM ve ikinci VM üzerinde SQL Server veritabanı üzerinde çalışan bir web uygulaması ile bir iki katmanlı seyahat uygulaması değil. Uygulama, VMware içinde dağıtılır ve ortam, vCenter Server tarafından yönetilir. 

İçinde [Senaryo 1: Azure geçişi değerlendirmek](migrate-scenarios-assessment.md), veri geçiş Yardımcısı (DMA) uygulaması için SQL Server veritabanı değerlendirmek için kullandıkları. Buna ek olarak, bunlar Azure geçirmek VM'ler uygulama değerlendirmek için kullanılan hizmet. Şimdi, bu senaryoda, değerlendirme başarıyla tamamladıktan sonra Azure veritabanı geçiş hizmeti (DMS) ve şirket içi makineler Azure Site Recovery hizmetini kullanarak Azure vm'lerine kullanarak Azure SQL yönetilen bir örneğine veritabanı geçirmek isterler.

Denemek istiyorsanız [Senaryo 1](migrate-scenarios-assessment.md), ve ardından bu görsel seyahat uygulamayı kullanarak bu senaryo, buradan indirebilirsiniz [github](https://github.com/Microsoft/SmartHotel360).



**Hizmet** | **Açıklama** | **Maliyet**
--- | --- | ---
[Veritabanı yönetim hizmeti](https://docs.microsoft.com/azure/dms/dms-overview) | DMS en az kapalı kalma süresi ile Azure veri platformlar için birden fazla veritabanı kaynaktan sorunsuz geçiş sağlar. | Hizmet şu anda genel önizlemede (Nisan 2018) ve ücretsiz kullanılabilir Önizleme sırasında giderin.<br/><br/> Genel Önizleme için DMS sınırlı olduğunu unutmayın bir [bölgeler sayısı](https://docs.microsoft.com/azure/dms/dms-overview).
[Azure SQL yönetilen örneği](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Bir tam olarak yönetilen SQL Server örneği Azure bulutta temsil eden bir yönetilen veritabanı hizmeti yönetilen örneğidir. SQL Server Veritabanı Altyapısı'nın en son sürümüyle aynı kodu paylaşır ve en son özellikleri, performans iyileştirmeleri ve güvenlik yamaları vardır. | Azure SQL veritabanı yönetilen örnekleri kullanılarak azure'da çalışan kapasitesini temel ücret doğurur. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Hizmet düzenler ve geçiş ve olağanüstü durum kurtarma Azure VM'ler için yönetir ve sanal makineleri ve fiziksel sunucuları şirket içi.  | Azure'a çoğaltma sırasında Azure depolama ücretleri ücrete.  Azure VM'ler oluşturulur ve yük devretme oluştuğunda ücretler, uygulanır. [Daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/site-recovery/) ücretleri ve fiyatlandırma hakkında.

Bu senaryoda, DMS şirket içi veritabanına bağlanmak, yönetilen bir Azure SQL örneği oluşturma ve veritabanını geçirme için siteden siteye VPN yaparız. Site kurtarma için biz şirket içi VMware ortamını hazırlayın, çoğaltmayı ayarlama ve Vm'leri Azure'a geçirme.  


> [!IMPORTANT]
> SQL yönetilen örneği sınırlı genel önizlemede kayıtlı olması gerekir. Aşağıdakileri yapmak için bir Azure aboneliğine ihtiyacınız [kaydolun](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Kayıt, bu senaryoyu dağıtmaya başlamadan önce emin olun Bunu şekilde tamamlanması için birkaç gün sürebilir.

## <a name="architecture"></a>Mimari

### <a name="site-recovery"></a>Site Recovery

![Site Recovery](media/migrate-scenarios-lift-and-shift/asr-architecture.png)  

### <a name="dms"></a>DMS
![DMS](media/migrate-scenarios-lift-and-shift/dms-architecture.png)  

Bu senaryoda:

- Contoso, tipik kurumsal kuruluşu temsil eden fictious bir addır. Contoso değerlendirmek ve bunların iki katmanlı şirket içi seyahat uygulama geçirmek istiyor.
- Contoso olan bir şirket içi veri merkezi (contoso-datacenter)'ile bir şirket içi etki alanı denetleyicisi (**contosodc1 adlı**).
- İç seyahat uygulama arasında iki VM, WEBVM ve SQLVM katmanlı ve VMware ESXi ana bilgisayarda yer alan (**contosohost1.contoso.com**).
- VMware ortamı bir sanal makine üzerinde çalıştırılan vCenter Server (**vcenter.contoso.com**) tarafından yönetilir.

## <a name="prerequisites"></a>Önkoşullar

Bu senaryo çalıştırmak istiyorsanız, işte olması gerekir.

Gereksinimler | Ayrıntılar
--- | ---
**Azure aboneliği** | Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.<br/><br/> Ücretsiz bir hesap oluşturursanız, aboneliğinizin yöneticisi siz olur ve tüm eylemleri gerçekleştirebilirsiniz.<br/><br/> Var olan bir abonelik kullanın ve Yönetici değilseniz, sahibi veya katkıda bulunan izinleri atamak için yönetici ile çalışmak için gerekir.<br/><br/> Daha ayrıntılı izinler gerekiyorsa, gözden [bu makalede](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site Kurtarma (şirket içi)** | Bir şirket içi vCenter server 5.5, 6.0 veya 6.5 sürümü<br/><br/> 5.5, 6.0 veya 6.5 sürümünü çalıştıran bir ESXi ana bilgisayar<br/><br/> Bir veya daha fazla VMware ESXi ana bilgisayarında çalışan sanal makineleri.<br/><br/> Sanal makineleri karşılamalıdır [Azure gereksinimleri](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Desteklenen [ağ](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) ve [depolama](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) yapılandırma.
**DMS** | DMS için gereksinim duyduğunuz bir [içi uyumlu VPN cihazı](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Şirket içi VPN cihazı yapılandırma olması gerekir. Dışarıya dönük bir genel IPv4 adresi olmalıdır ve bir NAT cihazının arkasında adresi bulunamıyor.<br/><br/> Şirket içi SQL Server veritabanına erişiminiz olduğundan emin olun.<br/><br/> Windows Güvenlik Duvarı'nı kaynak veritabanı altyapısı erişebilir olması gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Veritabanını makinenize önünde bir güvenlik duvarı varsa, veritabanını ve dosyalarına SMB bağlantı noktası 445 üzerinden erişime izin vermek için kurallar ekleyin.<br/><br/> Yönetilen örneğini hedeflemek ve SQL Server kaynağına bağlanmak için kullanılan kimlik bilgileri sysadmin sunucu rolünün üyeleri olmalıdır.<br/><br/> Bir ağ gerekir paylaşmak şirket içi veritabanınızda DMS kaynak veritabanını yedeklemek için kullanabilirsiniz.<br/><br/> Kaynak SQL Server örneğinin çalıştığı hizmet hesabının ağ paylaşımına yazma ayrıcalıklarına sahip olduğundan emin olun.<br/><br/> Ağ paylaşımında tam denetim ayrıcalığına sahip bir Windows kullanıcısı (ve parola) not edin. Azure veritabanı geçiş hizmeti Azure storage kapsayıcısına yedekleme dosyalarını karşıya yüklemek için bu kullanıcı kimlik bilgileri temsil eder.<br/><br/> TCP/IP Protokolü SQL Server Express yükleme işlemi ayarlar **devre dışı** varsayılan olarak. Etkin olduğundan emin olun.


# <a name="scenario-overview"></a>Senaryoya genel bakış

Yapacaklarımız aşağıda verilmiştir:

> [!div class="checklist"]
> * **1. adım: bir siteden siteye VPN bağlantısını Ayarla**: DMS siteden siteye VPN üzerinden şirket içi veritabanına bağlanır. Bir sanal ağ VPN bağlantısı için ve daha sonra oluşturuyoruz, biz bu ağ için Site Recovery yeniden kullanabilirsiniz. Oluşturduğunuz ağ bir kurtarma Hizmetleri kasası oluşturma bölgede bulunması gerekir.
> * **2. adım: yönetilen bir SQL Azure örneği ayarlama**:, şirket içi SQL Server veritabanına geçirme önceden oluşturulmuş bir yönetilen örneği gerekir.
> * **3. adım: DMS SA URI'sini ayarlamak**: depolama nesneleri sınırlı izinleri verebilirsiniz paylaşılan erişim imzası (SAS) Tekdüzen Kaynak Tanımlayıcısı (URI) depolama hesabınızdaki kaynaklara atanmış erişim sağlar. DMS hizmetini SQL Server Yedekleme dosyalarını karşıya yükleme depolama hesabı kapsayıcısının erişebilmesi için bir SAS URI'sini ayarlayın.
> * **4. adım: DMS hazırlama**: veritabanı geçiş sağlayıcısını kaydetmek, örnek oluşturmak ve sonra DMS projesi oluşturun.
> * **5. adım: Azure Site Recovery için hazırlama**: Çoğaltılan verileri tutmak için bir Azure depolama hesabı oluşturun.
> * **6. adım: şirket içi VMware Site Recovery için hazırlama**: hesapları VM Keşif ve aracı yüklemesine hazırlamak ve yük devretme sonrasında Azure Vm'lerine bağlanmak hazırlayın. 
> * **7. adım: Çoğaltma sanal makineleri**: Site Recovery kaynak ve hedef ortamını yapılandırmak, bir çoğaltma ilkesini ayarlayın ve Azure depolama alanına sanal makineleri çoğaltma işlemi başlatma.
> * **8. adım: DMS veritabanıyla geçirmek**: bir veritabanı geçiş çalıştırın.
> * **9. adım: Site Recovery ile sanal makineleri geçirmek**: sanal makineleri Azure'a geçirmek için yük devretme gerçekleştirme.


## <a name="step-1-set-up-site-to-site-vpn-connection"></a>1. adım: siteden siteye VPN bağlantısını Ayarla

Siteden siteye VPN bağlantısı için hazırlamak için bir sanal ağ ve alt ağları ayarlayın, sanal ağ için bir VPN ağ geçidi oluşturmak ve bu Azure, şirket içi VPN bağlanma bilmesi için bir yerel ağ geçidi ayarlayın. Ardından, oluşturma ve siteden siteye bağlantı doğrulayın.

### <a name="set-up-a-virtual-network"></a>Bir sanal ağı ayarlama

DMS, şirket içi SQL Server'ınızı ile siteden siteye bağlantı sağlamak için Azure sanal ağı oluşturun.


1. İçinde [Azure portal](https://portal.azure.com), tıklatın **kaynak oluşturma**. Seçin **ağ** > **sanal ağ**.
2. İçinde **sanal ağ** > **dağıtım modeli seçin**seçin **Resource Manager** > **oluştur**
3. İçinde **sanal ağ oluştur** > **adı**, benzersiz ağ adı girin. Bizim senaryomuz için **ContosoVNET**.
4. İçinde **adres alanı**, IP adres aralığı Ekle. Aralığın, şirket içi adres alanlarının çakışmaması gerekir.
5. İçinde **kaynak grubu**, yeni bir grup oluşturun veya varolan bir tanesini seçin. Bu senaryoda, biz kullanmakta olduğunuz **ContosoRG**.
6. İçinde **konumu**, sanal ağın oluşturulacağı bölgeyi seçin. Kullanmakta olduğunuz **Doğu ABD 2**.
7. İçinde **alt**, ilk alt ağ adı ekleyin ve adres aralığı. Oluşturduk **uygulamaları** alt ağ.
8. Hizmet uç noktaları devre dışı bırakın.

    ![Sanal ağ oluşturma](media/migrate-scenarios-lift-and-shift/vnet-create-network.png)

9. Seçin **panoya Sabitle** kolayca ağınız gelecekte bulabilir ve ardından böylece **oluşturma**.
10. Sanal ağın oluşturulması birkaç saniye sürer. Oluşturulduktan sonra Azure portal panosunda doğrulayın.
11. Bu iletişim bağlantı noktalarını sanal ağınızda kullanılabilir olduğundan emin olun: 443, 53, 9354, 445, 12000.


### <a name="set-up-subnets"></a>Alt ağlar ayarlayın

Biz, dört alt ağlar'ı Azure ağımız olacaktır:

![Alt ağ listesi](media/migrate-scenarios-lift-and-shift/vnet-subnet-list.png)

Ağ oluştururken ilk alt ağını oluşturan (uygulamalar) ayarlayın. Şimdi, rest alt ağ oluşturun. Ağ geçidi alt ağı trafiği, şirket içi sitenizi bir VPN bağlantısı üzerinden göndermek Azure VPN ağ geçidi bağlantısı tarafından kullanılıyor.

1. Sanal ağda altında **ayarları**, tıklatın **alt** > **+ alt**.
2. Bir alt ağını oluşturan ayarlamak **Datamigration**, bu adres aralığı ve ardından **Tamam**.

    ![Veri alt ağ](media/migrate-scenarios-lift-and-shift/vnet-subnet-data.png)  


3. İçinde **+ alt**, alt ağını oluşturan ayarlamak **kimlik**, bu adres aralığı ve ardından **Tamam**.
    ![Alt ağ kimliği](media/migrate-scenarios-lift-and-shift/vnet-subnet-identity.png)

4. İçinde **alt**, tıklatın **ağ geçidi alt ağı**ve ardından **Tamam**.
    - Bu alt ağ VPN ağ geçidinizi VPN bağlantıları için kullanacağınız IP adreslerini içerir.
    - Bu Azure tanımak için bu alt ağ adını otomatik olarak ayarlanır.
    - Şirket içi alt ağıyla eşleşecek şekilde otomatik olarak doldurulur adres aralığını ayarlayın. 

    ![Ağ geçidi alt ağı](media/migrate-scenarios-lift-and-shift/vnet-subnet-gateway.png)
    
### <a name="set-up-a-virtual-network-gateway"></a>Bir sanal ağ ağ geçidi kurun

1. Portal sayfasının sol tarafına **+** simgesine tıklayın ve arama alanına ‘Sanal Ağ Geçidi’ yazın. İçinde **sonuçları**, tıklatın **sanal ağ geçidi**.
2. ‘Sanal ağ geçidi’ sayfasının alt tarafındaki **Oluştur**'a tıklayın.
3. İçinde **sanal ağ geçidi Oluştur** >  **adı**, ağ geçidi nesnesi için bir ad belirtin.
4. İçinde **ağ geçidi türü**seçin **VPN**.
5. İçinde **VPN türü**seçin **rota tabanlı**.
6. İçinde **SKU**, ağ geçidi SKU'su aşağı açılır listeden seçin. Açılır listede sıralanan SKU’lar seçtiğiniz VPN türüne bağlıdır. Etkin-etkin modu etkinleştirmeyin. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) SKU'ları hakkında.
7. İçinde **sanal ağ** > **sanal ağ seçin**, oluşturduğunuz ağ ekleyin. Bu ağ için ağ geçidi eklenir.
8. İçinde **ilk IP yapılandırması**, bir ortak IP adresi belirtin.  Bir VPN ağ geçidi, bir ortak IP adresi olmalıdır. VPN ağ geçidi oluşturulduğunda, bu adres için kaynak dinamik olarak atanır. Yalnızca dinamik adresleri şu anda desteklenir, ancak IP adresi atandıktan sonra değişmez.
    - Tıklatın **oluşturma ağ geçidi IP yapılandırması**. İçinde **genel IP adresi seçin**, tıklatın **+ Yeni Oluştur**.
    - İçinde **ortak IP adresi oluştur**, genel IP adresi (Contoso-ağ geçidi) için bir ad belirtin. SKU olarak bırakın **temel**, tıklatıp **Tamam** değişiklikleri kaydedin.
        
    ![Sanal ağ geçidi](media/migrate-scenarios-lift-and-shift/vnet-create-vpn-gateway2.png) 

9. VPN ağ geçidi oluşturmaya başlamak için **Oluştur**'a tıklayın. Ayarları doğrulanır ve "Dağıtma sanal ağ geçidi" panosunda görüntülenir.

    ![Sanal ağ geçidi doğrula](media/migrate-scenarios-lift-and-shift/vnet-vpn-gateway-validate.png)
    
Bir ağ geçidi oluşturma 45 dakika kadar sürebilir. Tamamlanma durumunu görmek için portal sayfanızı yenilemeniz gerekebilir.

### <a name="set-up-a-local-network-gateway"></a>Bir yerel ağ ağ geçidi kurun

Şirket içi sitenizi temsil etmek için bir yerel ağ geçidi, azure'da ayarlayın.

1. Portalda **+Kaynak oluştur**’a tıklayın.
2. Arama kutusuna **Yerel ağ geçidi** yazıp aramak için **Enter** tuşuna basın. Sonuçlarda tıklatın **yerel ağ geçidi** > **oluşturma**.
3. İçinde **oluşturma yerel ağ geçidi** > **adı**, Azure, yerel ağ geçidi nesnesi için kullandığı bir ad belirtin.
4. **IP adresi**, Azure bağlanacağı şirket içi VPN cihazının genel IP adresini belirtin. IP adresi NAT'ed olmamalıdır ve Azure bunu ulaşabilmesi gerekir.
5. İçinde **adres alanı**, şirket içi cihaz için VPN ağ geçidi üzerinden yönlendirilir yerel ağ adres aralığı yazın. Azure sanal ağı aralıkları ile çakışmadığından emin olun.
6. **BGP ayarlarını yapılandırmak** bu senaryo için geçerli değildir.
7. İçinde **abonelik**, abonelik doğru olduğundan emin olun.
8. İçinde **kaynak grubu**: (ContosoRG) ağ oluşturmak için kullanılan kaynak grubunu seçin.
9. İçinde **konumu**, sanal ağ oluşturulan bölgeyi seçin.

    ![Yerel ağ geçidi](media/migrate-scenarios-lift-and-shift/vnet-create-local-gateway.png)

4. Tıklatın **oluşturma** yerel ağ geçidi oluşturmak için.

### <a name="configure-your-on-premises-vpn-device"></a>Şirket içi VPN Cihazınızı yapılandırma

İşte şirket içi VPN Cihazınızı yapılandırmanız gerekir:

- Yeni oluşturduğunuz Azure sanal ağ geçidi genel IPv4 adresi.
- Önceden paylaşılan anahtarı (VPN siteden siteye bağlantı oluştururken kullanılan anahtarı).  

Şirket içi VPN Cihazınızı ayarlamanıza yardımcı olmak için:

- Sahip olduğunuz şirket içi VPN cihaza bağlı olarak bir VPN cihazı yapılandırma betiğini indir mümkün olabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-download-vpndevicescript).
- Gözden geçirme [daha yararlı kaynaklara](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VPNDevice).

### <a name="create-the-vpn-connection"></a>VPN bağlantısını oluşturma

Şimdi, sanal ağ geçidi ve şirket içi VPN cihazınız arasındaki siteden siteye VPN bağlantısı oluşturun.

1. Sanal ağ sayfasını açmak **sanal ağlar** > **genel bakış** > **bağlı cihazları**. 
2. Tıklatın **bağlantıları** > **+ Ekle**.
3. İçinde **Bağlantı Ekle** > **adı**, bağlantı için bir ad belirtin.
4. İçinde **bağlantı türü**seçin **siteden siteye (IPSec)**.
5. **Sanal ağ geçidi** değeri, bu ağ geçidi'nden bağlandığınız çünkü sabittir.
6. İçinde **yerel ağ geçidi**, oluşturduğunuz yerel ağ geçidi belirtin.
7. İçinde **paylaşılan anahtar**, yerel şirket içi VPN cihazınız için kullanmakta olduğunuz değerle anahtarını belirtin. Örnekte 'abc123' değeri kullanılmıştır, ancak siz daha karmaşık bir değer kullanabilirsiniz (ve kullanmalısınız). Önemli olan, burada belirttiğiniz değerin VPN cihazınızı yapılandırırken belirttiğiniz değerle aynı olmasıdır.
8. Değerleri **abonelik**, **kaynak grubu**, ve **konumu** düzeltilmiştir.

    ![VPN bağlantısı](media/migrate-scenarios-lift-and-shift/vpn-connection.png) 

4. Tıklatın **Tamam** bağlantı oluşturmak için. Göreceğiniz flash ekranda "bağlantısı oluşturma".
5. Bağlantı kurulduktan sonra üzerinde görüntülemek **bağlantıları** sanal ağ geçidinin sayfası. Durumu bilinmiyor gidecek > bağlanma > başarılı oldu.

### <a name="verify-the-vpn-connection"></a>VPN bağlantısını doğrulama

Azure portalında, bağlantıya giderek bir Resource Manager VPN Gateway bağlantısının durumunu görüntüleyebilirsiniz. 

1. Tıklatın **tüm kaynakları**ve sanal ağ geçidinizi gidin.
2. Sanal ağ geçidi sayfasını tıklatın **bağlantıları**. Her bağlantının durumunu görebilirsiniz.
3. Bağlantı adına tıklayın ve daha fazla bilgi görüntülemek **Essentials**. Başarılı bir bağlantı yaptıktan sonra durumu 'Başarılı' ve 'Bağlı' olur.

Şimdi SQL Server VM VPN üzerinden bağlanabildiğini doğrulayın. Uzak Masaüstü bağlantısı kullanın ve VM IP adresine bağlanın.



## <a name="step-2-prepare-an-azure-sql-managed-instance"></a>2. adım: Azure SQL yönetilen örneği hazırlama

### <a name="set-up-a-virtual-network"></a>Bir sanal ağı ayarlama

Bu senaryo için Azure SQL örneğine yönetilen ayrılmış başka bir sanal ağ oluşturmanız gerekir.  Aşağıdaki gereksinimleri dikkate alın:

- Yönetilen örneğini oluşturmak için ayrılmış bir alt ağ gerekir.
- Alt ağı boş olamaz ve herhangi bir bulut hizmeti içermiyor ve bir ağ geçidi alt ağı olamaz. Yönetilen örneği oluşturulduktan sonra alt ağına kaynakları ekleyin gerekir.
- Alt ağ ile ilişkilendirilmiş bir NSG olması gerekir.
- Alt ağ, kendisine atanmış yalnızca rota olarak bir kullanıcı rota tablosu (UDR) 0.0.0.0/0 sonraki atlama Internet ile olması gerekir. 
- İsteğe bağlı özel DNS: özel DNS VNet üzerinde belirtilirse, Azure'nın özyinelemeli çözümleyiciler IP adresi (örneğin, 168.63.129.16) listesine eklenmesi gerekir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Alt ağ ilişkili hizmet uç noktası (depolama veya SQL) olması gerekir. Hizmet uç noktaları sanal ağda devre dışı bırakılması gerekir.
- Alt ağ 16 IP adresini en az olmalıdır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) yönetilen örneği alt boyutlandırma hakkında.

Sanal ağı aşağıdaki gibi ayarlayın:

1.  Azure portalında **Kaynak oluştur**’a tıklayın. 
2. Seçin **ağ** > **sanal ağ**.
3. İçinde **sanal ağ** > **dağıtım modeli seçin**seçin **Resource Manager** > **oluşturma**.
4. İçinde **sanal ağ oluştur** > **adı**, benzersiz ağ adı girin. Bizim senaryomuz için **ContosoVNETSQLMI**.
5. İçinde **adres alanı**, aşağıda gösterilen IP adres aralığı Ekle. Aralığın, şirket içi ve ContosoVNET adres alanı ile çakışmaması gerekir.
6. İçinde **kaynak grubu**, yeni bir grup oluşturun veya varolan bir tanesini seçin. Bu senaryoda, biz kullanmakta olduğunuz **ContosoRG**.
7. İçinde **konumu**, sanal ağın oluşturulacağı bölgeyi seçin. Kullanmakta olduğunuz **Doğu ABD 2**.
8. İçinde **alt**, ilk alt ağ adı ekleyin ve adres aralığı. Oluşturduğumuz **ManagedInstances** alt ağ.
9. Hizmet uç noktaları devre dışı bırakın.

    ![Yönetilen örnek ağ](media/migrate-scenarios-lift-and-shift/network-mi.png)

10. Ağ dağıtıldıktan sonra DNS ayarlarını değiştirmeniz gerekir. Bu senaryoda, DNS ilk özel statik IP adresi (172.16.3.4) ve Azure DNS Çözümleyicisi 168.63.129.16 kullanarak kimlik alt ağındaki bir etki alanı denetleyicisi işaret eder.

    ![Yönetilen örnek ağ](media/migrate-scenarios-lift-and-shift/network-mi2.png)


### <a name="set-up-routing"></a>Yönlendirmeyi kurmak

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesine tıklayın. Tıklatın **yol tablosu** > **yol tablosu oluşturma** sayfası.
2. İçinde **oluşturma yol tablosu** > **adı**, tablo için bir ad belirtin.
3. Abonelik, kaynak grubunu ve konumu seçin. BGP devre dışı bırakın ve tıklatın **oluşturma**.
    ![Yol tablosu](media/migrate-scenarios-lift-and-shift/route-table.png)

4. Yol tablosu oluşturulduktan sonra açmak ve tıklatın **yollar** > **+ Ekle**.
5. İçinde **Add yolun** > **adı**, bir rota adı belirtin.
6. İçinde **adres ön eki**, 0.0.0.0/0 belirtin.
7. İçinde **sonraki atlama türü**seçin **Internet**. Daha sonra, **Tamam**'a tıklayın.

     ![Rota tablosu](media/migrate-scenarios-lift-and-shift/route-table2.png)


### <a name="apply-the-route-to-the-managed-instance-subnet"></a>Yol yönetilen örneği alt ağına Uygula

1. Oluşturduğunuz sanal ağ açın. Tıklatın **alt ağlar** > Bilgisayarınızı-alt-adı.
2. Tıklatın **yol tablosu** > Bilgisayarınızı-tablo-adı. Daha sonra **Kaydet**'e tıklayın.

     ![Rota tablosu](media/migrate-scenarios-lift-and-shift/route-table3.png)

### <a name="create-a-managed-instance"></a>Yönetilen bir örneği oluşturma

1. Tıklatın **kaynak oluşturma**, bulun **yönetilen örneği**seçip **Azure SQL veritabanı örneği (Önizleme) yönetilen**.
2. **Oluştur**’a tıklayın.
3. İçinde **SQL yönetilen örneği**, aboneliğinizi seçin ve doğrulayın **Önizleme koşulları** Göster **kabul edilen**.
4. İçinde **yönetilen örnek adı**, bir ad belirtin. 
5. Örneği için yönetici olarak bir kullanıcı adı ve parola belirtin.
6. Kaynak grubu, konum ve örnek için sanal ağ seçin.
7. Tıklatın **fiyatlandırma katmanı** boyutu işlem ve depolama için. Varsayılan olarak, 32 GB depolama alanı (Nisan 2018) ücretsiz örneğini alır.
8. Depolama belirtin ve sanal çekirdek sayısı.
9. Tıklatın **Uygula** ayarlarını kaydetmek için ve **oluşturma** yönetilen örneği dağıtmak için. Dağıtım durumunu görüntülemek için **bildirimleri**, ve **dağıtımı devam ediyor**.

    ![Yönetilen örnek](media/migrate-scenarios-lift-and-shift/mi-1.png)

Sonra yönetilen örnek dağıtım, iki yeni kaynaklar ContosoRG kaynak grubunda görünür tamamlandıktan: Sanal küme yönetilen örnekleri ve yönetilen SQL örneği için. Her ikisi de Doğu ABD 2 konumunda var.

![Yönetilen örnek](media/migrate-scenarios-lift-and-shift/managed-instance2.png)


## <a name="step-3-get-a-sas-uri-for-dms"></a>3. adım: DMS için bir SAS URI'sini Al

DMS veritabanları Azure SQL veritabanı yönetilen örneğine geçirmek için kullanılan yedekleme dosyaları karşıya yüklemek için depolama hesabı kapsayıcısının erişebilmesi için bir SAS URI'sini Al. 

1. Depolama Gezgini’ni (Önizleme) açın.
2. Sol bölmede, SAS almak istediğiniz blob kapsayıcısı içeren depolama hesabı'nı genişletin. Depolama hesabının genişletin **Blob kapsayıcıları**.
3. Kapsayıcıya sağ tıklayın ve seçin **paylaşılan erişim imzası Al**.

     ![SAS](media/migrate-scenarios-lift-and-shift/sas-1.png)

4. İçinde **paylaşılan erişim imzası**, ilke, başlangıç ve sona erme tarihleri, saat dilimini belirtin ve erişim düzeyleri kaynak için istediğiniz. Sonra **Oluştur**’a tıklayın.

    ![SAS](media/migrate-scenarios-lift-and-shift/sas-2.png)

5. İkinci bir iletişim kutusu URL ve depolama kaynağa erişmek için kullanabileceğiniz QueryStrings yanı sıra blob kapsayıcısında görüntüler. Seçin **kopyalama** değerleri kopyalamak için. Bunları güvenli bir yerde tutun. DMS ayarladığınızda, onları gerekir.

      ![SAS](media/migrate-scenarios-lift-and-shift/sas-3.png)  

## <a name="step-4-prepare-dms"></a>4. adım: DMS hazırlama

Veritabanı geçiş sağlayıcıyı kaydettirin ve bir örneği oluşturun.

### <a name="register-the-microsoftdatamigration-resource-provider"></a>Microsoft.DataMigration kaynak sağlayıcısını Kaydet

1. Aboneliğinizde seçin **kaynak sağlayıcıları**. 
2. "Geçiş" için arama yapın ve ardından Microsoft.DataMigration sağında **kaydetmek**. 


### <a name="create-an-instance"></a>Bir örneği oluşturma

1. Seçin **+ kaynak oluşturma**'Veritabanı geçiş hizmeti için Azure' araması yapın ve ardından **Azure veritabanı geçiş hizmeti** aşağı açılan listeden.
2. Azure veritabanı geçiş hizmeti (Önizleme) ekranında seçin **oluşturma**.
3. Hizmet, abonelik, kaynak grubu, sanal ağ ve fiyatlandırma katmanı için bir ad belirtin.
4. Seçin **oluşturma** hizmeti oluşturmak için.

    ![DMS](media/migrate-scenarios-lift-and-shift/dms-instance.png)  




## <a name="step-5-prepare-azure-for-the-site-recovery-service"></a>5. adım: Azure Site Recovery hizmeti için hazırlama

### <a name="set-up-a-virtual-network"></a>Bir sanal ağı ayarlama

Yük devretme sonrasında oluşturulduğunda, Azure sanal makinelerini yer alacağı bir Azure ağı ve çoğaltılan verilerin depolandığı bir Azure depolama hesabı gerekir.

- Bu senaryo amaçları doğrultusunda, DMS için daha önce oluşturduğumuz Azure ağ kullanacağız.
- Kullandığınız ağ Site Recovery kasasıyla aynı bölgede olması gerektiğini unutmayın.


### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

Site kurtarma VM verileri Azure depolama alanına çoğaltır. Şirket içinden Azure’a yük devretme gerçekleştirdiğinizde depolama alanından Azure sanal makineleri oluşturulur.

1. [Azure portal](https://portal.azure.com)’da, **Yeni** > **Depolama** > **Depolama hesabı**’nı seçin.
2. **Depolama hesabı oluştur** bölümüne hesap için bir ad girin. Bu öğreticiler için **contosovmsacct1910171607** adını kullanın. Ad, Azure içinde benzersiz olmalı, 3 ila 24 karakter uzunluğunda olmalı ve yalnızca rakamlar ve küçük harfler içermelidir.
3. **Dağıtım modeli** bölümünde **Kaynak Yöneticisi**’ni seçin.
4. **Hesap türü** bölümünde **Genel amaçlı**’yı seçin. **Performans** bölümünde **Standart**’ı seçin. Blob depolamayı seçmeyin.
5. **Çoğaltma** bölümünde depolama yedeklemesi için **Okuma Erişimli Coğrafi Olarak Yedekli depolama**’yı seçin.
6. **Abonelik** bölümünde yeni depolama hesabını oluşturmak istediğiniz aboneliği seçin.
7. **Kaynak grubu** bölümünde yeni bir kaynak grubu adı girin. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Bu öğreticiler için **ContosoRG** adını kullanın.
8. **Konum** bölümünde depolama hesabınız için coğrafi konumu seçin. Depolama hesabının, Kurtarma Hizmetleri kasasıyla aynı bölgede olması gerekir. Bu senaryoda, biz kullanmakta olduğunuz **Doğu ABD 2** bölge.

   ![Depolama hesabı oluşturma](media/migrate-scenarios-lift-and-shift/create-storageacct.png)

9. Depolama hesabını oluşturmak için **Oluştur**’u seçin.

### <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. Azure portalında **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup and Site Recovery** seçeneklerini belirleyin.
2. **Ad** alanına kasayı tanımlamak için kolay bir ad girin. Bu öğretici için **ContosoVMVault** adını kullanın.
3. **Kaynak grubu** bölümünde **contosoRG** adlı mevcut kaynak grubunu seçin.
4. İçinde **konumu**, Azure bölgesi girin **Doğu ABD 2**.
5. Panodan kasaya hızlı şekilde erişmek için **Panoya sabitle** > **Oluştur**’u seçin.
Yeni kasa, **Pano** > **Tüm kaynaklar** bölümünde ve ana **Kurtarma Hizmetleri kasaları** sayfasında görüntülenir.

## <a name="step-6-prepare-on-premises-vmware-for-site-recovery"></a>6. adım: şirket içi VMware Site Recovery için hazırlama

Site Recovery'nin için burada VMware hazırlamak için yapmanız gerekenler:

- VM bulmayı otomatikleştirmek için vCenter sunucusunda veya vSphere ESXi ana bilgisayarında bir hesap hazırlama
- VMware VM’lerinde Mobility hizmetini otomatik olarak yüklemek için bir hesap hazırlama
- Yük devretme sonrasında Azure Vm'lerine bağlanmak isterseniz şirket içi sanal makineleri hazırlayın.


### <a name="prepare-an-account-for-automatic-discovery"></a>Otomatik bulma için bir hesap hazırlama

Site Recovery aşağıdakiler için VMware sunucularına erişmesi gerekir:

- Sanal makineleri otomatik olarak bulur. En az bir salt okunur hesap gereklidir.
- Çoğaltma, yük devretme ve yeniden çalıştırmayı yönetme. Oluşturma ve diskleri kaldırma ve sanal makine kapatma gibi işlemleri çalıştırılan bir hesap gerekir.

Hesabı aşağıdaki gibi oluşturun:

1. Ayrılmış bir hesap kullanmak için, rolü vCenter düzeyinde oluşturun. Role **Azure_Site_Recovery** gibi bir ad verin.
2. Role aşağıdaki tabloda özetlenen izinleri atayın.
3. vCenter sunucusu veya vSphere ana bilgisayarında bir kullanıcı oluşturun. Rolü kullanıcıya atayın.

**Görev** | **Rol/İzinler** | **Ayrıntılar**
--- | --- | ---
**VM bulma** | En az bir salt okunur kullanıcı<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Read-only | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.
**Tam çoğaltma, yük devretme, yeniden çalışma** |  Gerekli izinlere sahip bir rol (Azure_Site_Recovery) oluşturup rolü VMware kullanıcısı veya grubuna atayın<br/><br/> Veri Merkezi nesnesi –> Alt Nesneye Yay, role=Azure_Site_Recovery<br/><br/> Veri deposu -> Alan ayırma, veri deposuna göz atma, düşük düzeyli dosya işlemleri, dosyayı kaldırma, sanal makine dosyalarını güncelleştirme<br/><br/> Ağ -> Ağ ataması<br/><br/> Kaynak -> VM’yi kaynak havuzuna atama, kapalı VM’yi geçirme, açık VM’yi geçirme<br/><br/> Görevler -> Görev oluşturma, görevi güncelleştirme<br/><br/> Sanal makine -> Yapılandırma<br/><br/> Sanal makine -> Etkileşim -> soruyu yanıtlama, cihaz bağlantısı, CD ortamını yapılandırma, disket ortamını yapılandırma, kapatma, açma, VMware araçlarını yükleme<br/><br/> Sanal makine -> Envanter -> Oluşturma, kaydetme, kaydı kaldırma<br/><br/> Sanal makine -> Sağlama -> Sanal makine indirmeye izin verme, Sanal makine dosyalarını karşıya yüklemeye izin verme<br/><br/> Sanal makine -> Anlık görüntüler -> Anlık görüntüleri kaldırma | Kullanıcı veri merkezi düzeyinde atandı ve bu veri merkezindeki tüm nesnelere erişimi var.<br/><br/> Erişimi kısıtlamak için, **Alt öğeye yay** nesnesi ile **Erişim yok** rolünü alt nesnelere (vSphere ana bilgisayarları, veri depoları, VM’ler ve ağlar) atayın.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Bir hesabı Mobility hizmeti yüklemesi için hazırlama

Çoğaltmak istediğiniz sanal makinede Mobility hizmeti yüklü olmalıdır.

- Sanal makine için çoğaltmayı etkinleştirdiğinizde, site kurtarma Bu bileşen otomatik itme yüklemesi yapabilirsiniz.
- Otomatik gönderme yüklemesi için Site Recovery VM erişmek için kullanacağı bir hesap hazırlamanız gerekir.
- Azure konsolunda çoğaltma ayarladığınızda bu hesabı belirtin.
- Bir etki alanı veya yerel hesap VM'de yüklemek için gerekli izinlere sahip gerekir.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Yük devretmeden sonra Azure VM'lerine bağlanmak için hazırlık yapma

Azure'a yük devretme sonrasında çoğaltılmış VM'ler için Azure'da şirket içi ağınızdan bağlanmak isteyebilirsiniz.

Yük devretmeden sonra RDP kullanarak Windows VM’lerine bağlanmak için aşağıdakileri yapın:

1. İnternet üzerinden erişmek için, yük devretmeden önce şirket içi VM’de RDP’yi etkinleştirin. İçin TCP ve UDP kurallarının eklendiğinden emin olun **ortak** profili ve RDP izin verildiğinden emin **Windows Güvenlik Duvarı** > **izin verilen uygulamaları** tüm profiller için.
2. Siteden siteye VPN üzerinden erişmek için, şirket içi makinede RDP’yi etkinleştirin. **Etki Alanı ve Özel** ağlar için **Windows Güvenlik Duvarı** -> **İzin verilen uygulama ve özellikler içinde** RDP’ye izin verilmelidir.
3. İşletim sisteminin SAN ilkesinin **OnlineAll** olarak ayarlandığından emin olun. [Daha fazla bilgi edinin](https://support.microsoft.com/kb/3031135).
4. Bir yük devretme tetiklediğinizde VM’de bekleyen Windows güncelleştirmelerinin olmaması gerekir. Varsa, güncelleştirme tamamlanana kadar sanal makinede oturum açmanız mümkün olmayacaktır.
5. Yük devretmeden sonra Windows Azure VM’sinde, VM’nin bir ekran görüntüsünü görmek için **Önyükleme tanılaması**’nı kontrol edin. Bağlanamıyorsanız, VM’nin çalıştığından emin olun ve şu [sorun giderme ipuçlarını](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx) gözden geçirin.



## <a name="step-7-replicate-vms-with-site-recovery"></a>7. adım: Site Recovery ile Vm'lerini çoğaltma

### <a name="select-a-replication-goal"></a>Çoğaltma hedefi seçme

1. **Kurtarma Hizmetleri kasalarında** **ContosoVMVault** kasa adını seçin.
2. **Başlarken** bölümünde Site Recovery’yi seçin. Daha sonra **Altyapıyı Hazırlama**’yı seçin.
3. **Koruma hedefi** > **Makineleriniz nerede** bölümünde **Şirket içi** seçeneğini belirleyin.
4. **Makinelerinizi nereye çoğaltmak istiyorsunuz** bölümünde **Azure’a** seçeneğini belirleyin.
5. **Makineleriniz sanallaştırıldı mı** bölümünde **Evet, VMware vSphere Hypervisor ile** seçeneğini belirleyin. Sonra **Tamam**’ı seçin.

    ![OVF şablonu](./media/migrate-scenarios-lift-and-shift/replication-goal.png)


### <a name="confirm-deployment-planning"></a>Dağıtım Planlama onaylayın

İçinde **dağıtımı planlama**, tıklatın **Evet, yaptım**, aşağı açılan listesinde.

### <a name="set-up-the-source-environment"></a>Kaynak ortamı ayarlama


Kaynak ortamı ayarlamak için, şirket içi Site Recovery bileşenlerini barındıracak, yüksek kullanılabilirliğe sahip tek bir şirket içi makineye ihtiyacınız vardır. Yapılandırma sunucusu ve işlem sunucusu bileşenleri içerir.

- Yapılandırma sunucusu, şirket içi bileşenler ile Azure arasındaki iletişimleri koordine eder ve veri çoğaltma işlemini yönetir.
- İşlem sunucusu, çoğaltma ağ geçidi olarak davranır. Çoğaltma verilerini alıp bunları önbelleğe alma, sıkıştırma ve şifreleme ile iyileştirir ve Azure depolamaya gönderir.
- İşlem sunucusu aynı zamanda çoğaltmak istediğiniz VM’lere Mobility Hizmetini yükler ve şirket içi VMware VM’lerinin otomatik olarak bulunmasını sağlar.


Yapılandırma sunucusunu ayarlamayı yüksek oranda kullanılabilir bir VMware VM olarak ayarlamak için:

- Hazırlanan bir açık sanallaştırma biçimi (OVF) şablonu indirin.
- Şablon oluşturmak için VMware alın.
- Yapılandırma sunucusunu ayarlamayı ve kasaya kaydedin. 

Kayıt işleminden sonra Site Recovery, şirket içi VMware VM’lerini bulur.



#### <a name="download-the-vm-template"></a>VM şablonunu indirme

1. Kasada **Altyapıyı Hazırlama** > **Kaynak** seçeneğine gidin.
2. **Kaynağı hazırla** bölümünde **+Yapılandırma sunucusu**’nu seçin.

    ![Şablon indirme](./media/migrate-scenarios-lift-and-shift/new-cs.png)

4. **Sunucu Ekle** bölümünde **Sunucu türü**’nde **VMware için yapılandırma sunucusu**’nun görüntülenip görüntülenmediğini kontrol edin.
2. Yapılandırma sunucusu için OVF şablonunu indirin.

    ![OVF indirin](./media/migrate-scenarios-lift-and-shift/add-cs.png)

  > [!TIP]
  Yapılandırma sunucusu şablonunun en son sürümünü doğrudan [Microsoft Yükleme Merkezi](https://aka.ms/asrconfigurationserver)’nden indirebilirsiniz.

#### <a name="import-the-template-in-vmware"></a>VMware’de şablonu içeri aktarma

1. VMWare vSphere istemci kullanarak VMware vCenter sunucusuna oturum açın.
2. **Dosya** menüsünde **OVF Şablonunu Dağıt** seçeneğini belirleyerek OVF Şablonu Dağıtma sihirbazını başlatın. 

     ![OVF şablonu](./media/migrate-scenarios-lift-and-shift/vcenter-wizard.png)

3. **Kaynak seçin** bölümünde, indirilen OVF’nin konumunu girin.
4. **Değerlendirme ayrıntıları** bölümünde **İleri** seçeneğini belirleyin.
5. **Ad ve klasör seçin** ve **Yapılandırma seçin** bölümünde varsayılan ayarları kabul edin.
6. **Select storage** bölümünde, en iyi performans için **Select virtual disk format** bölümünden **Thick Provision Eager Zeroed** seçeneğini belirleyin.
7. Sihirbaz sayfalarının geri kalan kısmında varsayılan ayarları kabul edin.
8. **Tamamlanmaya hazır** olduğunda:

    * VM’yi varsayılan ayarlarla kurmak için **Dağıtımdan sonra aç** > **Son** seçeneğini belirleyin.

    * Bir ağ arabirimi daha eklemek istiyorsanız **Dağıtımdan sonra aç** seçeneğinin işaretini kaldırın. Ardından **Son**’u seçin. Varsayılan olarak, yapılandırma sunucusu şablonu tek bir NIC ile dağıtılır. Dağıtımdan sonra daha fazla NIC ekleyebilirsiniz.



#### <a name="register-the-configuration-server"></a>Yapılandırma sunucusunu kaydetme 

1. VMWare vSphere Client konsolundan VM’yi açın.
2. VM’de Windows Server 2016 yükleme deneyimi önyüklemesi yapılır. Lisans sözleşmesini kabul edin ve bir yönetici parolası girin.
3. Yükleme tamamlandıktan sonra VM’de yönetici olarak oturum açın.
4. İlk oturum açma işleminizde Azure Site Recovery Yapılandırma Aracı başlar.
5. Yapılandırma sunucusunu Site Recovery’ye kaydetmek için kullanılacak bir ad girin. Sonra **İleri**’yi seçin.

    ![OVF şablonu](./media/migrate-scenarios-lift-and-shift/config-server-register1.png)

6. Araç, VM’nin Azure bağlanıp bağlanamadığını denetler. Bağlantı kurulduktan sonra Azure aboneliğinizde oturum açmak için **Oturum aç** seçeneğini belirleyin. Kimlik bilgilerinin, yapılandırma sunucusunu kaydetmek istediğiniz kasaya erişim izni olmalıdır.

    ![OVF şablonu](./media/migrate-scenarios-lift-and-shift/config-server-register2.png)

7. Araç bazı yapılandırma görevleri gerçekleştir ve sonra yeniden başlatılır.
8. Makinede tekrar oturum açın. Yapılandırma sunucusu yönetim sihirbazı otomatik olarak başlar.

#### <a name="configure-settings-and-add-the-vmware-server"></a>Ayarları yapılandırma ve VMware sunucusunu ekleme

1. Yapılandırma sunucusu yönetim sihirbazında **Bağlantı kurma** seçeneğini belirleyip, çoğaltma trafiğini almak için NIC’yi seçin. Daha sonra **Kaydet**’e tıklayın. Bu ayar yapılandırıldıktan sonra değiştirilemez.
2. **Recovery Services kasasını seçin** bölümünde Azure aboneliğinizi, ilgili kaynak grubunu ve kasayı seçin.

    ![Kasa](./media/migrate-scenarios-lift-and-shift/cswiz1.png) 

3. **Üçüncü taraf yazılımı yükleyin** bölümünde lisans sözleşmesini kabul edin. MySQL Server’ı yüklemek için **İndir ve Yükle** seçeneğini belirleyin.
4. **VMware PowerCLI’yi Yükle** seçeneğini belirleyin. Bunu yapmadan önce tüm tarayıcı pencerelerinin kapalı olduğundan emin olun. Daha sonra **Devam** seçeneğini belirleyin.
5. **Gereç yapılandırmasını doğrulama** bölümünde ön koşullar, siz devam etmeden önce doğrulanır.
6. **vCenter Server/vSphere ESXi sunucusu yapılandırma** bölümünde, çoğaltmak istediğiniz VM’lerin bulunduğu vCenter sunucusunun veya vSphere konağının FQDN’sini ya da IP adresini girin. Sunucunun dinleme gerçekleştirdiği bağlantı noktasını girin. Kasadaki VMware sunucusu için kullanılacak bir kolay ad girin.
7. VMware sunucusu ile bağlantı için yapılandırma sunucusu tarafından kullanılacak kimlik bilgilerini girin. Site Recovery, bu kimlik bilgilerini çoğaltma için kullanılabilen VMware VM’lerini otomatik olarak bulmak üzere kullanır. **Ekle** seçeneğini ve sonra **Devam** seçeneğini belirleyin.

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

8. **Sanal makine kimlik bilgilerini yapılandırma** bölümünde, çoğaltma etkinleştirildiğinde makinelere Mobility Hizmetinin otomatik olarak yüklenmesi için kullanılacak kullanıcı adını ve parolayı girin. Windows makinelerinde hesap için, çoğaltmak istediğiniz makinelerde yerel yönetici ayrıcalıkları gerekir. 

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

7. Kaydı tamamlamak için **Yapılandırmayı son haline getir** seçeneğini belirleyin. 
8. Kayıt bittikten sonra Azure portalında, yapılandırma sunucusunun ve VMware sunucusunun kasadaki **Kaynak** sayfasında listelendiğinden emin olun. Daha sonra hedef ayarlarını yapılandırmak için **Tamam**’ı seçin.
9. Site Recovery, belirtilen ayarları kullanarak VMware sunucularına bağlanır ve VM’leri bulur.

> [!NOTE]
> Hesap adının portalda görünmesi 15 dakika veya daha fazla sürebilir. Hemen güncelleştirme yapmak için **Yapılandırma Sunucuları** > ***sunucu adı*** > **Sunucuyu Yenile** seçeneğini belirleyin.

### <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. İçinde **altyapıyı hazırlama** > **hedef**, kullanmak istediğiniz Azure aboneliğini seçin.
2. Hedef dağıtım modelini seçin **Azure Resource Manager**.

3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


### <a name="create-a-replication-policy"></a>Çoğaltma ilkesi oluşturma

1. İçinde **altyapıyı hazırlama** > **çoğaltma ayarları** > **Çoğaltma İlkesi**, tıklatın **oluşturun ve ilişkilendirin** .
1. **Çoğaltma ilkesi oluştur** seçeneğinde, **VMwareRepPolicy** ilke adını girin.
2. **RPO eşiği** bölümünde varsayılan 60 dakika seçeneğini kullanın. Bu değer kurtarma noktalarının hangi sıklıkta oluşturulacağını tanımlar. Devamlı çoğaltma bu sınırı aşarsa bir uyarı oluşturulur.
3. **Kurtarma noktası bekletme** bölümünde her bir kurtarma noktasının bekletme aralığı için varsayılan 24 saat seçeneğini kullanın. Bu öğretici için 72 saat seçeneğini kullanın. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir.
4. **Uygulamayla tutarlı anlık görüntü sıklığı** bölümünde uygulamayla tutarlı anlık görüntülerin oluşturulma sıklığı için varsayılan 60 dakika seçeneğini kullanın. İlkeyi oluşturmak için **Tamam**’ı seçin.

    ![Çoğaltma ilkesi oluşturma](./media/migrate-scenarios-lift-and-shift/replication-policy.png)

5. İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir. 

    ![Çoğaltma İlkesi ilişkilendirme](./media/migrate-scenarios-lift-and-shift/replication-policy2.png)



### <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Şimdi sanal makineleri çoğaltma işlemi başlatma. Site Recovery Mobility hizmeti her VM yükler çoğaltmayı etkinleştirebilirsiniz. 


1. **Uygulamayı çoğalt** > **Kaynak** seçeneğini belirleyin.
2. **Kaynak** bölümünde yapılandırma sunucusunu seçin.
3. **Makine türü** bölümünde **Sanal Makineler**’i seçin.
4. **vCenter/vSphere Hypervisor** bölümünde vSphere konağını yöneten vCenter sunucusunu veya konağı seçin.
5. İşlem sunucusunu (yapılandırma sunucusu) seçin. Sonra **Tamam**’ı seçin.

    ![Çoğaltmayı etkinleştirme](./media/migrate-scenarios-lift-and-shift/enable-replication1.png)

1. **Hedef** bölümünde, yükü devredilen VM’leri oluşturmak istediğiniz aboneliği ve kaynak grubunu seçin. Yükü devredilen VM’ler için Azure (klasik veya Resource Manager) üzerinde kullanmak istediğiniz dağıtım modelini seçin.
2. Verileri çoğaltmak için kullanmak istediğiniz Azure depolama hesabını seçin.
3. Yük devretme işleminden sonra oluşturulan Azure VM’lerin bağlandığı Azure ağını ve alt ağını seçin.
4. Koruma için seçtiğiniz tüm makinelere ağ ayarını uygulamak için **Seçili makineler için şimdi yapılandır**’ı seçin. Makineler için Azure ağını ayrı ayrı seçmek için **Daha sonra yapılandır**'ı seçin.
5. Sanal ağ alt ağı seçin. Sonra **Tamam**’ı seçin.

    ![Çoğaltmayı etkinleştirme](./media/migrate-scenarios-lift-and-shift/enable-replication2.png)

6. İçinde **sanal makineleri** > **sanal makine Seç**, VM (istediğiniz WEBVM) seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. 

    ![Çoğaltmayı etkinleştirme](./media/migrate-scenarios-lift-and-shift/enable-replication3.png)

7. Eklediğiniz VM’leri izlemek için **Configuration Servers** > **Last Contact At** bölümünde VM’lerin son bulunma zamanını kontrol edin. Zamanlanan bulma işlemini beklemeden VM’leri eklemek için yapılandırma sunucusunu vurgulayın (seçmeyin) ve **Yenile**’yi seçin. Değişikliklerin geçerli olması ve portalda görüntülenmesi 15 dakika veya daha uzun sürebilir.
8. **Özellikler** > **Özellikleri yapılandırma** bölümünde, Mobility Hizmeti’nin makineye otomatik olarak yüklenmesi için işlem sunucusu tarafından kullanılacak hesabı seçin. 

    ![Çoğaltmayı etkinleştirme](./media/migrate-scenarios-lift-and-shift/enable-replication4.png)

9. **Çoğaltma ayarları** > **Çoğaltma ayarlarını yapılandırma** bölümünde doğru çoğaltma ilkesinin seçilip seçilmediğini doğrulayın.
10. **Çoğaltmayı Etkinleştir** seçeneğini belirleyin.

    ![Çoğaltmayı etkinleştirme](./media/migrate-scenarios-lift-and-shift/enable-replication5.png)

11. İlerleme durumunu izlemek **korumayı etkinleştir** iş **ayarları** > **işleri** > **Site Recovery işleri**. 
12. Sonra **korumayı Sonlandır** iş çalıştırmaları, makinenin yük devretme için hazır ve görünür **genel bakış** > **Essentials**.

    ![Temel Bileşenler](./media/migrate-scenarios-lift-and-shift/essentials.png)


## <a name="step-8-migrate-the-database-with-dms"></a>8. adım: DMS veritabanıyla geçirme

DMS projesi oluşturun ve geçiş çalıştırın.


### <a name="create-a-database-migration-project"></a>Bir veritabanı geçiş projesi oluşturma

1. Azure portalında DMS kaynak bulun ve açın.
2. Seçin **+ yeni geçiş proje**.
3. İçinde **yeni geçiş proje**, proje için bir ad belirtin.
4. **Kaynak sunucu türü** bölümünde **SQL Server**’ı seçin. İçinde **hedef sunucu türü**seçin **yönetilen Azure SQL veritabanı örneği**.
5. Tıklatın **oluşturma** projesi oluşturmak için.
6. İçinde **kaynak Ayrıntılar**, şirket içi SQL Server kaynak bağlantı ayrıntılarını belirtin. **Kaydet**’e tıklayın.
7. İçinde **hedef ayrıntıları**, önceden sağlanan Azure SQL veritabanı yönetilen için şirket içi veritabanının taşınır örnek hedef bağlantı ayrıntılarını belirtin. Daha sonra **Kaydet**'e tıklayın.
8. İçinde **Proje Özeti**, gözden geçirin ve geçiş projeyle ilişkili ayrıntılarını doğrulayın.

    ![DMS proje](./media/migrate-scenarios-lift-and-shift/dms-project.png)

### <a name="migrate-the-database"></a>Veritabanını geçirme

1. Projesi oluşturduktan sonra Geçiş Sihirbazı görünür.
2. Kaynak ve hedef sunucular için kimlik bilgilerini belirtin ve tıklatın **kaydetmek**. Sihirbazı, şirket içi SQL Server'ınızı oturum açmayı dener.

    ![DMS Sihirbazı](./media/migrate-scenarios-lift-and-shift/dms-wiz1.png)

3. İçinde **hedef veritabanlarına harita**, geçiş yapmak istediğiniz kaynak veritabanını seçin. Daha sonra **Kaydet**'e tıklayın.

    ![DMS Sihirbazı](./media/migrate-scenarios-lift-and-shift/dms-wiz2.png)

4. İçinde **hedef ayrıntıları**seçin **my hedef ayrıntıları bilmek**. SQL yönetilen örneklerinizi, kimlik bilgilerinin yanı sıra DNS adını belirtin. Daha sonra **Kaydet**'e tıklayın.

    ![DMS Sihirbazı](./media/migrate-scenarios-lift-and-shift/dms-wiz3.png)

5. Kaydedilen projedeki seçin **+ yeni etkinlik**. Ardından **çalıştırmak geçiş**.
6. İstendiğinde, kaynak ve hedef sunucuları için kimlik bilgilerini girin. Daha sonra **Kaydet**'e tıklayın.
7. İçinde **hedef veritabanlarına harita**, geçirmek istediğiniz kaynak veritabanını seçin. Ardından **Kaydet**
8. İçinde **geçiş ayarları yapılandırmak** > **sunucusu yedekleme konumu**, şirket içi makinede oluşturduğunuz ağ paylaşımını belirtin. DMS bu paylaşıma kaynak yedeklemeler alır.  Kaynak SQL Server örneğinin çalıştığı hizmet hesabının bu paylaşımı üzerinde yazma ayrıcalıklarına olması gerektiğini unutmayın.
9. Kullanıcı adı ve DMS geri yüklemek için Azure depolama kapsayıcısı yedekleme dosyalarını yüklemek için bürünebilir parola belirtin.
10. DMS hizmetini yükler yedekleme dosyaları, depolama hesabı kapsayıcısı için erişimi sağlayan SAS URI'sini belirtin ve yönetilen Azure SQL veritabanı örneğine geçişi için kullanılır.  Daha sonra **Kaydet**'e tıklayın.
11. İçinde **geçiş Özet**, geçiş etkinliği için bir ad belirtin > **geçişi çalıştırma**.
12. Proje > **genel bakış**, geçiş durumunu izleyin. Geçiş tamamlandıktan sonra hedef veritabanlarına yönetilen örneğinde var olduğunu doğrulayın.


## <a name="step-9-migrate-vms-with-site-recovery"></a>9. adım: Site Recovery ile Vm'leri geçirme

Her şeyin tam geçiş çalıştırmadan önce beklendiği gibi çalıştığından emin olmak için yük devretme testi çalıştırmak öneririz. Yük devretme testi çalıştırdığınızda şunlar olur:

1. Tüm geçiş için gereken koşulların karşılandığından emin olmak için çalışır bir ön koşullarını denetleyin.
2. Yük devretme, bir Azure sanal makinesi oluşturulabilmesi için verileri işler. En son kurtarma noktasını seçerseniz verilerden bir kurtarma noktası oluşturulur.
3. Önceki adımda işlenen veriler kullanılarak bir Azure sanal makinesi oluşturulur.

### <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma

1. **Korunan Öğeler**’de **Çoğaltılan Öğeler** > VM seçeneğine tıklayın.
2. VM Özellikleri'nde tıklatın **+ yük devretme testi**.

    ![Yük devretme sınaması](./media/migrate-scenarios-lift-and-shift/test-failover.png)

3. Seçin **son işlenen**, zaman içinde kullanılabilir son noktanın VM'ye devretmek için. Zaman damgası gösterilir. Bu seçenekte, verileri işlemek için zaman harcanmadığından düşük bir RTO (kurtarma süresi hedefi) sağlanır.
2. **Yük Devretme Testi** bölümünde, yük devretme gerçekleştikten sonra Azure VM’lerinin bağlanacağı hedef Azure ağını seçin.
3. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. İlerleme depo kasa izleyebilirsiniz > **ayarları** > **işleri** > **yük devretme testi**.
4. Yük devretme bittikten sonra, çoğaltma Azure VM, Azure portalı > **Sanal Makineler** bölümünde görünür. Sanal makinenin uygun boyutta olduğundan, doğru ağa bağlandığından ve çalıştığından emin olun. Şimdi Azure’da çoğaltılan sanal makineye bağlanabiliyor olmanız gerekir.
5. Yük devretme testi sırasında oluşturulan Azure sanal makinelerini silmek için, VM’de **Yük devretme testini temizle**’ye tıklayın. Yük devretme testiyle ilişkili gözlemlerinizi **Notlar**’da kaydedin veya saklayın.

### <a name="migrate-the-machines"></a>Makineleri geçirin

Yük devretme sınaması beklenen şekilde çalıştığını doğruladıktan sonra Azure'a geçirmek için bir kurtarma planı oluşturun.

### <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma

1. Kasada seçin **kurtarma planları (Site Recovery)** > **+ kurtarma planı**.
2. İçinde **oluşturma kurtarma planı**, plan için bir ad belirtin.
3. Kaynak yapılandırma sunucusu ve Azure hedefi olarak seçin. Seçin **Resource Manager** dağıtım modeli için. Kaynak konumu, etkinleştirilen makineleri yük devretme ve kurtarma olması gerekir. 
4. İçinde **seçin öğeleri**, plana (WEBVM) makineleri ekleyin. Daha sonra, **Tamam**'a tıklayın.
5. Tıklatın **Tamam** planı oluşturmak için.

    ![Kurtarma planı](./media/migrate-scenarios-lift-and-shift/recovery-plan1.png)

### <a name="run-a-failover"></a>Yük devretme çalıştırma

1. İçinde **kurtarma planları**, plana tıklayın > **yük devretme**.
2. İçinde **yük devretme** > **kurtarma noktası**, en son kurtarma noktası seçin.
3. Şifreleme anahtarı ayarı, bu senaryo için geçerli değildir.
4. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery yük devretmeyi tetiklemeden önce kaynak sanal makineleri kapatmayı dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.

    ![Yük devretme](./media/migrate-scenarios-lift-and-shift/failover1.png)

5. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.

    ![Yük devretme](./media/migrate-scenarios-lift-and-shift/failover2.png)

6. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Böylece geçiş işlemi tamamlanır, VM için çoğaltma durdurulur ve sanal makine için Site Recovery faturalaması durdurulur.

    ![Yük devretme](./media/migrate-scenarios-lift-and-shift/failover3.png)

### <a name="update-the-connection-string"></a>Bağlantı dizesi güncelleştir

Geçiş işleminin son adımı, SQL mı üzerinde çalışan geçirilmiş veritabanına işaret etmek için uygulamanın bağlantı dizesini güncellemeniz olmaktır.

1. Tıklayarak Azure portalında yeni bağlantı dizesini bulun **ayarları** > **bağlantı dizeleri**.

    ![Yük devretme](./media/migrate-scenarios-lift-and-shift/failover4.png)  

2. Bu dize, SQL mı için kullanıcı adı ve parola ile güncelleştirmeniz gerekir.
3. Dize yapılandırıldıktan sonra geçerli bağlantı dizesi, uygulamanızın web.config dosyasında değiştirmeniz gerekiyor.
4. Dosyayı güncelleştirme ve kaydettikten sonra IIS WEBVM üzerinde yeniden başlatın. Bu yapılabilir bir komut isteminden /RESTART IISRESET kullanma.
5. IIS yeniden başlatıldıktan sonra uygulamanız şimdi, SQL mı üzerinde çalışan veritabanı kullanır.
6. Bu noktada SQLVM makine şirket içi kapatılıp ve geçiş tamamlanır.



## <a name="conclusion"></a>Sonuç

Bu senaryoda yaptıklarımız:

> [!div class="checklist"]
> * Bir Azure SQL örneğine yönetilen DMS ile şirket içi Veritabanımıza geçirildi.
> * Bizim şirket içi sanal makineleri Azure VM'ler için Azure Site Recovery hizmetiyle geçirildi.
