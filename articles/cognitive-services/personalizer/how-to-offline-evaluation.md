---
title: Çevrimdışı değerlendirme
titleSuffix: Personalizer - Azure Cognitive Services
description: Çevrimdışı bir değerlendirme ile öğrenme Döngüsü Analiz etmeyi öğrenin
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: e99a8242e438ef5a8ab7fd917724450f8080bb41
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027064"
---
# <a name="how-to-analyze-your-learning-loop-with-an-offline-evaluation"></a>Çevrimdışı bir değerlendirme ile öğrenme döngüsü analiz etme


Çevrimdışı bir değerlendirmesini tamamlamak ve sonuçları anlama hakkında bilgi edinin.

Çevrimdışı değerlendirmelere izin ver ölçmek için uygulamanızın varsayılan davranışı için etkili Personalizer karşılaştırılır nasıl hangi özellikler kişiselleştirmeyi en katkıda bulunuyorsanız öğrenin ve yeni machine learning ayarları keşfedin otomatik olarak.

Hakkında bilgi edinin [çevrimdışı değerlendirmeleri](concepts-offline-evaluation.md) daha fazla bilgi için.


## <a name="prerequisites"></a>Önkoşullar

1. Yapılandırılmış bir Personalizer döngü olmalıdır
1. Personalizer döngü 50. 000'en az olayları, anlamlı değerlendirme sonuçlarını günlüklerinde olması gerekir.

İsteğe bağlı olarak, ayrıca daha önce dışarı aktardığı _ilke öğrenme_ karşılaştırın ve test aynı veriyi değerlendirmede dosyaları.

## <a name="steps-to-start-a-new-offline-evaluation"></a>Yeni bir çevrimdışı değerlendirmesi başlamak için adımları

1. Azure portalında kişiselleştirme döngü kaynağınızı bulun.
1. "Değerlendirme" bölümüne gidin.
1. Yeni değerlendirme üzerinde tıklayın
1. Çevrimdışı değerlendirme için bir başlangıç ve bitiş tarihi seçin. Bu, veriyi değerlendirmede kullanılacak veri aralığını belirtin geçmişteki tarihler vardır. Bu veri günlükleri belirtildiği gibi mevcut olmalıdır [veri saklama](how-to-settings.md) ayarı.
1. İsteğe bağlı olarak, kendi öğrenme ilke karşıya yükleyebilirsiniz. 
1. Belirtin. Bu süre içinde gözlemlenen kullanıcı davranışa göre en iyi duruma getirilmiş bir öğrenme İlkesi Personalizer oluşturmanız gerekir.
1. Değerlendirmeyi Başlat

## <a name="results"></a>Sonuçlar

Değerlendirmeleri çalıştırılacak karşılaştırmak için ilkeleri öğrenme sayısı İşlenecek veri miktarına bağlı olarak uzun zaman alabilir ve iyileştirme olup istendi.

Tamamlandığında, aşağıdaki sonuçları görebilirsiniz:

1. Karşılaştırmalar Learning dahil olmak üzere ilkelerin:
    * **Online İlkesi**: Geçerli öğrenme ilke Personalizer içinde kullanılır.
    * **Taban çizgisi**: Uygulamanın varsayılan (derece çağrılarında gönderilen ilk eylemi tarafından belirlendiği)
    * **Rastgele ilke**: Sanal bir sıralama davranışını, her zaman sağlanan olanlardan eylemlerin rastgele seçim döndürür.
    * **Özel ilkeler**: Ek öğrenme ilkeler, değerlendirme başlatırken karşıya yüklendi.
    * **İlke en iyi duruma getirilmiş**: Değerlendirme en iyi duruma getirilmiş bir ilke Bul seçeneği ile başladıysanız, ayrıca Karşılaştırılacak ve, geçerli bir değiştirme indirin veya çevrimiçi öğrenme ilke yapmak mümkün olacaktır.

1. Verimliliğini [özellikleri](concepts-features.md) Eylemler, bağlam.


## <a name="more-information"></a>Daha Fazla Bilgi

* Bilgi [nasıl çevrimdışı Değerlendirme çalışma](concepts-offline-evaluation.md).