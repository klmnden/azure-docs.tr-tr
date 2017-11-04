---
title: "Azure Active Directory kimlik koruması - kullanıcıların engelini kaldırma | Microsoft Docs"
description: "Bilgi nasıl bir Azure Active Directory kimlik koruması İlkesi tarafından engellendi kullanıcıların Engellemeyi Kaldır."
services: active-directory
keywords: "Azure active directory kimlik koruması, kullanıcının engelini kaldırma"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 0bef2fa17474298f36f360efa49922f5f85b21a1
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory kimlik koruması - kullanıcıların engelini kaldırma
Azure Active Directory kimlik koruması ile yapılandırılmış koşullar sağlanırsa kullanıcıları engellemek için ilkeler yapılandırabilirsiniz. Engeli olmasını genellikle, bir engellenen kullanıcı kişiler Yardım Masası. Bu konular, engellenen bir kullanıcının engelini kaldırmak için uygulayacağınız adımlar açıklanmaktadır.

## <a name="determine-the-reason-for-blocking"></a>Engelleme nedeni belirleme
Bir kullanıcının engelini kaldırmak için ilk adım, sonraki adımlarda üzerinde çünkü kullanıcı engelledi İlkesi türünü belirlemeniz gerekir.
Azure Active Directory kimlik koruması ile bir kullanıcı ya da bir oturum açma riski İlkesi veya bir kullanıcı risk İlkesi tarafından engellenebilir.

Bir kullanıcıdan kullanıcıya bir oturum açma girişimi sırasında sunulan iletişim başlığına engelledi ilke türünü alabilirsiniz:

| İlke | Kullanıcı iletişim kutusu |
| --- | --- |
| Oturum açma riski |![Engellenen oturum açma](./media/active-directory-identityprotection-unblock-howto/02.png) |
| Kullanıcı riski |![Engellenen hesabı](./media/active-directory-identityprotection-unblock-howto/104.png) |

Tarafından engellenen bir kullanıcı:

* Bir oturum açma riski olarak da bilinen şüpheli oturum açma ilkedir
* Kullanıcı risk ilkesi olarak da bilinen bir risk hesabıdır

## <a name="unblocking-suspicious-sign-ins"></a>Engellemeyi kaldırma şüpheli oturum açma işlemleri
Bir şüpheli oturum açma engelini kaldırmak için aşağıdaki seçenekleriniz vardır:

1. **Oturum açma tanıdık bir konumdan veya aygıt** -engellenen şüpheli oturum açma işlemleri için ortak bir nedeni olan oturum açma denemeleri tanınmayan konumlardan veya cihazlar. Kullanıcılarınıza hızlı bir şekilde oturum tanıdık konumu ya da cihaz açmak deneyerek bu engelleme nedeni geçerli olup olmadığını belirleyebilirsiniz.
2. **İlkenin dışında tutmak** - düşünüyorsanız, oturum açma ilkenizin geçerli yapılandırmasını belirli kullanıcılar için sorunlara neden olup, kullanıcıların dışlayabilirsiniz. Bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) daha fazla ayrıntı için.
3. **İlkeyi devre dışı** - düşünüyorsanız, ilke yapılandırması, tüm kullanıcılar için sorunlara neden olup, ilkeyi devre dışı bırakabilirsiniz. Bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) daha fazla ayrıntı için.

## <a name="unblocking-accounts-at-risk"></a>Risk engellemesini kaldırma hesapları
Hesabınız risk engelini kaldırmak için aşağıdaki seçenekleriniz vardır:

1. **Parola sıfırlama** -kullanıcının parolasını sıfırlayabilir. Bkz: [el ile güvenli parola sıfırlama](active-directory-identityprotection.md#manual-secure-password-reset) daha fazla ayrıntı için.
2. **Tüm risk olayı kapatmak** - kullanıcı erişimi engelleme için düzeyi yapılandırılan kullanıcı riski varsa ulaşıldı kullanıcı risk İlkesi engeller. Bir kullanıcı azaltabilir el ile kapatarak risk düzeyi risk olaylarını bildirilen kullanıcının. Daha fazla ayrıntı için bkz: [risk olaylarını el ile kapatma](active-directory-identityprotection.md#closing-risk-events-manually).
3. **İlkenin dışında tutmak** - düşünüyorsanız, oturum açma ilkenizin geçerli yapılandırmasını belirli kullanıcılar için sorunlara neden olup, kullanıcıların dışlayabilirsiniz. Bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) daha fazla ayrıntı için.
4. **İlkeyi devre dışı** - düşünüyorsanız, ilke yapılandırması, tüm kullanıcılar için sorunlara neden olup, ilkeyi devre dışı bırakabilirsiniz. Bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md) daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar
 Azure AD kimlik koruması hakkında daha fazla bilgi istiyor musunuz? Kullanıma [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).
