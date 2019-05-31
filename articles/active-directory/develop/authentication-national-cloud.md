---
title: Ulusal bulutlarda Azure Active Directory'yi kullanarak kimlik doğrulaması
description: Ulusal Bulutlar için uygulama kaydı ve kimlik doğrulama uç hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: negoe,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd37366697a9c1f5019d2864e6d81a4dcd02e3a2
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235482"
---
# <a name="national-clouds"></a>Ulusal Bulutlar

Ulusal Bulutlar Azure fiziksel olarak izole edilmiş örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını emin olmak için tasarlanmıştır.

Genel bulut dahil olmak üzere, Azure Active Directory (Azure AD) aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure Kamu
- Azure Almanya
- Azure Çin 21Vianet

Ulusal Bulutlar benzersizdir ve Azure genel ayrı bir ortam. Bu ortamlar için uygulamanızı geliştirmeye devam ederken önemli farklılıklara dikkat edin önemlidir. Uygulamaları kaydetme, belirteçlerini almak ve uç noktaları yapılandırma farklılıkları içerir.

## <a name="app-registration-endpoints"></a>Uygulama kayıt uç noktaları

Ulusal Bulutlar her biri için ayrı bir Azure portalı yoktur. Microsoft kimlik platformu Ulusal bulut uygulamalarını tümleştirmek için ayrı ayrı her Azure Portal'da ortama özgü kaydedin isteniyor.

Aşağıdaki tabloda temel bir uygulama her Ulusal bulut kaydetmek için kullanılan Azure AD uç noktaların URL'leri listeler.

| Ulusal bulut | Azure AD portalı uç noktası |
|----------------|--------------------------|
| ABD kamu için Azure AD | `https://portal.azure.us` |
| Azure AD Almanya | `https://portal.microsoftazure.de` |
| 21Vianet tarafından işletilen Azure AD Çin | `https://portal.azure.cn` |
| Azure AD (küresel hizmet) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama uç noktaları

Ulusal bulutlarda tüm ayrı ayrı her ortamda kullanıcıların kimliğini doğrulama ve kimlik doğrulama uç sahip.

Aşağıdaki tabloda her Ulusal bulut belirteçlerini almak için kullanılan Azure AD uç noktalar için temel URL'leri listeler.

| Ulusal bulut | Azure AD kimlik doğrulama uç noktası |
|----------------|-------------------------|
| ABD kamu için Azure AD | `https://login.microsoftonline.us` |
| Azure AD Almanya| `https://login.microsoftonline.de` |
| 21Vianet tarafından işletilen Azure AD Çin | `https://login.chinacloudapi.cn` |
| Azure AD (küresel hizmet)| `https://login.microsoftonline.com` |

İlgili bölgeye özgü temel URL'ı kullanarak Azure AD yetkilendirme veya belirteç uç noktaları için istek oluşturabilir. Örneğin, Azure Almanya için:

  - Yetkilendirme ortak uç nokta `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Belirteç ortak uç noktası `https://login.microsoftonline.de/common/oauth2/token`.

Tek kiracılı uygulamalar için "Genel" önceki URL'lerde Kiracı kimliği veya adı ile değiştirin. `https://login.microsoftonline.de/contoso.com` bunun bir örneğidir.

> [!NOTE]
> [Azure AD v2.0 yetkilendirme]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) ve belirteç uç noktaları yalnızca küresel hizmet için kullanılabilir. Bunlar, Ulusal bulut dağıtımları için desteklenmiyor.

## <a name="microsoft-graph-api"></a>Microsoft Graph API'si

Ulusal bulut ortamında Microsoft Graph API'lerini çağırma öğrenmek için Git [Ulusal bulut dağıtımlarında Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bazı hizmetler ve küresel hizmet belirli bölgelerde özellikler tüm Ulusal bulutlarda kullanılabilir olmayabilir. Hangi hizmetlerin kullanılabilir olduğunu öğrenmek için Git [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

Microsoft kimlik platformu kullanarak bir uygulama oluşturmayı öğrenmek için izleyin [Microsoft Authentication Library (MSAL) öğretici](msal-national-cloud.md). Özellikle, bu uygulama bir kullanıcının oturum açmasını ve Microsoft Graph API'sini çağırmak için bir erişim belirteci alın.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi:

- [Azure Devlet Kurumları](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)
- [Azure AD kimlik doğrulaması temel bilgileri](authentication-scenarios.md)
