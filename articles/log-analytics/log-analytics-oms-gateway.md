---
title: OMS ağ geçidi kullanarak bilgisayarlara bağlanma | Microsoft Docs
description: Operations Manager izlenen bilgisayarlar ve cihazlar internet erişimi olmadığında Azure Automation ve günlük analizi hizmeti veri göndermek için OMS geçidiyle bağlayın.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2018
ms.author: magoedte
ms.openlocfilehash: 66e5444f5346a44cfc8a43cf2b43dbaeacffedc9
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="connect-computers-without-internet-access-by-using-the-oms-gateway"></a>İnternet erişimi olmayan bilgisayarları OMS ağ geçidi kullanarak bağlanma
Bu belge, Azure Automation ile iletişimi yapılandırılacağını açıklar ve olduğunda, doğrudan OMS ağ geçidi kullanarak günlük analizi bağlı bilgisayarlar veya Operations Manager izlenen bilgisayarların internet erişimi yok. HTTP BAĞLAMAK komutunu kullanarak tünel HTTP destekleyen bir HTTP iletme proxy OMS geçididir. Bu verileri toplamak ve internet erişimi olmayan bilgisayarları adına Azure Automation ve günlük analizi gönderin.  

OMS ağ geçidi destekler:

* Azure Otomasyon karma Runbook çalışanları  
* Windows bilgisayarları Microsoft Monitoring Agent ile doğrudan günlük analizi çalışma alanına bağlı
* Linux için OMS Aracısı ile Linux bilgisayarlar doğrudan bir günlük analizi çalışma alanına bağlı  
* System Center Operations Manager 2012 SP1 UR7 ile Operations Manager 2012 R2 UR3, Operations Manager 2016 ve Operations Manager sürüm 1801 yönetim grubu ile günlük analizi ile tümleşik  

BT güvenlik ilkeleri (örneğin, satış noktası cihazları veya BT Hizmetleri destekleyen sunucular) Internet'e bağlanmak için ağınızdaki belirli bilgisayarlara izin verme ancak onları, izlemek ve yönetmek için Azure Otomasyonu veya günlük analizi bağlanmak gerekiyorsa, doğrudan OMS ağ geçidi ile iletişim kurmak için yapılandırılabilir.

 Bu bilgisayarlar doğrudan bir günlük analizi çalışma alanına bağlanmak için OMS Aracısı ile yapılandırılmışsa, tüm bilgisayarlar yerine OMS ağ geçidi ile iletişim kurar. OMS ağ geçidi verileri aracılardan hizmetine doğrudan aktarır. Yoldaki olsa verilerin hiçbirini analiz etmez.

Bir Operations Manager yönetim grubu günlük analizi ile tümleştirildiğinde, yönetim sunucuları OMS yapılandırma bilgilerini almak ve toplanan verileri göndermek için ağ geçidine bağlanmak için yapılandırılabilir. Operations Manager aracıları Operations Manager uyarıları, yapılandırma değerlendirmesi, örnek alanı ve kapasite verileri gibi bazı veriler yönetim sunucusuna gönderir. IIS günlükleri, performans bilgileri ve güvenlik olayları gibi diğer yüksek hacimli verileri doğrudan OMS ağ geçidi için gönderilir.  

Güvenilmeyen sistemlerini izlemek üzere bir DMZ veya diğer yalıtılmış ağ dağıtılmış bir veya daha fazla Operations Manager Ağ Geçidi sunucuları varsa, bunlar bir OMS ağ geçidi ile iletişim kuramıyor. Operations Manager Ağ Geçidi sunucuları, yalnızca bir yönetim sunucusuna rapor edebilir. 

Bir Operations Manager yönetim grubu OMS ağ geçidi ile iletişim kurmak için yapılandırıldığında proxy yapılandırma bilgilerini günlük analizi için veri toplamak üzere yapılandırılmış her aracı yönetilen bir bilgisayar için otomatik olarak dağıtılır olsa bile boş bir ayardır.    

İçin yüksek kullanılabilirlik sağlamak için doğrudan bağlı veya günlük analizi OMS ağ geçidi üzerinden iletişim Operations Yönetim grupları, Ağ Yükü Dengeleme yönlendirebilir ve trafik birden fazla OMS ağ geçidi sunucusu arasında dağıtmak için kullanabilirsiniz. Bir ağ geçidi sunucusu kullanılamaz hale gelirse trafiği kullanılabilir başka bir düğüme yönlendirilir.  

OMS Aracısı izlemek ve performans veya olay verileri çözümlemek için OMS ağ geçidi yazılımını çalıştıran bilgisayara yüklemeniz önerilir. Ayrıca, aracı ile iletişim kurması için gereken hizmet uç noktaları tanımlamak OMS Gateway yardımcı olur.

Veri ağ geçidi gelen ve giden otomatik olarak aktarmasına olanak veren tüm aracıların kendi ağ geçidi ağ bağlantısı olması gerekir. Bir etki alanı denetleyicisinde bir ağ geçidi yüklemenizi öneririz yok.

Aşağıdaki diyagramda doğrudan aracılardan Azure Automation ve ağ geçidi sunucusu kullanarak günlük analizi veri akışı gösterilmektedir. Aracı proxy yapılandırmasını OMS ağ geçidi hizmet ile iletişim kurmak için kullandığı aynı bağlantı noktasını kullanmanız gerekir.  

![Hizmetleri diyagramı ile doğrudan aracı iletişimi](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Aşağıdaki diyagramda, günlük analizi için Operations Manager yönetim grubundan veri akışı gösterilmektedir.   

![Günlük analizi diyagramı ile Operations Manager iletişim](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Önkoşullar

OMS ağ geçidini çalıştırmak için bir bilgisayar atadığınızda, aşağıdaki bileşenleri sahip olduğundan emin olun:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET framework 4.5
* En az bir 4 çekirdekli işlemci ve 8 GB bellek 

### <a name="language-availability"></a>Dil kullanılabilirliği

OMS ağ geçidi aşağıdaki dillerde kullanılabilir:

- Çince (Basitleştirilmiş)
- Çince (Geleneksel)
- Çekçe
- Hollanda dili
- Türkçe
- Fransızca
- Almanca
- Macarca
- İtalyanca
- Japonca
- Kore dili
- Lehçe
- Portekizce (Brezilya)
- Portekizce (Portekiz)
- Rusça
- İspanyolca (uluslararası)

### <a name="supported-encryption-protocols"></a>Desteklenen şifreleme protokolleri
OMS ağ geçidi yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2 destekler.  Güvenli Yuva Katmanı (SSL) desteklemez.

### <a name="supported-number-of-agent-connections"></a>Desteklenen Aracısı bağlantısı sayısı
Aşağıdaki tabloda desteklenen bir ağ geçidi sunucusu ile iletişim kuran aracıları sayısıdır vurgular. Bu destek 6 saniyede yaklaşık 200 KB veri karşıya aracısında temel alır. Test aracısı başına veri hacmi hakkında 2.7 günde GB'dir.

|Ağ geçidi |Desteklenen aracıları yaklaşık sayısı|  
|--------|----------------------------------|  
|-CPU: Intel XEON CPU E5-2660 v3 2.6 GHz 2 Çekirdek @<br> -Bellek: 4 GB<br> -Ağ bant genişliği: 1 GB/sn| 600|  
|-CPU: Intel XEON CPU E5-2660 v3 2.6 GHz 4 çekirdek @<br> -Bellek: 8 GB<br> -Ağ bant genişliği: 1 GB/sn| 1000|  

## <a name="download-the-oms-gateway"></a>OMS ağ geçidini indirin

OMS ağ geçidi Kurulum dosyasının en son sürümünü almak için iki yolu vardır.

1. İndirin [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443).

2. Azure portalından indirin. Azure portalında oturum açtıktan sonra aşağıdaki adımları uygulayın:  

   1. Hizmetlerin listesini bulun ve seçin **günlük analizi**.  
   2. Bir çalışma alanı seçin.
   3. Çalışma alanı dikey penceresinde altında **genel**seçin **Hızlı Başlangıç**.
   4. Altında **çalışma alanına bağlanmak için veri kaynağı seçin**seçin **bilgisayarlar**.
   5. İçinde **doğrudan Aracısı** paneli, select **OMS ağ geçidi indirme**.
   
    ![OMS ağ geçidini indirin](./media/log-analytics-oms-gateway/download-gateway.png)

-- OR-- 

   1. Çalışma alanı dikey penceresinde altında **ayarları**seçin **Gelişmiş ayarları**.
   2. Seçin **bağlı kaynakları** > **Windows sunucuları**. Ardından **OMS ağ geçidi indirme**.

## <a name="install-the-oms-gateway"></a>OMS ağ geçidini yükleyin

Bir ağ geçidi yüklemek için aşağıdaki adımları uygulayın. Önceki sürümünü yüklediyseniz, adıysa *günlük analizi iletici*, bu sürüme yükseltilir.  

1. Hedef klasör seçin **OMS Gateway.msi**.
2. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.

 ![Ağ geçidi Kurulum Sihirbazı](./media/log-analytics-oms-gateway/gateway-wizard01.png)
  
3. Üzerinde **Lisans Sözleşmesi'ni** sayfasında, **lisans sözleşmesinin koşullarını kabul ediyorum**. Sonra **İleri**’yi seçin.

4. Üzerinde **bağlantı noktası ve proxy adresi** sayfa:

   a. Ağ geçidi için kullanılan TCP bağlantı noktası numarasını yazın. Kurulum, Windows Güvenlik Duvarı'nda bu bağlantı noktası numarasına sahip bir gelen kuralı yapılandırır. Varsayılan değer 8080'dir. Geçerli bağlantı noktası numarası 1 ile 65535 arasındadır. Giriş bu aralığa uymazsa bir hata iletisi görüntülenir.

   b. İsteğe bağlı olarak, ağ geçidinin yüklü olduğu sunucunun bir proxy üzerinden iletişim kurması gerekirse, ağ geçidi bağlanması için gereken proxy yazın. Bir örnek olabilir `http://myorgname.corp.contoso.com:80`aşağıdaki ekran görüntüsünde gösterildiği gibi. Bu alanı boş bırakırsanız, doğrudan internet'e bağlanmak ağ geçidi çalışır.  Proxy sunucusu kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola girin.
   
    ![Ağ geçidi Sihirbazı proxy yapılandırması](./media/log-analytics-oms-gateway/gateway-wizard02.png)

4. **İleri**’yi seçin.

5. Ürününe sahip değilseniz, Microsoft Update etkinleştirilmiş, Microsoft Update sayfası görünür ve etkinleştirmek seçebilirsiniz. Bir seçim yapın ve ardından **sonraki**. Aksi halde, sonraki adıma devam edin.

6. Üzerinde **hedef klasörü** sayfasında, ya da bırakın varsayılan klasör olarak **C:\Program Files\OMS ağ geçidi** veya OMS ağ geçidi yüklemek istediğiniz konumu girin. Sonra **İleri**’yi seçin.

7. Üzerinde **yüklenmeye hazır** sayfasında, **yükleme**. Kullanıcı hesabı denetimi yüklemeye izin istemeden görünebilir. Bir istek için izin almak, seçin **Evet**.

8. Kurulum bittikten sonra seçin **son**. Services.msc ek bileşenini açarak hizmetinin çalıştığından emin doğrulayabilirsiniz **OMS ağ geçidi** Hizmetler listesinde görüntülenir ve durumu **çalıştıran**.

    ![Hizmetleri – OMS ağ geçidi](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Ağ Yükü Dengeleme yapılandırma 
Ağ Yükü Dengeleme (NLB) kullanarak yüksek kullanılabilirlik için ağ geçidini yapılandırabilirsiniz. Microsoft Ağ Yükü Dengelemesi veya donanım tabanlı yük dengeleyici kullanın.  Yük Dengeleyici OMS Aracısı istenen bağlantılarından veya Operations Manager yönetim sunucuları arasında düğümlerinden yönlendirerek trafiği yönetir. Bir ağ geçidi sunucusu kullanılamaz hale gelirse trafiği diğer düğümlere yönlendirilir.

Tasarım ve bir Windows Server 2016 Ağ Yükü Dengeleme kümesi dağıtma hakkında bilgi edinmek için [Ağ Yükü Dengeleme](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Aşağıdaki adımlar, Microsoft Ağ Yükü Dengeleme kümesini yapılandırın açıklar.  

1.  NLB kümesinin bir üyesi olan Windows server için bir Yönetici hesabınızla oturum açın.  

2.  Sunucu Yöneticisi'nde Ağ Yükü Dengeleme Yöneticisi'ni açın. Seçin **Araçları**ve ardından **Ağ Yükü Dengeleme Yöneticisi**.

3. Microsoft izleme aracısının yüklü bir OMS Ağ Geçidi sunucusuna bağlanmak için kümenin IP adresine sağ tıklayın ve ardından **kümeye konak Ekle**.

 ![Ağ Yükü Dengeleme Yöneticisi--kümeye konak Ekle](./media/log-analytics-oms-gateway/nlb02.png)

4. Bağlanmak istediğiniz Ağ Geçidi sunucusunun IP adresini girin.

    ![Ağ Yükü Dengeleme Yöneticisi--kümeye konak Ekle: bağlanma](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-the-oms-agent-and-the-operations-manager-management-group"></a>OMS Aracısı'nı ve Operations Manager yönetim grubu yapılandırma
Aşağıdaki bölümlerde bu makalede Azure Otomasyonu veya günlük analizi ile iletişim kurmak için OMS ağ geçidi ile doğrudan bağlı OMS Aracısı, bir Operations Manager yönetim grubu ya da Azure Otomasyon karma Runbook çalışanlarını yapılandırma adımları içerir.  

Gereksinimler ve doğrudan günlük Analizi'ne bağlamak Windows bilgisayarlarda OMS Aracısı'nı yüklemek için adımları anlamak için bkz: [günlük analizi ile ortamınızdaki bilgisayarlardan verileri toplama](log-analytics-concept-hybrid.md#prerequisites).

 Linux bilgisayarlar için bkz: [günlük analizi bağlanmak Linux bilgisayarlara](log-analytics-quick-collect-linux-computer.md). Otomasyon karma Runbook çalışanı ilgili daha fazla bilgi için bkz: [dağıtmak karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-the-standalone-oms-agent"></a>Tek başına OMS Aracısı'nı yapılandırma
Bir aracı (Bu durumda ağ geçididir) proxy sunucusu kullanacak şekilde yapılandırma hakkında daha fazla bilgi için bkz: [günlük analizi ile ortamınızdaki bilgisayarlardan verileri toplama](log-analytics-concept-hybrid.md#prerequisites). Ağ Yük Dengeleyici arkasında birden çok ağ geçidi sunucusu dağıttıysanız, OMS Aracısı proxy yapılandırmasını NLB sanal IP adresi şöyledir:

![Microsoft Monitoring Agent özellikleri – proxy ayarları](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager-all-agents-use-the-same-proxy-server"></a>Operations Manager yapılandırın: tüm aracıları aynı proxy sunucuyu kullan
Operations Manager ağ geçidi sunucusu eklemek için yapılandırın.  Ayarı boş olsa bile, Operations Manager proxy yapılandırması Operations Manager için rapor veren tüm aracıların otomatik olarak uygulanır.  

Operations Manager desteklemek için OMS ağ geçidini kullanmak için aşağıdaki bileşenler yerinde olması gerekir:

* Microsoft Monitoring Agent (aracı sürümü **8.0.10900.0** veya sonrası) Ağ Geçidi sunucusunda yüklü ve istediğiniz iletişim kurmak günlük analizi çalışma alanları için yapılandırılmış.

* İnternet bağlantısı veya mu bir proxy sunucusu bağlantısı olan bir ağ geçidi.

> [!NOTE]
> Ağ geçidi için bir değer belirtmezseniz, boş değerler için tüm aracılar atılır.
> 

Operations Manager yönetim grubunuzu günlük analizi çalışma alanıyla kaydetme ilk kez kullanıyorsanız Operations konsolunda yönetim grubu için proxy yapılandırması belirtme seçeneği kullanılamaz. 

Yönetim grubu bu seçeneği kullanılabilir olmadan önce Hizmeti'ne başarıyla kayıtlı olması gerekir. Üzerinde işlemler konsolu ve tüm yönetim sunucuları yönetim grubunda çalıştırdığınız aynı sistemde Netsh kullanarak sistem proxy yapılandırmasını güncelleştirin.

1. Yükseltilmiş bir komut istemi açın.

    a. Git **Başlat**. Yazın **cmd**.
    
    b. Sağ **komut istemi**. Ardından **yönetici olarak çalıştır**.
    
2. Aşağıdaki komutu girin ve ardından **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Günlük analizi ile tümleştirmesini tamamladıktan sonra çalıştırarak değişiklik kaldırabilirsiniz `netsh winhttp reset proxy`. Ardından **proxy sunucusunu yapılandır** OMS ağ geçidi sunucusu belirtmek için işletim konsolunda seçeneği. 

1. Operations Manager konsolunu açın. Altında **Operations Management Suite**seçin **bağlantı**. Ardından **Proxy sunucusunu yapılandırma**.

    ![Operations Manager--Proxy sunucusunu yapılandırın](./media/log-analytics-oms-gateway/scom01.png)

2. Seçin **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak**. OMS Ağ Geçidi sunucusunun IP adresini veya NLB sanal IP adresini yazın. İle başlatıldığından emin olmak `http://` öneki.

    ![Operations Manager--proxy sunucusu adresi](./media/log-analytics-oms-gateway/scom02.png)

3. **Son**’u seçin. Operations Manager yönetim grubunuzu şimdi günlük analizi hizmeti için ağ geçidi sunucusu üzerinden iletişim kuracak şekilde yapılandırıldı.

### <a name="configure-operations-manager-specific-agents-use-proxy-server"></a>Operations Manager yapılandırın: belirli aracıları proxy sunucusu kullan
Büyük veya karmaşık ortamları için OMS ağ geçidi sunucusu kullanmak için belirli sunucuları (veya gruplar) yalnızca isteyebilirsiniz. Doğrudan yönetim grubu için genel değere göre bu değerin üzerine için bu sunucular için Operations Manager Aracısı güncelleştirilemiyor. Bunun yerine bu değerleri göndermek için kullanılan kuralı geçersiz kıl.  

> [!NOTE] 
> Aynı yapılandırma tekniği ortamınızda birden çok OMS ağ geçidi sunucusu kullanımına izin vermek için kullanılabilir. Örneğin, bir bölge başına temelinde belirtilmesi için belirli OMS Ağ Geçidi sunucuları gerektirebilir.
>  

1. Operations Manager konsolunu açın ve ardından **yazma** çalışma.

2. Yazma çalışma alanında seçin **kuralları**. Ardından **kapsam** Operations Manager araç çubuğunda. Bu düğme kullanılamıyorsa, seçili bir nesne olan, bir klasör değil olduğundan emin olmak için kontrol **izleme** bölmesi. **Kapsam Yönetim Paketi nesneleri** iletişim kutusunda ortak hedeflenen sınıfları, grupları veya nesneleri listesini görüntüler. 

3. içinde **Ara** alanında, yazın **sistem durumu hizmeti**. Ardından listeden seçin. **Tamam**’ı seçin.  

4. Arama kuralı için **Danışmanı Proxy ayarı kural**. İşlemler konsolu araç seçin **geçersiz kılmaları**. Ardından **Rule\For sınıfın belirli bir nesnesi geçersiz kılma: sistem durumu hizmeti**. 

5. Ardından, belirli bir nesneyi listeden seçin. 

6. (İsteğe bağlı) Bu geçersiz kılma uygulamak istediğiniz sunucuları sistem durumu hizmeti nesnesinin içeren özel bir grup oluşturun. Daha sonra bu gruba geçersiz kılma işleminin ardından uygulayabilirsiniz.

7. İçinde **geçersiz kılma özellikleri** iletişim kutusu, bir onay işareti tıklatıp **geçersiz kılma** yanındaki sütuna **WebProxyAddress** parametresi.  İçinde **geçersiz kılma değeri** alanına, OMS Ağ Geçidi sunucusunun URL'sini ile başlayan sağlama `http://` öneki.  

    >[!NOTE]
    > Kural etkinleştirmeniz gerekmez. Bunu zaten otomatik olarak Microsoft System Center Advisor Güvenli başvuru geçersiz kılma yönetim paketinde yer alan hedefleyen Microsoft System Center Advisor Monitoring Server grubu geçersiz kılma ile yönetilir.
    >   

8. Bir yönetim paketinden seçin **hedef Yönetim Paketi seçin** listesi veya seçerek yeni bir korumasız Yönetim Paketi oluşturun **yeni**. 

9. Değişikliklerinizi tamamladığınızda seçin **Tamam**. 

### <a name="configure-the-oms-gateway-for-automation-hybrid-runbook-workers"></a>Otomasyon karma Runbook çalışanları için OMS ağ geçidini yapılandırma
Ortamınızda Otomasyon karma Runbook çalışanları varsa, aşağıdaki adımlar desteklemek için OMS ağ geçidini yapılandırmak için el ile geçici geçici çözümler sağlar.

Aşağıdaki adımlarda Otomasyon hesabının bulunduğu Azure bölgesi bilmeniz gerekir. Konumu bulmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure Otomasyon hizmetine seçin.
3. Uygun Azure Otomasyon hesabı seçin.
4. Altında **konumu**, hesap bölge görüntüleyin.

    ![Azure portal--Otomasyon hesabı konumu](./media/log-analytics-oms-gateway/location.png)  

Her konum URL'sini belirlemek için aşağıdaki tabloları kullanın:

**İş çalışma zamanı veri hizmeti URL'leri**

| **Konum** | **URL** |
| --- | --- |
| Orta Kuzey ABD |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net |
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net |
| Japonya |jpe-jobruntimedata-prod-su1.azure-automation.net |
| Avustralya |ase-jobruntimedata-prod-su1.azure-automation.net |

**Aracı hizmeti URL'leri**

| **Konum** | **URL** |
| --- | --- |
| Orta Kuzey ABD |ncus-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-agentservice-prod-1.azure-automation.net |
| Orta Kanada |cc-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-agentservice-prod-1.azure-automation.net |
| Orta Hindistan |cid-agentservice-prod-1.azure-automation.net |
| Japonya |jpe-agentservice-prod-1.azure-automation.net |
| Avustralya |ase-agentservice-prod-1.azure-automation.net |

Bilgisayarınızı güncelleştirme yönetimi çözümü kullanarak düzeltme eki uygulama için bir karma Runbook çalışanı olarak otomatik olarak kayıtlı değilse, aşağıdaki adımları izleyin:

1. İş çalışma zamanı veri hizmeti URL'leri OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, aşağıdaki komutu yazın: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

2. OMS ağ geçidi hizmeti aşağıdaki PowerShell cmdlet'ini kullanarak yeniden başlatın: `Restart-Service OMSGatewayService`

Yerleşik, karma Runbook çalışanı kayıt cmdlet'ini kullanarak Azure Otomasyonu bilgisayarınıza izlerseniz adımları:

1. Aracı hizmeti kayıt URL'si OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, aşağıdaki komutu yazın: `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`

2. İş çalışma zamanı veri hizmeti URL'leri OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, aşağıdaki komutu yazın: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`

3. OMS ağ geçidi hizmeti yeniden başlatın:  `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Yararlı PowerShell cmdlet'leri
Cmdlet'leri OMS ağ geçidi yapılandırma ayarlarını güncelleştirmek için gerekli görevleri tamamlamanıza yardımcı olabilir. Kullanmadan önce aşağıdaki adımlar emin olun:

1. OMS ağ geçidi (MSI) yükleyin.
2. Bir PowerShell konsol penceresi açın.
3. Aşağıdaki komutu yazarak modülünü içeri aktarın: `Import-Module OMSGateway`.
4. Önceki adımda herhangi bir hata oluştu, modülü başarıyla içeri aktarıldı ve cmdlet'leri kullanılabilir. `Get-Module OMSGateway` yazın.
5. Cmdlet'lerini kullanarak değişiklikleri yaptıktan sonra OMS ağ geçidi hizmeti yeniden emin olun.

3. adımında bir hata alırsanız, modül içeri değildi. PowerShell modülü bulamıyor olduğunda hata oluşabilir. OMS ağ geçidi yükleme yolunda bulabilirsiniz: *C:\Program Files\Microsoft OMS Gateway\PowerShell*.

| **cmdlet'i** | **Parametreler** | **Açıklama** | **Örnek** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Anahtar |Hizmet yapılandırmasını alır |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Anahtarı (gerekli) <br> Değer |Hizmet yapılandırma değişiklikleri |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Geçiş (Yukarı Akış) proxy adresi alır |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Kullanıcı adı<br> Parola |Geçiş (Yukarı Akış) proxy adresi (ve kimlik bilgisi) ayarlar |1. Bir geçiş proxy ve kimlik bilgilerini ayarlayın:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Kimlik doğrulama gerekmez geçiş proxy ayarlayın: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Geçiş proxy ayarını temizleyin:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Şu anda izin verilen ana bilgisayar alır (yalnızca yerel olarak yapılandırılmış konak; izin verilen otomatik olarak indirilen izin verilen konakları dahil değildir) |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |İzin verilenler konağa ekler |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayar izin verilen listeden kaldırır |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İzin verilenler tabi istemci sertifikası ekler |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası konu izin verilen listeden kaldırır |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Şu anda izin verilen istemci sertifika konuları alır (yalnızca yerel olarak yapılandırılmış konuları; izin verilen otomatik olarak indirilen izin verilen konuları içermez) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Sorun giderme
Ağ Geçidi tarafından günlüğe kaydedilen olayları toplamak için de OMS aracısının yüklü olması gerekir.

![Olay Görüntüleyicisi'ni--OMS ağ geçidi günlük](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS ağ geçidi olay kimlikleri ve açıklamaları**

Aşağıdaki tabloda, olay kimlikleri ve açıklamaları OMS ağ geçidi günlük olayları gösterir:

| **ID** | **Açıklama** |
| --- | --- |
| 400 |Belirli bir kimliği yok. herhangi bir uygulama hatası |
| 401 |Yanlış yapılandırma. Örneğin: listenPort = "text" yerine bir tamsayı. |
| 402 |TLS el sıkışma iletileri ayrıştırılırken özel durum. |
| 403 |Ağ hatası. Örneğin: hedef sunucuya bağlanılamıyor. |
| 100 |Genel bilgiler. |
| 101 |Hizmeti başlatıldı. |
| 102 |Hizmeti durduruldu. |
| 103 |Bir HTTP BAĞLAMAK komutu istemciden aldı. |
| 104 |Olmayan bir HTTP BAĞLAMAK komutu. |
| 105 |Hedef sunucuda izin verilen listesindeki değil veya hedef bağlantı noktası güvenli bağlantı noktası (443) değil. <br> <br> Ağ geçidi sunucunuzda MMA aracı ve ağ geçidi ile iletişim kuran aracıları bağlandığını aynı günlük analizi çalışma alanına emin olun. |
| 105 |HATA TcpConnection – geçersiz bir istemci sertifikası: CN = ağ geçidi <br><br> Emin olun: <br>    <br> -Bir ağ geçidi 1.0.395.0 sürüm numarasıyla kullanmakta olduğunuz veya daha büyük. <br> -Ağ geçidi sunucunuzda MMA aracı ve ağ geçidi ile iletişim kuran aracıları aynı günlük analizi çalışma alanına bağlanır. |
| 106 |OMS ağ geçidi yalnızca TLS 1.0, TLS 1.1 ve 1.2 destekler.  SSL desteklemez. Tüm TLS/SSL protokolü için desteklenmeyen sürüm, olay kimliği 106 OMS ağ geçidi oluşturur.|
| 107 |TLS oturum doğrulandı. |

**Toplanacak performans sayaçları**

Aşağıdaki tabloda OMS ağ geçidi için kullanılabilen performans sayaçlarını gösterir. Sayaçlar, Performans İzleyicisi'ni kullanarak ekleyebilirsiniz.

| **Ad** | **Açıklama** |
| --- | --- |
| OMS ağ geçidi/etkin istemci bağlantısı |Etkin istemci (TCP) ağ bağlantısı sayısı |
| OMS ağ geçidi/hata sayısı |Hata sayısı |
| OMS ağ geçidi ve bağlı istemci |Bağlı istemci sayısı |
| OMS ağ geçidi/reddetme sayısı |Herhangi bir TLS doğrulama hatası nedeniyle reddi sayısı |

![OMS ağ geçidi performans sayaçları](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Yardım alın
Azure portalında oturum açtığında, Yardım için bir istek OMS ağ geçidi veya herhangi başka Azure hizmeti veya özellik, bir hizmetin ile oluşturabilirsiniz.

Yardım istemek için portalın sağ üst köşesindeki soru işareti simge seçin. Ardından **yeni destek isteği**. Yeni destek isteği formunu tamamlayın.

![Yeni destek isteği](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Sonraki adımlar
[Veri kaynakları ekleyin](log-analytics-data-sources.md) bağlı kaynaklarınızdan veri toplamak ve günlük analizi çalışma alanınızda depolamak için.
