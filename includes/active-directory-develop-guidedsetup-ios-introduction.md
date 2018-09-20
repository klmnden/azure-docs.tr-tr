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
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 09/19/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 93a1772eba291b5141a0bba4ee9a777feb51ab9e
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46466198"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Bir iOS uygulamasından Microsoft Graph API çağırma

Bu kılavuz, bir yerel iOS uygulaması (Swift) Microsoft Azure Active Directory (Azure AD) v2.0 uç noktasından erişim belirteçlerini gerektiren API'leri nasıl çağırabilirsiniz gösterir. Kılavuzu, erişim belirteçleri almak ve bunları Microsoft Graph API ve diğer API çağrıları kullanma açıklanmaktadır.

Bu kılavuz sonraki alıştırmalarda tamamladıktan sonra uygulamanızı herhangi bir şirket veya kuruluş Azure AD'ye sahip korumalı bir API çağırabilirsiniz. Uygulamanızı korumalı API çağrıları, outlook.com, live.com ve diğerleri gibi kişisel hesaplarının yanı sıra, iş veya Okul hesaplarını kullanarak yapabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
- XCode sürüm bu kılavuzda oluşturulan örnek 10.x gereklidir. Xcode'dan indirebileceğiniz [iTunes Web sitesi](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode indirme URL'si").
- [Carthage](https://github.com/Carthage/Carthage) bağımlılık Yöneticisi paket yönetimi için gereklidir.

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.png)

Bu kılavuzda, örnek uygulamayı Microsoft Graph API veya web API'si Azure AD v2.0 uç noktasından belirteçleri kabul eden sorgulamak bir iOS uygulaması sağlar. Bu senaryo için kullanarak bir belirteç HTTP isteklerine eklenen **yetkilendirme** başlığı. Microsoft Authentication Library (MSAL tarafından) belirteç edinme ve yenileme işlenir.


### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Korumalı web API erişimi için tanıtıcı belirteç edinme

Örnek uygulama, kullanıcı kimlik doğrulaması yaptıktan sonra bir belirteç alır. Belirteç, Microsoft Graph API veya web API'si Azure AD v2.0 uç noktası tarafından güvenli hale getirilen sorgulamak için kullanılır.

API'leri, Microsoft Graph gibi belirli kaynaklara erişmesine izin vermek için bir erişim belirteci gerektirir. Belirteçleri, bir kullanıcının profilini okuyun, bir kullanıcının takvime erişip, bir e-posta gönderin ve benzeri için gereklidir. MSAL kullanarak ve API kapsamlarını belirterek, uygulamanız bir erişim belirteci isteyebilir. Erişim belirteci için HTTP eklenir **yetkilendirme** korumalı kaynağa karşı yapılan her çağrı için başlığı.

MSAL, önbelleğe alma ve uygulamanız gerekmez erişim belirteçleri, yenileme yönetir.


## <a name="libraries"></a>Kitaplıkları

Bu kılavuz, aşağıdaki kitaplığı kullanır:

|Kitaplık|Açıklama|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|İOS için Microsoft kimlik doğrulama kitaplığı önizlemesi|

