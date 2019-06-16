---
title: Azure Event Hubs için genel güvenlik öznitelikleri
description: Azure Event Hubs'ı değerlendirmek için genel güvenlik öznitelikleri listesi
services: event-hubs
ms.service: event-hubs
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: mbaldwin
ms.openlocfilehash: 50f19d50f211972c2042c69a1e3177ffb4a47624
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66247207"
---
# <a name="common-security-attributes-for-azure-event-hubs"></a>Azure Event Hubs için genel güvenlik öznitelikleri

Bu makalede, Azure Event Hubs'a yerleşik genel güvenlik öznitelikleri belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Evet | |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| vNET ekleme desteği| Hayır | |
| Ağ yalıtımı ve saldırısından desteği| Evet |  |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | |
| Yetkilendirme|  Evet | |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet |  |
| Veri düzlemi günlük kaydı ve Denetim| Evet |   |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | |
