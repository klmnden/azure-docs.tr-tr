---
title: Karma kimlik tasarımı - Azure çok faktörlü kimlik doğrulaması gereksinimleri | Microsoft Docs
description: Koşullu erişim denetimi ile Azure Active Directory kullanıcı kimlik doğrulaması yapılırken ve uygulamaya erişime izin vermeden önce çekme belirli koşullara bakar. Bu koşullar sağlandığında, kullanıcı kimlik doğrulaması ve uygulamaya erişim izni.
documentationcenter: ''
services: active-directory
author: billmath
manager: billmath
editor: ''
ms.assetid: 9c59fda9-47d0-4c7e-b3e7-3575c29beabe
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3dabb381c16aa107e41c1d556e61e020b8c6a6c3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60455746"
---
# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için çok faktörlü kimlik doğrulama gereksinimlerini belirleme
Bu dünyada Mobility, veri ve uygulamaları bulutta ve herhangi bir CİHAZDAN erişen kullanıcılar ile bu bilgiyi güvenli hale getirme güvenemediği haline gelmiştir.  Her gün bir güvenlik ihlali hakkında yeni bir başlık yoktur.  Olmasa da, bu tür İhlallerin karşı garanti, çok faktörlü kimlik doğrulaması, bir ek bu ihlallerini önlemeye yardımcı olmak için güvenlik katmanı sağlar.
Çok faktörlü kimlik doğrulaması için kuruluşların gereksinimleri değerlendirerek başlatın. Diğer bir deyişle, güvenli hale getirmek ne kuruluş çalışıyor.  Bu değerlendirme ayarlama ve kuruluşların kullanıcıları çok faktörlü kimlik doğrulaması için etkinleştirme teknik gereksinimlerini tanımlamak önemlidir.

Aşağıda yanıtlanacak emin olun:

* Şirketiniz Microsoft uygulamaları güvenli hale getirmek çalışıyor? 
* Bu uygulama nasıl yayımlanır?
* Şirket, çalışanların şirket içi uygulamalara erişmesine izin vermek için uzaktan erişim sağlar mı?

Yanıt Evet ise, uzaktan erişimi ne tür? Ayrıca bu uygulamalara erişen kullanıcılar konumlandırılacağı değerlendirilecek gerekir. Bu değerlendirme uygun çok faktörlü kimlik doğrulama stratejisi tanımlamak için başka bir önemli bir adımdır. Aşağıdaki soruları yanıtlamak emin olun:

* Kullanıcılarınızın bulunduğu nereye gittiğini?
* Bunlar herhangi bir yerde bulunan olabilir?
* Şirketiniz kullanıcının konumuna göre kısıtlamaları kurmak istiyor mu?

Bu gereksinimler anladığınızda, ayrıca çok faktörlü kimlik doğrulaması kullanıcının gereksinimlerini değerlendirmek önemlidir. Bu değerlendirme önemlidir, çünkü çok faktörlü kimlik doğrulaması alma için gereksinimleri tanımlar. Aşağıdaki soruları yanıtlamak emin olun:

* Kullanıcıların multi-Factor authentication ile tanıyor musunuz?
* Bazı kullanan ek kimlik doğrulaması sağlamak için gerekli olacak mı?  
  * Yanıt Evet ise, her zaman dış ağlara ya da erişimi belirli uygulamalara veya diğer koşullar altında Bekletmeden çıkarken?
* Kullanıcıların, Kurulum ve çok faktörlü kimlik doğrulaması uygulamak için eğitim gerekiyor mu?
* Şirket kullanıcıları için çok faktörlü kimlik doğrulamasını etkinleştirmek isteyen önemli senaryolar nelerdir?

Önceki soruyu yanıtlayarak sonra şirket içinde çok faktörlü kimlik doğrulamasını zaten uygulanmış olup olmadığının anlaşılması mümkün olacaktır. Bu değerlendirme ayarlama ve kuruluşların kullanıcıları çok faktörlü kimlik doğrulaması için etkinleştirme teknik gereksinimlerini tanımlamak önemlidir. Aşağıdaki soruları yanıtlamak emin olun:

* MFA ile ayrıcalıklı hesapları korumak şirketinizin gerekiyor mu?
* Şirketiniz, mfa'yı uyumluluk nedenleriyle belirli uygulama etkinleştirme gerekiyor mu?
* Şirketiniz tüm kullanıcılar bu uygulamayı ya da yalnızca Yöneticiler için mfa'yı etkinleştirme gerekiyor mu?
* Mfa'yı her zaman etkin veya yalnızca kullanıcılar, kuruluş ağının dışında olduğunda açmış mı?

## <a name="next-steps"></a>Sonraki adımlar
[Karma kimlik benimseme stratejinizi tanımlayın](plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)

## <a name="see-also"></a>Ayrıca bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

