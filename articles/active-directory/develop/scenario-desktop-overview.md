---
title: Web API'leri (genel bakış) - Microsoft kimlik platformu çağrıları masaüstü uygulaması
description: Bir masaüstü uygulaması oluşturmayı öğrenin çağrıları veritabanını web API'leri (Genel)
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 204e230f7ff0897d2ba97213ebc836aff9d0324b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204642"
---
# <a name="scenario-desktop-app-that-calls-web-apis"></a>Senaryo: Web API'lerini çağıran masaüstü uygulaması

Web API'leri çağıran bir masaüstü uygulaması oluşturmak için gereken her şeyi öğrenin

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="getting-started"></a>Başlarken

Henüz yapmadıysanız, .NET Masaüstü hızlı başlangıç veya UWP hızlı başlangıcı izleyerek ilk uygulamanızı oluşturun:

> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir belirteç almak ve bir Windows Masaüstü uygulamasından Microsoft Graph API çağırma](./quickstart-v2-windows-desktop.md)


> [!div class="nextstepaction"]
> [Hızlı Başlangıç: Bir belirteç almak ve Microsoft Graph API'sini çağırmak bir UWP uygulaması](./quickstart-v2-uwp.md)

## <a name="overview"></a>Genel Bakış

Bir masaüstü uygulaması yazma ve kullanıcılar uygulamanıza oturum açma ve web API'leri gibi Microsoft Graph, diğer Microsoft APIs veya kendi web API'sini çağırmak istiyorsunuz. Birkaç olasılık vardır:

- Masaüstü uygulamanızı grafik denetimleri, örneğin destekleyip desteklemediğini Windows.Form uygulaması veya bir WPF uygulaması ise, etkileşimli belirteç edinme işlemi kullanabilirsiniz.
- Barındırılan Windows uygulamaları için de bir Windows etki alanına katılmış bilgisayarlarda çalışan uygulamalar için olası veya tümleşik Windows kimlik doğrulaması kullanarak sessizce belirteç almak için AAD birleştirilmiş.
- Son olarak, bu önerilmez olsa da, kullanıcı adı/parola genel istemci uygulamalarında kullanabilirsiniz. (DevOps gibi) Bazı senaryolarda yine de gereklidir, ancak bunu kullanarak uygulamanızı kısıtlamaları dayatır dikkatli olun. Örneğin, çok faktörlü kimlik doğrulaması (koşullu erişim) gerçekleştirmek için gereken kullanıcı oturum açamaz. Ayrıca uygulamanıza çoklu oturum açma (SSO) yararlı olmaz.

  Modern kimlik doğrulama ilkelerine karşı da olduğundan ve eski nedenlerle yalnızca sağlanır.

  ![Masaüstü uygulaması](media/scenarios/desktop-app.svg)

- Taşınabilir bir komut satırı aracı - Linux veya Mac üzerinde çalışan bir .NET Core uygulaması büyük olasılıkla yazıyorsanız - ya da etkileşimli kimlik doğrulaması kullanmanız mümkün olmayacaktır, (.NET Core sağlamaz gibi bir [Web tarayıcısı](https://aka.ms/msal-net-uses-web-browser)), veya tümleşik Windows kimlik doğrulaması. Bu durumda en iyi seçenek cihaz kod akışını kullanmaktır. Bu akış ayrıca IOT uygulamaları gibi tarayıcı olmayan uygulamalar için kullanılır

  ![Browserless uygulama](media/scenarios/device-code-flow-app.svg)

## <a name="specifics"></a>Özellikleri

Masaüstü uygulamaları, çoğunlukla olup uygulamanızı etkileşimli kimlik doğrulaması veya kullandığına bağlıdır specificities, birtakım sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Masaüstü uygulaması - uygulama kaydı](scenario-desktop-app-registration.md)
