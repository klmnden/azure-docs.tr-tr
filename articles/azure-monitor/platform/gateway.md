---
title: Bilgisayarları Log Analytics ağ geçidini kullanarak bağlan | Microsoft Docs
description: Operations Manager tarafından izlenen bilgisayarların ve cihazların Internet erişimi olmadığında Azure otomasyon ve Log Analytics hizmeti için veri göndermek için Log Analytics ağ geçidi ile bağlanın.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ae9a1623-d2ba-41d3-bd97-36e65d3ca119
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: magoedte
ms.openlocfilehash: e4ea964600c03ce3f3b5b276ed02d12f573814bf
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2019
ms.locfileid: "55756504"
---
# <a name="connect-computers-without-internet-access-using-the-log-analytics-gateway"></a>Bilgisayarları Log Analytics ağ geçidini kullanarak Internet erişimi olmadan bağlayın

>[!NOTE]
>Azure İzleyici sürekli geçiş Microsoft Operations Management Suite (OMS) gelen bir parçası olarak, OMS ağ geçidi için Log Analytics ağ geçidi olarak adlandırılır. 
>

Bu belge, Azure Otomasyonu ile iletişim yapılandırılacağını açıklar ve bağlı olduğunda doğrudan Log Analytics ağ geçidi kullanarak Log Analytics'e veya Operations Manager'ın izlenen bilgisayarların Internet erişimi yoktur.  HTTP HTTP CONNECT komutunu kullanarak tüneli destekleyen bir HTTP iletim proxy'si olan Log Analytics ağ geçidi, veri toplamak ve Log Analytics ve Azure Otomasyonu ile kendi adınıza gönderin.  

Log Analytics gateway destekler:

* Raporlama kadar aynı dört Log Analytics çalışma alanları aracıları arkasındaki ile yapılandırılır.  
* Azure Otomasyon karma Runbook çalışanları  
* Microsoft İzleme Aracısı ile Windows bilgisayarları doğrudan Log Analytics çalışma alanına bağlı
* Linux için Log Analytics Aracısı ile Linux bilgisayarları doğrudan Log Analytics çalışma alanına bağlı  
* System Center Operations Manager 2012 SP1 UR7 ile Operations Manager 2012 R2 UR3, Operations Manager 2016 ve Operations Manager sürümü 1801 yönetim grubu ile Log Analytics ile tümleşiktir.  

BT güvenlik ilkeleriniz ağınızdaki noktası satışı (POS) cihazları veya BT Hizmetleri destekleyen sunucular gibi Internet'e bağlanmak için bilgisayarları nepovolit ancak bunları izlemek ve yönetmek için Azure Otomasyonu veya Log Analytics'e bağlanmak gerekiyorsa , yapılandırma ve gerçekleştirilemeyeceğine ilişkin verileri almak için doğrudan Log Analytics ağ geçidi ile iletişim kurmak için yapılandırılabilir.  Bu bilgisayarları doğrudan Log Analytics çalışma alanına bağlamak için Log Analytics aracısını ile yapılandırıldıysa, tüm bilgisayarları bunun yerine Log Analytics ağ geçidi ile iletişim kurar.  Ağ geçidi veri aracılardan hizmete doğrudan aktarır, bunu herhangi bir veri aktarım sırasında analiz etmez.

Bir Operations Manager yönetim grubunu Log Analytics ile tümleştirildiğinde, yönetim sunucuları yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak toplanan verileri göndermek için Log Analytics ağ geçidine bağlanmak için yapılandırılabilir.  Operations Manager aracıları Operations Manager uyarıları, yapılandırma değerlendirmesi, örnek alanı ve kapasite verileri gibi bazı verileri yönetim sunucusuna gönderir. IIS günlükleri, performans ve güvenlik olaylarını gibi diğer yüksek hacimli verileri doğrudan Log Analytics ağ geçidine gönderilir.  Güvenilmeyen Sistemleri'ni izlemek için bir DMZ veya diğer yalıtılmış ağda dağıtılan bir veya daha fazla Operations Manager Ağ Geçidi sunucuları varsa, Log Analytics-ağ geçidi ile iletişim kuramıyor.  Operations Manager Ağ Geçidi sunucuları, yalnızca bir yönetim sunucusuna rapor edebilirsiniz.  Bir Operations Manager yönetim grubunu Log Analytics ağ geçidi ile iletişim kurmak için yapılandırıldığında proxy yapılandırma bilgileri için Log Analytics, hatta veri toplamak üzere yapılandırılmış tüm aracıyla yönetilen bilgisayara otomatik olarak dağıtılır ayarı boş ise.    

Yüksek kullanılabilirlik sağlamak için doğrudan bağlanabilir veya Log Analytics ile iletişim kurmak için ağ geçidinden Operations Yönetim grupları, Ağ Yükü Dengeleme yeniden yönlendirme ve trafik birden fazla ağ geçidi sunucusu arasında dağıtmak için kullanabilirsiniz.  Bir ağ geçidi sunucusu kalırsa, trafiği kullanılabilir başka bir düğüme yönlendirilir.  

Log Analytics Windows aracısını Log Analytics ağ geçidi ile iletişim kurmak için gereken hizmet uç noktaları yalnızca tanımlamak için aynı zamanda rapor için sırayla aynı çalışma alanına çalıştıran bilgisayarda gereklidir, aracılar veya Operations Manager ağ geçidinin arkasındaki yönetim grubu ile yapılandırılır. Bu, ağ geçidinin kendi atanan çalışma alanına iletişim kurmasına izin vermek gereklidir. Bu çalışma alanlarının bir Windows aracısının desteklediği toplam sayısını olduğu en fazla dört çalışma alanları için birden çok girişli bir ağ geçidi olabilir.  

Her bir aracı, aracıları otomatik olarak verileri azure'a veya dışarı aktarabilmesi ağ geçidi için ağ bağlantısı olması gerekir. Ağ geçidini bir etki alanı denetleyicisine yüklenmesi önerilmez.

Doğrudan aracılardan Azure Otomasyonu ve ağ geçidi sunucusu kullanarak Log Analytics'e veri akışı aşağıdaki diyagramda gösterilmektedir. Aracıları, Log Analytics ağ geçidi ile yapılandırılmış aynı bağlantı noktası eşleşmesi kendi proxy yapılandırmasına sahip olmalıdır.  

![Hizmetleri diyagramı ile doğrudan aracı iletişimi](./media/gateway/oms-omsgateway-agentdirectconnect.png)

Aşağıdaki diyagramda, bir Operations Manager yönetim grubundan Log Analytics'e veri akışı gösterilmektedir.   

![Log Analytics diyagram ile Operations Manager iletişim](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="prerequisites"></a>Önkoşullar

Log Analytics ağ geçidini çalıştırmak için bir bilgisayar belirlerken, bu bilgisayarın aşağıdakilere sahip olmanız gerekir:

* Windows 10, Windows 8.1, Windows 7
* Windows Server 2016, Windows Server 2012 R2'de, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008
* .NET framework 4.5
* En az bir 4 çekirdekli işlemci ve 8 GB bellek 
* [Windows için log Analytics aracısını](agent-windows.md) yüklenir ve ağ geçidi üzerinden iletişim kuran aracıları aynı çalışma alanına rapor için yapılandırılır.  

### <a name="language-availability"></a>Dil kullanılabilirlik

Log Analytics ağ geçidi, aşağıdaki dillerde kullanılabilir:

- Çince (Basitleştirilmiş)
- seçenekleri yerine
- Çekçe
- Felemenkçe
- Türkçe
- Fransızca 
- Almanca 
- Macarca
- İtalyanca
- Japonca
- Korece
- Lehçe
- Portekizce (Brezilya)
- Portekizce (Portekiz)
- Rusça
- İspanyolca (uluslararası)

### <a name="supported-encryption-protocols"></a>Desteklenen şifreleme protokolleri
Log Analytics ağ geçidi, yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2 destekler.  Güvenli Yuva Katmanı (SSL) desteklemiyor.  Log analytics'e Aktarımdaki verilerin güvenliğini sağlamak üzere en az ağ geçidini yapılandırmak için önemle öneririz Aktarım Katmanı Güvenliği (TLS) 1.2. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**.  Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

### <a name="supported-number-of-agent-connections"></a>Desteklenen aracı bağlantı sayısı
Aşağıdaki tabloda, desteklenen bir ağ geçidi sunucusu ile iletişim kuran aracıları sayısını vurgulanmaktadır.  Bu destek, 6 saniyede yaklaşık 200 KB veri karşıya aracılarda temel alır. Test aracı başına veri hacmi hakkında 2.7 günde GB'dir.

|Ağ geçidi |Yaklaşık desteklenen aracı sayısı|  
|--------|----------------------------------|  
|- CPU: Intel XEON 2660 CPU E5 v3 \@ 2,6 GHz 2 Çekirdek<br> -Bellek: 4 GB<br> -Ağ bant genişliği: 1 Gbps| 600|  
|- CPU: Intel XEON 2660 CPU E5 v3 \@ 2,6 GHz 4 çekirdek<br> -Bellek: 8 GB<br> -Ağ bant genişliği: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Log Analytics ağ geçidini indirin

Log Analytics gateway kurulum dosyası en son sürümünü almak için iki yolu vardır.

1. İndirmesine [Microsoft İndirme Merkezi](https://www.microsoft.com/download/details.aspx?id=54443).

1. Azure portalından indirin.  Sonra Azure portalında oturum açın:  

   1. Hizmetler listesine göz atın ve ardından **Log Analytics**.  
   1. Bir çalışma alanı seçin.
   1. Çalışma alanı dikey penceresinde altında **genel**, tıklayın **Hızlı Başlangıç**.
   1. Altında **çalışma alanına bağlamak için bir veri kaynağı seçin**, tıklayın **bilgisayarlar**.
   1. İçinde **doğrudan aracı** dikey penceresinde tıklayın **indirme Log Analytics gateway**.<br><br> ![Log Analytics gateway indirin](./media/gateway/download-gateway.png)

or 

   1. Çalışma alanı dikey penceresinde altında **ayarları**, tıklayın **Gelişmiş ayarlar**.
   1. Gidin **bağlı kaynaklar** > **Windows sunucuları** tıklatıp **indirme Log Analytics gateway**.

## <a name="install-the-log-analytics-gateway"></a>Log Analytics ağ geçidi yükleme

Bir ağ geçidi yüklemek için aşağıdaki adımları gerçekleştirin.  Önceki bir sürümü yüklü değilse, eski adıyla *Log Analytics ileticisi*, bu sürümüne yükseltilir.  

1. Hedef klasördeki çift **Log Analytics gateway.msi**.
1. **Hoş Geldiniz** sayfasında **İleri**'ye tıklayın.<br><br> ![Ağ geçidi Kurulum Sihirbazı](./media/gateway/gateway-wizard01.png)<br> 
1. Üzerinde **Lisans Sözleşmesi** sayfasında **lisans sözleşmesinin koşullarını kabul ediyorum** EULA'yı kabul edin ve ardından **sonraki**.
1. Üzerinde **bağlantı noktası ve proxy adresi** sayfası:
   1. Ağ geçidi için kullanılan TCP bağlantı noktası numarasını yazın. Kurulum, Windows Güvenlik duvarında bu bağlantı noktası numarası ile bir gelen kuralı yapılandırır.  Varsayılan değer 8080'dir.
      Bağlantı noktası numarası geçerli aralık 1-65535 arasındadır. Giriş bu aralığı içinde kalmıyorsa, bir hata iletisi görüntülenir.
   1. İsteğe bağlı olarak, ağ geçidinin yüklü olduğu sunucunun bir proxy üzerinden iletişim kurması gerekiyorsa, ağ geçidine bağlanmak için gereken yere proxy adresi yazın. Örneğin, `http://myorgname.corp.contoso.com:80`.  Boş bırakılırsa, ağ geçidi doğrudan Internet'e bağlanmaya çalışacaktır.  Ara sunucunuz kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola girin.<br><br> ![Ağ geçidi Sihirbazı proxy yapılandırması](./media/gateway/gateway-wizard02.png)<br>   
   1. **İleri**'ye tıklayın.
1. Microsoft Update etkin değilse, bunu etkinleştirmek seçebileceğiniz Microsoft Update sayfasında görünür. Bir seçim yapın ve ardından **sonraki**. Aksi halde, sonraki adıma devam edin.
1. Üzerinde **hedef klasör** sayfasında varsayılan klasörü C:\Program Files\OMS ağ geçidi bırakın ya da ağ geçidi yüklemeniz ve ardından istediğiniz konumu yazın **sonraki**.
1. Üzerinde **yüklenmeye hazır** sayfasında **yükleme**. Kullanıcı hesabı denetimi yükleme izni isteyen görünebilir. Öyleyse **Evet**.
1. Kurulum tamamlandıktan sonra tıklayın **son**. Hizmeti services.msc ek bileşenini açarak çalışan ve doğrulayın doğrulayabilirsiniz **OMS ağ geçidi** durumu ve Hizmetler listesinde görünür olan **çalıştıran**.<br><br> ![Hizmetleri – Log Analytics ağ geçidi](./media/gateway/gateway-service.png)  


## <a name="configure-network-load-balancing"></a>Ağ Yükü Dengeleme yapılandırma 
Ağ geçidi Ağ Yükü Dengeleme (NLB Microsoft Ağ Yükü Dengeleme (NLB) veya donanım tabanlı yük Dengeleyiciler kullanarak) kullanarak yüksek kullanılabilirlik için yapılandırabilirsiniz.  Yük dengeleyicinin trafiği istenen bağlantılar Log Analytics aracılardan veya Operations Manager yönetim sunucuları arasında düğümlerini yönlendirerek yönetir. Bir ağ geçidi sunucusu kalırsa, trafiğin diğer düğümlere yönlendirilir.

Tasarım ve bir Windows Server 2016 Ağ Yükü Dengeleme kümesi dağıtma hakkında bilgi edinmek için [Ağ Yükü Dengeleme](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  Aşağıdaki adımlar, Microsoft Ağ Yükü Dengeleme kümesini yapılandırın açıklanmaktadır.  

1. Bir yönetim hesabıyla NLB kümesinin bir üyesi olan Windows sunucuya oturum açın.  
1. Sunucu Yöneticisi'nde Ağ Yükü Dengeleme Yöneticisi'ni açın, **Araçları**ve ardından **Ağ Yükü Dengeleme Yöneticisi**.
1. Microsoft izleme aracısının yüklü olduğu bir Log Analytics Ağ Geçidi sunucusuna bağlanmak için kümenin IP adresine sağ tıklayın ve ardından **konak kümesine Ekle**.<br><br> ![Ağ Yükü Dengeleme Yöneticisi – kümeye konak Ekle](./media/gateway/nlb02.png)<br> 
1. Bağlamak istediğiniz ağ geçidi sunucusu IP adresini girin.<br><br> ![Ağ Yükü Dengeleme Yöneticisi – konak kümesine ekleyin: Bağlan](./media/gateway/nlb03.png) 
    
## <a name="configure-log-analytics-agent-and-operations-manager-management-group"></a>Log Analytics aracısını ve Operations Manager yönetim grubu yapılandırma
Aşağıdaki bölümde, Azure Otomasyonu ya da günlük ile iletişim kurmak için Log Analytics ağ geçidi ile doğrudan bağlı Log Analytics aracılarını, bir Operations Manager yönetim grubu veya Azure Otomasyon karma Runbook çalışanlarını yapılandırma adımları içerir Analytics.  

### <a name="configure-standalone-log-analytics-agent"></a>Tek başına Log Analytics aracısını yapılandırma
Gereksinimler ve Log Analytics aracısını Windows bilgisayarları doğrudan Log Analytics'e bağlanması ve ağ geçidi yüklemek adımları anlamak için bkz [bağlanmak Windows bilgisayarlarını Log Analytics'e](agent-windows.md) veya Linux için bilgisayarları bakın.[ Linux bilgisayarlarını Log Analytics'e bağlama](../../azure-monitor/learn/quick-collect-linux-computer.md). Aracısı yapılandırılırken bir ara sunucu belirtmek yerine, bu değer, bağlantı noktası numarası ile Log Analytics ağ geçidi sunucusu ve IP adresi ile değiştirin. Bir ağ yük dengeleyicinin arkasına birden çok ağ geçidi sunucusu dağıttıysanız, Log Analytics Aracısı Ara sunucu yapılandırmasını NLB sanal IP adresi ' dir.  

Ağ Geçidi sunucusunda aracıyı yükledikten sonra çalışma alanına veya çalışma alanları aracıları ağ geçidine Konuşmayı bildirmek için yapılandırabilirsiniz. Log Analytics Windows Aracısı ağ geçidinde yüklü değilse, olay 300 yazılan **OMS ağ geçidi günlüğüne** olay günlüğünü aracı belirten yüklü olması gerekir. Aracı yüklü ancak üzerinden iletişim kuran aracıları aynı çalışma alanına rapor için yapılandırılmamış, olay 105 ağ geçidi aracıda t için Konuşmayı aracıları aynı çalışma alanına rapor için yapılandırılması gerekir belirten aynı olay günlüğüne yazılır He ağ geçidi.

Yapılandırmayı tamamladıktan sonra yeniden başlatmanız **OMS ağ geçidi** değişikliklerin etkili olması hizmet. Aksi takdirde, ağ geçidi Log Analytics ve rapor olayı kimliği 105 ile iletişim kurmaya çalışan aracıları reddeder **OMS ağ geçidi günlüğüne** olay günlüğü. Eklediğinizde veya aracı yapılandırması Ağ Geçidi sunucusunda bir çalışma alanı kaldırmak için de geçerlidir.   

Otomasyon karma Runbook çalışanı için ilgili daha fazla bilgi için bkz: [karma Runbook çalışanı dağıtma](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager---all-agents-use-the-same-proxy-server"></a>Operations Manager yapılandırma - tüm aracıları aynı proxy sunucusu kullan
Operations Manager ağ geçidi sunucusu eklemek için yapılandırın.  Ayarı boş olsa bile, Operations Manager proxy yapılandırması tüm aracılar Operations Manager'da raporlama için otomatik olarak uygulanır.  

Operations Manager'ı desteklemek için ağ geçidini kullanmak için şunlara sahip olmalısınız:

* Microsoft Monitoring Agent (aracı sürümü – **8.0.10900.0** veya üzeri) Ağ Geçidi sunucusuna ve yönetim grubunuzun raporlama yapacak şekilde yapılandırıldığını aynı Log Analytics çalışma alanları ile yapılandırılmış.
* Ağ geçidi yapan bir proxy sunucusuna bağlanabilir veya Internet bağlantısı olması gerekir.

> [!NOTE]
> Ağ geçidi için bir değer belirtmezseniz, boş değerler için tüm aracılar itilir.
> 

Yönetim grubu için proxy yapılandırmasını belirtme seçeneği, Operations Manager yönetim grubunuzun bir Log Analytics çalışma alanıyla kaydediyor ilk kez kullanıyorsanız, işletim konsolunda kullanılabilir değil.  Bu seçeneğin sağlanması için önce yönetim grubunun hizmete başarıyla kaydedilmiş olması gerekir.  Sistemde Netsh kullanarak sistem proxy yapılandırması, tümleştirme ve tüm yönetim sunucuları, yönetim grubunda yapılandırmak için işletim konsolunda çalışan güncelleştirin.  

1. Yükseltilmiş bir komut istemi açın.

    a. Git **Başlat** ve türü **cmd**.  
    b. Sağ **komut istemi** seçip **yönetici olarak çalıştır**.  

1. Aşağıdaki komutu girin ve **Enter** tuşuna basın:

    `netsh winhttp set proxy <proxy>:<port>`

Log Analytics ile tümleştirmesini tamamladıktan sonra değişikliğin çalıştırarak kaldırabilirsiniz `netsh winhttp reset proxy` ve ardından **proxy sunucusunu yapılandır** Log Analytics Ağ Geçidi sunucusunu belirtmek için işletim konsolunda seçeneği. 

1. Operations Manager konsolunu açın ve altında **Operations Management Suite**, tıklayın **bağlantı** ve ardından **Ara sunucuyu yapılandır**.<br><br> ![Operations Manager – Ara sunucuyu yapılandırma](./media/gateway/scom01.png)<br> 
1. Seçin **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak** ve Log Analytics ağ geçidi sunucusu IP adresini veya NLB sanal IP adresini yazın. İle başlatıldığından emin olmak `http://` önek.<br><br> ![Operations Manager – proxy sunucu adresi](./media/gateway/scom02.png)<br> 
1. **Son**'a tıklayın. Operations Manager yönetim grubunuzu Log Analytics hizmetinin ağ geçidi sunucusu üzerinden iletişim kurması artık yapılandırılmıştır.

### <a name="configure-operations-manager---specific-agents-use-proxy-server"></a>Operations Manager yapılandırma - belirli aracıları proxy sunucusu kullan
Büyük veya karmaşık ortamları için Log Analytics ağ geçidi sunucusu kullanmak için belirli sunucuları (veya gruplar) yalnızca isteyebilirsiniz.  Bu sunucular için Operations Manager Aracısı güncelleştirilemiyor. yönetim grubu için genel değer tarafından bu değer üzerine doğrudan.  Bunun yerine, bu değerleri göndermek için kullanılan kuralı geçersiz kıl gerekir.  

> [!NOTE] 
> Aynı yapılandırma tekniği, ortamınızdaki birden fazla Log Analytics Ağ Geçidi sunucularının kullanılmasını sağlamak için kullanılabilir.  Örneğin, belirli Log Analytics Ağ Geçidi sunucuları, bölge başına temelinde belirtilmesi gerektirebilir.
>  

1. Operations Manager konsolunu açın ve seçin **yazma** çalışma.  
1. Yazma çalışma alanında **kuralları** tıklatıp **kapsam** Operations Manager araç çubuğunda. Bu düğme kullanılamıyorsa, izleme bölmesinde bir nesne seçili bir klasör değil emin olmak için kontrol edin. **Kapsam Yönetim Paketi nesneleri** iletişim kutusunda ortak hedeflenen sınıfları, grupları veya nesneleri listesini görüntüler. 
1. Tür **sistem sağlığı hizmeti** içinde **Ara** alan ve listeden seçin.  **Tamam** düğmesine tıklayın.  
1. Kuralını arayın **Advisor Proxy ayarı kural** ve Operations konsolu araç çubuğunda **geçersiz kılmalar** gelin ve ardından **Rule\For sınıfın belirli bir nesnesi geçersiz kıl: Sistem sağlığı hizmeti** ve belirli bir nesneyi listeden seçin.  İsteğe bağlı olarak, bu geçersiz kılma uygulayın ve ardından bu grup için geçersiz kılmanın istediğiniz sunucuları sistem durumu hizmeti nesnesinin içeren özel bir grup oluşturabilirsiniz.
1. İçinde **geçersiz kılma özellikleri** iletişim kutusu, bir onay işareti koyun **geçersiz kılma** yanındaki sütuna **WebProxyAddress** parametresi.  İçinde **geçersiz kılma değeri** ile başlayan Log Analytics ağ geçidi sunucusu sağlayarak URL'sini girin `http://` önek.  

    >[!NOTE]
    > Bunu zaten otomatik olarak Microsoft System Center Advisor izleme sunucusu grubu hedefleme Microsoft System Center Advisor Güvenli başvuru geçersiz kılma Yönetim Paketi içinde yer alan bir geçersiz kılma ile yönetiliyor olarak Kuralı etkinleştirmek gerekmez.
    >   

1. Bir yönetim paketi seçin ya da **hedef Yönetim paketini seçin** tıklayarak yeni bir korumasız Yönetim Paketi oluşturun ya da liste **yeni**. 
1. Değişikliklerinizi tamamladığınızda tıklayın **Tamam**. 

### <a name="configure-for-automation-hybrid-workers"></a>Otomasyon karma çalışanı için yapılandırma
Aşağıdaki adımlar, ortamınızda Otomasyon karma Runbook çalışanları varsa, bunları desteklemek için ağ geçidini yapılandırmak için el ile geçici geçici çözümler sağlar.

Aşağıdaki adımlarda, Otomasyon hesabının bulunduğu Azure bölgesine bilmeniz gerekir. Konumu bulmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure Otomasyonu hizmetini seçin.
1. Uygun Azure Otomasyon hesabı seçin.
1. Alt bölgenin altında görüntülemek **konumu**.<br><br> ![Azure portal-Otomasyon hesabı konumu](./media/gateway/location.png)  

Her konum URL'sini belirlemek için aşağıdaki tabloları kullanın:

**Proje çalışma zamanı veri hizmeti URL'leri**

| **konum** | **URL** |
| --- | --- |
| Orta Kuzey ABD |ncus-jobruntimedata-prod-su1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net |
| Kanada Orta |cc-jobruntimedata-prod-su1.azure-automation.net |
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
| Doğu ABD 2 |eus2-agentservice-prod-1.azure-automation.net |
| Kanada Orta |cc-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-agentservice-prod-1.azure-automation.net |
| Orta Hindistan |cid-agentservice-prod-1.azure-automation.net |
| Japonya |jpe-agentservice-prod-1.azure-automation.net |
| Avustralya |ase-agentservice-prod-1.azure-automation.net |

Bilgisayarınızı bir karma Runbook çalışanı olarak otomatik olarak güncelleştirme yönetimi çözümünü kullanarak düzeltme eki uygulama için kayıtlı değilse, aşağıdaki adımları izleyin:

1. Proje çalışma zamanı veri hizmeti URL'leri, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Log Analytics ağ geçidi hizmeti, aşağıdaki PowerShell cmdlet'ini kullanarak yeniden başlatın: `Restart-Service OMSGatewayService`

Bilgisayarınız karma Runbook çalışanı kayıt cmdlet'ini kullanarak Azure Otomasyonu'na ekleme eklendiğinden, şu adımları izleyin:

1. Aracı hizmeti kayıt URL'si, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. Proje çalışma zamanı veri hizmeti URL'leri, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Log Analytics ağ geçidi hizmetini yeniden başlatın.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Yararlı PowerShell cmdlet'leri
Cmdlet'leri Log Analytics ağ yapılandırma ayarlarını güncelleştirmek için gerekli görevleri tamamlamanıza yardımcı olabilir. Kullanmadan önce emin olun:

1. Log Analytics ağ geçidi (MSI) yükleyin.
1. Bir PowerShell konsol penceresi açın.
1. Modülü içeri aktarmak için aşağıdaki komutu yazın: `Import-Module OMSGateway`
1. Önceki adımda herhangi bir hata oluştu, modülü başarıyla içeri aktarıldı ve cmdlet'leri kullanılabilir. Türü `Get-Module OMSGateway`
1. Cmdlet'lerini kullanarak değişiklikleri yaptıktan sonra ağ geçidi hizmetini yeniden başlatmayacağından emin olun.

3. adımda bir hata alırsanız, modülü içeri aktarılamadı. PowerShell modülü bulamıyor olduğunda hata oluşabilir. Ağ geçidi yükleme yolunda bulabilirsiniz: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet'i** | **Parametreler** | **Açıklama** | **Örnek** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Anahtar |Hizmet yapılandırmasını alır |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Anahtarı (gerekli) <br> Değer |Hizmetin yapılandırma değişiklikleri |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Geçiş (Yukarı Akış) proxy adresini alır |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Kullanıcı adı<br> Parola |Geçiş (Yukarı Akış) Ara sunucu adresi (ve kimlik bilgisi) ayarlar |1. Bir geçiş Ara sunucu ve kimlik bilgilerini ayarlayın:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Kimlik doğrulaması gerekli olmayan bir geçiş proxy ayarlayın: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Geçiş proxy ayarını temizleyin:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |(Yalnızca yerel olarak yapılandırılmış izin verilen bir ana, izin verilen otomatik olarak indirilen konakları içermez) şu anda izin verilen ana bilgisayar alır |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |İzin verilenler konağa ekler |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayar izin verilen listesinden kaldırır. |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası izin verilenler tabi ekler |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası konu izin verilen listesinden kaldırır. |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |Şu anda izin verilen istemci sertifika konuları alır (yerel olarak yapılandırılmış konuları izin yalnızca izin verilen otomatik olarak indirilen konuları içermez) |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Sorun giderme
Ağ Geçidi tarafından günlüğe kaydedilen olayları toplamak için de Log Analytics aracısı yüklü olması gerekir.<br><br> ![Olay Görüntüleyicisi-Log Analytics ağ geçidi günlüğü](./media/gateway/event-viewer.png)

**Log Analytics ağ geçidi olay kimlikleri ve açıklamaları**

Aşağıdaki tabloda olay kimlikleri ve açıklamaları için Log Analytics ağ geçidi günlüğü olaylarını gösterir.

| **ID** | **Açıklama** |
| --- | --- |
| 400 |Belirli bir kimliği olmayan herhangi bir uygulama hatası |
| 401 |Yanlış yapılandırma. Örneğin: listenPort = "text" tamsayı değil |
| 402 |TLS el sıkışma iletilerini ayrıştırma özel durumu |
| 403 |Ağ hatası oluştu. Örneğin: hedef sunucuya bağlanılamıyor |
| 100 |Genel bilgiler |
| 101 |Hizmet başlatıldı |
| 102 |Hizmeti durdu |
| 103 |İstemciden bir HTTP bağlantısı komutu aldı |
| 104 |Olmayan bir HTTP bağlantısı komutu |
| 105 |Hedef sunucu izin verilenler listesinde değil veya hedef bağlantı noktası güvenli bağlantı noktası (443) değil <br> <br> Ağ geçidi sunucunuzda MMA aracısını ve ağ geçidi ile iletişim kuran aracıları bağlandığını aynı Log Analytics çalışma alanına emin olun. |
| 105 |HATA TcpConnection – geçersiz istemci sertifikası: CN=Gateway <br><br> Emin olun: <br>    <br> &#149;Bir ağ geçidi sürüm numarası 1.0.395.0 ile kullandığınız ya da daha büyük. <br> &#149;Ağ geçidi sunucunuzda MMA aracısını ve ağ geçidi ile iletişim kuran aracıları aynı Log Analytics çalışma alanına bağlı. |
| 106 |Log Analytics ağ geçidi, yalnızca TLS 1.0, TLS 1.1 ve 1.2 destekler.  SSL desteklemez. Bir TLS/SSL protokolü için desteklenmeyen sürümü, olay kimliği 106 Log Analytics ağ geçidi oluşturur.|
| 107 |TLS oturum doğrulandı |

**Toplanacak performans sayaçları**

Aşağıdaki tablo Log Analytics ağ geçidi için kullanılabilen performans sayaçlarının gösterir. Performans İzleyicisi'ni kullanarak sayaçları ekleyebilirsiniz.

| **Ad** | **Açıklama** |
| --- | --- |
| Log Analytics Gateway/etkin istemci bağlantısı |Etkin istemci (TCP) ağ bağlantısı sayısı |
| Log Analytics Gateway/hata sayısı |Hata sayısı |
| Ağ geçidi ve bağlı log Analytics istemcisi |Bağlı istemci sayısı |
| Log Analytics Gateway/reddetme sayısı |Herhangi bir TLS doğrulama hatası nedeniyle reddi sayısı |

![Günlük analizi gateway performans sayaçları](./media/gateway/counters.png)

## <a name="get-assistance"></a>Yardım alın
Azure portalında oturum açtığınızda, Log Analytics ağ geçidi veya herhangi başka Azure hizmeti veya özelliği bir hizmetin ile Yardım isteği oluşturabilirsiniz.
Yardım isteğinde, portalın sağ üst köşedeki soru işareti simgesine tıklayın ve ardından **yeni destek isteği**. Ardından, yeni destek isteği formu doldurun.

![Yeni destek isteği](./media/gateway/support.png)

## <a name="next-steps"></a>Sonraki adımlar
[Veri kaynağı ekleme](../../azure-monitor/platform/agent-data-sources.md) bağlı kaynaklardan gelen verileri toplamak ve Log Analytics çalışma alanınızda depolamak için.
