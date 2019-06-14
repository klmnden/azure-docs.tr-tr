---
title: Karma kimlik tasarım erişim denetimi gereksinimleri Azure | Microsoft Docs
description: Kimlik ve karma bir ortamda kullanıcılar için kaynaklar için erişim gereksinimleri tanımlama, yapı taşına kapsar.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84b786a1701892823554a83fa2015ac88d6eff4d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60295152"
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için erişim denetimi gereksinimleri belirleme
Bir kuruluşun kendi karma kimlik çözümü tasarlarken, bunlar bu fırsattan kullanıcılar için kullanılabilir hale getirmek için planlama kaynaklar için erişim gereksinimleri gözden geçirmek için kullanabilirsiniz. Veri erişimi olan tüm dört yapı taşları kimliğinin, çapraz:

* Yönetim
* Kimlik Doğrulaması
* Yetkilendirme
* Denetim

Aşağıdaki bölümlerde, kimlik doğrulama ve yetkilendirme yönetimi hakkında daha fazla bilgi kapsamakta ve karma kimlik yaşam döngüsü bir parçası olan denetim. Okuma [karma kimlik yönetimi görevleri belirlemek](plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) bu özellikler hakkında daha fazla bilgi.

> [!NOTE]
> Okuma [dört yapı taşları, Identity - karma BT yaş Identity Management'ta](https://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) her biri, bu yapı taşları hakkında daha fazla bilgi.
> 
> 

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Kimlik doğrulaması ve yetkilendirme farklı senaryo vardır, bu senaryolar şirket benimsemek için gittiği karma kimlik çözümü tarafından yerine getirilmesi gereken belirli gereksinimleri vardır. Kuruluş tarafından kullanılan kimlik doğrulama ve yetkilendirme yöntemi iş ortaklarıyla iletişim kurabildiğinden emin olun gerekir bu yana işletmeler arası (B2B) iletişimi içeren senaryolar BT yöneticileri için ek bir sınama ekleyebilirsiniz. Kimlik doğrulama ve yetkilendirme gereksinimlerine yönelik tasarım sürecinde, aşağıdaki soruları yanıtlanır emin olun:

* , Kuruluşunuzun kimlik doğrulaması ve kimlik yönetimi sistemlerine bulunan kullanıcıları yetkilendirme?
  * B2B senaryoları için herhangi bir plan var mı?
  * Yanıt Evet ise, hangi protokollerin (SAML, OAuth, Kerberos veya Sertifikalar) hem de işletmelerin bağlanmak için kullanılan zaten biliyor musunuz?
* Destek almayı oluşturacağınız karma kimlik çözümü protokollerin mu?

Dikkate alınması gereken bir diğer önemli nokta, kullanıcılar ve iş ortakları tarafından kullanılacak kimlik doğrulama deposu konumlandırılacağı ve kullanılacak yönetim modeli ' dir. Aşağıdaki iki çekirdek seçenekleri göz önünde bulundurun:

* Merkezi: Bu modelde, kullanıcının kimlik bilgilerini, ilkeleri ve Yönetim Merkezi şirket içi olarak veya bulutta.
* Karma: Bu modelde, kullanıcının kimlik bilgilerini, ilkeleri ve Yönetim Merkezi şirket içinde hem de çoğaltılmış bir bulutta olacaktır.

Kuruluşunuz benimseyeceği hangi modelle iş gereksinimlerine göre farklılık gösterir, kimlik yönetimi sistemi bulunacağı ve kullanmak için yönetici moduna tanımlamak için aşağıdaki soruları cevaplamak istediğiniz:

* Kuruluşunuz Kimlik Yönetimi şu anda sahip şirket içi?
  * Yanıt Evet ise, kalmasını sağlamak planlıyor musunuz?
  * Kuruluşunuz kimlik yönetimi sistemi bulunduğu, belirtir izlemelidir düzenlemeler veya uyumluluk gereksinimleri var mı?
* Kuruluşunuz, çoklu oturum açma bulunan uygulamalar şirket içi veya bulutta kullanıyor?
  * Evet ise, bu işlem karma kimlik modeli benimsenmesini etkiliyor?

## <a name="access-control"></a>Erişim Denetimi
Kimlik doğrulama ve yetkilendirme kullanıcının doğrulama şirket verilerine erişimi etkinleştirmek için çekirdek öğeleri olsa da, ayrıca, bu kullanıcılar ve kaynaklar erişim yöneticileri düzeyini alacaktır erişim düzeyini denetlemek önemlidir Bunlar, yönetiyorsunuz. Karma kimlik çözümü, kaynakları, temsilci ve rol tabanlı erişim denetimini ayrıntılı erişim sağlamak mümkün olması gerekir. Erişim denetimi ile ilgili aşağıdaki sorunun yanıtlandığını emin olun:

* Şirketinizin kimlik sisteminizde yönetmek için yükseltilmiş ayrıcalığa sahip birden fazla kullanıcı var mı?
  * Yanıt Evet ise, her kullanıcı aynı erişim düzeyini gerekiyor mu?
* Şirketinizin belirli kaynakları yönetmek için kullanıcılara erişimi devretmek değiştirmeliyim?
  * Yanıt Evet ise, bu ne sıklıkta gerçekleşir?
* Şirketiniz şirket içi ve bulut arasında erişim denetimi özellikleri tümleştirme gerek kaynakları?
* Şirketiniz, bazı koşullara uygun kaynaklarına erişimi sınırlamak değiştirmeliyim?
* Şirketinizin özel denetim bazı kaynaklara erişim gerektiren herhangi bir uygulama mı isterdiniz?
  * Yanıt Evet ise, burada uygulamalarla bulunur (şirket içinde veya bulutta)?
  * Evet ise burada olan bu hedef kaynaklara (şirket içinde veya bulutta)?

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md) her seçeneğin olumlu/olumsuz değerlendirilir ve seçeneklerin geçilir.  Bu soruyu yanıtlayarak, en iyi hangi seçeneğin iş gereksinimlerinize en uygun seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Olay yanıtı gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

