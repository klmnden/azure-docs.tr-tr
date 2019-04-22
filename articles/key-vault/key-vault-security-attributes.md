---
title: Azure Key Vault için genel güvenlik öznitelikleri
description: Azure Key Vault değerlendirmek için genel güvenlik öznitelikleri listesi
services: key-vault
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: fa36a3c1eb6bda109c7985fa7cade496d2ccf9f4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677808"
---
# <a name="common-security-attributes-for-azure-key-vault"></a>Azure Key Vault için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, Azure Key Vault'a yerleşik genel güvenlik öznitelikleri belgeler. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Tüm nesneleri şifrelenir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Tüm iletişimidir şifrelenmiş API çağrıları |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, anahtar Kasası'nda tüm anahtarları denetler. Donanım Güvenlik Modülü (HSM) yedeklenen anahtarları belirtildiğinde, FIPS Düzey 2 HSM anahtarı, sertifika veya gizli anahtarı korur. |
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
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Log Analytics'i kullanma. |

## <a name="iam-support"></a>IAM desteği

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Erişim yönetimi - kimlik doğrulama| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Erişim Yönetimi - yetkilendirme| Evet | Anahtar kasası erişim ilkesini kullanma. |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Log Analytics'i kullanma. |
| Veri günlük kaydı ve denetim düzlemi| Evet | Log Analytics'i kullanma. |

## <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Anahtar kasası erişim ilkesi |