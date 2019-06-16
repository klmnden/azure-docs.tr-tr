---
title: Azure VPN ağ geçidi için güvenlik öznitelikleri
description: Azure VPN ağ geçidi değerlendirmek için güvenlik öznitelikleri listesi
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e58e7b3f77d9bb673e300aa60ad07ca9dba5153
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083138"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Azure VPN ağ geçidi için güvenlik öznitelikleri

Bu makale Azure VPN ağ geçidine yerleşik genel güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Yok | VPN ağ geçidi geçişi Müşteri verilerinin, müşteri verilerini depolamaz |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Evet | VPN ağ geçidi, Azure VPN ağ geçitleri ve müşterilerin şirket içi VPN cihazlarını (S2S) veya (P2S) VPN istemcileri arasındaki müşteri paketleri şifreler. VPN ağ geçitleri de VNet-VNet şifrelemeyi destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır | Müşteri tarafından belirtilen önceden paylaşılan anahtarlar bekleme durumundayken şifrelenir; ancak CMK ile henüz tümleşik değil. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | . |
| Ağ yalıtımı ve Firewalling desteği| Evet | Ayrılmış VM örnekleri her bir müşteri sanal ağ için VPN ağ geçitleri olan  |
| Zorlamalı tünel oluşturma desteği| Evet |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Bkz: [Azure İzleyici tanılama günlükleri/uyarı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure İzleyici ölçümleri/uyarı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) hizmet yönetmek ve Azure VPN ağ geçidini yapılandırma. |
| Yetkilendirme| Evet | Destek ile yetkilendirme [RBAC](../role-based-access-control/overview.md). |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Azure Resource Manager etkinlik günlüğü. |
| Veri düzlemi günlük kaydı ve Denetim | Evet | [Azure İzleyici tanılama günlükleri](../azure-resource-manager/resource-group-audit.md) günlüğe kaydetme ve denetim VPN bağlantısı için. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Yönetim işlemleri için bir Azure VPN ağ geçidi yapılandırması durumu bir Azure Resource Manager şablonu olarak ve tutulan zamanla dışarı aktarılabilir. | 