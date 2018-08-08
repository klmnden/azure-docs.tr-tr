---
title: Azure Active Directory'yi kullanmak için uygulamanızı kaydetme | Microsoft Docs
description: BT uzmanı için yazılan, uygulamaları Azure Active Directory ile tümleştirme için bu makaleyi kılavuz bilgiler verilmektedir.
services: active-directory
documentationcenter: ''
author: kgremban
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/14/2018
ms.author: kgremban
ms.custom: seohack1
ms.openlocfilehash: e3b22c0c602e8f3d47fbfc179fb8d0fb985d55d6
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39619330"
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Azure Active Directory için satır iş kolu uygulamaları geliştirme
Bu kılavuz, satır iş kolu (LoB) uygulamaları için Azure Active Directory (AD) geliştirmeye genel bakış sağlar. Hedef kitlesi, Active Directory/Office 365 genel yöneticileri olur.

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleştirilmiş uygulamalar oluşturmak, kuruluş çoklu oturum açmayı Office 365 ile kullanıcılara sağlar. Uygulamaya sahip olunması, üzerinde uygulama için kimlik doğrulama İlkesi denetim Azure AD'ye sağlar. Koşullu erişim ve çok faktörlü kimlik doğrulaması (MFA) bakın ile uygulamaları koruma hakkında daha fazla bilgi için [yapılandırma erişim kuralları](conditional-access/app-based-mfa.md).

Azure Active Directory'yi kullanmak için uygulamanızı kaydedin. Uygulama kaydetme, geliştiricilerinizin Azure AD kullanıcıların kimliğini doğrulama ve kullanıcı kaynaklarını e-posta, Takvim ve belgeler gibi erişim istemek için kullanabileceği anlamına gelir.

Aksi takdirde olarak bilinen, bir uygulama dizininize (Konukları değil) herhangi bir üyesi kaydedebilirsiniz *uygulama nesnesi oluşturma*.

Uygulama kaydetme, herhangi bir kullanıcı aşağıdakileri sağlar:

* Azure AD tanır, uygulama için bir kimlik almak
* Bir veya daha fazla gizli dizileri/uygulama için AD kendi kimliğini doğrulamak için kullanabileceğiniz anahtarları alma
* Marka özel ad, logo, vb. ile Azure portalında uygulama.
* Azure AD yetkilendirme özellikleri kendi, uygulamanıza dahil olmak üzere:

  * Rol Tabanlı Erişim Denetimi (RBAC)
  * OAuth yetkilendirme sunucusu olarak Azure Active Directory (uygulama tarafından kullanıma sunulan bir API güvenliğini sağlama)
* Gerekli izinleri uygulama işlevi için gerekli dahil olmak üzere beklenen gibi tanımlayın:

      - Uygulama izinleri (yalnızca genel Yöneticiler). Örneğin: bir Azure kaynak, kaynak grubuna veya aboneliğe göre başka bir Azure AD uygulama veya rol üyeliğine rol üyeliği
      - Temsilci izinleri (herhangi bir kullanıcı). Örneğin: Azure AD oturum açma ve profil okuma

> [!NOTE]
> Varsayılan olarak, herhangi bir üyenin, bir uygulama kaydedebilirsiniz. Belirli üyeleri için uygulamaları kaydetmek için izinleri kısıtla öğrenmek için bkz. [uygulamaları Azure AD'ye nasıl eklenir](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

Genel Yöneticisi olarak geliştiriciler uygulamalarını üretim için hazır olun yardımcı olması için yapmanız gerekenler şu şekildedir:

* (Erişim ilkesi/MFA) erişim kurallarını yapılandırma
* Kullanıcı atamasını gerektirir ve kullanıcılara atamak için uygulamayı yapılandırma
* Varsayılan kullanıcı onayı deneyimi Gizle

## <a name="configure-access-rules"></a>Erişim kurallarını yapılandırma
SaaS uygulamalarınız için uygulama başına erişim kurallarını yapılandırın. Örneğin, mfa'yı gerekli veya yalnızca güvenilen ağlarda kullanıcılar erişime izin vermek. Ayrıntılar için bu belgede kullanılabilir [yapılandırma erişim kuralları](conditional-access/app-based-mfa.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Kullanıcı atamasını gerektirir ve kullanıcılara atamak için uygulamayı yapılandırma
Varsayılan olarak, kullanıcılar atanmadan uygulamalarına erişebilir. Ancak, uygulama rolleri sunarsa ya da uygulamanın bir kullanıcının erişim panelinde görünmesini istiyorsanız kullanıcı atamasını istemeniz gerekir.

[Kullanıcı ataması gerektirme](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Bir Azure AD Premium veya Enterprise Mobility Suite (EMS) abonesi iseniz, grupları kullanarak kesinlikle öneririz. Grupları uygulamaya atama temsilci grubun sahibine sürekli erişim yönetimi sağlar. Grubu oluşturun veya sorumlu kişi, Grup Yönetimi özelliğini kullanarak bir grup oluşturmak için kuruluşunuzda isteyin.

[Uygulamaya kullanıcı atama](active-directory-applications-guiding-developers-assigning-users.md)  
[Uygulamaya grup atama](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Kullanıcı onayı Gizle
Varsayılan olarak, her bir kullanıcı oturum açmak için bir onay deneyiminde geçer. Bir uygulama izni vermek isteyen onayı deneyimi disconcerting kullanıcılar bu kararların ile bilginiz olabilir.

Güvendiğiniz uygulamalar için kuruluşunuz adına uygulamayı tümleştirip tarafından kullanıcı deneyimini kolaylaştırabilirsiniz.

Kullanıcı onayı hakkında daha fazla bilgi ve onay Azure'da deneyimi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](develop/quickstart-v1-integrate-apps-with-azure-ad.md).

## <a name="related-articles"></a>İlgili makaleler
* [Azure AD uygulama ara sunucusu ile şirket içi uygulamalara güvenli uzaktan erişim'i etkinleştir](manage-apps/application-proxy.md)
* [Azure AD ile uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
