---
title: Ortak güvenlik öznitelikleri için Azure Resource Manager
description: Azure Resource Manager'ı değerlendirmek için genel güvenlik öznitelikleri listesi
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: ebc39dcd9fe921c794add48cc677a799841cbb78
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64943634"
---
# <a name="common-security-attributes-for-azure-resource-manager"></a>Ortak güvenlik öznitelikleri için Azure Resource Manager

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, Azure Resource Manager ile oluşturulmuş genel güvenlik öznitelikleri belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Yok | Azure Resource Manager denetim verilerinin hiçbir müşteri içeriği depolar. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Evet | |
| Şifrelenmiş API çağrıları| Evet | |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve saldırısından desteği| Hayır |  |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Hayır | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | [Azure Active Directory](/azure/active-directory) göre.|
| Yetkilendirme| Evet | |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm yazma işlemlerini (PUT, POST, DELETE), kaynaklarınız üzerinde gerçekleştirilen sunmaya etkinlik günlükleri; bkz: [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](resource-group-audit.md). |
| Veri düzlemi günlük kaydı ve Denetim| Yok | |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet |  |
