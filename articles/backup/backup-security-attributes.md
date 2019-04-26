---
title: Azure Backup için genel güvenlik öznitelikleri
description: Azure Backup'ı değerlendirmek için genel güvenlik öznitelikleri listesi
services: backup
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: cf012a452d1e43134d955e03ccf34e153b1282ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60253812"
---
# <a name="common-security-attributes-for-azure-backup"></a>Azure Backup için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, Azure yedekleme ile oluşturulmuş genel güvenlik öznitelikleri belgeler. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Depolama hesapları için depolama hizmeti şifrelemesi kullanarak. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Hayır | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| vNET ekleme desteği| Hayır |  |
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


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Azure portalından tüm müşteri tetiklenen eylemler, etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlük kaydı ve Denetim| Hayır | Azure Backup veri düzlemi doğrudan ulaşılamıyor.  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet|  |