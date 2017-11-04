---
title: "Uygulama Hizmetleri uygulamanız için Azure Active Directory kimlik doğrulamasını yapılandırma"
description: "Uygulama Hizmetleri uygulamanız için Azure Active Directory kimlik doğrulaması yapılandırma konusunda bilgi edinin."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: 6ec6a46c-bce4-47aa-b8a3-e133baef22eb
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 25f0578a9e273c30ecc98af5b66c6dd43305aa03
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>App Service uygulamanızı Azure Active Directory oturum açma bilgilerini kullanacak şekilde yapılandırma
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

Bu konu Azure uygulama Azure Active Directory kimlik doğrulama sağlayıcısı olarak kullanmak üzere nasıl yapılandırılacağını gösterir.

## <a name="express"></a>Yapılandırma Azure hızlı ayarları kullanarak Active Directory
1. İçinde [Azure portal], uygulamanıza gidin. Tıklatın **ayarları**ve ardından **kimlik doğrulama/yetkilendirme**.
2. Kimlik doğrulama / yetkilendirme özelliği etkin değil, anahtar etkinleştirmek **üzerinde**.
3. Tıklatın **Azure Active Directory**ve ardından **Express** altında **yönetim modu**.
4. Tıklatın **Tamam** uygulama Azure Active Directory'ye kaydetmek için. Bu yeni bir kayıt oluşturur. Bunun yerine var olan bir kaydı seçmek istiyorsanız, tıklatın **mevcut uygulamayı Seç** arayın ve sonra Kiracı içinde önceden oluşturduğunuz bir kayıt adı.
   Seçin ve kaydın tıklatın **Tamam**. Ardından **Tamam** Azure Active Directory ayarlar dikey penceresinde.
   Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir.
5. (İsteğe bağlı) Sitenize yalnızca Azure Active Directory tarafından kimliği doğrulanmış kullanıcılar için erişimi kısıtlamak üzere koyulan **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem** için **Azure Active Directory ile oturum aç**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler kimlik doğrulaması için Azure Active Directory'ye yönlendirilir.
6. **Kaydet** düğmesine tıklayın.

Şimdi uygulamanıza kimlik doğrulaması için Azure Active Directory'yi kullanmak hazırsınız.

## <a name="advanced"></a>(Alternatif yöntem) el ile Azure Active Directory ile gelişmiş ayarları yapılandır
Ayrıca yapılandırma ayarlarını sağlamak seçebileceğiniz el ile. Kullanmak istediğiniz bir AAD kiracısı ile Azure'da oturum Kiracı farklı ise, bu tercih edilen bir çözümdür. Yapılandırmayı tamamlamak için önce bir kayıt Azure Active Directory'de oluşturmanız gerekir ve ardından kayıt ayrıntıları bazıları App Service'e sağlamanız gerekir.

### <a name="register"></a>Azure Active Directory ile uygulamanızı kaydetme
1. Oturum [Azure portal]ve uygulamanıza gidin. Uygulamanızı kopyalama **URL**. Azure Active Directory Uygulamanızı yapılandırmak için bunu kullanır.
2. Gidin **Active Directory**seçeneğini belirleyip **uygulama kayıtlar**, ardından **yeni uygulama kaydı** yeni bir uygulama kaydı başlatmak için üst. 
3. Create uygulama kayıt iletişim kutusuna girin bir **adı** uygulamanız için seçin **Web App API** yazın, buna **oturum açma URL'si** kutusunu (uygulama URL'den yapıştırın 1. adım). Tıklatıp **oluşturma**.
4. Birkaç saniye içinde yeni görmelisiniz uygulama oluşturduğunuz kaydı görünür.
5. Uygulama eklendikten sonra uygulama kaydı adına tıklayın, tıklayın **ayarları** en üstte tıklayın **özellikleri** 
6. İçinde **uygulama kimliği URI'si** kutusunda, (1. adımdaki), uygulama URL'sini yapıştırın de **giriş sayfası URL'si** Yapıştır uygulama URL'den (1. adım)'de, ardından **Kaydet**
7. Şimdi tıklayın **yanıt URL'leri**, düzenleme **yanıt URL'si**, uygulama URL'sini (1. adım) yapıştırın, sahip olduğunuzdan emin olmak için Protokolü Değiştir **https://** Protokolü (http:// değil) ardından URL'nin sonuna */.auth/login/aad/callback* . (Örneğin, `https://contoso.azurewebsites.net/.auth/login/aad/callback`.) **Save (Kaydet)** düğmesine tıklayın.   
8.  Bu noktada, kopyalama **uygulama kimliği** uygulaması. Bu, daha sonra kullanmak üzere saklayın. Bu web Uygulamanızı yapılandırmak için gerekir.
9. Uygulama kaydı ayrıntıları dikey penceresini kapatın. Azure Active Directory Uygulama kaydı Özete döndürmelidir tıklayın **uç noktaları** düğmesi üst kısmında, ardından kopyalama **Federasyon meta veri belgesi** URL. 
10. Yeni bir tarayıcı penceresi açın ve XML sayfasına göz atma ve yapıştırma tarafından URL'ye gidin. AT belgenin üst kısmında olacak bir **EntityDescriptor** öğesi olması gerekir bir **Entityıd** özniteliği formun `https://sts.windows.net/` bir GUID belirli kiracınıza ("Kiracı kimliği" olarak adlandırılır) ve ardından. Bu değer kopyalama - olarak hizmet verecektir, **veren URL'si**. Uygulamanız bu daha sonra kullanmak üzere yapılandırır.

### <a name="secrets"></a>Uygulamanıza eklemek Azure Active Directory bilgileri
1. Geri [Azure portal], uygulamanıza gidin. Tıklatın **kimlik doğrulama/yetkilendirme**. Kimlik doğrulama/yetkilendirme özelliği etkin değilse, anahtar etkinleştirmek **üzerinde**. Tıklayın **Azure Active Directory**, uygulamanızı yapılandırmak için kimlik doğrulama sağlayıcıları altında. (İsteğe bağlı) Varsayılan olarak, App Service kimlik doğrulaması sağlar ancak yetkili erişimi üzere site içeriğini ve API'lerini kısıtlamaz. Kullanıcılar, uygulama kodunuzda yetkilendirmeniz gerekir. Seçin **isteğin kimliği doğrulanmamış olduğunda gerçekleştirilecek eylem**, bu ayar **Azure Active Directory ile oturum aç**. Bu, tüm istekleri doğrulanmasını gerektirir ve tüm kimliği doğrulanmamış istekler kimlik doğrulaması için Azure Active Directory'ye yönlendirilir.
Active Directory kimlik doğrulaması yapılandırması 2 **Gelişmiş** altında **yönetim modu**. İstemci kimliği kutusundan (adım 8) uygulama kimliği yapıştırın ve Entityıd (10 adımdan) veren URL değeri yapıştırın. Daha sonra, **Tamam**'a tıklayın.
3. Active Directory kimlik doğrulaması yapılandırma dikey penceresinde **kaydetmek**.

Şimdi uygulamanıza kimlik doğrulaması için Azure Active Directory'yi kullanmak hazırsınız.

## <a name="optional-configure-a-native-client-application"></a>(İsteğe bağlı) Yerel istemci uygulaması yapılandırın
Azure Active Directory yerel istemcilerini kaydetmek eşleme izinleri üzerinde daha fazla denetim sağlayan sağlar. Bir kitaplık kullanılarak oturumları gerçekleştirmek isterseniz, bu gereksinim **Active Directory kimlik doğrulama Kitaplığı**.

1. Gidin **Active Directory** içinde [Klasik Azure portalı].
2. Dizininizi seçin ve ardından **uygulamaları** üst sekmesini. Tıklatın **eklemek** altındaki yeni bir uygulama kaydı oluşturun.
3. Tıklatın **kuruluşumun geliştirmekte olduğu bir uygulama Ekle**.
4. Uygulama Ekleme Sihirbazı'nda girin bir **adı** tıklatın ve uygulama için **yerel istemci uygulaması** türü. Devam etmek için'ye tıklayın.
5. İçinde **yeniden yönlendirme URI'si** kutusuna, sitenizin girin */.auth/login/done* uç noktasını, HTTPS şeması kullanarak. Bu değer benzer olmalıdır *https://contoso.azurewebsites.net/.auth/login/done*. Bir Windows uygulaması kullanmak yerine oluşturuyorsanız [SID paketini](../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) URI olarak.
6. Yerel uygulama eklendiğinde tıklatın **yapılandırma** sekmesi. Bul **istemci kimliği** ve bu değeri not edin.
7. Sayfayı aşağı kaydırın **diğer uygulamalara izinler** 'ye tıklayın **uygulama eklemek**.
8. Daha önce kaydettiğiniz web uygulaması için arama ve artı simgesine tıklayın. Onay iletişim kutusunu kapatmak için'ye tıklayın. Web uygulaması bulunamadı, kayıt için gidin ve yeni bir yanıt URL'si (örn., HTTP sürümü geçerli URL'niz) eklemek bulamazsa, Kaydet'i tıklatın ve ardından aşağıdaki adımları tekrarlayın.-uygulama listesinde göstermelidir.
9. Yeni eklediğiniz yeni girişinde açmak **izinlere temsilci** açılır ve seçin **erişim (appName)**. Daha sonra **Kaydet**'e tıklayın.

Uygulama hizmeti uygulamanızı erişebileceğiniz bir yerel istemci uygulaması artık yapılandırdınız.

## <a name="related-content"></a>İlgili içerik
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
[Klasik Azure portalı]: https://manage.windowsazure.com/
[alternative method]:#advanced
