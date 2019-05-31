---
title: Kimlik doğrulama ve yetkilendirme Azure Time Series Insights API'si tarafından nasıl | Microsoft Docs
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
ms.openlocfilehash: 9b6cd993e9f6c6dbf173c161de638c6c4a8b18d3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237045"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Kimlik doğrulama ve yetkilendirme için Azure zaman serisi öngörüleri API'si

Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulamada kullanılan nasıl yapılandırılacağı açıklanmaktadır.

> [!TIP]
> Hakkında bilgi edinin [veri erişimi verme](./time-series-insights-data-access.md) zaman serisi görüşleri ortamınıza Azure Active Directory'de.

## <a name="service-principal"></a>Hizmet sorumlusu

Bu ve aşağıdaki bölümlerde, zaman serisi öngörüleri API adına uygulamaya erişmek için bir uygulama yapılandırma açıklanmaktadır. Uygulama ardından sorgulayabilir veya kullanıcı kimlik bilgileri yerine uygulama kimlik bilgileri ile zaman serisi görüşleri ortamdaki başvuru verilerini yayımlama.

## <a name="best-practices"></a>En iyi uygulamalar

Erişim zaman serisi görüşleri gereken bir uygulama varsa:

1. Bir Azure Active Directory uygulaması ayarlayın.
1. [Veri erişimi ilkeleri atama](./time-series-insights-data-access.md) zaman serisi görüşleri ortamı içinde.

Kullanıcı kimlik bilgilerinizi yerine uygulama itibaren tercih edilir:

* Uygulama kimliği, kendi izinlerinden ayrı izinler atayabilirsiniz. Genellikle, bu izinler için yalnızca ne gerektiren uygulama kısıtlanır. Örneğin, yalnızca belirli bir zaman serisi görüşleri ortamına verileri okumasına izin verebilirsiniz.
* Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmek zorunda değilsiniz.
* Katılımsız betik çalıştırırken kimlik doğrulaması otomatikleştirmek için bir sertifika veya bir uygulama anahtarı'nı kullanabilirsiniz.

Aşağıdaki bölümlerde, Azure portalı üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Bu makalede tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Tek kiracılı uygulamalar, kuruluşunuzda satır iş kolu uygulamaları için genellikle kullanacaksınız.

## <a name="set-up-summary"></a>Özeti ayarlayın

Bir kurulum akışında üç adımdan oluşur:

1. Azure Active Directory'de bir uygulama oluşturun.
1. Zaman serisi görüşleri ortamına erişmek için bu uygulamayı yetkilendirin.
1. Uygulamasından bir belirteç almak üzere uygulama kimliği ve anahtarı kullan `https://api.timeseries.azure.com/`. Belirteç, daha sonra zaman serisi öngörüleri API'sini çağırmak için de kullanılabilir.

## <a name="detailed-setup"></a>Ayrıntılı Kurulum

1. Azure portalında **Azure Active Directory** > **uygulama kayıtları** > **yeni uygulama kaydı**.

   [![Azure Active Directory'de yeni uygulama kaydı](media/authentication-and-authorization/active-directory-new-application-registration.png)](media/authentication-and-authorization/active-directory-new-application-registration.png#lightbox)

1. Uygulama bir ad verin, türünün olmasını seçin **Web uygulaması / API**, için geçerli bir URI seçin **oturum açma URL'si**, tıklatıp **Oluştur**.

   [![Azure Active Directory'de uygulama oluşturma](media/authentication-and-authorization/active-directory-create-web-api-application.png)](media/authentication-and-authorization/active-directory-create-web-api-application.png#lightbox)

1. Yeni oluşturulan uygulamanızı seçin ve uygulama Kimliğini, sık kullandığınız metin düzenleyicisine kopyalayın.

   [![Uygulama Kimliğini kopyalama](media/authentication-and-authorization/active-directory-copy-application-id.png)](media/authentication-and-authorization/active-directory-copy-application-id.png#lightbox)

1. Seçin **anahtarları**, anahtar adı girin, sona erme seçin ve tıklayın **Kaydet**.

   [![Uygulama anahtarları seçin](media/authentication-and-authorization/active-directory-application-keys.png)](media/authentication-and-authorization/active-directory-application-keys.png#lightbox)

   [![Süre sonu ve anahtar adını girin ve Kaydet'e tıklayın](media/authentication-and-authorization/active-directory-application-keys-save.png)](media/authentication-and-authorization/active-directory-application-keys-save.png#lightbox)

1. Anahtar, sık kullandığınız metin düzenleyiciye kopyalayın.

   [![Uygulama anahtarını kopyalama](media/authentication-and-authorization/active-directory-copy-application-key.png)](media/authentication-and-authorization/active-directory-copy-application-key.png#lightbox)

1. Zaman serisi görüşleri ortamı için seçin **veri erişimi ilkeleri** tıklatıp **Ekle**.

   [![Zaman serisi görüşleri ortamına yeni bir veri erişim ilkesi Ekle](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. İçinde **Kullanıcı Seç** iletişim kutusunda, uygulama adı yapıştırın (gelen **2. adım**) veya uygulama kimliği (gelen **3. adım**).

   [![Kullanıcı Seç iletişim kutusunda uygulama Bul](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rol seçin (**okuyucu** verileri sorgulamak için **katkıda bulunan** verileri sorgulama ve başvuru verilerini değiştirmek için) ve tıklayın **Tamam**.

   [![Okuyucu veya katkıda bulunan rolü Seç iletişim kutusunda seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Tıklayarak ilkeyi kaydedin **Tamam**.

1. Uygulama kimliğini (gelen **3. adım**) ve uygulama anahtarı (gelen **5. adım**) uygulama adına belirteci almak için. Belirteç, ardından geçirilebilir `Authorization` uygulama zaman serisi öngörüleri API çağırdığında başlığı.

    C# kullanıyorsanız, uygulama adına belirteci almak için aşağıdaki kodu kullanabilirsiniz. Eksiksiz bir örnek için bkz. [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).

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
