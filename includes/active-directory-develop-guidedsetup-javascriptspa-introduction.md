---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: cfbf4dc08ccea95f99216f6cc82c1ee56d65768a
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36943360"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Bir JavaScript tek sayfa uygulama (SPA) Microsoft Graph API çağrısı

Bu kılavuz bir JavaScript tek sayfa uygulama (SPA) kişisel, iş ne kaydolabilirsiniz gösterir ve Okul hesapları, erişim belirteci almak ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırın.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulaması nasıl çalışır

![Bu kılavuz tarafından oluşturulan örnek uygulaması nasıl çalışır](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Daha Fazla Bilgi

Bu kılavuz tarafından oluşturulan örnek uygulamayı Microsoft Graph API veya Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API sorgulamak bir JavaScript SPA sağlar. Bu senaryo için kullanıcının oturum açtığı sonra bir erişim belirteci istenen ve HTTP isteklerini authorization üstbilgisi yoluyla eklenir. Belirteç edinme ve yenileme Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından işlenir.

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|JavaScript Önizleme için Microsoft kimlik doğrulama kitaplığı|

> [!NOTE]
> *msal.js* hedefleri *Azure Active Directory v2 endpoint* -oturum açmak ve belirteçleri almak kişisel ve iş, okul hesapları sağlar. *Azure Active Directory v2 endpoint* sahip [bazı sınırlamalar](..\articles\active-directory\develop\active-directory-v2-limitations.md). Yalnızca iş ve Okul hesapları ilgileniyorsanız kullanmak *adal.js* ve *V1 endpoint*. Okuma v1 ve v2 uç noktaları arasındaki farkları anlamak için [v1 v2 karşılaştırma](..\articles\active-directory\develop\active-directory-v2-compare.md).

<!--end-collapse-->
