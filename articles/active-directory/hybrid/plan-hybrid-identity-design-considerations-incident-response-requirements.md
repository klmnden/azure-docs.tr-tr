---
title: Karma kimlik tasarımı - olay yanıtlama gereksinimlerini Azure | Microsoft Docs
description: İzleme ve raporlama özellikleri tarafından kullanılan karma kimlik çözümü belirlemek tanımlamak ve olası tehditleri önlemek için eylemleri için BT
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
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
ms.openlocfilehash: 08bf7950ada5db90d2b8bfea751b39ffc21f3ee9
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65950856"
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümünüz için olay yanıtlama gereksinimlerini belirleme
Büyük ve orta ölçekli kuruluşlar büyük olasılıkla olacaktır bir [güvenlik olayı yanıt](https://technet.microsoft.com/library/cc700825.aspx) yardımcı olmak için BT uygun şekilde eylemleri için olay düzeyi. Kimlik yönetimi sistemi olay yanıt işlemi önemli bir bileşenidir çünkü hedefe karşı belirli bir eylemi gerçekleştiren tanımlamaya yardımcı olmak için kullanılabilir. Karma kimlik çözümü tarafından yararlanılabilir izleme ve raporlama özellikleri sağlayabilir olası tehdidi azaltmak üzere eylemleri mümkün KILAR. Tipik bir olay yanıtlama planında planının bir parçası aşağıdaki aşamaları olacaktır:

1. İlk değerlendirmeyi.
2. Olay iletişimini.
3. Zarar denetimi ve risk azaltma.
4. Kimliği'ne önem ve güvenlik ihlali oluştu.
5. Kanıt korunması.
6. Uygun taraflara bildirimi.
7. Sistem Kurtarma.
8. Belgeleri.
9. Zarar ve maliyet değerlendirmesi.
10. İşlem ve planı gözden geçirme.

BT'nin kimliği sırasında güvenliğinin aşılması ve önem derecesi aşaması, tehlikeye girmiş, erişilen ve bu dosyaların gizlilik düzeyini belirleyen dosya sistemlerini tanımlamak üzere gerekli olacaktır. Karma kimlik sisteminizde yapılan bu değişiklikleri kullanıcı belirlemenize yardımcı olması için bu gereksinimleri karşılamak üzere başlatabilmeniz gerekir. 

## <a name="monitoring-and-reporting"></a>İzleme ve raporlama
Çoğunlukla sistem denetim ve raporlama özellikleri yerleşik olan birden çok kez kimlik sistemi de ilk değerlendirme aşamasında yardımcı olabilir. İlk değerlendirme sırasında BT yöneticisinin bir şüpheli etkinliğini tanımlamanız kullanabilirsiniz veya sistem için otomatik olarak önceden yapılandırılmış bir görev tabanlı olmalıdır. Diğer durumlarda, hatalı yapılandırılmış bir sistem hatalı pozitif uyarıların sayısını için bir yetkisiz erişim algılama sistemi neden olabilir ancak birçok etkinlik olası bir saldırı olduğunu gösteriyor olabilir. 

Kimlik yönetimi sistemi tanımlamak ve bu şüpheli etkinlikleri bildirmek için BT yöneticilerinin yardımcı olmalıdır. Genellikle bu teknik gereksinimleri tüm sistemler izleme ve olası tehditleri vurgulayabilirsiniz raporlama bir özelliği olan yerine. Olay yanıtlama gereksinimlerini göz önünde bulundurarak alırken çalışırken, karma kimlik çözümü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Mu şirketinizin sahip bir güvenlik olayı yanıt yerinde?
  * Yanıt Evet ise, geçerli kimlik yönetimi sistemi işleminin bir parçası kullanılır?
* Şirketinizin farklı cihazlarda kullanıcılardan şüpheli oturum açma denemesi tanımlayın gerekiyor mu?
* Şirketinizin olası güvenliği aşılan kullanıcı kimlik bilgilerini algılamak gerekiyor mu?
* Şirketiniz kullanıcının erişim ve işlem denetim gerekiyor mu?
* Şirketinizin ne zaman bir kullanıcının parolasını sıfırlar bilmeniz gerekiyor mu?

## <a name="policy-enforcement"></a>İlke uygulama
Zarar denetimi ve risk azaltma-aşaması sırasında hızlı bir şekilde gerçek ve potansiyel bir saldırının etkilerini azaltmak önemlidir. Sizi bu eylem, bu noktada arasındaki küçük ve büyük bir fark yapabilirsiniz. Tam tepki, kuruluşunuz ve karşılaştığınız saldırının yapısını bağlıdır. Bir hesap aşılmış ilk değerlendirmeyi adlı yönelik, bu hesap engellemek için ilkeyi uygulamak gerekir. Kimlik yönetimi sistemi yararlanılarak burada yalnızca bir örnektir. Devam eden bir olaya tepki vermek için ilkeleri nasıl zorlanır dikkate alarak, karma kimlik çözümü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Şirketinizin ilkeleri yerinde blok kullanıcılara erişimden ağ gerekirse var mı?
  * Yanıt Evet ise, geçerli çözüm benimsemek için uygulayacağınız karma kimlik yönetimi sistemi ile tümleştirilir?
* Şirketiniz Karantinada olan kullanıcılar için koşullu erişimi zorunlu gerekiyor mu? 

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md) her seçeneğin olumlu/olumsuz değerlendirilir ve seçeneklerin geçilir.  En iyi seçeneği işletmenizi uygun seçersiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

