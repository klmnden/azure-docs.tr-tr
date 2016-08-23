<properties
   pageTitle="Azure Active Directory geliştirici kılavuzu | Microsoft Azure"
   description="Bu makale Azure Active Directory'nin geliştirici yönelimli kaynakları için kapsamlı bir kılavuz sağlar."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="msmbaldwin"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/06/2016"
   ms.author="mbaldwin"/>


# Azure Active Directory geliştirici kılavuzu

## Genel Bakış
Bir hizmet olarak kimlik yönetimi (IDMaaS) platformu olan Azure Active Directory (AD), kimlik yönetimini uygulamalarına tümleştirmeleri için geliştiricilere etkili bir yol sağlar. Aşağıdaki makaleler Azure AD'nin uygulanmasına ve önemli özelliklerine genel bakış sunar. Bunları sırayla okumanızı öneririz, çalışmaya başlamak için hazır olduğunuzda [Başlarken](#getting-started) bölümüne atlayabilirsiniz.


1. [Azure Active Directory tümleştirmenin avantajları](active-directory-how-to-integrate.md): Güvenli oturum açma ve kimlik doğrulama için en iyi çözümü neden Azure AD ile tümleştirmenin sunduğunu keşfedin.

1. [Active Directory kimlik doğrulama senaryoları](active-directory-authentication-scenarios.md): Uygulamanıza oturum açma bağlantısı sağlamak için Azure AD'deki basitleştirilmiş kimlik doğrulama özelliğinden yararlanın.

1. [Uygulamaları Azure Active Directory ile tümleştirme](active-directory-integrating-applications.md): Azure AD'de uygulamaları nasıl ekleyeceğinizi, güncelleştireceğinizi ve kaldıracağınızı öğrenin ve tümleşik uygulamalar için markalama talimatları hakkında bilgi alın.

1. [Azure Active Directory Grafik API'si](active-directory-graph-api.md): Azure AD'ye REST API uç noktaları yoluyla programlı olarak erişmek için Azure AD Grafik API'sini kullanın. Azure AD Grafik API'sine [Microsoft Graph](https://graph.microsoft.io/) yoluyla da erişilebileceğini unutmayın. Microsoft Graph, tek bir REST API uç noktası ve tek bir erişim belirteci ile birden çok Microsoft bulut hizmeti API'sine erişim sağlayan bir birleşik API'dir.

1. [Azure Active Directory kimlik doğrulama kitaplıkları](active-directory-authentication-libraries.md): .NET, JavaScript, Objective-C, Android ve daha fazlası için Azure AD kimlik doğrulama kitaplıklarını kullanın ve erişim belirteçleri almak üzere kullanıcıların kimliklerini kolaylıkla doğrulayın.


## Başlarken

Bu öğreticiler birden çok platform için uyarlanabilir ve Azure Active Directory'yi geliştirmeye hızla başlamanıza yardımcı olabilir. Önkoşul olarak, [bir Azure Active Directory kiracısı edinmeniz](active-directory-howto-tenant.md) gerekir.

### Mobil ve PC uygulaması hızlı başlangıç kılavuzları

|[![iOS](./media/active-directory-developers-guide/ios.png)](active-directory-devquickstarts-ios.md)|[![Android](./media/active-directory-developers-guide/android.png)](active-directory-devquickstarts-android.md)|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-dotnet.md)|[![Windows Universal](./media/active-directory-developers-guide/windows.png)](active-directory-devquickstarts-windowsstore.md)|[![Xamarin](./media/active-directory-developers-guide/xamarin.png)](active-directory-devquickstarts-xamarin.md)|[![Cordova](./media/active-directory-developers-guide/cordova.png)](active-directory-devquickstarts-cordova.md)|[![OAuth 2.0](./media/active-directory-developers-guide/oauth-2.png)](active-directory-protocols-oauth-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|[iOS](active-directory-devquickstarts-ios.md)|[Android](active-directory-devquickstarts-android.md)|[.NET](active-directory-devquickstarts-dotnet.md)|[Windows Evrensel](active-directory-devquickstarts-windowsstore.md)|[Xamarin](active-directory-devquickstarts-xamarin.md)|[Cordova](active-directory-devquickstarts-cordova.md)|[OAuth 2.0 ile doğrudan tümleştirme](active-directory-protocols-oauth-code.md)|

### Web uygulaması hızlı başlangıç kılavuzları

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapp-dotnet.md)|[![Java](./media/active-directory-developers-guide/java.png)](active-directory-devquickstarts-webapp-java.md)|[![AngularJS](./media/active-directory-developers-guide/angularjs.png)](active-directory-devquickstarts-angular.md)|[![Javascript](./media/active-directory-developers-guide/javascript.png)](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-openidconnect-nodejs.md) | [![OpenID Connect](./media/active-directory-developers-guide/openid-connect.png)](active-directory-protocols-openid-connect-code.md)
|:--:|:--:|:--:|:--:|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapp-dotnet.md)|[Java](active-directory-devquickstarts-webapp-java.md)|[AngularJS](active-directory-devquickstarts-angular.md)|[Javascript](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi)|[Node.js](active-directory-devquickstarts-openidconnect-nodejs.md)|[OpenID Connect ile doğrudan tümleştirme](active-directory-protocols-openid-connect-code.md)|

### Web API'si hızlı başlangıç kılavuzları

|[![.NET](./media/active-directory-developers-guide/net.png)](active-directory-devquickstarts-webapi-dotnet.md)|[![Node.js](./media/active-directory-developers-guide/nodejs.png)](active-directory-devquickstarts-webapi-nodejs.md)
|:--:|:--:|
|[.NET](active-directory-devquickstarts-webapi-dotnet.md)|[Node.js](active-directory-devquickstarts-webapi-nodejs.md)

### Dizini sorgulama hızlı başlangıç kılavuzu

| [![.NET](./media/active-directory-developers-guide/graph.png)](active-directory-graph-api-quickstart.md)|
|:--:|
|[Grafik API'si](active-directory-graph-api-quickstart.md)|

## Nasıl yapılır makaleleri

Bu makalelerde Azure Active Directory kullanılarak belirli görevlerin nasıl gerçekleştirileceği açıklanmaktadır:

- [Azure AD kiracısı edinin](active-directory-howto-tenant.md)
- [Çok kiracılığı uygulama düzenini kullanarak herhangi bir Azure AD kullanıcısı ile oturum açın](active-directory-devhowto-multi-tenant-overview.md) 
- [Android](active-directory-sso-android.md) ve [iOS](active-directory-sso-ios.md) cihazlarında ADAL kullanarak uygulamalar arası SSO'yu etkinleştirin
- [Uygulamanızı Azure AD için AppSource Sertifikalı yapın](active-directory-devhowto-appsource-certified.md)
- [Uygulamanızı Azure AD uygulama galerisinde listeleyin](active-directory-app-gallery-listing.md)
- [Office 365 web uygulamalarını Satıcı Panosu'na gönderme](https://msdn.microsoft.com/office/office365/howto/submit-web-apps-seller-dashboard)
- [Azure Active Directory uygulama bildirimini anlama](active-directory-application-manifest.md)
- [İstemci uygulamanızdaki oturum açma ve uygulama edinme düğmeleri için marka yönergelerini öğrenin](active-directory-branding-guidelines.md)
- [Önizleme: Kullanıcıların hem kişisel hem de iş veya okul hesaplarıyla oturum açan uygulamalar oluşturma](active-directory-appmodel-v2-overview.md)
- [Önizleme: Kullanıcıları kaydeden ve oturumlarını açan uygulamalar oluşturma](../active-directory-b2c/active-directory-b2c-overview.md)


## Başvuru

Bu makaleler REST ve kimlik doğrulama kitaplığı API'leri, protokolleri, hataları, kod örnekleri ve uç noktaları için temel bir başvuru sağlar.  

###  Destek
- [Etiketli sorular](http://stackoverflow.com/questions/tagged/azure-active-directory): Azure Active Directory çözümlerini Stack Overflow'da bulmak için [azure-active-directory](http://stackoverflow.com/questions/tagged/azure-active-directory) ve [adal](http://stackoverflow.com/questions/tagged/adal) etiketlerini arayın.

### Kod

- [Azure Active Directory açık kaynak kitaplıkları](http://github.com/AzureAD): Bir kitaplığın kaynağını bulmanın en kolay yolu [kitaplık listemizi](active-directory-authentication-libraries.md) kullanmaktır.

- [Azure Active Directory örnekleri](https://github.com/azure-samples?query=active-directory): Örnekler listesinde gezinmenin en kolay yolu [kod örnekleri dizinini](active-directory-code-samples.md) kullanmaktır.

- [.NET için ADAL](https://msdn.microsoft.com/library/azure/mt417579.aspx): .NET kimlik doğrulama kitaplığı belgeleri.

### Grafik API'si

- [Grafik API'si başvurusu](https://msdn.microsoft.com/library/azure/hh974476.aspx): Azure Active Directory Grafik API'si için REST başvurusu. [Etkileşimli Grafik API'si başvurusu deneyimini görüntüleyin](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

- [Grafik API'si izin kapsamları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes): Bir kiracıdaki dizin verilerine bir uygulamanın sahip olduğu erişimi kontrol etmek için kullanılan OAuth 2.0 izin kapsamları.

### Kimlik doğrulama ve yetkilendirme protokolleri

- [Azure AD’de İmzalama Anahtar Geçişi](active-directory-signing-key-rollover.md): Azure AD’nin imzalama anahtar geçişi uyumu ve anahtarın en yaygın uygulama senaryoları için nasıl güncelleştirileceği hakkında bilgi edinin.

- [OAuth 2.0 protokolü: Yetkilendirme kodu vermeyi kullanma](active-directory-protocols-oauth-code.md): Azure Active Directory kiracınızdaki Web uygulamalarına ve Web API’lerine erişim izni vermek için OAuth 2.0 protokolünün yetkilendirme kodu verme özelliğini kullanabilirsiniz.

- [OAuth 2.0 protokolü: Örtük vermeyi anlama](active-directory-dev-understanding-oauth2-implicit-grant.md): Örtük yetki verme ve uygulamanız için doğru olup olmadığını anlama hakkında daha fazla bilgi edinin.

- [OAuth 2.0 protokolü: İstemci Kimlik Bilgileri ile Hizmetten Hizmete Çağrılar](active-directory-protocols-oauth-service-to-service.md): OAuth 2.0 İstemci Kimlik Bilgileri Verme akışı bir web hizmetinin (gizli bir istemci) başka bir web hizmetini çağırırken bir kullanıcının kimliğine bürünmek yerine kendi kimlik bilgilerini kullanmasına izin verir. Bu senaryoda istemci genellikle bir orta katman web hizmeti, arka plan programı hizmeti veya web sitesi olur.

- [OpenID Connect 1.0 protokolü: Oturum açma ve kimlik doğrulaması](active-directory-protocols-openid-connect-code.md): OpenID Connect 1.0 protokolü, OAuth 2.0'ı bir kimlik doğrulama protokolü olarak kullanılmak üzere genişletir. Bir istemci uygulaması, oturum açma işlemini yönetmek üzere bir id_token alır ya da yetkilendirme kodu akışını hem bir id_token hem de yetkilendirme kodu alacak şekilde büyütür.

- [SAML 2.0 protokolü başvurusu](active-directory-saml-protocol-reference.md): SAML 2.0 protokolü, uygulamaların kullanıcılarına çoklu oturum açma deneyimi sunmasını sağlar.

- [WS-Federasyon 1.2 protokolü](http://docs.oasis-open.org/wsfed/federation/v1.2/os/ws-federation-1.2-spec-os.html): Azure Active Directory, Web Hizmetleri Federasyonu 1.2 Sürümü Belirtimine göre WS-Federasyon 1.2'yi destekler. Federasyon meta veri belgesi hakkında daha fazla bilgi için lütfen bkz. [Federasyon Meta Verileri](active-directory-federation-metadata.md).

- [Desteklenen belirteç ve talep türleri](active-directory-token-and-claims.md): SAML 2.0 ve JSON Web Token (JWT) belirteçlerindeki talepleri anlamak ve değerlendirmek için bu kılavuzu kullanabilirsiniz.

## Videolar

### Oluşturma

Azure Active Directory kullanılarak uygulamaların geliştirilmesini açıklayan bu genel bakış sunularında, doğrudan mühendislik ekibinde çalışan konuşmacılar yer almaktadır. Sunular IDMaaS, kimlik doğrulama, kimlik federasyonu ve çoklu oturum açma dahil olmak üzere temel konu başlıklarını kapsar.

- [Microsoft Identity: Birliğin Durumu ve Gelecekteki Yönü](https://azure.microsoft.com/documentation/videos/build-2016-microsoft-identity-state-of-the-union-and-future-direction/)
- [Azure Active Directory: Modern uygulamalar için bir hizmet olarak kimlik yönetimi](https://azure.microsoft.com/documentation/videos/build-2015-azure-active-directory-identity-management-as-a-service-for-modern-applications/)
- [Azure Active Directory ile modern web uygulamaları geliştirin](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-web-applications-with-azure-active-directory/)
- [Azure Active Directory ile modern yerel uygulamalar geliştirin](https://azure.microsoft.com/documentation/videos/build-2015-develop-modern-native-applications-with-azure-active-directory/)

### Azure Friday
[Azure Friday](https://azure.microsoft.com/documentation/videos/azure-friday/), her Cuma günü yinelenen ve Azure ile ilgili çeşitli konu başlıklarında uzmanlarla bire bir olarak yapılan kısa (10-15 dakikalık) görüşmeleri sizlere aktaran bir video serisidir.  Tüm Azure Active Directory videolarını görmek için sayfadaki Hizmetler Filtresi'ni kullanın.

- [Azure Identity 101](https://azure.microsoft.com/documentation/videos/azure-identity-basics/)
- [Azure Identity 102](https://azure.microsoft.com/documentation/videos/azure-identity-creating-active-directory/)
- [Azure Identity 103](https://azure.microsoft.com/documentation/videos/azure-identity-application-to-authenticate/)

## Sosyal

- [Active Directory Ekip blogu](http://blogs.technet.com/b/ad/): Azure Active Directory dünyasındaki en son gelişmeler.

- [Azure Active Directory Graph Ekip blogu](http://blogs.msdn.com/b/aadgraphteam): Grafik API'sine özel Azure Active Directory bilgileri.

- [Bulut Kimliği](http://www.cloudidentity.net): Azure Active Directory'nin ana proje yöneticilerinden birinin hizmet olarak kimlik yönetimi hakkındaki düşünceleri.  

- [Twitter'da Azure Active Directory](https://twitter.com/azuread): 140 veya daha az karakterli Azure Active Directory duyuruları.



<!--HONumber=Aug16_HO1-->


