---
title: Bilgisayarları Log Analytics ağ geçidini kullanarak bağlan | Microsoft Docs
description: Operations Manager tarafından izlenen bilgisayarların ve cihazların internet erişimi olmadığında Azure otomasyon ve Log Analytics hizmeti için veri göndermek için Log Analytics ağ geçidi kullanarak bağlanın.
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
ms.date: 03/04/2019
ms.author: magoedte
ms.openlocfilehash: 81005c2c95c9cccb32796d1afca4208f5ff8b919
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58437348"
---
# <a name="connect-computers-without-internet-access-by-using-the-log-analytics-gateway-in-azure-monitor"></a>İnternet erişimi olmayan bilgisayarları Azure İzleyici'de Log Analytics ağ geçidini kullanarak bağlan

>[!NOTE]
>Microsoft Operations Management Suite (OMS) için Microsoft Azure İzleyici geçişleri gibi terminolojisi değişiyor. Bu makalede, Azure Log Analytics ağ geçidi olarak OMS ağ geçidi için ifade eder. 
>

Bu makalede iletişim Azure Otomasyonu ve Azure İzleyici ile doğrudan bağlı veya Operations Manager tarafından izlenen bilgisayarların internet erişimi varsa, Log Analytics ağ geçidi kullanarak nasıl yapılandırılacağını açıklar. 

Log Analytics, HTTP HTTP CONNECT komutunu kullanarak tüneli destekleyen bir HTTP iletim proxy'si geçididir. Bu ağ geçidi, veri toplamak ve Azure Otomasyonu ve Azure İzleyici'de Log Analytics çalışma alanı adına internet'e bağlı olmayan bilgisayarlar için Gönder.  

Log Analytics gateway destekler:

* Aynı dört Log Analytics kadar arkasında olan ve Azure Otomasyon karma Runbook çalışanlarıyla birlikte yapılandırılmış çalışma alanı aracılar raporlama.  
* Windows bilgisayarlar üzerinde Microsoft Monitoring Agent doğrudan Azure İzleyici'de bir Log Analytics çalışma alanına bağlı.
* Linux bilgisayarlar üzerinde bir Linux için Log Analytics aracısını doğrudan Azure İzleyici'de bir Log Analytics çalışma alanına bağlı.  
* Log Analytics ile tümleşik system Center Operations Manager 2012 SP1 UR7, Operations Manager 2012 R2 UR3 veya Operations Manager 2016 veya sonraki bir yönetim grubu ile.  

Bazı BT güvenlik ilkeleri, internet bağlantısı ağ bilgisayarlar için izin verme. Bağlantısız bu bilgisayarların noktası satışı (POS) cihazları veya örneğin BT Hizmetleri destekleyen sunucular olabilir. Azure Otomasyonu veya yönetebileceğiniz için bir Log Analytics çalışma alanı ve İzleyici bu cihazlar bağlanmak için yapılandırma bunları doğrudan Log Analytics ağ geçidi ile iletişim kurmak için. Log Analytics ağ geçidi yapılandırma bilgilerini ve gerçekleştirilemeyeceğine ilişkin verileri alabilir. Bilgisayarları doğrudan Log Analytics çalışma alanına bağlamak için Log Analytics aracısını ile yapılandırılmışsa, bilgisayarları bunun yerine Log Analytics ağ geçidi ile iletişim kurar.  

Log Analytics gateway verileri aracılardan hizmete doğrudan aktarır. Aktarımdaki verileri analiz etmez.

Yapılandırma bilgilerini almak ve etkinleştirdiğiniz çözümüne bağlı olarak, toplanan verileri göndermek için Log Analytics ağ geçidine bağlanmak için bir Operations Manager yönetim grubunu Log Analytics ile tümleştirildiğinde, yönetim sunucuları yapılandırılabilir .  Operations Manager aracıları bazı verilerini yönetim sunucusuna gönderir. Örneğin, aracıları Operations Manager uyarıları, yapılandırma değerlendirmesi verileri, örnek alanı verileri ve kapasite veri gönderebilir. Internet Information Services (IIS) günlükleri, performans verilerini ve güvenlik olaylarını gibi diğer yüksek hacimli verileri doğrudan Log Analytics ağ geçidine gönderilir. 

Güvenilmeyen sistemlerini bir çevre ağında veya yalıtılmış ağ izlemek için bir veya daha fazla Operations Manager ağ geçidi sunucusu dağıtılırsa, bu sunucular bir Log Analytics ağ geçidi ile iletişim kuramıyor.  Operations Manager Ağ Geçidi sunucuları, yalnızca bir yönetim sunucusuna rapor edebilirsiniz.  Bir Operations Manager yönetim grubunu Log Analytics ağ geçidi ile iletişim kurmak için yapılandırıldığında proxy yapılandırma bilgileri için Azure İzleyici günlük verilerini toplamak için yapılandırılmış her aracıyla yönetilen bilgisayar otomatik olarak dağıtılır, olsa bile boş bir ayardır.    

Doğrudan için yüksek kullanılabilirlik sağlamak için bağlı veya bir ağ geçidi üzerinden Log Analytics çalışma alanıyla iletişim Operations Yönetim grupları, Ağ Yükü Dengeleme birden fazla ağ geçidi sunucusu arasında yönlendirme ve trafik dağıtmak için (NLB) kullanabilirsiniz. Bu şekilde, bir ağ geçidi sunucusu kalırsa, trafiği kullanılabilir başka bir düğüme yönlendirilir.  

Log Analytics ağ geçidi'ni çalıştıran bilgisayar, ağ geçidi ile iletişim kurması gereken hizmet uç noktalarını tanımlamak Log Analytics Windows Aracısı gerekir. Aracıyı da aynı çalışma alanına raporlama yapacak ağ geçidi doğrudan gerekir, aracılar veya Operations Manager yönetim grubu, ağ geçidinin arkasındaki ile yapılandırılır. Bu yapılandırma, ağ geçidi ve bunların atanmış bir çalışma alanıyla iletişim kurabilmesi için sağlar.

Bir ağ geçidi en fazla dört çalışma alanları için ana bilgisayara bağlı olabilir. Bu çalışma alanlarının bir Windows aracısının desteklediği toplam sayısıdır.  

Her bir aracı, aracıları otomatik olarak ağ geçidi veri aktarabilmesi ağ geçidi için ağ bağlantısı olması gerekir. Ağ geçidini bir etki alanı denetleyicisinde yüklemekten kaçının.

Aşağıdaki diyagramda Azure otomasyon ve Log Analytics için ağ geçidi üzerinden doğrudan aracılardan akan verileri gösterir. Aracı proxy yapılandırması, Log Analytics ağ geçidi ile yapılandırılan bağlantı noktası eşleşmesi gerekir.  

![Doğrudan aracı iletişimi hizmetleriyle diyagramı](./media/gateway/oms-omsgateway-agentdirectconnect.png)

Aşağıdaki diyagramda, bir Operations Manager yönetim grubundan Log Analytics'e veri akışı gösterilmektedir.   

![Log Analytics ile Operations Manager iletişim diyagramı](./media/gateway/log-analytics-agent-opsmgrconnect.png)

## <a name="set-up-your-system"></a>Sisteminizi ayarladıktan

Log Analytics ağ geçidini çalıştırmak için atanan bilgisayarlarda aşağıdaki yapılandırmaya sahip olmanız gerekir:

* Windows 10, Windows 8.1 veya Windows 7
* Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 veya Windows Server 2008
* Microsoft .NET Framework 4.5
* En az bir 4 çekirdekli işlemci ve 8 GB bellek 
* A [Windows için Log Analytics aracısını](agent-windows.md) ağ geçidi üzerinden iletişim kuran aracıları aynı çalışma alanına rapor için yapılandırılmış

### <a name="language-availability"></a>Dil kullanılabilirlik

Log Analytics ağ geçidi, şu dillerde kullanılabilir:

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
Log Analytics ağ geçidi yalnızca Aktarım Katmanı Güvenliği (TLS) 1.0, 1.1 ve 1.2 destekler.  Bunu yapmak için Güvenli Yuva Katmanı (SSL) desteklemiyor.  Log analytics'e Aktarımdaki verilerin güvenliğini sağlamak için en az ağ geçidini yapılandırma TLS 1.2. TLS veya SSL eski sürümlerini savunmasızdır. Bunlar şu anda geriye dönük uyumluluk izin verse de, bunları kullanmaktan kaçının.  

Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

### <a name="supported-number-of-agent-connections"></a>Desteklenen aracı bağlantı sayısı
Aşağıdaki tablo, kaç aracının bir ağ geçidi sunucusu ile iletişim kurup yaklaşık gösterir. Destek 6 saniyede yaklaşık 200 KB'lık veri karşıya aracılarda temel alır. Test edilen her aracı için veri hacmi hakkında 2.7 günde GB'dir.

|Ağ geçidi |(Yaklaşık) desteklenen aracıları|  
|--------|----------------------------------|  
|CPU: Intel Xeon İşlemci E5 2660 v3 \@ 2,6 GHz 2 Çekirdek<br> Bellek: 4 GB<br> Ağ bant genişliği: 1 Gbps| 600|  
|CPU: Intel Xeon İşlemci E5 2660 v3 \@ 2,6 GHz 4 çekirdek<br> Bellek: 8 GB<br> Ağ bant genişliği: 1 Gbps| 1000|  

## <a name="download-the-log-analytics-gateway"></a>Log Analytics ağ geçidini indirin

Log Analytics gateway Kurulum dosyasının en son sürümü'nden ya da Al [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=54443) veya Azure portalında.

Azure portalında Log Analytics gateway almak için şu adımları izleyin:

1. Hizmetler listesine göz atın ve ardından **Log Analytics**. 
1. Bir çalışma alanı seçin.
1. Çalışma alanı dikey penceresinde altında **genel**seçin **Hızlı Başlangıç**. 
1. Altında **çalışma alanına bağlamak için bir veri kaynağı seçin**seçin **bilgisayarlar**.
1. İçinde **doğrudan aracı** dikey penceresinde **indirme Log Analytics gateway**.
 
   ![Log Analytics ağ geçidini yüklemek için adımları ekran görüntüsü](./media/gateway/download-gateway.png)

or 

1. Çalışma alanı dikey penceresinde altında **ayarları**seçin **Gelişmiş ayarlar**.
1. Git **bağlı kaynaklar** > **Windows sunucuları** seçip **indirme Log Analytics gateway**.

## <a name="install-the-log-analytics-gateway"></a>Log Analytics ağ geçidi yükleme

Bir ağ geçidi yüklemek için aşağıdaki adımları izleyin.  (Log Analytics ileticisi adlı önceki bir sürümü yüklü değilse, bunu bu sürümüne yükseltilir.)

1. Hedef klasördeki çift **Log Analytics gateway.msi**.
1. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.

   ![Ağ geçidi Kurulum sihirbazında, ekran Karşılama sayfası](./media/gateway/gateway-wizard01.png)

1. Üzerinde **Lisans Sözleşmesi** sayfasında **lisans sözleşmesinin koşullarını kabul ediyorum** Microsoft Yazılımı Lisans koşullarını kabul edin ve sonra seçin **sonraki**.
1. Üzerinde **bağlantı noktası ve proxy adresi** sayfası:

   a. Ağ geçidi için kullanılan TCP bağlantı noktası numarasını girin. Kurulum, Windows Güvenlik Duvarı'nı bir gelen kuralı yapılandırmak için bu bağlantı noktası numarası kullanır.  Varsayılan değer 8080'dir.
      Geçerli bağlantı noktası numarası 1 ile 65535 aralığı. Giriş bu aralığı içinde kalmıyorsa, bir hata iletisi görüntülenir.

   b. Ağ geçidinin yüklü olduğu sunucunun bir proxy üzerinden iletişim kurması gerekiyorsa, ağ geçidine bağlanmak gereken yere proxy adresini girin. Örneğin, `http://myorgname.corp.contoso.com:80` girin.  Bu alanı boş bırakırsanız, ağ geçidi doğrudan internet'e bağlanmaya çalışacaktır.  Ara sunucunuz kimlik doğrulaması gerektiriyorsa, kullanıcı adı ve parola girin.

   c. **İleri**’yi seçin.

   ![Ağ Geçidi Proxy yapılandırmasının ekran görüntüsü](./media/gateway/gateway-wizard02.png)

1. Yoksa, Microsoft Update etkinleştirilmiş, Microsoft Update sayfasında görünür ve etkinleştirmeyi seçebilirsiniz. Bir seçim yapın ve ardından **sonraki**. Aksi halde, sonraki adıma devam edin.
1. Üzerinde **hedef klasör** sayfasında varsayılan klasörü C:\Program Files\OMS ağ geçidi bırakın ya da ağ geçidi yüklemek istediğiniz konumu girin. Sonra **İleri**’yi seçin.
1. Üzerinde **yüklenmeye hazır** sayfasında **yükleme**. Kullanıcı hesabı denetimi yükleme izni isterse seçin **Evet**.
1. Kurulum bittikten sonra seçin **son**. Hizmetinin çalıştığını doğrulamak için services.msc ek bileşenini açın ve doğrulayın **OMS ağ geçidi** Hizmetler listesinde görünür ve durumu olan **çalıştıran**.

   ![OMS ağ geçidi çalıştığını gösteren ekran görüntüsü yerel Hizmetleri](./media/gateway/gateway-service.png)


## <a name="configure-network-load-balancing"></a>Ağ Yükü Dengeleme yapılandırma 
Ağ geçidi Ağ Yükü Dengeleme (NLB kullanarak ya da Microsoft) kullanarak yüksek kullanılabilirlik için yapılandırabileceğiniz [Ağ Yükü Dengeleme (NLB)](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing), [Azure Load Balancer](../../load-balancer/load-balancer-overview.md), veya donanım tabanlı yük Dengeleyiciler. Yük dengeleyicinin trafiği istenen bağlantılar Log Analytics aracılardan veya Operations Manager yönetim sunucuları arasında düğümlerini yönlendirerek yönetir. Bir ağ geçidi sunucusu kalırsa, trafiğin diğer düğümlere yönlendirilir.

### <a name="microsoft-network-load-balancing"></a>Microsoft Ağ Yükü Dengeleme
Tasarım ve bir Windows Server 2016 Ağ Yükü Dengeleme kümesi dağıtma hakkında bilgi edinmek için [Ağ Yükü Dengeleme](https://docs.microsoft.com/windows-server/networking/technologies/network-load-balancing). Aşağıdaki adımlar, Microsoft Ağ Yükü Dengeleme kümesini yapılandırın açıklanmaktadır.  

1. Bir yönetim hesabıyla NLB kümesinin bir üyesi olan Windows sunucuya oturum açın.  
2. Sunucu Yöneticisi'nde Ağ Yükü Dengeleme Yöneticisi'ni açın, **Araçları**ve ardından **Ağ Yükü Dengeleme Yöneticisi**.
3. Microsoft izleme aracısının yüklü olduğu bir Log Analytics Ağ Geçidi sunucusuna bağlanmak için kümenin IP adresine sağ tıklayın ve ardından **konak kümesine Ekle**. 

    ![Ağ Yükü Dengeleme Yöneticisi – kümeye konak Ekle](./media/gateway/nlb02.png)
 
4. Bağlamak istediğiniz ağ geçidi sunucusu IP adresini girin. 

    ![Ağ Yükü Dengeleme Yöneticisi – konak kümesine ekleyin: Bağlan](./media/gateway/nlb03.png) 

### <a name="azure-load-balancer"></a>Azure Load Balancer
Tasarım ve bir Azure yük dengeleyici dağıtma hakkında bilgi edinmek için [Azure Load Balancer nedir?](../../load-balancer/load-balancer-overview.md). Temel yük dengeleyici dağıtmak için bu konuda özetlenen adımları izleyin. [hızlı](../../load-balancer/quickstart-create-basic-load-balancer-portal.md) bölümünde özetlenen adımları hariç **arka uç sunucular oluşturma**.   

> [!NOTE]
> Kullanarak Azure Load Balancer yapılandırma **temel SKU**, Azure sanal makineleri bir kullanılabilirlik kümesine ait olmasını gerektirir. Kullanılabilirlik kümeleri hakkında daha fazla bilgi için bkz. [azure'daki Windows sanal makinelerin kullanılabilirliğini yönetme](../../virtual-machines/windows/manage-availability.md). Mevcut sanal makinelerin bir kullanılabilirlik kümesine eklenecek başvurmak [Azure Resource Manager VM kullanılabilirlik Set](https://gallery.technet.microsoft.com/Set-Azure-Resource-Manager-f7509ec4).
> 

Yük Dengeleyiciyi oluşturduktan sonra bir arka uç havuzu oluşturulması için bir veya daha fazla ağ geçidi sunucuları trafiği dağıtır gerekir. Hızlı Başlangıç makale bölümünde açıklanan adımları [yük dengeleyici kaynakları oluşturma](../../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-resources-for-the-load-balancer).  

>[!NOTE]
>Durum yoklaması yapılandırırken ağ geçidi sunucusu TCP bağlantı noktasını kullanacak şekilde yapılandırılmalıdır. Durum yoklaması, dinamik olarak ekler veya ağ geçidi sunucularının sistem durumu denetimleri verdikleri yanıtlara göre yük dengeleyici rotasyonuna kaldırır. 
>

## <a name="configure-the-log-analytics-agent-and-operations-manager-management-group"></a>Log Analytics aracısını ve Operations Manager yönetim grubu yapılandırma
Bu bölümde, Azure Otomasyonu veya Log Analytics ile iletişim kurmak için Log Analytics ağ geçidi ile doğrudan bağlı Log Analytics aracılarını, bir Operations Manager yönetim grubu veya Azure Otomasyon karma Runbook çalışanlarını yapılandırma görürsünüz.  

### <a name="configure-a-standalone-log-analytics-agent"></a>Tek başına Log Analytics aracısını yapılandırma
Log Analytics aracısını yapılandırırken bir proxy sunucusu değeri Log Analytics ağ geçidi sunucusu ve bağlantı noktası numarası, IP adresi ile değiştirin. Bir yük dengeleyicinin arkasına birden çok ağ geçidi sunucusu dağıttıysanız, Log Analytics aracı proxy yapılandırması yük dengeleyicinin sanal IP adresi ' dir.  

>[!NOTE]
>Ağ geçidi ve Windows bilgisayarları doğrudan Log Analytics'e bağlama Log Analytics aracısını yüklemek için bkz [Azure Log Analytics hizmetine bağlama Windows bilgisayarlara](agent-windows.md). Linux bilgisayarları bağlamak için bkz: [karma bir ortamda, Linux bilgisayarlar için bir Log Analytics Aracısını Yapılandırma](../../azure-monitor/learn/quick-collect-linux-computer.md). 
>

Ağ Geçidi sunucusunda aracıyı yükledikten sonra çalışma alanına veya ağ geçidi ile iletişim kuran aracıları çalışma alanı bildirmek için yapılandırın. Log Analytics Windows Aracısı ağ geçidinde yüklü değilse, olay 300 aracının yüklenmesi gerektiğini belirten OMS ağ geçidi olay günlüğe yazılır. Aracı yüklü ancak üzerinden iletişim kuran aracıları aynı çalışma alanına rapor için yapılandırılmamış, olay 105 aynı günlüğüne, ağ geçidi aracıda aracıları aynı çalışma alanına rapor yapılandırılması gerektiğini belirten, ortak yazılır ağ geçidi ile mmunicate.

Yapılandırmasını tamamladıktan sonra değişiklikleri uygulamak için OMS ağ geçidi hizmetini yeniden başlatın. Aksi takdirde, ağ geçidi olay 105 OMS ağ geçidi olay günlüğünde bildirir ve Log Analytics ile iletişim kurmayı denemeye aracılar reddeder. Bu, ayrıca eklediğinizde veya bir çalışma alanı, aracı yapılandırması Ağ Geçidi sunucusunda kaldırma gerçekleşir.   

Otomasyon karma Runbook çalışanı için ilgili daha fazla bilgi için bkz: [veri merkezinde veya bulutta kaynaklarında karma Runbook çalışanı kullanarak otomatikleştirmek](../../automation/automation-hybrid-runbook-worker.md).

### <a name="configure-operations-manager-where-all-agents-use-the-same-proxy-server"></a>Burada tüm aracıları aynı proxy sunucusunu kullanmak, Operations Manager'ı yapılandırma
Ayarı boş olsa bile, Operations Manager proxy yapılandırması Operations Manager için rapor veren tüm aracıların otomatik olarak uygulanır.  

Operations Manager'ı desteklemek için OMS ağ geçidi kullanmak için şunlara sahip olmalısınız:

* Microsoft Monitoring Agent (8.0.10900.0 sürüm veya üstü) yüklü OMS Ağ Geçidi sunucusunda ve yönetim grubunuzun raporlama yapacak şekilde yapılandırıldığını aynı Log Analytics çalışma alanları ile yapılandırılmış.
* Internet bağlantısı. Alternatif olarak, OMS ağ geçidi, internet'e bağlı bir proxy sunucusuna bağlanması gerekir.

> [!NOTE]
> Ağ geçidi için bir değer belirtirseniz, boş değerler için tüm aracılar itilir.
>

Operations Manager yönetim grubunuzun bir Log Analytics çalışma alanı ile ilk kez kaydediyor proxy yapılandırması yönetim grubunun işletim konsolunda belirtme seçeneği görmeyeceksiniz. Bu seçenek, yalnızca yönetim grubu ile hizmeti kaydedilmişse kullanılabilir.  

Tümleştirmesini yapılandırmak için Netsh nerede işletim konsolunu çalıştırdığınız sistem üzerinde ve yönetim grubundaki tüm yönetim sunucularında kullanarak sistem proxy yapılandırması'nı güncelleştirin. Şu adımları uygulayın:

1. Yükseltilmiş bir komut istemi açın:

   a. Seçin **Başlat** girin **cmd**.  

   b. Sağ **komut istemi** seçip **yönetici olarak çalıştır**.  

1. Aşağıdaki komutu girin:

   `netsh winhttp set proxy <proxy>:<port>`

Log Analytics ile tümleştirmesini tamamladıktan sonra değişiklik çalıştırarak kaldırın. `netsh winhttp reset proxy`. Ardından Operations konsolunda kullanın **proxy sunucusunu yapılandır** Log Analytics Ağ Geçidi sunucusunu belirtmek için seçeneği. 

1. Operations Manager konsolundaki altında **Operations Management Suite**seçin **bağlantı**ve ardından **Ara sunucuyu yapılandır**.

   ![Ekran Operations Proxy sunucusunu yapılandır seçimi gösteren Manager'ın,](./media/gateway/scom01.png)

1. Seçin **Operations Management Suite erişimi için bir proxy sunucusunu kullanmak** ve Log Analytics ağ geçidi sunucusu IP adresini veya yük dengeleyicinin sanal IP adresi girin. Ön ekine sahip özen `http://`.

   ![Ekran Operations proxy sunucu adresi gösteren Manager'ın,](./media/gateway/scom02.png)

1. **Son**’u seçin. Operations Manager yönetim grubunuzu Log Analytics hizmetinin ağ geçidi sunucusu üzerinden iletişim kurması artık yapılandırılmıştır.

### <a name="configure-operations-manager-where-specific-agents-use-a-proxy-server"></a>Operations Manager, burada belirli aracıları bir proxy sunucusu kullan'ı yapılandırma
Büyük veya karmaşık ortamları için yalnızca belirli sunuculara (veya gruplar) için Log Analytics ağ geçidi sunucusu kullanmak isteyebilirsiniz.  Bu değer, yönetim grubu için genel değer tarafından üzerine yazılır olduğundan bu sunucular için Operations Manager Aracısı, doğrudan güncelleştirilemiyor.  Bunun yerine, bu değerleri göndermek için kullanılan kuralı geçersiz kıl.  

> [!NOTE] 
> Ortamınızda birden fazla Log Analytics Ağ Geçidi sunucuları için izin vermek istiyorsanız bu yapılandırmayı tekniği kullanın.  Örneğin, belirli Log Analytics Ağ Geçidi sunucuları bölgesel olarak belirtilmesini gerektirir.
>

Belirli sunucu veya Log Analytics ağ geçidi sunucusu kullanmak için grupları yapılandırmak için: 

1. Operations Manager konsolunu açın ve seçin **yazma** çalışma.  
1. Yazma çalışma alanında **kuralları**. 
1. Operations Manager araç çubuğunda **kapsam** düğmesi. Bu düğme kullanılamıyorsa, seçtiğiniz bir klasör değil bir nesne emin **izleme** bölmesi. **Kapsam Yönetim Paketi nesneleri** iletişim kutusunda ortak hedeflenen sınıfları, grupları veya nesneleri listesini görüntüler. 
1. İçinde **Ara** alanına **sistem sağlığı hizmeti** ve listeden seçin. **Tamam**’ı seçin.  
1. Arama **Advisor Proxy ayarı kural**. 
1. Operations Manager araç çubuğunda **geçersiz kılmalar** gelin ve ardından **Rule\For sınıfın belirli bir nesnesi geçersiz kıl: Sistem sağlığı hizmeti** ve listeden bir nesne seçin.  Veya sistem durumu hizmeti nesnesinin bu geçersiz kılma için uygulamak istediğiniz sunucuları içeren özel bir grup oluşturun. Ardından, özel bir grup için geçersiz kılma uygulayın.
1. İçinde **geçersiz kılma özellikleri** iletişim kutusunda, bir onay işareti ekleyin **geçersiz kılma** yanındaki sütuna **WebProxyAddress** parametresi.  İçinde **geçersiz kılma değeri** Log Analytics Ağ Geçidi sunucusunun URL'sini girin. Ön ekine sahip özen `http://`.  

    >[!NOTE]
    > Kuralı etkinleştirmek gerek yoktur. Bunu zaten otomatik olarak Microsoft System Center Advisor Güvenli başvuru geçersiz kılma yönetim paketindeki bir geçersiz kılma hedefleyen Microsoft System Center Advisor izleme sunucusu grubu tarafından yönetilen.
    > 

1. Bir yönetim paketinden seçin **hedef Yönetim paketini seçin** listelemek veya seçerek yeni bir korumasız Yönetim Paketi oluşturma **yeni**. 
1. İşiniz bittiğinde **Tamam**'a tıklayın. 

### <a name="configure-for-automation-hybrid-runbook-workers"></a>Otomasyon karma Runbook çalışanları için yapılandırma
Ortamınızda Otomasyon karma Runbook çalışanları varsa, çalışanlar desteklemek için OMS ağ geçidi yapılandırmak el ile geçici çözümleri için bu adımları izleyin.

Bu bölümdeki adımları için Otomasyon hesabının bulunduğu Azure bölgesine bilmeniz gerekir. Bu konum bulmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
1. Azure Otomasyonu hizmetini seçin.
1. Uygun Azure Otomasyon hesabı seçin.
1. Alt bölgenin altında görüntülemek **konumu**.

   ![Azure portalında Otomasyon hesabı konumunun ekran görüntüsü](./media/gateway/location.png)

Her konum URL'sini belirlemek için aşağıdaki tabloları kullanın.

**Proje çalışma zamanı veri hizmeti URL'leri**

| **Konum** | **URL** |
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

| **Konum** | **URL** |
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

Bilgisayarınız otomatik olarak bir karma Runbook çalışanı olarak kayıtlı değilse, düzeltme eki yönetmek için güncelleştirme yönetimi çözümü kullanın. Şu adımları uygulayın:

1. Proje çalışma zamanı veri hizmeti URL'leri, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Log Analytics ağ geçidi hizmeti, aşağıdaki PowerShell cmdlet'ini kullanarak yeniden başlatın: `Restart-Service OMSGatewayService`

Bilgisayarınız, karma Runbook çalışanı kayıt cmdlet'ini kullanarak Azure Otomasyonu'na ekleme katılırsa, şu adımları izleyin:

1. Aracı hizmeti kayıt URL'si, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
1. Proje çalışma zamanı veri hizmeti URL'leri, Log Analytics Gateway konak izin listesine ekleyin. Örneğin, `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
1. Log Analytics ağ geçidi hizmetini yeniden başlatın.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Yararlı PowerShell cmdlet'leri
Log Analytics ağ geçidinin yapılandırma ayarlarını güncelleştirmek için görevleri tamamlamak için cmdlet'lerini kullanabilirsiniz. Cmdlet'lerini kullanmadan önce emin olun:

1. Log Analytics ağ geçidi (Microsoft Windows Yükleyici) yükleyin.
1. Bir PowerShell konsol penceresi açın.
1. Bu komutu yazarak modülü içeri aktarın: `Import-Module OMSGateway`
1. Önceki adımda herhangi bir hata oluştu, modülü başarıyla içeri aktarıldı ve cmdlet'leri kullanılabilir. Girin `Get-Module OMSGateway`
1. Değişiklik yapmak için cmdlet'ler kullandıktan sonra OMS ağ geçidi hizmetini yeniden başlatın.

Adım 3'teki bir hata, modülü içeri aktarılamadı anlamına gelir. PowerShell modülü bulduğunuzda hata oluşabilir. Modül OMS ağ geçidi yükleme yolunda bulabilirsiniz: *C:\Program Files\Microsoft OMS Gateway\PowerShell\OmsGateway*.

| **Cmdlet'i** | **Parametreler** | **Açıklama** | **Örnek** |
| --- | --- | --- | --- |  
| `Get-OMSGatewayConfig` |Anahtar |Hizmet yapılandırmasını alır |`Get-OMSGatewayConfig` |  
| `Set-OMSGatewayConfig` |Anahtarı (gerekli) <br> Değer |Hizmetin yapılandırma değişiklikleri |`Set-OMSGatewayConfig -Name ListenPort -Value 8080` |  
| `Get-OMSGatewayRelayProxy` | |Geçiş (Yukarı Akış) proxy adresini alır |`Get-OMSGatewayRelayProxy` |  
| `Set-OMSGatewayRelayProxy` |Adres<br> Kullanıcı adı<br> Parola |Geçiş (Yukarı Akış) Ara sunucu adresi (ve kimlik bilgisi) ayarlar |1. Bir geçiş Ara sunucu ve kimlik bilgilerini ayarlayın:<br> `Set-OMSGatewayRelayProxy`<br>`-Address http://www.myproxy.com:8080`<br>`-Username user1 -Password 123` <br><br> 2. Kimlik doğrulaması gerekli olmayan bir geçiş proxy ayarlayın: `Set-OMSGatewayRelayProxy`<br> `-Address http://www.myproxy.com:8080` <br><br> 3. Geçiş proxy ayarını temizleyin:<br> `Set-OMSGatewayRelayProxy` <br> `-Address ""` |  
| `Get-OMSGatewayAllowedHost` | |Şu anda izin verilen ana bilgisayar (konakları otomatik olarak indirilen yerel olarak yapılandırılmış bir izin verilen ana izin yalnızca) alır |`Get-OMSGatewayAllowedHost` | 
| `Add-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |İzin verilenler konağa ekler |`Add-OMSGatewayAllowedHost -Host www.test.com` |  
| `Remove-OMSGatewayAllowedHost` |Ana bilgisayar (gerekli) |Ana bilgisayar izin verilen listesinden kaldırır. |`Remove-OMSGatewayAllowedHost`<br> `-Host www.test.com` |  
| `Add-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası izin verilenler tabi ekler |`Add-OMSGatewayAllowed`<br>`ClientCertificate` <br> `-Subject mycert` |  
| `Remove-OMSGatewayAllowedClientCertificate` |Konu (gerekli) |İstemci sertifikası konu izin verilen listesinden kaldırır. |`Remove-OMSGatewayAllowed` <br> `ClientCertificate` <br> `-Subject mycert` |  
| `Get-OMSGatewayAllowedClientCertificate` | |(Otomatik olarak indirilen yerel olarak yapılandırılmış bir izin verilen konuları konuları izin yalnızca) şu anda izin verilen istemci sertifika konuları alır. |`Get-`<br>`OMSGatewayAllowed`<br>`ClientCertificate` |  

## <a name="troubleshooting"></a>Sorun giderme
Ağ Geçidi tarafından günlüğe kaydedilen olayları toplamak için Log Analytics aracısı yüklü olmalıdır.

![Log Analytics gateway günlüğündeki Olay Görüntüleyicisi'ni listesinin ekran görüntüsü](./media/gateway/event-viewer.png)

### <a name="log-analytics-gateway-event-ids-and-descriptions"></a>Log Analytics ağ geçidi olay kimlikleri ve açıklamaları

Aşağıdaki tabloda, olay kimlikleri ve açıklamaları için Log Analytics ağ geçidi günlüğü olaylarını gösterir.

| **ID** | **Açıklama** |
| --- | --- |
| 400 |Hiçbir özel bir kimliğe sahip herhangi bir uygulama hatası |
| 401 |Yanlış yapılandırma. Örneğin, listenPort = "text" tamsayı değil. |
| 402 |TLS el sıkışma iletilerini ayrıştırma özel durumu. |
| 403 |Ağ hatası oluştu. Örneğin, hedef sunucuya bağlanamıyor. |
| 100 |Genel bilgiler. |
| 101 |Hizmet başlatıldı. |
| 102 |Hizmeti durduruldu. |
| 103 |Bir HTTP bağlantısı komut istemcisinde alındı. |
| 104 |Olmayan bir HTTP bağlantısı komutu. |
| 105 |Hedef sunucu izin verilenler listesinde değil veya hedef bağlantı noktası (443) güvenli değil. <br> <br> OMS ağ geçidi sunucunuzda MMA aracısını ve OMS ağ geçidi ile iletişim kuran aracıları aynı Log Analytics çalışma alanına bağlı olduğunuzdan emin olun. |
| 105 |HATA TcpConnection – geçersiz istemci sertifikası: CN=Gateway. <br><br> OMS ağ geçidi 1.0.395.0 sürümü kullandığınızdan emin olun veya büyük. Ayrıca, OMS ağ geçidi sunucunuzda MMA aracısını ve OMS ağ geçidi ile iletişim kuran aracıları aynı Log Analytics çalışma alanına bağlı olduğunuzdan emin olun. |
| 106 |TLS/SSL protokolü sürümü desteklenmiyor.<br><br> Log Analytics ağ geçidi, yalnızca TLS 1.0, TLS 1.1 ve 1.2 destekler. SSL desteklemez.|
| 107 |TLS oturum doğrulandı. |

### <a name="performance-counters-to-collect"></a>Toplanacak performans sayaçları

Aşağıdaki tablo Log Analytics ağ geçidi için kullanılabilen performans sayaçlarının gösterir. Performans İzleyicisi sayaçları eklemek için kullanın.

| **Ad** | **Açıklama** |
| --- | --- |
| Log Analytics Gateway/etkin istemci bağlantısı |Etkin istemci (TCP) ağ bağlantısı sayısı |
| Log Analytics Gateway/hata sayısı |Hata sayısı |
| Ağ geçidi ve bağlı log Analytics istemcisi |Bağlı istemci sayısı |
| Log Analytics Gateway/reddetme sayısı |Herhangi bir TLS doğrulama hatası nedeniyle reddi sayısı |

![Performans sayaçları gösteren ekran görüntüsü, Log Analytics Ağ Geçidi Arabirimi](./media/gateway/counters.png)

## <a name="assistance"></a>Yardım
Azure portalında oturum açmadıysanız, Log Analytics ağ geçidi veya başka Azure hizmeti veya özellik ile ilgili Yardım alabilirsiniz.
Yardım almak için portal seçin ve sağ üst köşedeki soru işareti simgesini seçin **yeni destek isteği**. Yeni destek isteği formunu doldurun.

![Yeni bir destek isteği ekran görüntüsü](./media/gateway/support.png)

## <a name="next-steps"></a>Sonraki adımlar
[Veri kaynağı ekleme](../../azure-monitor/platform/agent-data-sources.md) bağlı kaynaklardan veri toplamak ve Log Analytics çalışma alanınızda verileri depolamak için.