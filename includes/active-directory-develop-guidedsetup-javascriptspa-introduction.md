---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: b816d94c8d85ad7d28dfc76072627423a30f6704
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46293574"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Bir JavaScript tek sayfalı uygulama (SPA) Microsoft Graph API çağırma

Bu kılavuzda bir JavaScript tek sayfa uygulama (SPA) kişisel, iş nasıl kaydolabilirsiniz gösterir ve Okul hesapları, bir erişim belirteci alma ve Microsoft Graph API'si veya Azure Active Directory v2 uç noktasından erişim belirteçlerini gerektiren diğer API çağrısı.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bu kılavuzda oluşturulan örnek uygulama, Microsoft Graph API'si veya Azure Active Directory v2 uç noktasından belirteçleri kabul eden bir Web API sorgulamak bir JavaScript SPA sağlar. Bu senaryo için bir kullanıcı oturum açtıktan sonra bir erişim belirteci istenen ve HTTP istekleri aracılığıyla yetkilendirme üst bilgisi eklenir. Microsoft Authentication Library (MSAL tarafından) belirteç edinme ve yenileme işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıkları

Bu kılavuz, aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Preview için Microsoft kimlik doğrulama kitaplığı|

> [!NOTE]
> *msal.js* hedefleri *Azure Active Directory v2 uç noktası* -oturum açmak ve belirteçleri almak kişisel, okul ve iş hesapları sağlar. *Azure Active Directory v2 uç noktası* sahip [bazı sınırlamalar](..\articles\active-directory\develop\active-directory-v2-limitations.md). Yalnızca çalışabilir ve Okul hesaplarında ilgileniyorsanız kullanın *adal.js* ve *V1 uç noktası*. Okuma v1 ve v2 uç noktaları arasındaki farkları [v1-v2 karşılaştırması](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
