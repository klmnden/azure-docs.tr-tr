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
ms.openlocfilehash: f12d11cecbf682ae82f9c432804b1d611ee3e39f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719410"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Azure Service Fabric için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makale, Azure Service Fabric'te yerleşik genel güvenlik özniteliklerini içermektedir. 

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


## <a name="audit-trail"></a>Denetim Kaydı

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | Tüm denetim düzlemi işlemleri ile işlemleri denetleme ve onayları için çalıştırın. |
| Veri düzlemi günlük kaydı ve Denetim| Yok | Müşteri kümesine sahip.  |

## <a name="configuration-management"></a>Yapılandırma Yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | |