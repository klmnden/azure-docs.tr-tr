---
title: "Karma kimlik tasarımı erişim denetimi gereksinimlerine Azure | Microsoft Docs"
description: "Kimlik ve karma bir ortamda kullanıcıları için kaynaklar için erişim gereksinimleri tanımlama ayaklar kapsar."
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: e3b3b984-0d15-4654-93be-a396324b9f5e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: 161820e69b0c9d0dc376a62cecceb9cc5e83c8ce
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümü için erişim denetimi gereksinimleri belirleme
Bir kuruluş kendi karma kimlik çözümü tasarlarken, bu fırsatı kullanıcılar için kullanılabilir hale getirmek için planlama kaynaklar için erişim gereksinimleri gözden geçirmek için kullanabilirsiniz. Veri erişimi olan tüm dört ayaklar kimliğini, çapraz:

* Yönetim
* Kimlik Doğrulaması
* Yetkilendirme
* Denetim

Aşağıdaki bölümlerde, kimlik doğrulama ve yetkilendirme daha ayrıntılı ele alınacaktır, yönetim ve denetimi karma kimlik yaşam döngüsü parçasıdır. Okuma [karma kimlik yönetimi görevleri belirlemek](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) bu özellikleri hakkında daha fazla bilgi için.

> [!NOTE]
> Okuma [dört ayaklar kimliği - karma BT yaş Identity Management](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx) bu dayanaklarından her biri hakkında daha fazla bilgi.
> 
> 

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Kimlik doğrulama ve yetkilendirme için farklı senaryolar vardır, bu senaryoların şirket benimsemeye gittiği karma kimlik çözümü tarafından yerine getirilmesi gereken belirli gereksinimleri vardır. Kuruluş tarafından kullanılan kimlik doğrulama ve yetkilendirme yöntemi iş ortakları ile iletişim kurabildiğinden emin olun gerekir bu yana işletmeler için (B2B) iletişim içeren senaryoları için BT yöneticileri bir ek sınama ekleyebilirsiniz. Kimlik doğrulama ve yetkilendirme gereksinimlerine yönelik tasarım işlemi sırasında aşağıdaki soruları yanıtlanır emin olun:

* Kuruluşunuzun kimlik doğrulaması ve kimlik yönetimi sistemlerine bulunan kullanıcıları yetkilendirmek?
  * B2B senaryolarını için herhangi bir plan var mı?
  * Yanıt Evet ise, hangi protokollerin (SAML, OAuth, Kerberos, belirteçleri veya sertifikaları) her iki işletmeler bağlanmak için kullanılan zaten biliyor musunuz?
* Destek benimsemeyi kalacaklarını karma kimlik çözümü protokollerin mu?

Dikkate alınması gereken başka bir önemli olduğu kullanıcılar ve iş ortakları tarafından kullanılacak kimlik doğrulama deposu bulunur ve kullanılacak yönetim modeli noktasıdır. Aşağıdaki iki çekirdek seçenekleri göz önünde bulundurun:

* Merkezi: Bu modelde, kullanıcının kimlik bilgilerini, ilkeleri ve Yönetim Merkezi şirket içi olabilir veya bulutta.
* Karma: Bu modelde, kullanıcının kimlik bilgilerini, ilkeleri ve Yönetim Merkezi şirket içi ve çoğaltılan bulutta olacaktır.

Kuruluşunuz benimseyeceği hangi modeli iş gereksinimlerine göre farklılık gösterir, kimlik yönetimi sistemi bulunacağı ve kullanmak için yönetici moduna belirlemek için aşağıdaki soruları yanıtlayın istiyor:

* Kuruluşunuz şu anda bir kimlik yönetimi sahip şirket içi?
  * Yanıt Evet ise, kalmasını sağlamak planlıyor musunuz?
  * Kuruluşunuz bu belirtir kimlik yönetimi sistemi bulunduğu izlemelisiniz düzenleme veya uyumluluk gereksinimleri var mı?
* Kuruluşunuz çoklu oturum açma bulunan uygulamalar şirket içi veya bulutta kullanıyor mu?
  * Yanıt Evet ise, bir karma kimlik modeli benimsenmesi bu işlem etkiliyor mu?

## <a name="access-control"></a>Access Control
Kimlik doğrulama ve yetkilendirme kullanıcının doğrulama şirket verilerine erişimi etkinleştirmek için çekirdek öğeleri olsa da, ayrıca, bu kullanıcıların sahip ve erişim yöneticileri düzeyini yönetmekte olduğunuz kaynakları sahip erişim düzeyini denetlemek önemlidir. Karma kimlik çözümü kaynaklar, temsilci ve rol tabanlı erişim denetimini ayrıntılı erişim sağlamak mümkün olması gerekir. Aşağıdaki soruyu erişim denetimi ile ilgili yanıtlanır emin olun:

* Şirketinizin yükseltilmiş ayrıcalığa sahip birden fazla kullanıcı, kimlik sistemi yönetmek için var mı?
  * Yanıt Evet ise, her kullanıcı aynı erişim düzeyini gerekiyor mu?
* Şirketinizin belirli kaynakları yönetmek için kullanıcılara erişim vermek gerekir?
  * Yanıt Evet ise, bu ne sıklıkta olur?
* Şirketiniz şirket içi ve bulut arasında erişim denetim özelliklerini tümleştiren gerek kaynakları?
* Bazı koşullar uygun olarak kaynaklara erişimi sınırlamak, şirketinizin gerekir?
* Şirketiniz bazı kaynaklar özel denetim erişmesi gereken herhangi bir uygulama bulunur?
  * Yanıt Evet ise, burada uygulamalarla bulunur (şirket içi veya bulutta)?
  * Yanıtınız evet ise, burada olan bu hedef kaynaklara (şirket içi veya bulutta)?

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) seçenekleri ve her seçeneğin olumlu/olumsuz üzerinden geçer.  Bu soruyu yanıtlayarak, iş gereksinimlerinize en iyi hangi seçeneği en seçersiniz.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Olay yanıtlama gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

