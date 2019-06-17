---
title: Evrensel Windows platformunda dikkat edilecek noktalar (Microsoft kimlik doğrulama kitaplığı .NET için) | Azure
description: Evrensel Windows platformu ile Microsoft kimlik doğrulama kitaplığı .NET (MSAL.NET) kullanırken hakkında belirli değerlendirmeler öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/24/2019
ms.author: ryanwi
ms.reviewer: saeeda
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 83fb999b0cf66cfd8d96e82d23ed43626352a8aa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65544131"
---
# <a name="universal-windows-platform-specific-considerations-with-msalnet"></a>MSAL.NET ile Evrensel Windows platformu özgü konuları
Xamarin iOS, MSAL.NET kullanırken dikkate almanız gereken birkaç nokta vardır.

## <a name="the-usecorporatenetwork-property"></a>UseCorporateNetwork özelliği
WinRT platformunda `PublicClientApplication` Boole şu özelliğe sahip ``UseCorporateNetwork``. Bu özellik, tümleşik Windows kimlik doğrulaması yararlanmak Win8.1 ve UWP uygulamaları etkinleştirir. (ve bu nedenle SSO ile kullanıcı oturum açma işletim sistemi) kullanıcı, oturum açmış bir hesapla Federasyon azure'da ise AD Kiracı. Bu, WAB (Web kimlik doğrulama Aracısı) yararlanır. 

> [!IMPORTANT]
> Bu özelliği true olarak ayarlamak uygulama geliştiricisi uygulamayı tümleşik Windows kimlik doğrulaması (IWA) etkinleştirilmiş olduğu varsayılır. Bunun için:
> - İçinde ``Package.appxmanifest`` UWP uygulamanızın içinde **özellikleri** sekmesinde, aşağıdaki özellikleri sağlar:
>   - Kurumsal kimlik doğrulaması
>   - Özel ağlar (istemci ve sunucu)
>   - Paylaşılan kullanıcı sertifikası

Kurumsal kimlik doğrulaması veya paylaşılan kullanıcı sertifikaları özellikleri isteyen uygulamalar daha yüksek bir Windows Store kabul edilmesi için doğrulama düzeyi gerektirdiğinden IWA varsayılan olarak etkin değildir ve tüm geliştiricilerin yüksek gerçekleştirmek isteyebilirsiniz doğrulama düzeyi. 

Temel uygulama (WAB) UWP platformunda Kurumsal senaryolarda, koşullu erişim etkinleştirdiğiniz düzgün çalışmaz. Kullanıcı Windows hello ile oturum açması çalışır ve bir sertifika, ancak iğnesi için sertifika bulunamadı veya kullanıcının seçtiği seçin, ancak hiçbir zaman PIN istenir için önerilen belirtisidir. Geçici bir çözüm için alternatif bir yöntem kullanmaktır (kullanıcı adı/parola + telefon kimlik doğrulama), ancak deneyimi iyi değil. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki örneklerde, daha fazla ayrıntı sağlanır:

Örnek | Platform | Açıklama 
|------ | -------- | -----------|
|[active-directory-dotnet-native-uwp-v2](https://github.com/azure-samples/active-directory-dotnet-native-uwp-v2) | UWP | Bir kullanıcı Azure AD v2.0 uç noktası ile kimlik doğrulaması için Microsoft Graph erişme, msal.net kullanarak bir evrensel Windows platformu istemci uygulaması. <br>![Topoloji](media/msal-net-uwp-considerations/topology-native-uwp.png)|
|[https://github.com/Azure-Samples/active-directory-xamarin-native-v2](https://github.com/Azure-Samples/active-directory-xamarin-native-v2) | Xamarin iOS, Android, UWP | MSAL MSA ve AAD v2.0 uç noktası aracılığıyla Azure AD kimlik doğrulaması ve Microsoft Graph ile elde edilen belirteç erişmek için nasıl kullanılacağını gösteren basit Xamarin.Forms uygulaması. <br>![Topoloji](media/msal-net-uwp-considerations/topology-xamarin-native.png)|