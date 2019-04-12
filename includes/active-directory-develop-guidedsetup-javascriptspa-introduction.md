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
ms.date: 04/11/2019
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 8c2dc41fde9387da291b6e4a6c8a6220ae62b514
ms.sourcegitcommit: 41015688dc94593fd9662a7f0ba0e72f044915d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59503140"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Kullanıcılar oturum ve bir JavaScript tek sayfalı uygulama (SPA) Microsoft Graph API çağırma

Bu kılavuz, nasıl bir JavaScript tek sayfalı uygulama (SPA) kişisel, iş oturum açabilirsiniz ve Okul hesapları, bir erişim belirteci alma ve Microsoft Graph API'sini veya Microsoft kimlik platformu uç noktasından erişim belirteçlerini gerektiren diğer API'leri çağırmak gösterir.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.svg)

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bu kılavuzda oluşturulan örnek uygulama, Microsoft Graph API'sini veya Microsoft kimlik platformu uç noktasından belirteçleri kabul eden bir Web API sorgulamak bir JavaScript SPA sağlar. Bu senaryo için bir kullanıcı oturum açtıktan sonra bir erişim belirteci istenen ve HTTP isteklerini yetkilendirme üst bilgisi eklenir. Microsoft Authentication Library (MSAL tarafından) belirteç edinme ve yenileme işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıklar

Bu kılavuz, aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Preview için Microsoft kimlik doğrulama kitaplığı|

> [!NOTE]
> *msal.js* hedefleri *Microsoft kimlik platformu uç nokta* -oturum açmak ve belirteçleri almak kişisel, okul ve iş hesapları sağlar. *Microsoft kimlik platformu uç nokta* sahip [bazı sınırlamalar](../articles/active-directory/develop/active-directory-v2-limitations.md).
> Okuma v1.0 ve v2.0 uç noktaları arasındaki farkları [uç nokta karşılaştırma Kılavuzu](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
