---
title: Çevrimdışı değerlendirme - Personalizer
titleSuffix: Azure Cognitive Services
description: Bu geri bildirim döngüsü oluşturma C# Personalizer Service hızlı başlangıç.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 3fdedd1af9b683b221dfa4aebad7a30559b7abff
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722498"
---
# <a name="offline-evaluation"></a>Çevrimdışı değerlendirme

Çevrimdışı değerlendirme kodunuzu değiştirmek veya kullanıcı deneyimi etkileyen Personalizer hizmet verimliliğini değerlendirmek ve test etmenize olanak tanıyan bir yöntemdir. Çevrimdışı değerlendirme nasıl farklı sıralamalara sahip karşılaştırmak için uygulamanızdan derece API'sine gönderilen veriler, gerçekleştirdiğiniz geçmiş kullanır.

Çevrimdışı değerlendirme, bir tarih aralığı gerçekleştirilir. Aralık geçerli saat kadar geç tamamlayabilir. Aralık başlangıcı için belirtilen gün sayısından daha fazla olamaz [veri saklama](how-to-settings.md).

Çevrimdışı değerlendirme aşağıdaki soruları yanıtlamanıza yardımcı olabilir:

* Ne kadar verimli başarılı kişiselleştirme için Personalizer sıralamalara sahip misiniz?
    * İlke Personalizer çevrimiçi makine öğrenimi tarafından elde edilen ortalama ödül nelerdir?
    * Nasıl Personalizer ne uygulama varsayılan olarak Bitti sahip verimliliğini arasındaki fark nedir?
    * Kişiselleştirme rastgele seçim comparative verimliliğini ne olurdu?
    * El ile belirtilen farklı öğrenme ilkeleri comparative verimliliğini ne olurdu?
* Başarılı kişiselleştirme içeriğinin hangi özelliklerin daha az veya katkıda bulunan?
* Hangi eylemleri özelliklerinin daha az veya başarılı kişiselleştirme katkıda bulunuyorsanız?

Ayrıca, çevrimdışı değerlendirme bulmak için daha Personalizer gelecekte sonuçlarını geliştirmek için kullanabileceğiniz ilke öğrenimi için iyileştirilmiş kullanılabilir.

Çevrimdışı değerlendirmeleri araştırma için kullanılacak olaylarının yüzdesini dair yönergeler sağlamaz.

## <a name="prerequisites-for-offline-evaluation"></a>Çevrimdışı değerlendirmesi için önkoşulları

Temsili çevrimdışı değerlendirme için önemli noktalar şunlardır:

* Yeterli veri var. Önerilen en az 50. 000'en az olayları ' dir.
* Temsili kullanıcı davranışı ve trafiğe sahip dönemlere ait verileri toplayın.

## <a name="discovering-the-optimized-learning-policy"></a>En iyi duruma getirilmiş öğrenme ilkeyi bulma

Personalizer çevrimdışı değerlendirme işlemi, daha iyi bir öğrenme ilke otomatik olarak bulmak üzere kullanabilirsiniz.

Çevrimdışı değerlendirme gerçekleştirdikten sonra geçerli çevrimiçi ilkesiyle karşılaştırıldığında bu yeni ilkeye Personalizer comparative etkinliğini görebilirsiniz. Hemen Personalizer içinde etkin hale getirmek için bu öğrenme ilkeyi uygulamak veya ileride çözümlenmek veya kullanım için indirin.

## <a name="understanding-the-relevance-of-offline-evaluation-results"></a>İlgi düzeyini çevrimdışı değerlendirme sonuçlarını anlama

Çevrimdışı bir değerlendirme çalıştırdığınızda, analiz etmek çok önemli olduğu _güven sınırları_ sonuçları. Geniş olmaları durumunda uygulamanızın ödül tahminleri kesin veya önemli için yeterli veri almadı anlamına gelir. Daha fazla veri sistemde toplanır ve çevrimdışı değerlendirmeleri uzun süreler boyunca çalıştırın güvenle aralıkları dar haline gelir.

## <a name="how-offline-evaluations-are-done"></a>Değerlendirmeleri nasıl çevrimdışı yapılır

Çevrimdışı değerlendirmeleri yapılır bir yöntem kullanılarak çağrılacak **Counterfactual değerlendirme**. 

Personalizer varsayımına yerleşik kullanıcı davranışı (ve bu nedenle ödül) retrospectively tahmin mümkün olduğunu (Personalizer olamaz biliyor olması gereken kullanıcıya ne oldukları gördünüz mü daha farklı bir gösterilmiştir, oldu) ve yalnızca öğrenin ölçülen ödül. 

Bu, değerlendirme için kullanılan kavramsal işlemdir:

```
[For a given _learning policy), such as the online learning policy, uploaded learning policies, or optimized candidate policies]:
{
    Initialize a virtual instance of Personalizer with that policy and a blank model;

    [For every chronological event in the logs]
    {
        - Perform a Rank call
    
        - Compare the reward of the results against the logged user behavior.
            - If they match, train the model on the observed reward in the logs.
            - If they don't match, then what the user would have done is unknown, so the event is discarded and not used for training or measurement.
        
    }

    Add up the rewards and statistics that were predicted, do some aggregation to aid visualizations, and save the results.
}
```

Çevrimdışı değerlendirme yalnızca gözlemlenen kullanıcı davranışı kullanır. Özellikle uygulama çok sayıda eylem çağrıları Rank, bu işlem, büyük hacimli verileri, atar.


## <a name="evaluation-of-features"></a>Değerlendirme özellikleri

Eylemler veya bağlam kolaylığı karşılaştırması için daha yüksek ödüller için çevrimdışı değerlendirmeleri ne kadar belirli özellikleri hakkında bilgi sağlar. Bilgiler, verilere ve belirli bir sürede karşı değerlendirme kullanılarak hesaplanır ve zaman değişebilir.

Arama özelliği değerlendirmeleri ve isteyen öneririz:

* Hangi diğer, ek özellikler, daha etkili olan satırlar boyunca uygulama ya da sistemin sağlayabilir?
* Hangi özellikler nedeniyle düşük verimliliğini Kaldırılabilir? Düşük verimliliğini özellikler eklemek _gürültü_ machine Learning.
* Yanlışlıkla dahil olan herhangi bir özelliği var mı? Bu örnekler: kişisel olarak tanımlanabilir bilgileri (PII) yinelenen kimlikleri, vs.
* Yasal veya sorumlu nedeniyle kişiselleştirmek için kullanılmaması herhangi bir istenmeyen özellik konularını kullanın var mı? Proxy verebilir özellikleri vardır (diğer bir deyişle, yakından yansıtma veya ile ilişkilendirmek) istenmeyen özellikleri?


## <a name="next-steps"></a>Sonraki adımlar

[Personalizer yapılandırın](how-to-settings.md)
