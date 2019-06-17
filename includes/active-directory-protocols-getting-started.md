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
ms.date: 05/22/2019
ms.author: priyamo
ms.openlocfilehash: b6dd4cd55755ae2c92afd327ad72ffe6966b9a07
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121526"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Uygulamanızı AD kiracınıza kaydetme
İlk olarak, uygulamanızı, Azure Active Directory (Azure AD) kiracısına kaydetmeniz gerekir. Bu, uygulamanıza bir Uygulama Kimliği verir ve uygulamanızın belirteçleri alabilmesini sağlar.

* [Azure Portal](https://portal.azure.com) oturum açın.
* Üzerine tıklayarak ve ardından sayfanın sağ üst köşesinde hesabınıza tıklayarak Azure AD kiracınızı seçin **dizini Değiştir** gezinti ve uygun bir kiracı seçin. 
  * Hesabınızın altında tek bir Azure AD kiracısı varsa veya uygun Azure AD kiracısını zaten seçtiyseniz, bu adımı atlayın.
* Sol taraftaki gezinti bölmesinde, **Azure Active Directory**’ye tıklayın.
* Tıklayarak **uygulama kayıtları** tıklayın **yeni kayıt**.
* Komut istemlerini izleyin ve yeni bir uygulama oluşturun. Olmayan bir web uygulaması veya Bu öğretici için ortak istemci (Mobil ve Masaüstü) uygulaması ise, önemli, ancak belirli örnekler web uygulamaları veya genel istemci uygulamaları için Dilerseniz kullanıma sunduğumuz [hızlı başlangıçlar](../articles/active-directory/develop/v1-overview.md).
  * **Ad**, uygulamanın adıdır ve uygulamanızı son kullanıcılara açıklar.
  * Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizinini ve kişisel Microsoft hesapları hesaplarında**.
  * Sağlamak **yeniden yönlendirme URI'si**. Web uygulamaları için uygulamanızın temel URL'si nerede kullanıcılar oturum açabilir budur.  Örneğin, `http://localhost:12345`. Genel istemci için (Mobil ve Masaüstü), Azure AD, belirteç yanıtlarını döndürmek için kullanır. Uygulamanıza özgü bir değer girin.  Örneğin, `http://MyFirstAADApp`.
    <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
* Kayıt tamamlandıktan sonra Azure AD uygulamanızın benzersiz istemci tanımlayıcısı atar ( **uygulama kimliği**). Bu değere ihtiyacınız sonraki bölümlerde, bu nedenle uygulama sayfasından kopyalayın.
* Uygulamanızı Azure portalında bulmak için tıklatın **uygulama kayıtları**ve ardından **tüm uygulamaları görüntüle**.
