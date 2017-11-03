---
title: "Şirket içi veri ağ geçidi - Azure mantıksal uygulamaları yükleme | Microsoft Docs"
description: "Şirket içi veri kaynaklarına erişim önce hızlı veri aktarımı ve şirket içi veri kaynakları ve mantıksal uygulamalar arasında şifreleme için şirket içi veri ağ geçidi yükleyin"
keywords: "Şirket içi veri aktarımı, şifreleme, veri kaynakları veri erişimi"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/14/2017
ms.author: LADocs; millopis; estfan
ms.openlocfilehash: b3c1e2afadea91f010c3e4b43206b6d30a75ec38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="install-the-on-premises-data-gateway-for-azure-logic-apps"></a>Azure mantıksal uygulamaları için şirket içi veri ağ geçidini yükleyin

Mantıksal uygulamalarınızı şirket içi veri kaynaklarına erişebilmesi için yüklemek ve şirket içi veri ağ geçidi kurun. Ağ geçidi hızlı veri aktarımı ve şirket içi sistemleri ve logic apps arasında şifreleme sağlayan köprü gibi davranır. Ağ geçidi şifrelenmiş kanalda Azure Service Bus aracılığıyla şirket içi kaynaklardan veri aktarır. Ağ geçidi aracısından güvenli giden trafik olarak tüm trafiğin kaynaklandığı. Daha fazla bilgi edinmek [veri ağ geçidinin nasıl çalıştığını](#gateway-cloud-service).

Ağ geçidi, şirket içinde bu veri kaynaklarının bağlantılarını destekler:

*   BizTalk Server 2016
*   DB2  
*   Dosya Sistemi
*   Informix
*   MQ
*   MySQL
*   Oracle Veritabanı
*   PostgreSQL
*   SAP uygulama sunucusu 
*   SAP ileti sunucusu
*   SharePoint
*   SQL Server
*   Teradata

İlk önce şirket içi veri ağ geçidi yükleme adımları Göster [ağ geçidi ve logic apps arasında bir bağlantı ayarlayın](./logic-apps-gateway-connection.md). Desteklenen bağlayıcılar hakkında daha fazla bilgi için bkz: [Azure Logic Apps bağlayıcılarının](https://docs.microsoft.com/azure/connectors/apis-list). 

Ağ geçidi diğer hizmetlerle birlikte kullanma hakkında daha fazla bilgi için bu makalelere bakın:

*   [Microsoft Power BI şirket içi veri ağ geçidi](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [Azure Analysis Services veri ağ geçidi şirket içi](../analysis-services/analysis-services-gateway.md)
*   [Microsoft Flow şirket içi veri ağ geçidi](https://flow.microsoft.com/documentation/gateway-manage/)
*   [Microsoft PowerApps şirket içi veri ağ geçidi](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>

## <a name="requirements"></a>Gereksinimler

**Minimum**:

* .NET 4.5 framework
* Windows 7 veya Windows Server 2008 R2 64-bit sürümünü (veya üstü)

**Önerilen**:

* 8 çekirdekli CPU
* 8 GB bellek
* 64 bit sürümü Windows 2012 R2'in (veya üstü)

**İle ilgili önemli noktalar**:

* Şirket içi veri ağ geçidini yalnızca yerel bir bilgisayara yükleyin.
Ağ geçidi etki alanı denetleyicisine yükleyemezsiniz.

   > [!TIP]
   > Ağ geçidi, veri kaynağınız ile aynı bilgisayara yüklemeniz gerekmez. Gecikmeyi en aza indirmek için izinlere sahip olduğunu varsayarsak, veri kaynağına ya da aynı bilgisayarda olası olabildiğince yakın ağ geçidi yükleyebilirsiniz.

* Ağ geçidi devre dışı bırakır, uyku moduna geçer bir bilgisayarda yüklemeyin ya da ağ geçidi bu koşullarda çalıştığından Internet'e değil. Ayrıca, kablosuz ağ üzerinden ağ geçidi performansı düşebilir.

* Yükleme sırasında bilgileriyle oturum açmalıdır bir [iş veya Okul hesabı](https://docs.microsoft.com/azure/active-directory/sign-up-organization) Azure Active Directory tarafından (Azure AD), bir Microsoft hesabı yönetilir.

  > [!TIP]
  > Visual Studio MSDN aboneliğiniz ile olan bir Microsoft hesabı kullanmak istiyorsanız, [Azure Active Directory'de bir dizin (Kiracı) oluşturun](../active-directory/develop/active-directory-howto-tenant.md) Microsoft hesabı veya varsayılan dizini kullanın. Dizine bir parolası olan bir kullanıcı eklemek sonra aboneliğiniz bu kullanıcı erişimi verin. Ardından bu kullanıcı adı ve parola ile ağ geçidi yüklemesi sırasında oturum açabilirsiniz.

  Oluşturduğunuzda ve bir ağ geçidi kaynağı ile ağ geçidi yüklemenizi ilişkilendirmek daha sonra Azure Portalı'nda aynı iş veya Okul hesabı kullanmanız gerekir. Mantıksal uygulamanızı ve şirket içi veri kaynağı bağlantıyı oluşturduğunuzda, ardından bu ağ geçidi kaynağı seçin. [Neden gerekir t bir Azure AD iş veya Okul hesabı?](#why-azure-work-school-account)

  > [!TIP]
  > Bir Office 365 teklif için kaydolan ve gerçek iş e-sağlamadı, oturum açma adresinizi nasıl görünebileceği jeff@contoso.onmicrosoft.com. 

* 14.16.6317.4 sürümden daha eski bir yükleyici ile ayarladığınız mevcut bir ağ geçidi varsa, en son Installer çalıştırarak ağ geçidinizin konumunu değiştiremezsiniz. Ancak, bunun yerine istediğiniz konumuna sahip yeni bir ağ geçidi ayarlamak üzere en son Installer kullanabilirsiniz.
  
  14.16.6317.4 sürümden daha eski bir ağ geçidi yükleyicisi varsa, ancak ağ geçidiniz yüklemediniz henüz indirebilir ve en son yükleyiciyi kullanın.

<a name="install-gateway"></a>

## <a name="install-the-data-gateway"></a>Veri ağ geçidini yükleyin

1.  [Karşıdan yükle ve yerel bir bilgisayarda ağ geçidi çalıştırmak](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).

2. Gözden geçirin ve kullanım ve gizlilik bildirimini koşullarını kabul edin.

3. Yerel bilgisayarınızda ağ geçidi yüklemek istediğiniz yolu belirtin.

4. İstendiğinde, Azure iş veya Okul hesabı, bir Microsoft hesabı ile oturum açın.

   ![Oturum oturum Azure iş veya Okul hesabı](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. Yüklü ağ geçidi ile kaydettirmek [ağ geçidi bulut Hizmeti'ne](#gateway-cloud-service). Seçin **bu bilgisayarda yeni bir ağ geçidini kaydetmek**.

   Ağ geçidi bulut Hizmeti'ne şifreler ve veri kaynağı kimlik bilgilerini ve ağ geçidi ayrıntıları depolar. 
   Hizmeti, şirket içinde sorguları ve sonuçları mantıksal uygulamanızı, şirket içi veri ağ geçidi ve veri kaynağınız arasında da yönlendirir.

6. Ağ geçidi yüklemeniz için bir ad sağlayın. Bir kurtarma anahtarı oluşturun, sonra kurtarma anahtarını doğrulayın. 

   > [!IMPORTANT] 
   > Kurtarma anahtarı en az sekiz karakter içermelidir. Kaydet ve anahtarı güvenli bir yerde sakladığınızdan emin olun. Geçirme, geri yüklemek veya mevcut bir ağ geçidi almak istediğinizde de bu anahtarı gerekir.

   1. Ağ geçidi yüklemeniz tarafından kullanılan Azure Service Bus ve ağ geçidi bulut hizmeti için varsayılan bölgeyi değiştirmek için seçin **değiştirmek bölge**.

      ![Değişiklik bölgesi](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      Varsayılan bölge, Azure AD Kiracı ile ilişkilendirilen bölgedir.

   2. Sonraki bölmesinde, açmak **seçin bölge** farklı bir bölge seçin.

      ![Başka bir bölge seçin](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      Örneğin, mantıksal uygulamanızı aynı bölgede seçin veya gecikme süresini azaltmak için şirket içi veri kaynağına en yakın bölgeyi seçin. Ağ geçidi kaynak ve mantıksal uygulamanızı farklı konumlarda olabilir.

      > [!IMPORTANT]
      > Yükleme sonrasında bu bölgeyi değiştiremezsiniz. Bu bölge ayrıca belirler ve Azure kaynak ağ geçidiniz için oluşturabileceğiniz konumu kısıtlar. Bu nedenle, ağ geçidi kaynağı Azure'da oluşturduğunuzda, kaynak konumu ağ geçidi yüklemesi sırasında seçilen bölge eşleştiğinden emin olun.
      > 
      > Farklı bir bölgeye ağ geçidiniz için daha sonra kullanmak istiyorsanız, yeni bir ağ geçidi kurun ayarlamanız gerekir.

   3. Hazır olduğunuzda, seçin **Bitti**.

7. Böylece artık Azure Portalı'ndaki adımları [ağ geçidiniz için bir Azure kaynağı oluşturma](../logic-apps/logic-apps-gateway-connection.md). 

Daha fazla bilgi edinmek [veri ağ geçidinin nasıl çalıştığını](#gateway-cloud-service).

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a>Geçirme, geri yüklemek veya mevcut bir ağ geçidi alın

Bu görevleri gerçekleştirmek için ağ geçidi yüklendiğinde, belirttiğiniz kurtarma anahtarı olması gerekir.

1. Bilgisayarınızın Başlat menüsünden seçin **şirket içi veri ağ geçidi**.

2. Yükleyici açıldıktan sonra oturum oturum aynı Azure iş veya Okul daha önce ağ geçidi yüklemek için kullanılan hesabı.

3. Seçin **geçirme, geri yükleme veya mevcut bir ağ geçidi üzerinden Al**.

4. Geçirme, geri yüklemek veya devralır istediğiniz ağ geçidi için adı ve kurtarma anahtarı sağlayın.

<a name="windows-service-account"></a>

## <a name="windows-service-account"></a>Windows hizmet hesabı

Şirket içi veri ağ geçidi bir Windows hizmet olarak çalışır ve kullanmak üzere ayarlanmış `NT SERVICE\PBIEgwService` için Windows hizmeti oturum açma kimlik bilgileri. Varsayılan olarak, ağ geçidi, ağ geçidi yüklediğiniz makine "hizmet olarak oturum aç" hakkı vardır. Oluşturmak ve ağ geçidi Azure portalında korumak için Windows hizmet hesabının en az olması **katkıda bulunan** izinleri. 

> [!NOTE]
> Windows hizmet hesabını şirket içi veri kaynaklarına bağlanmak için kullanılan hesap farklıdır ve Azure'dan iş veya Okul hesabı bulut hizmetlerine oturum açmak için kullandığınız.

<a name="restart-gateway"></a>

## <a name="restart-the-gateway"></a>Ağ geçidini yeniden başlatın

Diğer Windows hizmeti gibi başlatın ve birden çok yolla hizmetini durdurun. Örneğin, ağ geçidi çalıştığı bilgisayarda yükseltilmiş izinleri olan bir komut istemi açın ve ya da şu komutları çalıştırın:

* Hizmeti durdurmak için şu komutu çalıştırın:
  
    `net stop PBIEgwService`

* Hizmeti başlatmak için şu komutu çalıştırın:
  
    `net start PBIEgwService`

## <a name="configure-a-firewall-or-proxy"></a>Bir güvenlik duvarı veya proxy yapılandırma

Ağ geçidi giden bir bağlantı oluşturur [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Ağ geçidiniz için proxy bilgilerini sağlamak için bkz: [proxy ayarlarını yapılandırma](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).

Güvenlik Duvarı veya proxy bağlantıları engelleyebilecek olup olmadığını denetlemek için makinenizde gerçekte Internet'e bağlanıp bağlanamayacağını onaylayın ve [Azure Service Bus](https://azure.microsoft.com/services/service-bus/). Bir PowerShell isteminden şu komutu çalıştırın:

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> Bu komut, yalnızca bir ağ bağlantısı ve Azure Service Bus bağlantı'test eder. Bu nedenle komut ağ geçidi veya şifreler ve kimlik bilgilerinizi ve ağ geçidi ayrıntıları depolayan ağ geçidi bulut hizmeti ile yapmak için herhangi bir şey yok. 
>
> Ayrıca, bu komut yalnızca Windows Server 2012 R2 veya daha sonra kullanılabilir ve Windows 8.1 veya üzeri. Önceki işletim sistemi sürümlerinde, Telnet bağlantısını test etme için kullanabilirsiniz. Daha fazla bilgi edinmek [Azure Service Bus ve karma çözümleri](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

Sonuçlarınızı bu örneğe benzer görünmelidir:

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

Varsa **TcpTestSucceeded** ayarlanmazsa **doğru**, güvenlik duvarı tarafından engellenmiş olabilir. Kapsamlı olmasını istiyorsanız, yedek **ComputerName** ve **bağlantı noktası** altında listelenen değerleri değerlerle [bağlantı noktalarını yapılandırma](#configure-ports) bu konuda.

Güvenlik duvarını ayrıca Azure Service Bus Azure veri merkezleri için yaptığı bağlantıları engelleyebilir. Bu senaryo durumda Onayla (engelini kaldırma) bu veri merkezlerinde bölgeniz için tüm IP adresleri. Bu IP adresleri için [Azure IP adresleri listesi alma](https://www.microsoft.com/download/details.aspx?id=41653).

## <a name="configure-ports"></a>Bağlantı noktalarını yapılandırma

Ağ geçidi giden bir bağlantı oluşturur [Azure Service Bus](https://azure.microsoft.com/services/service-bus/) ve giden bağlantı noktaları iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9354 aracılığıyla 9350. Ağ geçidi gelen bağlantı noktalarının gerektirmez. Daha fazla bilgi edinmek [Azure Service Bus ve karma çözümleri](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).

| ETKİ ALANI ADLARI | GİDEN BAĞLANTI NOKTALARI | AÇIKLAMA |
| --- | --- | --- |
| *. analysis.windows.net | 443 | HTTPS | 
| *. login.windows.net | 443 | HTTPS | 
| *. servicebus.windows.net | 5671-5672 | Gelişmiş Message Queuing Protokolü (AMQP) | 
| *. servicebus.windows.net | 443, 9350-9354 | Hizmet veri yolu geçişi (erişim denetimi belirteci alımı için 443'ü gerektirir) TCP üzerinden üzerindeki dinleyicileri | 
| *. frontend.clouddatahub.net | 443 | HTTPS | 
| *. core.windows.net | 443 | HTTPS | 
| Login.microsoftonline.com | 443 | HTTPS | 
| *. msftncsi.com | 443 | Ağ geçidi Power BI hizmeti tarafından erişilemiyor olduğunda internet bağlantısı test etmek için kullanılır. | 

IP adresleri etki alanları yerine onaylamak varsa, kullanmak karşıdan yükleyip [Microsoft Azure veri merkezi IP aralıkları listesi](https://www.microsoft.com/download/details.aspx?id=41653). Bazı durumlarda, tam etki alanı adı yerine IP adresi ile Azure Service Bus bağlantılar yapılır.

<a name="gateway-cloud-service"></a>
## <a name="how-does-the-data-gateway-work"></a>Veri ağ geçidi nasıl çalışır?

Veri ağ geçidi mantıksal uygulamanızı, ağ geçidi bulut Hizmeti'ne ve şirket içi veri kaynağınız arasında hızlı ve güvenli iletişimi kolaylaştırır. 

![Diagram-for-on-Premises-Data-Gateway-Flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

Bunu bulutta kullanıcı, şirket içi veri kaynağına bağlı olan bir öğe ile etkileşime giren zaman:

1. Ağ geçidi bulut Hizmeti'ne şifrelenmiş kimlik bilgileri veri kaynağı için birlikte bir sorgu oluşturur ve işlemek ağ geçidi için sorgu gönderir.

2. Ağ geçidi bulut Hizmeti'ne sorgu analiz eder ve Azure Service Bus hizmetine istek iter.

3. Şirket içi veri ağ geçidi bekleyen istekler için Azure Service Bus yoklar.

4. Ağ geçidi sorgu alır, kimlik bilgileri şifresini çözer ve bu kimlik bilgileri ile veri kaynağına bağlanır.

5. Ağ geçidi yürütme için veri kaynağı sorgusu gönderir.

6. Sonuçlar veri kaynağından ağ geçidi dönün ve ağ geçidi bulut hizmetine gönderilir. Ağ geçidi bulut Hizmeti'ne ardından sonuçları kullanır.

<a name="faq"></a>
## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="general"></a>Genel

**Q**: bir ağ geçidi bulutta SQL Azure gibi veri kaynakları için ihtiyacım var? <br/>
**A**: Hayır Bir ağ geçidi yalnızca şirket içi veri kaynağına bağlanır.

**Q**: ağ geçidi veri kaynağı ile aynı makinede yüklü olması gerekmez? <br/>
**A**: Hayır Ağ geçidi sağlanan bağlantı bilgilerini kullanarak veri kaynağına bağlanır. Bu bağlamda bir istemci uygulaması olarak, ağ geçidi göz önünde bulundurun. Ağ geçidi sağlanan sunucusu adı bağlanma özelliği yalnızca gerekir.

<a name="why-azure-work-school-account"></a>

**Q**: neden gerekir t bir Azure okul veya iş hesabı oturum açmak için? <br/>
**A**: yalnızca Azure bir iş veya Okul hesabınız, şirket içi veri ağ geçidi yüklediğinizde. Oturum açma hesabınızın Azure Active Directory (Azure AD) tarafından yönetilen bir kiracı depolanır. Genellikle, Azure AD hesabınızın kullanıcı asıl adı (UPN) e-posta adresi ile eşleşir.

**Q**: kimlik bilgilerimi depolandığı? <br/>
**A**: bir veri kaynağı için girdiğiniz kimlik bilgileri şifrelenir ve ağ geçidi bulut hizmetinde depolanır. Kimlik bilgileri şirket içi veri ağ geçidi şifresi çözülür.

**Q**: ağ bant genişliği için tüm gereksinimleri vardır? <br/>
**A**: ağ bağlantısı iyi üretilen işi olan öneririz. Her ortam farklıdır ve gönderilen verilerin miktarını sonuçları etkiler. ExpressRoute kullanarak şirket içi ve Azure veri merkezleri arasında işleme düzeyini güvence altına almak için yardımcı olabilir.
Üçüncü taraf aracı Azure hızı Test uygulaması, üretilen iş ölçer yardımcı olmak için kullanabilirsiniz.

**Q**: bir veri kaynağına ağ geçidi'nden sorguları çalıştırmak için gecikme süresi nedir? En iyi mimarisi nedir? <br/>
**A**: ağ gecikmesini azaltmak için veri kaynağı olarak mümkün olduğunca yakın ağ geçidini yükleyin. Gerçek veri kaynağı üzerinde ağ geçidi yükleyebilirsiniz, bu yakınlık sunulan gecikme süresi en aza indirir. Veri merkezleri çok göz önünde bulundurun. Örneğin, Batı ABD datacenter hizmetinizi kullanır ve SQL Server bir Azure VM ile barındırılan varsa, Azure VM Batı ABD çok olması gerekir. Bu yakınlık gecikme süresi en aza indirir ve çıkış ücretlerini Azure VM'de önler.

**Q**: nasıl sonuçları gönderilir bulut için? <br/>
**A**: sonuçları, Azure Service Bus gönderilir.

**Q**: tüm gelen bağlantıları buluttan ağ geçidine vardır? <br/>
**A**: Hayır Ağ geçidi, Azure Service Bus giden bağlantılara kullanır.

**Q**: ne ı giden bağlantıları engelle? Açmak neler gerekir? <br/>
**A**: bağlantı noktaları ve ağ geçidini kullanan konakları bakın.

**Q**: ne gerçek Windows hizmeti adı verilir?<br/>
**A**: içinde Hizmetleri, ağ geçidi Power BI kurumsal ağ geçidi hizmeti çağrılır.

**Q**: ağ geçidi Windows hizmeti bir Azure Active Directory hesabıyla çalıştırabilir miyim? <br/>
**A**: Hayır Windows hizmeti geçerli bir Windows hesabı olması gerekir. Varsayılan olarak, hizmet hizmet SID, NT SERVICE\PBIEgwService çalışır.

### <a name="high-availability-and-disaster-recovery"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

**Q**: olağanüstü durum kurtarma için kullanılabilir seçenekleri nelerdir? <br/>
**A**: Kurtarma anahtarını geri yüklemek veya bir ağ geçidi taşımak için kullanabilirsiniz. Ağ geçidi yüklediğinizde, Kurtarma anahtarını belirtin.

**Q**: Kurtarma anahtarı avantajı nedir? <br/>
**A**: Kurtarma anahtarı geçirmek veya ağ geçidi ayarlarınızı sonra bir olağanüstü durum kurtarma için bir yol sağlar.

**Q**: ağ geçidi ile yüksek kullanılabilirlik senaryolarını etkinleştirmek için herhangi bir plan vardır? <br/>
**A**: yol haritası üzerinde bu senaryolar verilmiştir ancak henüz bir zaman çizelgesi bulunmuyor.

## <a name="troubleshooting"></a>Sorun giderme

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

**Q**: nasıl ı görebilir ne sorguları yükleniyor şirket içi veri kaynağına gönderilen? <br/>
**A**: gönderilen sorgular sorgu izlemeyi etkinleştirebilirsiniz. Sorgu geri sorun giderme tamamlanınca özgün değeri izleme değiştirmeyi unutmayın. Sorgu izlemesi açık bırakarak daha büyük günlükleri oluşturur.

Ayrıca, izleme sorguları için veri kaynağınız olan araçlar da bakabilirsiniz. Örneğin, SQL Server ve Analysis Services için genişletilmiş olaylar veya SQL Profiler kullanabilirsiniz.

**Q**: ağ geçidi günlüklerini nerede? <br/>
**A**: Bu konunun ilerleyen bölümlerinde bkz araçları.

### <a name="update-to-the-latest-version"></a>Son sürüme güncelleştir

Ağ geçidi sürümü güncel olmayan hale geldiğinde birçok sorunları ortaya. Genel iyi uygulama olarak, en son sürümünü kullandığınızdan emin olun. Ağ geçidi, bir veya daha uzun bir ay için güncelleştirmediyseniz, ağ geçidinin en son sürümünü yüklemeyi göz önünde bulundurun ve sorunu yeniden bakın.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hata: kullanıcı gruba eklenemedi. (-2147463168 PBIEgwService performans günlük kullanıcılar)

Desteklenmeyen bir etki alanı denetleyicisinde ağ geçidini yüklemeye çalıştığınızda bu hatayı alabilirsiniz. Bir etki alanı denetleyicisi olmayan bir makineye ağ geçidi dağıttığınızdan emin olun.

## <a name="tools"></a>Araçlar

### <a name="collect-logs-from-the-gateway-configurer"></a>Ağ geçidi configurer günlüklerini toplayın

Ağ geçidi için birkaç günlüklerini toplayabilir. Her zaman günlükleri ile başlayın!

#### <a name="installer-logs"></a>Yükleyici günlükleri

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a>Yapılandırma günlükleri

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a>Kurumsal ağ geçidi hizmeti günlükleri

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a>Olay günlükleri

Veri Yönetimi ağ geçidi ve PowerBIGateway logs altında bulabilirsiniz **uygulama ve hizmet günlükleri**.

### <a name="fiddler-trace"></a>Fiddler'ı izleme

[Fiddler](http://www.telerik.com/fiddler) HTTP trafiği izler telerik'ten ücretsiz bir araçtır. İstemci makine Power BI hizmetinden ile bu trafiği görebilirsiniz. Bu hizmet, hataları ve diğer ilgili bilgileri gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar
    
* [Şirket içi veri mantığı uygulamalardan bağlanmak](../logic-apps/logic-apps-gateway-connection.md)
* [Kurumsal tümleştirme özellikleri](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Azure mantıksal uygulamaları için bağlayıcılar](../connectors/apis-list.md)
