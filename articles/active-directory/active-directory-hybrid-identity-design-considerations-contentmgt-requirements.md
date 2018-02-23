---
title: "Karma kimlik tasarımı - içerik yönetimi gereksinimlerini Azure | Microsoft Docs"
description: "İşinizin içerik yönetimi gereksinimlerini belirleme hakkında bilgi sağlar. Genellikle bir kullanıcı kendi aygıt olduğunda kendisi de kendisine kullanan uygulamayı göre değişen birden çok kimlik olabilir. Hangi içerik Kurumsal kimlik bilgileri kullanılarak oluşturulan olanları karşı kişisel kimlik bilgileri kullanılarak oluşturulmuş ayırt etmek önemlidir. Kimlik çözümünüzü bulut ile etkileşim sırasında son kullanıcının sorunsuz bir deneyim sağlamak üzere hizmetler kendi gizlilik sağlamak ve veri sızıntısına karşı koruma artırın."
documentationcenter: 
services: active-directory
author: billmath
manager: mtillman
editor: 
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 71e33ec82c3db6fb7efa52dd12315e309658aab9
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/22/2018
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümü için içerik yönetimi gereksinimlerini belirleyin
İşletmeniz için içerik yönetimi gereksinimlerini anlama doğrudan kullanmak için hangi karma kimlik çözümü üzerinde kararınızı etkileyebilir. Birden çok aygıt ve kullanıcılar kendi aygıtlarını getirip yeteneği artışı ile ([KCG](https://aka.ms/byodcg)), şirket kendi verilerini korumanız gerekir, ancak bu ayrıca kullanıcının gizliliğini korumanız gerekir. Genellikle bir kullanıcı kendi aygıt olduğunda kendisi de kendisine kullanan uygulamayı göre değişen birden çok kimlik olabilir. Hangi içerik Kurumsal kimlik bilgileri kullanılarak oluşturulan olanları karşı kişisel kimlik bilgileri kullanılarak oluşturulmuş ayırt etmek önemlidir. Kimlik çözümünüzü bulut ile etkileşim sırasında son kullanıcının sorunsuz bir deneyim sağlamak üzere hizmetler kendi gizlilik sağlamak ve veri sızıntısına karşı koruma artırın. 

Kimlik çözümü, aşağıdaki çizimde gösterildiği gibi içerik yönetimi sağlamak için farklı teknik denetimler tarafından işlevden:

![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Kimlik yönetimi sistemi yararlanarak güvenlik denetimleri**

Genel olarak, içerik yönetimi gereksinimlerini kimlik yönetimi sisteminizi aşağıdaki alanlarda özelliğinden yararlanır:

* Gizlilik: bir kaynağın sahibi kullanıcı tanımlama ve bütünlüğünü sağlamak için uygun denetimleri uygulama.
* Veri Sınıflandırması: kullanıcıyı veya grubu ve kendi sınıflandırmasını göre bir nesne erişim düzeyini belirleyin. 
* Veri sızıntısı koruma: kullanıcının kimliğini doğrulamak için kimlik sistemi ile etkileşim kurmak güvenlik denetimleri veri sızıntısını önlemek için koruma sorumluluğunu gerekir. Bu da izi amacı denetimi için önemlidir.

> [!NOTE]
> Okuma [bulut hazırlık için veri sınıflandırma](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en iyi yöntemler ve veri sınıflandırması için yönergeleri hakkında daha fazla bilgi.
> 
> 

Kullanırken, karma kimlik çözümü planlama emin olun, kuruluşunuzun gereksinimlerine göre aşağıdaki soruları yanıtlanır:

* Şirketinizin güvenlik denetimleri veri gizliliği zorunlu yerinde var mı?
  * Yanıt Evet ise, bu güvenlik denetimleri benimsemek için uygulayacağınız karma kimlik çözümü ile tümleştirmek erişebilecek mi?
* Şirketiniz veri sınıflandırması kullanıyor mu?
  * Yanıt Evet ise, geçerli çözüme benimsemek için uygulayacağınız karma kimlik çözümü ile tümleştirmek mi?
* Şirketiniz veri sızıntısı için herhangi bir çözümü şu anda var mı? 
  * Yanıt Evet ise, geçerli çözüme benimsemek için uygulayacağınız karma kimlik çözümü ile tümleştirmek mi?
* Şirket kaynaklarına erişimi denetleme gerekiyor mu?
  * Yanıt Evet ise, hangi kaynakları tür?
  * Yanıt Evet ise, hangi bilgilerin düzeyini gerekli mi?
  * Yanıt Evet ise, Denetim günlüğü bulunduğu gerekir? Şirket içi veya bulutta?
* Şirketinizin hassas veriler (Ssn'ler, kredi kartı numaraları, vb.) içeren e-postaları şifrelemek gerekiyor mu?
* Şirketiniz dış iş ortaklarıyla paylaşılan tüm belgeler/içerik şifrelemek gerekiyor mu?
* Şirketinizin e-postalar belirli türden şirket ilkelerini zorlamak gerekiyor mu (tüm yanıt yapmak için iletme)?

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) seçenekleri ve her seçeneğin olumlu/olumsuz üzerinden geçer.  Seçeneği, iş gereksinimlerinize en uygun belirleyeceksiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Erişim denetimi gereksinimlerini belirleme](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

