---
title: Azure Active Directory kimlik doğrulama - Azure App Service'ı yapılandırma
description: App Service uygulamanız için Azure Active Directory kimlik doğrulamasını yapılandırma konusunda bilgi edinin.
author: mattchenderson
services: app-service
documentationcenter: ''
manager: syntaxc4
editor: ''
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 02/20/2019
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: d687e770fae6c32ee351a597e12d1aca6094e5cb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60851384"
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-sign-in"></a>App Service uygulamanızı Azure Active Directory oturum açma kullanacak şekilde yapılandırma

> [!NOTE]
> Şu anda AAD V2 (MSAL dahil) desteklenmez Azure App Services ve Azure işlevleri için. Lütfen geri güncelleştirmeleri denetleyin.
>

[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure App Services'ı, Azure Active Directory kimlik doğrulama sağlayıcısı kullanmak için yapılandırma gösterilmektedir.

## <a name="express"> </a>Hızlı ayarlarla yapılandırın

1. İçinde [Azure portal], App Service uygulamanıza gidin. Sol gezinti bölmesinde **kimlik doğrulama / yetkilendirme**.
2. Varsa **kimlik doğrulama / yetkilendirme** seçeneğini etkin olmadığından **üzerinde**.
3. Seçin **Azure Active Directory**ve ardından **Express** altında **yönetim modu**.
4. Seçin **Tamam** App Service uygulaması Azure Active Directory'ye kaydetmek için. Bu, yeni bir uygulama kaydı oluşturur. Bunun yerine mevcut bir uygulama kaydı seçmek istiyorsanız, tıklayın **mevcut uygulamayı Seç** kiracınızdaki bir önceden oluşturulmuş uygulama kayıt adını bulun.
   Uygulama kaydı, onu seçin ve tıklayın **Tamam**. Ardından **Tamam** Azure Active Directory Ayarları sayfasında.
   Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
5. (İsteğe bağlı) Sitenizi yalnızca Azure Active Directory tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak için ayarlanmış **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**. Bu, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri için Azure Active Directory kimlik doğrulaması için yönlendirilirsiniz.
6. **Kaydet**’e tıklayın.

## <a name="advanced"> </a>Gelişmiş ayarlarla yapılandırın

Yapılandırma ayarlarını sağlamak el ile. Kullanmak istediğiniz Azure Active Directory kiracısı ile Azure'da oturum açın kiracıda farklı ise bu tercih edilen bir çözümdür. Yapılandırmayı tamamlamak için öncelikle bir kaydı Azure Active Directory'de oluşturmanız gerekir ve ardından kayıt ayrıntılarının bazılarını App Service'e sağlamanız gerekir.

### <a name="register"> </a>App Service uygulamanızı Azure Active Directory'ye kaydetme

1. Oturum [Azure portal]ve App Service uygulamanıza gidin. Uygulamanızı kopyalama **URL**. Bu, Azure Active Directory Uygulama kaydını yapılandırmak için kullanır.
2. Gidin **Active Directory**seçin **uygulama kayıtları**, ardından **yeni uygulama kaydı** üst yeni bir uygulama kaydı başlatın. 
3. İçinde **Oluştur** want bir **adı** seçin, uygulama kaydı için **Web uygulaması / API** yazın, buna **oturum açma URL'si** kutusuna yapıştırın Uygulama URL'si (Başlangıç 1. adım). Ardından **Oluştur**.
4. Birkaç saniye içinde oluşturduğunuz yeni uygulama kaydı görmeniz gerekir.
5. Uygulama kaydı eklendikten sonra uygulama kayıt adına tıklayın, tıklayarak **ayarları** en üstünde, ardından **özellikleri** 
6. İçinde **uygulama kimliği URI'si** kutusunda, (1. adımdan), uygulama URL'sini yapıştırın de **giriş sayfası URL'si** (Başlangıç 1. adım) uygulama URL'yi yapıştırın da ardından **Kaydet**
7. Artık tıklayarak **yanıt URL'leri**, Düzenle **yanıt URL'si**(Başlangıç 1. adım) uygulama URL'sini yapıştırın ve ardından, URL'nin sonuna append */.auth/login/aad/callback* (için Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`). **Kaydet**’e tıklayın.

   > [!NOTE]
   > Ek ekleyerek, birden çok etki alanı için aynı uygulama kaydı kullanabilirsiniz **yanıt URL'leri**. Kendi izinler ve onay sahiptir, kendi kayıt, her bir App Service örnek model emin olun. Ayrıca ayrı site yuvaları için ayrı uygulama kayıtları kullanmayı düşünün. Bu, test ettiğiniz yeni kodda bir hata, üretim etkilemez böylece, ortamlar arasında paylaşılmasını izinleri önlemek içindir.
    
8. Bu noktada, kopyalama **uygulama kimliği** uygulama için. Bu, daha sonra kullanmak üzere saklayın. App Service yapılandırmanız gerekir.
9. Kapat **kayıtlı uygulama** sayfası. Üzerinde **uygulama kayıtları** Sayfası'na tıklayın **uç noktaları** düğmesi en üstünde, ardından kopyalama **WS-FEDERASYON oturum açma uç noktası** URL ancak Kaldır `/wsfed` bitiş URL. Sonuç gibi görünmelidir `https://login.microsoftonline.com/00000000-0000-0000-0000-000000000000`. Etki alanı adı bağımsız bir bulut için farklı olabilir. Daha sonra kullanmak üzere bu veren URL'si davranacak.

### <a name="secrets"> </a>App Service uygulamanızı Azure Active Directory bilgilerini ekleyin

1. Geri [Azure portal], App Service uygulamanıza gidin. Tıklayın **kimlik doğrulama/yetkilendirme**. Anahtar kimlik doğrulaması/yetkilendirme özelliğini etkin değilse, açmak **üzerinde**. Tıklayarak **Azure Active Directory**, uygulamanızı yapılandırmak için kimlik doğrulama sağlayıcıları altında.

    (İsteğe bağlı) Varsayılan olarak, App Service kimlik doğrulaması sağlar, ancak site içerik ve API'ler için yetkili erişimi kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir. Ayarlama **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**. Bu seçenek, tüm istekleri kimliğinin doğrulanmasını gerektirir ve kimliği doğrulanmamış tüm istekleri için Azure Active Directory kimlik doğrulaması için yönlendirilirsiniz.
2. Active Directory kimlik doğrulaması Yapılandırması'nda tıklatın **Gelişmiş** altında **yönetim modu**. İstemci kimliği kutusundan (8. adım) uygulama Kimliğini yapıştırın ve URL'den (9. adım) veren URL değeri yapıştırın. Daha sonra, **Tamam**'a tıklayın.
3. Active Directory kimlik doğrulamasını yapılandırma sayfasında tıklayın **Kaydet**.

App Service uygulamanızda kimlik doğrulaması için Azure Active Directory kullanmak artık hazırsınız.

## <a name="configure-a-native-client-application"></a>Yerel istemci uygulaması yapılandırma
Yerel istemci eşleme izinleri üzerinde daha fazla denetim sağlayan kaydedebilirsiniz. Oturum açma işlemleri gibi bir istemci kitaplığı kullanarak gerçekleştirmek istiyorsanız buna ihtiyacınız **Active Directory Authentication Library**.

1. Gidin **Azure Active Directory** içinde [Azure portal].
2. Sol gezinti bölmesinde **uygulama kayıtları**. Tıklayın **yeni uygulama kaydı** en üstünde.
4. İçinde **Oluştur** want bir **adı** , uygulama kaydı için. Seçin **yerel** içinde **uygulama türü**.
5. İçinde **yeniden yönlendirme URI'si** kutusuna, sitenizin girin */.auth/login/done* uç noktasını, HTTPS düzenini kullanarak. Bu değer, aşağıdakine benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done* . Bir Windows uygulaması kullanmak yerine oluşturuyorsanız [SID paketini](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) olarak URI.
5. **Oluştur**’a tıklayın.
6. Uygulama kaydı eklendikten sonra açmak için seçin. Bulma **uygulama kimliği** ve bu değeri not edin.
7. Tıklayın **tüm ayarlar** > **gerekli izinler** > **Ekle** > **bir API seçin**.
8. İçin arama sonra seçin ve daha önce kaydettiğiniz App Service uygulaması adı **seçin**.
9. Seçin **erişim \<app_name >** . Ardından **Seç**'e tıklayın. Sonra da **Bitti**’ye tıklayın.

App Service uygulamanıza erişmek bir yerel istemci uygulaması yapılandırdınız.

## <a name="related-content"> </a>İlgili içerik

[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-url.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app_registration.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-create.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-appidurl.png
[4]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-registration-config-replyurl.png
[5]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints.png
[6]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-endpoints-fedmetadataxml.png
[7]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth.png
[8]: ./media/configure-authentication-provider-aad/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[alternative method]:#advanced
