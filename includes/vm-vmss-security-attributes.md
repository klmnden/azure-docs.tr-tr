---
author: msmbaldwin
ms.service: virtual-machines
ms.topic: include
ms.date: 07/10/2019
ms.author: mbaldwin
ms.openlocfilehash: df11493fa9663d3fcbf0a2f74a5acbead55a25fb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800282"
---
## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Evet | Bkz: [azure'da bir Linux sanal makinesi şifreleme](/azure/virtual-machines/linux/encrypt-disks.md) ve [bir Windows VM'de sanal diskleri şifreleme](/azure/virtual-machines/windows/encrypt-disks.md). |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Evet | Azure sanal makineleri destekleyen [ExpressRoute](/azure/expressroute) ve VNET şifreleme. Bkz: [aktarım sırasında şifreleme vm'lerde](/azure/security/security-azure-encryption-overview.md#in-transit-encryption-in-vms). |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri tarafından yönetilen anahtarlar desteklenen Azure şifreleme senaryosu olduğunu; bkz: [Azure şifrelemeye genel bakış](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms.md).|
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | HTTPS ve SSL. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | |
| VNet ekleme desteği| Evet | . |
| Ağ yalıtımı ve Firewalling desteği| Evet |  |
| Zorlamalı tünel oluşturma desteği| Evet | Bkz: [Azure Resource Manager dağıtım modelini kullanarak zorlamalı tüneli yapılandırma](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Bkz: [izlemek ve güncelleştirmek azure'da bir Linux sanal makinesi](/azure/virtual-machines/linux/tutorial-monitoring.md) ve [izlemek ve güncelleştirmek azure'da Windows sanal makinesi](/azure/virtual-machines/windows/tutorial-monitoring.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet |  |
| Authorization| Evet |  |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet |  |
| Veri düzlemi günlük kaydı ve Denetim | Hayır |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet |  | 
