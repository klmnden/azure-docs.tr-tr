---
title: B2B işbirliği lisanslama Kılavuzu - Azure Active Directory | Microsoft Docs
description: Azure AD lisansı işbirliği gerektirmez Azure Active Directory B2B Ücretli, ancak, ayrıca özellikleri B2B Konuk kullanıcılar için ödeme
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7fa01a6bf522061e54e9622cb9201f81c699a8ec
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60412446"
---
# <a name="azure-active-directory-b2b-collaboration-licensing-guidance"></a>Azure Active Directory B2B işbirliği lisanslama kılavuzu

Azure Active Directory (Azure AD) ile işletmeden işletmeye (B2B) işbirliği, ücretli, kullanılacak dış kullanıcılar (veya "konuk kullanıcılar") davet edebilirsiniz Azure AD Hizmetleri. Her Ücretli Azure AD lisansı, bir kullanıcıya atamak için dış kullanıcı indirimi altında en fazla beş Konuk kullanıcılar davet edebilirsiniz.

B2B Konuk kullanıcı lisanslama otomatik olarak hesaplanır ve 1:5 oranını tabanlı bildirdi. Şu anda, B2B atamak mümkün değildir Konuk kullanıcı lisanslarını doğrudan konuk kullanıcılara.

Ayrıca, konuk kullanıcıların kullanabileceği ek lisans gereksinimi ile Azure AD özellikleri boşaltın. Konuk kullanıcıların erişiminin bile sizde yoksa, Azure AD özellikleri boşaltmak için ücretli Azure AD lisansı. 

## <a name="examples-calculating-guest-user-licenses"></a>Örnekler: Konuk kullanıcı lisanslarını hesaplanıyor
Konuk kullanıcı, ücretli erişmesi gereken belirledikten sonra Azure AD Hizmetleri, gerekli 1:5 oranı, konuk kullanıcıların kapsanması için yeterince Azure AD lisansları Ücretli sahip olduğunuzdan emin olun. İşte bazı örnekler:

- Azure AD uygulamaları veya hizmetleri için 100 Konuk kullanıcıları davet etmek istediğiniz ve erişim yönetimi ve tüm Konuk kullanıcılar için sağlama atamak istediğiniz. Ayrıca, 50 bu Konuk kullanıcılar için mfa'yı ve koşullu erişim gerektiren istiyorsunuz. Bu birleşim karşılamak üzere 10 Azure AD temel lisansı ve 10 Azure AD Premium P1 lisansı gerekir. Konuk Kullanıcılarınızla kimlik koruma özelliklerini kullanmayı planlıyorsanız, konuk kullanıcıların kapsanması için aynı 1:5 oranı, Azure AD Premium P2 lisansı gerekir.
- Bu nedenle en az 12 Azure AD Premium P1 lisansı olması gerekir isteyen tüm MFA'yı 60 Konuk kullanıcılar davet etmek istediğiniz. 1:5 oranı lisansı altında 50 adede kadar Konuk kullanıcı izin Azure AD Premium P1 lisansı ile 10 kişiden var. 10 ek konuk kullanıcıların kapsanması için iki ek Premium P1 lisansı satın almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD B2B işbirliği hakkında aşağıdaki kaynaklara bakın:

* [Azure Active Directory fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/)
* [Azure AD B2B işbirliği nedir?](what-is-b2b.md)
* [Azure Active Directory B2B işbirliği hakkında sık sorulan sorular (SSS)](faq.md)
