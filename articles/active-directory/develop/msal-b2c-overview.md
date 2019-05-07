---
title: Azure AD B2C ile tümleştirmeyi öğrenin Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma
description: Microsoft Authentication Library (MSAL), Azure AD B2C ile tümleştirin ve güvenli Web API'lerini çağırma için belirteçlerini almak uygulama geliştiricilerin sağlar. Bu Web API'leri, Microsoft Graph, diğer Microsoft APIS, üçüncü taraf Web API'leri veya kendi Web API'nizi olabilir.
services: active-directory
documentationcenter: dev-center-name
author: negoe
manager: celested
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/04/2019
ms.author: negoe
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 87bd8834f840c2246bf3adc1d1f9cd9b8f635915
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191012"
---
# <a name="integrate-microsoft-authentication-library-msal-with-azure-active-directory-b2c"></a>Microsoft kimlik doğrulama kitaplığı (MSAL) Azure Active Directory B2C ile tümleştirin

Microsoft Authentication Library (MSAL) kullanarak sosyal ve yerel kimlikler ile kullanıcıların kimliğini doğrulamak, uygulama geliştiricilerinin sağlayan [Azure Active Directory (Azure AD) B2C](https://docs.microsoft.com/azure/active-directory-b2c/). Azure Active Directory (Azure AD) B2C, müşterilerin uygulamalarınızı kullanırken nasıl kaydolduğunu, oturum açtığını ve profillerini yönettiğini özelleştirip denetlemenizi sağlayan bir kimlik yönetimi sistemidir.

Azure Active Directory (Azure AD) B2C, marka ve kullanıcı arabirimi (UI) özelleştirmek müşterinize sorunsuz bir deneyim sağlamak için uygulamalarınızı tanır.

Bu öğretici, Azure Active Directory (Azure AD) B2C ile tümleştirmek için Microsoft kimlik doğrulama kitaplığı (MSAL) kullanma işlemini gösterir.


## <a name="prerequisites"></a>Önkoşullar

Henüz kendi oluşturmadıysanız [Azure AD B2C Kiracısı](https://docs.microsoft.com/azure/active-directory-b2c/tutorial-create-tenant), şimdi oluşturun. Mevcut bir Azure AD B2C kiracınızı kullanabilirsiniz. 

## <a name="javascript"></a>Javascript

Aşağıdaki adımları nasıl bir tek sayfalı uygulama Azure AD B2C kullanıcı kaydolma, oturum açma ve kullanabileceğiniz korumalı web API'si çağırma gösterilmektedir.

### <a name="step-1-register-your-application"></a>1. Adım: Uygulamanızı kaydetme

Kimlik doğrulaması uygulamak için öncelikle uygulamanızı kaydetmeniz gerekir. Uygulamanızı kaydetmek için izleyin [uygulamanızı kaydetmeniz](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#step-4-register-your-own-web-application-with-azure-ad-b2c) ayrıntılı adımlar için.

### <a name="steps-2-download-applications"></a>2. adımları: Uygulamaları yükle

Zip dosyasını indirin veya örnek github'dan kopyalayabilirsiniz.
>Git kopya https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp.git

### <a name="steps-3-authentication"></a>Adım 3: Kimlik Doğrulaması

1. Aşağıdaki örnekte index.html dosyasını açın.

2. Örnek uygulama kimliği ve uygulamanızı kaydedilirken daha önce kaydettiğiniz anahtarını yapılandırın. API'ler ve dizin adları ile değerlerini değiştirerek aşağıdaki kod satırlarını değiştirin:

```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://login.microsoftonline.com/tfp/<your-tenant-name>.onmicrosoft.com/<your-sign-in-sign-up-policy>",
        b2cScopes: ["https://<your-tenant-name>.onmicrosoft.com/hello/demo.read"],
        webApi: 'http://localhost:5000/hello',
  };

// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

Adını [kullanıcı akışı](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies) Bu öğreticide B2C_1_signupsignin1 kullanılır. Farklı bir userjourney adı kullanıyorsanız, kullanıcı akışı adınızı yetkilisi değeri kullanın.


### <a name="configure-application-to-use-b2clogincom"></a>Kullanmak için uygulamayı yapılandırma `b2clogin.com`

Kullanabileceğiniz `b2clogin.com` yerine `login.microsoftonline.com` bir yeniden yönlendirme olarak bir kimlik sağlayıcısı için kaydolma ve oturum açma, Azure Active Directory (Azure AD) B2C uygulamanızda ayarladığınızda URL'si.

**`b2clogin.com`** yani 
`https://your-tenant-name.b2clogin.com/your-tenant-guid` şunlar için kullanılır:

- Tanımlama bilgisi üstbilgisi içinde Microsoft hizmetler tarafından kullanılan alanı azalır.
- URL'nizde artık Microsoft bir başvuru içerir. Örneğin, Azure AD B2C'yi uygulamanız login.microsoftonline.com için büyük olasılıkla başvuruyor


 'B2clogin.com' kullanmak için uygulamanızı yapılandırmasını güncelleştirmeniz gerekiyor.  

1. Güncelleştirme ValidateAuthority: ayarlayın **validateAuthority** özelliğini `false`. Zaman **validateAuthority** yanlış olarak yeniden yönlendirmeleri verilir b2clogin.com için ayarlanmış.

Aşağıdaki örnek, özelliğin nasıl ayarlayabilir gösterir:
```javascript
// The current application coordinates were pre-registered in a B2C directory.

const msalConfig = {
    auth:{
        clientId: "Enter_the_Application_Id_here",
        authority: "https://contoso.b2clogin.com/tfp/contoso.onmicrosoft.com/B2C_1_signupsignin1",
        b2cScopes: ["https://contoso.onmicrosoft.com/demoapi/demo.read"],
        webApi: 'https://contosohello.azurewebsites.net/hello',
        validateAuthority: false;

};
// create UserAgentApplication instance
const myMSALObj = new UserAgentApplication(msalConfig);
```

> [!NOTE]
> Azure AD B2C'yi uygulamanız büyük olasılıkla login.microsoftonline.com kullanıcı akışı başvuruları ve belirteç uç noktaları gibi çeşitli yerlerde ifade eder. Kiracı name.b2clogin.com bilgisayarınızı kullanmak için yetkilendirme uç noktası, belirteç uç noktası ve veren güncelleştirildi emin olun.

İzleyin [MSAL JS örnek](https://github.com/Azure-Samples/active-directory-b2c-javascript-msal-singlepageapp#single-page-application-built-on-msaljs-with-azure-ad-b2c) bir erişim belirteci alma ve Azure AD B2C ile güvenliği sağlanan bir API'yi çağırmak için JavaScript (msal.js) için Microsoft kimlik doğrulama kitaplığı önizlemesi kullanma.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Özel ilkeler](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-overview-custom)
- [Kullanıcı arabirimi özelleştirme](https://docs.microsoft.com/azure/active-directory-b2c/customize-ui-overview)