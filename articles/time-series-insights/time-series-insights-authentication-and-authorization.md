---
title: Kimlik doğrulama ve yetkilendirme Azure Time Series Insights içinde bir API kullanarak | Microsoft Docs
description: Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulama için nasıl yapılandırılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: seodec18
ms.openlocfilehash: 385fa0e714c66b4798dfcc3874810d97b76b2a1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67115795"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Kimlik doğrulama ve yetkilendirme için Azure zaman serisi öngörüleri API'si

Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulamada kullanılan nasıl yapılandırılacağı açıklanmaktadır.

> [!TIP]
> Hakkında bilgi edinin [veri erişimi verme](./time-series-insights-data-access.md) zaman serisi görüşleri ortamınıza Azure Active Directory'de.

## <a name="service-principal"></a>Hizmet sorumlusu

Aşağıdaki bölümlerde, uygulama için zaman serisi öngörüleri API'ye erişmek için bir uygulama yapılandırma açıklanmaktadır. Uygulama ardından sorgulayabilir veya kullanıcı kimlik bilgileri yerine uygulama kimlik bilgileri ile zaman serisi görüşleri ortamdaki başvuru verilerini yayımlama.

## <a name="best-practices"></a>En iyi uygulamalar

Erişim zaman serisi görüşleri gereken bir uygulama varsa:

1. Bir Azure Active Directory uygulaması ayarlayın.
1. [Veri erişimi ilkeleri atama](./time-series-insights-data-access.md) zaman serisi görüşleri ortamı içinde.

Kullanıcı kimlik bilgilerinizi yerine uygulama kimlik bilgileri kullanarak istenen çünkü:

* Uygulama kimliği, kendi izinlerinden ayrı izinler atayabilirsiniz. Genellikle, bu izinler için yalnızca ne gerektiren uygulama kısıtlanır. Örneğin, yalnızca belirli bir zaman serisi görüşleri ortamına verileri okumasına izin verebilirsiniz.
* Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmeniz gerekmez.
* Katılımsız betik çalıştırdığınızda, kimlik doğrulaması otomatikleştirmek için bir sertifika veya bir uygulama anahtarı'nı kullanabilirsiniz.

Aşağıdaki bölümlerde, Azure portalı üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Bu makalede tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Tek kiracılı uygulamalar genellikle kuruluşunuzdaki satır iş kolu uygulamaları için kullanırsınız.

## <a name="setup-summary"></a>Kurulum özeti

Bir kurulum akışında üç adımdan oluşur:

1. Azure Active Directory'de bir uygulama oluşturun.
1. Zaman serisi görüşleri ortamına erişmek için bu uygulamayı yetkilendirin.
1. Uygulamasından bir belirteç almak üzere uygulama kimliği ve anahtarı kullan `https://api.timeseries.azure.com/`. Belirteç, daha sonra zaman serisi öngörüleri API'sini çağırmak için de kullanılabilir.

## <a name="detailed-setup"></a>Ayrıntılı Kurulum

1. Azure portalında **Azure Active Directory** > **uygulama kayıtları** > **yeni uygulama kaydı**.

   [![Azure Active Directory'de yeni uygulama kaydı](media/authentication-and-authorization/active-directory-new-application-registration.png)](media/authentication-and-authorization/active-directory-new-application-registration.png#lightbox)

1. Uygulama bir ad verin, türünün olmasını seçin **Web uygulaması / API**, için geçerli bir URI seçin **oturum açma URL'si**seçip **Oluştur**.

   [![Azure Active Directory'de uygulama oluşturma](media/authentication-and-authorization/active-directory-create-web-api-application.png)](media/authentication-and-authorization/active-directory-create-web-api-application.png#lightbox)

1. Yeni oluşturulan uygulamanızı seçin ve uygulama Kimliğini, sık kullandığınız metin düzenleyicisine kopyalayın.

   [![Uygulama Kimliğini kopyalama](media/authentication-and-authorization/active-directory-copy-application-id.png)](media/authentication-and-authorization/active-directory-copy-application-id.png#lightbox)

1. Seçin **anahtarları**, anahtar adı girin, sona erme seçip **Kaydet**.

   [![Uygulama anahtarları seçin](media/authentication-and-authorization/active-directory-application-keys.png)](media/authentication-and-authorization/active-directory-application-keys.png#lightbox)

   [![Süre sonu ve anahtar adını girin ve Kaydet'i seçin](media/authentication-and-authorization/active-directory-application-keys-save.png)](media/authentication-and-authorization/active-directory-application-keys-save.png#lightbox)

1. Anahtar, sık kullandığınız metin düzenleyiciye kopyalayın.

   [![Uygulama anahtarını kopyalama](media/authentication-and-authorization/active-directory-copy-application-key.png)](media/authentication-and-authorization/active-directory-copy-application-key.png#lightbox)

1. Zaman serisi görüşleri ortamı için seçin **veri erişimi ilkeleri** seçip **Ekle**.

   [![Zaman serisi görüşleri ortamına yeni bir veri erişim ilkesi Ekle](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. İçinde **Kullanıcı Seç** iletişim kutusu, yapıştırma 2. adımda uygulama adı veya 3. adımdaki uygulama kimliği.

   [![Kullanıcı Seç iletişim kutusunda uygulama Bul](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rolü seçin. Seçin **okuyucu** veri veya **katkıda bulunan** verileri sorgulamak ve başvuru verilerini değiştirmek için. **Tamam**’ı seçin.

   [![Okuyucu veya katkıda bulunan kullanıcı rolü Seç iletişim kutusunda seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Seçerek ilkeyi kaydedin **Tamam**.

1. Uygulama için bir belirteç almak için 3. adımdaki uygulama kimliği ve 5. adım uygulama anahtarı'nı kullanın. Belirteç, ardından geçirilebilir `Authorization` uygulama zaman serisi öngörüleri API çağırdığında başlığı.

    Kullanırsanız C#, uygulama için bir belirteç almak için aşağıdaki kodu kullanın. Eksiksiz bir örnek için bkz. [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).

    ```csharp
    // Enter your Active Directory tenant domain name
    var tenant = "YOUR_AD_TENANT.onmicrosoft.com";
    var authenticationContext = new AuthenticationContext(
        $"https://login.microsoftonline.com/{tenant}",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set the resource URI to the Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/",
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "YOUR_APPLICATION_ID",
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "YOUR_CLIENT_APPLICATION_KEY"));

    string accessToken = token.AccessToken;
    ```

Kullanım **uygulama kimliği** ve **anahtar** uygulamanızda Azure zaman serisi görüşleri ile kimlik doğrulaması.

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi öngörüleri API'yi çağıran örnek kod için bkz: [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).
- API başvuru bilgileri için bkz: [sorgu API'si başvurusu](/rest/api/time-series-insights/ga-query-api).
- Bilgi edinmek için nasıl [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).
