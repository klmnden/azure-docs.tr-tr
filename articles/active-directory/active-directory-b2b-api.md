---
title: "Azure Active Directory B2B işbirliği API ve özelleştirme | Microsoft Docs"
description: "Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler."
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 04/11/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: b40dc42c1dfc8910f9be9242fee3beeade92d193
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-b2b-collaboration-api-and-customization"></a>Azure Active Directory B2B işbirliği API ve özelleştirme

Sizi davet işlemi kuruluşları için en iyi şekilde özelleştirmek istedikleri bize birçok müşteri karşılaşmışsınız. Bizim API'si ile tam olarak bunu yapabilirsiniz. [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="capabilities-of-the-invitation-api"></a>API davet özellikleri
API aşağıdaki özellikleri sunar:

1. Bir dış kullanıcı ile davet *herhangi* e-posta adresi.

    ```
    "invitedUserDisplayName": "Sam"
    "invitedUserEmailAddress": "gsamoogle@gmail.com"
    ```

2. Bunlar, daveti kabul ettikten sonra güden kullanıcılarınıza istediğiniz özelleştirin.

    ```
    "inviteRedirectUrl": "https://myapps.microsoft.com/"
    ```

3. Bize aracılığıyla standart davet posta göndermeyi seçin

    ```
    "sendInvitationMessage": true
    ```

  içeren bir ileti özelleştirebileceğiniz alıcısına

    ```
    "customizedMessageBody": "Hello Sam, let's collaborate!"
    ```

4. Ve cc seçin: Bu ortak çalışanı davet hakkında Döngüdeki tutmak istediğiniz kişilerin.

5. Veya Azure AD aracılığıyla bildirimleri göndermek seçerek daveti ve hazırlama iş akışı tamamen özelleştirebilirsiniz.

    ```
    "sendInvitationMessage": false
    ```

  Bu durumda, bir kullanım URL bir e-posta şablonu, anlık ileti veya tercih ettiğiniz başka bir dağıtım yöntem katıştırma API'sinden ulaşırsınız.

6. Son olarak, bir yönetici, üye olarak kullanıcı davet seçebilirsiniz.

    ```
    "invitedUserType": "Member"
    ```


## <a name="authorization-model"></a>Yetkilendirme modeli
API şu yetkilendirme modlarında çalıştırabilirsiniz:

### <a name="app--user-mode"></a>Uygulama + kullanıcı modu
Bu modda, aktaranın B2B davetleri olması oluşturma izinlerine sahip olmasını API gereksinimlerini kullanıyor.

### <a name="app-only-mode"></a>Uygulama yalnızca modu
Uygulama yalnızca bağlamda uygulamanın başarılı olması davet User.Invite.All kapsamın gerekir.

Daha fazla bilgi için bkz: https://graph.microsoft.io/docs/authorization/permission_scopes


## <a name="powershell"></a>PowerShell
Şimdi, ekleme ve bir kuruluş için dış kullanıcılar kolayca davet etmek için PowerShell kullanmak da mümkündür. Cmdlet'ini kullanarak bir davet oluşturun:

```
New-AzureADMSInvitation
```

Aşağıdaki seçenekleri kullanabilirsiniz:

* -InvitedUserDisplayName
* -InvitedUserEmailAddress
* -SendInvitationMessage
* -InvitedUserMessageInfo

Ayrıca daveti API Başvurusu'nda uğradı kontrol edebilirsiniz [https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation)

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure AD B2B işbirliği lisanslama](active-directory-b2b-licensing.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
