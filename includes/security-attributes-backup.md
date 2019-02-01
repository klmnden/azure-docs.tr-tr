---
author: msmbaldwin
ms.service: backup
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 5a4cc3ea822c5613fc7a50b8e370d6846b683721
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55513515"
---
## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Depolama hesapları için depolama hizmeti şifrelemesi kullanarak. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Hayır | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |  |
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| vNET ekleme desteği| Hayır |  |
| Ağ yalıtımı / destek özellikleri| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM içinde çalışan iş yükleri için desteklenmiyor. |
| Zorlamalı tünel için destek | Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, App ınsights vb.)| Evet | Log Analytics, tanılama günlükleri desteklenir. Bkz. İzleyici Azure Yedekleme'de korunan Log Analytics kullanarak iş yüklerini (https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) daha fazla bilgi için. |

## <a name="iam-support"></a>IAM desteği

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Erişim yönetimi - kimlik doğrulama| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Erişim Yönetimi - yetkilendirme| Evet | Müşteri oluşturuldu ve yerleşik RBAC rolleri kullanılır. Azure Backup kurtarma noktaları yönetmek için Use role-based erişim denetimi (https://docs.microsoft.com/azure/backup/backup-rbac-rs-vault) daha fazla bilgi için. |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama| Evet | Azure portalından tüm müşteri tetiklenen eylemler, etkinlik günlüklerine kaydedilir. |
| Veri günlük kaydı ve denetim düzlemi| Hayır | Azure Backup veri düzlemi doğrudan ulaşılamıyor.  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm oluşturma, yapılandırma vb.)| Evet|  |