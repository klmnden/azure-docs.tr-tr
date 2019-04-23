---
title: Microsoft Azure veri kutusu Edge sistem gereksinimleri | Microsoft Docs
description: Yazılım ve Azure veri kutusu Ucunuzdaki için ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/03/2019
ms.author: alkohli
ms.openlocfilehash: 90c60d586d505ca0c9bd787c37e137f7a38ee1f7
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996756"
---
# <a name="azure-data-box-edge-system-requirements"></a>Azure veri kutusu Edge sistem gereksinimleri

Bu makalede, Microsoft Azure veri kutusu Edge çözümünüz için ve Azure veri kutusu kenarına bağlanan istemciler için önemli sistem gereksinimlerini açıklar. Veri kutusu Ucunuzdaki dağıtmadan önce bilgileri dikkatle gözden öneririz. Yeniden dağıtım ve sonraki işlemi sırasında gerektiğinde bu bilgilere başvurabilir.

Veri kutusu Edge için sistem gereksinimleri şunlardır:

- **Konaklar için yazılım gereksinimleri** -desteklenen platformlar, yerel yapılandırma kullanıcı Arabirimi için tarayıcılar, SMB istemcileri ve cihaz erişen istemciler için tüm ek gereksinimleri açıklanmaktadır.
- **Cihaz için ağ gereksinimleri** -fiziksel cihazın işlemi için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.

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

## <a name="networking-port-requirements"></a>Ağ bağlantı noktası gereksinimleri

### <a name="port-requirements-for-data-box-edge"></a>Veri kutusu Edge için bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB, Bulut ve yönetim trafiği için izin vermek için güvenlik duvarını açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* hangi gelen istemci istekleri erişimden Cihazınızı yönü belirtir. *Çıkış* veya *giden* içinde veri kutusu Edge cihazınıza gönderir dışarıdan, veri dağıtımı dışında Örneğin, internet'e giden yön ifade eder.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

### <a name="port-requirements-for-iot-edge"></a>IOT Edge için bağlantı noktası gereksinimleri

Azure IOT Edge, IOT hub'ı Desteklenen protokoller kullanarak Azure bulutuna bir şirket içi uç CİHAZDAN giden iletişim sağlar. Gelen iletişim istekleri, yalnızca cihaz Mesajlaşma (bulut) Örneğin, Azure IOT Edge cihazı için iletileri göndermek için Azure IOT hub'ı gereken yere belirli senaryolar için gereklidir.

Azure IOT Edge çalışma zamanı barındırma sunucuları için bağlantı noktası yapılandırması için aşağıdaki tabloyu kullanın:

| Bağlantı noktası yok. | Daraltma veya genişletme | Bağlantı noktası kapsamı | Gereklidir | Rehber |
|----------|-----------|------------|----------|----------|
| TCP 443 (HTTPS)| Çıkış       | WAN        | Evet      | IOT Edge sağlama için giden açın. Bu yapılandırma, el ile komut dosyaları veya Azure IOT cihaz sağlama hizmeti (DPS) kullanılırken gereklidir.|

Tam bilgi için [güvenlik duvarı ve bağlantı noktası IOT Edge dağıtımı için yapılandırma kuralları](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları

Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamaları veri kutusu Edge cihazınıza ve hizmete bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Bu değişiklikler, izleme ve güvenlik duvarı kuralları, veri kutusu Edge için ve gerektiğinde güncelleştirme için ağ yöneticisine gerektirir.

Veri kutusu liberally çoğu zaman sabit IP adreslerinin, Edge temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - (Kaynak) IP'ler cihaz her zaman tüm bulut özellikli ağ arabirimleri için ayarlanması gerekir.
> - IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).

### <a name="url-patterns-for-gateway-feature"></a>Ağ geçidi özelliği için URL desenleri

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-feature"></a>İşlem özelliği için URL desenleri

| URL deseni                      | Bileşen veya işlevi                     |   
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.io | Microsoft kapsayıcı kayıt defteri (gerekli)               |
| https://\*.azurecr.io                     | Kişisel ve üçüncü taraf kapsayıcı kayıt defterleri (isteğe bağlı) | 
| https://\*.azure devices.net              | IOT hub'ı erişim (gerekli)                             | 

### <a name="url-patterns-for-gateway-for-azure-government"></a>Azure Kamu'ya yönelik ağ geçidi için URL desenleri

[!INCLUDE [Azure Government URL patterns for firewall](../../includes/data-box-edge-gateway-gov-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-for-azure-government"></a>İşlem, Azure kamu için URL desenleri

| URL deseni                      | Bileşen veya işlevi                     |  
|----------------------------------|---------------------------------------------|
| https:\//mcr.microsoft.com<br></br>https://\*.cdn.mscr.com | Microsoft kapsayıcı kayıt defteri (gerekli)               |
| https://\*.azure devices.us              | IOT hub'ı erişim (gerekli)           |
| https://\*.azurecr.us                    | Kişisel ve üçüncü taraf kapsayıcı kayıt defterleri (isteğe bağlı) | 

## <a name="internet-bandwidth"></a>Internet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="compute-sizing-considerations"></a>Boyutlandırma konuları işlem

Deneyiminizi geliştirmeye ve çözümünüzü test etmeye çalışırken, veri kutusu Edge Cihazınızda yeterli kapasite yoktur ve cihazınızın en iyi performans elde emin olmak için kullanın.

Dikkate almanız gereken faktörler aşağıda verilmiştir:

- **Kapsayıcı özellikleri** -aşağıdaki hakkında düşünün.

    - İş yükünüzü kaç kapsayıcının misiniz? Çok sayıda birkaç kaynak yoğunluklu değerler ve basit bir kapsayıcı olabilir.
    - Bunlar kullandığı kaynakları nelerdir ve bu kapsayıcılar için ayrılan kaynaklar nelerdir?
    - Kaç tane katmanları kapsayıcılarınızı paylaşma?
    - Kullanılmayan kapsayıcı var mı? Durdurulmuş bir kapsayıcı yine de disk alanı kaplar.
    - Kapsayıcılarınızı hangi dilde yazılmış?
- **İşlenen verilerin boyutunu** -ne kadar veri kapsayıcılarınızı olacak işleniyor olabilir? Bu veriler, disk alanı kullanacak veya bellekte veriler işlenir?
- **Beklenen performansı** -çözümünüzü istenen performans özellikleri nelerdir? 

Anlama ve çözümünüzün performansını geliştirmek için kullanabilirsiniz:

- Azure portalında işlem ölçümleri. Veri kutusu Edge kaynağınıza gidin ve ardından Git **İzleme > ölçümleri**. Bakmak **işlem Edge - bellek kullanımı** ve **işlem Edge - CPU yüzdesi** kullanılabilir kaynakları anlamak için ve nasıl kaynaklar.
- İzleme komutları gibi cihazın PowerShell arabirimi üzerinden kullanılabilir:

    - `dkr` kapsayıcılarda Canlı akışı kaynak kullanım istatistiklerini almak için istatistikleri. Komut, CPU, bellek kullanımı, bellek sınırını ve ağ GÇ ölçümleri destekliyor.
    - `dkr system df` kullanılan disk alanı miktarı ile ilgili bilgi almak için. 
    - `dkr image [prune]` kullanılmayan görüntülerini temizleyin ve yer kazanmak için.
    - `dkr ps --size` çalışan kapsayıcıya yaklaşık boyutunu görüntülemek için. 

    Kullanılabilir komutlar hakkında daha fazla bilgi için Git [izleme ve sorun giderme işlem modülleri](data-box-edge-connect-powershell-interface.md#monitor-and-troubleshoot-compute-modules).

Son olarak, veri kümenize çözümünüzü doğrulamak ve üretimde dağıtmadan önce veri kutusu Edge performansını ölçme emin olun.


## <a name="next-step"></a>Sonraki adım

- [Azure veri kutusu Ucunuzdaki dağıtma](data-box-edge-deploy-prep.md)
