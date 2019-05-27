---
title: Azure Service Bus Mesajlaşması için genel güvenlik öznitelikleri
description: Azure Service Bus Mesajlaşması değerlendirmek için genel güvenlik öznitelikleri listesi
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: d68ffe6561da6a23c288dfabd1d3eb6b34099bb3
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003105"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Azure Service Bus Mesajlaşması için güvenlik öznitelikleri

Bu makale, Azure Service Bus Mesajlaşması ile yerleşik güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Varsayılan olarak sunucu tarafı şifreleme bekleyen için Evet. | Müşteri tarafından yönetilen anahtarları ve BYOK henüz desteklenmiyor. İstemci tarafı şifreleme istemcinin sorumluluğudur |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Standart HTTPS/TLS düzeneğini destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |   |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | API çağrıları üzerinden yapılan [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet (yalnızca Premium katman için) | Sanal ağ hizmet uç noktaları için desteklenen [Service Bus Premium katmanı](service-bus-premium-messaging.md) yalnızca. |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve saldırısından desteği| Evet (yalnızca Premium katman için) |  |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Aracılığıyla desteklenen [Azure İzleyici ve uyarı](service-bus-metrics-azure-monitor.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | İle yönetilen [Azure Active Directory yönetilen hizmet kimliği](service-bus-managed-service-identity.md); bkz [Service Bus kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md).|
| Yetkilendirme| Evet | Yetkilendirme aracılığıyla destekler [RBAC](service-bus-role-based-access-control.md) (Önizleme) ve SAS belirteci; bkz [Service Bus kimlik doğrulama ve yetkilendirme](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | İşlem günlükleri kullanılabilir; bkz: [Service Bus tanılama günlükleri](service-bus-diagnostic-logs.md).  |
| Veri düzlemi günlük kaydı ve Denetim| Hayır |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Kaynak sağlayıcısı sürüm aracılığıyla oluşturmayı destekleyen [Azure Resource Manager API'si](/rest/api/resources/).|
