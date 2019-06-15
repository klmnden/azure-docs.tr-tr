---
title: Şirket içi veri ağ geçidi
description: Azure Analysis Services sunucunuzu şirket içi veri kaynaklarına bağlanır, bir şirket içi ağ geçidi gereklidir.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/30/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 190394481f17310784f87c9e2f642eeea0b2597f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062229"
---
# <a name="connecting-to-on-premises-data-sources-with-on-premises-data-gateway"></a>Şirket içi veri ağ geçidi ile şirket içi veri kaynaklarına bağlanma
Şirket içi veri ağ geçidi, şirket içi veri kaynakları ile bulutta, Azure Analysis Services sunucuları arasında güvenli veri aktarımı sağlar. Aynı bölgede birden fazla Azure Analysis Services sunucuları ile çalışma ek olarak, ağ geçidinin en son sürümünü de Azure Logic Apps, Power BI, Power Apps ve Microsoft Flow ile çalışır. Tek bir ağ geçidi ile aynı abonelik ve aynı bölgede birden çok hizmet ilişkilendirebilirsiniz. 

Ağ geçidi ile Kurulum ilk kez alma dört kısımlı bir işlemdir:

- **İndirme ve çalıştırma kurulum** -Bu adım, kuruluşunuzdaki bir bilgisayarda bir ağ geçidi hizmetini yükler. Ayrıca bir hesap kullanarak Azure'da oturum, [kiracının](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant) Azure AD. Azure B2B (konuk) hesapları desteklenmez.

- **Ağ geçidi kaydetme** - Bu adımda, bir ad belirtin ve kurtarma anahtarı, ağ geçidiniz için ve ağ geçidinizin ağ geçidi bulut hizmeti ile kaydetme, bir bölge seçin. Ağ geçidi kaynağı herhangi bir bölgede kaydedilebilir, ancak, Analysis Services sunucuları aynı bölgede olması önerilir. 

- **Azure'da bir ağ geçidi kaynağı oluşturmak** -Bu adımda, Azure aboneliğinizde bir ağ geçidi kaynağı oluşturun.

- **Ağ geçidi kaynağınıza sunucularınızın bağlanmak** -aboneliğinizde bir ağ geçidi kaynağına sahip olduğunuzda, sunucularınızı bağlamadan başlayabilirsiniz. Aynı abonelik ve aynı bölgede olması koşuluyla birden çok sunucu ve diğer kaynaklara bağlanabilir.

Hemen kullanmaya başlamak için bkz: [yükleyin ve şirket içi veri ağ geçidinin yapılandırılması](analysis-services-gateway-install.md).

## <a name="how-it-works"> </a>Nasıl çalışır?
Kuruluşunuzda bir bilgisayara yüklemek ağ geçidi Windows hizmeti olarak çalıştırılır **şirket içi veri ağ geçidi**. Bu yerel hizmet aracılığıyla Azure Service Bus ağ geçidi bulut hizmeti ile kaydedilmiştir. Ardından, Azure aboneliğiniz için bir ağ geçidi kaynağı ağ geçidi bulut hizmeti oluşturun. Ardından, Azure Analysis Services sunucuları, ağ geçidi kaynağına bağlı. Sunucunuzdaki modelleri kaynakları sorguları veya işleme için şirket içi verilerinize bağlanmak gerektiğinde bir sorgu ve veri akışı ağ geçidi kaynağı, Azure Service Bus, yerel şirket içi veri ağ geçidi hizmeti ve veri kaynaklarınızı erişir. 

![Nasıl çalışır?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Sorgu ve veri akışı:

1. Bir sorgu, şirket içi veri kaynağı için şifrelenmiş kimlik bilgileri ile bulut hizmeti tarafından oluşturulur. Ardından, ağ geçidinin işlemek kuyruğa gönderilir.
2. Ağ geçidi bulut hizmeti sorguyu analiz eder ve isteği gönderim [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Şirket içi veri ağ geçidi, Azure Service Bus'ta bekleyen istekler yoklar.
4. Ağ geçidi sorguyu alır, kimlik bilgilerinin şifresini çözer ve bu kimlik bilgileriyle veri kaynaklarına bağlanır.
5. Ağ geçidi, yürütme için veri kaynağına sorgu gönderir.
6. Sonuçlar veri kaynağından tekrar ağ geçidi ve ardından bulut hizmeti ve sunucunuzu gönderilir.

## <a name="windows-service-account"> </a>Windows hizmet hesabı
Şirket içi veri ağ geçidi kullanmak üzere yapılandırılmış *NT servıce\pbıegwservice* Windows hizmeti oturum açma kimlik bilgileri için. Varsayılan olarak, hizmet olarak oturum açma hakkı vardır; ağ geçidini yüklemekte olduğunuz makinenin bağlamında. Bu kimlik bilgileri, şirket içi veri kaynaklarına bağlanmak için kullanılan hesap veya Azure hesabınızı değil.  

Ara sunucunuzda kimlik doğrulaması ile sorunlarla karşılaşırsanız Windows hizmet hesabını bir etki alanı kullanıcısı olarak değiştirmek isteyebilirsiniz veya yönetilen hizmet hesabı.

## <a name="ports"> </a>Bağlantı noktaları
Ağ geçidi, Azure Service Bus'a yönelik bir giden bağlantı oluşturur. Bu, giden bağlantı noktaları üzerinden iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9350-9354.  Ağ geçidi için gelen bağlantı noktalarının gerektirmez.

Duvarınızda veri bölgenize IP adreslerini güvenilir listeye almak öneririz. İndirebileceğiniz [Microsoft Azure veri merkezi IP listesini](https://www.microsoft.com/download/details.aspx?id=41653). Bu liste haftalık olarak güncelleştirilir.

> [!NOTE]
> Azure veri merkezi IP listesinde adresler, CIDR gösteriminde ' dir. Daha fazla bilgi için bkz. [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
>
>

Ağ Geçidi tarafından kullanılan tam etki alanı adları şunlardır:

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| --- | --- | --- |
| *.powerbi.com |80 |Yükleyiciyi indirmek için kullanılan HTTP. |
| *.powerbi.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *.login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Gelişmiş ileti sıraya alma Protokolü (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Dinleyiciler (erişim denetimi belirtecinin alınması için 443 gerekir) TCP üzerinden Service Bus geçişi hakkında |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *.msftncsi.com |443 |Power BI hizmeti tarafından ağ geçidine erişilemiyorsa internet bağlantısını test etmek için kullanılır. |
| *.microsoftonline-p.com |443 |Yapılandırmasına bağlı olarak kimlik doğrulaması için kullanılır. |

### <a name="force-https"></a>Azure Service Bus ile HTTPS iletişimini zorlama
Ağ geçidinin Azure Service Bus ile doğrudan TCP yerine HTTPS kullanarak iletişim kurmak için zorunlu kılabilirsiniz; Ancak, bunu yapmanız bu nedenle büyük ölçüde performansı düşürebilir. Değiştirebileceğiniz *Microsoft.powerbı.datamovement.Pipeline.gatewaycore.dll.config* değerini değiştirerek dosya `AutoDetect` için `Https`. Bu dosya genellikle şu konumda bulunur *C:\Program Files\On-premises data gateway*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="tenant-level-administration"></a>Kiracı düzeyinde Yönetim 

Şu anda Kiracı yöneticilerinin diğer kullanıcılar tarafından yüklenmiş ve yapılandırılmış olan tüm ağ geçitlerini yönetebileceği tek bir yer.  Kiracı yöneticisiyseniz, kuruluşunuzdaki yükledikleri her ağ geçidine yönetici olarak eklemek için kullanıcılara sor önerilir. Bu sayede kuruluşunuzdaki ağ geçidi ayarları sayfasından veya aracılığıyla tüm ağ geçitlerini yönetme [PowerShell komutlarını](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters). 


## <a name="faq"></a>Sık sorulan sorular

### <a name="general"></a>Genel

**Q**: Bir ağ geçidi, Azure SQL veritabanı gibi buluttaki veri kaynakları için ihtiyacım var? <br/>
**A**: Hayır. Bir ağ geçidi, yalnızca şirket içi veri kaynaklarına bağlanmak için gereklidir.

**Q**: Ağ geçidi veri kaynağı ile aynı makinede yüklü olması gerekiyor mu? <br/>
**A**: Hayır. Ağ geçidinin genellikle aynı ağ üzerinde sunucuya bağlanma özelliği yalnızca gerekir.

<a name="why-azure-work-school-account"></a>

**Q**: Oturum açmak için bir iş veya Okul hesabını kullanmayı neden ihtiyacım var? <br/>
**A**: Şirket içi veri ağ geçidi yüklediğinizde yalnızca bir kuruluş iş veya Okul hesabı kullanabilirsiniz. Ağ geçidi kaynağı ve hesap aboneliğinizle aynı kiracıda, olmanızın yapılandırdığınızı varsayalım. Oturum açma hesabınız Azure Active Directory (Azure AD) tarafından yönetilen bir kiracı depolanır. Genellikle, Azure AD hesabınızın kullanıcı asıl adı (UPN) e-posta adresi ile eşleşir.

**Q**: Kimlik bilgilerim nerede depolanır? <br/>
**A**: Bir veri kaynağı için girdiğiniz kimlik bilgileri şifrelenir ve ağ geçidi bulut hizmetinde depolanır. Kimlik bilgileri, şirket içi veri ağ geçidi şifresi çözülür.

**Q**: Ağ bant genişliği için herhangi bir gereksinimi var mıdır? <br/>
**A**: Ağ bağlantısı iyi aktarım hızı olan önerilir. Her ortam farklıdır ve gönderilmekte olan veri miktarı sonuçları etkiler. ExpressRoute kullanarak bir şirket içi ve Azure veri merkezleri arasında aktarım hızı düzeyini garanti edilmesine yardımcı olabilir.
Aktarım hızınızı ölçmenize yardımcı olması için üçüncü taraf aracı Azure Speed Test uygulamasını kullanabilirsiniz.

**Q**: Ağ geçidinden bir veri kaynağına sorguları çalıştırmak için gecikme süresi nedir? En iyi mimari nedir? <br/>
**A**: Ağ gecikme süresini azaltmak için ağ geçidi olarak veri kaynağına yakın olabildiğince yükleyin. Ağ geçidini gerçek veri kaynağına yükleyebilirseniz bu yakınlık yükleyebilirseniz gecikme süresi en aza indirir. Veri merkezleri çok göz önünde bulundurun. Örneğin, Batı ABD veri merkezinde hizmetinizi kullanır ve bir Azure VM'de SQL Server varsa, Azure VM yer olan Batı ABD'de çok olması gerekir. Bu yakınlık gecikme süresini en aza indirir ve Azure sanal makinesinde çıkış ücretlerini ortadan kaldırır.

**Q**: Nasıl sonuçlar buluta nasıl gönderilir? <br/>
**A**: Sonuçlar Azure Service Bus gönderilir.

**Q**: Herhangi bir gelen bağlantı ağ geçidine buluttan var mı? <br/>
**A**: Hayır. Ağ geçidi, Azure Service Bus'a yönelik giden bağlantılar kullanır.

**Q**: Peki giden bağlantıları engellersem? Açmak ne gerekiyor? <br/>
**A**: Ağ geçidini kullanan konak ve bağlantı noktalarını bakın.

**Q**: Ne, gerçek Windows hizmetinin adı nedir?<br/>
**A**: Hizmetleri, ağ geçidi şirket içi veri ağ geçidi hizmeti olarak adlandırılır.

**Q**: Ağ geçidi Windows hizmetini bir Azure Active Directory hesabıyla çalıştırılabilir mi? <br/>
**A**: Hayır. Windows hizmetinin geçerli bir Windows hesabı olması gerekir. Varsayılan olarak, hizmet, NT servıce\pbıegwservice hizmet SID'si ile çalışır.

**Q**: Nasıl yapılır? devralma bir ağ geçidi <br/>
**A**: Devralma bir ağ geçidi için (Denetim Masası'ndaki Kurulum/Değiştir'i çalıştırarak > program), Azure ağ geçidi kaynağı için bir sahip olmanız ve kurtarma anahtarı olması gerekir. Ağ geçidi kaynak sahiplerinin, erişim denetimi yapılandırılabilir özelliktedir.

### <a name="high-availability"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

**Q**: Yüksek kullanılabilirlik şeklinize ediyoruz?  
**A**: Bir küme oluşturmak için başka bir bilgisayarda bir ağ geçidi yükleyebilirsiniz. Daha fazla bilgi için bkz. [şirket içi veri ağ geçidi için yüksek kullanılabilirlik kümeleri](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters) , Power BI Gateway belgeleri.

**Q**: Olağanüstü durum kurtarma için hangi seçenekler kullanılabilir? <br/>
**A**: Geri yüklemek veya bir ağ geçidi taşımak için kurtarma anahtarını kullanabilirsiniz. Ağ geçidini yüklerken kurtarma anahtarını belirtin.

**Q**: Kurtarma anahtarının avantajı nedir? <br/>
**A**: Kurtarma anahtarı, olağanüstü bir durumla karşılaştığınızda ağ geçidinizi kurtarmak veya geçirmek için bir yol sağlar.

## <a name="troubleshooting"> </a>Sorun giderme

**Q**: Azure'da ağ geçidi kaynağı oluşturmak çalışırken ağ geçidi örneklerinin listesini neden göremiyorum? <br/>
**A**: İki olası nedeni vardır. Önce bir kaynak zaten geçerli ya da başka bir aboneliğe ağ geçidi için oluşturulur. Bu olanağına ortadan kaldırmak için kaynak türü numaralandırma **şirket içi veri ağ geçitleri** portalından. Tüm abonelikleri tüm kaynakları sıralanırken seçtiğinizden emin olun. Kaynak oluşturulduktan sonra ağ geçidini ağ geçidi kaynağı oluşturma portal deneyiminde ağ geçidi örneklerinin listesini görünmez. İkinci olasılığını ağ geçidini yükleyen kullanıcının Azure AD kimlik Azure portalında oturum açmıştır kullanıcıdan farklı olmasıdır. Gidermek için ağ geçidini yükleyen kullanıcı aynı hesabı kullanarak portalda oturum açın.

**Q**: Hangi sorguların gönderildiğini nasıl görebilirim şirket içi veri kaynağına gönderilen? <br/>
**A**: Gönderilen sorgular içeren sorgu izlemeyi etkinleştirebilirsiniz. Sorgu sorun giderme işlemi tamamlandıktan özgün değerine geri izleme değiştirmeyi unutmayın. Sorgu izleme açık bırakarak daha büyük günlükleri oluşturur.

Veri kaynağınızda bulunan, sorguları izlemeye yönelik araçlara da göz atabilirsiniz. Örneğin, SQL Server ve Analysis Services için SQL Profiler veya genişletilmiş olaylar kullanabilirsiniz.

**Q**: Ağ geçidi günlükleri nerededir? <br/>
**A**: Bu makalenin sonraki bölümlerinde günlüklerine bakın.

### <a name="update"></a>En son sürüme güncelleştirme

Ağ geçidi sürümü güncel olmayan hale geldiğinde birçok sorun ortaya çıkabilir. Genel iyi uygulama olarak, en son sürümü kullandığınızdan emin olun. Ağ geçidini bir ay veya daha uzun güncelleştirmediyseniz ağ geçidinin en son sürümünü yüklemeyi deneyin ve sorun oluşturamadığınızı.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hata: Kullanıcı grubuna eklenemedi. (-2147463168 PBIEgwService performans günlük kullanıcılar)

Ağ geçidi desteklenmez ve etki alanı denetleyicisi üzerinde yüklemeye çalıştığınızda bu hatayı alabilirsiniz. Ağ geçidini bir etki alanı denetleyicisi olmayan bir makineye dağıttığınızdan emin olun.

## <a name="logs"></a>Günlükleri

Günlük dosyaları, sorun giderme sırasında önemli bir kaynak olduğundan.

#### <a name="enterprise-gateway-service-logs"></a>Kurumsal ağ geçidi hizmeti günlükleri

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Yapılandırma günlükleri

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`

#### <a name="event-logs"></a>Olay günlükleri

Veri Yönetimi ağ geçidi ve Powerbıgateway günlükleri altında bulabilirsiniz **uygulama ve hizmet günlükleri**.

## <a name="next-steps"></a>Sonraki adımlar
* [Şirket içi veri ağ geçidi yükleyip yapılandırmanız](analysis-services-gateway-install.md).   
* [Analysis Services'ı yönetme](analysis-services-manage.md)
* [Azure Analysis Services veri alma](analysis-services-connect.md)
