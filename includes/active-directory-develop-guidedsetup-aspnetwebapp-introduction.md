---
title: include dosyası
description: include dosyası
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 5a1ff10901d10fb274a6fa1418f04e1f20113cb7
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203641"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuzda oturum Openıd Connect'i kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile bir ASP.NET MVC çözümünü kullanarak Microsoft ile nasıl uygulanacağını gösterir.

Bu kılavuzun sonunda, uygulamanızın oturum açma işlemleri kişisel hesabı (outlook.com, live.com ve diğerleri dahil) kabul yanı sıra iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş olacaktır.

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3 veya Visual Studio 2017'yi gerektirir.  Sizde yok mu?  [Visual Studio 2017’yi ücretsiz indirin](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Örnek uygulama tarafından bu öğreticileri çalışır nasıl oluşturulacağını gösterir](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral-updated.png)

Bu kılavuzda oluşturulan örnek uygulamayı nerede isteyen bir kullanıcı oturum açma düğmesi aracılığıyla kimlik doğrulaması için bir ASP.NET web sitesine erişmek için tarayıcıyı bir kullanıcının kullandığı senaryo temel alır. Bu senaryoda web sayfasını oluşturma işlemlerinin çoğu sunucu tarafında gerçekleştirilmektedir.

## <a name="libraries"></a>Kitaplıklar

Bu kılavuz, aşağıdaki kitaplıkların kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Uygulamanın kimlik doğrulaması için OpenIdConnect kullanmasını sağlayan ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Uygulamanın tanımlama bilgilerini kullanarak kullanıcı oturumunu korumasını sağlayan ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|OWIN tabanlı uygulamaların ASP.NET istek işlem hattını kullanarak IIS üzerinde çalışmasını sağlar|
