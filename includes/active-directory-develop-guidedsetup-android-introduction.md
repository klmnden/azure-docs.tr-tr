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
ms.openlocfilehash: e25c8d97698d7a61501094a457174202322d146f
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297958"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Oturum açma kullanıcı ve Microsoft Graph API çağrısından bir Android uygulaması

Bu kılavuz, nasıl yerel bir Android uygulaması bir erişim belirteci alın ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırmak gösterir.

Kılavuzu tamamladıktan sonra uygulama oturum açma işlemleri kişisel hesapları (dahil olmak üzere outlook.com, live.com ve diğerleri) ve herhangi bir şirket veya Azure Active Directory kullanan kuruluş iş ve Okul hesaplarını kabul edemiyor olacaktır. Uygulama daha sonra Azure Active Directory v2 bitiş noktası tarafından korunan bir API çağırır.  

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulaması nasıl çalışır
![Bu örnek nasıl çalışır?](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

Bu kılavuz ile oluşturduğunuz örnek uygulama bir Android uygulaması (Microsoft Graph API, bu durumda) Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API sorgulamak için kullanıldığı bir senaryo temel alır. Bu senaryo için uygulamanızı HTTP isteklerini Authorization Üstbilgisi yoluyla alınan simgesi ekler. Microsoft kimlik doğrulama kitaplığı (MSAL), belirteç edinme ve yenileme sizin için işler.

## <a name="prerequisites"></a>Önkoşullar
* Bu Destekli Kurulum Android Studio odaklanmıştır, ancak herhangi bir Android uygulaması geliştirme ortamında da kabul edilebilir. 
* Android SDK 21 veya üstü gereklidir (SDK 25 önerilir).
* Google Chrome veya özel sekmeler kullanan bir web tarayıcısı MSAL bu sürümünde Android için gereklidir.

> [!NOTE]
> Google Chrome Android için Visual Studio öykünücüsü ile dahil değildir. Google Chrome yüklü olan bir öykünücü API 25 veya bir görüntü ile API 21 veya daha yeni bu kodu sınamanızı öneririz.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Korumalı Web API'lerine erişmek için belirteç edinme işleme

Kullanıcının kimliği doğrulandıktan sonra örnek uygulamayı Microsoft Graph API veya Azure Active Directory v2 ile güvenli bir Web API sorgulamak için kullanılan bir erişim belirteci alır.

API Microsoft Graph gibi belirli kaynaklara erişmesine izin vermek için bir erişim belirteci gerektirir. Örneğin, bir erişim belirteci kullanıcı profilini okuma, bir kullanıcının Takvim erişmek veya e-posta göndermek için gereklidir. Uygulamanız, API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından HTTP Authorization Üstbilgisi karşı korunan bir kaynağa yapılan her çağrı eklenir. 

MSAL önbelleğe alma ve erişim belirteçleri, yenileme yönetir uygulamanız gerekmez.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplıklarını kullanır:

|Kitaplık|Açıklama|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft kimlik doğrulama kitaplığı (MSAL)|
