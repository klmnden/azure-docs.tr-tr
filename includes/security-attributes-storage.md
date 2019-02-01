---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: d8e33113ca9f0886a4cef1c8f9acb855b32c2973
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513550"
---
## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS mekanizmalarını destekler.  Hizmete aktarılmadan önce kullanıcılar ayrıca veri şifreleyebilirsiniz. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Bkz: [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](../articles/storage/common/storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| vNET ekleme desteği| Yok |  |
| Ağ yalıtımı / destek özellikleri| Evet | |
| Zorlamalı tünel için destek | Yok |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, App ınsights vb.)| Evet | Azure İzleyici ölçümleri artık, başlangıç Önizleme günlükleri |

## <a name="iam-support"></a>IAM desteği

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Erişim yönetimi - kimlik doğrulama| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Erişim Yönetimi - yetkilendirme| Evet | RBAC, POSIX ACL ve SAS belirteçleri ile yetkilendirme desteği |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama | Evet | Azure Resource Manager etkinlik günlüğü |
| Veri günlük kaydı ve denetim düzlemi| Evet | Hizmet tanılama günlükleri ve Azure izleyici günlüğü başlangıç Önizleme  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm oluşturma, yapılandırma vb.)| Evet | Azure Resource Manager API'leri aracılığıyla kaynak sağlayıcısı sürüm desteği |