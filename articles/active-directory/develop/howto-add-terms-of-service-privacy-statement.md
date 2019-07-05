---
title: Uygulamalar için hizmet ve gizlilik bildirimini koşulları | Azure
description: Azure AD kullanmak için kayıtlı uygulamalar için hizmet ve gizlilik bildirimini koşullarını nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/22/2019
ms.author: ryanwi
ms.reviwer: lenalepa, sureshja
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: b0a01b50573405964b09339d03e84c62dbdd8582
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482860"
---
# <a name="how-to-configure-terms-of-service-and-privacy-statement-for-an-app"></a>Nasıl yapılır: Hizmet ve gizlilik bildirimini bir uygulama için koşulları yapılandırma

Oluşturun ve Azure Active Directory (Azure AD) ve Microsoft hesapları ile tümleştirilen uygulamalar yöneten geliştiriciler, hizmet ve gizlilik bildirimini uygulamanın koşullarını bağlantılar içermelidir. Koşulları hizmet ve gizlilik bildirimi kullanıcı onayı deneyimi aracılığıyla kullanıcılara çıkarılır. Kullanıcılarınızın uygulamanızı güvenebilir bilmeniz yardımcı olurlar. Hizmet ve gizlilik bildirimini koşullarını, kullanıcıya yönelik çok kiracılı uygulamalar--birden çok dizini tarafından kullanılan veya herhangi bir Microsoft hesabı için kullanılabilir uygulamalar için özellikle önemlidir.

Hizmet Koşulları ve gizlilik bildirimi belgeleri uygulamanız için ve bu belgelere URL'leri sağlamak için oluşturmaktan sorumlu olursunuz. Bu bağlantıları sağlayamamaktadır çok kiracılı uygulamalar için uygulamanız için kullanıcı onayı deneyimi kullanıcıların uygulamanıza onay önerilmemektedir bir uyarı gösterilir.

> [!NOTE]
> * Tek kiracılı uygulamalar bir uyarı gösterilmez.
> * Bir veya iki iki bağlantı eksikse, uygulamanızı bir uyarı gösterilir.

## <a name="user-consent-experience"></a>Kullanıcı onayı deneyimi

Aşağıdaki örnekler, hizmet ve gizlilik bildirimini koşulları yapılandırıldığında deneyimi onay ve ne zaman bu bağlantıları yapılandırılmamış kullanıcı gösterir.

![Gizlilik bildirimi ve sağlanan hizmet koşullarını olmadan ve ekran görüntüleri](./media/howto-add-terms-of-service-privacy-statement/user-consent-exp-privacy-statement-terms-service.png)

## <a name="formatting-links-to-the-terms-of-service-and-privacy-statement-documents"></a>Hizmet ve gizlilik bildirimi belgeleri koşullarını bağlantılar biçimlendirme

Uygulamanızın koşullarını hizmet ve gizlilik bildirimi belgelerin bağlantıları eklemeden önce URL'leri aşağıdaki yönergeleri izleyin emin olun.

| Yönerge     | Açıklama                           |
|---------------|---------------------------------------|
| Biçimi        | Geçerli bir URL                             |
| Geçerli şemalar | HTTP ve HTTPS<br/>HTTPS öneririz. |
| En büyük uzunluk    | 2048 karakter                       |

Örnekler: `https://myapp.com/terms-of-service` ve `https://myapp.com/privacy-statement`

## <a name="adding-links-to-the-terms-of-service-and-privacy-statement"></a>Hizmet ve gizlilik bildirimini koşullarını bağlantılar ekleme

Hizmet ve gizlilik bildirimini koşullarını hazır olduğunuzda, aşağıdaki yöntemlerden birini kullanarak uygulamanızda bu belgelere bağlantılar ekleyebilirsiniz:

* [Azure portalı üzerinden](#azure-portal)
* [Uygulama nesnesi JSON kullanma](#app-object-json)
* [MSGraph beta REST API kullanma](#msgraph-beta-rest-api)

### <a name="azure-portal"></a>Azure portalını kullanarak
Azure portalında aşağıdaki adımları izleyin.

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Gidin **uygulama kayıtları** bölümünde ve uygulamanızı seçin.
3. Açık **markalama** bölmesi.
4. Doldurun **hizmet koşulları URL'si** ve **gizlilik bildirimi URL'si** alanları.
5. Yaptığınız değişiklikleri kaydedin.

    ![Uygulama özellikleri içeren hizmet ve gizlilik bildirimi URL'leri koşulları](./media/howto-add-terms-of-service-privacy-statement/azure-portal-terms-service-privacy-statement-urls.png)

### <a name="app-object-json"></a>Uygulama nesnesi JSON kullanma

Uygulama nesnesi JSON doğrudan değiştirmek isterseniz, hizmet ve gizlilik bildirimini uygulamanızın koşullarını bağlantılar dahil etmek için Azure portalında veya uygulama kayıt portalı bildirim düzenleyicisini kullanabilirsiniz.

```json
    "informationalUrls": { 
        "termsOfService": "<your_terms_of_service_url>", 
        "privacy": "<your_privacy_statement_url>" 
    }
```

### <a name="msgraph-beta-rest-api"></a>MSGraph beta REST API kullanma

Program aracılığıyla tüm uygulamaları güncelleştirmek için REST API MSGraph beta koşullarını hizmet ve gizlilik bildirimi belgelerin bağlantıları eklemek için tüm uygulamaları güncelleştirmek için kullanabilirsiniz.

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
> * Bu alanlardan birini atamış tüm önceden var olan değerlerin üzerine değil dikkatli olun: `supportUrl`, `marketingUrl`, ve `logoUrl`
> * Bir Azure AD hesabıyla oturum açtığınızda MSGraph beta REST API yalnızca çalışır. Kişisel Microsoft hesapları desteklenmez.
