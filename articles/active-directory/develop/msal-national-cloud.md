---
title: Microsoft Authentication Library (MSAL) Ulusal bulutlarda - Microsoft kimlik platformu kullanın
description: Microsoft Authentication Library (MSAL), uygulama geliştiricilerin güvenli web API'lerini çağırma için belirteçlerini almak sağlar. Bu web API'leri, Microsoft Graph, diğer Microsoft APIS, üçüncü taraf web API'leri veya web API olabilir. MSAL, birden çok uygulama mimarileri ve platformları destekler.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 18eccd6b6d29a43cc3fa13d133d449bf0b9be657
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075798"
---
# <a name="use-msal-in-national-cloud-environment"></a>MSAL Ulusal bulut ortamında kullanın

[Ulusal Bulutlar](authentication-national-cloud.md) fiziksel olarak yalıtılmış Azure örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını emin olmak için tasarlanmıştır.

Microsoft Dünya çapında buluta ek olarak Microsoft Authentication Library (MSAL) kimlik doğrulaması ve güvenli web API'lerini çağırma için belirteçlerini almak, uygulama geliştiricilerin Ulusal bulutlarda da sağlar. Bu web API'leri, Microsoft Graph veya diğer Microsoft APIS olabilir.

Genel bulut dahil olmak üzere, Azure Active Directory (Azure AD) aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure US Government
- Azure Çin 21Vianet
- Azure Almanya

Bu kılavuz, iş ve Okul hesaplarında oturum, bir erişim belirteci alma ve Microsoft Graph API'sini çağırmak gösterilmiştir [ABD kamu için Microsoft bulut](https://azure.microsoft.com/global-infrastructure/government/) ortam.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki önkoşulları karşıladığınızdan emin olun.

### <a name="choose-the-appropriate-identities"></a>Uygun bir kimlik seçin

[Azure kamu](https://docs.microsoft.com/azure/azure-government/) uygulamalar Azure AD genel kimlikleri yanı sıra, Azure AD kamu kimlikleri kullanıcıların kimliğini doğrulamak için kullanabilirsiniz. Bu kimliklerin kullanabildiğinden, anlamanıza ve senaryonuz için seçmelidir yetkili uç noktası karar vermeniz gerekir:

- Azure AD genel: Kuruluşunuzda zaten Office 365 destek için bir Azure AD genel Kiracı (genel veya GCC) veya başka bir uygulama varsa yaygın olarak kullanılır.
- Azure AD kamu: Kuruluşunuzda zaten Office 365 (GCC yüksek veya DoD) desteklemek için bir Azure AD kamu kiracısı varsa veya yeni bir kiracı Azure AD Kamu'da oluşturmakta olduğunuz yaygın olarak kullanılır.

Karar sonra özel uygulama kaydınızı gerçekleştirmek burada noktadır. Azure kamu uygulamanız için Azure AD genel kimlikleri seçerseniz, uygulamayı Azure AD genel kiracınıza kaydetmeniz gerekir.

### <a name="get-an-azure-government-subscription"></a>Bir Azure kamu aboneliği edinme

- Bir Azure kamu aboneliği edinmek için bkz. [yönetmek ve Azure kamu, aboneliğinize bağlanma](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).
- Bir Azure kamu aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/global-infrastructure/government/request/) başlamadan önce.

## <a name="javascript"></a>JavaScript

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

1. Oturum [Azure portalında](https://portal.azure.us/) bir uygulamayı kaydetme.
    1. Diğer Ulusal Bulutlar için Azure portal uç noktalarını bulmak için bkz: [uygulama kayıt uç noktaları](authentication-national-cloud.md#app-registration-endpoints)

1. Kiracı, erişmek için birden fazla Kiracı, sağ üst köşedeki hesabınızı seçin ve istenen Azure AD'ye portal oturumunuzu ayarlama, hesap sağlar.
1. Geliştiriciler için Microsoft identity platformuna gidin [uygulama kayıtları](https://aka.ms/ra/ff) sayfası.
1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizini hesaplarında**.
1. Altında **yeniden yönlendirme URI'si** bölümünden **Web** platform ve uygulama URL'sine değerine göre web sunucunuzda kümesi. Ayarlayın ve Visual Studio ve düğüm yeniden yönlendirme URL'sini almak hakkında yönergeler için aşağıdaki bölümlere bakın.
1. Bittiğinde **Kaydet**’i seçin.
1. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** değeri.
1. Bu öğretici [örtük izin akışı](v2-oauth2-implicit-grant-flow.md) etkinleştirilecek. Kayıtlı uygulama sol gezinti bölmesinde seçin **kimlik doğrulaması**.
1. İçinde **Gelişmiş ayarlar**altında **örtük vermeyi**, her ikisini de etkinleştirmek **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutularını. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırmak sonun kimliği ve erişim belirteçler gereklidir.
1. **Kaydet**’i seçin.

### <a name="step-2--set-up-your-web-server-or-project"></a>2. Adım:  Web sunucusu veya proje ayarlama

- [Proje dosyalarını indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) düğüm gibi bir yerel web sunucusu için.

  or

- [Visual Studio projeyi indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

Ve ardından atlamak [', JavaScript SPA'ya Yapılandır'](#step-4-configure-your-javascript-spa) çalıştırmadan önce kodu örneği yapılandırmak için.

### <a name="step-3-use-the-microsoft-authentication-library-msal-to-sign-in-the-user"></a>3. Adım: Kullanıcının oturum açmak için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma

İzleyeceğiniz adımlar [Javascript öğretici](tutorial-v2-javascript-spa.md#create-your-project) projenizi oluşturmak ve tümleştirme ile Microsoft kimlik doğrulama kitaplığı (kullanıcının oturum açmak için MSAL).

### <a name="step-4-configure-your-javascript-spa"></a>4. Adım: JavaScript SPA'ya yapılandırın

İçinde `index.html` dosyası proje Kurulum sırasında oluşturulur, uygulama kayıt bilgilerini ekleyin. Üst içinde aşağıdaki kodu ekleyin `<script></script>` gövdesinde etiketleri, `index.html` dosyası:

```javascript
const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.us/Enter_the_Tenant_Info_Here",
        }
}

const graphConfig = {
        graphEndpoint: "https://graph.microsoft.us",
        graphScopes: ["user.read"],
}

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Konumlar:

- `Enter_the_Application_Id_here` - kaydettiğiniz uygulamanın **Uygulama (istemci) Kimliği** değeridir.
- `Enter_the_Tenant_Info_Here` - aşağıdaki seçeneklerden birine ayarlanır:
    - Uygulamanız destekliyorsa **kuruluş bu dizinde hesapları**, bu değeri ile değiştirin **Kiracı kimliği** veya **Kiracı adı** (örneğin, contoso.microsoft.com)
    - Uygulamanız **Herhangi bir kuruluş dizinindeki hesaplar** yaklaşımını destekliyorsa bu değeri `organizations` ile değiştirin
    -  Ulusal Bulutlar için kimlik doğrulama uç noktaları öğrenmek için bkz [Azure AD kimlik doğrulama uç noktaları](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints)

     > [!NOTE]
     > Kişisel Microsoft hesapları (MSA) senaryoları Ulusal bulutlarda desteklenmez.
  
-   `graphEndpoint` -ABD kamu için Microsoft bulut için Microsoft Graph uç noktadır.
    -  Ulusal Bulutlar için Microsoft Graph uç noktaları öğrenmek için bkz [Ulusal bulutta Microsoft Graph uç noktaları](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints)

## <a name="net"></a>.NET

MSAL .NET kullanıcılarının oturumunu, belirteci almak ve Ulusal bulutlarda Microsoft Graph API'sini çağırmak olanak tanır.

Aşağıdaki öğreticiye 'iş ve Okul' hesaplarını Ulusal bulutlara ait kuruluşlarındaki kullanıcıların oturum açmak için Openıd Connect kullanan bir .NET Core 2.2 MVC Web uygulamasının nasıl oluşturulacağını gösterir.

- Kullanıcıların oturum açmak ve belirteci almak için izleyin [Bu öğreticide](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Microsoft Graph API'sini çağırmak için izleyin [Bu öğreticide](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure Devlet Kurumları](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)