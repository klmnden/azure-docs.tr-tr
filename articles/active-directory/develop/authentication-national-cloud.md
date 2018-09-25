---
title: Ulusal bulutlarda Azure AD kullanarak kimlik doğrulaması
description: Ulusal Bulutlar için uygulama kaydı ve kimlik doğrulama uç hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: negoe
ms.reviewer: negoe,andret,saeeda
ms.custom: aaddev
ms.openlocfilehash: 866a86178d66b7b4af069d684e4eb56c12db47ca
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982012"
---
# <a name="national-clouds"></a>Ulusal Bulutlar

Ulusal Bulutlar (diğer adıyla bağımsız bulutlarda) fiziksel olarak yalıtılmış Azure örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını emin olmak için tasarlanmıştır.

Genel bulut dahil olmak üzere, Azure Active Directory aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure US Government
- Azure Almanya
- Azure Çin 21Vianet

## <a name="app-registration-endpoints"></a>Uygulama kayıt uç noktaları

Aşağıdaki tabloda temel bir uygulama her Ulusal bulut kaydetmek için kullanılan Azure Active Directory (Azure AD) uç noktaların URL'leri listeler.

| Ulusal bulut | Azure AD portalı uç noktası
| --- | --- |
| ABD kamu için Azure AD |https://portal.azure.us
|Azure AD Almanya |https://portal.microsoftazure.de
|21Vianet tarafından işletilen Azure AD Çin |https://portal.azure.cn
|Azure AD (küresel hizmet)|https://portal.azure.com

## <a name="azure-ad-authentication-endpoints"></a>Azure AD kimlik doğrulama uç noktaları

Aşağıdaki tabloda her Ulusal bulut için Microsoft Graph'i çağırmaya yönelik belirteçlerini almak için kullanılan Azure Active Directory (Azure AD) uç noktalar için temel URL'leri listeler.

| Ulusal bulut | Azure AD kimlik doğrulama uç noktası
| --- | --- |
| ABD kamu için Azure AD |`https://login.microsoftonline.us`
|Azure AD Almanya| `https://login.microsoftonline.de`
|21Vianet tarafından işletilen Azure AD Çin | `https://login.chinacloudapi.cn`
|Azure AD (küresel hizmet)|`https://login.microsoftonline.com`

Azure AD yetkilendirme veya belirteç uç noktası istekleri uygun bölgeye özgü temel URL'yi kullanarak biçimlendirilmiş olmalıdır. Örneğin, Almanya durumunda:

- Ortak uç nokta yetkilendirmesi `https://login.microsoftonline.de/common/oauth2/authorize`
- Belirteç ortak uç noktası `https://login.microsoftonline.de/common/oauth2/token` 

Tek kiracılı uygulamalar için genel Kiracı kimliği veya adı, yukarıdaki URL'lerinde değiştirin; Örneğin, `https://login.microsoftonline.de/contoso.com`

>[!NOTE]
> [Azure AD v2.0 yetkilendirme]( https://docs.microsoft.com/azure/active-directory/develop/active-directory-appmodel-v2-overview) ve belirteç uç noktaları yalnızca küresel hizmet için kullanılabilir. Ulusal bulut dağıtımları için henüz desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure kamu](https://docs.microsoft.com/azure/azure-government/)
- Daha fazla bilgi edinin [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- Daha fazla bilgi edinin [Azure Almanya](https://docs.microsoft.com/azure/germany/)
- Hakkında bilgi edinin [Azure AD kimlik doğrulaması temelleri](authentication-scenarios.md)
- Daha fazla bilgi edinin [Ulusal bulutta Microsoft Graph dağıtım](https://developer.microsoft.com/graph/docs/concepts/deployments)