---
title: Mevcut iş şirket içi proxy sunucuları ve Azure AD | Microsoft Docs
description: Mevcut şirket içi proxy sunucuları ile çalışma konusunu kapsar.
services: active-directory
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1e4b073a63b5b6bec565aed67bcaec7ed014261b
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807864"
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Mevcut şirket içi proxy sunucuları ile çalışma

Bu makalede, Azure Active Directory (Azure AD) uygulama Proxy bağlayıcıları giden bağlantı proxy'si sunucularıyla çalışacak şekilde yapılandırmak açıklanmaktadır. Var olan proxy sahip ağ ortamları sahip müşteriler için tasarlanmıştır.

Biz bu ana dağıtım senaryolarında bakarak başlayın:

* Bağlayıcılar, şirket içi giden proxy atlamak için yapılandırın.
* Azure AD uygulama proxy'si erişmek için bir giden proxy kullanmak için bağlayıcıları yapılandırın.

Bağlayıcıları nasıl çalışır hakkında daha fazla bilgi için bkz. [anlamak Azure AD uygulama ara sunucusu bağlayıcıları](application-proxy-connectors.md).

## <a name="bypass-outbound-proxies"></a>Giden proxy atlama

Bağlayıcılar, giden isteklerde temel işletim sistemi bileşeni vardır. Bu bileşenler, Web Proxy Otomatik Bulma (WPAD) kullanarak ağ üzerinde bir proxy sunucusunu bulmak otomatik olarak deneyin.

İşletim sistemi bileşenleri wpad.domainsuffix için DNS araması gerçekleştirerek bir ara sunucu bulmaya. DNS Arama çözümlenirse, bir HTTP isteği wpad.dat IP adresine yapılır. Bu istek proxy yapılandırma betiği, ortamınızdaki olur. Bağlayıcı, bir giden proxy sunucusunun seçmek için bu betiği kullanır. Ancak, bağlayıcı trafik yine de proxy gereken ek yapılandırma ayarları nedeniyle geçmesine değil.

Azure hizmetlerine doğrudan bağlantı kullandığından emin olmak için şirket içi proxy atlama bağlayıcıyı yapılandırabilirsiniz. Korumak için daha az bir yapılandırma olduğunu gösterdiğinden, ağ ilkesi, izin verdiği sürece bu yaklaşım önerilir.

Bağlayıcı için giden bağlantı proxy'si kullanımını devre dışı bırakmak için C:\Program Files\Microsoft AAD uygulaması Proxy Connector\ApplicationProxyConnectorService.exe.config dosyasını düzenleyin ve ekleyin *system.net* bölümü bu kod örneğinde gösterilen:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>
    <defaultProxy enabled="false"></defaultProxy>
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Connector Updater Hizmeti ayrıca Proxy'yi atlar için benzer bir değişiklik ApplicationProxyConnectorUpdaterService.exe.config dosyasına olun. Bu dosya, C:\Program Files\Microsoft AAD uygulama Proxy Connector Updater bulunur.

Varsayılan .config dosyasına geri yapmanız durumunda orijinal dosyalarının kopyalarını emin olun.

## <a name="use-the-outbound-proxy-server"></a>Giden proxy sunucusu kullan

Bazı ortamlarda özel durum olmadan bir giden proxy üzerinden gitmek için tüm giden trafiği gerektirir. Sonuç olarak, proxy atlama bir seçenek değildir.

Giden proxy üzerinden gitmek için bağlayıcı trafiği, aşağıdaki diyagramda gösterildiği gibi yapılandırabilirsiniz:

 ![Azure AD uygulama ara sunucusuna bir giden proxy üzerinden gitmek için trafiği bağlayıcı yapılandırma](./media/application-proxy-configure-connectors-with-proxy-servers/configure-proxy-settings.png)

Yalnızca giden trafiğe sahip sonucu olarak, güvenlik duvarları üzerinden gelen erişimi yapılandırmak için gerek yoktur.

> [!NOTE]
> Uygulama proxy'si kimlik doğrulama için diğer proxy'leri desteklemez. Bağlayıcı/güncelleştirici ağ hizmeti hesabı için kimlik doğrulaması yanıt olmadan bir proxy sunucusuna bağlanmak görebilmeniz gerekir.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>1\. adım: Giden proxy üzerinden gitmek için ilgili hizmetler ve bağlayıcı yapılandırma

WPAD ortamda etkin olduğundan ve uygun şekilde yapılandırıldığından, bağlayıcıyı kullanmak için girişim ve giden proxy sunucusu otomatik olarak bulur. Ancak, bir giden proxy üzerinden gitmek için bağlayıcıyı açıkça yapılandırabilirsiniz.

Bunu yapmak için C:\Program Files\Microsoft AAD uygulaması Proxy Connector\ApplicationProxyConnectorService.exe.config dosyasını düzenleyin ve ekleyin *system.net* Bu kod örneğinde gösterilen bölümü. Değişiklik *proxyserver:8080* yerel ara sunucu adınız veya IP adresi ve dinleme yaptığı bağlantı noktasını yansıtmak için. Bir IP adresi kullanıyor olsanız bile değeri önek http:// olması gerekir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.net>  
    <defaultProxy>   
      <proxy proxyaddress="http://proxyserver:8080" bypassonlocal="True" usesystemdefault="True"/>   
    </defaultProxy>  
  </system.net>
  <runtime>
    <gcServer enabled="true"/>
  </runtime>
  <appSettings>
    <add key="TraceFilename" value="AadAppProxyConnector.log" />
  </appSettings>
</configuration>
```

Ardından, Connector Updater hizmetini C:\Program Files\Microsoft AAD uygulama Proxy Bağlayıcısı Updater\ApplicationProxyConnectorUpdaterService.exe.config dosyasına benzer bir değişiklik yaparak proxy kullanacak şekilde yapılandırın.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>2\. adım: Bağlayıcı ve ilgili hizmetleri akışına trafiğine izin vermek için proxy ayarlarını yapılandırma

Giden proxy dikkate alınması gereken dört unsur vardır:

* Proxy giden kuralları
* Ara sunucu kimlik doğrulaması
* Proxy bağlantı noktası
* SSL denetimi

#### <a name="proxy-outbound-rules"></a>Proxy giden kuralları

Aşağıdaki URL'lere erişim izin ver:

| URL | Nasıl kullanılır |
| --- | --- |
| \*. msappproxy.net<br>\*. servicebus.windows.net | Bağlayıcı ve uygulama proxy'si bulut hizmeti arasında iletişim |
| mscrl.microsoft.com:80<br>CRL.microsoft.com:80<br>ocsp.msocsp.com:80<br>www.microsoft.com:80 | Azure sertifikaları doğrulamak için bu URL'leri kullanır. |
| login.windows.net<br>login.microsoftonline.com | Bağlayıcı, bu URL'ler kayıt işlemi sırasında kullanır. |

Güvenlik Duvarı veya proxy yapılandırmanıza izin veriyorsa DNS izin listeleri, bağlantılara izin vermek \*. msappproxy.net ve \*. servicebus.windows.net. Erişime izin vermek, gerekirse [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). IP aralıklarını haftalık olarak güncelleştirilir.

FQDN DEĞERİNE göre bağlantısına izin vermek ve bunun yerine IP aralıklarını belirtmeniz gerekir, bu seçenekleri kullanın:

* Tüm hedeflere bağlayıcı giden erişim sağlar.
* Tüm bağlayıcı giden erişime izin ver [Azure veri merkezi IP aralıkları](https://www.microsoft.com//download/details.aspx?id=41653). Azure veri merkezi IP aralıkları listesini kullanarak haftalık güncelleştirilir zorluktur. Bir işlem erişim kurallarınızı uygun şekilde güncelleştirildiğinden emin olmak için bir yere koymak gerekir. Yalnızca IP adresleri kümesini kullanarak yapılandırmanızı ayırmak neden olabilir.

#### <a name="proxy-authentication"></a>Ara sunucu kimlik doğrulaması

Ara sunucu kimlik doğrulaması şu anda desteklenmiyor. Bizim geçerli Internet hedeflerini bağlayıcı anonim erişime izin vermek için önerilir.

#### <a name="proxy-ports"></a>Proxy bağlantı noktası

Bağlayıcı, BAĞLAN metoduyla giden SSL tabanlı bağlantılar oluşturur. Bu yöntem bir tünel üzerinden giden bağlantı proxy'si temel olarak ayarlar. Proxy sunucunun tünel 443 ve 80 numaralı bağlantı noktalarına izin verecek şekilde yapılandırın.

> [!NOTE]
> Service Bus HTTPS üzerinden çalıştığında, 443 numaralı bağlantı noktasını kullanır. Ancak, varsayılan olarak, Service Bus doğrudan TCP bağlantıları çalışır ve yalnızca doğrudan bağlantı başarısız olursa HTTPS'ye geri döner.

#### <a name="ssl-inspection"></a>SSL denetimi

Bağlayıcı trafiği için sorunlara neden olduğundan SSL incelemesi bağlayıcı trafiği için kullanmayın. Uygulama proxy'si hizmeti için kimlik doğrulaması için bir sertifika Bağlayıcısı'nı kullanır ve bu sertifikayı SSL incelemesi sırasında kaybolur.

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Bağlayıcı proxy sorunları ve hizmet bağlantı sorunlarını giderme

Artık proxy üzerinden akan tüm trafiği görmeniz gerekir. Sorunlarla karşılaşırsanız, aşağıdaki sorun giderme bilgileri yardımcı olmalıdır.

En iyi yolu belirleyip Bağlayıcısı bağlantı sorunlarını gidermek için bağlayıcı hizmeti başlatılırken bir ağ görüntüsü alın sağlamaktır. Burada, yakalama ve ağ izlemelerini filtreleme hızlı bazı ipuçları verilmektedir.

İzleme aracını kullanabilirsiniz. Bu makalenin amaçları için Microsoft Message Analyzer kullandık. Yapabilecekleriniz [Microsoft'tan indirmek](https://www.microsoft.com/download/details.aspx?id=44226).

Aşağıdaki örnekler için ileti Çözümleyicisi özgüdür, ancak herhangi bir analiz aracı ilkeler uygulanabilir.

### <a name="take-a-capture-of-connector-traffic"></a>Bağlayıcı trafik yakalama Al

İlk sorun giderme için aşağıdaki adımları gerçekleştirin:

1. Services.msc ' Azure AD uygulama ara sunucusu Bağlayıcısı hizmeti durdurun.

   ![Azure AD uygulama ara sunucusu Bağlayıcısı hizmeti services.msc](./media/application-proxy-configure-connectors-with-proxy-servers/services-local.png)

1. İleti Çözümleyicisi'ni yönetici olarak çalıştırın.
1. Seçin **Başlat yerel izleme**.
1. Azure AD uygulama ara sunucusu Bağlayıcısı hizmeti başlatın.
1. Ağ yakalama işlemini durdurun.

   ![Ekran görüntüsü, Dur ağ yakalama düğmesini gösterir.](./media/application-proxy-configure-connectors-with-proxy-servers/stop-trace.png)

### <a name="check-if-the-connector-traffic-bypasses-outbound-proxies"></a>Giden proxy Bağlayıcısı trafiği atlar, denetleyin

Proxy sunucuları atlamak ve doğrudan uygulama ara Sunucusu hizmetine bağlanmak için uygulama ara sunucusu bağlayıcısını yapılandırdıysanız, TCP bağlantı girişimleri başarısız için ağ yakalama bakın istiyorsunuz.

Eğer bu girişimlerden tanımlamak için ileti Çözümleyicisi filtreyi kullanın. Girin `property.TCPSynRetransmit` seçip filtre kutusuna **Uygula**.

TCP bağlantısı kurmak için gönderilen ilk paketi SYN pakettir. Bu paket yanıtı döndürmezse SYN reattempted. Yukarıdaki filtre, tüm yeniden SYN isteklerini görmek için kullanabilirsiniz. Ardından, bu SYN istekleri Bağlayıcısı ile ilgili tüm trafik için karşılık gelen olup olmadığını kontrol edebilirsiniz.

Azure hizmetlerine doğrudan bağlantı kurmak için bağlayıcıyı bekliyorsanız, 443 numaralı bağlantı noktasında SynRetransmit yanıtları göstergesidir bir ağ veya güvenlik duvarı sorunu sahip değildir.

### <a name="check-if-the-connector-traffic-uses-outbound-proxies"></a>Bağlayıcı trafiği giden proxy kullanıp kullanmadığını denetleyin

Uygulama Ara sunucusu Bağlayıcısı trafiğiniz Ara sunucularınız üzerinden gitmek için yapılandırılmışsa, başarısız https bağlantıları için proxy Ara istiyorsunuz.

Bu bağlantı girişimleri için ağ yakalama filtrelemek için girin `(https.Request or https.Response) and tcp.port==8080` ileti Çözümleyicisi filtrede 8080 proxy hizmet bağlantı noktası ile değiştiriliyor. Seçin **Uygula** filtre sonuçlarını görmek için.

Yukarıdaki filtre, gelen/giden proxy bağlantı noktası yalnızca HTTPs isteklerini ve yanıtlarını gösterir. Proxy sunucusu ile iletişim gösteren CONNECT istekler için bakıyorsunuz. Başarılı olduktan sonra bir HTTP Tamam (200) yanıt alın.

407 veya 502, gibi diğer yanıt kodlarını görürseniz bu proxy kimlik doğrulaması gerektiren veya başka bir nedenle trafiğe izin vermiyor anlamına gelir. Bu noktada, proxy sunucusu destek ekibinize etkileşim kurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
* Bağlayıcı bağlantı sorunları ile ilgili sorunlar varsa, sorunuzu içinde sorun [Azure Active Directory Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD&forum=WindowsAzureAD) veya destek ekibimiz ile bir bilet oluşturun.
