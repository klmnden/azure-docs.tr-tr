---
title: Azure AD Avrupa müşteriler için kimlik verilerini depoladığı | Microsoft Docs
description: Microsoft Azure Active Directory kimlik ile ilgili veriler için Avrupa, müşterilerinin depoladığı hakkında bilgi edinin.
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
ms.author: lizross
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2018
ms.custom: it-pro
ms.openlocfilehash: 19dc163dbb6dd296a417f5c313a36c7f7c9e50d7
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="where-does-microsoft-azure-active-directory-azure-ad-store-identity-data-for-european-customers"></a>Burada Microsoft Azure Active Directory (Azure AD) kimlik verilerini Avrupa müşteriler için depoluyor mu
Azure AD, kullanıcı kimliklerini yönetmek ve kuruluşunuzun kaynakları güvenli hale Intelligence güdümlü erişim ilkeleri oluşturmak için yardımcı olur. Kimlik verilerini hizmete abone olduğunuzda sağlanan kuruluşunuzun adresine dayalı bir konumda depolanır. Örneğin, ne zaman, Office 365 veya Azure abone. Kimlik verilerinin depolandığı hakkında daha ayrıntılı bilgi için kullandığınız [bulunan, verilerinizin nerede olduğuna?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) Microsoft Trust Center bölümü.

Çoğu Azure AD ile ilgili Avrupa kimlik verilerini Avrupa veri merkezlerinde kalarak, genellikle ABD veri merkezlerine depolanır, kullanıcıyla ilgili beş özniteliği vardır. Bu, GivenName, Soyadı, userPrincipalName, etki alanı ve PasswordHash öznitelikleridir. PasswordHash özniteliği özel bir durum olabilir ve birisi şirket içi, Azure AD ile eşitlemeyi PasswordHash değeri durdurur federe kimlik doğrulama yöntemi kullanıyorsa, ABD'de depolanmaz. Ayrıca, normal için gerekli olan bazı işlem, hizmete özgü veriler var. ABD'de depolanır ve herhangi bir kişisel veri içermeyen Azure AD işlemi.

## <a name="data-stored-outside-of-european-datacenters-for-european-customers"></a>Avrupa müşteriler için Avrupa veri merkezleri dışında depolanan veriler

Avrupa tabanlı adresleriyle kuruluşlarda çoğu Azure AD ile ilgili Avrupa kimlik verilerini Avrupa veri merkezlerinde kalır. Avrupa veri merkezlerinde, depolanmaz azure AD veri içerir:

- **Kimlikle ilgili öznitelikleri**

    Amerika Birleşik Devletleri kimlikle ilgili aşağıdaki öznitelikler çoğaltılır:

    - givenName
    - Soyadı
    - userPrincipalName
    - Etki alanı
    - PasswordHash
    - sourceAnchor
    - accountEnabled
    - passwordPolicies
    - StrongAuthenticationRequirement
    - ApplicationPassword
    - PUID

- **Microsoft Azure multi-Factor authentication (MFA) ve Azure AD Self Servis parola sıfırlama (SSPR)**
    
    Tüm kullanıcı verilerini Avrupa veri merkezlerinde çalışmıyorken MFA depolar. Ancak, bazı MFA hizmete özgü ABD'de veriler de dahil olmak üzere:
    
    - MFA veya SSPR kullanıyorsanız, iki öğeli kimlik doğrulama ve ilgili kişisel verilerini ABD'de depolanabilir.
        - Tüm iki öğeli kimlik doğrulama telefon çağrısı veya SMS kullanarak ABD hizmet sağlayıcılar tarafından tamamlanmış.
        - Microsoft Authenticator uygulaması kullanarak anında iletme bildirimleri bildirimleri Avrupa olabilecek üreticinin bildirim Hizmeti'nden (Apple veya Google) gerektirir.
        - OATH kodları ABD'de her zaman doğrulanır 
    - Bazı MFA ve SSPR günlükleri, kimlik doğrulama türü ne olursa olsun 30 gündür ABD'de depolanır.

- **Microsoft Azure Active Directory B2C (Azure AD B2C)**

    Tüm kullanıcı verilerini Avrupa veri merkezlerinde çalışmıyorken azure AD B2C depolar. Ancak, işlem günlükleri (ile kişisel veriler kaldırıldı) kişi Hizmetleri burada erişimini konumda kalır. Örneğin, bir B2C kullanıcı ABD hizmetinde erişirse, işlem günlükleri ABD'de kalır Ayrıca, kişisel verileri içermeyen tüm ilke yapılandırması verilerin depolandığı yalnızca ABD'de İlke yapılandırmaları hakkında daha fazla bilgi için bkz: [Azure Active Directory B2C: yerleşik ilkeleri](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) makalesi.

- **Microsoft Azure Active Directory B2B (Azure AD B2B)** 
    
    Tüm kullanıcı verilerini Avrupa veri merkezlerinde çalışmıyorken azure AD B2B depolar. Ancak, B2B ABD veri merkezleri içinde tablolardaki kendi kişisel olmayan meta verileri depolar. Bu tablo redeemUrl, invitationTicket, kaynak Kiracı kimliği, InviteRedirectUrl ve InviterAppId gibi alanları içerir.

- **Microsoft Azure Active Directory etki alanı Hizmetleri (Azure AD DS)**

    Azure AD DS, müşteri tarafından seçilen Azure sanal ağ ile aynı konumda kullanıcı verilerini depolar. Bu nedenle, ağ Avrupa ise, veriler çoğaltılan ve Avrupa depolanır.

- **Hizmetler ve Azure AD ile tümleşik uygulamalar**

    Tüm hizmetler ve Azure AD ile tümleştirmek uygulamaları kimlik verilerini erişimi. Her hizmet ve bu belirli hizmet ve uygulama tarafından kimlik verilerini nasıl işlenir ve şirketinizin veri depolama gereksinimleri karşıladıklarından olup olmadığını belirlemek için uygulama değerlendirin.

    Microsoft Hizmetleri veri residency hakkında daha fazla bilgi için bkz: [bulunan, verilerinizin nerede olduğuna?](https://www.microsoft.com/en-us/trustcenter/privacy/where-your-data-is-located) Microsoft Trust Center bölümü.

## <a name="next-steps"></a>Sonraki adımlar
Tüm özellikleri ve işlevleri yukarıda açıklanan hakkında daha fazla bilgi için bu makalelerine bakın.
- [Azure Active Directory ile çalışmaya başlama](get-started-azure-ad.md)
- [Çok faktörlü kimlik doğrulaması nedir?](https://docs.microsoft.com/en-us/azure/active-directory/authentication/multi-factor-authentication)
- [Azure AD Self Servis parola sıfırlama](https://docs.microsoft.com/en-us/azure/active-directory/authentication/active-directory-passwords-overview)
- [Azure Active Directory B2C nedir?](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview)
- [Azure AD B2B işbirliği nedir?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
- [Azure Active Directory (AD) etki alanı Hizmetleri](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-overview)
