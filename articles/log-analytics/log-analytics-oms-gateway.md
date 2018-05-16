---
title: OMS ağ geçidini kullanarak bilgisayarları bağlama | Microsoft Docs
description: Operations Manager izlenen bilgisayarlar ve cihazlar Internet erişimi olmadığında Azure Automation ve günlük analizi hizmeti veri göndermek için OMS geçidiyle bağlayın.
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
ms.date: 05/14/2018
ms.author: magoedte
ms.openlocfilehash: 18f7c0323493b73f4f136228fb9535ed63323c05
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="connect-computers-without-internet-access-using-the-oms-gateway"></a>OMS ağ geçidi'ni kullanarak Internet erişimi bilgisayarları bağlama
Bu belge, Azure Automation ile iletişimi yapılandırmak açıklar ve günlük doğrudan olduğunda OMS ağ geçidini kullanma analizi bağlı veya izlenen Operations Manager bilgisayarları Internet erişimine sahip değildir.  OMS HTTP BAĞLAMAK komutunu kullanarak HTTP tüneli destekleyen bir HTTP iletme proxy ağ geçidi, veri toplamak ve onların adına Azure Automation ve günlük analizi gönderebilirsiniz.  

OMS ağ geçidi destekler:

* Azure Otomasyon karma Runbook çalışanları  
* Windows bilgisayarları Microsoft Monitoring Agent ile doğrudan günlük analizi çalışma alanına bağlı
* Linux için OMS Aracısı ile Linux bilgisayarlar doğrudan bir günlük analizi çalışma alanına bağlı  
* System Center Operations Manager 2012 SP1 UR7 ile Operations Manager 2012 R2 UR3, Operations Manager 2016 ve Operations Manager sürüm 1801 yönetim grubu ile günlük analizi ile tümleşiktir.  

BT güvenlik ilkelerinizi bilgisayarlar noktası (POS) satış aygıtları veya BT Hizmetleri destekleyen sunucular gibi Internet bağlanmak için ağınızdaki izin vermez ancak bunları izlemek ve yönetmek için Azure Otomasyonu veya günlük analizi bağlanmak gerekiyorsa , yapılandırma ve şirket adına verileri almak için OMS ağ geçidi ile doğrudan iletişim kurmak için yapılandırılabilir.  Bu bilgisayarlar doğrudan bir günlük analizi çalışma alanına bağlanmak için OMS Aracısı ile yapılandırılmışsa, tüm bilgisayarlar yerine OMS ağ geçidi ile iletişim kurar.  Ağ geçidi veri aracılardan hizmetine doğrudan aktarır, bunu herhangi Aktarımdaki verileri analiz etmez.

Bir Operations Manager yönetim grubu günlük analizi ile tümleştirildiğinde, yönetim sunucuları yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak toplanan verileri göndermek için OMS ağ geçidine bağlanmak için yapılandırılabilir.  Operations Manager aracıları Operations Manager uyarıları, yapılandırma değerlendirmesi, örnek alanı ve kapasite verileri gibi bazı verileri yönetim sunucusuna gönderir. IIS günlükleri, performans ve güvenlik olayları gibi diğer yüksek hacimli verileri doğrudan OMS ağ geçidi için gönderilir.  Güvenilmeyen sistemlerini izlemek üzere bir DMZ veya diğer yalıtılmış ağ dağıtılmış bir veya daha fazla Operations Manager Ağ Geçidi sunucuları varsa, bir OMS ağ geçidi ile iletişim kuramıyor.  Operations Manager Ağ Geçidi sunucuları, yalnızca bir yönetim sunucusuna rapor edebilir.  Bir Operations Manager yönetim grubu OMS ağ geçidi ile iletişim kurmak için yapılandırıldığında proxy yapılandırma bilgilerini günlük analizi için veri toplamak üzere yapılandırılmış her aracı yönetilen bir bilgisayar için otomatik olarak dağıtılır olsa bile boş bir ayardır.    

Yüksek kullanılabilirlik sağlamak için doğrudan bağlı veya günlük analizi ile ağ geçidi üzerinden iletişim Operations Yönetim grupları, Ağ Yükü Dengeleme yönlendirebilir ve trafik birden fazla ağ geçidi sunucusu arasında dağıtmak için kullanabilirsiniz.  Bir ağ geçidi sunucusu kullanılamaz hale gelirse trafiği kullanılabilir başka bir düğüme yönlendirilir.  

OMS aracısı için bu iletişim ve kendi performansını veya olay verileri çözümlemek için OMS ağ geçidi izlemek için gereken hizmet uç noktaları tanımlamak OMS ağ geçidi'ni çalıştıran bilgisayar üzerindeki gereklidir.

Aracıları otomatik olarak ağ geçidi veri aktarmasına olanak veren her bir aracının kendi ağ geçidi için ağ bağlantısı olmalıdır. Ağ geçidi etki alanı denetleyicisine yüklenmesi önerilmez.

Aşağıdaki diyagramda doğrudan aracılardan Azure Automation ve ağ geçidi sunucusu kullanarak günlük analizi veri akışı gösterilmektedir.  Aracıları OMS ağ geçidi hizmete iletişim kuracak şekilde yapılandırıldı aynı bağlantı noktası eşleşmesi kendi proxy yapılandırmasını olması gerekir.  

![Hizmetleri diyagramı ile doğrudan aracı iletişimi](./media/log-analytics-oms-gateway/oms-omsgateway-agentdirectconnect.png)

Aşağıdaki diyagramda, günlük analizi için Operations Manager yönetim grubundan veri akışı gösterilmektedir.   

![Günlük analizi diyagramı ile Operations Manager iletişim](./media/log-analytics-oms-gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Önkoşullar

OMS ağ geçidini çalıştırmak için bir bilgisayara atandığında, bu bilgisayar aşağıdakilere sahip olmanız gerekir:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET framework 4.5
* En az bir 4 çekirdekli işlemci ve 8 GB bellek 
* Windows için OMS Aracısı 

### <a name="language-availability"></a>Dil kullanılabilirliği

OMS ağ geçidi aşağıdaki dillerde kullanılabilir:

- Çince (Basitleştirilmiş)
- Çince (Geleneksel)
- Çekçe
- Felemenkçe
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
Aşağıdaki tabloda vurgular desteklenen bir ağ geçidi sunucusu ile iletişim kurulurken aracıları sayısıdır.  Bu destek ~ 200 KB veri 6 saniyede karşıya aracısında temel alır. Test aracısı başına veri hacmi hakkında 2.7 günde GB'dir.

|Ağ geçidi |Desteklenen aracı yaklaşık sayısı|  
|--------|----------------------------------|  
|-CPU: Intel XEON CPU E5-2660 v3 2.6GHz 2 Çekirdek @<br> -Bellek: 4 GB<br> -Ağ bant genişliği: 1 GB/sn| 600|  
|-CPU: Intel XEON CPU E5-2660 v3 2.6GHz 4 çekirdek @<br> -Bellek: 8 GB<br> -Ağ bant genişliği: 1 GB/sn| 1000|  

## <a name="download-the-oms-gateway"></a>OMS ağ geçidini indirin

OMS ağ geçidi Kurulum dosyasının en son sürümünü almak için iki yolu vardır.

1. İndirin [Microsoft Yükleme Merkezi'nden](https://www.microsoft.com/download/details.aspx?id=54443).

2. Azure portalından indirin.  Sonra Azure portalında oturum açın:  

   1. Hizmetlerin listesini bulun ve seçin **günlük analizi**.  
   2. Bir çalışma alanı seçin.
   3. Çalışma alanı dikey penceresinde altında **genel**, tıklatın **Hızlı Başlangıç**.
   4. Altında **çalışma alanına bağlanmak için veri kaynağı seçin**, tıklatın **bilgisayarlar**.
   5. İçinde **doğrudan Aracısı** dikey penceresinde tıklatın **OMS ağ geçidi indirme**.<br><br> ![OMS ağ geçidini indirin](./media/log-analytics-oms-gateway/download-gateway.png)

or 

   1. Çalışma alanı dikey penceresinde altında **ayarları**, tıklatın **Gelişmiş ayarları**.
   2. Gidin **bağlı kaynakları** > **Windows sunucuları** tıklatıp **OMS ağ geçidi indirme**.

## <a name="install-the-oms-gateway"></a>OMS ağ geçidini yükleyin

Bir ağ geçidi yüklemek için aşağıdaki adımları gerçekleştirin.  Önceki bir sürümü yüklü değilse adıysa *günlük analizi iletici*, bu sürüme yükseltilir.  

1. Hedef klasördeki çift **OMS Gateway.msi**.
2. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.<br><br> ![Ağ geçidi Kurulum Sihirbazı](./media/log-analytics-oms-gateway/gateway-wizard01.png)<br> 
3. Üzerinde **Lisans Sözleşmesi'ni** sayfasında, **lisans sözleşmesinin koşullarını kabul ediyorum** EULA'yı kabul ve ardından **sonraki**.
4. Üzerinde **bağlantı noktası ve proxy adresi** sayfa:
   1. Ağ geçidi için kullanılan TCP bağlantı noktası numarasını yazın. Kurulum, Windows Güvenlik Duvarı'nda bu bağlantı noktası numarasına sahip bir gelen kuralı yapılandırır.  Varsayılan değer 8080'dir.
      Geçerli bağlantı noktası numarası 1 ile 65535 arasındadır. Giriş bu aralığa uymazsa bir hata iletisi görüntülenir.
   2. İsteğe bağlı olarak, ağ geçidinin yüklü olduğu sunucunun bir proxy üzerinden iletişim kurması gerekirse, burada bağlanmak için ağ geçidi gerekli proxy adresi yazın. Örneğin, `http://myorgname.corp.contoso.com:80`.  Boş bırakılırsa, doğrudan Internet'e bağlanmak ağ geçidi çalışacaktır.  Proxy sunucusu kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola girin.<br><br> ![Ağ geçidi Sihirbazı proxy yapılandırması](./media/log-analytics-oms-gateway/gateway-wizard02.png)<br>   
   3. **İleri**’ye tıklayın.
5. Microsoft Update etkinleştirilmişse yoksa etkinleştirmek seçebileceğiniz Microsoft Update sayfasında görüntülenir. Bir seçim yapın ve ardından **sonraki**. Aksi halde, sonraki adıma devam edin.
6. Üzerinde **hedef klasörü** sayfasında, varsayılan klasör C:\Program Files\OMS ağ geçidi bırakın ya da ağ geçidi yükleyin ve ardından istediğiniz konumu girin **sonraki**.
7. Üzerinde **yüklenmeye hazır** sayfasında, **yükleme**. Kullanıcı hesabı denetimi yüklemeye izin istemeden görünebilir. Öyleyse **Evet**.
8. Kurulum tamamlandıktan sonra **son**. Hizmet services.msc ek bileşenini açarak çalıştıran ve doğrulayın doğrulayabilirsiniz **OMS ağ geçidi** durum Hizmetleri ve listesinde görünür olan **çalıştıran**.<br><br> ![Hizmetleri – OMS ağ geçidi](./media/log-analytics-oms-gateway/gateway-service.png)  

## <a name="configure-network-load-balancing"></a>Ağ Yükü Dengeleme yapılandırma 
Ağ geçidi Ağ Yükü Dengeleme (NLB Microsoft Ağ Yükü Dengeleme (NLB) veya donanım tabanlı yük dengeleyici kullanarak) kullanarak yüksek kullanılabilirlik için yapılandırabilirsiniz.  Yük Dengeleyici OMS Aracısı istenen bağlantılarından veya Operations Manager yönetim sunucuları arasında düğümlerinden yönlendirerek trafiği yönetir. Bir ağ geçidi sunucusu kullanılamaz hale gelirse trafiği diğer düğümlere yönlendirilir.

Tasarım ve bir Windows Server 2016 Ağ Yükü Dengeleme kümesi dağıtma hakkında bilgi edinmek için [Ağ Yükü Dengeleme](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Aşağıdaki adımlar, Microsoft Ağ Yükü Dengeleme kümesini yapılandırın açıklar.  

1.  Bir yönetici hesabı ile NLB kümesinin bir üyesi olan Windows sunucuya oturum açın.  
2.  Sunucu Yöneticisi'nde Ağ Yükü Dengeleme Yöneticisi'ni açın, **Araçları**ve ardından **Ağ Yükü Dengeleme Yöneticisi**.
3. Microsoft izleme aracısının yüklü bir OMS Ağ Geçidi sunucusuna bağlanmak için kümenin IP adresine sağ tıklayın ve ardından **kümeye konak Ekle**.<br><br> ![Ağ Yükü Dengeleme Yöneticisi – kümeye konak Ekle](./media/log-analytics-oms-gateway/nlb02.png)<br> 
4. Bağlanmak istediğiniz Ağ Geçidi sunucusunun IP adresini girin.<br><br> ![Ağ Yükü Dengeleme Yöneticisi – kümeye konak Ekle: bağlanma](./media/log-analytics-oms-gateway/nlb03.png) 
    
## <a name="configure-oms-agent-and-operations-manager-management-group"></a>OMS aracısı ve Operations Manager yönetim grubu yapılandırma
Aşağıdaki bölümde, Azure Automation veya günlük analizi ile iletişim kurmak için OMS ağ geçidi ile doğrudan bağlı OMS Aracısı, bir Operations Manager yönetim grubu ya da Azure Otomasyon karma Runbook çalışanlarını yapılandırma adımlarını içerir.  

Gereksinimleri ve Windows bilgisayarları doğrudan Log Analytics'e bağlanması OMS Aracısı'nı yüklemek adımları anlamak için bkz: [günlük analizi bağlanmak Windows bilgisayarlara](log-analytics-windows-agents.md) veya Linux bilgisayarları bakın [bağlanmak Linux Günlük analizi bilgisayarlara](log-analytics-quick-collect-linux-computer.md).  Otomasyon karma Runbook çalışanı ilgili daha fazla bilgi için bkz: [dağıtmak karma Runbook çalışanı](../automation/automation-hybrid-runbook-worker.md).

### <a name="configuring-the-oms-agent-and-operations-manager-to-use-the-oms-gateway-as-a-proxy-server"></a>OMS aracısı ve Operations Manager OMS ağ geçidi bir proxy sunucusu olarak kullanacak şekilde yapılandırma

### <a name="configure-standalone-oms-agent"></a>Tek başına OMS Aracısı'nı yapılandırma
Bkz: [proxy ve güvenlik duvarı ayarlarını Microsoft İzleme Aracısı ile yapılandırma](log-analytics-proxy-firewall.md) bir aracının bir proxy sunucusu kullanacak şekilde yapılandırma hakkında daha fazla bilgi için bu durumda olduğu ağ geçidi.  Ağ Yük Dengeleyici arkasında birden çok ağ geçidi sunucusu dağıttıysanız, OMS Aracısı proxy yapılandırmasını NLB sanal IP adresi şöyledir:<br><br> ![Microsoft İzleme Aracısı Özellikleri – Proxy ayarları](./media/log-analytics-oms-gateway/nlb04.png)

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Operations Manager yapılandırma - tüm aracıları aynı proxy sunucuyu kullan
Operations Manager ağ geçidi sunucusu eklemek için yapılandırın.  Ayarı boş olsa bile, Operations Manager proxy yapılandırması tüm aracılar Operations Manager için raporlama otomatik olarak uygulanır.  

Operations Manager desteklemek için ağ geçidini kullanmak için şunlara sahip olmalısınız:

* Microsoft Monitoring Agent (aracı sürümü – **8.0.10900.0** veya sonrası) Ağ Geçidi sunucusunda yüklü ve istediğiniz iletişim kurmak günlük analizi çalışma alanları için yapılandırılmış.
* Ağ geçidi yapan bir proxy sunucusuna bağlı olmanız veya Internet bağlantısı olması gerekir.

> [!NOTE]
> Ağ geçidi için bir değer belirtmezseniz, boş değerler için tüm aracılar atılır.
> 

Operations Manager yönetim grubunuzu günlük analizi çalışma alanıyla kaydetme ilk kez kullanıyorsanız, proxy yapılandırması yönetim grubu için belirtme seçeneği Operations konsolunda kullanılabilir değil.  Yönetim grubu bu seçeneği kullanılabilir olmadan önce Hizmeti'ne başarıyla kayıtlı olması gerekir.  Netsh sistemde Operations konsolundan çalıştıran tümleştirme ve tüm yönetim sunucuları yönetim grubunda yapılandırmak için kullanarak sistem proxy yapılandırmasını güncelleştirmeniz gerekir.  

1. Bir yükseltilmiş bir komut istemi açın.
   a. Git **Başlat** ve türü **cmd**.
   b. Sağ **komut istemi** ve Çalıştır yönetici ** seçin.
2. Aşağıdaki komut ve ENTER tuşuna basın **Enter**:

    `netsh winhttp set proxy <proxy>:<port>`

Günlük analizi ile tümleştirme tamamladıktan sonra değişikliğin çalıştırarak kaldırabilirsiniz `netsh winhttp reset proxy` ve sonra da **proxy sunucusunu yapılandır** OMS ağ geçidi sunucusu belirtmek için işletim konsolunda seçeneği. 

1. Operations Manager konsolunu açın ve altında **Operations Management Suite**, tıklatın **bağlantı** ve ardından **Proxy sunucusunu yapılandırma**.<br><br> ![Operations Manager – Proxy sunucusunu yapılandırın](./media/log-analytics-oms-gateway/scom01.png)<br> 
2. Seçin **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak** ve OMS Ağ Geçidi sunucusunun IP adresini veya NLB sanal IP adresini yazın. İle başlatıldığından emin olmak `http://` öneki.<br><br> ![Operations Manager – proxy sunucusu adresi](./media/log-analytics-oms-gateway/scom02.png)<br> 
3. **Son**'a tıklayın. Operations Manager yönetim grubunuzu şimdi günlük analizi hizmeti için ağ geçidi sunucusu üzerinden iletişim kuracak şekilde yapılandırıldı.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Operations Manager yapılandırma - belirli aracıları proxy sunucusu kullan
Büyük veya karmaşık ortamları için OMS ağ geçidi sunucusu kullanmak için belirli sunucuları (veya gruplar) yalnızca isteyebilirsiniz.  Bu sunucular için Operations Manager Aracısı güncelleştirilemiyor. Bu değer yönetim grubu için genel değeriyle üzerine doğrudan.  Bunun yerine bu değerleri göndermek için kullanılan kural geçersiz kılmanız gerekir.  

> [!NOTE] 
> Aynı yapılandırma tekniği ortamınızda birden çok OMS ağ geçidi sunucusu kullanımına izin vermek için kullanılabilir.  Örneğin, bir bölge başına temelinde belirtilmesi için belirli OMS Ağ Geçidi sunucuları gerektirebilir.
>  

1. Operations Manager konsolunu açın ve seçin **yazma** çalışma.  
2. Yazma çalışma alanında seçin **kuralları** tıklatıp **kapsam** Operations Manager araç çubuğunda. Bu düğme kullanılamıyorsa, izleme bölmesinde bir nesne seçili bir klasör değil sahip olduğunuzdan emin olmak için kontrol edin. **Kapsam Yönetim Paketi nesneleri** iletişim kutusunda ortak hedeflenen sınıfları, grupları veya nesneleri listesini görüntüler. 
3. Tür **sistem durumu hizmeti** içinde **Ara** alan ve listeden seçin.  **Tamam**’a tıklayın.  
4. Arama kuralı için **Danışmanı Proxy ayarı kural** Operations konsolu araç tıklatıp **geçersiz kılar** üzerine gelin ve ardından **Rule\For sınıfın belirli bir nesnesi geçersiz kılma: sistem durumu hizmeti**  ve belirli bir nesneyi listeden seçin.  İsteğe bağlı olarak, sistem durumu hizmeti nesnesi, bu geçersiz kılma uygulayın ve ardından bu gruba geçersiz kılma uygulayın istediğiniz sunucuları içeren özel bir grup oluşturabilirsiniz.
5. İçinde **geçersiz kılma özellikleri** iletişim kutusu, bir onay işareti tıklatıp **geçersiz kılma** yanındaki sütuna **WebProxyAddress** parametresi.  İçinde **geçersiz kılma değeri** ile başlayan URL OMS ağ geçidi sunucusu sağlamaya alanına, `http://` öneki.  

    >[!NOTE]
    > Bunu zaten otomatik olarak Microsoft System Center Advisor Monitoring Server grubu hedefleme Microsoft System Center Advisor Güvenli başvuru geçersiz kılma yönetim paketinde bir geçersiz kılma ile yönetilen olarak Kuralı etkinleştirmek gerekmez.
    >   

6. Bir yönetim paketinden seçin **hedef Yönetim Paketi seçin** listelemek veya tıklatarak yeni bir korumasız Yönetim Paketi oluşturun **yeni**. 
7. Değişikliklerinizi tamamladığınızda tıklatın **Tamam**. 

### <a name="configure-for-automation-hybrid-workers"></a>Otomasyon karma çalışanları için yapılandırma
Ortamınızda Otomasyon karma Runbook çalışanları varsa, aşağıdaki adımlar desteklemek için ağ geçidini yapılandırmak için el ile geçici geçici çözümler sağlar.

Aşağıdaki adımlarda Otomasyon hesabının bulunduğu Azure bölgesi bilmeniz gerekir. Konumu bulmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Azure Otomasyon hizmetine seçin.
3. Uygun Azure Otomasyon hesabı seçin.
4. Kendi bölge altında görüntülemek **konumu**.<br><br> ![Azure portal – Otomasyon hesabı konumu](./media/log-analytics-oms-gateway/location.png)  

Her konum URL'sini belirlemek için aşağıdaki tabloları kullanın:

**İş çalışma zamanı veri hizmeti URL'leri**

| **konum** | **URL** |
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

| **konum** | **URL** |
| --- | --- |
| Orta Kuzey ABD |ncus-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-agentservice-üretim-1.azure-automation.net |
| Orta Kanada |cc-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-agentservice-üretim-1.azure-automation.net |
| Güneydoğu Asya |sea-agentservice-prod-1.azure-automation.net |
| Orta Hindistan |cid-agentservice-prod-1.azure-automation.net |
| Japonya |jpe-agentservice-üretim-1.azure-automation.net |
| Avustralya |Ana-agentservice-üretim-1.azure-automation.net |

Bilgisayarınızı bir karma Runbook çalışanı olarak otomatik olarak güncelleştirme yönetimi çözümü kullanarak düzeltme eki uygulama için kayıtlı değilse, aşağıdaki adımları izleyin:

1. İş çalışma zamanı veri hizmeti URL'leri OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. OMS ağ geçidi hizmeti aşağıdaki PowerShell cmdlet'ini kullanarak yeniden başlatın: `Restart-Service OMSGatewayService`

Bilgisayarınız karma Runbook çalışanı kayıt cmdlet'ini kullanarak Azure Otomasyonu dahil edilmiş ise, şu adımları izleyin:

1. Aracı hizmeti kayıt URL'si OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. İş çalışma zamanı veri hizmeti URL'leri OMS ağ geçidi ana bilgisayar izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. OMS ağ geçidi hizmeti yeniden başlatın.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Yararlı PowerShell cmdlet'leri
Cmdlet'leri OMS ağ geçidi yapılandırma ayarlarını güncelleştirmek için gerekli görevleri tamamlamanıza yardımcı olabilir. Kullanmadan önce emin olun:

1. OMS ağ geçidi (MSI) yükleyin.
2. Bir PowerShell konsol penceresi açın.
3. Modülü içeri aktarmak için aşağıdaki komutu yazın: `Import-Module OMSGateway`
4. Önceki adımda herhangi bir hata oluştu, modülü başarıyla içeri aktarıldı ve cmdlet'leri kullanılabilir. Türü `Get-Module OMSGateway`
5. Cmdlet'lerini kullanarak değişiklikleri yaptıktan sonra ağ geçidi hizmeti yeniden emin olun.

3. adımında bir hata alırsanız, modül içeri değildi. PowerShell modülü bulamıyor olduğunda hata oluşabilir. Ağ geçidi yükleme yolunda bulabilirsiniz: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **cmdlet'i** | **Parametreler** | **Açıklama** | **Örnek** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Anahtar |Hizmet yapılandırmasını alır |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Anahtarı (gerekli) <br> Değer |Hizmet yapılandırma değişiklikleri |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Geçiş (Yukarı Akış) proxy adresi alır |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Kullanıcı adı<br> Parola |Geçiş (Yukarı Akış) proxy adresi (ve kimlik bilgisi) ayarlar |1. Bir geçiş proxy ve kimlik bilgilerini ayarlayın:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Kimlik doğrulama gerekmez geçiş proxy ayarlayın: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Geçiş proxy ayarını temizleyin:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |(Yalnızca yerel olarak yapılandırılmış izin verilen ana, otomatik olarak indirilen izin verilen ana içermez) şu anda izin verilen ana bilgisayar alır |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |İzin verilenler konağa ekler |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayar izin verilen listeden kaldırır |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İzin verilenler tabi istemci sertifikası ekler |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası konu izin verilen listeden kaldırır |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Şu anda izin verilen istemci sertifika konuları alır (yalnızca yerel olarak yapılandırılmış konuları izin verilen, izin verilen otomatik olarak indirilen konuları içermez) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Sorun giderme
Ağ Geçidi tarafından günlüğe kaydedilen olayları toplamak için de OMS aracısının yüklü olması gerekir.<br><br> ![Olay Görüntüleyicisi'ni – OMS ağ geçidi günlük](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS ağ geçidi olay kimlikleri ve açıklamaları**

Aşağıdaki tabloda, olay kimlikleri ve açıklamaları OMS ağ geçidi günlük olayları gösterir.

| **ID** | **Açıklama** |
| --- | --- |
| 400 |Belirli bir Kimliğe sahip olmayan herhangi bir uygulama hatası |
| 401 |Yanlış yapılandırma. Örneğin: listenPort tamsayı yerine "metin" = |
| 402 |TLS el sıkışma iletileri ayrıştırılırken bir özel durum |
| 403 |Ağ hatası. Örneğin: hedef sunucuya bağlanılamıyor |
| 100 |Genel bilgiler |
| 101 |Hizmeti başlatıldı |
| 102 |Hizmeti durdu |
| 103 |Bir HTTP BAĞLAMAK komutu istemciden alınan |
| 104 |Olmayan bir HTTP BAĞLAMAK komutu |
| 105 |Hedef sunucuda izin verilen listesindeki değil veya hedef bağlantı noktası güvenli bağlantı noktası (443) değil <br> <br> Ağ geçidi sunucunuzda MMA aracı ve ağ geçidi ile iletişim kurmasını aracıları bağlandığını aynı günlük analizi çalışma alanına emin olun. |
| 105 |HATA TcpConnection – geçersiz bir istemci sertifikası: CN = ağ geçidi <br><br> Emin olun: <br>    <br> &#149;1.0.395.0 sürüm numarasına sahip bir ağ geçidi kullanarak veya daha büyük. <br> &#149;Ağ geçidi sunucunuzda MMA aracı ve ağ geçidi ile iletişim kurmasını aracıları aynı günlük analizi çalışma alanına bağlı. |
| 106 |OMS ağ geçidi yalnızca TLS 1.0, TLS 1.1 ve 1.2 destekler.  SSL desteklemez. Tüm TLS/SSL protokolü için desteklenmeyen sürüm, olay kimliği 106 OMS ağ geçidi oluşturur.|
| 107 |TLS oturum doğrulandı |

**Toplanacak performans sayaçları**

Aşağıdaki tabloda OMS ağ geçidi için kullanılabilen performans sayaçlarını gösterir. Performans İzleyicisi'ni kullanarak sayaçlar ekleyebilirsiniz.

| **Ad** | **Açıklama** |
| --- | --- |
| OMS ağ geçidi/etkin istemci bağlantısı |Etkin istemci (TCP) ağ bağlantısı sayısı |
| OMS ağ geçidi/hata sayısı |Hata sayısı |
| OMS ağ geçidi ve bağlı istemci |Bağlı istemci sayısı |
| OMS ağ geçidi/reddetme sayısı |Herhangi bir TLS doğrulama hatası nedeniyle reddi sayısı |

![OMS ağ geçidi performans sayaçları](./media/log-analytics-oms-gateway/counters.png)

## <a name="get-assistance"></a>Yardım alın
Azure portalında oturum açtığında, Yardım için bir istek OMS ağ geçidi veya herhangi başka Azure hizmeti veya özellik, bir hizmetin ile oluşturabilirsiniz.
Yardım isteğinde, portalın sağ üst köşedeki soru işareti sembolü tıklatın ve ardından **yeni destek isteği**. Ardından, yeni destek isteği formunu tamamlayın.

![Yeni destek isteği](./media/log-analytics-oms-gateway/support.png)

## <a name="next-steps"></a>Sonraki adımlar
[Veri kaynakları ekleyin](log-analytics-data-sources.md) bağlı kaynaklardan veri toplamak ve günlük analizi çalışma alanınızda depolamak için.
