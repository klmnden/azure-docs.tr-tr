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
ms.openlocfilehash: 33765d57f1a0e5788011b2d9d9c2f57d06713ddb
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680698"
---
# <a name="common-security-attributes-for-azure-storage"></a>Azure depolama için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, Azure Depolama'ya yerleşik genel güvenlik öznitelikleri belgeler. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS mekanizmalarını destekler.  Hizmete aktarılmadan önce kullanıcılar ayrıca veri şifreleyebilirsiniz. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Bkz: [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
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
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure İzleyici ölçümleri artık, başlangıç Önizleme günlükleri |

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
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Azure Resource Manager API'leri aracılığıyla kaynak sağlayıcısı sürüm desteği |