---
title: "Azure Active Directory B2C: Genel Bakış | Microsoft Belgeleri"
description: "Azure Active Directory B2C ile tüketiciye yönelik uygulamalar geliştirme"
services: active-directory-b2c
documentationcenter: 
author: saeeda
manager: krassk
editor: parja
ms.assetid: c465dbde-f800-4f2e-8814-0ff5f5dae610
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/06/2016
ms.author: saeeda
ms.translationtype: Human Translation
ms.sourcegitcommit: 532ff423ff53567b6ce40c0ea7ec09a689cee1e7
ms.openlocfilehash: f3c2760ec66c0292ebeb53d0acb5f9ee1df388ae
ms.contentlocale: tr-tr
ms.lasthandoff: 06/05/2017


---
# <a name="azure-ad-b2c-focus-on-your-app-let-us-worry-about-sign-up-and-sign-in"></a>Azure AD B2C: Siz uygulamanıza odaklanın, kayıt ve oturum açma işlemlerini bize bırakın

Azure AD B2C, web ve mobil uygulamalarınızda kullanabileceğiniz bulut tabanlı kimlik yönetim çözümüdür. Yüz milyonlarca kimlik ölçeğinde yüksek oranda kullanılabilir bir küresel hizmettir. Kurumsal düzeyde güvenli bir platform üzerinde oluşturulan Azure AD B2C uygulamalarınızın, işinizin ve müşterilerinizin korunmasını sağlar.

Minimum düzeyde yapılandırma gereksinimine sahip olan Azure AD B2C, uygulamanızın şu kimlikleri doğrulamasını sağlar:

* **Sosyal Hesaplar** (Facebook, Google, LinkedIn vs.)
* **Kurumsal Hesaplar** (açık standart protokollerini, OpenID Connect veya SAML kullanarak)
* **Yerel Hesaplar** (e-posta adresi ve parola veya kullanıcı adı ve parola)

## <a name="get-started"></a>başlarken

Öncelikle [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin.

Ardından uygulama geliştirme senaryonuzu seçin:

|  |  |  |  |
| --- | --- | --- | --- |
| <center>![Mobil Uygulamalar ve Masaüstü Uygulamaları](../active-directory/develop/media/active-directory-developers-guide/NativeApp_Icon.png)<br />Mobil Uygulamalar ve Masaüstü Uygulamaları</center> | [Genel Bakış](active-directory-b2c-reference-oauth-code.md)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<br /><br />[iOS](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal)<br /><br />[Android](https://github.com/Azure-Samples/active-directory-b2c-android-native-msal) | [.NET](https://github.com/Azure-Samples/active-directory-b2c-dotnet-desktop)<br /><br />[Xamarin](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native) |  |
| <center>![Web Apps](../active-directory/develop/media/active-directory-developers-guide/Web_app.png)<br />Web Apps</center> | [Genel Bakış](active-directory-b2c-reference-oidc.md)<br /><br />[ASP.NET](active-directory-b2c-devquickstarts-web-dotnet-susi.md)<br /><br />[ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapp) | [Node.js](active-directory-b2c-devquickstarts-web-node.md) |  |
| <center>![Tek Sayfa Uygulamaları](../active-directory/develop/media/active-directory-developers-guide/SPA.png)<br />Tek Sayfa Uygulamaları</center> | [Genel Bakış](active-directory-b2c-reference-spa.md)<br /><br />[JavaScript](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp)<br /><br /> |  |  |
| <center>![Web API'leri](../active-directory/develop/media/active-directory-developers-guide/Web_API.png)<br />Web API'leri</center> | [ASP.NET](active-directory-b2c-devquickstarts-api-dotnet.md)<br /><br /> [ASP.NET Core](https://github.com/Azure-Samples/active-directory-b2c-dotnetcore-webapi)<br /><br /> [Node.js](https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi) | [.NET web API'si çağırma](active-directory-b2c-devquickstarts-web-api-dotnet.md) |

## <a name="whats-new"></a>Yenilikler

Gelecekteki Azure Active Directory B2C değişiklikleri için burayı sıkça tekrar kontrol edin. @AzureAD kullanarak tüm değişiklikler hakkında tweet de göndereceğiz.

* "Yerleşik İlkeler"e (Genel Kullanım) ek olarak ["Özel İlkeler"](active-directory-b2c-overview-custom.md) özelliği de genel önizleme sürümünde.  Özel ilkeler, kimlik deneyiminin içeriğini denetlemek isteyen kimlik uzmanlarına özeldir.
* [Erişim Belirteci](https://azure.microsoft.com/en-us/blog/azure-ad-b2c-access-tokens-now-in-public-preview) özelliği genel önizleme sürümünde.
* [Avrupa tabanlı Azure AD B2C dizinlerinin genel kullanıma açıldığı](https://azure.microsoft.com/en-us/blog/azuread-b2c-ga-eu/) duyuruldu.
* Her geçen gün büyüyen [GitHub'daki kod örnekleri](https://github.com/Azure-Samples?q=b2c) kitaplığımıza göz atın!

## <a name="how-to-articles"></a>Nasıl yapılır makaleleri

Belirli Azure Active Directory B2C özelliklerinin nasıl kullanılacağını öğrenin:

* [Facebook](active-directory-b2c-setup-fb-app.md), [Google +](active-directory-b2c-setup-goog-app.md), [Microsoft hesabı](active-directory-b2c-setup-msa-app.md), [Amazon](active-directory-b2c-setup-amzn-app.md) ve [LinkedIn](active-directory-b2c-setup-li-app.md) gibi hesapları, tüketiciye yönelik uygulamalarınızda kullanım için yapılandırın.
* [Tüketicileriniz hakkında bilgiler toplamak için özel öznitelikler kullanın](active-directory-b2c-reference-custom-attr.md).
* [Tüketiciye yönelik uygulamalarınızda Azure Multi-Factor Authentication'ı etkinleştirin](active-directory-b2c-reference-mfa.md).
* [Tüketicileriniz için self servis parola sıfırlama ayarlayın](active-directory-b2c-reference-sspr.md).
* Azure Active Directory B2C tarafından sunulan [kaydolma, oturum açma ve diğer tüketiciye yönelik sayfaların görünümünü özelleştirin](active-directory-b2c-reference-ui-customization.md).
* Azure Active Directory B2C kiracınızda [Tüketicileri programlama yoluyla oluşturma, okuma, güncelleştirme ve silme için Azure Active Directory Grafik API'sini kullanın](active-directory-b2c-devquickstarts-graph-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu bağlantılar hizmeti derinlemesine keşfetmek için kullanışlıdır:

* Bkz. [Azure Active Directory B2C fiyatlandırma bilgileri](https://azure.microsoft.com/pricing/details/active-directory-b2c/).
* Azure Active Directory B2C için [kod örneklerimizi](https://azure.microsoft.com/en-us/resources/samples/?service=active-directory&term=b2c) gözden geçirin. 
* Stack Overflow konusunda yardım almak için [azure-ad-b2c](http://stackoverflow.com/questions/tagged/azure-ad-b2c) etiketini kullanın.
* [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c) kullanarak fikirlerinizi bize ulaştırabilirsiniz, fikirlerinizi duymak istiyoruz!
* [Azure AD B2C Protokolü Başvurusu](active-directory-b2c-reference-protocols.md)’nu gözden geçirin.
* [Azure AD B2C Belirteci Başvurusu](active-directory-b2c-reference-tokens.md)’nu gözden geçirin.
* [Azure Active Directory B2C ile ilgili SSS](active-directory-b2c-faqs.md) makalesini okuyun.
* [Azure Active Directory B2C için dosya desteği istekleri](active-directory-b2c-support.md).

## <a name="get-security-updates-for-our-products"></a>Ürünlerimiz için güvenlik güncelleştirmelerini alma

[Bu sayfayı](https://technet.microsoft.com/security/dd252948) ziyaret ederek ve Güvenlik Önerisi Uyarılarına abone olarak güvenlik olaylarının ne zaman ortaya çıkacağı hakkında bildirimleri almanızı öneririz.


