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
ms.openlocfilehash: a0d4586df23548854f4acbfefd32081a36906097
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067913"
---
# <a name="national-clouds"></a>Ulusal Bulutlar

Ulusal Bulutlar Azure fiziksel olarak izole edilmiş örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını emin olmak için tasarlanmıştır.

Genel bulut dahil olmak üzere, Azure Active Directory aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure US Government
- Azure Almanya
- Azure Çin 21Vianet

Ulusal Bulutlar Azure genel benzersiz ve farklı ortamında var. Bu nedenle, bazı temel farklar uygulamanızı uygulama kayıt belirteçlerini almak ve uç noktalarını yapılandırma gibi bu ortam için geliştirirken dikkat etmeniz önemlidir.

## <a name="app-registration-endpoints"></a>Uygulama kayıt uç noktaları

Ulusal Bulutlar her biri için ayrı bir Azure portalı yoktur. Microsoft kimlik platformu Ulusal bulut uygulamalarını tümleştirmek için uygulamanızı her ortam için belirli Azure portal'ın ayrı olarak kaydetmek için gereklidir.

Aşağıdaki tabloda temel bir uygulama her Ulusal bulut kaydetmek için kullanılan Azure Active Directory (Azure AD) uç noktaların URL'leri listeler.

| Ulusal bulut | Azure AD portalı uç noktası |
|----------------|--------------------------|
| ABD kamu için Azure AD | `https://portal.azure.us` |
| Azure AD Almanya | `https://portal.microsoftazure.de` |
| 21Vianet tarafından işletilen Azure AD Çin | `https://portal.azure.cn` |
| Azure AD (küresel hizmet) |`https://portal.azure.com` |

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama uç noktaları

Ulusal bulutlarda tüm ayrı ayrı her ortamda kullanıcıların kimliğini doğrulama ve kimlik doğrulama uç sahip.

Aşağıdaki tabloda her Ulusal bulut belirteçlerini almak için kullanılan Azure Active Directory (Azure AD) uç noktalar için temel URL'leri listeler.

| Ulusal bulut | Azure AD kimlik doğrulama uç noktası |
|----------------|-------------------------|
| ABD kamu için Azure AD | `https://login.microsoftonline.us` |
| Azure AD Almanya| `https://login.microsoftonline.de` |
| 21Vianet tarafından işletilen Azure AD Çin | `https://login.chinacloudapi.cn` |
| Azure AD (küresel hizmet)| `https://login.microsoftonline.com` |

- Azure AD yetkilendirme veya belirteç uç noktası istekleri uygun bölgeye özgü temel URL'yi kullanarak biçimlendirilmiş olmalıdır. Örneğin, Azure Almanya için:

  - Yetkilendirme ortak uç nokta `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Belirteç ortak uç noktası `https://login.microsoftonline.de/common/oauth2/token`.

- Tek kiracılı uygulamalar için örneğin, ortak Kiracı kimliği veya adı, önceki URL'lerinde Değiştir `https://login.microsoftonline.de/contoso.com`.

> [!NOTE]
> [Azure AD v2.0 yetkilendirme]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) ve belirteç uç noktaları yalnızca küresel hizmet için kullanılabilir. Ulusal bulut dağıtımları için henüz desteklenmiyor.

## <a name="microsoft-graph-api"></a>Microsoft Graph API'si

Arama hakkında bilgi edinmek için Microsoft Graph API'lerini Ulusal bulut ortamında Git [Ulusal bulutta Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/deployments).

> [!IMPORTANT]
> Bazı hizmetler ve küresel hizmet belirli bölgelerde özellikler tüm Ulusal bulutlarda kullanılabilir olmayabilir. Hangi hizmetlerin kullanılabilir olduğunu öğrenmek için Git [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

İzleyin [Microsoft Authentication Library (MSAL) öğretici](msal-national-cloud.md) Microsoft kimlik platformu kullanarak bir uygulamanın nasıl oluşturulacağını öğrenin. Özellikle, bu uygulama bir kullanıcı, oturum, Microsoft Graph API'sini çağırmak için bir erişim belirteci alın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure Devlet Kurumları](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)
- [Azure AD kimlik doğrulaması temel bilgileri](authentication-scenarios.md)
