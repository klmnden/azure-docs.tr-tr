---
title: Azure Active Directory B2B işbirliği lisanslama Kılavuzu | Microsoft Docs
description: Azure AD lisansı işbirliği gerektirmez Azure Active Directory B2B Ücretli, ancak, ayrıca özellikleri B2B Konuk kullanıcılar için ödeme
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: mtillman
ms.reviewer: mal
ms.openlocfilehash: d80794511f334cd6dc5af418e24fc774b7d8728f
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48867519"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B işbirliği lisanslama kılavuzu

Azure Active Directory (Azure AD) ile işletmeden işletmeye (B2B) işbirliği, ücretli, kullanılacak dış kullanıcılar (veya "konuk kullanıcılar") davet edebilirsiniz Azure AD Hizmetleri. Her Ücretli Azure AD lisansı, bir kullanıcıya atamak için dış kullanıcı indirimi altında en fazla beş Konuk kullanıcılar davet edebilirsiniz.

Konuk kullanıcı, kuruluşunuz ve kuruluşunuzun kuruluşları hiçbirini üyesi olmayan bir kullanıcıdır. Konuk kullanıcılar, oturum açmak için kullandıkları kimlik bilgilerini değil ilişkilerini, kuruluşunuz tarafından tanımlanır. Aslında, Konuk kullanıcı, bir dış kimlik bilgilerinizle veya kuruluşunuza ait kimlik bilgileri ile oturum açabilirsiniz.

Aşağıdaki *değil* Konuk kullanıcılar:
- Çalışanlar, Yükleniciler yerinde ya da yerinde aracıları
- Çalışanlar, yerinde yüklenici veya yerinde aracıları, bağlı kuruluşlarının

B2B Konuk kullanıcı lisanslama otomatik olarak hesaplanır ve 1:5 oranını tabanlı bildirdi. Şu anda, B2B atamak mümkün değildir Konuk kullanıcı lisanslarını doğrudan konuk kullanıcılara.

Bazı durumlarda değilse Konuk kullanıcı burada raporlanan 1:5 dış kullanıcı indirimi kullanarak. Konuk kullanıcı zaten bir Ücretli varsa, kullanıcının kendi kuruluşundaki, kullanıcının lisans Azure AD B2B Konuk kullanıcı lisanslarınızdan birini kullanır değil. Ayrıca, konuk kullanıcıların kullanabileceği ek lisans gereksinimi ile Azure AD özellikleri boşaltın. Konuk kullanıcıların erişiminin bile sizde yoksa, Azure AD özellikleri boşaltmak için ücretli Azure AD lisansı. 

## <a name="examples-calculating-guest-user-licenses"></a>Örnekler: Konuk kullanıcı lisanslarını hesaplanıyor
Konuk kullanıcı, ücretli erişmesi gereken belirledikten sonra Azure AD Hizmetleri, gerekli 1:5 oranı, konuk kullanıcıların kapsanması için yeterince Azure AD lisansları Ücretli sahip olduğunuzdan emin olun. İşte bazı örnekler:

- Azure AD uygulamaları veya hizmetleri için 100 Konuk kullanıcıları davet etmek istediğiniz ve erişim yönetimi ve tüm Konuk kullanıcılar için sağlama atamak istediğiniz. Ayrıca, 50 bu Konuk kullanıcılar için mfa'yı ve koşullu erişim gerektiren istiyorsunuz. Bu birleşim karşılamak üzere 10 Azure AD temel lisansı ve 10 Azure AD Premium P1 lisansı gerekir. Konuk Kullanıcılarınızla kimlik koruma özelliklerini kullanmayı planlıyorsanız, konuk kullanıcıların kapsanması için aynı 1:5 oranı, Azure AD Premium P2 lisansı gerekir.
- Bu nedenle en az 12 Azure AD Premium P1 lisansı olması gerekir isteyen tüm MFA'yı 60 Konuk kullanıcılar davet etmek istediğiniz. 1:5 oranı lisansı altında 50 adede kadar Konuk kullanıcı izin Azure AD Premium P1 lisansı ile 10 kişiden var. 10 ek konuk kullanıcıların kapsanması için iki ek Premium P1 lisansı satın almanız gerekir.

## <a name="using-the-b2b-collaboration-api-to-invite-users-from-your-affiliates"></a>B2B işbirliği API, bağlı kuruluşlarımızla kullanıcıları davet etmek kullanma

Tanımı gereği, B2B Konuk kullanıcı davet, ücretli kullanmak için bir dış kullanıcı olduğu Azure AD uygulamaları ve Hizmetleri. B2B özellikleri kullanılıyor olsa bile, bir çalışan, yüklenici yerinde veya şirketiniz veya, bağlı kuruluşlarımızla birinin yerinde aracı, B2B işbirliği için uygun olmaz. İşte bazı örnekler: 
- Kuruluşunuzun bir çalışan bir kullanıcıyı davet etmek için harici kimlik bilgilerini (örneğin, bir sosyal kimlik) kullanmak istiyorsunuz. Bu senaryo, lisanslama gereksinimleriyle uyumlu değil ve izin verilen değil. Dış kimlik bilgileri, bir çalışan bir dış kullanıcının yapmayın.  
- Kuruluşunuzun kuruluşları birinden bir kullanıcı davet etme B2B API'leri kullanmak istiyorsunuz. Bu senaryoda kullanıcı davet etme B2B API'leri kullansa da, B2B işbirliği kabul değil. Bir kullanıcıdan, bağlı bir dış kullanıcı olmadığından lisanslama gereksinimleriyle uyumlu değil. 

Bu senaryoların her ikisinde üye olarak davet B2B API'yi kullanmak için daha iyi çözüm olduğundan (invitedUserType = üyesi) ve bunları her bir Azure AD lisansı atayın. 

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki kaynaklara bakın:

* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure Active Directory B2B işbirliği sık sorulan sorular (SSS)](faq.md)
