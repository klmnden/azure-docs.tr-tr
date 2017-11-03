---
title: "Azure AD .NET Protokolü’ne Genel Bakış | Microsoft Docs"
description: "Kiracınızda Azure AD kullanan web uygulamalarına ve web API’lerine erişim izni vermek için HTTP iletilerini kullanma."
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: mbaldwin
editor: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/21/2016
ms.author: priyamo
ms.openlocfilehash: 42ce8dfd30cda7d4085778954350550fd9fdf13d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
## Uygulamanızı AD kiracınıza kaydetme
İlk olarak, uygulamanızı Azure Active Directory (Azure AD) kiracısına kaydetmeniz gerekir. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

* [Azure Portal](https://portal.azure.com)’da oturum açın.
* Sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin.
* Sol taraftaki gezinti bölmesinde, **Azure Active Directory**’ye tıklayın.
* **Uygulama Kayıtları**’na ve **Ekle**’ye tıklayın.
* Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Bu öğretici için oluşturduğunuz uygulamanın web uygulaması veya yerel uygulama olması önemli değildir, ancak web uygulamaları veya yerel uygulamalar için belirli örnekler görmek istiyorsanız [hızlı başlangıç](../articles/active-directory/develop/active-directory-developers-guide.md) bölümümüze bakabilirsiniz.
  * Web Uygulamaları için, kullanıcıların oturum açabileceği temel uygulama URL’si olan **Oturum Açma URL’sini** sağlayın (örn. `http://localhost:12345`).
<!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->
  * Yerel Uygulamalar için, Azure AD’nin belirteç yanıtlarını döndürmek için kullanacağı bir **Yeniden Yönlendirme URI’si** sağlayın. Uygulamanıza özgü bir değer girin, örn. `http://MyFirstAADApp`
* Kayıt tamamlandıktan sonra Azure AD, uygulamanıza benzersiz bir istemci tanımlayıcısı olan Uygulama Kimliği’ni atar. Bu değeri sonraki bölümlerde kullanacağınız için, uygulama sayfasından kopyalayın.
