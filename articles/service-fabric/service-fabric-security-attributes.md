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
ms.openlocfilehash: f6db40a35c289fa0870ac4c9e5e55b773c84f4f4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59679236"
---
# <a name="common-security-attributes-for-azure-service-fabric"></a>Azure Service Fabric için genel güvenlik öznitelikleri

Güvenlik, bir Azure hizmeti her yönüyle tümleştirilmiştir. Bu makale, Azure Service Fabric'te yerleşik genel güvenlik özniteliklerini içermektedir. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet | Müşteri, küme ve sanal makine ölçek kümesi yerleşik kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>Vnet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet |  |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Evet | Müşteri, küme ve sanal makine ölçek kümesi yerleşik kümesi üstlenir. Sanal makine ölçek kümesinde Azure disk şifrelemesi etkin hale getirilebilir. |
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
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | Azure ve üçüncü taraf desteği izleme kullanıyor. |

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
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Hizmet yapılandırması, oluşturulan ve dağıtılan kullanarak Azure'da dağıtın. (Uygulama ve çalışma zamanı) kullanarak Azure yapı tutulan kodudur.