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
ms.date: 04/11/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 46247d42837f8ac181d33216d2b93d28e2533c09
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66482321"
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuzda oturum Openıd Connect'i kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile bir ASP.NET MVC çözümünü kullanarak Microsoft ile nasıl uygulanacağını gösterir.

Bu kılavuzun sonunda, uygulamanızın oturum açma işlemleri örneğin outlook.com, live.com ve diğer kişisel hesapların kabul etmek üzere mümkün olacaktır. Bu hesaplar dahil iş ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan kuruluş.

> Bu kılavuz, Visual Studio 2019 gerektirir.  Sizde yok mu?  [Visual Studio 2019 ücretsiz olarak indirin](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuzda oluşturulan örnek uygulamasını nasıl çalışır?

![Bu öğretici tarafından oluşturulan örnek uygulamasını nasıl çalıştığını gösterir](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.svg)

Oluşturduğunuz örnek uygulama, tarayıcının bir kullanıcının, bir oturum açma düğmesi aracılığıyla kimlik doğrulaması isteklerini bir ASP.NET web sitesine erişmek için kullandığınız senaryonun temel alır. Bu senaryoda web sayfasını oluşturma işlemlerinin çoğu sunucu tarafında gerçekleştirilmektedir.

## <a name="libraries"></a>Kitaplıklar

Bu kılavuz, aşağıdaki kitaplıkların kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Uygulamanın kimlik doğrulaması için OpenIdConnect kullanmasını sağlayan ara yazılım|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Uygulamanın tanımlama bilgilerini kullanarak kullanıcı oturumunu korumasını sağlayan ara yazılım|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|OWIN tabanlı uygulamaların ASP.NET istek işlem hattını kullanarak IIS üzerinde çalışmasını sağlar|
