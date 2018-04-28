---
title: Azure AD .NET Protokolü’ne Genel Bakış | Microsoft Docs
description: Kiracınızda Azure AD kullanan web uygulamalarına ve web API’lerine erişim izni vermek için HTTP iletilerini kullanma.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: 0b78ed6cdb1209d70cf0d561f74cfcddc09b2391
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uygulamanızı AD kiracınıza kaydetme
İlk olarak, uygulamanızı Azure Active Directory (Azure AD) kiracınıza kaydetmeniz gerekir. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

* [Azure Portal](https://portal.azure.com) oturum açın.
* Sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin.
* Sol Gezinti Bölmesi'nde tıklayın **Azure Active Directory**.
* Tıklayın **uygulama kayıtlar** ve tıklayın **yeni uygulama kaydı**.
* Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Bu öğretici için oluşturduğunuz uygulamanın web uygulaması veya yerel uygulama olması önemli değildir, ancak web uygulamaları veya yerel uygulamalar için belirli örnekler görmek istiyorsanız [hızlı başlangıç](../articles/active-directory/develop/active-directory-developers-guide.md) bölümümüze bakabilirsiniz.
  * Web uygulamaları için sağlamak **oturum açma URL'si**, kullanıcılar, örneğin,'burada oturum açabilir, uygulamanızın temel URL olduğu `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Yerel uygulamaları sağlamak için bir **yeniden yönlendirme URI'si**, Azure AD belirteci yanıtları döndürmek için kullanacağı. Uygulamanıza özgü bir değer girin, örn. `http://MyFirstAADApp`
* Kayıt tamamladıktan sonra Azure AD benzersiz istemci tanımlayıcısı uygulamanızı atamak **uygulama kimliği**. Bu değer gereken sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
* Uygulamanızı Azure Portalı'nda bulmak için tıklatın **uygulama kayıtlar**ve ardından **tüm uygulamaları görüntülemek**.