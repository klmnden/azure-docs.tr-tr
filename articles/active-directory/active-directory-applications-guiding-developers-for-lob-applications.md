---
title: "Uygulama geliştirmek için Azure AD | Microsoft Docs"
description: "BT uzmanı için yazılmış, Azure uygulamalarını Active Directory ile tümleştirme için bu makaleyi kılavuz bilgiler verilmektedir."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: dd69f2bc-37c5-457c-857d-27acb84267fb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 76bb8d239b54b5236d077b98a5908c230cdc31a1
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2017
---
# <a name="develop-line-of-business-apps-for-azure-active-directory"></a>Azure Active Directory için satır iş kolu uygulamaları geliştirme
Bu kılavuz çizgi iş kolu (LoB) uygulamaları için Azure Active Directory (AD) geliştirme genel bir bakış sağlar. Hedef kitle Active Directory/Office 365 genel yönetici olur.

## <a name="overview"></a>Genel Bakış
Azure AD ile tümleşik uygulamaları oluşturmak, kuruluş çoklu oturum açma Office 365 ile kullanıcılara sağlar. Uygulama bu uygulama için kimlik doğrulama İlkesi üzerinden denetim Azure AD sağlar sahip. Koşullu erişim ve çok faktörlü kimlik doğrulama (MFA) bkz: uygulamalarla koruma hakkında daha fazla bilgi için [yapılandırma erişim kuralları](active-directory-conditional-access-azure-portal-get-started.md).

Azure Active Directory'yi kullanmak için uygulamanızı kaydetme. Uygulama kaydetme, geliştiricilerin Azure AD kullanıcıların kimliğini doğrulamak ve e-posta, Takvim ve belgeler gibi kullanıcı kaynaklara erişim istemek için kullanabileceğiniz anlamına gelir.

Aksi takdirde olarak bilinen bir uygulama dizininize (Konuklar değil) herhangi bir üyenin kaydedebilirsiniz *uygulama nesnesi oluşturma*.

Uygulama kaydetme herhangi bir kullanıcı aşağıdakileri sağlar:

* Azure AD tanıdığı kendi uygulama için bir kimlik alma
* Bir veya daha fazla gizli/uygulama için AD kendi kimliğini doğrulamak için kullanabileceğiniz anahtarı alma
* Marka özel adı, logosu, vb. ile Azure portalında uygulama.
* Azure AD yetkilendirme özelliklerini kendi uygulama için geçerli dahil olmak üzere:

  * Rol Tabanlı Erişim Denetimi (RBAC)
  * OAuth yetkilendirme sunucusu olarak Azure Active Directory (uygulama tarafından kullanıma sunulan bir API güvenli)
* Gerekli izinleri uygulama işlevi için gerekli dahil olmak üzere, beklendiği gibi bildirin:

      - Uygulama izinleri (yalnızca genel Yöneticiler). Örneğin: bir Azure kaynak, kaynak grubuna veya aboneliğe göre başka bir Azure AD uygulama ya da rolü üyeliği rolü üyeliği
      - Temsilci izinleri (herhangi bir kullanıcı). Örneğin: Azure AD oturum açma ve okuma profili

> [!NOTE]
> Varsayılan olarak, bir uygulama herhangi bir üyenin kaydedebilirsiniz. Uygulamalar belirli üyeleri kaydettirmek için izinleri kısıtlamak öğrenmek için bkz: [uygulamaları Azure AD ile nasıl eklenir](develop/active-directory-how-applications-are-added.md#who-has-permission-to-add-applications-to-my-azure-ad-instance).
>
>

İşte, genel yönetici olarak size, geliştiricilerin kendi uygulama üretime hazır olun yardımcı olmak için yapmanız gerekenler:

* Erişim kuralları (erişim ilkesi/MFA) yapılandırma
* Kullanıcı Ataması gerektirir ve kullanıcılara atamak için uygulamayı yapılandırma
* Varsayılan kullanıcı onayı deneyimi gösterme

## <a name="configure-access-rules"></a>Erişim kuralları yapılandırın
SaaS uygulamaları için uygulama başına erişim kurallarını yapılandırın. Örneğin, MFA gerektirecek veya yalnızca güvenilen ağlarda kullanıcılara erişimi izin verebilirsiniz. Ayrıntılar için bu belgede mevcuttur [yapılandırma erişim kuralları](active-directory-conditional-access-azure-portal-get-started.md).

## <a name="configure-the-app-to-require-user-assignment-and-assign-users"></a>Kullanıcı Ataması gerektirir ve kullanıcılara atamak için uygulamayı yapılandırma
Varsayılan olarak, kullanıcılar atanmadan uygulamalarına erişebilir. Ancak, uygulama rolleri gösterir veya uygulamanın bir kullanıcının erişim Panoda görünmesini istiyorsanız, kullanıcı ataması istemeniz gerekir.

[Kullanıcı ataması gerektirme](active-directory-applications-guiding-developers-requiring-user-assignment.md)

Bir Azure AD Premium veya Enterprise Mobility Suite (EMS) abone değilseniz, grupları kullanmanızı öneririz. Uygulama grupları atama grubunun devam eden erişim yönetimi sahibine temsilci sağlar. Grubu oluşturmak veya Grup Yönetimi özelliğini kullanarak grubu oluşturmak için kuruluşunuzda sorumlu taraf isteyin.

[Uygulamaya kullanıcı atama](active-directory-applications-guiding-developers-assigning-users.md)  
[Uygulamaya grup atama](active-directory-applications-guiding-developers-assigning-groups.md)

## <a name="suppress-user-consent"></a>Kullanıcı izni gösterme
Varsayılan olarak, her bir kullanıcı oturum açmak için onayı deneyimi geçer. Kullanıcıların bir uygulama izinleri isteyen onayı deneyimi böyle kararları ile ilgili bilginiz kullanıcılar için disconcerting olabilir.

Güvendiğiniz uygulamalar için kuruluşunuz adına uygulama onaylıyorsunuz tarafından kullanıcı deneyimini basitleştirebilirsiniz.

Kullanıcı izni hakkında daha fazla bilgi ve onay Azure'da deneyimi için bkz: [Azure Active Directory Tümleştirme uygulamalarla](active-directory-integrating-applications.md).

## <a name="related-articles"></a>İlgili Makaleler
* [Azure AD uygulama proxy'si ile şirket içi uygulamalara güvenli uzaktan erişimi etkinleştir](active-directory-application-proxy-get-started.md)
* [SaaS uygulamaları için Azure koşullu erişim önizlemesi](active-directory-conditional-access-azure-portal-get-started.md)
* [Azure AD ile uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
