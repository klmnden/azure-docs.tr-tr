---
title: Azure Service Bus geçişi için ortak güvenlik öznitelikleri
description: Azure Service Bus geçişi değerlendirmek için genel güvenlik öznitelikleri listesi
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: f8827ac290393c9f394c3b13149555a1a2aa6df9
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64927500"
---
# <a name="common-security-attributes-for-azure-service-bus-relay"></a>Azure Service Bus geçişi için ortak güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makale, Azure Service Bus geçişi içinde oluşturulan genel güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Yok | Geçiş, bir web yuvası ve veri devam etmez. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Hizmet, TLS gerektirir. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır | Yalnızca Microsoft TLS sertifikaları kullanır.  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | HTTPS. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| Ağ yalıtımı ve saldırısından desteği| Hayır |  |
| Zorlamalı tünel oluşturma desteği| Yok | Geçiş TLS tünelidir  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Via SAS. |
| Yetkilendirme|  Evet | Via SAS. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Bağlantı başarı / hata ve hataları ve günlüğe kaydedilir.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml).|
