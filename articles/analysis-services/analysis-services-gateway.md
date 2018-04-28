---
title: Şirket içi veri ağ geçidi | Microsoft Docs
description: Analysis Services sunucunuzun azure'da şirket içi veri kaynağına bağlanır, bir şirket içi ağ geçidi gereklidir.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: abd2d2c8e220a946d08774f8e55ea968008c1757
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="connecting-to-on-premises-data-sources-with-azure-on-premises-data-gateway"></a>Azure şirket içi veri ağ geçidi ile şirket içi veri kaynaklarına bağlanma
Şirket içi veri ağ geçidi, şirket içi veri kaynakları ve Azure Analysis Services sunucularınızı bulutta arasında güvenli veri aktarımını sağlayan bir köprü gibi davranır. Aynı bölgede birden çok Azure Analysis Services sunucusu ile çalışma ek olarak, ağ geçidinin en son sürümünü de Azure Logic Apps, Power BI, güç uygulamaları ve Microsoft Flow ile çalışır. Tek bir ağ geçidi ile aynı bölgede birden çok hizmet ilişkilendirebilirsiniz. 

İlk kez ağ geçidi ile kurulumun tamamlanmasında dört bölümden oluşan bir işlemdir:

- **Kurulumunu indirin ve çalıştırın** -Bu adım, kuruluşunuzdaki bir bilgisayarda bir ağ geçidi hizmeti yükler. Ayrıca Azure bir hesabı kullanarak oturum, [kiracının](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant) Azure AD. Azure B2B (konuk) hesapları desteklenmez.

- **Ağ geçidini kaydetmek** - Bu adımda, bir ad belirtin ve kurtarma anahtarı, ağ geçidiniz için ve ağ geçidiniz ağ geçidi bulut hizmeti ile kaydetme bir bölge seçin. Ağ geçidi kaynağı herhangi bir bölgede kaydedilebilir, ancak Analysis Services sunucuları ile aynı bölgede olması önerilir. 

- **Bir ağ geçidi kaynağı oluşturma** -Bu adımda, Azure aboneliğinizde bir ağ geçidi kaynağı oluşturun.

- **Sunucularınız, ağ geçidi kaynağına bağlanma** -aboneliğinizde bir ağ geçidi kaynağına sahip olduktan sonra sunucularınız tarafından bağlanan başlayabilirsiniz. İçin birden çok sunucu ve diğer kaynaklara bağlanabilir.

Hemen kullanmaya başlamak için bkz: [yüklemek ve şirket içi veri ağ geçidi yapılandırma](analysis-services-gateway-install.md).

## <a name="how-it-works"> </a>Nasıl çalışır?
Bir bilgisayara yüklemeniz, kuruluşunuzda ağ geçidi Windows hizmeti olarak çalışan **şirket içi veri ağ geçidi**. Bu yerel hizmet Azure Service Bus aracılığıyla ağ geçidi bulut hizmetine kayıtlı. Ardından Azure aboneliğiniz için bir ağ geçidi kaynağı ağ geçidi bulut hizmeti oluşturun. Azure Analysis Services sunucuları, ağ geçidi kaynağı bağlanmıştır. Sunucunuzdaki modelleri kaynakları sorgular veya işleme için şirket içi verilerinize bağlanın gerektiğinde, bir sorgu ve veri akışı ağ geçidi kaynağı, Azure Service Bus, yerel şirket içi veri ağ geçidi hizmeti ve veri kaynaklarınızı erişir. 

![Nasıl çalışır?](./media/analysis-services-gateway/aas-gateway-how-it-works.png)

Sorgular ve veri akış:

1. Bir sorgu tarafından şifrelenmiş kimlik bilgileri bulut hizmetiyle şirket içi veri kaynağı için oluşturulur. Daha sonra işlemek ağ geçidi için bir sırasına gönderilir.
2. Ağ geçidi bulut Hizmeti'ne sorgu analiz eder ve isteği iter [Azure Service Bus](https://azure.microsoft.com/documentation/services/service-bus/).
3. Şirket içi veri ağ geçidi bekleyen istekler için Azure Service Bus yoklar.
4. Ağ geçidi sorgu alır, kimlik bilgileri şifresini çözer ve veri kaynağı bu kimlik bilgileri ile bağlanır.
5. Ağ geçidi yürütme için veri kaynağı sorgusu gönderir.
6. Sonuçları, ağ geçidi dönün ve bulut hizmeti ve sunucunuz üzerine veri kaynağından gönderilir.

## <a name="windows-service-account"> </a>Windows hizmet hesabı
Şirket içi veri ağ geçidi kullanacak şekilde yapılandırılmış *NT SERVICE\PBIEgwService* Windows hizmeti oturum açma kimlik bilgileri. Varsayılan olarak, bir hizmet olarak oturum açma hakkı vardır; ağ geçidi yüklüyorsanız makine bağlamında. Bu kimlik bilgileri, şirket içi veri kaynaklarına bağlanmak için kullanılan aynı hesap ya da Azure hesabınızda değil.  

Proxy sunucunuz kimlik doğrulaması nedeniyle ile sorunlarla karşılaşırsanız, Windows hizmet hesabı etki alanı kullanıcısına değiştirmek isteyebilirsiniz veya yönetilen hizmet hesabı.

## <a name="ports"> </a>Bağlantı noktaları
Ağ geçidi, Azure Service Bus giden bir bağlantı oluşturur. Giden bağlantı noktaları iletişim kurar: TCP 443 (varsayılan), 5671, 5672, 9354 aracılığıyla 9350.  Ağ geçidi gelen bağlantı noktalarının gerektirmez.

Güvenlik Duvarı'nda, veri bölgesinin IP adreslerini güvenilir listeye öneririz. İndirebilirsiniz [Microsoft Azure veri merkezi IP listesi](https://www.microsoft.com/download/details.aspx?id=41653). Bu liste haftalık güncelleştirilir.

> [!NOTE]
> Azure veri merkezi IP listesinde listelenen IP adresleri CIDR gösteriminde değil. Daha fazla bilgi için bkz: [sınıfsız etki alanları arası yönlendirme](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing).
>
>

Ağ Geçidi tarafından kullanılan tam etki alanı adları şunlardır:

| Etki alanı adları | Giden bağlantı noktaları | Açıklama |
| --- | --- | --- |
| *. powerbı.com |80 |Yükleyici indirmek için kullanılan HTTP. |
| *. powerbı.com |443 |HTTPS |
| *. analysis.windows.net |443 |HTTPS |
| *. login.windows.net |443 |HTTPS |
| *.servicebus.windows.net |5671-5672 |Gelişmiş Message Queuing Protokolü (AMQP) |
| *.servicebus.windows.net |443, 9350-9354 |Hizmet veri yolu geçişi (erişim denetimi belirteci alımı için 443'ü gerektirir) TCP üzerinden üzerindeki dinleyicileri |
| *.frontend.clouddatahub.net |443 |HTTPS |
| *. core.windows.net |443 |HTTPS |
| login.microsoftonline.com |443 |HTTPS |
| *. msftncsi.com |443 |Ağ geçidi Power BI hizmeti tarafından erişilemediğinde internet bağlantısı test etmek için kullanılır. |
| *.microsoftonline-p.com |443 |Yapılandırmasına bağlı olarak kimlik doğrulaması için kullanılır. |

### <a name="force-https"></a>Azure Service Bus ile HTTPS iletişimi zorlama
Doğrudan TCP yerine HTTPS kullanarak Azure Service Bus ile iletişim kurmak için ağ geçidi zorlayabilirsiniz; Ancak, bunun nedenle performansı büyük ölçüde düşürebilir. Değiştirebileceğiniz *Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config* değerini değiştirerek dosya `AutoDetect` için `Https`. Bu genellikle bir dosyadır *C:\Program Files\On içi veri ağ geçidi*.

```
<setting name="ServiceBusSystemConnectivityModeString" serializeAs="String">
    <value>Https</value>
</setting>
```

## <a name="tenant-level-administration"></a>Kiracı düzeyi Yönetimi 

Şu anda Kiracı Yöneticiler diğer kullanıcıların yüklenmiş ve yapılandırılmış tüm ağ geçitleri yönetebileceği tek bir konum.  Kiracı yöneticisi değilseniz, yükledikleri her bir ağ geçidi için bir yönetici olarak eklemek için kuruluşunuzdaki kullanıcılara sor önerilir. Bu sayede, kuruluşunuzda aracılığıyla veya ağ geçidi ayarları sayfası aracılığıyla tüm ağ geçitleri yönetmek [PowerShell komutlarını](https://docs.microsoft.com/power-bi/service-gateway-high-availability-clusters#powershell-support-for-gateway-clusters). 


## <a name="faq"></a>Sık sorulan sorular

### <a name="general"></a>Genel

**Q**: bir ağ geçidi bulutta Azure SQL veritabanı gibi veri kaynakları için ihtiyacım var? <br/>
**A**: Hayır Bir ağ geçidi, yalnızca şirket içi veri kaynaklarına bağlanmak için gereklidir.

**Q**: ağ geçidi veri kaynağı ile aynı makinede yüklü olması gerekmez? <br/>
**A**: Hayır Ağ geçidi yalnızca aynı ağ üzerinde genellikle sunucusuna bağlanmak için özellik gerekiyor.

<a name="why-azure-work-school-account"></a>

**Q**: neden gerekiyor mu oturum açmak için bir iş veya Okul hesabı kullanmanız? <br/>
**A**: şirket içi veri ağ geçidi yüklediğinizde, yalnızca bir kuruluş iş veya Okul hesabını kullanabilirsiniz. Ve hesap abonelik olarak aynı Kiracı içinde olmalıdır, ağ geçidi kaynak yapılandırmış olursunuz. Oturum açma hesabınızın Azure Active Directory (Azure AD) tarafından yönetilen bir kiracı depolanır. Genellikle, Azure AD hesabınızın kullanıcı asıl adı (UPN) e-posta adresi ile eşleşir.

**Q**: kimlik bilgilerimi depolandığı? <br/>
**A**: bir veri kaynağı için girdiğiniz kimlik bilgileri şifrelenir ve ağ geçidi bulut hizmetinde depolanır. Kimlik bilgileri şirket içi veri ağ geçidi şifresi çözülür.

**Q**: ağ bant genişliği için tüm gereksinimleri vardır? <br/>
**A**: ağ bağlantısı iyi verimlilik sahip önerilir. Her ortam farklıdır ve gönderilen verilerin miktarını sonuçları etkiler. ExpressRoute kullanarak şirket içi ve Azure veri merkezleri arasında işleme düzeyini güvence altına almak için yardımcı olabilir.
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
**A**: içinde Hizmetleri, ağ geçidi şirket içi veri ağ geçidi hizmeti çağrılır.

**Q**: ağ geçidi Windows hizmeti bir Azure Active Directory hesabıyla çalıştırabilir miyim? <br/>
**A**: Hayır Windows hizmeti geçerli bir Windows hesabı olması gerekir. Varsayılan olarak, hizmet hizmet SID, NT SERVICE\PBIEgwService çalışır.

**Q**: nasıl yedeklerim devralma bir ağ geçidi? <br/>
**A**: devralma bir ağ geçidi için (Denetim Masası'nda Kurulum/Değiştir çalıştırarak > Programlar), Azure ağ geçidi kaynak sahibi olması ve kurtarma anahtarı olması gerekir. Ağ geçidi kaynak sahiplerine erişim denetimi yapılandırılabilir.

### <a name="high-availability"></a>Yüksek kullanılabilirlik ve olağanüstü durum kurtarma

**Q**: olağanüstü durum kurtarma için kullanılabilir seçenekleri nelerdir? <br/>
**A**: Kurtarma anahtarını geri yüklemek veya bir ağ geçidi taşımak için kullanabilirsiniz. Ağ geçidi yüklediğinizde, Kurtarma anahtarını belirtin.

**Q**: Kurtarma anahtarı avantajı nedir? <br/>
**A**: Kurtarma anahtarı geçirmek veya ağ geçidi ayarlarınızı sonra bir olağanüstü durum kurtarma için bir yol sağlar.

## <a name="troubleshooting"> </a>Sorun giderme

**Q**: yok görmemin nedeni ağ geçidi örneklerinin listesi my ağ geçidi ağ geçidi kaynağı oluşturmak çalışırken? <br/>
**A**: iki olası nedeni vardır. Önce bir kaynak zaten ağ geçidi geçerli ya da başka bir abonelik oluşturulur. Bu olasılığını ortadan kaldırmak için türündeki kaynaklar listeleme **şirket içi veri ağ geçidi** portalından. Tüm abonelikleri tüm kaynakları numaralandırılırken seçtiğinizden emin olun. Kaynak oluşturulduktan sonra ağ geçidi ağ geçidi kaynağı oluşturmak portal deneyiminde ağ geçidi örneklerinin listesi görünmez. İkinci olasılığını ağ geçidi yükleyen kullanıcının Azure AD kimlik Azure portalında oturum açtığı kullanıcı farklı olmasıdır. Çözmek için aynı hesabı kullanarak ağ geçidini yükleyen kullanıcı portalında oturum açın.

**Q**: nasıl ı görebilir ne sorguları yükleniyor şirket içi veri kaynağına gönderilen? <br/>
**A**: gönderilen sorgular sorgu izlemeyi etkinleştirebilirsiniz. Sorgu geri sorun giderme tamamlanınca özgün değeri izleme değiştirmeyi unutmayın. Sorgu izlemesi açık bırakarak daha büyük günlükleri oluşturur.

Ayrıca, izleme sorguları için veri kaynağınız olan araçlar da bakabilirsiniz. Örneğin, SQL Server ve Analysis Services için genişletilmiş olaylar veya SQL Profiler kullanabilirsiniz.

**Q**: ağ geçidi günlüklerini nerede? <br/>
**A**: Bu makalenin sonraki bölümlerinde günlüklere bakın.

### <a name="update"></a>En son sürüme güncelleştir

Ağ geçidi sürümü güncel olmayan hale geldiğinde birçok sorunları ortaya. Genel iyi uygulama olarak, en son sürümünü kullandığınızdan emin olun. Ağ geçidi, bir veya daha uzun bir ay için güncelleştirmediyseniz, ağ geçidinin en son sürümünü yüklemeyi göz önünde bulundurun ve sorunu yeniden bakın.

### <a name="error-failed-to-add-user-to-group--2147463168-pbiegwservice-performance-log-users"></a>Hata: kullanıcı gruba eklenemedi. (-2147463168 PBIEgwService performans günlük kullanıcılar)

Desteklenmeyen bir etki alanı denetleyicisinde ağ geçidini yüklemeye çalıştığınızda bu hatayı alabilirsiniz. Bir etki alanı denetleyicisi olmayan bir makineye ağ geçidi dağıttığınızdan emin olun.

## <a name="logs"></a>Günlükleri

Günlük dosyaları bir önemli sorun giderirken kaynaktır.

#### <a name="enterprise-gateway-service-logs"></a>Kurumsal ağ geçidi hizmeti günlükleri

`C:\Users\PBIEgwService\AppData\Local\Microsoft\On-premises data gateway\<yyyyymmdd>.<Number>.log`

#### <a name="configuration-logs"></a>Yapılandırma günlükleri

`C:\Users\<username>\AppData\Local\Microsoft\On-premises data gateway\GatewayConfigurator.log`




#### <a name="event-logs"></a>Olay günlükleri

Veri Yönetimi ağ geçidi ve PowerBIGateway logs altında bulabilirsiniz **uygulama ve hizmet günlükleri**.


## <a name="telemetry"></a>Telemetri
Telemetri, izleme ve sorun giderme için kullanılabilir. Varsayılan olarak

**Telemetriyi etkinleştirmek için**

1.  Şirket içi veri ağ geçidi istemci dizini bilgisayarda denetleyin. Genellikle,. **%SystemDrive%\Program Files\On içi veri ağ geçidi**. Veya, Hizmetler konsolunu açın ve yürütülebilir dosya yolunu denetleyin: şirket içi veri ağ geçidi hizmeti bir özelliğidir.
2.  İstemci dizininden Microsoft.PowerBI.DataMovement.Pipeline.GatewayCore.dll.config dosyasında. SendTelemetry ayarı true olarak değiştirin.
        
    ```
        <setting name="SendTelemetry" serializeAs="String">
                    <value>true</value>
        </setting>
    ```

3.  Yaptığınız değişiklikleri kaydedin ve Windows hizmetini yeniden başlatın: şirket içi veri ağ geçidi hizmeti.




## <a name="next-steps"></a>Sonraki adımlar
* [Şirket içi veri ağ geçidi yükleyip](analysis-services-gateway-install.md).   
* [Çözümleme Hizmetleri yönetme](analysis-services-manage.md)
* [Azure Analysis Services Veri Al](analysis-services-connect.md)
