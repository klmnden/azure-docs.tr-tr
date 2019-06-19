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
ms.openlocfilehash: 64accb70561d4c0282b3ee45935d955dba1c67c4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66474544"
---
# <a name="security-attributes-for-azure-services"></a>Azure Hizmetleri için güvenlik öznitelikleri

Bu makalede, seçili Azure Hizmetleri için genel güvenlik öznitelikleri toplar. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="azure-api-managementapi-managementapi-management-security-attributesmd"></a>[Azure API Management](../api-management/api-management-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet (yalnızca hizmet tarafı şifreleme) | Sertifikalar, anahtarları ve gizli adlı değerleri gibi hassas veriler ile şifrelenmiş hizmet tarafından yönetilen, hizmet örneği anahtarlarını başına. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | [Express route](../expressroute/index.yml) ve VNet şifreleme tarafından sağlanır [Azure ağı](../virtual-network/index.yml). |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır | Tüm şifreleme anahtarları Hizmet örneği içindir ve yönetilen bir hizmet. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Yönetim düzlemi çağrılar aracılığıyla yapılan [Azure Resource Manager](../azure-resource-manager/index.yml) TLS üzerinden. Geçerli bir JSON web token (JWT) gereklidir.  Veri düzlemi çağrıları, TLS ve desteklenen kimlik doğrulama mekanizmaları (örn. istemci sertifikası veya JWT) biri ile güvenli hale getirilebilir.
 |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve saldırısından desteği| Evet | Ağ güvenlik grupları (NSG) kullanarak ve Azure Application Gateway (veya diğer yazılım aracı) sırasıyla. |
| Zorlamalı tünel oluşturma desteği| Evet | Azure ağı, zorlamalı tünel sağlar. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | |
| Yetkilendirme| Evet | |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | [Azure İzleyici etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md) |
| Veri düzlemi günlük kaydı ve Denetim| Evet | [Azure İzleyici tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) ve (isteğe bağlı olarak) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Kullanarak [Azure API Management DevOps Kaynak Seti](https://aka.ms/apimdevops) |


## <a name="azure-app-serviceapp-serviceapp-service-security-attributesmd"></a>[Azure uygulama hizmeti](../app-service/app-service-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Evet | Web sitesi dosya içeriği, bekleyen içeriği otomatik olarak şifreler Azure Depolama'da depolanır. Bkz: [bekleyen veriler için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md).<br><br>Müşteri tarafından sağlanan gizli dizileri bekleme durumundayken şifrelenir. Gizli dizileri, App Service yapılandırma veritabanlarını depolanma bekleme sırasında şifrelenir.<br><br>Yerel bağlanmış diskler (D:\local ve % TMP %) Web siteleri tarafından isteğe bağlı olarak geçici depolama alanı olarak kullanılabilir. Yerel bağlanmış diskler bekleme sırasında şifrelenmez. |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Evet | Müşteriler gerektirir ve gelen trafik için HTTPS kullanmak için web siteleri yapılandırabilirsiniz. Blog gönderisine bakın [yalnızca bir Azure uygulama hizmeti HTTPS yapma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteriler, Key Vault'ta uygulama gizli dizilerini depolamak ve bunları çalışma zamanında almak seçebilirsiniz. Bkz: [App Service ve Azure işlevleri'ni (Önizleme) için Key Vault'u başvuran](../app-service/app-service-key-vault-references.md).|
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | App Service'ı yapılandırmak için yönetim çağrılarını ortaya aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml) HTTPS üzerinden çağırır. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Şu anda App Service için önizleme modunda kullanılabilir. Bkz: [Azure App Service'e erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md). |
| VNet ekleme desteği| Evet | App Service ortamları, App Service'in bir müşterinin sanal ağa eklenmiş tek bir müşteriye ayrılmış özel uygulamalarıdır. Bkz: [App Service ortamlarına giriş](../app-service/environment/intro.md). |
| Ağ yalıtımı ve Firewalling desteği| Evet | App Service için genel çok kiracılı farklılığa, müşterilerin ağ ACL'lerini izin verilen gelen trafik kilitlemek için (IP kısıtlamaları) yapılandırabilirsiniz.  Bkz: [Azure App Service'e erişim kısıtlamalarını](../app-service/app-service-ip-restrictions.md).  App Service ortamları, doğrudan sanal ağlara dağıtılan ve bu nedenle Nsg'ler ile güvenli hale getirilebilir. |
| Zorlamalı tünel oluşturma desteği| Evet | App Service ortamları, zorlamalı tünel yapılandırıldığı bir müşterinin sanal ağa dağıtılabilir. Bölümündeki yönergeleri izleyin gereken müşteriler [App Service ortamınızı zorlamalı tünel ile yapılandırma](../app-service/environment/forced-tunnel-support.md). |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | App Service Application ınsights'ı (.NET Framework'ün tamamını, .NET Core, Java ve Node.JS) destekleyen diller için Application Insights ile tümleştirilir.  Bkz: [Performans İzleyicisi'ni Azure App Service](../azure-monitor/app/azure-web-apps.md). App Service, Azure İzleyici ile uygulama ölçümleri de gönderir. Bkz: [Azure App Service'te uygulamaları izleme](../app-service/web-sites-monitor.md). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Müşteriler, otomatik olarak ile tümleşen bir App Service üzerindeki uygulama oluşturabilirsiniz [Azure Active Directory (Azure AD)](../active-directory/index.yml) yanı sıra diğer OAuth uyumlu kimlik sağlayıcıları; Bkz [kimlik doğrulama ve yetkilendirme Azure App Service'e](../app-service/overview-authentication-authorization.md). App Service varlıklarına Management erişimi için tüm erişim sorumlusu Azure AD kimlik doğrulaması ve Azure Resource Manager RBAC rolleri birleşimiyle denetlenir. |
| Yetkilendirme| Evet | App Service varlıklarına Management erişimi için tüm erişim sorumlusu Azure AD kimlik doğrulaması ve Azure Resource Manager RBAC rolleri birleşimiyle denetlenir.  |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | App Service nesneler üzerinde gerçekleştirilen tüm yönetim işlemleri aracılığıyla ortaya [Azure Resource Manager](../azure-resource-manager/index.yml). Bu işlemlerin geçmiş günlükleri, hem portalında hem de CLI aracılığıyla kullanılabilir; bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftweb) ve [az İzleyici etkinlik günlüğü](/cli/azure/monitor/activity-log). |
| Veri düzlemi günlük kaydı ve Denetim | Hayır | App Service için veri düzlemi, bir müşteri s dağıtılan web sitesi içeriği içeren bir uzak dosya paylaşımıdır.  Yoktur uzak dosya paylaşımını denetleme yok. |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Yönetim işlemleri için bir App Service yapılandırma durumu bir Azure Resource Manager şablonu olarak ve tutulan zamanla dışarı aktarılabilir. Çalışma zamanı işlemlerinde, müşterilere App Service dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı Canlı sürümü tutabilirsiniz. | 


## <a name="azure-resource-managerazure-resource-managerazure-resource-manager-security-attributesmd"></a>[Azure Resource Manager](../azure-resource-manager/azure-resource-manager-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet |  |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS/TLS. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Yok | Azure Resource Manager denetim verilerinin hiçbir müşteri içeriği depolar. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Evet | |
| Şifrelenmiş API çağrıları| Evet | |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve saldırısından desteği| Hayır |  |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Hayır | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | [Azure Active Directory](/azure/active-directory) göre.|
| Yetkilendirme| Evet | |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm yazma işlemlerini (PUT, POST, DELETE), kaynaklarınız üzerinde gerçekleştirilen sunmaya etkinlik günlükleri; bkz: [kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/resource-group-audit.md). |
| Veri düzlemi günlük kaydı ve Denetim| Yok | |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet |  |


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


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Azure portalından tüm müşteri tetiklenen eylemler, etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlük kaydı ve Denetim| Hayır | Azure Backup veri düzlemi doğrudan ulaşılamıyor.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

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


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Log Analytics'i kullanma. |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Log Analytics'i kullanma. |

### <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Anahtar kasası erişim ilkesi |

## <a name="azure-service-bus-messagingservice-bus-messagingservice-bus-messaging-security-attributesmd"></a>[Azure Service Bus Mesajlaşması](../service-bus-messaging/service-bus-messaging-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Varsayılan olarak sunucu tarafı şifreleme bekleyen için Evet. | Müşteri tarafından yönetilen anahtarları ve BYOK henüz desteklenmiyor. İstemci tarafı şifreleme istemcinin sorumluluğudur |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Standart HTTPS/TLS düzeneğini destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır |   |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | API çağrıları üzerinden yapılan [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet (yalnızca Premium katman için) | Sanal ağ hizmet uç noktaları için desteklenen [Service Bus Premium katmanı](../service-bus-messaging/service-bus-premium-messaging.md) yalnızca. |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve saldırısından desteği| Evet (yalnızca Premium katman için) |  |
| Zorlamalı tünel oluşturma desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Aracılığıyla desteklenen [Azure İzleyici ve uyarı](../service-bus-messaging/service-bus-metrics-azure-monitor.md). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | İle yönetilen [Azure Active Directory yönetilen hizmet kimliği](../service-bus-messaging/service-bus-managed-service-identity.md); bkz [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md).|
| Yetkilendirme| Evet | Yetkilendirme aracılığıyla destekler [RBAC](../service-bus-messaging/service-bus-role-based-access-control.md) (Önizleme) ve SAS belirteci; bkz [Service Bus kimlik doğrulama ve yetkilendirme](../service-bus-messaging/service-bus-authentication-and-authorization.md). |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | İşlem günlükleri kullanılabilir; bkz: [Service Bus tanılama günlükleri](../service-bus-messaging/service-bus-diagnostic-logs.md).  |
| Veri düzlemi günlük kaydı ve Denetim| Hayır |  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Kaynak sağlayıcısı sürüm aracılığıyla oluşturmayı destekleyen [Azure Resource Manager API'si](/rest/api/resources/).|


## <a name="azure-service-bus-relayservice-bus-relayservice-bus-relay-security-attributesmd"></a>[Azure Service Bus geçişi](../service-bus-relay/service-bus-relay-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Yok | Geçiş, bir web yuvası ve veri devam etmez. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | Hizmet, TLS gerektirir. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır | Yalnızca Microsoft TLS sertifikaları kullanır.  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | HTTPS. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| Ağ yalıtımı ve saldırısından desteği| Hayır |  |
| Zorlamalı tünel oluşturma desteği| Yok | Geçiş TLS tünelidir  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Via SAS. |
| Yetkilendirme|  Evet | Via SAS. |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml). |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Bağlantı başarı / hata ve hataları ve günlüğe kaydedilir.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml).|


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

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm denetim düzlemi işlemleri ile işlemleri denetleme ve onayları için çalıştırın. |
| Veri düzlemi günlük kaydı ve Denetim| Yok | Müşteri kümesine sahip.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Hizmet yapılandırması, oluşturulan ve dağıtılan kullanarak Azure'da dağıtın. (Uygulama ve çalışma zamanı) kullanarak Azure yapı tutulan kodudur.
 |

## <a name="azure-sql-databasesql-databasesql-database-security-attributesmd"></a>[Azure SQL Veritabanı](../sql-database/sql-database-security-attributes.md)

### <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | "Şifreleme kullanımda", makalesinde açıklandığı şekilde başvurulan [Always Encrypted](../sql-database/sql-database-always-encrypted.md). Hizmet tarafı şifreleme kullanan [saydam veri şifrelemesi](../sql-database/transparent-data-encryption-azure-sql.md) (TDE).|
| Aktarım sırasında şifreleme:<ul><li>ExpressRoute şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | HTTPS kullanarak. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur (ikincisi aracılığıyla [Azure anahtar kasası](../key-vault/index.yml). |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Evet | Aracılığıyla [her zaman şifreli](../sql-database/sql-database-always-encrypted.md). |
| Şifrelenmiş API çağrıları| Evet | HTTPS/SSL kullanma. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Uygulandığı [tek veritabanı](../sql-database/sql-database-single-index.yml) yalnızca. |
| VNet ekleme desteği| Evet | Uygulandığı [yönetilen örnek](../sql-database/sql-database-managed-instance.md) yalnızca. |
| Ağ yalıtımı ve saldırısından desteği| Evet | Her iki veritabanı - ve sunucu düzeyinde güvenlik duvarı; ağ yalıtımı için [yönetilen örnek](../sql-database/sql-database-managed-instance.md) yalnızca |
| Zorlamalı tünel oluşturma desteği| Evet | [Yönetilen örnek](../sql-database/sql-database-managed-instance.md) aracılığıyla [Azure ExpressRoute](../expressroute/index.yml) VPN |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Imperva (SecureSphere) üçüncü taraf SIEM çözümünden ayrıca aracılığıyla desteklenir, [Azure Event Hubs](../event-hubs/index.yml) tümleştirme aracılığıyla [SQL denetim](../sql-database/sql-database-auditing.md). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory. |
| Yetkilendirme| Evet |  |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Yalnızca bazı olaylar için Evet. |
| Veri düzlemi günlük kaydı ve Denetim | Evet | Aracılığıyla [SQL denetim](../sql-database/sql-database-auditing.md). |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Hayır  | | 

### <a name="additional-security-attributes-for-sql-database"></a>SQL veritabanı için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Preventative: güvenlik açığı değerlendirmesi | Evet | Bkz: [veritabanı güvenlik açıklarını tanımlamanıza yardımcı hizmeti SQL güvenlik açığı değerlendirmesi](../sql-database/sql-vulnerability-assessment.md). |
| Preventative: veri bulma ve sınıflandırma  | Evet | Bkz: [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırma](../sql-database/sql-database-data-discovery-and-classification.md). |
| Algılama: tehdit algılama | Evet | Bkz: [Gelişmiş tehdit koruması için Azure SQL veritabanı](../sql-database/sql-database-threat-detection-overview.md). |

## <a name="azure-storagestoragecommonstorage-security-attributesmd"></a>[Azure Depolama](../storage/common/storage-security-attributes.md)

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


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim | Evet | Azure Resource Manager etkinlik günlüğü |
| Veri düzlemi günlük kaydı ve Denetim| Evet | Hizmet tanılama günlükleri ve Azure izleyici günlüğü başlangıç Önizleme  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Azure Resource Manager API'leri aracılığıyla kaynak sağlayıcısı sürüm desteği |