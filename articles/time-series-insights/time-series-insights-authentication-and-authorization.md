---
title: Kimlik doğrulama ve yetkilendirme Azure Time Series Insights içinde bir API kullanarak | Microsoft Docs
description: Bu makalede, kimlik doğrulama ve yetkilendirme Azure zaman serisi öngörüleri API'yi çağıran özel bir uygulama için nasıl yapılandırılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/29/2019
ms.custom: seodec18
ms.openlocfilehash: 899bcffaf3a54bd541d488f99c35ec6721d751ca
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543963"
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Kimlik doğrulama ve yetkilendirme için Azure zaman serisi öngörüleri API'si

Bu belge, Azure yeni Azure Active Directory dikey penceresini kullanarak Active Directory içinde bir uygulamayı kaydetme açıklar. Azure Active Directory'de kayıtlı uygulamalar kullanıcıların kimlik doğrulaması ve bir zaman serisi görüşleri ortamı ile ilişkili Azure zaman serisi Insight API kullanmak üzere yetki verilmesini sağlar.

## <a name="service-principal"></a>Hizmet sorumlusu

Aşağıdaki bölümlerde, bir uygulama adına zaman serisi öngörüleri API erişmek için bir uygulama yapılandırma açıklanmaktadır. Uygulama ardından sorgulayabilir veya Azure Active Directory aracılığıyla kendi uygulama kimlik bilgilerini kullanarak Time Series Insights ortamdaki başvuru verilerini yayımlama.

## <a name="summary-and-best-practices"></a>Özet ve en iyi yöntemler

Azure Active Directory uygulama kayıt akışı üç ana adımdan oluşur.

1. [Bir uygulamayı kaydetme](#azure-active-directory-app-registration) Azure Active Directory'de.
1. Uygulamayı Yetkilendir [zaman serisi görüşleri ortamına veri erişimi](#granting-data-access).
1. Kullanım **uygulama kimliği** ve **gizli** uygulamasından bir belirteç almak üzere `https://api.timeseries.azure.com/` içinde [istemci uygulaması](#client-app-initialization). Belirteç, daha sonra zaman serisi öngörüleri API'sini çağırmak için de kullanılabilir.

Başına **3. adım**, uygulamanızın ve kullanıcı kimlik bilgilerinizi ayırma olanak tanır:

* Kendi izinlerinden ayrı uygulama kimliğine izinleri atayın. Genellikle, bu izinler için yalnızca ne gerektiren uygulama kısıtlanır. Örneğin, yalnızca belirli bir zaman serisi görüşleri ortamından veri okumasına izin verebilirsiniz.
* Uygulamanın güvenlik oluşturma kullanıcının kimlik doğrulama bilgilerini kullanarak yalıtmak bir **gizli** veya güvenlik sertifikası. Sonuç olarak, uygulamanın kimlik bilgileri, belirli bir kullanıcının kimlik bilgilerine bağlı değildir. Kullanıcı rolü değişirse, uygulamanın yeni kimlik bilgileri veya daha fazla yapılandırma mutlaka gerektirmez. Tüm uygulama erişimi, kullanıcı parolasını değiştirirse, yeni kimlik bilgilerine veya anahtarlara gerektirmez.
* Katılımsız betik kullanarak çalıştırın bir **gizli** veya güvenlik sertifika yerine belirli bir kullanıcının kimlik bilgilerini (bulunması gerek).
* Azure zaman serisi öngörüleri API'NİZİN güvenliğini sağlamak için parola yerine bir güvenlik sertifikası'nı kullanın.

> [!IMPORTANT]
> İlkesini izleyin **ayrımı, ile ilgili sorunlar** (yukarıdaki senaryo için bu ne zaman açıklanmıştır) Azure Time Series Insights güvenlik ilkesini yapılandırma.

> [!NOTE]
> * Bu makalede tek kiracılı bir uygulama yalnızca bir kuruluş içinde çalıştırmak için uygulamayı nerede yöneliktir odaklanır.
> * Tek kiracılı uygulamalar, kuruluşunuzda satır iş kolu uygulamaları için genellikle kullanacaksınız.

## <a name="detailed-setup"></a>Ayrıntılı Kurulum

### <a name="azure-active-directory-app-registration"></a>Azure Active Directory Uygulama kaydı

[!INCLUDE [Azure Active Directory app registration](../../includes/time-series-insights-aad-registration.md)]

### <a name="granting-data-access"></a>Veri erişimi verme

1. Zaman serisi görüşleri ortamı için seçin **veri erişimi ilkeleri** seçip **Ekle**.

   [![Zaman serisi görüşleri ortamına yeni bir veri erişim ilkesi Ekle](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png#lightbox)

1. İçinde **Kullanıcı Seç** iletişim kutusunda, ya da yapıştırın **uygulama adı** veya **uygulama kimliği** Azure Active Directory uygulama kayıt bölümünden.

   [![Kullanıcı Seç iletişim kutusunda uygulama Bul](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png#lightbox)

1. Rolü seçin. Seçin **okuyucu** veri veya **katkıda bulunan** verileri sorgulamak ve başvuru verilerini değiştirmek için. **Tamam**’ı seçin.

   [![Okuyucu veya katkıda bulunan kullanıcı rolü Seç iletişim kutusunda seçin](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png#lightbox)

1. Seçerek ilkeyi kaydedin **Tamam**.

   > [!TIP]
   > Hakkında bilgi edinin [veri erişimi verme](./time-series-insights-data-access.md) zaman serisi görüşleri ortamınıza Azure Active Directory'de.

### <a name="client-app-initialization"></a>İstemci uygulama başlatma

1. Kullanım **uygulama kimliği** ve **gizli** (uygulama anahtarı) bölümünden uygulama adına belirteci almak için Azure Active Directory uygulama kayıt.

    İçinde C#, aşağıdaki kod uygulama adına bir belirteç elde edebilirsiniz. Eksiksiz bir örnek için bkz. [C# kullanarak veri sorgulama](time-series-insights-query-data-csharp.md).

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

1. Belirteç, ardından geçirilebilir `Authorization` uygulama zaman serisi öngörüleri API çağırdığında başlığı.

## <a name="next-steps"></a>Sonraki adımlar

- GA zaman serisi öngörüleri API'yi çağıran örnek kod için bkz: [sorgu kullanarak verileri C# ](./time-series-insights-query-data-csharp.md).

- Önizleme zaman serisi öngörüleri API kod örnekleri için bkz. [sorgu Önizleme verileri kullanarak C# ](./time-series-insights-update-query-data-csharp.md).

- API başvuru bilgileri için bkz: [sorgu API'si başvurusu](/rest/api/time-series-insights/ga-query-api).

- Bilgi edinmek için nasıl [hizmet sorumlusu oluşturma](../active-directory/develop/howto-create-service-principal-portal.md).