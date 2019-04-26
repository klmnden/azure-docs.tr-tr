---
title: SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişimi ayarlama | Microsoft Docs
description: SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişim ayarlama konusunda bilgi edinin.
services: active-directory
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: daveba
editor: ''
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.subservice: conditional-access
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/27/2019
ms.author: joflore
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a90cd381dbe3feaad110c7f10ae328915c051d0a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60411952"
---
# <a name="how-to-set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>Nasıl Yapılır: SharePoint Online ve Exchange Online için Azure Active Directory koşullu erişim ayarlama 

İle [Azure Active Directory (Azure AD) koşullu erişim](overview.md), kullanıcıların bulut uygulamalarınıza erişme denetleyebilirsiniz. SharePoint ve Exchange online erişimi denetlemek için koşullu erişim kullanmak istiyorsanız, yapmanız:

- Koşullu erişim senaryonuz desteklenip desteklenmediğini gözden geçirin
- İstemci uygulamaları koşullu erişim ilkelerinizi zorlama atlaması engelleyin.   

Bu makalede her iki durumda nasıl ele açıklanmaktadır.


## <a name="what-you-need-to-know"></a>Bilmeniz gerekenler

Azure AD koşullu erişim, kimlik doğrulama girişimi geldiğinde, bulut uygulamaları korumak için kullanabilirsiniz:

- Bir web tarayıcısı

- Kullanan bir istemci uygulaması [modern kimlik doğrulaması](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)

- Exchange ActiveSync 

Bazı bulut uygulamaları, eski bir kimlik doğrulama protokolleri de destekler. Bu, örneğin, SharePoint Online ve Exchange Online için geçerlidir. Azure AD, bir istemci uygulaması, eski bir kimlik doğrulama protokolü bir bulut uygulaması erişmek için kullanabileceğiniz, bu erişim girişimi bir koşullu erişim ilkesi zorunlu kılamaz. İlke zorlama atlamasını bir istemci uygulaması önlemek için yalnızca etkilenen bulut uygulamaları modern kimlik doğrulamasını etkinleştirmek mümkün olup olmadığını denetlemelisiniz. 

İstemci uygulamaları koşullu erişim uygulanmaz verilebilir:

- Office 2010 ve önceki sürümleri

- Office 2013'ün modern kimlik doğrulaması etkin değilse



 
## <a name="control-access-to-sharepoint-online"></a>SharePoint Online'a erişimi denetleme

Modern kimlik doğrulamanın yanı sıra, SharePoint Online de eski bir kimlik doğrulama protokollerini destekler. Eski bir kimlik doğrulama protokolleri etkinleştirilirse, SharePoint için koşullu erişim ilkelerinizi, modern kimlik doğrulaması kullanmayan istemciler için zorunlu değildir.

Kullanarak SharePoint erişimi için eski kimlik doğrulama protokollerini devre dışı bırakabilirsiniz **[Set-SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)** cmdlet: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Exchange Online'a erişimi denetleme

Exchange Online için koşullu erişim ilkeleri ayarlama, aşağıdakileri gözden geçirin yapmanız gerekir:

- Exchange ActiveSync

- Eski bir kimlik doğrulama protokolleri



### <a name="exchange-activesync"></a>Exchange ActiveSync

Modern kimlik doğrulaması Exchange Active Sync desteklese de, koşullu erişim senaryoları için destek ile ilgili bazı sınırlamalar vardır:

- Seçerken **Exchange Active Sync istemcilerinin** ilkenizde, diğer koşulları yapılandıramazsınız.  

    ![Cihaz platformları](./media/conditional-access-for-exo-and-spo/05.png)

- Çok faktörlü kimlik doğrulama gereksinimini ayarlama desteklenmiyor  

    ![Koşullu erişim](./media/conditional-access-for-exo-and-spo/01.png)

Etkili bir şekilde erişim Exchange Online için Exchange ActiveSync'ten korumak için şunları yapabilirsiniz:

- Aşağıdaki adımları izleyerek bir desteklenen koşullu erişim ilkesi yapılandırın:

    a. Seçmeniz yeterlidir **Office 365 Exchange Online** bulut uygulaması olarak.  

    ![Koşullu erişim](./media/conditional-access-for-exo-and-spo/04.png)

    b. Seçin **Exchange Active Sync** olarak **istemci uygulaması**.  

    ![Cihaz platformları](./media/conditional-access-for-exo-and-spo/03.png)

- Active Directory Federasyon Hizmetleri (AD FS) kurallarını kullanarak Exchange ActiveSync engelleyin.

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Eski bir kimlik doğrulama protokolleri

Modern kimlik doğrulamanın yanı sıra, Exchange Online de eski bir kimlik doğrulama protokollerini destekler. Eski bir kimlik doğrulama protokolleri etkinleştirilirse, Exchange Online için koşullu erişim ilkelerinizi, modern kimlik doğrulaması kullanmayan istemciler için zorunlu değildir.

Eski bir kimlik doğrulama protokolleri Exchange Online için AD FS kurallar ayarlayarak devre dışı bırakabilirsiniz. Bu üzerinden erişimi engeller:

- Office 2013 modern kimlik doğrulaması etkin olmayan gibi eski Office istemcileri 

- Office'in önceki sürümleri


## <a name="set-up-ad-fs-rules"></a>AD FS kurallarını ayarlayın

Etkinleştirmek veya AD FS düzeyinde trafiği engellemek için aşağıdaki verme yetkilendirme kuralları kullanabilirsiniz. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Extranetten gelen eski trafiği engelle

Aşağıdaki üç kuralları uygulayarak: 

- İçin erişim sağlar:
    - Exchange ActiveSync trafiği
    - Tarayıcı trafiği
    - Modern kimlik doğrulama trafiği
- Sizin için erişimini engelleme: 
    - Extranetten gelen eski istemci uygulamaları

**Kural 1:**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**2. kural:**

    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**3. kural:**

    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

### <a name="block-legacy-traffic-from-anywhere"></a>Her yerden eski trafiği engelle

Aşağıdaki üç kuralları uygulayarak:

- İçin erişim sağlar: 
    - Exchange ActiveSync trafiği
    - Tarayıcı trafiği
    - Modern kimlik doğrulama trafiği
- Sizin için erişimini engelleme: 
    - Herhangi bir konumdan eski uygulamaları

##### <a name="rule-1"></a>Kural 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Kural 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Kural 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için [Azure Active Directory'de koşullu erişim nedir](overview.md).

Talep kurallarına yapılandırma hakkında yönergeler için bkz: [talep kurallarını yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-claim-rules). 






