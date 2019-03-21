---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: a6fcbc0e8adac75f17d7379ff512ba650d0bb118
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203352"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Microsoft Graph API çağrısından bir JavaScript tek sayfalı uygulama (SPA)

Bu kılavuzda JavaScript tek sayfalı uygulama (SPA) kişisel, iş nasıl kaydolabilirsiniz gösterir ve Okul hesapları, bir erişim belirteci alma ve Microsoft Graph API'si veya Azure Active Directory v2.0 uç noktasından erişim belirteçlerini gerektiren diğer API çağrısı.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro-updated.png)

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bu kılavuzda oluşturulan örnek uygulama, Microsoft Graph API'si veya Azure Active Directory v2.0 uç noktasından belirteçleri kabul eden bir Web API sorgulamak bir JavaScript SPA sağlar. Bu senaryo için bir kullanıcı oturum açtıktan sonra bir erişim belirteci istenen ve HTTP isteklerini yetkilendirme üst bilgisi eklenir. Microsoft Authentication Library (MSAL tarafından) belirteç edinme ve yenileme işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu kılavuz, aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Preview için Microsoft kimlik doğrulama kitaplığı|

> [!NOTE]
> *msal.js* hedefleri *Azure Active Directory v2.0 uç noktası* -oturum açmak ve belirteçleri almak kişisel, okul ve iş hesapları sağlar. *Azure Active Directory v2.0 uç noktası* sahip [bazı sınırlamalar](../articles/active-directory/develop/active-directory-v2-limitations.md).
> Okuma v1.0 ve v2.0 uç noktaları arasındaki farkları [uç nokta karşılaştırma Kılavuzu](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
