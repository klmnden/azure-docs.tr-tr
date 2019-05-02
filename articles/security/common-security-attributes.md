---
title: Azure Hizmetleri için güvenlik öznitelikleri
description: Azure Service Fabric değerlendirmek için genel güvenlik öznitelikleri listesi
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: mbaldwin
ms.openlocfilehash: 94681351758f34cc53c425f3207660bdb09f6494
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64717810"
---
# <a name="common-security-attributes-for-azure-services"></a>Azure Hizmetleri için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makalede, seçili Azure Hizmetleri için genel güvenlik öznitelikleri toplar. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="azure-backupbackupbackup-security-attributesmd"></a>[Azure Backup](../backup/backup-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Depolama hesapları için depolama hizmeti şifrelemesi kullanarak. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Hayır | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM içinde çalışan iş yükleri için desteklenmiyor. |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Log Analytics, tanılama günlükleri desteklenir. Bkz. İzleyici Azure Yedekleme'de korunan Log Analytics kullanarak iş yüklerini (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) daha fazla bilgi için. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Yetkilendirme| Evet | Müşteri oluşturuldu ve yerleşik RBAC rolleri kullanılır. Azure Backup kurtarma noktaları yönetmek için Use role-based erişim denetimi (/ azure/yedekleme/yedekleme-rbac-rs-kasa) daha fazla bilgi için. |


### <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Azure portalından tüm müşteri tetiklenen eylemler, etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlük kaydı ve Denetim| Hayır | Azure Backup veri düzlemi doğrudan ulaşılamıyor.  |

### <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet|  |

## <a name="azure-key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Azure Anahtar Kasası.](../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Tüm nesneleri şifrelenir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Tüm iletişimidir şifrelenmiş API çağrıları |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, anahtar Kasası'nda tüm anahtarları denetler. Donanım Güvenlik Modülü (HSM) yedeklenen anahtarları belirtildiğinde, FIPS Düzey 2 HSM anahtarı, sertifika veya gizli anahtarı korur. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | HTTPS kullanarak. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Sanal ağ (VNet) hizmet uç noktaları kullanma. |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | VNet güvenlik duvarı kurallarını kullanarak. |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Log Analytics'i kullanma. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Yetkilendirme| Evet | Anahtar kasası erişim ilkesini kullanma. |


### <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Log Analytics'i kullanma. |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Log Analytics'i kullanma. |

### <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Anahtar kasası erişim ilkesi |

## <a name="azure-service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Azure Service Fabric](../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Müşteri, küme ve küme oluşturulan sanal makine (VM) ölçek kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet |  |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, küme ve küme oluşturulan sanal makine (VM) ölçek kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları, Azure Resource Manager üzerinden yapılır. Geçerli bir JSON web token (JWT) gereklidir. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | Ağ güvenlik grupları (NSG) kullanarak. |
| Zorlamalı tünel oluşturma desteği| Evet | Azure ağı, zorlamalı tünel sağlar. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure ve üçüncü taraf desteği izleme kullanıyor. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Yetkilendirme| Evet | Kimlik ve erişim yönetimi (IAM) aracılığıyla SFRP çağrıları için. Doğrudan küme uç noktasına çağrı iki rollerini destekler: Kullanıcı ve yönetici Müşteri API'leri için iki rol eşleyebilirsiniz. |


### <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm denetim düzlemi işlemleri ile işlemleri denetleme ve onayları için çalıştırın. |
| Veri düzlemi günlük kaydı ve Denetim| Yok | Müşteri kümesine sahip.  |

### <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Hizmet yapılandırması, oluşturulan ve dağıtılan kullanarak Azure'da dağıtın. (Uygulama ve çalışma zamanı) kullanarak Azure yapı tutulan kodudur.
 |

## <a name="azure-storage"></a>Azure Storage

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS mekanizmalarını destekler.  Hizmete aktarılmadan önce kullanıcılar ayrıca veri şifreleyebilirsiniz. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Bkz: [Azure anahtar Kasası'nda müşteri tarafından yönetilen anahtarlar kullanılarak depolama hizmeti şifrelemesi](../storage/common/storage-service-encryption-customer-managed-keys.md).|
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok |  |
| Şifrelenmiş API çağrıları| Evet |  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | |
| Zorlamalı tünel oluşturma desteği| Yok |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure İzleyici ölçümleri artık, başlangıç Önizleme günlükleri |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Yetkilendirme| Evet | RBAC, POSIX ACL ve SAS belirteçleri ile yetkilendirme desteği |


### <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim | Evet | Azure Resource Manager etkinlik günlüğü |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Hizmet tanılama günlükleri ve Azure izleyici günlüğü başlangıç Önizleme  |

### <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Azure Resource Manager API'leri aracılığıyla kaynak sağlayıcısı sürüm desteği |