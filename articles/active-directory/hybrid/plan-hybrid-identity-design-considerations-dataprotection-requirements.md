---
title: Karma kimlik tasarımı - veri koruma gereksinimlerini Azure | Microsoft Docs
description: Karma kimlik çözümünüzü planlarken, işletmeniz ve en iyi şekilde bu gereksinimleri karşılamak hangi seçenekler kullanılabilir veri koruma gereksinimlerini belirleyin.
documentationcenter: ''
services: active-directory
author: billmath
manager: daveba
editor: ''
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
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
ms.openlocfilehash: e734b932976896b2fb4d86e2627b7827d40a2545
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60454900"
---
# <a name="plan-for-enhancing-data-security-through-a-strong-identity-solution"></a>Aracılığıyla güçlü kimlik çözümü veri güvenliği iyileştirmeyi planlama
Verilerini korumanın ilk adımı, bu verilere kimlerin erişebileceğini belirlemektir. Ayrıca, kimlik doğrulama ve yetkilendirme özellikleri sağlamak için sisteminizi ile tümleştirebileceğiniz bir kimlik çözümü olması gerekir. Kimlik doğrulama ve Yetkilendirme genellikle birbiriyle karıştırılabilecek ve rollerine yanlış. Gerçekte, aşağıdaki çizimde gösterildiği gibi farklı, bunlar:

![](./media/plan-hybrid-identity-design-considerations/mobile-devicemgt-lifecycle.png)

**Mobil cihaz Yönetimi yaşam döngüsünün aşamaları**

Karma kimlik çözümünüzü planlarken bu gereksinimleri işletmenize ve en iyi hangi seçenekler kullanılabilir veri koruma gereksinimlerini karşılamak anlamanız gerekir.

> [!NOTE]
> Veri güvenliği için planlama tamamladıktan sonra gözden [çok faktörlü kimlik doğrulaması gereklilikleri](plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md) çok faktörlü kimlik doğrulaması gereksinimleri ile ilgili yaptığınız seçimleri, değildi emin olmak için tarafından kararların, etkilenen Bu bölümde yapılan.
> 
> 

## <a name="determine-data-protection-requirements"></a>Veri koruma gereksinimlerini belirleme
Mobility yaşı, çoğu şirketin ortak bir hedefe sahip: üretkenliği artırmak için mobil cihazlarından, şirket içi veya uzaktan her yerde üretken olması kullanıcıları etkinleştirin. Bu tür gereksinimlerin şirketler aynı zamanda şirket verilerinin korunmasına ve kullanıcı gizliliğini korumak için azaltılması gereken tehditleri sayısı hakkında endişe olacaktır. Her şirketin farklı gereksinimleri bu bağlamda olabilir; Şirket hangi sektörden göre hareket değişir farklı uyumluluk kuralları, farklı tasarım kararlarına yol açacaktır. 

Ancak, sektörden bağımsız olarak doğrulanan ve incelediniz gereken bazı güvenlik unsur vardır.

## <a name="data-protection-paths"></a>Veri koruma yolları
![](./media/plan-hybrid-identity-design-considerations/data-protection-paths.png)

**Veri koruma yolları**

Yukarıdaki diyagramda, veri erişmeden önce doğrulanması için birinci kimlik bileşen olacaktır. Ancak, bu veriler farklı durumlarda erişilmiş olan süre boyunca olabilir. Bu diyagramdaki her bir sayının hangi veri zaman içinde belirli bir noktada bulunabilir yolu temsil eder. Bu sayı, aşağıda açıklanmıştır:

1. Cihaz düzeyinde veri koruma.
2. Aktarım sırasında veri koruması.
3. Rest şirket içi sırada, veri koruma.
4. Bulutta sırada bekleyen verilerin korunması.

Karma kimlik çözümü hem şirket içi yararlanarak özellikli olduğunu gereklidir ve bulut Kimlik Yönetimi kaynaklarına erişim izni veri önce kullanıcıyı tanımlamak için. Karma kimlik çözümünüzü planlarken, aşağıdaki soruları kuruluşunuzun gereksinimlerine göre yanıtlandığından emin olun:

## <a name="data-protection-at-rest"></a>Bekleyen verilerin korunması konusunda
Verilerin (cihaz, Bulut veya şirket içi) bekleme durumunda olduğu bağımsız olarak, bir değerlendirme, bu konuda, kuruluşunuzun gereksinimlerini anlamak için önemlidir. Bu alan için aşağıdaki soruları isteniyor emin olun:

* Şirketiniz, bekleyen verileri koruma gerekiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü geçerli şirket içi altyapınızla tümleştirmeyi sağlayabilir mi?
  * Yanıt Evet ise, karma kimlik çözümü buluttaki iş yüklerinizi ile tümleştirebilir mi?
* Bulut Kimlik Yönetimi kullanıcının kimlik bilgilerini ve bulutta depolanan diğer verileri korumaya mi?

## <a name="data-protection-in-transit"></a>Aktarımdaki verileri koruma
Veya cihaz ile bulut cihaz hem de veri merkezi arasında Taşınmakta olan veriler korunmalıdır. Ancak, aktarım sırasında aktarılan mutlaka bulut hizmetinizi dışında bir bileşeni ile bir iletişim işlem gelmez; Ayrıca, dahili olarak hareket arasında iki sanal ağ gibi. Bu alan için aşağıdaki soruları isteniyor emin olun:

* Şirketinizin, Aktarımdaki verileri korumak gerekiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü gibi SSL/TLS güvenli denetimler ile tümleştirebilir mi?
* Bulut kimlik yönetimi ve dizin deposu (içinde ve veri merkezleri arasında) içinde imzalı trafiği tutmak mu?

## <a name="compliance"></a>Uyumluluk
Düzenlemeler, yasalara ve yasal uyumluluk gereksinimlerini şirket ait sektör göre değişir. Şirketler yüksek düzenlenen sektörde uyumluluk sorunlarıyla ilgili kimlik yönetimi endişelere gerekir. Düzenlemelere (SOX) Sarbanes Oxley, sağlık sigortası taşınabilirlik ve Sorumluluk Yasası (HIPAA), gerekli kılan Gramm-Leach-Bliley Yasası (GLBA) ve ödeme kartı sektör veri güvenliği standardı (PCI DSS) gibi kimlik ve erişim ile ilgili katı. Bir veya daha fazla bu yönetmelikler gereksinimlerini karşılar temel yetenekleri, şirketin benimseyeceği karma kimlik çözümü olması gerekir. Bu alan için aşağıdaki soruları isteniyor emin olun:

* Karma kimlik çözümü, işletmeniz için yasal gereksinimlerle uyumlu mu?
* Karma kimlik çözümünü mu 
* özelliklerinde, şirketinizin uyumlu yasal düzenleme gereksinimleri etkinleştirilsin mi? 

> [!NOTE]
> Her yanıtı Not ve yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](plan-hybrid-identity-design-considerations-data-protection-strategy.md) her seçeneğin olumlu/olumsuz değerlendirilir ve seçeneklerin geçilir.  En iyi seçeneği işletmenizi uygun seçersiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
 [İçerik Yönetimi gereksinimlerini belirleme](plan-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](plan-hybrid-identity-design-considerations-overview.md)

