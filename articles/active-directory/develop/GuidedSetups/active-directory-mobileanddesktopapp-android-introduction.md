---
title: "Azure AD v2 Android alma başlatıldı - giriş | Microsoft Docs"
description: "Nasıl bir Android uygulaması bir erişim belirteci alın ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren API'larını çağırma"
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: d933781713456f73aa76db557fdf35672dfb2a29
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="call-the-microsoft-graph-api-from-an-android-app"></a>Bir Android uygulaması Microsoft Graph API çağrısı

Bu kılavuz, nasıl yerel bir Android uygulaması bir erişim belirteci alın ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırmak gösterir.

Bu kılavuzun sonunda uygulamanız (outlook.com, live.com ve diğerleri dahil) kişisel hesapları hem de iş kullanan korumalı bir API çağrısı ve Okul hesapları herhangi bir şirket veya Azure Active Directory sahip kuruluş mümkün olacaktır.  

### <a name="how-this-sample-works"></a>Bu örnek nasıl çalışır?
![Bu örnek nasıl çalışır?](media/active-directory-mobileanddesktopapp-android-intro/android-intro.png)

Bu kılavuz tarafından oluşturulan örnek Android uygulama Azure Active Directory v2 uç noktasından – bu durumda, Microsoft Graph API belirteçleri kabul eder bir Web API sorgulamak için kullanıldığı bir senaryo temel alır. Bu senaryoda, HTTP isteklerini Authorization Üstbilgisi yoluyla bir belirteç eklenir. Belirteç edinme ve yenileme Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından gerçekleştirilir.

### <a name="pre-requisites"></a>Ön koşullar
* Bu Destekli Kurulum Android Studio odaklanmıştır, ancak herhangi bir Android uygulaması geliştirme ortamında da kabul edilebilir. 
* Android SDK 21 ya da daha yeni gereklidir (SDK 25 önerilir).
* Google Chrome veya özel sekmeleri kullanarak bir web tarayıcısı Android için bu sürüm Microsoft kimlik doğrulama kitaplığı (MSAL) için gereklidir.

> Not: Google Chrome Android için Visual Studio öykünücüsü bulunmaz. Bu kodu, Google Chrome yüklediği bir öykünücü API 25 veya bir görüntü ile API 21 ile veya test etmenizi öneririz.


### <a name="how-to-handle-token-acquisition-to-access-a-protected-web-api"></a>Korumalı bir Web API erişmek için belirteç edinme nasıl ele alınacağını

Kullanıcı kimlik doğrulaması yaptıktan sonra örnek uygulamayı Microsoft Graph API veya Microsoft Azure Active Directory v2 ile güvenli bir Web API sorgulamak için kullanılan bir belirteç alır.

API Microsoft Graph gibi belirli kaynaklara – Örneğin, bir kullanıcının profilini, erişim kullanıcının Takvim okuma ya da bir e-posta göndermek erişim izin vermek için bir erişim belirteci gerektirir. Uygulamanızı API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından HTTP Authorization Üstbilgisi karşı korunan bir kaynağa yapılan her çağrı eklenir. 

Önbelleğe alma ve böylece uygulamanız gerek olmayan erişim belirteçleri, yenileme MSAL yönetir.

### <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplıklarını kullanır:

|Kitaplık|Açıklama|
|---|---|
|[com.microsoft.identity.Client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Microsoft kimlik doğrulama kitaplığı (MSAL)|
