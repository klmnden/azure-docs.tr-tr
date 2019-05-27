---
title: Azure Backup için genel güvenlik öznitelikleri
description: Azure Backup'ı değerlendirmek için genel güvenlik öznitelikleri listesi
services: backup
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 86b8ab96a94a6ffc44c304d8a0a689301560a989
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66002813"
---
# <a name="security-attributes-for-azure-backup"></a>Azure Backup için güvenlik öznitelikleri

Bu makalede, Azure Backup ile yerleşik güvenlik öznitelikleri belgeler. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Depolama hesapları için depolama hizmeti şifrelemesi kullanarak. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Hayır | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM içinde çalışan iş yükleri için desteklenmiyor. |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Log Analytics, tanılama günlükleri desteklenir. Bkz: [İzleyici Azure Yedekleme'de korunan Log Analytics kullanarak iş yüklerini](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) daha fazla bilgi için. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Yetkilendirme| Evet | Müşteri oluşturuldu ve yerleşik RBAC rolleri kullanılır. Bkz: [Use Role-Based erişim denetimi, Azure Backup kurtarma noktaları yönetmek için](/azure/backup/backup-rbac-rs-vault) daha fazla bilgi için. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Azure portalından tüm müşteri tetiklenen eylemler, etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlük kaydı ve Denetim| Hayır | Azure Backup veri düzlemi doğrudan ulaşılamıyor.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet|  |