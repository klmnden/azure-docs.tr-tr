---
title: Yerel istemci uygulaması - Azure Active Directory B2C ekleme | Microsoft Docs
description: Active Directory B2C kiracınıza yerel istemci uygulaması eklemeyi öğrenin.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 772b6b8a4d71b7d1c6418651ee0a4ee7b03af0cc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703905"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Azure Active Directory B2C kiracınıza yerel istemci uygulaması Ekle

Yerel istemci kaynakların uygulamanızı Azure Active Directory B2C ile iletişim kurabilmesi için kiracınızda kayıtlı olması gerekir.

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Azure AD B2C kiracınızı tıklayarak içeren dizine kullandığınızdan emin olun **dizin ve abonelik filtresi** üst menü ve kiracınız içeren dizine seçme.
3. Seçin **tüm hizmetleri** Azure portalı ve ardından arayın ve seçin, sol üst köşedeki **Azure AD B2C**.
1. Seçin **uygulamaları**ve ardından **Ekle**.
2. Uygulama için bir ad girin. Örneğin, *nativeapp1*.
3. İçin **içeren web uygulaması / web API'sini**seçin **Hayır**.
4. İçin **yerel istemciyi dahil et**seçin **Evet**.
5. İçin **yeniden yönlendirme URI'si**, özel bir düzen ile geçerli bir yeniden yönlendirme URI'si girin. Yeniden yönlendirme URI'si seçerken iki önemli noktalar vardır:

    - **Benzersiz** -yeniden yönlendirme URI'si şeması her uygulama için benzersiz olmalıdır. Örnekte `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` düzenidir. Bu düzen gelmelidir. İki uygulama aynı şemayı paylaşıyorsa, kullanıcı bir uygulama seçmek için bir seçenek verilir. Kullanıcı yanlış seçim yaparsa, oturum açma başarısız olur.
    - **Tam** -yeniden yönlendirme URI'SİNİN bir şeması ve yolu olmalıdır. Yol, etki alanından sonra en az bir eğik çizgi içermelidir. Örneğin, `//contoso/` çalışır ve `//contoso` başarısız olur. Yeniden yönlendirme URI'si, alt çizgi gibi özel karakterleri içermeyen emin olun.

6. **Oluştur**’a tıklayın.
7. Özellikler sayfasında, yerel istemci uygulamanızı yapılandırırken kullanacağınız uygulama Kimliğini kaydedin.
