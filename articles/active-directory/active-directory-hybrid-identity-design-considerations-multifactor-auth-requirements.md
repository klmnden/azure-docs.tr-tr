---
title: Karma kimlik tasarımı - çok faktörlü kimlik doğrulama gereksinimlerini Azure | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı doğrulanırken ve uygulamaya erişimine izin vermeden önce çekme belirli koşullar denetler. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: femila
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 998aebfc38c4a0971a5071faebdeae4dbca86690
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümü çok faktörlü kimlik doğrulama gereksinimlerini belirleme
Veri ve uygulamaları bulutta ve herhangi bir CİHAZDAN erişen kullanıcılar ile yeteneği bu dünyasında bu bilgileri güvenlik altına almanın en önemli haline gelmiştir.  Her gün yeni bir başlık güvenlik ihlali hakkında yoktur.  Olmasına karşın, bu tür ihlallerine karşı garanti, çok faktörlü kimlik doğrulaması, ek bir bu ihlallerini önlemeye yardımcı olmak için güvenlik katmanı sağlar.
Çok faktörlü kimlik doğrulaması kuruluşların gereksinimleri değerlendirilirken tarafından başlatın. Diğer bir deyişle, ne güvenli hale getirmek kuruluş çalışıyor.  Bu değerlendirme ayarlama ve kuruluşların kullanıcıları çok faktörlü kimlik doğrulaması için etkinleştirme teknik gereksinimlerini tanımlamak önemlidir.

> [!NOTE]
> MFA ile ne yaptığını bilmiyorsanız, makaleyi okuyun önerilir [Azure multi-Factor Authentication nedir?](authentication/multi-factor-authentication.md) bu bölümü okumadan devam etmek için önceki.
> 
> 

Aşağıda yanıtlanacak emin olun:

* Şirketiniz Microsoft uygulamaları güvenli mi çalışıyor? 
* Bu uygulamaları nasıl yayımlanacak?
* Şirket içi uygulamalara erişmek çalışanlar izin vermek için uzaktan erişim sağlar?

Yanıt Evet ise, hangi uzaktan erişimi tür? Ayrıca bu uygulamalara erişen kullanıcılar nerede bulunacağı değerlendirmeniz gerekir. Bu değerlendirme uygun çok faktörlü kimlik doğrulama stratejisi tanımlamak için başka bir önemli bir adımdır. Aşağıdaki soruları yanıtlayın emin olun:

* Burada kullanıcıların bulunduğu olacak?
* Bunlar herhangi bir yerde bulunan olabilir mi?
* Şirketinizde kullanıcının konumuna göre kısıtlamaları kurmak istiyor mu?

Bu gereksinimleri anladığınızda, kullanıcının gereksinimlerinin çok faktörlü kimlik doğrulaması için de değerlendirmek önemlidir. Bu değerlendirme önemlidir, çünkü çok faktörlü kimlik doğrulaması alma gereksinimlerini tanımlayacaksınız. Aşağıdaki soruları yanıtlayın emin olun:

* Kullanıcılar, çok faktörlü kimlik doğrulamasıyla bilginiz?
* Bazı kullanan ek kimlik doğrulama sağlamak için gerekli olacak mı?  
  * Yanıt Evet ise, her zaman dış ağlara ya da erişimi belirli uygulamalar veya diğer koşullar altında çıkarken?
* Kullanıcılar, eğitim Kurulum ve çok faktörlü kimlik doğrulaması uygulamak nasıl gerektirecek mi?
* Kendi kullanıcıları için çok faktörlü kimlik doğrulamasını etkinleştirmek için şirketinizin istediği temel senaryolar nelerdir?

Önceki sorulara yanıt verilmesi sonra çok faktörlü kimlik doğrulaması zaten uygulanmış şirket içi varsa anlamak mümkün olacaktır. Bu değerlendirme ayarlama ve kuruluşların kullanıcıları çok faktörlü kimlik doğrulaması için etkinleştirme teknik gereksinimlerini tanımlamak önemlidir. Aşağıdaki soruları yanıtlayın emin olun:

* Şirketiniz MFA ile ayrıcalıklı hesaplara korumak istiyor mu?
* Şirketinizin uyumluluk nedenleriyle belirli uygulama MFA'yı etkinleştirmek istiyor mu?
* Şirketiniz, bu uygulama ya da yalnızca Yöneticiler tüm uygun kullanıcılar için MFA'yı etkinleştirmek istiyor mu?
* Her zaman etkin MFA veya yalnızca zaman kullanıcıların şirket ağının dışından oturum olmak zorunda?

## <a name="next-steps"></a>Sonraki adımlar
[Bir karma kimlik benimseme stratejinizi tanımlayın](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

