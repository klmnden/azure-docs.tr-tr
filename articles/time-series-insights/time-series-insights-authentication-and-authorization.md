---
title: "Kimlik doğrulaması ve Azure zaman serisi Insights API'si tarafından yetkilendirmek nasıl"
description: "Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi Öngörüler API çağrılarının özel bir uygulama için nasıl yapılandırılacağı açıklanmaktadır."
services: time-series-insights
ms.service: time-series-insights
author: dmdenmsft
ms.author: dmden
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/27/2017
ms.openlocfilehash: dd78e1e726029aaceef5aff0e0eed84acac646cf
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Kimlik doğrulama ve yetkilendirme Azure zaman serisi Insights API'si

Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi Öngörüler API çağrılarının özel bir uygulamada kullanılan nasıl yapılandırılacağı açıklanmaktadır.

## <a name="service-principal"></a>Hizmet sorumlusu

Bu bölümde, uygulama adına zaman serisi Insights API'si erişmek için bir uygulama yapılandırma açıklanmaktadır. Uygulama sonra verileri sorgulamak veya kullanıcı kimlik bilgilerini yerine uygulama kimlik bilgileri ile zaman serisi Öngörüler ortamında başvuru veri yayımlama.

Erişim zaman serisi Öngörüler gerekir bir uygulamanız varsa, Azure Active Directory Uygulama ayarlama ve veri erişimi ilkelerini zaman serisi Öngörüler ortamında atamanız gerekir. Bu yaklaşım, çünkü uygulama kendi kimlik bilgileri altında çalışırken için tercih edilir:

* Kendi izinlerinin farklı uygulama kimliği için izinleri atayabilirsiniz. Genellikle, bu izinler hangi uygulamanın yalnızca gerektirdiğini için kısıtlanır. Örneğin, yalnızca belirli bir zaman serisi Öngörüler ortamda verileri okumak uygulama izin verebilirsiniz.
* Sizin Sorumluluklarınız değiştirirseniz uygulamanın kimlik bilgilerini değiştirmek zorunda değilsiniz.
* Katılımsız betik çalışırken kimlik doğrulaması otomatikleştirmek için bir sertifika veya bir uygulama anahtarı kullanın.

Bu makalede Azure Portalı aracılığıyla bu adımların nasıl gerçekleştirileceğini gösterir. Uygulama yalnızca bir kuruluşta çalıştırmak için burada hedeflenen bir tek kiracılı uygulama odaklanır. Genellikle, kuruluşunuzda çalıştırmak satır iş kolu uygulamaları için tek Kiracı uygulamaları kullanın.

Kurulum akışında üç üst düzey adımları içerir:

1. Azure Active Directory'de bir uygulama oluşturun.
2. Bu uygulamanın zaman serisi Öngörüler ortam erişim yetkisi verir.
3. Bir belirteç almak için uygulama kimliği ve anahtarı kullanın `"https://api.timeseries.azure.com/"` İzleyici veya kaynak. Belirteç ardından zaman serisi Öngörüler API'sini çağırmak için kullanılabilir.

Ayrıntılı adımlar şunlardır:

1. Azure portalında seçin **Azure Active Directory** > **uygulama kayıtlar** > **yeni uygulama kaydı**.

   ![Azure Active Directory'de yeni uygulama kaydı](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Uygulama bir ad verin, olmasını seçin **Web uygulaması / API**, için geçerli bir URI seçin **oturum açma URL'si**, tıklatıp **oluşturma**.

   ![Azure Active Directory'de uygulama oluşturma](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Yeni oluşturulan uygulamanızı seçin ve uygulama Kimliğini, sık kullandığınız metin düzenleyicisine kopyalayın.

   ![Uygulama Kimliğini kopyalayın](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Seçin **anahtarları**anahtar adını girin, sona erme seçin ve tıklatın **kaydetmek**.

   ![Uygulama anahtarı seçin](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Süre sonu ve anahtar adını girin ve Kaydet'e tıklayın.](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Anahtarı, sık kullandığınız metin düzenleyicisine kopyalayın.

   ![Uygulama anahtarı kopyalayın](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Zaman serisi Öngörüler ortamı için seçin **veri erişimi ilkelerini** tıklatıp **Ekle**.

   ![Zaman serisi Öngörüler ortamına yeni veri erişim ilkesi Ekle](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. İçinde **Kullanıcı Seç** iletişim kutusunda, yapıştırma uygulama adı (2. adım) veya uygulama Kimliğinden (3. adım).

   ![Kullanıcı Seç iletişim kutusunda bir uygulama Bul](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Rol seçin (**okuyucu** veri sorgulama için **katkıda bulunan** veri sorgulama ve başvuru verileri değiştirme) tıklatıp **Tamam**.

   ![Okuyucu ve katkıda bulunan rolü Seç iletişim kutusunda seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Tıklayarak ilkeyi kaydedin **Tamam**.

10. Uygulama adına belirtecini almak için uygulama Kimliğinden (3. adım) ve uygulama anahtarından (5. adım) kullanın. Belirteç sonra geçirilebilir `Authorization` uygulama zaman serisi Insights API'si çağırdığında üstbilgi.

    C# kullanıyorsanız, uygulama adına belirtecini almak için aşağıdaki kodu kullanabilirsiniz. Tam bir örnek için bkz: [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
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

Uygulama kimliği ve anahtarı uygulamanızın Azure zaman serisi Insight ile kimlik doğrulaması yapmak için kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
- Zaman serisi Öngörüler API çağrılarının örnek kod için bkz: [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).
- API başvuru bilgileri için bkz: [sorgu API Başvurusu](/rest/api/time-series-insights/time-series-insights-reference-queryapi).

> [!div class="nextstepaction"]
> [Hizmet sorumlusu oluşturma](../azure-resource-manager/resource-group-create-service-principal-portal.md)
