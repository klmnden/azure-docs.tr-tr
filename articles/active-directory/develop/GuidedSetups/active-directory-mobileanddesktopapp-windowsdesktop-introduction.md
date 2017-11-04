---
title: "Azure AD v2 Windows Masaüstü tıklatmalarını başlatıldı - giriş | Microsoft Docs"
description: "Windows Masaüstü .NET (XAML) uygulamaları, Azure Active Directory v2 bitiş noktası tarafından erişim belirteçleri gerektiren bir API nasıl çağırabilirsiniz"
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
ms.openlocfilehash: 4a695c00fce4deb02261ba58ec95469746bb1486
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Windows Masaüstü uygulamasından Microsoft Graph API çağırma

Bu kılavuz, nasıl bir yerel Windows Masaüstü .NET (XAML) uygulama erişim belirteci almak ve Microsoft Graph API veya Azure Active Directory v2 uç noktasından erişim belirteçleri gerektiren diğer API'leri çağırmak gösterir.

Bu kılavuzun sonunda uygulamanız (outlook.com, live.com ve diğerleri dahil) kişisel hesapları hem de iş kullanan korumalı bir API çağrısı ve Okul hesapları herhangi bir şirket veya Azure Active Directory sahip kuruluş mümkün olacaktır.  

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 gerektirir.  Sahip değilseniz? [Visual Studio 2017 ücretsiz indirme](https://www.visualstudio.com/downloads/)

### <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-mobileanddesktopapp-windowsdesktop-intro/windesktophowitworks.png)

Bu kılavuz tarafından oluşturulan örnek uygulamayı Microsoft Graph API veya Azure Active Directory v2 uç noktasından belirteçleri kabul eder bir Web API sorgulamak bir Windows masaüstü uygulaması sağlar. Bu senaryoda, HTTP isteklerini Authorization Üstbilgisi yoluyla bir belirteç eklenir. Belirteç edinme ve yenileme Microsoft kimlik doğrulama kitaplığı (MSAL) tarafından gerçekleştirilir.


### <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Korumalı Web API'lerine erişmek için belirteç edinme işleme

Kullanıcı kimlik doğrulaması yaptıktan sonra örnek uygulamayı Microsoft Graph API veya Microsoft Azure Active Directory v2 ile güvenli bir Web API sorgulamak için kullanılan bir belirteç alır.

API Microsoft Graph gibi belirli kaynaklara – Örneğin, bir kullanıcının profilini, erişim kullanıcının Takvim okuma ya da bir e-posta göndermek erişim izin vermek için bir erişim belirteci gerektirir. Uygulamanızı API kapsamları belirterek bu kaynaklara erişmek için MSAL kullanarak bir erişim belirteci isteyebilirsiniz. Bu erişim belirteci, ardından HTTP Authorization Üstbilgisi karşı korunan bir kaynağa yapılan her çağrı eklenir. 

Önbelleğe alma ve böylece uygulamanız gerek olmayan erişim belirteçleri, yenileme MSAL yönetir.


### <a name="nuget-packages"></a>NuGet paketleri

Bu kılavuz aşağıdaki NuGet paketlerini kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Microsoft kimlik doğrulama kitaplığı (MSAL)|

