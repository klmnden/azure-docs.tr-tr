---
title: Azure ExpressRoute için genel güvenlik öznitelikleri
description: Azure ExpressRoute değerlendirmek için genel güvenlik öznitelikleri listesi
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: d6156715fb87831d465197fd8eec59d245221e48
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67083281"
---
# <a name="common-security-attributes-for-azure-expressroute"></a>Azure ExpressRoute için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makale, Azure ExpressRoute ile oluşturulmuş genel güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>|  Yok | ExpressRoute, müşteri verilerini depolamaz. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Hayır | |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Yok |  |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok |  |
| vNET ekleme desteği| Yok | |
| Ağ yalıtımı ve saldırısından desteği| Evet | Her müşteriye kendi yönlendirme etki alanında yer alan ve kendi sanal ağa tünel |
| Zorlamalı tünel oluşturma desteği| Yok | Sınır Ağ Geçidi Protokolü (BGP). |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Bkz: [ExpressRoute izleme, Ölçümler ve Uyarılar](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Ağ geçidi için Microsoft (GWM) (denetleyicisi);'için hizmet hesabı Geliştirme ve OP. için tam zamanında (JIT) erişim |
| Yetkilendirme|  Evet |Ağ geçidi için Microsoft (GWM) (denetleyicisi);'için hizmet hesabı Geliştirme ve OP. için tam zamanında (JIT) erişim |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar| 
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet |  |
| Veri düzlemi günlük kaydı ve Denetim| Hayır |   |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Ağ kaynak sağlayıcısı (NRP). |
