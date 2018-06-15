---
title: Azure Active Directory kimlik koruması - kullanıcıların engelini kaldırma | Microsoft Docs
description: Bilgi nasıl bir Azure Active Directory kimlik koruması İlkesi tarafından engellendi kullanıcıların Engellemeyi Kaldır.
services: active-directory
keywords: Azure active directory kimlik koruması, kullanıcının engelini kaldırma
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.component: protection
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1e96f24aeb0083e57618ad3e38163f50c23c55d3
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34713465"
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory kimlik koruması - kullanıcıların engelini kaldırma
Azure Active Directory kimlik koruması ile yapılandırılmış koşullar sağlanırsa kullanıcıları engellemek için ilkeler yapılandırabilirsiniz. Engeli olmasını genellikle, bir engellenen kullanıcı kişiler Yardım Masası. Bu makalede, engellenen bir kullanıcının engelini kaldırmak için uygulayacağınız adımlar açıklanmaktadır.

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

1. **Bir bilinen konumu veya aygıt oturum** -engellenen şüpheli oturum açma işlemleri için ortak bir nedeni olan oturum açma denemeleri tanınmayan konumlardan veya cihazlar. Kullanıcılarınıza hızlı bir şekilde oturum tanıdık konumu ya da cihaz açmak deneyerek bu engelleme nedeni geçerli olup olmadığını belirleyebilirsiniz.
2. **İlkenin dışında tutmak** - düşünüyorsanız, oturum açma ilkenizin geçerli yapılandırmasını belirli kullanıcılar için sorunlara neden olup, kullanıcıların dışlayabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).
3. **İlkeyi devre dışı** - düşünüyorsanız, ilke yapılandırması, tüm kullanıcılar için sorunlara neden olup, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Risk engellemesini kaldırma hesapları
Hesabınız risk engelini kaldırmak için aşağıdaki seçenekleriniz vardır:

1. **Parola sıfırlama** -kullanıcının parolasını sıfırlayabilir. Daha fazla bilgi için bkz: [el ile güvenli parola sıfırlama](active-directory-identityprotection.md#manual-secure-password-reset).
2. **Tüm risk olayı kapatmak** - kullanıcı erişimi engelleme için düzeyi yapılandırılan kullanıcı riski varsa ulaşıldı kullanıcı risk İlkesi engeller. Bir kullanıcı azaltabilir el ile kapatarak risk düzeyi risk olaylarını bildirilen kullanıcının. Daha fazla bilgi için bkz: [risk olaylarını el ile kapatma](active-directory-identityprotection.md#closing-risk-events-manually).
3. **İlkenin dışında tutmak** - düşünüyorsanız, oturum açma ilkenizin geçerli yapılandırmasını belirli kullanıcılar için sorunlara neden olup, kullanıcıların dışlayabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).
4. **İlkeyi devre dışı** - düşünüyorsanız, ilke yapılandırması, tüm kullanıcılar için sorunlara neden olup, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için bkz: [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).

## <a name="next-steps"></a>Sonraki adımlar
 Azure AD kimlik koruması hakkında daha fazla bilgi istiyor musunuz? Kullanıma [Azure Active Directory kimlik koruması](active-directory-identityprotection.md).
