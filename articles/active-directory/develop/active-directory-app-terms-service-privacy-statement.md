---
title: Azure AD uygulamalarınız için hizmet ve gizlilik bildirimini koşulları | Microsoft Docs
description: Azure AD kullanmak için kayıtlı uygulamalar için hizmet ve gizlilik bildirimini koşullarını nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/23/2018
ms.author: celested
ms.reviwer: lenalepa; sureshja
ms.custom: aaddev
ms.openlocfilehash: 8abd9c35c7d979e1e1a42dd7e178d802f06c4227
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655334"
---
# <a name="terms-of-service-and-privacy-statement-for-registered-azure-active-directory-apps"></a>Kayıtlı Azure Active Directory uygulamaları için hizmet ve gizlilik bildirimini koşulları

Derleme ve Azure Active Directory (Azure AD) ve Microsoft hesapları ile tümleştirme uygulamalarını yönetme geliştiriciler hizmet ve gizlilik bildirimini uygulamanın koşullarını bağlantılarını içermelidir. Hizmet ve gizlilik bildirimini koşullarını kullanıcı onayı deneyimi aracılığıyla kullanıcılara çıkmış. Uygulamanızı güvenebileceği biliyorsanız, kullanıcılarınızın yardımcı olurlar. Hizmet ve gizlilik bildirimini koşullarını kullanıcı dönük çok kiracılı uygulamalara--birden çok dizin tarafından kullanılan veya herhangi bir Microsoft hesabı kullanılabilir için özellikle önemlidir.

Uygulamanız için ve bu belgeleri URL'lere sağlamak için deyimi belgeleri hizmet ve gizlilik koşullarını oluşturmaktan sorumlu. Bu bağlantıları sağlamak için başarısız çok kiracılı uygulamalar için uygulamanız için kullanıcı onayı deneyimi uygulamanıza onaylıyorsunuz kullanıcıların önerilmemektedir bir uyarı gösterilir.

> [!NOTE]
> * Tek Kiracı uygulamalar, bir uyarı göstermez.
> * Bir veya iki iki bağlantılar eksikse, uygulamanızı bir uyarı gösterilir.

## <a name="user-consent-experience"></a>Kullanıcı onayı deneyimi

Aşağıdaki örnekler, hizmet ve gizlilik bildirimini koşullarını yapılandırıldığında deneyimi onay ve ne zaman bu bağlantıları yapılandırılmamış kullanıcı gösterir.

![Ekran görüntüleri ile ve gizlilik bildirimini ve sağlanan hizmet koşulları olmadan](./media/active-directory-integrating-applications/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Hizmet ve gizlilik bildirimini belgeleri koşullarını bağlantılar biçimlendirme

Hizmet ve gizlilik bildirimini belgeleri, uygulamanızın koşullarını bağlantılar eklemeden önce URL'leri bu yönergelere emin olun.

| Kılavuz     | Açıklama                           |
|---------------|---------------------------------------|
| Biçimlendir        | Geçerli bir URL                             |
| Geçerli şemalar | HTTP ve HTTPS</br>HTTPS öneririz |
| En fazla uzunluk    | 2048 karakter                       |

Örnekler: `https://myapp.com/terms-of-service` ve `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Hizmet ve gizlilik bildirimini koşullarını bağlantılar ekleme

Hizmet ve gizlilik bildirimini koşullarını hazır olduğunuzda, aşağıdaki yöntemlerden birini kullanarak uygulamanızda bu belgeleri bağlantılar ekleyebilirsiniz:
* [Azure portalı üzerinden](#registered-in-azure-portal)
* [Uygulama kayıt portalı veya Geliştirme Merkezi](#registered-in-app-reg-portal)
* [Uygulama nesnesi JSON kullanma](#app-object-json)
* [MSGraph beta REST API kullanma](#msgraph-beta-rest-api)

### <a name="registered-in-azure-portal"></a>Uygulamanızı Azure Portalı'nda kayıtlı
Uygulamanızı Azure portalında kaydettiyseniz aşağıdaki adımları izleyin.

1. Oturum [Azure portal](https://portal.azure.com/).
2. Gidin **uygulama kayıtlar** bölümünde ve uygulamanızı seçin.
3. Açık **özellikleri** uygulama bölümü.
4. Doldurmak **hizmet koşulları URL'si** ve **gizlilik bildirimi URL'si** alanları.
5. Yaptığınız değişiklikleri kaydedin.

![Hizmet ve gizlilik bildirimini URL'leri koşullarla uygulama özellikler bölümü](./media/active-directory-integrating-applications/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="registered-in-app-reg-portal"></a>Uygulamanızın uygulama kayıt Portalı'nda kayıtlı
Uygulama kayıt portalı ya da Geliştirme Merkezi uygulamanızı kaydettiyseniz aşağıdaki adımları izleyin.

1. Oturum [uygulama kayıt portalı](https://apps.dev.microsoft.com/).
2. Uygulamanızı seçin ve kaydırma **profil** bölümü.
3. Doldurmak **hizmet koşulları URL'si** ve **gizlilik bildirimi URL'si** alanları.
4. Yaptığınız değişiklikleri kaydedin.

![Uygulama profili bölümünü koşullarla hizmet ve gizlilik bildirimini URL'leri](./media/active-directory-integrating-applications/app-registration-portal-profile-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>Uygulama nesnesi JSON kullanma
Uygulama nesnesini JSON doğrudan değiştirmek isterseniz, hizmet ve gizlilik bildirimi, uygulamanızın koşullarını bağlantılar dahil etmek için bildirim düzenleyicisinde Azure portal ya da uygulama kayıt portalı kullanabilirsiniz.

```json
    "informationalUrls": { 
        "termsOfService": “<your_terms_of_service_url>”, 
        "privacy": “<your_privacy_statement_url>” 
    }
```

### <a name="msgraph-beta-rest-api"></a>MSGraph beta REST API kullanma
Program aracılığıyla tüm uygulamaları güncelleştirmek için hizmet ve gizlilik bildirimini belgeleri koşullarını bağlantılar dahil etmek için tüm uygulamaları güncelleştirmek için REST API MSGraph beta kullanabilirsiniz.

```
PATCH https://graph.microsoft.com/beta/applications/{application id}
{ 
    "appId": "{your application id}", 
    "info": { 
        "termsOfServiceUrl": "<your_terms_of_service_url>", 
        "supportUrl": null, 
        "privacyStatementUrl": "<your_privacy_statement_url>", 
        "marketingUrl": null, 
        "logoUrl": null 
    }
}
```

> [!NOTE]
> * Bu alanların hiçbirini atamış herhangi önceden var olan değerlerin üzerine değildir dikkatli olun: `supportUrl`, `marketingUrl`, ve `logoUrl`
> * Bir Azure AD hesabı ile oturum açtığınızda MSGraph beta REST API yalnızca çalışır. Kişisel Microsoft hesapları desteklenmez.