---
title: Azure Load Balancer için güvenlik öznitelikleri
description: Load Balancer'ı değerlendirmek için güvenlik öznitelikleri listesi
services: load-balancer
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 06/10/2019
ms.author: mbaldwin
ms.openlocfilehash: 1892b918ed35221bc73d5070d5a73ecc359aa4c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67800087"
---
# <a name="security-attributes-for-azure-load-balancer"></a>Azure Load Balancer için güvenlik öznitelikleri

Bu makale, Azure Load Balancer ile oluşturulmuş genel güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| (Örneğin, sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) bekleme sırasında şifreleme | Yok | |
| (Örneğin, ExpressRoute şifreleme, şifreleme sanal ağ ve VNet-VNet şifreleme) aktarım sırasında şifreleme| Yok | |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Yok | |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Aracılığıyla [Azure Resource Manager](../azure-resource-manager/index.yml). |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | . |
| Ağ yalıtımı ve Firewalling desteği| Yok |  |
| Zorlamalı tünel oluşturma desteği| Yok | |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Bkz: [Azure İzleyici için genel temel yük dengeleyici günlüklerini](load-balancer-monitor-log.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Yok |  |
| Authorization| Yok |  |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Bkz: [Azure İzleyici için genel temel yük dengeleyici günlüklerini](load-balancer-monitor-log.md). |
| Veri düzlemi günlük kaydı ve Denetim | Yok |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Yok |  | 
