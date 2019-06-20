---
title: (Uygulama kaydı) - kullanıcılar oturum açtığında web uygulaması Microsoft kimlik platformu
description: (Uygulama kaydı) kullanıcılar oturum açtığında bir web uygulaması oluşturmayı öğrenin
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0ae638f8cbef29c5d167a3ab59188169cbd934ef
ms.sourcegitcommit: 6e6813f8e5fa1f6f4661a640a49dc4c864f8a6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67150219"
---
# <a name="web-app-that-signs-in-users---app-registration"></a>Kullanıcılar - uygulama kaydı imzalar web uygulaması

Bu sayfa, oturum açtığında, kullanıcıların bir web uygulaması için uygulama kaydı özellikleri açıklanmaktadır.

Uygulamanızı kaydetmek için kullanabilirsiniz:

- [Web uygulaması hızlı başlangıç kılavuzlarımız](#register-an-app-using-the-quickstarts) -hızlı başlangıçlar Azure portalında adlı bir düğme içeren bir uygulama oluşturma konusunda harika bir ilk deneyim olmasının yanı sıra **benim için bu değişikliği yapmak**. Bu düğme, bile var olan bir uygulama için gereken özellikleri ayarlamak için kullanabilirsiniz. Kendi çalışması için bu özelliklerin değerlerini uyum gerekecektir. Özellikle, uygulamanız için web API URL'si büyük olasılıkla farklı URI oturumunuzu da etkiler önerilen varsayılan zordur.
- Azure portalında [el ile kaydedin](#register-an-app-using-azure-portal)
- PowerShell ve komut satırı araçları

## <a name="register-an-app-using-the-quickstarts"></a>Hızlı başlangıçları kullanarak bir uygulamayı kaydetme

Bu bağlantıya gidin, web uygulamanızın oluşturulmasını önyükleme oluşturabilirsiniz:

- [ASP.NET Core](https://aka.ms/aspnetcore2-1-aad-quickstart-v2)
- [ASP.NET](https://ms.portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/AspNetWebAppQuickstartPage/sourceType/docs)

### <a name="register-an-app-using-azure-portal"></a>Azure portalını kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Portalı kullanmak için Microsoft Azure ortak bulutuna veya ulusal veya bağımsız bulut uygulamanızın çalıştığı farklı bağlı değildir. Daha fazla bilgi için [Ulusal Bulutlar](./authentication-national-cloud.md#app-registration-endpoints)

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın. Alternatif olarak, Ulusal bulut tercih ettiğiniz Azure portalında oturum açın.
1. Kiracı, erişmek için birden fazla Kiracı, sağ üst köşedeki hesabınızı seçin ve istenen Azure AD'ye portal oturumunuzu ayarlama, hesap sağlar.
1. Sol gezinti bölmesinde seçin **Azure Active Directory** hizmet ve ardından **uygulama kayıtları** > **yeni kayıt**.
1. **Uygulama kaydet** sayfası göründüğünde uygulamanızın kayıt bilgilerini girin:
   1. Uygulamanız için desteklenen bir hesap türlerini seçin (bkz [desteklenen hesap türleri](./v2-supported-account-types.md))
   1. **Ad** alanına uygulama kullanıcılarına gösterilecek anlamlı bir uygulama adı girin, örneğin `AspNetCore-WebApp`.
   1. İçinde **yeniden yönlendirme URI'si**, uygulama türünü eklemek ve başarıyla kimlik doğrulandıktan sonra belirteç yanıtlarını kabul URI hedef döndürdü. Örneğin, `https://localhost:44321/`.  **Kaydol**’u seçin.
1. Seçin **kimlik doğrulaması** menüsünü ve ardından aşağıdaki bilgileri ekleyin:
   1. İçinde **yanıt URL'si**, ekleme `https://localhost:44321/signin-oidc`.
   1. İçinde **Gelişmiş ayarlar** bölümünde, **oturum kapatma URL'si** için `https://localhost:44321/signout-oidc`.
   1. Altında **örtük vermeyi**, kontrol **kimlik belirteçlerini**.
   1. **Kaydet**’i seçin.

### <a name="register-an-app-using-powershell"></a>PowerShell kullanarak bir uygulamayı kaydetme

> [!NOTE]
> Şu anda Azure AD PowerShell ile aşağıdaki desteklenen hesap türleri yalnızca uygulamaları oluşturur:
>
> - MyOrg (yalnızca kuruluş bu dizinde hesapları)
> - AnyOrg (herhangi bir kuruluş dizini hesaplarında).
>
> Kişisel Microsoft Accounts (örneğin, Skype, XBox, Outlook.com) ile oturum açtığında, kullanıcıların bir uygulama oluşturmak istiyorsanız, önce çok kiracılı bir uygulama oluşturabilirsiniz (hesap türleri için desteklenen herhangi bir kurumsal dizinde hesabı =) ve ardından değiştirin `signInAudience` Azure Portalı'ndan uygulama bildiriminde özellik. Bu adımda Ayrıntılar bölümünde açıklanan [1.3](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-3-AnyOrgOrPersonal#step-1-register-the-sample-with-your-azure-ad-tenant) ASP.NET Core Öğreticisi (ve herhangi bir dilde web uygulamaları için genelleştirilmiş).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uygulama kodu yapılandırma](scenario-web-app-sign-user-app-configuration.md)
