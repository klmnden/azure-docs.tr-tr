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
ms.openlocfilehash: 71e87a3295a9cd73dca2f97b3fa04a5d5ff76f84
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798682"
---
# <a name="security-attributes-for-azure-sql-database"></a>Azure SQL veritabanı için güvenlik öznitelikleri

Bu makalede, Azure SQL veritabanı'na yerleşik güvenlik öznitelikleri ortak belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

SQL veritabanı hem de içeren [tek veritabanı](sql-database-single-index.yml) ve [yönetilen örnek](sql-database-managed-instance.md). Aşağıdaki girişler dışında tersi belirtilmedikçe her iki teklifleri uygulanır.

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>İstemci tarafı veya Always Encrypted gibi diğer şifreleme özellikleri</ul>| Evet | "Şifreleme kullanımda," makalesinde açıklanan şekilde çağırılır [Always Encrypted](sql-database-always-encrypted.md). Sunucu tarafı şifrelemesini kullanır [saydam veri şifrelemesi](transparent-data-encryption-azure-sql.md).|
| Aktarım sırasında şifreleme:<ul><li>Azure ExpressRoute şifreleme</li><li>Bir sanal ağda şifreleme</li><li>Sanal ağlar arasında şifreleme</ul>| Evet | HTTPS kullanarak. |
| Şifreleme anahtarı CMK veya BYOK gibi işleme| Evet | Hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur. İkinci aracılığıyla sunulan [Azure anahtar kasası](../key-vault/index.yml). |
| Azure Veri Hizmetleri tarafından sağlanan sütun düzeyinde şifreleme| Evet | Aracılığıyla [her zaman şifreli](sql-database-always-encrypted.md). |
| Şifrelenmiş API çağrıları| Evet | HTTPS/SSL kullanma. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Uygulandığı [tek veritabanı](sql-database-single-index.yml) yalnızca. |
| Azure sanal ağ ekleme desteği| Evet | Uygulandığı [yönetilen örnek](sql-database-managed-instance.md) yalnızca. |
| Ağ yalıtım ve güvenlik duvarı desteği| Evet | Hem veritabanı düzeyinde ve sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı içindir [yönetilen örnek](sql-database-managed-instance.md) yalnızca. |
| Zorlamalı tünel oluşturma desteği| Evet | [Yönetilen örnek](sql-database-managed-instance.md) aracılığıyla bir [ExpressRoute](../expressroute/index.yml) VPN. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure desteği, Log Analytics veya Application Insights gibi izleme| Evet | SecureSphere, Imperva, SIEM çözümünden aracılığıyla da desteklenir [Azure Event Hubs](../event-hubs/index.yml) tümleştirme aracılığıyla [SQL denetim](sql-database-auditing.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Azure Active Directory (Azure AD) |
| Authorization| Evet | Yok. |

## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim düzlemi ile yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Yalnızca bazı olaylar için Evet |
| Veri düzlemi günlüğe kaydetme ve Denetim | Evet | Aracılığıyla [SQL denetim](sql-database-auditing.md) |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Sürüm oluşturma yapılandırması gibi yapılandırma yönetimi desteği| Hayır  | None |

## <a name="additional-security-attributes-for-sql-database"></a>SQL veritabanı için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Preventative: güvenlik açığı değerlendirmesi | Evet | Bkz: [veritabanı güvenlik açıklarını tanımlamanıza yardımcı hizmeti SQL güvenlik açığı değerlendirmesi](sql-vulnerability-assessment.md). |
| Preventative: veri bulma ve sınıflandırma  | Evet | Bkz: [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma](sql-database-data-discovery-and-classification.md). |
| Algılama: tehdit algılama | Evet | Bkz: [Gelişmiş tehdit koruması için Azure SQL veritabanı](sql-database-threat-detection-overview.md). |
