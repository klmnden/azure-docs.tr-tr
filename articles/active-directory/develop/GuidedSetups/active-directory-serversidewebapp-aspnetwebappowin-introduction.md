---
title: "Azure AD v2 ASP.NET Web sunucusu alma başlatıldı - giriş | Microsoft Docs"
description: "Microsoft oturum açma Openıd Connect standardını kullanan geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET çözümünü uygulama"
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
ms.openlocfilehash: 8062923b6270ec6253dc231a3db4333cf4666b42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuz, Openıd Connect kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET MVC çözümünü kullanarak Microsoft ile oturum açma uygulamak gösterilmiştir. 

Bu kılavuzun sonunda uygulamanız kişisel bileşenleri (outlook.com, live.com ve diğerleri dahil) hesaplarının yanı sıra iş oturum kabul edin ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş mümkün olacaktır. 

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 gerektirir.  Sahip değilseniz?  [Visual Studio 2017 ücretsiz indirme](https://www.visualstudio.com/downloads/)

## <a name="how-this-guide-works"></a>Bu kılavuz nasıl çalışır?

![Bu kılavuz nasıl çalışır?](media/active-directory-serversidewebapp-aspnetwebappowin-intro/aspnetbrowsergeneral.png)

Bu kılavuz, bir oturum açma düğmesi kimlik doğrulaması için bir kullanıcı isteyen burada bir tarayıcı bir ASP.NET web sitesine erişen senaryosunda temel alır. Bu senaryoda, işlerin çoğunu web sayfasını işlemek için sunucu tarafında gerçekleşir.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplıklarını kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Openıdconnect kimlik doğrulaması için kullanılacak bir uygulama sağlar Ara|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Tanımlama bilgilerini kullanarak kullanıcı oturumu korumak bir uygulama sağlar Ara|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|IIS üzerinde ASP.NET istek ardışık düzenini kullanarak çalıştırmak OWIN tabanlı uygulamalar sağlar|

