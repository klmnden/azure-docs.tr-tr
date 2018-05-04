---
title: Uygulama Hizmetleri uygulamanız için Azure Active Directory kimlik doğrulamasını yapılandırma
description: Uygulama Hizmetleri uygulamanız için Azure Active Directory kimlik doğrulaması yapılandırma konusunda bilgi edinin.
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
ms.date: 04/19/2018
ms.author: mahender
ms.openlocfilehash: 2530cb55cb054c02df5d55ccb86e959a061e2499
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="configure-your-app-service-app-to-use-azure-active-directory-login"></a>Uygulama hizmeti uygulamanızı Azure Active Directory oturum açma kullanmak için yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu makalede, Azure Active Directory kimlik doğrulama sağlayıcısı olarak kullanmak için Azure App hizmetlerin nasıl yapılandırılacağı gösterilmektedir.

## <a name="express"> </a>Hızlı ayarları kullanarak Azure Active Directory yapılandırma
1. İçinde [Azure portal], uygulama hizmeti uygulamanızı gidin. Sol gezinti bölmesinde seçin **kimlik doğrulama / yetkilendirme**.
2. Varsa **kimlik doğrulama / yetkilendirme** etkin, select değil **üzerinde**.
3. Seçin **Azure Active Directory**ve ardından **Express** altında **yönetim modu**.
4. Seçin **Tamam** App Service uygulaması Azure Active Directory'ye kaydetmek için. Bu, yeni bir uygulama kaydı oluşturur. Bunun yerine var olan bir uygulama kaydı seçmek istiyorsanız, tıklatın **mevcut uygulamayı Seç** arayın ve sonra Kiracı içinde daha önce oluşturulmuş uygulama kayıt adı.
   Bunu seçin ve uygulama kaydı tıklatın **Tamam**. Ardından **Tamam** Azure Active Directory Ayarları sayfasında.
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
5. (İsteğe bağlı) Sitenize yalnızca Azure Active Directory tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak üzere koyulan **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler kimlik doğrulaması için Azure Active Directory'ye yönlendirilir.
6. **Kaydet**’e tıklayın.

Artık Azure Active Directory kimlik doğrulaması için uygulama hizmeti uygulamanızı ile kullanmak hazırsınız.

## <a name="advanced"> </a>(Alternatif yöntem) Azure Active Directory ile gelişmiş ayarları el ile yapılandır
Ayrıca yapılandırma ayarlarını sağlamak seçebileceğiniz el ile. Kullanmak istediğiniz bir AAD kiracısı ile Azure'da oturum Kiracı farklı ise, bu tercih edilen bir çözümdür. Yapılandırmayı tamamlamak için önce bir kayıt Azure Active Directory'de oluşturmanız gerekir ve ardından kayıt ayrıntıları bazıları App Service'e sağlamanız gerekir.

### <a name="register"> </a>Uygulama hizmeti uygulamanızı Azure Active Directory'ye kaydetme
1. Oturum [Azure portal]ve uygulama hizmeti uygulamanızı gidin. Uygulamanızı kopyalama **URL**. Bu, Azure Active Directory Uygulama kaydı yapılandırmak için kullanın.
2. Gidin **Active Directory**seçeneğini belirleyip **uygulama kayıtlar**, ardından **yeni uygulama kaydı** yeni bir uygulama kaydı başlatmak için üst. 
3. İçinde **oluşturma** want bir **adı** uygulama kaydınız seçin **Web uygulaması / API** yazın, buna **oturum açma URL'si** kutusunda Yapıştır Uygulama URL'den (1. adım). Tıklatıp **oluşturma**.
4. Birkaç saniye içinde az önce oluşturduğunuz yeni uygulama kaydı görmeniz gerekir.
5. Uygulama kayıt eklendikten sonra uygulama kayıt adına tıklayın, tıklayın **ayarları** en üstte tıklayın **özellikleri** 
6. İçinde **uygulama kimliği URI'si** kutusunda, (1. adımdaki), uygulama URL'sini yapıştırın de **giriş sayfası URL'si** Yapıştır uygulama URL'den (1. adım)'de, ardından **Kaydet**
7. Şimdi tıklayın **yanıt URL'leri**, düzenleme **yanıt URL'si**, uygulama URL'sini (1. adım) yapıştırın, sahip olduğunuzdan emin olmak için Protokolü Değiştir **https://** Protokolü (http:// değil) ardından URL'nin sonuna */.auth/login/aad/callback* (örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`). **Kaydet**’e tıklayın.   
8.  Bu noktada, kopyalama **uygulama kimliği** uygulaması. Daha sonra kullanmak üzere saklayın. Uygulama hizmeti Uygulamanızı yapılandırmak için gerekir.
9. Kapat **kayıtlı uygulama** sayfası. Üzerinde **uygulama kayıtlar** sayfasında, tıklatın **uç noktaları** düğmesi üst kısmında, ardından kopyalama **Federasyon meta veri belgesi** URL. 
10. Yeni bir tarayıcı penceresi açın ve XML sayfasına göz atma ve yapıştırma tarafından URL'ye gidin. Belge üstünde bir **EntityDescriptor** öğesi olması gerekir bir **Entityıd** özniteliği formun `https://sts.windows.net/` bir GUID belirli kiracınıza ("Kiracı kimliği" olarak adlandırılır) ve ardından. Bu değer kopyalama - bu görevi gören, **veren URL'si**. Uygulamanızı daha sonra kullanmak üzere yapılandırır.

### <a name="secrets"> </a>Uygulama hizmeti uygulamanızı Azure Active Directory bilgileri ekleme
1. Geri [Azure portal], uygulama hizmeti uygulamanızı gidin. Tıklatın **kimlik doğrulama/yetkilendirme**. Kimlik doğrulama/yetkilendirme özelliği etkin değilse, anahtar etkinleştirmek **üzerinde**. Tıklayın **Azure Active Directory**, uygulamanızı yapılandırmak için kimlik doğrulama sağlayıcıları altında. (İsteğe bağlı) Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir. Ayarlama **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**. Bu seçenek, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler kimlik doğrulaması için Azure Active Directory'ye yönlendirilir.
2. Active Directory kimlik doğrulaması yapılandırmasında tıklatın **Gelişmiş** altında **yönetim modu**. İstemci kimliği kutusundan (adım 8) uygulama kimliği yapıştırın ve Entityıd (10 adımdan) veren URL değeri yapıştırın. Daha sonra, **Tamam**'a tıklayın.
3. Active Directory kimlik doğrulaması yapılandırma sayfasında, tıklatın **kaydetmek**.

Artık Azure Active Directory kimlik doğrulaması için uygulama hizmeti uygulamanızı ile kullanmak hazırsınız.

## <a name="optional-configure-a-native-client-application"></a>(İsteğe bağlı) Yerel istemci uygulaması yapılandırın
Azure Active Directory yerel istemcilerini kaydetmek eşleme izinleri üzerinde daha fazla denetim sağlayan sağlar. Bir kitaplık kullanılarak oturumları gerçekleştirmek isterseniz, bu gereksinim **Active Directory kimlik doğrulama Kitaplığı**.

1. Gidin **Azure Active Directory** içinde [Azure portal].
2. Sol gezinti bölmesinde seçin **uygulama kayıtlar**. Tıklatın **yeni uygulama kaydı** üstünde.
4. İçinde **oluşturma** want bir **adı** , uygulama kaydı için. Seçin **yerel** içinde **uygulama türü**.
5. İçinde **yeniden yönlendirme URI'si** kutusuna, sitenizin girin */.auth/login/done* uç noktasını, HTTPS şeması kullanarak. Bu değer benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*. Bir Windows uygulaması kullanmak yerine oluşturuyorsanız [SID paketini](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) URI olarak.
5. **Oluştur**’a tıklayın.
6. Uygulama kayıt eklendikten sonra açmak için seçin. Bul **uygulama kimliği** ve bu değeri not edin.
7. Tıklatın **tüm ayarları** > **gerekli izinleri** > **Ekle** > **bir API seçin**.
8. İçin arama sonra seçin ve daha önce kaydedilen uygulama hizmeti uygulaması adı yazın **seçin**. 
9. Seçin **erişim \<app_name >**. Ardından **seçin**. Sonra da **Bitti**’ye tıklayın.

Uygulama hizmeti uygulamanızı erişmek için bir yerel istemci uygulaması artık yapılandırdınız.

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
[8]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-webapp-auth-config.png



<!-- URLs. -->

[Azure portal]: https://portal.azure.com/
[alternative method]:#advanced
