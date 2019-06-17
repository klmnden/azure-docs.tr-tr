---
title: Bulut iş ortağı portalı API Başvurusu | Azure Market
description: Açıklaması, kullanmaya yönelik Önkoşullar ve Market API işlemlerin listesi.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 9626fdb5dbdf720a65217421565fc559a0cee684
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935469"
---
<a name="cloud-partner-portal-api-reference"></a>Bulut iş ortağı portalı API Başvurusu
==================================

Bulut iş ortağı portalı REST API'leri, programlı alma ve işleme iş yükleri, teklifler ve yayımcı profilleri izin verir. API'leri, işleme zaman doğru izinleri uygulamak için rol tabanlı erişim denetimi (RBAC) kullanın.

Bu başvuru, bulut iş ortağı portalı REST API'leri için teknik ayrıntıları sağlar. Bu belgedeki yükü örnekleri yalnızca başvuru amaçlıdır ve yeni işlevler eklendikçe değişikliğe uğrayabilir.


<a name="prerequisites-and-considerations"></a>Önkoşullar ve önemli noktalar
-------------------------------

API'leri kullanmadan önce şunları gözden geçirmelisiniz:

- [Önkoşulları](./cloud-partner-portal-api-prerequisites.md) hesabınıza bir hizmet sorumlusu ekleme hakkında bilgi edinmek için makale ve kimlik doğrulaması için Azure Active Directory (Azure AD) erişim belirteci alma. 
- İki [eşzamanlılık denetimi](./cloud-partner-portal-api-concurrency-control.md).
Bu API'leri çağırmak için kullanılabilen stratejiler.
- Ek API [konuları](./cloud-partner-portal-api-considerations.md)gibi sürüm oluşturma ve hata işleme.


<a name="common-tasks"></a>Genel görevler
------------
Bu başvuru aşağıdaki ortak görevleri gerçekleştirmek için API ayrıntıları.


### <a name="offers"></a>Teklifler

-   [Tüm teklifler alma](./cloud-partner-portal-api-retrieve-offers.md)
-   [Belirli bir teklif alma](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Teklif durumunu alma](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Teklif oluşturma](./cloud-partner-portal-api-creating-offer.md)
-   [Teklif yayımlama](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>İşlemler

-   [İşlemleri alma](./cloud-partner-portal-api-retrieve-operations.md)
-   [İşlemleri iptal etme](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Uygulama yayımlama

-   [Canlı yayına geçme](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Diğer görevler

-   [Sanal makine teklifleri için fiyatlandırma ayarlayın](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Sorun giderme

-   [Kimlik doğrulaması sorunlarını giderme](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
