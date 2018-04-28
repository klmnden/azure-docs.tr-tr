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
ms.openlocfilehash: cf6604a0e22ca72c8aabd0603e42469cc71c9680
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="add-sign-in-with-microsoft-to-an-aspnet-web-app"></a>Oturum açma Microsoft ile bir ASP.NET web uygulamasına ekleme

Bu kılavuz, Openıd Connect kullanarak geleneksel web tarayıcı tabanlı bir uygulama ile ASP.NET MVC çözümünü kullanarak Microsoft ile oturum açma uygulamak gösterilmiştir. 

Bu kılavuzun sonunda uygulamanız kişisel bileşenleri (outlook.com, live.com ve diğerleri dahil) hesaplarının yanı sıra iş oturum kabul edin ve Okul hesapları herhangi bir şirket veya Azure Active Directory ile tümleşik olan Kuruluş mümkün olacaktır. 

> Bu kılavuz, Visual Studio 2015 güncelleştirme 3'ü veya Visual Studio 2017 gerektirir.  Sahip değilseniz?  [Visual Studio 2017 ücretsiz indirme](https://www.visualstudio.com/downloads/)

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Bu kılavuz tarafından oluşturulan örnek uygulaması nasıl çalışır

![Bu kılavuz nasıl çalışır?](media/active-directory-develop-guidedsetup-aspnetwebapp-intro/aspnetbrowsergeneral.png)

Bu kılavuzu tarafından oluşturulan örnek uygulamasının olduğu bir kullanıcı bir oturum açma düğmesi kimlik doğrulaması için bir kullanıcı isteyen bir ASP.NET web sitesine erişmek için tarayıcı kullanır senaryo temel alır. Bu senaryoda, işlerin çoğunu web sayfasını işlemek için sunucu tarafında gerçekleşir.

## <a name="libraries"></a>Kitaplıkları

Bu kılavuz aşağıdaki kitaplıklarını kullanır:

|Kitaplık|Açıklama|
|---|---|
|[Microsoft.Owin.Security.OpenIdConnect](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/)|Openıdconnect kimlik doğrulaması için kullanılacak bir uygulama sağlar Ara|
|[Microsoft.Owin.Security.Cookies](https://www.nuget.org/packages/Microsoft.Owin.Security.Cookies)|Tanımlama bilgilerini kullanarak kullanıcı oturumu korumak bir uygulama sağlar Ara|
|[Microsoft.Owin.Host.SystemWeb](https://www.nuget.org/packages/Microsoft.Owin.Host.SystemWeb)|IIS üzerinde ASP.NET istek ardışık düzenini kullanarak çalıştırmak OWIN tabanlı uygulamalar sağlar|

