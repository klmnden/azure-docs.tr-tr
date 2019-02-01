---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 7ac0072b10489deb1b18ea737301ab698dce9f89
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513508"
---
## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Tüm nesneleri şifrelenir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Tüm iletişimidir şifrelenmiş API çağrıları |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, anahtar Kasası'nda tüm anahtarları denetler. Donanım Güvenlik Modülü (HSM) yedeklenen anahtarları specifiecd olduğunda FIPS Düzey 2 HSM anahtarı, sertifika veya gizli anahtarı korur. |
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | HTTPS kullanarak. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Sanal ağ (Vnet) hizmet uç noktaları kullanma. |
| vNET ekleme desteği| Hayır |  |
| Ağ yalıtımı / destek özellikleri| Evet | Vnet güvenlik duvarı kurallarını kullanarak. |
| Zorlamalı tünel için destek | Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, App ınsights vb.)| Evet | Log Analytics'i kullanma. |

## <a name="iam-support"></a>IAM desteği

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Erişim yönetimi - kimlik doğrulama| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Erişim Yönetimi - yetkilendirme| Evet | Anahtar kasası erişim ilkesini kullanma. |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama| Evet | Log Analytics'i kullanma. |
| Veri günlük kaydı ve denetim düzlemi| Evet | Log Analytics'i kullanma. |

## <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Anahtar kasası erişim ilkesi |