---
title: Var olan iş şirket içi proxy sunucuları ve Azure AD | Microsoft Docs
description: Mevcut şirket içi proxy sunucularıyla çalışacak şekilde nasıl ele alınmaktadır.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 0456a46262c6ad27b19487a6e4b2d1b6a139bbc3
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162032"
---
# <a name="work-with-existing-on-premises-proxy-servers"></a>Mevcut şirket içi proxy sunucuları ile çalışma

Bu makalede, giden proxy sunucuları ile çalışmak için Azure Active Directory (Azure AD) uygulama proxy'si bağlayıcıları yapılandırın açıklanmaktadır. Varolan proxy'leri sahip ağ ortamları olan müşteriler için tasarlanmıştır.

Bu ana dağıtım senaryolarında bakarak başlatın:
* Şirket içi giden proxy atlama için bağlayıcıları yapılandırın.
* Azure AD uygulama proxy'si erişmek için bir giden proxy kullanmak için bağlayıcıları yapılandırın.

Bağlayıcıları nasıl çalıştığı hakkında daha fazla bilgi için bkz: [anlamak Azure AD uygulama proxy'si Bağlayıcılar](application-proxy-connectors.md).

## <a name="bypass-outbound-proxies"></a>Giden proxy atlama

Bağlayıcılar giden isteklerde temel işletim sistemi bileşeni vardır. Bu bileşenleri otomatik olarak Web Proxy Otomatik Bulma (WPAD) kullanarak ağ üzerinde bir proxy sunucu bulmaya çalışır.

İşletim sistemi bileşenlerini wpad.domainsuffix için DNS araması gerçekleştirerek bir proxy sunucu bulmaya çalışır. Arama DNS'de çözümlenirse, bir HTTP isteği wpad.dat için IP adresi için yapılır. Bu istek proxy yapılandırması komut dosyası, ortamınızdaki olur. Bağlayıcı, bir giden proxy sunucusu seçmek için bu komut dosyasını kullanır. Ancak, bağlayıcı trafiği hala aracılığıyla, proxy gereken ek yapılandırma ayarları nedeniyle geçebilir değil.

Azure hizmetlerine doğrudan bağlantı kullandığından emin olmak için şirket içi proxy atlama bağlayıcıyı yapılandırabilirsiniz. Korumak için daha az bir yapılandırmaya sahip gösterdiğinden ağ ilkeniz için izin veriyorsa sürece bu yaklaşım öneririz.

Bağlayıcı için giden proxy kullanımını devre dışı bırakmak için C:\Program Files\Microsoft AAD uygulama Proxy Connector\ApplicationProxyConnectorService.exe.config dosyasını düzenleyin ve ekleme *system.net* Bu kod örneğinde gösterildiği bölümü:

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
Bağlayıcı güncelleştirici hizmetini de proxy atladığı emin olmak için benzer ApplicationProxyConnectorUpdaterService.exe.config dosyasına değişiklik. Bu dosya, C:\Program Files\Microsoft AAD uygulama Proxy Connector Updater bulunur.

Varsayılan .config dosyaları geri gerekebileceği özgün dosyaların kopyalarını yaptığınızdan emin olun.

## <a name="use-the-outbound-proxy-server"></a>Giden proxy sunucusu kullan

Bazı ortamlar özel durum olmadan bir giden proxy gitmesi tüm giden trafiği gerektirir. Sonuç olarak, proxy atlama bir seçenek değil.

Aşağıdaki çizimde gösterildiği gibi giden proxy üzerinden gitmek için bağlayıcı trafiğini yapılandırabilirsiniz:

 ![Azure AD uygulama proxy'si giden bir proxy üzerinden gitmek için trafiği bağlayıcı yapılandırma](./media/application-proxy-configure-connectors-with-proxy-servers/configure-proxy-settings.png)

Yalnızca giden trafiğe sahip sonucu olarak, güvenlik duvarı üzerinden gelen erişimi yapılandırmak için gerek yoktur.

>[!NOTE]
>Uygulama proxy'si diğer proxy'leri kimlik doğrulamasını desteklemez. Bağlayıcı/updater ağ hizmeti hesabı kimlik doğrulaması için yüküyle olmadan proxy sunucusuna bağlanmak gerekir.

### <a name="step-1-configure-the-connector-and-related-services-to-go-through-the-outbound-proxy"></a>Adım 1: giden proxy üzerinden gitmek için ilgili hizmetler ve bağlayıcı yapılandırma

WPAD ortamda etkin ve uygun şekilde yapılandırılmış, bağlayıcıyı kullanmak için girişimi ve giden proxy sunucusu otomatik olarak bulur. Ancak, bir giden proxy üzerinden gitmek için bağlayıcı açıkça yapılandırabilirsiniz.

Bunu yapmak için C:\Program Files\Microsoft AAD uygulama Proxy Connector\ApplicationProxyConnectorService.exe.config dosyasını düzenleyin ve ekleme *system.net* Bu kod örneğinde gösterildiği bölümü. Değişiklik *proxyserver:8080* yerel proxy sunucunuzun adını veya IP adresi ve üzerinde dinleme bağlantı noktasını yansıtmak için.

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

Ardından, bağlayıcı güncelleştirici hizmetini C:\Program Files\Microsoft AAD uygulama Proxy Bağlayıcısı Updater\ApplicationProxyConnectorUpdaterService.exe.config dosyasına benzer bir değişiklik yapılarak proxy kullanacak biçimde yapılandırın.

### <a name="step-2-configure-the-proxy-to-allow-traffic-from-the-connector-and-related-services-to-flow-through"></a>Adım 2: proxy Bağlayıcısı ve ilgili hizmetler akışına trafiğe izin verecek şekilde yapılandırma

Giden proxy dikkate alınması gereken dört nokta vardır:
* Proxy giden kuralları
* Proxy kimlik doğrulaması
* Proxy bağlantı noktaları
* SSL denetleme

#### <a name="proxy-outbound-rules"></a>Proxy giden kuralları
Bağlayıcı hizmeti erişim şu uç noktalar için erişim izni ver:

* *. msappproxy.net
* *.servicebus.windows.net

İlk kayıt için aşağıdaki uç noktalarına erişime izin ver:

* login.windows.net
* login.microsoftonline.com

FQDN DEĞERİNE göre bağlantı sağlar ve bunun yerine IP aralıklarını belirtmeniz gerekir, bu seçenekleri kullanın:

* Tüm hedefler bağlayıcı giden erişim sağlar.
* Tüm bağlayıcı giden erişime izin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/en-gb/download/details.aspx?id=41653). Azure veri merkezi IP aralıkları listesi kullanarak haftalık güncelleştirilmiş iştir. Bir işlem, erişim kuralları buna göre güncelleştirilir emin olmak için yerleştirdiniz gerekir. Yalnızca bir alt IP adreslerinin bölüneceği yapılandırmanızı neden olabilir.

#### <a name="proxy-authentication"></a>Proxy kimlik doğrulaması

Proxy kimlik doğrulaması şu anda desteklenmiyor. Bizim geçerli Internet hedeflere bağlayıcı anonim erişime izin vermek için önerilir.

#### <a name="proxy-ports"></a>Proxy bağlantı noktaları

Bağlayıcı BAĞLAN yöntemini kullanarak giden SSL tabanlı bağlantılar sağlar. Bu yöntem temelde giden proxy üzerinden tünel ayarlar. Bağlantı noktaları için 443 ve 80 tünel izin vermek için proxy sunucusunu yapılandırın.

>[!NOTE]
>Hizmet veri yolu HTTPS üzerinden çalıştığında, 443 numaralı bağlantı noktasını kullanır. Ancak, varsayılan olarak, hizmet veri yolu doğrudan TCP bağlantıları çalışır ve yalnızca doğrudan bağlantı başarısız olursa HTTPS'ye geri döner.

#### <a name="ssl-inspection"></a>SSL denetleme
Bağlayıcı trafiği için sorunlara neden olduğu SSL denetleme bağlayıcı trafiği için kullanmayın. Uygulama proxy'si hizmeti için kimlik doğrulaması için bir sertifika Bağlayıcısı'nı kullanır ve bu sertifikayı SSL İnceleme sırasında kaybolmuş olabilir. 

## <a name="troubleshoot-connector-proxy-problems-and-service-connectivity-issues"></a>Bağlayıcı proxy sorunları ve hizmet bağlantı sorunlarını giderme
Şimdi proxy akan tüm trafiği görmeniz gerekir. Sorunlarınız varsa, aşağıdaki sorun giderme bilgileri yardımcı olmalıdır.

Tanımlamak ve bağlayıcı bağlantı sorunlarını gidermek için en iyi yolu bağlayıcı hizmeti başlatma sırasında bir ağ yakalama almaktır. Burada, yakalama ve ağ izlemelerini filtreleme hızlı bazı ipuçları verilmektedir.

Tercih ettiğiniz izleme aracını kullanabilirsiniz. Bu makalede amaçları doğrultusunda, Microsoft Message Analyzer kullandık. Yapabilecekleriniz [Microsoft'tan indirmeniz](https://www.microsoft.com/download/details.aspx?id=44226).

Aşağıdaki örnekler için ileti Çözümleyicisi özeldir, ancak herhangi bir analiz aracı ilkeleri uygulanabilir.

### <a name="take-a-capture-of-connector-traffic"></a>Bağlayıcı trafik görüntüsü alın

İlk sorun giderme için aşağıdaki adımları gerçekleştirin:

1. Services.msc Azure AD uygulama ara sunucusu Bağlayıcısı hizmetini durdurun.

   ![Services.msc Azure AD uygulama ara sunucusu Bağlayıcısı hizmetinde](./media/application-proxy-configure-connectors-with-proxy-servers/services-local.png)

2. İleti Çözümleyicisi yönetici olarak çalıştırın.
3. Seçin **Başlat yerel izleme**.

   ![Ağ Yakalama Başlat](./media/application-proxy-configure-connectors-with-proxy-servers/start-local-trace.png)

3. Azure AD uygulama ara sunucusu Bağlayıcısı hizmetini başlatın.
4. Ağ yakalama işlemini durdurun.

   ![Ağ yakalama işlemini durdurun](./media/application-proxy-configure-connectors-with-proxy-servers/stop-trace.png)

### <a name="check-if-the-connector-traffic-bypasses-outbound-proxies"></a>Bağlayıcı trafiğin giden proxy'leri atladığını olmadığını denetleyin

Proxy sunucuları atlayıp doğrudan uygulama proxy'si hizmete bağlanmak için uygulama proxy'si Bağlayıcınızı yapılandırdıysanız, TCP bağlantı girişimleri başarısız ağ yakalama bakın istiyor. 

İleti Çözümleyicisi filtre bu girişimleri tanımlamak için kullanın. Girin `property.TCPSynRetransmit` seçin ve filtre kutusuna **Uygula**. 

Bir TCP bağlantı kurmak üzere gönderilen ilk paket Eşitlemeye pakettir. Bu paket yanıt döndürmezse Eşitlemeye reattempted. Yeniden aktarılan tüm SYN isteklerini görmek için yukarıdaki filtresini kullanabilirsiniz. Ardından, bu SYN istekleri Bağlayıcısı ile ilgili tüm trafik için karşılık gelen olup olmadığını kontrol edebilirsiniz.

Azure hizmetlerine doğrudan bağlantı bağlayıcı bekliyorsanız, bağlantı noktası 443 üzerinden SynRetransmit yanıtları göstergesidir bir ağ veya güvenlik duvarı sorunu sahip değildir.

### <a name="check-if-the-connector-traffic-uses-outbound-proxies"></a>Bağlayıcı trafiği giden proxy kullanıp kullanmadığını denetleyin

Uygulama Proxy Bağlayıcısı trafiğinizi proxy sunucular üzerinden gitmek için yapılandırdıysanız, başarısız https bağlantıları için proxy aramak istediğiniz. 

Bu bağlantı girişimleri için ağ yakalama filtrelemek için girin `(https.Request or https.Response) and tcp.port==8080` ileti Çözümleyicisi filtrede 8080 proxy hizmet bağlantı noktası ile değiştirme. Seçin **Uygula** filtre sonuçlarını görmek için. 

Önceki filtresinin denetleyicisinden proxy bağlantı noktası yalnızca HTTPs isteklerini ve yanıtlarını gösterir. Proxy sunucusu ile iletişim Göster CONNECT isteklerini arıyorsanız. Başarı bir HTTP Tamam (200) yanıt alın.

407 veya 502, gibi diğer yanıt kodlarını görürseniz, proxy kimlik doğrulaması gerektiren veya başka bir nedenle trafiğe izin değil demektir. Bu noktada, proxy sunucusu destek ekibinize göster.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD uygulama proxy'si bağlayıcılar anlama](application-proxy-connectors.md)

- Bağlayıcı bağlantı sorunları ile ilgili sorununuz olursa, soru sorun [Azure Active Directory Forumu](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD&forum=WindowsAzureAD) veya bizim destek ekibi ile bir anahtar oluşturun.
