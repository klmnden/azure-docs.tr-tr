---
title: Azure depolama için genel güvenlik öznitelikleri
description: Azure depolama değerlendirmek için genel güvenlik öznitelikleri listesi
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 7868b52fee991d4b9323fa0b7969aeca4dc83cdb
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64711963"
---
# <a name="common-security-attributes-for-azure-storage"></a>Azure depolama için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, Azure Depolama'ya yerleşik genel güvenlik öznitelikleri belgeler. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS mekanizmalarını destekler.  Hizmete aktarılmadan önce kullanıcılar ayrıca veri şifreleyebilirsiniz. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Bkz: [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | |
| Zorlamalı tünel oluşturma desteği| Yok |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure İzleyici ölçümleri artık, başlangıç Önizleme günlükleri |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Yetkilendirme| Evet | RBAC, POSIX ACL ve SAS belirteçleri ile yetkilendirme desteği |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim | Evet | Azure Resource Manager etkinlik günlüğü |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Hizmet tanılama günlükleri ve Azure izleyici günlüğü başlangıç Önizleme  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Azure Resource Manager API'leri aracılığıyla kaynak sağlayıcısı sürüm desteği |