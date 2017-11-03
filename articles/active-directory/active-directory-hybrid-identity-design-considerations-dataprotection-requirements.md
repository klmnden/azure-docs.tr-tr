---
title: "Azure Active Directory karma kimlik tasarımı hakkında dikkat edilecek noktalar - veri koruma gereksinimlerini belirleme | Microsoft Docs"
description: "Karma kimlik çözümü planlarken, işinizi ve hangi seçeneklerin en iyi bu gereksinimleri karşılamak kullanılabilir olan veri koruma gereksinimlerini belirleyin."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: 40dc4baa-fe82-4ab6-a3e4-f36fa9dcd0df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 96bf9d4c26a22f718c29804c11681199e775f589
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Güçlü kimlik çözümü ile veri güvenliği iyileştirmeyi planlama
Verileri korumak için ilk bu verilere kimlerin erişebileceğini belirlemek ve bir kimliğe sahip gerek bu işlemin bir parçası olarak kimlik doğrulaması ve yetkilendirme özellikleri sağlamak üzere sisteminizle için çözüm tümleştirir adımdır. Kimlik doğrulama ve Yetkilendirme genellikle birbiriyle yanıltıcı olmaktadır ve rollerine yanlış. Gerçekte aşağıdaki çizimde gösterildiği gibi oldukça farklı bunlar:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)

**Mobil cihaz Yönetimi yaşam döngüsünün aşamaları**

Karma kimlik çözümü planlarken hangi seçeneklerin en iyi bu gereksinimleri karşılamak kullanılabilir olduğunu ve işinizin için veri koruma gereksinimlerini anlamanız gerekir.

> [!NOTE]
> Veri güvenliği için planlama tamamladıktan sonra gözden [çok faktörlü kimlik doğrulama gereksinimlerini belirlemek](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) seçimlerinizi çok faktörlü kimlik doğrulama gereksinimleri ile ilgili olmadığını emin olmak için tarafından kararlar, etkilenen Bu bölümde yapılan.
> 
> 

## <a name="determine-data-protection-requirements"></a>Veri koruma gereksinimlerini belirleme
Mobility yaşı çoğu şirketin ortak bir hedefe sahip: üretkenliği artırmak için herhangi bir yerden kullanıcıların mobil cihazlarda şirket içi veya uzaktan üretken olmak, kullanıcılar etkinleştirin. Bu ortak bir hedefe olabilir ancak böyle bir gereklilik kullanan şirket aynı zamanda şirket verilerinin güvenliğini sağlayabilir ve kullanıcı gizliliğini için azaltılması gereken tehditleri miktarı ile ilgili sorun olacaktır. Her şirketin farklı gereksinimleri bu bağlamda olabilir; hangi endüstri göre şirket hareket değişir farklı uyumluluk kuralları, farklı tasarım kararlarına yol gösterecektir. 

Ancak, sonraki bölümde açıklanacak, sektörden bağımsız doğrulandı ve incelediniz bazı güvenlik konuları vardır.

## <a name="data-protection-paths"></a>Veri koruma yolları
![](./media/hybrid-id-design-considerations/data-protection-paths.png)

**Veri koruma yolları**

Yukarıdaki diyagramda kimlik bileşen veri erişmeden önce doğrulanması için birinci olacaktır. Ancak, bu veriler farklı durumlarda erişilmiş olan süre boyunca olabilir. Bu diyagramdaki her sayı, veri zamanında bir noktada bulunabilir yolu temsil eder. Bu sayı, aşağıda açıklanmıştır:

1. Cihaz düzeyinde veri koruma.
2. Aktarım sırasında veri koruması.
3. Rest şirket içi karşın, veri koruma.
4. Veri koruma beklerken bulutta.

Teknik yüklenmesini denetimleri rağmen bu aşamalar her biri veri koruma sağlama doğrudan sunulmaz karma kimlik çözümü tarafından karma kimlik çözümü şirket içi ve bulut yararlanarak kapasitesine sahip olduğunu gerekli olduğu önce kullanıcıyı tanımlamak için Kimlik Yönetimi kaynak verilere erişim verin. Kullanırken, karma kimlik çözümü planlama emin olun, kuruluşunuzun gereksinimlerine göre aşağıdaki soruları yanıtlanır:

## <a name="data-protection-at-rest"></a>Bekleyen verilerin korunması konusunda
Verileri bekleyen (cihaz, Bulut veya şirket içi) nerede bağımsız olarak, bu konuda, kuruluşunuzun gereksinimlerini anlamak için bir değerlendirme yapmak önemlidir. Bu alan için aşağıdaki soruları sorulur emin olun:

* Şirketiniz, kalan verileri korumak istiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü geçerli şirket içi altyapınızı ile tümleştirmek mi?
  * Yanıt Evet ise, karma kimlik çözümü buluttaki, iş yükleri ile tümleştirmek mi?
* Kullanıcının kimlik bilgilerini ve bulutta depolanan diğer verileri korumak bulut Kimlik Yönetimi mi?

## <a name="data-protection-in-transit"></a>Aktarımdaki verileri koruma
Cihaz ve bulut arasında veya cihaz ve veri merkezi arasında Aktarımdaki verileri korunması gerekir. Ancak, aktarım sırasında olan mutlaka bulut hizmetiniz dışında bir bileşen olan bir iletişim işlem gelmez; Ayrıca, dahili olarak, taşır iki sanal ağ arasında gibi. Bu alan için aşağıdaki soruları sorulur emin olun:

* Şirketiniz Aktarımdaki verileri korumak istiyor mu?
  * Yanıt Evet ise, karma kimlik çözümü gibi SSL/TLS güvenli denetimleri ile tümleştirmek mi?
* Bulut kimlik yönetimi ve dizin deposu (içinde ve veri merkezleri arasında) içinde imzalı trafiği tutmak mu?

## <a name="compliance"></a>Uyumluluk
Düzenlemeleri, yasa ve yönetmeliklere uygunluğu gereksinimleri şirket ait endüstri göre değişir. Yüksek düzenlenen sektörlerde şirketlerde uyumluluk sorunlarıyla ilgili kimlik yönetimi endişelere gerekir. Sarbanes-Oxley (SOX), sağlık sigortası taşınabilirlik ve Sorumluluk Yasası (HIPAA), Gramm Leach Bliley Act (GLBA) ve ödeme kartı sektör veri güvenliği standardı (PCI DSS) gibi düzenlemeler, kimlik ve erişim ile ilgili çok sıkı. Bir veya daha fazla bu yönetmelikler gereksinimlerini karşılar çekirdek özellikler, şirketin benimseyeceği karma kimlik çözümü olması gerekir. Bu alan için aşağıdaki soruları sorulur emin olun:

* Karma kimlik çözümü işletmeniz için yasal gereksinimleriyle uyumlu mu?
* Mu karma kimlik çözümü olan yerleşik uyumlu Mevzuat gereklilikleri olması, şirketinizin sağlayacak özellikleri? 

> [!NOTE]
> Her yanıtı not alın ve yanıtın yanıtın arkasındaki mantığı anladığınızdan emin olun. [Veri koruma stratejisini tanımlayın](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) seçenekleri ve her seçeneğin olumlu/olumsuz üzerinden geçer.  Seçeneği, iş gereksinimlerinize en uygun belirleyeceksiniz bu soruları yanıtladığınızda gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
 [İçerik Yönetimi gereksinimlerini belirleyin](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)

## <a name="see-also"></a>Ayrıca Bkz.
[Tasarım konularına genel bakış](active-directory-hybrid-identity-design-considerations-overview.md)

