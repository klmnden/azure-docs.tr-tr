---
title: Etkin öğrenme - Personalizer
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: edjez
ms.openlocfilehash: c44afc81a7ec9d05cdc04cc8bc46c77cd51ceaf8
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722518"
---
# <a name="active-learning-and-learning-policies"></a>Etkin olarak öğrenmeye ve ilkeleri öğrenme 

Uygulamanız sıralama API çağırdığında, içeriği derecesini alırsınız. İş mantığı bu boyut, içerik kullanıcıya görünen kaldırılması gerekip gerekmediğini belirlemek için kullanabilirsiniz. Görüntülediğinizde dereceli içeriği, diğer bir deyişle bir _etkin_ derece olay. Uygulamanızı değil görüntülediğinizde, içerik, yani sıralanmış bir _etkin olmayan_ derece olay. 

Etkin derece olay bilgileri için Personalizer döndürülür. Bu bilgiler, geçerli öğrenme İlkesi aracılığıyla modeli eğitmek devam etmek için kullanılır.

## <a name="active-events"></a>Etkin olaylar

Etkin olaylar her zaman kullanıcıya gösterilen ve ödül çağrı öğrenme döngüyü yönlendirileceksiniz. 

### <a name="inactive-events"></a>Etkin olmayan olaylar 

Kullanıcı dereceli içeriğinden seçmek için bir fırsat sağlanmamış olduğundan etkin olmayan olayları temel alınan modelin değiştirmeniz gerekmez.

## <a name="dont-train-with-inactive-rank-events"></a>Etkin olmayan sıralama olaylarla eğitme yok 

Bazı uygulamalarda, uygulamanızın sonuçları kullanıcıya görüntülenecekse henüz bilmeden derece API'ye çağrı gerekebilir. 

Böyle olduğunda:

* Bazı kullanıcı Arabirimi kullanıcı görmek alamayabilir veya önceden işleme. 
* Uygulamanızı, daha az gerçek zamanlı bağlamı ile yapılan derece çağrıları ve çıktılarını olabilir veya uygulama tarafından kullanılamayabilir Tahmine dayalı kişiselleştirme yapılıyor. 

### <a name="disable-active-learning-for-inactive-rank-events-during-rank-call"></a>Etkin olmayan bir derecelendirme olaylar için etkin olarak öğrenmeye derece çağrısı sırasında devre dışı bırak

Otomatik öğrenme devre dışı bırakmak için derecesi ile çağrı `learningEnabled = False`.

Boyut için bir ödül gönderirseniz öğrenme etkin olmayan bir olay için örtük olarak etkinleştirilir.

## <a name="learning-policies"></a>İlkeleri öğrenme

İlke öğrenme belirler belirli *hiperparametreleri* model eğitimini. Farklı öğrenme ilkeleri, eğitim aynı verilerin iki modeli farklı davranır.

### <a name="importing-and-exporting-learning-policies"></a>İçeri ve dışarı aktarma öğrenme ilkeleri

İçeri ve dışarı aktarma öğrenme Azure portalından ilke dosyaları. Bu, mevcut ilkeleri kaydedin, bunları test etmeniz, bunları değiştirmek ve gelecekte başvurmak ve denetim için yapıt olarak, kaynak kodu denetiminde arşivleyin sağlar.

### <a name="learning-policy-settings"></a>Öğrenme ilke ayarları

Ayarlarında **öğrenme ilke** değiştirilmesi amaçlanmayan. Yalnızca bunların Personalizer nasıl etkilediği anladığınızda ayarlarını değiştirin. Bu bir bilgi olmadan ayarlarını değiştirme Personalizer modelleri geçersiz kılmalarını da dahil olmak üzere, yan etkileri neden olur.

### <a name="comparing-effectiveness-of-learning-policies"></a>İlkeleri öğrenme açısından karşılaştırma

Farklı öğrenme ilkeleri karşılaştırabilirsiniz Personalizer günlüklerinde geçmiş verilere karşı yaparak yapacağı [çevrimdışı değerlendirmeleri](concepts-offline-evaluation.md).

[Kendi öğrenme ilkelerinizi karşıya](how-to-offline-evaluation.md) geçerli öğrenme ilke ile karşılaştırmak için.

### <a name="discovery-of-optimized-learning-policies"></a>En iyi duruma getirilmiş öğrenme ilkeleri bulma

Personalizer, daha en iyi duruma getirilmiş bir öğrenme ilkesi oluşturabilir, yaparken bir [çevrimdışı değerlendirme](how-to-offline-evaluation.md). Çevrimdışı bir değerlendirme daha iyi ödüller için gösterilen daha en iyi duruma getirilmiş bir öğrenme ilke çevrimiçi Personalizer içinde kullanıldığında daha iyi sonuçlar verecek.

En iyi duruma getirilmiş öğrenme ilke oluşturulduktan sonra bunu doğrudan Personalizer için geçerli ilkeyi hemen değiştirir veya daha fazla değerlendirme için Kaydet ve gelecekte AT, kaydedin ve daha sonra uygulama karar uygulayabilirsiniz.