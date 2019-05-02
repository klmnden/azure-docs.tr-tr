---
title: Kimlik doğrulama ve yetkilendirme Azure Time Series Insights API'si tarafından nasıl | Microsoft Docs
description: Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulama için nasıl yapılandırılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/27/2017
ms.custom: seodec18
ms.openlocfilehash: 66a3c40bf1e1e1dc6253520a555e19ebf011297c
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64698566"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Kimlik doğrulama ve yetkilendirme için Azure zaman serisi öngörüleri API'si

Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulamada kullanılan nasıl yapılandırılacağı açıklanmaktadır.

## <a name="service-principal"></a>Hizmet sorumlusu

Bu bölümde, zaman serisi öngörüleri API adına uygulamaya erişmek için bir uygulama yapılandırma açıklanmaktadır. Uygulama daha sonra verileri sorgulayabilir veya kullanıcı kimlik bilgileri yerine uygulama kimlik bilgileri ile zaman serisi görüşleri ortamına başvuru verilerini yayımlamak.

Erişim zaman serisi görüşleri gereken bir uygulamanız varsa, bir Azure Active Directory uygulaması oluşturma ve zaman serisi görüşleri ortamına veri erişimi ilkeleri atamanız gerekir. Bu yaklaşım olmadığından uygulama kendi kimlik bilgileriniz altında çalışan için tercih edilir:

* Kendi izinlerinden farklı uygulama kimliği izinler atayabilirsiniz. Genellikle, bu izinler için yalnızca ne gerektiren uygulama kısıtlanır. Örneğin, yalnızca belirli bir zaman serisi görüşleri ortamına verileri okumasına izin verebilirsiniz.
* Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmeniz gerekmez.
* Katılımsız betik çalıştırırken kimlik doğrulaması otomatikleştirmek için bir sertifika veya bir uygulama anahtarı'nı kullanabilirsiniz.

Bu makalede, Azure portalı üzerinden bu adımların nasıl gerçekleştirileceğini gösterir. Tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır. Tek kiracılı uygulamalar genellikle kuruluşunuzdaki satır iş kolu uygulamaları için kullanırsınız.

Bir kurulum akışında, üst düzey üç adımdan oluşur:

1. Azure Active Directory'de bir uygulama oluşturun.
2. Zaman serisi görüşleri ortamına erişmek için bu uygulamayı yetkilendirin.
3. İçin bir belirteç almak için uygulama kimliği ve anahtarı kullan `"https://api.timeseries.azure.com/"` hedef kitleden veya kaynaktan. Belirteç, daha sonra zaman serisi öngörüleri API'sini çağırmak için de kullanılabilir.

Ayrıntılı adımlar şunlardır:

1. Azure portalında **Azure Active Directory** > **uygulama kayıtları** > **yeni uygulama kaydı**.

   ![Azure Active Directory'de yeni uygulama kaydı](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Uygulama bir ad verin, türünün olmasını seçin **Web uygulaması / API**, için geçerli bir URI seçin **oturum açma URL'si**, tıklatıp **Oluştur**.

   ![Azure Active Directory'de uygulama oluşturma](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Yeni oluşturulan uygulamanızı seçin ve uygulama Kimliğini, sık kullandığınız metin düzenleyicisine kopyalayın.

   ![Uygulama Kimliğini kopyalama](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Seçin **anahtarları**, anahtar adı girin, sona erme seçin ve tıklayın **Kaydet**.

   ![Uygulama anahtarları seçin](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Süre sonu ve anahtar adını girin ve Kaydet'e tıklayın](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Anahtar, sık kullandığınız metin düzenleyiciye kopyalayın.

   ![Uygulama anahtarını kopyalama](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Zaman serisi görüşleri ortamı için seçin **veri erişimi ilkeleri** tıklatıp **Ekle**.

   ![Zaman serisi görüşleri ortamına yeni bir veri erişim ilkesi Ekle](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. İçinde **Kullanıcı Seç** iletişim kutusu, yapıştırma uygulama adı (2. adım) veya uygulama kimliği (', 3. adım).

   ![Kullanıcı Seç iletişim kutusunda uygulama Bul](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Rol seçin (**okuyucu** verileri sorgulamak için **katkıda bulunan** verileri sorgulama ve başvuru verilerini değiştirmek için) ve tıklayın **Tamam**.

   ![Okuyucu veya katkıda bulunan rolü Seç iletişim kutusunda seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Tıklayarak ilkeyi kaydedin **Tamam**.

10. Uygulama adına belirteci almak için uygulama kimliği (3. adımdaki) ve uygulama anahtarından (5. adım) kullanın. Belirteç, ardından geçirilebilir `Authorization` uygulama zaman serisi öngörüleri API çağırdığında başlığı.

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
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of the application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

Uygulama kimliği ve anahtarı Azure zaman serisi görüşleri ile kimlik doğrulaması için uygulamanızı kullanın. 

## <a name="next-steps"></a>Sonraki adımlar

- Zaman serisi öngörüleri API'yi çağıran örnek kod için bkz: [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).

- API başvuru bilgileri için bkz: [sorgu API'si başvurusu](/rest/api/time-series-insights/ga-query-api).

- Bilgi edinmek için nasıl [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).