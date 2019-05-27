---
title: Azure SQL veritabanı için güvenlik öznitelikleri
description: Azure SQL veritabanı değerlendirmek için güvenlik öznitelikleri listesi
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 6c495456a5a3295abe5460ff6b5586e41fab2d95
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001032"
---
# <a name="security-attributes-for-azure-sql-database"></a>Azure SQL veritabanı için güvenlik öznitelikleri

Bu makale, Azure SQL veritabanında yerleşik genel güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

Azure SQL veritabanı hem de içeren [tek veritabanı](sql-database-single-index.yml) ve [yönetilen örnek](sql-database-managed-instance.md). Aşağıdaki girişler için her iki teklifleri, dışında aksi belirtilmedikçe uygulayın.

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | "Şifreleme kullanımda", makalesinde açıklandığı şekilde başvurulan [Always Encrypted](sql-database-always-encrypted.md). Hizmet tarafı şifreleme kullanan [saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md) (TDE).|
| Aktarım sırasında şifreleme:<ul><li>ExpressRoute şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur (ikincisi aracılığıyla [Azure anahtar kasası](../key-vault/index.yml). |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Evet | Aracılığıyla [her zaman şifreli](sql-database-always-encrypted.md). |
| Şifrelenmiş API çağrıları| Evet | HTTPS/SSL kullanma. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Uygulandığı [tek veritabanı](sql-database-single-index.yml) yalnızca. |
| VNet ekleme desteği| Evet | Uygulandığı [yönetilen örnek](sql-database-managed-instance.md) yalnızca. |
| Ağ yalıtımı ve saldırısından desteği| Evet | Her iki veritabanı - ve sunucu düzeyinde güvenlik duvarı; ağ yalıtımı için [yönetilen örnek](sql-database-managed-instance.md) yalnızca |
| Zorlamalı tünel oluşturma desteği| Evet | [Yönetilen örnek](sql-database-managed-instance.md) aracılığıyla [Azure ExpressRoute](../expressroute/index.yml) VPN |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Imperva (SecureSphere) üçüncü taraf SIEM çözümünden ayrıca aracılığıyla desteklenir, [Azure Event Hubs](../event-hubs/index.yml) tümleştirme aracılığıyla [SQL denetim](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory. |
| Yetkilendirme| Evet |  |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Yalnızca bazı olaylar için Evet. |
| Veri düzlemi günlük kaydı ve Denetim | Evet | Aracılığıyla [SQL denetim](sql-database-auditing.md). |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Hayır  | | 

## <a name="additional-security-attributes-for-sql-database"></a>SQL veritabanı için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Preventative: güvenlik açığı değerlendirmesi | Evet | Bkz: [veritabanı güvenlik açıklarını tanımlamanıza yardımcı hizmeti SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md). |
| Preventative: veri bulma ve sınıflandırma  | Evet | Bkz: [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md). |
| Algılama: tehdit algılama | Evet | Bkz: [Gelişmiş tehdit koruması için Azure SQL veritabanı](sql-database-threat-detection-overview.md). |
