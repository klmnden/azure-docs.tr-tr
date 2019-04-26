---
title: Ulusal bulutlarda Azure AD kullanarak kimlik doğrulaması
description: Ulusal Bulutlar için uygulama kaydı ve kimlik doğrulama uç hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: negoe
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/20/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda,CelesteDG
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4feaf97de7b833514113af6c91b3745be0503eff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411122"
---
# <a name="national-clouds"></a>Ulusal Bulutlar

Ulusal Bulutlar (diğer adıyla bağımsız bulutlarda) fiziksel olarak yalıtılmış Azure örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını emin olmak için tasarlanmıştır.

Genel bulut dahil olmak üzere, Azure Active Directory aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure US Government
- Azure Almanya
- Azure Çin 21Vianet

Ulusal Bulutlar Azure genel benzersiz ve farklı ortamında var. Bu nedenle, bazı temel farklar uygulamanızı uygulama kayıt belirteçlerini almak ve uç noktalarını yapılandırma gibi bu ortam için geliştirirken dikkat etmeniz önemlidir.

## <a name="app-registration-endpoints"></a>Uygulama kayıt uç noktaları

Ulusal Bulutlar her biri için ayrı bir Azure portalı yoktur. Microsoft kimlik platformu Ulusal bulut uygulamalarını tümleştirmek için uygulamanızı her ortam için belirli Azure portal'ın ayrı olarak kaydetmek için gereklidir.

Aşağıdaki tabloda temel bir uygulama her Ulusal bulut kaydetmek için kullanılan Azure Active Directory (Azure AD) uç noktaların URL'leri listeler.

| Ulusal bulut | Azure AD portalı uç noktası
| --- | --- |
| ABD kamu için Azure AD |`https://portal.azure.us`
|Azure AD Almanya |`https://portal.microsoftazure.de`
|21Vianet tarafından işletilen Azure AD Çin |`https://portal.azure.cn`
|Azure AD (küresel hizmet)|`https://portal.azure.com` 

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama uç noktaları

Ulusal bulutlarda tüm ayrı ayrı her ortamda kullanıcıların kimliğini doğrulama ve kimlik doğrulama uç sahip.

Aşağıdaki tabloda her Ulusal bulut belirteçlerini almak için kullanılan Azure Active Directory (Azure AD) uç noktalar için temel URL'leri listeler.

| Ulusal bulut | Azure AD kimlik doğrulama uç noktası
| --- | --- |
| ABD kamu için Azure AD |`https://login.microsoftonline.us`
|Azure AD Almanya| `https://login.microsoftonline.de`
|21Vianet tarafından işletilen Azure AD Çin | `https://login.chinacloudapi.cn`
|Azure AD (küresel hizmet)|`https://login.microsoftonline.com`

- Azure AD yetkilendirme veya belirteç uç noktası istekleri uygun bölgeye özgü temel URL'yi kullanarak biçimlendirilmiş olmalıdır. Örneğin, Azure Almanya için:

  - Yetkilendirme ortak uç nokta `https://login.microsoftonline.de/common/oauth2/authorize`.
  - Belirteç ortak uç noktası `https://login.microsoftonline.de/common/oauth2/token`.

- Tek kiracılı uygulamalar için örneğin, ortak Kiracı kimliği veya adı, önceki URL'lerinde Değiştir `https://login.microsoftonline.de/contoso.com`.

>[!NOTE]
> [Azure AD v2.0 yetkilendirme]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) ve belirteç uç noktaları yalnızca küresel hizmet için kullanılabilir. Ulusal bulut dağıtımları için henüz desteklenmiyor.

## <a name="microsoft-graph-api"></a>Microsoft Graph API'si

Arama hakkında bilgi edinmek için Microsoft Graph API'lerini Ulusal bulut ortamında Git [Ulusal bulutta Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/deployments).



> [!IMPORTANT]
> Bazı hizmetler ve küresel hizmet belirli bölgelerde özellikler tüm Ulusal bulutlarda kullanılabilir olmayabilir. Hangi hizmetlerin kullanılabilir olduğunu öğrenmek için Git [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=all&regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia,china-non-regional,china-east,china-east-2,china-north,china-north-2,germany-non-regional,germany-central,germany-northeast).

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure kamu](https://docs.microsoft.com/azure/azure-government/).
- Daha fazla bilgi edinin [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/).
- Daha fazla bilgi edinin [Azure Almanya](https://docs.microsoft.com/azure/germany/).
- Hakkında bilgi edinin [Azure AD kimlik doğrulaması Temelleri](authentication-scenarios.md).
