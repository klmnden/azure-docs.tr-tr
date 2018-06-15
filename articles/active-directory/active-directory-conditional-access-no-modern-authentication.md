---
title: SharePoint Online ve Exchange Online için koşullu erişim Azure Active Directory ayarlama | Microsoft Docs
description: SharePoint Online ve Exchange Online için koşullu erişim Azure Active Directory ayarlama öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 62349fba-3cc0-4ab5-babe-372b3389eff6
ms.service: active-directory
ms.component: protection
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 398b69769cebb9d54121e357faed06294499bd61
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34723788"
---
# <a name="set-up-sharepoint-online-and-exchange-online-for-azure-active-directory-conditional-access"></a>SharePoint Online ve Exchange Online için koşullu erişim Azure Active Directory ayarlama 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), kullanıcıların, bulut uygulamalarınızı erişme kontrol edebilirsiniz. SharePoint ve Exchange online erişimini denetlemek için koşullu erişim kullanmak istiyorsanız, için gerekir:

- Koşullu erişim senaryonuz desteklenip desteklenmediğini gözden geçirin
- İstemci uygulamaları koşullu erişim ilkelerinizi zorlama atlamalarını engeller.   

Bu makalede her iki durumda nasıl ele açıklanmaktadır.


## <a name="what-you-need-to-know"></a>Bilmeniz gerekenler

Kimlik doğrulama girişimi geldiğinde bulut uygulamaları korumak için Azure AD koşullu erişimi kullanın:

- Bir web tarayıcısı

- Kullanan bir istemci uygulaması [modern kimlik doğrulaması](https://support.office.com/article/Using-Office-365-modern-authentication-with-Office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)

- Exchange ActiveSync 

Bazı bulut uygulamaları, aynı zamanda eski kimlik doğrulama protokollerini destekler. Bu, örneğin, SharePoint Online ve Exchange Online için geçerlidir. Azure AD, bir istemci uygulaması eski kimlik doğrulama protokolü bir bulut uygulama erişmek için kullanabileceğiniz, bu erişim denemede koşullu erişim ilkesi zorunlu kılamaz. Bir istemci uygulaması ilkeleri zorlama atlamasını önlemek için yalnızca etkilenen bulut uygulamalarını modern kimlik doğrulamasını etkinleştirmek mümkün olup olmadığını denetlemelisiniz. 

İstemci uygulamaları koşullu erişim uygulanmaz örnekler şunlardır:

- Office 2010 ve önceki sürümleri

- Office 2013 modern kimlik doğrulaması etkin



 
## <a name="control-access-to-sharepoint-online"></a>SharePoint Online'a erişimi denetleme

Modern kimlik doğrulaması ek olarak, SharePoint Online de eski kimlik doğrulama protokollerini destekler. Eski kimlik doğrulama protokollerini etkinleştirilirse, SharePoint için koşullu erişim ilkelerini, modern kimlik doğrulaması kullanmayan istemciler için zorunlu değildir.

Kullanarak SharePoint erişimi için eski kimlik doğrulama protokollerini devre dışı bırakabilirsiniz **[kümesi SPOTenant](https://technet.microsoft.com/library/fp161390.aspx)** cmdlet: 

    Set-SPOTenant -LegacyAuthProtocolsEnabled $false

## <a name="control-access-to-exchange-online"></a>Exchange Online'a erişimi denetleme

Exchange Online için koşullu erişim ilkeleri ayarla, aşağıdakileri gözden geçirin gerekir:

- Exchange ActiveSync

- Eski kimlik doğrulama protokolleri



### <a name="exchange-activesync"></a>Exchange ActiveSync

Exchange Active Sync modern kimlik doğrulaması desteklerken, koşullu erişim senaryoları için destek ile ilgili bazı sınırlamalar vardır:

- Cihaz platformları koşulu yalnızca yapılandırabilirsiniz  

    ![Cihaz platformları](./media/active-directory-conditional-access-no-modern-authentication/05.png)

- Çok faktörlü kimlik doğrulama gereksinimini ayarlama desteklenmiyor  

    ![Koşullu erişim](./media/active-directory-conditional-access-no-modern-authentication/01.png)

Etkili bir şekilde erişim Exchange Online için Exchange ActiveSync'ten korumak için şunları yapabilirsiniz:

- Aşağıdaki adımları izleyerek desteklenen koşullu erişim ilkesi yapılandırın:

    a. Yalnızca seçin **Office 365 Exchange Online** bulut uygulaması olarak.  

    ![Koşullu erişim](./media/active-directory-conditional-access-no-modern-authentication/04.png)

    b. Seçin **Exchange Active Sync** olarak **istemci uygulaması**ve ardından **yalnızca desteklenen platformları için ilke geçerli**.  

    ![Cihaz platformları](./media/active-directory-conditional-access-no-modern-authentication/03.png)

- Active Directory Federasyon Hizmetleri (AD FS) kurallarını kullanarak Exchange ActiveSync engelleyin.

        @RuleName = "Block Exchange ActiveSync"
        c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
        => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "false");




### <a name="legacy-authentication-protocols"></a>Eski kimlik doğrulama protokolleri

Modern kimlik doğrulaması ek olarak, Exchange Online de eski kimlik doğrulama protokollerini destekler. Eski kimlik doğrulama protokolleri etkinleştirilirse, Exchange Online için koşullu erişim ilkelerini, modern kimlik doğrulaması kullanmayan istemciler için zorunlu değildir.

Eski kimlik doğrulama protokolleri Exchange Online için AD FS ayarlayarak devre dışı bırakabilirsiniz. Bu blokları erişimden:

- Office 2013 modern kimlik doğrulaması etkin yok gibi eski Office istemcileri 

- Office'in önceki sürümleri


## <a name="set-up-ad-fs-rules"></a>AD FS kurallar oluşturun

Etkinleştirmek veya AD FS düzeyindeki trafiği engellemek için aşağıdaki verme yetkilendirme kurallarını kullanabilirsiniz. 

### <a name="block-legacy-traffic-from-the-extranet"></a>Extranet üzerinden eski trafiği engelle

Aşağıdaki üç kuralları uygulayarak: 

- Erişim için etkinleştir:
    - Exchange ActiveSync trafiği
    - Tarayıcı trafiğini
    - Modern kimlik doğrulama trafiğini
- Erişimi engeller: 
    - Extranet üzerinden eski istemci uygulamaları

**Kural 1:**

    @RuleName = "Allow all intranet traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

**Kural 2:**

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

- Erişim için etkinleştir: 
    - Exchange ActiveSync trafiği
    - Tarayıcı trafiğini
    - Modern kimlik doğrulama trafiğini
- Erişimi engeller: 
    - Herhangi bir yerden eski uygulamalar

##### <a name="rule-1"></a>Kural 1
    @RuleName = "Allow all intranet traffic only for browser and modern authentication clients"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "true"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-2"></a>Kural 2
    @RuleName = "Allow Exchange ActiveSync"
    c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value == "Microsoft.Exchange.ActiveSync"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");


##### <a name="rule-3"></a>Kuralı 3
    @RuleName = "Allow extranet browser and browser dialog traffic"
    c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] &&
    c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value =~ "(/adfs/ls)|(/adfs/oauth2)"]
    => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz: [koşullu erişim Azure Active Directory'de](active-directory-conditional-access-azure-portal.md).

Talep kuralları yapılandırma hakkında yönergeler için bkz: [talep kurallarını yapılandırma](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-claim-rules). 






