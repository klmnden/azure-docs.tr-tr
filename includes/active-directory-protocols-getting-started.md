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
ms.openlocfilehash: cb31bb91c80e4d5dd032b009b40d8e3fc435e0c8
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39359468"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uygulamanızı AD kiracınıza kaydetme
İlk olarak, uygulamanızı, Azure Active Directory (Azure AD) kiracısına kaydetmeniz gerekir. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

* [Azure Portal](https://portal.azure.com) oturum açın.
* Üzerine tıklayarak ve ardından sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin **dizini Değiştir** gezinti ve uygun bir kiracı seçin. 
  * Hesabınızın altında yalnızca bir Azure AD kiracısı getirdik veya zaten uygun seçtiyseniz, bu adımı atlayın. Azure AD kiracısı.
* Sol taraftaki gezinti bölmesinde, **Azure Active Directory**’ye tıklayın.
* Tıklayarak **uygulama kayıtları** tıklayın **yeni uygulama kaydı**.
* Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Bu öğretici için oluşturduğunuz uygulamanın web uygulaması veya yerel uygulama olması önemli değildir, ancak web uygulamaları veya yerel uygulamalar için belirli örnekler görmek istiyorsanız [hızlı başlangıç](../articles/active-directory/develop/active-directory-developers-guide.md) bölümümüze bakabilirsiniz.
  * Web uygulamaları için girin **oturum açma URL'si**, kullanıcıların oturum açacağı uygulamanızın temel URL'si olan `http://localhost:12345`.
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Yerel uygulamaları sağlamak için bir **yeniden yönlendirme URI'si**, hangi Azure AD'nin belirteç yanıtlarını döndürmek için kullanacağı. Uygulamanıza özgü bir değer girin, örn. `http://MyFirstAADApp`
* Kayıt tamamlandıktan sonra Azure AD uygulamanızı bir benzersiz istemci tanımlayıcısı atar **uygulama kimliği**. Bu değere ihtiyacınız sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
* Uygulamanızı Azure portalında bulmak için tıklatın **uygulama kayıtları**ve ardından **tüm uygulamaları görüntüle**.
