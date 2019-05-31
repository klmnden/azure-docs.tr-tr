---
title: Microsoft Authentication Library (MSAL) Ulusal bulutlarda - Microsoft kimlik platformu kullanın
description: Microsoft Authentication Library (MSAL), uygulama geliştiricilerin güvenli web API'lerini çağırma için belirteçlerini almak sağlar. Bu web API'leri, Microsoft Graph, diğer Microsoft APIs, iş ortağı web API'leri veya web API olabilir. MSAL, birden çok uygulama mimarileri ve platformları destekler.
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
ms.openlocfilehash: f9958356cae3c486ecf68e280f33d63c6a537b14
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235258"
---
# <a name="use-msal-in-a-national-cloud-environment"></a>MSAL Ulusal bulut ortamında kullanın

[Ulusal Bulutlar](authentication-national-cloud.md) fiziksel olarak yalıtılmış Azure örnekleridir. Bu Azure bölgelerine veri yerleşikliği, özerkliği ve uyumluluk gereksinimlerini coğrafi bölge içinde karşılanmasını sağlanmasına yardımcı olur.

Microsoft Dünya çapında bulutun yanı sıra, Ulusal bulutlarda uygulama geliştiricileri için kimlik doğrulaması ve güvenli web API'lerini çağırma belirteçlerini almak Microsoft kimlik doğrulama kitaplığı (MSAL) sağlar. Bu web API'leri, Microsoft Graph veya diğer Microsoft APIs olabilir.

Genel bulut dahil olmak üzere, Azure Active Directory (Azure AD) aşağıdaki Ulusal bulutlarda dağıtılır:  

- Azure Kamu
- Azure Çin 21Vianet
- Azure Almanya

Bu kılavuz, iş ve Okul hesapları, bir erişim belirteci alma ve Microsoft Graph API'sini çağırmak oturum açma gösterir [Azure kamu Bulutu](https://azure.microsoft.com/global-infrastructure/government/) ortam.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce bu önkoşulların karşılandığından emin olun.

### <a name="choose-the-appropriate-identities"></a>Uygun bir kimlik seçin

[Azure kamu](https://docs.microsoft.com/azure/azure-government/) uygulamalar Azure AD kamu kimlikleri ve Azure AD genel kimlikleri kullanıcıların kimliğini doğrulamak için kullanabilirsiniz. Bu kimliklerin kullanabileceğinizden, senaryonuz için seçmelidir yetkili uç noktası karar vermeniz gerekir:

- Azure AD genel: Kuruluşunuzda zaten Office 365 destek için bir Azure AD genel Kiracı (genel veya GCC) veya başka bir uygulama varsa yaygın olarak kullanılır.
- Azure AD kamu: Kuruluşunuz zaten bir Azure AD kamu kiracısı için destek Office 365 (GCC yüksek veya DoD) varsa veya yeni bir kiracı, Azure AD devlet kurumları'nda oluşturma yaygın olarak kullanılır.

Karar verdikten sonra bir özel uygulama kaydınızı gerçekleştirmek burada noktadır. Azure kamu uygulamanız için Azure AD genel kimlikleri seçerseniz, uygulamayı Azure AD genel kiracınıza kaydetmeniz gerekir.

### <a name="get-an-azure-government-subscription"></a>Bir Azure kamu aboneliği edinme

Bir Azure kamu aboneliği edinmek için bkz. [yönetme ve Azure kamu, aboneliğinize bağlanma](https://docs.microsoft.com/azure/azure-government/documentation-government-manage-subscriptions).

Bir Azure kamu aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/global-infrastructure/government/request/) başlamadan önce.

## <a name="javascript"></a>JavaScript

### <a name="step-1-register-your-application"></a>1. adım: Uygulamanızı kaydetme

1. [Azure Portal](https://portal.azure.us/) oturum açın.
    
   Diğer Ulusal Bulutlar için Azure portal uç noktalarını bulmak için bkz: [uygulama kayıt uç noktaları](authentication-national-cloud.md#app-registration-endpoints).

1. Kiracı, erişmek için birden fazla Kiracı, sağ üst köşedeki hesabınızı seçin ve istenen Azure AD'ye portal oturumunuzu ayarlama, hesap sağlar.
1. Git [uygulama kayıtları](https://aka.ms/ra/ff) geliştiriciler için Microsoft kimlik platformu sayfasında.
1. Zaman **bir uygulamayı kaydetme** sayfası görüntülenirse, uygulamanız için bir ad girin.
1. Altında **desteklenen hesap türleri**seçin **herhangi bir kuruluş dizini hesaplarında**.
1. İçinde **yeniden yönlendirme URI'si** bölümünden **Web** platform ve uygulama URL'sine değerine göre web sunucunuzda kümesi. Ayarlayın ve Visual Studio ve düğüm yeniden yönlendirme URL'sini almak hakkında yönergeler için bir sonraki bölüme bakın.
1. **Kaydol**’u seçin.
1. Uygulamasında **genel bakış** sayfa, Not **uygulama (istemci) kimliği** değeri.
1. Bu öğretici etkinleştirmenizi gerektirir [örtük verme akışı](v2-oauth2-implicit-grant-flow.md). Kayıtlı uygulama sol bölmesinde seçin **kimlik doğrulaması**.
1. İçinde **Gelişmiş ayarlar**altında **örtük vermeyi**seçin **kimlik belirteçlerini** ve **erişim belirteçlerini** onay kutuları. Bu uygulama kullanıcılarının oturumunu ve bir API'yi çağırması gerekir çünkü kimlik ve erişim belirteçler gereklidir.
1. **Kaydet**’i seçin.

### <a name="step-2--set-up-your-web-server-or-project"></a>2. adım:  Web sunucusu veya proje ayarlama

- [Proje dosyalarını indirme](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/quickstart.zip) düğüm gibi bir yerel web sunucusu için.

  or

- [Visual Studio projeyi indirmek](https://github.com/Azure-Samples/active-directory-javascript-graphapi-v2/archive/vsquickstart.zip).

Ardından atlamak [, JavaScript SPA'ya yapılandırma](#step-4-configure-your-javascript-spa) çalıştırmadan önce kodu örneği yapılandırmak için.

### <a name="step-3-use-the-microsoft-authentication-library-to-sign-in-the-user"></a>3. adım: Kullanıcının oturum açmak için Microsoft kimlik doğrulama kitaplığını kullanma

İzleyeceğiniz adımlar [JavaScript öğretici](tutorial-v2-javascript-spa.md#create-your-project) projenizi oluşturma ve kullanıcı oturum açmak için MSAL tümleştirin.

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

Bu kodu:

- `Enter_the_Application_Id_here` olan **uygulama (istemci) kimliği** , kayıtlı uygulama için değer.
- `Enter_the_Tenant_Info_Here` Aşağıdaki seçeneklerden birine ayarlayın:
    - Uygulamanız destekliyorsa **hesapları kuruluş bu dizinde**, bu değer Kiracı Kimliğiyle değiştirin veya Kiracı adı (örneğin, contoso.microsoft.com).
    - Uygulamanız destekliyorsa **herhangi bir kuruluş dizini hesaplarında**, bu değeri ile değiştirin `organizations`.
    
    Ulusal Bulutlar için kimlik doğrulama uç noktaları öğrenmek için bkz [Azure AD kimlik doğrulama uç noktaları](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud#azure-ad-authentication-endpoints).

    > [!NOTE]
    > Ulusal bulutlarda kişisel Microsoft hesapları desteklenmez.
  
- `graphEndpoint` ABD kamu için Microsoft bulut için Microsoft Graph uç noktadır.

   Ulusal Bulutlar için Microsoft Graph uç noktaları öğrenmek için bkz [Microsoft Graph uç noktaları Ulusal bulutlarda](https://docs.microsoft.com/graph/deployments#microsoft-graph-and-graph-explorer-service-root-endpoints).

## <a name="net"></a>.NET

Kullanıcılarının oturumunu belirteçlerini almak ve Ulusal bulutlarda Microsoft Graph API'sini çağırmak için MSAL.NET kullanabilirsiniz.

Aşağıdaki öğreticilerde, bir .NET Core 2.2 MVC Web uygulamasının nasıl oluşturulacağını gösterir. Uygulama, kullanıcıların bir Ulusal buluta ait bir kuruluşta bir iş ve Okul hesabı oturum açmak için Openıd Connect kullanır.

- Kullanıcıların oturum açmak ve belirteçleri almak için izleyin [Bu öğreticide](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-4-Sovereign#build-an-aspnet-core-web-app-signing-in-users-in-sovereign-clouds-with-the-microsoft-identity-platform).
- Microsoft Graph API'sini çağırmak için izleyin [Bu öğreticide](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/2-WebApp-graph-user/2-4-Sovereign-Call-MSGraph#using-the-microsoft-identity-platform-to-call-the-microsoft-graph-api-from-an-an-aspnet-core-2x-web-app-on-behalf-of-a-user-signing-in-using-their-work-and-school-account-in-microsoft-national-cloud).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi:

- [Azure Devlet Kurumları](https://docs.microsoft.com/azure/azure-government/)
- [Azure Çin 21Vianet](https://docs.microsoft.com/azure/china/)
- [Azure Almanya](https://docs.microsoft.com/azure/germany/)