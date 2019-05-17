---
title: Azure App Service için güvenlik öznitelikleri
description: Azure App Service'ı değerlendirmek için güvenlik öznitelikleri listesi
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: d22fca27943be7ac7db8b8edd5882b9fa4ab3ab9
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65607266"
---
# <a name="security-attributes-for-azure-app-service"></a>Azure App Service için güvenlik öznitelikleri

Bu makalede, Azure App Service içinde oluşturulmuş genel güvenlik öznitelikleri belgeler.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Evet | Web sitesi dosya içeriği, bekleyen içeriği otomatik olarak şifreler Azure Depolama'da depolanır. Bkz: [bekleyen veriler için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md).<br><br>Müşteri tarafından sağlanan gizli dizileri bekleme durumundayken şifrelenir. Gizli dizileri, App Service yapılandırma veritabanlarını depolanma bekleme sırasında şifrelenir.<br><br>Yerel bağlanmış diskler (D:\local ve % TMP %) Web siteleri tarafından isteğe bağlı olarak geçici depolama alanı olarak kullanılabilir. Yerel bağlanmış diskler bekleme sırasında şifrelenmez. |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Evet | Müşteriler gerektirir ve gelen trafik için HTTPS kullanmak için web siteleri yapılandırabilirsiniz. Blog gönderisine bakın [yalnızca bir Azure uygulama hizmeti HTTPS yapma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteriler, Key Vault'ta uygulama gizli dizilerini depolamak ve bunları çalışma zamanında almak seçebilirsiniz. Bkz: [App Service ve Azure işlevleri'ni (Önizleme) için Key Vault'u başvuran](app-service-key-vault-references.md).|
| Sütun düzeyinde şifrelemeyi (Azure Veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | App Service'ı yapılandırmak için yönetim çağrılarını ortaya aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml) HTTPS üzerinden çağırır. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Şu anda App Service için önizleme modunda kullanılabilir. Bkz: [Azure App Service'e erişim kısıtlamalarını](app-service-ip-restrictions.md). |
| vNET ekleme desteği| Evet | App Service ortamları, App Service'in bir müşterinin sanal ağa eklenmiş tek bir müşteriye ayrılmış özel uygulamalarıdır. Bkz: [App Service ortamlarına giriş](environment/intro.md). |
| Ağ yalıtımı ve Firewalling desteği| Evet | App Service için genel çok kiracılı farklılığa, müşterilerin ağ ACL'lerini izin verilen gelen trafik kilitlemek için (IP kısıtlamaları) yapılandırabilirsiniz.  Bkz: [Azure App Service'e erişim kısıtlamalarını](app-service-ip-restrictions.md).  App Service ortamları, doğrudan sanal ağlara dağıtılan ve bu nedenle Nsg'ler ile güvenli hale getirilebilir. |
| Zorlamalı tünel için destek | Evet | App Service ortamları, zorlamalı tünel yapılandırıldığı bir müşterinin sanal ağa dağıtılabilir. Bölümündeki yönergeleri izleyin gereken müşteriler [App Service ortamınızı zorlamalı tünel ile yapılandırma](environment/forced-tunnel-support.md). |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | App Service Application ınsights'ı (.NET Framework'ün tamamını, .NET Core, Java ve Node.JS) destekleyen diller için Application Insights ile tümleştirilir.  Bkz: [Performans İzleyicisi'ni Azure App Service](../azure-monitor/app/azure-web-apps.md). App Service, Azure İzleyici ile uygulama ölçümleri de gönderir. Bkz: [Azure App Service'te uygulamaları izleme](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Müşteriler, otomatik olarak ile tümleşen bir App Service üzerindeki uygulama oluşturabilirsiniz [Azure Active Directory (Azure AD)](../active-directory/index.md) yanı sıra diğer OAuth uyumlu kimlik sağlayıcıları; Bkz [kimlik doğrulama ve yetkilendirme Azure App Service'e](overview-authentication-authorization.md). App Service varlıklarına Management erişimi için tüm erişim sorumlusu Azure AD kimlik doğrulaması ve Azure Resource Manager RBAC rolleri birleşimiyle denetlenir. |
| Yetkilendirme| Evet | App Service varlıklarına Management erişimi için tüm erişim sorumlusu Azure AD kimlik doğrulaması ve Azure Resource Manager RBAC rolleri birleşimiyle denetlenir.  |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Günlüğe kaydetme ve denetim Denetim/yönetimini planlama| Evet | App Service nesneler üzerinde gerçekleştirilen tüm yönetim işlemleri aracılığıyla ortaya [Azure Resource Manager](../azure-resource-manager/index.yml). Bu işlemlerin geçmiş günlükleri, hem portalında hem de CLI aracılığıyla kullanılabilir; bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftweb) ve [az İzleyici etkinlik günlüğü](/cli/azure/monitor/activity-log). |
| Veri günlük kaydı ve denetim düzlemi | Hayır | App Service için veri düzlemi, bir müşterinin dağıtılan web sitesi içeriğini içeren bir uzak dosya paylaşımıdır.  Yoktur uzak dosya paylaşımını denetleme yok. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Yönetim işlemleri için bir App Service yapılandırma durumu bir Azure Resource Manager şablonu olarak ve tutulan zamanla dışarı aktarılabilir. Çalışma zamanı işlemlerinde, müşterilere App Service dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı Canlı sürümü tutabilirsiniz. | 

