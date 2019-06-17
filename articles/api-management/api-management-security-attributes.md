---
title: Azure API yönetimi için güvenlik öznitelikleri
description: API Management'ı değerlendirmek için genel güvenlik öznitelikleri listesi
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 3b5826d472b80179c5eb76e0e3a6b1c7ee282487
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66001083"
---
# <a name="security-attributes-for-api-management"></a>API yönetimi için güvenlik öznitelikleri

Bu makale, API yönetime yerleşik güvenlik özniteliklerini içermektedir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Preventative

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleme sırasında şifreleme:<ul><li>Sunucu tarafı şifrelemesi</li><li>Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifrelemesi</li><li>Diğer şifreleme özellikleri (örneğin, istemci tarafı, her zaman şifreli, vb.)</ul>| Evet (yalnızca hizmet tarafı şifreleme) | Sertifikalar, anahtarları ve gizli adlı değerleri gibi hassas veriler ile şifrelenmiş hizmet tarafından yönetilen, hizmet örneği anahtarlarını başına. |
| Aktarım sırasında şifreleme:<ul><li>Express route şifreleme</li><li>VNet şifreleme</li><li>VNet-VNet şifreleme</ul>| Evet | [Express route](../expressroute/index.yml) ve VNet şifreleme tarafından sağlanır [Azure ağı](../virtual-network/index.yml). |
| Şifreleme anahtarı işleme (CMK, BYOK, vb.)| Hayır | Tüm şifreleme anahtarları Hizmet örneği içindir ve yönetilen bir hizmet. |
| Sütun düzeyinde şifrelemeyi (Azure Data Services)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Yönetim düzlemi çağrılar aracılığıyla yapılan [Azure Resource Manager](../azure-resource-manager/index.yml) TLS üzerinden. Geçerli bir JSON web token (JWT) gereklidir.  Veri düzlemi çağrıları, TLS ve desteklenen kimlik doğrulama mekanizmaları (örneğin, istemci sertifikası veya JWT) ile güvenli hale getirilebilir.
 |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve saldırısından desteği| Evet | Ağ güvenlik grupları (NSG) kullanarak ve Azure Application Gateway (veya diğer yazılım aracı) sırasıyla. |
| Zorlamalı tünel oluşturma desteği| Evet | Azure ağı, zorlamalı tünel sağlar. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analytics, Application ınsights, vb.)| Evet | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | |
| Yetkilendirme| Evet | |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğe kaydetme ve Denetim| Evet | [Azure İzleyici etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md) |
| Veri düzlemi günlük kaydı ve Denetim| Evet | [Azure İzleyici tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) ve (isteğe bağlı olarak) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (sürüm yapılandırması, vs.)| Evet | Kullanarak [Azure API Management DevOps Kaynak Seti](https://aka.ms/apimdevops) |

## <a name="vulnerability-scans-false-positives"></a>Güvenlik Açığı taramaları hatalı pozitif sonuçları

Bu bölümde, Azure API Management etkilemez yaygın güvenlik açıklarına belgeler.

| Güvenlik Açığı               | Açıklama                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Ticketbleed (CVE-2016-9244) | Ticketbleed bazı F5 ürünlerde bulunan TLS SessionTicket uzantısı uygulamasında güvenlik açığı var. ("Kadar 31 bayt başlatılmamış belleği verileri taşmasını") sızıntı sağlar. Bu istemciden 32 bit uzunluğunda olmak için verileri ile geçen bir oturum kimliği doldurma TLS yığın kaynaklanır. |
