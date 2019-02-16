---
author: msmbaldwin
ms.service: service-fabric
ms.topic: include
ms.date: 01/31/2019
ms.author: mbaldwin
ms.openlocfilehash: 179d87a0c1af587148f1b5ffa2cad8085ef0886f
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56306835"
---
## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Müşteri, küme ve küme oluşturulan sanal makine (VM) ölçek kümesi üstlenir. Azure disk şifrelemesi, VM ölçek kümesinde etkinleştirilebilir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet |  |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, küme ve küme oluşturulan sanal makine (VM) ölçek kümesi üstlenir. Azure disk şifrelemesi, VM ölçek kümesinde etkinleştirilebilir. |
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları, Azure Resource Manager üzerinden yapılır. Geçerli bir JSON web token (JWT) gereklidir. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| vNET ekleme desteği| Evet |  |
| Ağ yalıtımı / destek özellikleri| Evet | Ağ güvenlik grupları (NSG) kullanarak. |
| Zorlamalı tünel için destek | Evet | Azure ağı, zorlamalı tünel sağlar. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, App ınsights vb.)| Evet | Azure ve üçüncü taraf desteği izleme kullanarak. |

## <a name="iam-support"></a>IAM desteği

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Erişim yönetimi - kimlik doğrulama| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Erişim Yönetimi - yetkilendirme| Evet | Kimlik ve erişim yönetimi (IAM) aracılığıyla SFRP çağrıları için. Doğrudan küme uç noktasına çağrı iki rollerini destekler: Kullanıcı ve yönetici Müşteri API'leri için iki rol eşleyebilirsiniz. |


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama| Evet | Tüm denetim düzlemi işlemleri ile işlemleri denetleme ve onayları için çalıştırın. |
| Veri günlük kaydı ve denetim düzlemi| Yok | Müşteri kümesine sahip.  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm oluşturma, yapılandırma vb.)| Evet | Hizmet yapılandırması, oluşturulan ve dağıtılan kullanarak Azure'da dağıtın. (Uygulama ve çalışma zamanı) kullanarak Azure yapı tutulan kodudur.
 |
