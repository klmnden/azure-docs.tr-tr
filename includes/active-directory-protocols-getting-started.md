---
title: Azure AD .NET Protokolü’ne Genel Bakış | Microsoft Docs
description: Kiracınızda Azure AD kullanan web uygulamalarına ve web API’lerine erişim izni vermek için HTTP iletilerini kullanma.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2018
ms.author: priyamo
ms.openlocfilehash: d97aa34f8e76a1ea49841f26534b6a5403e6b5d7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60251636"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uygulamanızı AD kiracınıza kaydetme
İlk olarak, uygulamanızı, Azure Active Directory (Azure AD) kiracısına kaydetmeniz gerekir. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

* [Azure Portal](https://portal.azure.com) oturum açın.
* Üzerine tıklayarak ve ardından sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin **dizini Değiştir** gezinti ve uygun bir kiracı seçin. 
  * Hesabınızın altında tek bir Azure AD kiracısı varsa veya uygun Azure AD kiracısını zaten seçtiyseniz, bu adımı atlayın.
* Sol taraftaki gezinti bölmesinde, **Azure Active Directory**’ye tıklayın.
* Tıklayarak **uygulama kayıtları** tıklayın **yeni uygulama kaydı**.
* Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Bu öğretici için oluşturduğunuz uygulamanın web uygulaması veya yerel uygulama olması önemli değildir, ancak web uygulamaları veya yerel uygulamalar için belirli örnekler görmek istiyorsanız [hızlı başlangıç](../articles/active-directory/develop/v1-overview.md) bölümümüze bakabilirsiniz.
  * Web uygulamaları için girin **oturum açma URL'si**, kullanıcıların oturum açacağı uygulamanızın temel URL'si olan `http://localhost:12345`.
    <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Yerel uygulamaları sağlamak için bir **yeniden yönlendirme URI'si**, hangi Azure AD'nin belirteç yanıtlarını döndürmek için kullanacağı. Uygulamanıza özgü bir değer girin, örn. `http://MyFirstAADApp`
* Kayıt tamamlandıktan sonra Azure AD uygulamanızı bir benzersiz istemci tanımlayıcısı atar **uygulama kimliği**. Bu değere ihtiyacınız sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
* Uygulamanızı Azure portalında bulmak için tıklatın **uygulama kayıtları**ve ardından **tüm uygulamaları görüntüle**.
