---
title: Azure Active Directory kimlik koruması - kullanıcı engelini kaldırmak nasıl | Microsoft Docs
description: Bilgi nasıl bir Azure Active Directory kimlik koruması ilke tarafından engellenen kullanıcıların engelini kaldırma.
services: active-directory
keywords: Azure active directory kimlik koruması, kullanıcı engelini kaldır
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: a953d425-a3ef-41f8-a55d-0202c3f250a7
ms.service: active-directory
ms.component: conditional-access
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2018
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 1bfc70680ecef2ee4fe162f81aac71430c773740
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005087"
---
# <a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory kimlik koruması - kullanıcı engellemesini kaldırma
Azure Active Directory kimlik koruması ile yapılandırılmış koşullara sağlanırsa, kullanıcıları engellemek için ilkeler yapılandırabilirsiniz. Engeli için genellikle, engellenen bir kullanıcı kişiler Yardım Masası. Bu makalede, engellenen bir kullanıcının engelini kaldırmak için gerçekleştirebileceğiniz adımlar açıklanmaktadır.

## <a name="determine-the-reason-for-blocking"></a>Engelleme nedeni belirleyin
Bir kullanıcının engelini kaldırmak için ilk adım, sonraki adımlarda üzerinde çünkü kullanıcı engelledi ilke türünü belirlemeniz gerekir.
Azure Active Directory kimlik koruması ile bir kullanıcı ya da bir oturum açma riski İlkesi veya bir kullanıcı risk İlkesi tarafından engellenebilir.

Bir kullanıcıdan kullanıcıya bir oturum açma girişimi sırasında sunulan iletişim başlığı engelledi ilke türünü alabilirsiniz:

| İlke | Kullanıcı iletişim kutusu |
| --- | --- |
| Oturum açma riski |![Engellenen oturum açma](./media/howto-unblock-user/02.png) |
| Kullanıcı riski |![Engellenen hesap](./media/howto-unblock-user/104.png) |

Tarafından engellenen bir kullanıcı:

* Şüpheli oturum açma olarak da bilinen olan bir oturum açma riski İlkesi
* Kullanıcı riski ilkesi olarak da bilinen bir risk hesabıdır

## <a name="unblocking-suspicious-sign-ins"></a>Engellemeyi kaldırma şüpheli oturum açma işlemleri
Bir şüpheli oturum açma engelini kaldırmak için aşağıdaki seçenekleriniz vardır:

1. **Tanıdık konumu veya CİHAZDAN oturum** -engellenen şüpheli oturum açma işlemleri için yaygın bir nedeni olan oturum açma denemesi alışılmadık konumlardan veya cihazlar. Kullanıcılarınız tanıdık konumu veya CİHAZDAN oturum deneyerek bu engelleme nedeni geçerli olup olmadığını hızlı bir şekilde belirleyebilirsiniz.
2. **İlkenin dışında bırakılacak** - düşünüyorsanız, oturum açma ilkesi yapılandırmasına belirli kullanıcılar için sorunlarına neden, kullanıcıların dışlayabilirsiniz. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).
3. **Bu ilkeyi devre dışı** - düşünüyorsanız İlkesi yapılandırmanızı, kullanıcılarınızın sorunlarına neden, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

## <a name="unblocking-accounts-at-risk"></a>Engellemeyi kaldırma hesapları risk altında
Hesabınız risk altında engelini kaldırmak için aşağıdaki seçenekleriniz vardır:

1. **Parolayı Sıfırla** -kullanıcının parolasını sıfırlayabilirsiniz. Daha fazla bilgi için [el ile güvenli parola sıfırlama](overview.md#manual-secure-password-reset).
2. **Tüm risk olayları kapatılamadı** - bir kullanıcı yapılandırılmış kullanıcı risk düzeyi için erişimi engelleme, ulaşıldı kullanıcı riski İlkesi engeller. Bir kullanıcı azaltabilir risk olaylarını elle kapatma tarafından risk düzeyi bildirilen kullanıcının. Daha fazla bilgi için [risk olaylarını elle kapatma](overview.md#closing-risk-events-manually).
3. **İlkenin dışında bırakılacak** - düşünüyorsanız, oturum açma ilkesi yapılandırmasına belirli kullanıcılar için sorunlarına neden, kullanıcıların dışlayabilirsiniz. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).
4. **Bu ilkeyi devre dışı** - düşünüyorsanız İlkesi yapılandırmanızı, kullanıcılarınızın sorunlarına neden, ilkeyi devre dışı bırakabilirsiniz. Daha fazla bilgi için [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).

## <a name="next-steps"></a>Sonraki adımlar
 
Azure AD kimlik koruması hakkında daha fazla bilgi edinmek istiyor musunuz? Kullanıma [Azure Active Directory kimlik koruması](../active-directory-identityprotection.md).
