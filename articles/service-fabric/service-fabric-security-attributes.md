---
title: Azure Service Fabric için genel güvenlik öznitelikleri
description: Azure Service Fabric değerlendirmek için genel güvenlik öznitelikleri listesi
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 7c1718298c3f7c3fea28fa0b18569085f071696f
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003068"
---
# <a name="security-attributes-for-azure-service-fabric"></a>Azure Service Fabric için güvenlik öznitelikleri

Bu makale, Azure Service Fabric'te yerleşik güvenlik özniteliklerini içermektedir. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Müşteri, küme ve sanal makine ölçek kümesi yerleşik kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet |  |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, küme ve sanal makine ölçek kümesi yerleşik kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları, Azure Resource Manager üzerinden yapılır. Geçerli bir JSON web token (JWT) gereklidir. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet |  |
| Ağ yalıtımı ve saldırısından desteği| Evet | Ağ güvenlik grupları (NSG) kullanarak. |
| Zorlamalı tünel oluşturma desteği| Evet | Azure ağı, zorlamalı tünel sağlar. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure ve üçüncü taraf desteği izleme kullanıyor. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory aracılığıyla kimlik doğrulaması şeklindedir. |
| Yetkilendirme| Evet | Kimlik ve erişim yönetimi (IAM) aracılığıyla SFRP çağrıları için. Doğrudan küme uç noktasına çağrı iki rollerini destekler: Kullanıcı ve yönetici Müşteri API'leri için iki rol eşleyebilirsiniz. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm denetim düzlemi işlemleri ile işlemleri denetleme ve onayları için çalıştırın. |
| Veri düzlemi günlük kaydı ve Denetim| Yok | Müşteri kümesine sahip.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | |