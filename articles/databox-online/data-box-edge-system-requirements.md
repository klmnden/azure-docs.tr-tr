---
title: Microsoft Azure veri kutusu Edge sistem gereksinimleri | Microsoft Docs
description: Yazılım ve Azure veri kutusu Ucunuzdaki için ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 01/14/2019
ms.author: alkohli
ms.openlocfilehash: 05f6208788060459e67a787507a56adeed980e2d
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319003"
---
# <a name="azure-data-box-edge-system-requirements-preview"></a>Azure veri kutusu Edge sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure veri kutusu Edge çözümünüz için ve Azure veri kutusu kenarına bağlanan istemciler için önemli sistem gereksinimlerini açıklar. Veri kutusu Ucunuzdaki dağıtmadan önce bilgileri dikkatle gözden öneririz. Yeniden dağıtım ve sonraki işlemi sırasında gerektiğinde bu bilgilere başvurabilir.

Veri kutusu Edge için sistem gereksinimleri şunlardır:

- **Konaklar için yazılım gereksinimleri** -desteklenen platformlar, yerel yapılandırma kullanıcı Arabirimi için tarayıcılar, SMB istemcileri ve cihaz erişen istemciler için tüm ek gereksinimleri açıklanmaktadır.
- **Cihaz için ağ gereksinimleri** -fiziksel cihazın işlemi için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.

> [!IMPORTANT]
> Data Box Edge, önizleme aşamasındadır. Bu çözümü dağıtmadan önce lütfen [Önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin.

## <a name="supported-os-for-clients-connected-to-device"></a>Cihaz için bağlanan istemciler için desteklenen işletim sistemi

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Cihaz erişen istemciler için Desteklenen protokoller

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Yerel web kullanıcı Arabirimi ile desteklenen tarayıcılar

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="port-configuration-for-data-box-edge"></a>Veri kutusu Edge için bağlantı noktası yapılandırması

Aşağıdaki tabloda, SMB, Bulut ve yönetim trafiği için izin vermek için güvenlik duvarını açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* hangi gelen istemci istekleri erişimden Cihazınızı yönü belirtir. *Çıkış* veya *giden* içinde veri kutusu Edge cihazınıza gönderir dışarıdan, veri dağıtımı dışında Örneğin, internet'e giden yön ifade eder.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="port-configuration-for-iot-edge"></a>IOT Edge için bağlantı noktası yapılandırması

Azure IOT Edge, IOT hub'ı Desteklenen protokoller kullanarak Azure bulutuna bir şirket içi uç CİHAZDAN giden iletişim sağlar. Gelen iletişim istekleri, yalnızca cihaz Mesajlaşma (bulut) Örneğin, Azure IOT Edge cihazı için iletileri göndermek için Azure IOT hub'ı gereken yere belirli senaryolar için gereklidir.

Azure IOT Edge çalışma zamanı barındırma sunucuları için bağlantı noktası yapılandırması için aşağıdaki tabloyu kullanın:

| Bağlantı noktası yok. | Daraltma veya genişletme | Bağlantı noktası kapsamı | Gereklidir | Rehber |
|----------|-----------|------------|----------|----------|
| TCP 5671 (AMQP)| Çıkan       | WAN        | Evet      | IOT Edge için varsayılan iletişim protokolü. Azure IOT Edge, diğer Desteklenen protokoller için yapılandırılmadı veya AMQP istenen iletişim protokolü, açık olması gerekir. <br>AMQP için 5672, IOT Edge tarafından desteklenmiyor. <br>Azure IOT Edge kullanan farklı bir IOT Hub protokol desteklendiğinde, bu bağlantı noktası engelleyin. |
| TCP 443 (HTTPS)| Çıkan       | WAN        | Evet      | IOT Edge sağlama için giden açın. Yöntem isteği gönderebilir yaprak cihazlar ile saydam bir ağ geçidi varsa. Bu durumda, bağlantı noktası 443 IOT hub'a bağlama ya da IOT Hub, Azure IOT Edge üzerinden hizmetleri sağlamak için dış ağlara açık olması gerekmez. Bu nedenle gelen kuralı yalnızca iç ağdan gelen açmak için kısıtlı olabilir. |
| TCP 5671 (AMQP) | İçinde        |            | Hayır       | Gelen bağlantılar engellenir.|
| TCP 443 (HTTPS) | İçinde        |            | Bazı durumlarda, yorumlara bakın. | Gelen bağlantılar yalnızca belirli senaryolar için açık olmalıdır. AMQP, MQTT yapılandırılamaz gibi HTTP olmayan protokolleri, bağlantı noktası 443 aracılığıyla iletileri WebSockets üzerinden gönderilebilir. |

Tam bilgi için [güvenlik duvarı ve bağlantı noktası IOT Edge dağıtımı için yapılandırma kuralları](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları

Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamaları veri kutusu Edge cihazınıza ve hizmete bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Bu değişiklikler, izleme ve güvenlik duvarı kuralları, veri kutusu Edge için ve gerektiğinde güncelleştirme için ağ yöneticisine gerektirir.

Veri kutusu liberally çoğu zaman sabit IP adreslerinin, Edge temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - (Kaynak) IP'ler cihaz her zaman tüm bulut özellikli ağ arabirimleri için ayarlanması gerekir.
> - IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).

### <a name="url-patterns-for-gateway-feature"></a>Ağ geçidi özelliği için URL desenleri

|    URL deseni                                                                                                                                                                                                                                                                                                                                                                                                                                                      |    Bileşen veya işlevi                                                                           |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
|    https://\*.databoxedge.azure.com/\*<br>https://\*.servicebus.windows.net/\*<br>https://login.windows.net                                                                                                                                                                                                                                                                                                                                                           |    Azure veri kutusu Edge hizmeti<br>Azure Service Bus<br>Kimlik Doğrulama Hizmeti                           |
|    http://\*. backup.windowsazure.com                                                                                                                                                                                                                                                                                                                                                                                                                                |    Cihaz etkinleştirme                                                                                    |
|    http://crl.microsoft.com/pki/\*<br>http://www.microsoft.com/pki/\*                                                                                                                                                                                                                                                                                                                                                                                                  |    Sertifika iptal etme                                                                               |
|    https://\*.core.windows.net/\*<br>https://\*. data.microsoft.com<br>http://\*. msftncsi.com                                                                                                                                                                                                                                                                                                                                                                            |    Azure depolama hesapları ve izleme                                                                |
|    http://windowsupdate.microsoft.com<br>http://\*. windowsupdate.microsoft.com<br>https://\*. windowsupdate.microsoft.com<br>http://\*. update.microsoft.com<br>https://\*. update.microsoft.com<br>http://\*. windowsupdate.com<br>http://download.microsoft.com<br>http://\*. download.windowsupdate.com<br>http://wustat.windows.com<br>http://ntservicepack.microsoft.com<br>http://\*. ws.microsoft.com<br>https://\*. ws.microsoft.com<br>http://\*. mp.microsoft.com |    Microsoft Update sunucularına                                                                             |
|    http://\*. deploy.akamaitechnologies.com                                                                                                                                                                                                                                                                                                                                                                                                                          |    Akamai CDN                                                                                           |
|    https://\*.partners.extranet.microsoft.com/\*                                                                                                                                                                                                                                                                                                                                                                                                                    |    Destek paketi                                                                                      |
|    http://\*. data.microsoft.com                                                                                                                                                                                                                                                                                                                                                                                                                                     |    Telemetri hizmeti, Windows müşteri deneyimini ve tanılama telemetrisi güncelleştirmesi bakın      |
|                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |                                                                                                         |


### <a name="url-patterns-for-compute-feature"></a>İşlem özelliği için URL desenleri

| URL deseni                      | Bileşen veya işlevi                     |   |
|----------------------------------|---------------------------------------------|---|
| https://mcr.microsoft.com<br></br>https://\*.cdn.mscr.io | Microsoft kapsayıcı kayıt defteri (gerekli)               |   |
| https://\*.azurecr.io                     | Kişisel ve 3. taraf kapsayıcı kayıt defterleri (isteğe bağlı) |   |
| https://\*.azure devices.net              | IOT hub'ı erişim (gerekli)                             |   |

## <a name="internet-bandwidth"></a>Internet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Sonraki adım

* [Azure veri kutusu Ucunuzdaki dağıtma](data-box-Edge-deploy-prep.md)
