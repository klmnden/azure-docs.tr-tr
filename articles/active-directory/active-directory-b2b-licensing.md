---
title: "Azure Active Directory B2B işbirliği Kılavuzu lisanslama | Microsoft Docs"
description: "Azure AD lisansları işbirliği gerektirmez Azure Active Directory B2B Ücretli ancak, aynı zamanda özellikleri B2B Konuk kullanıcılar için ücretli"
services: active-directory
documentationcenter: 
author: sasubram
manager: mtillman
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: sasubram
ms.custom: it-pro
ms.openlocfilehash: 664398eb71501ff450b785928992729f91740a19
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B işbirliği lisanslama kılavuzu

Azure AD B2B işbirliği özelliklerinden Konuk kullanıcılar bunları Azure AD hizmetlerinde ve kuruluşunuzdaki diğer kaynaklara erişmesine izin vermek için Azure AD kiracınıza içine davet etmek için kullanabilirsiniz. Azure AD özelliklerini Ücretli erişim sağlamak istiyorsanız, B2B işbirliği Konuk kullanıcılar ile lisansına sahip olması gerekir Azure AD lisansları uygun. 

Bu avantajlar şunlardır:
* Azure AD boş yetenekleri ek lisans olmadan Konuk kullanıcılar için kullanılabilir.
* B2B kullanıcılara Azure AD özelliklerini Ücretli erişim sağlamak istiyorsanız, bu B2B Konuk kullanıcılar desteklemek için yeterince lisansa sahip olmalıdır.
* Lisans Ücretli bir Azure AD ile bir davet Kiracı kullanım haklarına Kiracı için davet bir ek beş B2B konuk kullanıcılara B2B işbirliği sahiptir.
* Davet Kiracı sahibi olan müşterinin Azure AD özellikleri kullanıcıların gereksinim kaç B2B işbirliği Ücretli belirlemek için bir olmalıdır. Ücretli Azure AD bağlı olarak yeterince Azure AD olmalıdır, Konuk kullanıcılar için istediğiniz özellikleri aynı 5:1 oranında B2B işbirliği kullanıcıları kapsayacak şekilde lisansları Ücretli.

B2B işbirliği Konuk kullanıcı, iş ortağı şirkete veya değil, bir çalışan, kuruluşunuzun farklı bir iş, conglomerate içinde çalışan bir kullanıcı olarak eklenir. B2B Konuk kullanıcı dış kimlik bilgileri veya kuruluşunuz tarafından bu makalede açıklanan ait kimlik bilgilerinizle oturum açabilirsiniz. 

Diğer bir deyişle, B2B lisans kullanıcı kimliğini nasıl doğruladığını tarafından değil ancak kuruluşunuz için kullanıcı ilişkisi yerine tarafından ayarlanır. Bu kullanıcılar iş ortakları emin değilseniz, farklı olarak, Lisans Koşulları'kabul edilir. B2B işbirliği kullanıcının kendi UserType "Konuk" olarak işaretlenmiş olsa bile amacıyla lisans için olması düşünülür değil Bunlar normalde, bir lisansı kullanıcı başına lisansına sahip olması. Bu kullanıcılar şunları içerir:
* Çalışanlarınızın
* Dış kimlikler kullanarak oturum açmayı personel
* Bir çalışan, conglomerate içinde farklı bir iş


## <a name="licensing-examples"></a>Lisans örnekleri
- Bir müşteri, kendi Azure AD kiracısı 100 B2B işbirliği kullanıcılara davet istiyor. Müşteri erişim yönetimi ve tüm kullanıcılar için sağlama atar ancak 50 kullanıcılar için de MFA ve koşullu erişim gerekir. Bu müşteri 10 Azure AD temel lisansları ve bu B2B kullanıcılar doğru karşılamak üzere 10 Azure AD Premium P1 lisansları satın alması gerekir. Müşteri B2B kullanıcılarla kimlik koruması özellikleri kullanmak planlıyorsa, aynı 5:1 oranında davet edilen kullanıcılar karşılamak için Azure AD Premium P2 lisansları olması gerekir.
- Bir müşteri, Azure AD Premium P1 olan tüm lisanslı 10 çalışanlar sahiptir. Şimdi tüm multi-Factor authentication (MFA) gerekli 60 B2B kullanıcıları davet etmek istedikleri. 5:1 lisans kural altında müşterinin tüm 60 B2B işbirliği kullanıcılar karşılamak için en az 12 Azure AD Premium P1 lisansları olması gerekir. Zaten 10 Premium P1 lisansı 10 çalışanlar için sahip oldukları için MFA gibi Premium P1 özelliklerle 50 B2B kullanıcıları davet hakkına sahip oldukları. Bu örnekte, daha sonra bunların kalan 10 B2B işbirliği kullanıcıları kapsayacak şekilde 2 ek Premium P1 lisansları satın almalısınız.

> [!NOTE]
> Henüz bu B2B işbirliği kullanıcı hakları etkinleştirmek için doğrudan B2B kullanıcılara lisans atamak için bir yolu yoktur.

Davet Kiracı sahibi olan müşterinin Azure AD özellikleri kullanıcıların gereksinim kaç B2B işbirliği Ücretli belirlemek için bir olmalıdır. Hangi Konuk kullanıcılar için istediğiniz Azure AD özelliklerini Ücretli bağlı olarak, 5:1 oranında B2B işbirliği kullanıcılar karşılamak üzere yeterli Azure AD Ücretli lisansı olması gerekir. 

## <a name="additional-licensing-details"></a>Ek lisans ayrıntıları
- Gerçekte B2B kullanıcı hesaplarına lisansları atamak için gerek yoktur. 5:1 oranında bağlı olarak, lisans otomatik olarak hesaplanan bildirilen ve.
- Hayır Ücretli Azure AD lisans kiracısı'nda varsa Azure AD ücretsiz sürüm sunar hakları davet edilen her kullanıcı alır.
- Kendi kuruluştan işbirliği kullanıcı zaten Ücretli bir Azure AD B2B lisans varsa, bunlar davet Kiracı B2B işbirliği lisanstan kullanamayacaktır.

## <a name="advanced-discussion-what-are-the-licensing-considerations-when-we-add-users-from-a-conglomerate-organization-as-members-using-your-apis"></a>Tartışma Gelişmiş: biz "üye Apı'lerinizi kullanarak" olarak kümeleme kuruluştan kullanıcılar eklediğinizde, lisans dikkat edilecek noktalar nelerdir?
B2B Konuk kullanıcı bir iş ortağı kuruluştan konak organizasyonunuzla çalışmak için davet biridir. Genellikle, herhangi bir durumu B2B bile, kullandığı B2B özellikleri uygun değil. Özellikle iki durumda bakalım:

1. Bir tüketici adresini kullanarak bir çalışan bir ana bilgisayar davet ederse
  * Bu senaryo bizim lisans ilkeleri ile uyumlu değildir ve önerilmez.

2. Bir ana bilgisayar kuruluş, başka bir kümeleme kuruluştan bir kullanıcı eklerse
  1. Bu durumda, B2B API'lerini kullanarak kullanıcı davet, ancak bu durumda geleneksel B2B değil. İdeal olarak, biz (bizim API sağlayan) üye olarak diğer düzenlemeler kullanıcıları davet bu kuruluşların olması gerekir. Bu durumda, lisansları davet kuruluşunda bulunan kaynaklara erişmek için bu üyeler için bunları atanması gerekir.

  2. Bazı kuruluşlar, ilke olarak "Konuk" olarak eklenecek diğer kuruluş ait kullanıcılar eklemek isteyebilirsiniz. Burada iki durum vardır:
      * Kümeleme kuruluş zaten Azure AD kullanıyorsanız ve davet edilen kullanıcılar farklı bir kuruluştaki lisanslanır: Bu belgede daha önce düzenlendiği 1:5 formülü izlemeniz gereken için davet edilen kullanıcılar bu durumda, bekliyoruz yok. 

      * Kümeleme kuruluş Azure AD kullanmayan veya yeterli lisans yok: Bu durumda, bu belgenin önceki bölümlerinde düzenlendiği 1:5 formülü izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği ile ilgili diğer makalelerimize göz atın:

* [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Azure Active Directory yöneticileri B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-admin-add-users.md)
* [Bilgi çalışanları B2B işbirliği kullanıcıların nasıl eklenir?](active-directory-b2b-iw-add-users.md)
* [B2B işbirliği davet e-posta öğeleri](active-directory-b2b-invitation-email.md)
* [B2B işbirliği davet kullanım](active-directory-b2b-redemption-experience.md)
* [Azure Active Directory B2B işbirliği sorunlarını giderme](active-directory-b2b-troubleshooting.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](active-directory-b2b-faq.md)
* [Azure Active Directory B2B işbirliği API ve özelleştirme](active-directory-b2b-api.md)
* [B2B işbirliği kullanıcıları için çok faktörlü kimlik doğrulaması](active-directory-b2b-mfa-instructions.md)
* [B2B işbirliği kullanıcıları davet olmadan ekleme](active-directory-b2b-add-user-without-invite.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
