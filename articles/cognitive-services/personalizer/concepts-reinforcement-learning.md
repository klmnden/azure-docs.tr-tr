---
title: Pekiştirmeye dayalı öğrenme - Personalizer
titleSuffix: Azure Cognitive Services
description: Personalizer daha iyi sıralama önerilerde bulunmak için Eylemler ve geçerli bağlam bilgilerini kullanır. Bu eylemler ve bağlamı hakkında bilgi olan öznitelikler veya özellikler adlandırılan özellikleri.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 30f009f76c25d80281d748e1e484175380ca9743
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65027176"
---
# <a name="what-is-reinforcement-learning"></a>Pekiştirmeye dayalı öğrenme nedir?


Pekiştirmeye dayalı öğrenme, bunların kullanımından geri bildirim alarak davranışlarını öğrenen makine öğrenimine bir yaklaşımdır.
 
Pekiştirmeye dayalı öğrenme ile çalışır:

* Bir fırsat veya serbestlik derecesi kararları veya seçimler yapma gibi davranış - geçireceğini sağlama.
* Seçenek ve ortamı hakkında bağlamsal bilgiler sağlar.
* Ne kadar iyi belirli bir hedefe davranışı elde hakkında geri bildirim sağlama.

Birçok subtypes ve öğrenme pekiştirmeye türlerde olsa bu kavramı Personalizer içinde nasıl çalışır.

* Uygulamanızı içeriğin alternatifleri listesinden bir kısmını görüntülemeyi sağlar.
* Uygulamanızı her alternatif ve kullanıcı bağlamı hakkında bilgi sağlar.
* Uygulama hesaplar bir _puanı ödüllendirin_.

Öğrenme pekiştirmeye bazı yaklaşımları, bir simülasyon çalışacak şekilde Personalizer gerektirmez. Kendi öğrenme algoritmalarını tepki vermek için bir dış dünya için tasarlanmıştır (karşı denetlemesine) ve her bir veri noktasının anlayarak, zamandan ve paradan tasarruf oluşturmak için maliyet benzersiz bir fırsat olup olmadığını ve değilse sıfır olmayan pişman (olası ödül kaybı) olduğunu öğrenin performansın gerçekleşir.

## <a name="what-type-of-reinforcement-learning-algorithms-does-personalizer-use"></a>Hangi türde öğrenimi algoritmaları pekiştirmeye Personalizer kullanıyor mu?

Personalizer geçerli sürümünü kullanan **bağlamsal bandits**, diğer bir deyişle, pekiştirmeye öğrenme bir yaklaşım Çerçeveli kararlar veya belirli bir bağlamda farklı eylemler arasında seçimler yapma geçici bir çözüm.

_Karar bellek_, doğrusal bir model birtakım en iyi olası karar verilen bir bağlamda yakalamak için eğitilen modeli kullanır. Bu iş sonuçları art arda gösterilmesini ve kendini kanıtlamış bir yaklaşım kısmen bunlar gerçek dünyadan çok hızlı bir şekilde çoklu geçiş eğitim gerek kalmadan bilgi edinerek ve kısmen denetimli öğrenme modellerini ve derin sinir tamamlayabilir nedeni Ağ modeller.

Rastgele ayarlamak için keşif yüzdesi aşağıdaki Araştır/yararlanma trafiği ayırma yapılır ve araştırma için varsayılan algoritma epsilon doyumsuz.

### <a name="history-of-contextual-bandits"></a>Bağlamsal Bandits geçmişi

John Langford adı bağlamsal pekiştirmeye dayalı öğrenme tractable kümesini tanımlamak için Bandits (Langford ve Zhang [2007]) de ve geliştirmeye ilişkin bu programlamada bilgi edinmek nasıl bir yaklaşık olarak yarım düzine incelemeler çalıştı:

* Beygelzimer et al. [2011]
* Dudík et al. [2011a,b]
* Agarwal et al. [2014, 2012]
* Beygelzimer ve Langford [2009]
* Li et al. [2010]

John aynı zamanda birkaç öğreticiler daha önce birleşik tahmin (ICML 2015), bağlamsal Bandit teorik (NIPS 2013), etkin öğrenim (ICML 2009) ve örnek karmaşıklığı sınırları (ICML 2003) gibi konularda verdiği

## <a name="what-machine-learning-frameworks-does-personalizer-use"></a>Hangi makine öğrenimi çerçeveleri Personalizer kullanıyor mu?

Şu anda personalizer kullanır [Vowpal Wabbit](https://github.com/VowpalWabbit/vowpal_wabbit/wiki) machine learning için temel olarak. Bu çerçeve, en fazla aktarım hızı ve düşük kişiselleştirme yapmadan sıralayan zaman gecikme süresi ve tüm olayları modeli eğitmek için sağlar.

## <a name="references"></a>Başvurular

* [Düşük teknik borç ile bağlamsal bir karar almadan](https://arxiv.org/abs/1606.03966)
* [Bir adil sınıflandırma indirimleri yaklaşımı](https://arxiv.org/abs/1803.02453)
* [İleti örneği olmayan Dünyaları içinde verimli bağlamsal Bandits](https://arxiv.org/abs/1708.01799ds)
* [Kalan kaybı tahmin: Pekiştirmeye: No ile artımlı geribildirim öğrenme](https://openreview.net/pdf?id=HJNMYceCW)
* [Yönergeler ve görsel gözlemler Eylemler pekiştirmeye dayalı öğrenme ile eşleme](https://arxiv.org/abs/1704.08795)
* [Arama için daha iyi, Öğretmen öğrenme](https://arxiv.org/abs/1502.02206)

## <a name="next-steps"></a>Sonraki adımlar

[Çevrimdışı değerlendirme](concepts-offline-evaluation.md) 