---
title: "Azure Active Directory karma kimlik tasarımı hakkında dikkat edilecek noktalar - belirlemek olay rResponse gereksinimleri | Microsoft Docs"
description: "Tarafından yararlanılabilir karma kimlik çözümü için izleme ve raporlama özelliklerini belirlemek tanımlamak ve olası tehditleri azaltmak amacıyla önlemler almak için BT"
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: a3d2a459-599b-4b67-8e51-7369ee25082d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 536071ec61d093af243bfd42faa6bb404172fb8e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Karma kimlik çözümü için olay yanıtlama gereksinimlerini belirleyin
Büyük veya orta ölçekli kuruluşlarda büyük olasılıkla sahip olacak bir [güvenlik olay yanıtlama](https://technet.microsoft.com/library/cc700825.aspx) yardımcı olmak üzere BT buna göre eylemleri olay düzeyine. Hedefe karşı belirli bir eylemi gerçekleştiren kişiye tanımlayan yardımcı olmak için kullanılabileceğinden kimlik yönetimi sistemi olay yanıtlama işlemindeki önemli bir bileşendir. Karma kimlik çözümü tarafından yararlanılabilir izleme ve raporlama özelliklerini sağlayabilir tanımlamak ve bir olası tehdidi azaltmak amacıyla önlemler almak için BT. Bir genel olay yanıtlama planında planının bir parçası aşağıdaki aşamaları olacaktır:

1. İlk değerlendirmesi.
2. Olay iletişim.
3. Hasar denetim ve risk azaltma.
4. Ne tanımlaması güvenliğinin aşılması ve önem derecesi oluştu.
5. Kanıt koruma.
6. Bildirim uygun taraflara.
7. Sistem Kurtarma.
8. Belgeler.
9. Hasar ve maliyet değerlendirmesi.
10. İşlem ve planı gözden geçirme.

BT'nin tanımlanması sırasında güvenliğinin aşılması ve önem derecesi aşaması, aşılmış, sistemleri erişilen ve bu dosyaları duyarlılığını belirlemek dosyaları belirlemek gerekli olacaktır. Karma kimlik sistemi bu değişiklikleri yapılan kullanıcı belirlemenize yardımcı olması için bu gereksinimleri karşılamak üzere görebilmeniz gerekir. 

## <a name="monitoring-and-reporting"></a>İzleme ve Raporlama
Sistem Denetim ve raporlama özellikleri, çoğunlukla yerleşik olan birçok kez kimlik sistemi ayrıca ilk değerlendirme aşamasında yardımcı olabilir. İlk değerlendirme sırasında BT yöneticisi kuşkulu bir etkinlik tanımlayabilir veya otomatik olarak önceden yapılandırılmış bir görevde dayalı tetikleyici için sistem görebilmeniz gerekir. Diğer durumlarda, hatalı yapılandırılmış bir sistem sayıda hatalı pozitif sonuç bir yetkisiz erişim algılama sisteminde neden olabilir ancak birçok etkinlik olası bir saldırı olduğunu gösteriyor olabilir. 

Kimlik yönetimi sistemi tanımlamak ve kuşkulu etkinlikler Bu rapor için BT yöneticilerine yardımcı olmalıdır. Genellikle bu teknik gereksinimleri tüm sistemler izleme ve olası tehditler vurgulamak bir raporlama özelliği sahip uyması gereken. Olay yanıtlama gereksinimlerini dikkate alırken çalışırken, karma kimlik çözümü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Mu şirketinizin sahip bir güvenlik olay yanıtı yerinde?
  * Yanıt Evet ise, geçerli kimlik yönetimi sistemi işleminin bir parçası kullanılır?
* Şirketinizin farklı cihazlarda kullanıcılardan şüpheli oturum açma denemelerinin tanımlamak istiyor mu?
* Şirketiniz, güvenliği aşılan olası kullanıcının kimlik bilgilerini algılamak mu?
* Şirketiniz kullanıcının erişim ve eylem denetim gerekiyor mu?
* Şirket, kullanıcı parolasını sıfırlama bilmeniz gerekiyor mu?

## <a name="policy-enforcement"></a>İlke zorlama
Hasar denetim ve risk azaltma-aşaması sırasında hızlı bir şekilde bir saldırı gerçek ve olası etkisini azaltmak önemlidir. Sizi yönlendirir o eylemi bu noktada bir küçük ve büyük bir arasındaki farkı yapabilirsiniz. Tam tepki, kuruluşunuz ve karşılaştığınız saldırının yapısını değişir. Bir hesap aşılmış ilk değerlendirme sonuçları, bu hesap engellemek için politikasını gerekecektir. Bu kimlik yönetim sistemi işlevden burada yalnızca bir örnektir. Devam eden bir olaya tepki vermek için ilkeleri nasıl zorlanır göz önünde bulundurarak sırasında karma kimlik çözümü tasarlamanıza yardımcı olması için aşağıdaki soruları kullanın:

* Şirket ilkeleri blok kullanıcılara erişimden ağ gerekirse kullanıyor mu?
  * Yanıt Evet ise, geçerli çözüme benimsemek için uygulayacağınız karma kimlik yönetimi sistemi ile tümleşik çalışıyor mu?
* Şirketiniz Karantinada bulunan kullanıcılar için koşullu erişimi zorunlu gerekiyor mu? 

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) seçenekleri ve her seçeneğin olumlu/olumsuz üzerinden geçer.  Seçeneği, iş gereksinimlerinize en uygun belirleyeceksiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
[Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

