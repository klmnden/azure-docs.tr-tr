---
title: Microsoft Azure veri kutusu ağ geçidi sistem gereksinimleri | Microsoft Docs
description: Yazılım ve Azure veri kutusu ağ geçidi için ağ gereksinimleri hakkında bilgi edinin
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: be51e6c829c5f12d17c6302bb951039711f12075
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113235"
---
# <a name="azure-data-box-gateway-system-requirements-preview"></a>Azure veri kutusu ağ geçidi sistem gereksinimleri (Önizleme)

Bu makalede, Microsoft Azure veri kutusu ağ geçidi çözümünüz için ve Azure veri kutusu ağ geçidine bağlanma istemcileri için önemli sistem gereksinimlerini açıklar. Veri kutusu ağ geçidi dağıtın ve ardından geri gerekirse dağıtım ve sonraki işlemi sırasında başvurduğu önce bilgileri dikkatlice gözden öneririz.

Veri kutusu ağ geçidi sanal cihaz için sistem gereksinimleri şunlardır:

- **Konaklar için yazılım gereksinimleri** -desteklenen platformlar, yerel yapılandırma kullanıcı Arabirimi için tarayıcılar, SMB istemcileri ve cihaz için bağlanan konaklar için tüm ek gereksinimleri açıklanmaktadır.
- **Cihaz için ağ gereksinimleri** -sanal cihazın işlemi için herhangi bir ağ gereksinimleri hakkında bilgi sağlar.

> [!IMPORTANT]
> Data Box Gateway, Önizleme aşamasındadır. Bu çözümü dağıtmadan önce lütfen [Önizleme için kullanım koşullarını](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) gözden geçirin. 

## <a name="specifications-for-the-virtual-device"></a>Sanal cihaz özellikleri

Temel alınan bir konak sistemi veri kutusu ağ geçidi için sanal cihazınızın sağlamak için aşağıdaki kaynakları ayırmanız kuramıyor:

| Belirtimler                                          | Açıklama              |
|---------------------------------------------------------|--------------------------|
| Sanal işlemciler (çekirdekler)   | En az 4 |            
| Bellek  | En az 8 GB|
| Kullanılabilirlik|Tek düğüm|
| Diskler| İşletim sistemi diski: 250 GB <br> Veri diski: 2 TB en, ölçülü kaynak sağlanan ve SSD'ler ile desteklenir|
| Ağ arabirimleri|1 veya daha çok sanal ağ arabirimi|


## <a name="supported-os-for-clients-connected-to-device"></a>Cihaz için bağlanan istemciler için desteklenen işletim sistemi

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Cihaz erişen istemciler için Desteklenen protokoller

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-virtualization-platforms-for-device"></a>Cihaz desteklenen sanallaştırma platformları

| **İşletim sistemi/platform**  |**Sürümleri**   |**Notlar**  |
|---------|---------|---------|
|Hyper-V  |  2012 R2 <br> 2016  |         |
|VMware ESXi     | 6.0 <br> 6.5 <br> 6.7       |VMware araçları desteklenmez.         |


## <a name="supported-storage-accounts"></a>Desteklenen depolama hesapları

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]


## <a name="supported-storage-types"></a>Desteklenen depolama türleri

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Yerel web kullanıcı Arabirimi ile desteklenen tarayıcılar

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Ağ bağlantı noktası gereksinimleri

Aşağıdaki tabloda, SMB, Bulut ve yönetim trafiği için izin vermek için güvenlik duvarını açılması gereken bağlantı noktalarını listeler. Bu tabloda *içinde* veya *gelen* hangi gelen istemci istekleri erişimden Cihazınızı yönü belirtir. *Çıkış* veya *giden* hangi veri kutusu ağ geçidi Cihazınızı gönderir dışarıdan, veri dağıtımı dışında yön ifade eder: Örneğin, Internet'e giden.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

## <a name="url-patterns-for-firewall-rules"></a>URL desenleri için güvenlik duvarı kuralları

Ağ yöneticileri genellikle gelen filtrelemek için URL desenleri ve giden trafiğe göre Gelişmiş güvenlik duvarı kuralları yapılandırabilirsiniz. Veri kutusu ağ geçidi cihazınız ve veri kutusu ağ geçidi hizmeti Azure Service Bus, Azure Active Directory erişim denetimi, depolama hesapları ve Microsoft Update sunucularına gibi diğer Microsoft uygulamaları bağlıdır. Bu uygulamalarla ilişkili URL desenleri, güvenlik duvarı kurallarını yapılandırmak için kullanılabilir. Bu uygulamalarla ilişkili URL desenleri değiştirebilirsiniz anlamak önemlidir. Buna karşılık, izleme ve güvenlik duvarı kuralları, veri kutusu ağ geçidi olarak ve gerektiğinde güncelleştirme için ağ yöneticisine da gerekir.

Veri kutusu liberally çoğu zaman sabit IP adresleri, ağ geçidi temel giden trafik için güvenlik duvarı kurallarınızın ayarlamanızı öneririz. Ancak, güvenli bir ortam oluşturmak için gereken gelişmiş güvenlik duvarı kurallarını ayarlamak için aşağıdaki bilgileri kullanabilirsiniz.

> [!NOTE]
> - (Kaynak) IP'ler cihaz her zaman tüm bulut özellikli ağ arabirimleri için ayarlanması gerekir.
> - IP'ler ayarlanmalıdır hedef [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/confirmation.aspx?id=41653).

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

## <a name="internet-bandwidth"></a>Internet bant genişliği

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Sonraki adım

* [Azure veri kutusu ağ geçidi dağıtma](data-box-gateway-deploy-prep.md)

