---
title: Azure Active Directory B2B işbirliği API ve özelleştirme | Microsoft Docs
description: Azure Active Directory B2B işbirliği, iş ortaklarının kurumsal uygulamalarınıza seçmeli olarak erişmelerini mümkün kılarak şirketler arası ilişkilerinizi destekler.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/11/2017
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: e53bac5bca199fe08d53f232416a1c6432148f34
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
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

- [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
- [B2B işbirliği davet e-posta öğeleri](invitation-email-elements.md)
- [B2B işbirliği davet kullanım](redemption-experience.md)
- [B2B işbirliği kullanıcıları davet olmadan ekleme](add-user-without-invite.md)

