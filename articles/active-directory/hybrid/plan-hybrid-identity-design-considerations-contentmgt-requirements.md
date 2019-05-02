---
title: Karma kimlik tasarımı - içerik yönetimi gereksinimlerini Azure | Microsoft Docs
description: İşletmenizin içerik yönetimi gereksinimlerini belirlemek nasıl Öngörüler sağlar. Bir kullanıcı kendi cihazını olduğunda, genellikle, bunlar da, kullandıkları uygulama göre değişen birden çok kimlik bilgilerini olabilir. Kurumsal kimlik bilgilerini kullanarak oluşturulan ilkeleri ve kişisel kimlik bilgilerini kullanarak hangi içerik oluşturuldu ayırt etmek önemlidir. Kimlik çözümünüz bulut ile etkileşimde bulunmayı sırasında son kullanıcı için sorunsuz bir deneyim sağlamak için hizmetler kendi gizliliklerini sağlamak ve veri sızıntısına karşı koruma artırın.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: dd1ef776-db4d-4ab8-9761-2adaa5a4f004
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/29/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d970fd133f8c43319e7f1fdb6b3a50c3c05f687
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64918437"
---
# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için bir içerik yönetim gereksinimlerini belirleme
İşletmeniz için içerik yönetimi gereksinimlerini anlamaya kararınız üzerinde kullanmak için hangi karma kimlik çözümü doğrudan etkileyebilir. Birden çok cihaz ve kullanıcıların kendi cihazlarını getirmesine yeteneğini işyerinde ile ([KCG](https://aka.ms/byodcg)), şirketin kendi verilerini korumanız gerekir ancak bu aynı zamanda kullanıcı gizliliğini korumanız gerekir. Bir kullanıcı kendi cihazını olduğunda, genellikle, bunlar da, kullandıkları uygulama göre değişen birden çok kimlik bilgilerini olabilir. Kurumsal kimlik bilgilerini kullanarak oluşturulan ilkeleri ve kişisel kimlik bilgilerini kullanarak hangi içerik oluşturuldu ayırt etmek önemlidir. Kimlik çözümünüz bulut ile etkileşimde bulunmayı sırasında son kullanıcı için sorunsuz bir deneyim sağlamak için hizmetler kendi gizliliklerini sağlamak ve veri sızıntısına karşı koruma artırın. 

Kimlik çözümünüzü farklı teknik denetimler tarafından içerik yönetimi, aşağıdaki çizimde gösterildiği gibi sağlamak için kullanmalarını kullanılabilir:

![Güvenlik denetimleri](./media/plan-hybrid-identity-design-considerations/securitycontrols.png)

**Kimlik Yönetimi sisteminizde yararlanarak güvenlik denetimleri**

Genel olarak, içerik yönetimi gereksinimlerini Kimlik Yönetimi sisteminizde aşağıdaki alanlarda özelliğinden yararlanır:

* Gizlilik: bir kaynağa sahip kullanıcı tanımlama ve uygulama bütünlüğünü korumak için uygun denetimler.
* Veri Sınıflandırması: kullanıcı veya grup ve kendi sınıflandırmasını göre bir nesneye erişim düzeyini belirleyin. 
* Veri sızıntısı koruma: güvenlik denetimleri veri sızıntısını önlemek için korumak için sorumlu kullanıcının kimliğini doğrulamak için kimlik sistemi ile etkileşim gerekecektir. Bu da izleme amaçlı denetimi için önemlidir.

> [!NOTE]
> Okuma [bulut hazırlığı için veri sınıflandırması](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en iyi uygulamalar ve veri sınıflandırması için yönergeler hakkında daha fazla bilgi.
> 
> 

Ne zaman, karma kimlik çözümü planlama olun, kuruluşunuzun gereksinimlerine göre aşağıdaki soruların yanıtlanmasını:

* Şirketinizin güvenlik denetimleri, veri gizliliği zorunlu yerinde var mı?
  * Yanıt Evet ise, güvenlik denetimleri benimsemek için oluşturacağınız karma kimlik çözümü ile tümleştirebilir olacak mı?
* Şirketiniz veri sınıflandırması kullanıyor mu?
  * Yanıt Evet ise, geçerli çözüm benimsemek için oluşturacağınız karma kimlik çözümü ile tümleştirebilir mi?
* Şirketiniz veri sızıntısını için herhangi bir çözümü şu anda var mı? 
  * Yanıt Evet ise, geçerli çözüm benimsemek için oluşturacağınız karma kimlik çözümü ile tümleştirebilir mi?
* Şirket kaynaklarına erişimi denetlemek gerekiyor mu?
  * Evet ise, hangi kaynakların tür?
  * Yanıt Evet ise, hangi düzeyde bilgi gerekli mi?
  * Yanıt Evet ise, Denetim günlüğü bulunduğu gerekir? Şirket içi veya bulutta?
* Şirketinizin hassas verileri (Ssn'ler, kredi kartı numaraları, vs.) içeren e-postaları şifrelemek gerekiyor mu?
* Şirketinizin, dış iş ortakları ile paylaşılan tüm belgeleri/contents şifrelemek gerekiyor mu?
* Şirketinizin belirli türden bir e-postaları şirket ilkeleri zorunlu tutmanıza gerek yoktur (tüm yanıt yapın, iletme)?

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md) her seçeneğin olumlu/olumsuz değerlendirilir ve seçeneklerin geçilir.  En iyi seçeneği işletmenizi uygun seçersiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Erişim denetimi gereksinimleri belirleme](plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

