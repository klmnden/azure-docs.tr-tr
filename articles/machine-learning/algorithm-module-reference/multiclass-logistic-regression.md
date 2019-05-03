---
title: 'Çok sınıflı Lojistik regresyon: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Birden çok değer tahmin etmek için kullanılan bir Lojistik regresyon modeli oluşturmak için Azure Machine Learning hizmetinde veya çoklu sınıflar Lojistik regresyon modülünü kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ac4310e851808d6e6d89d1a2b506975eea3b1d69
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029333"
---
# <a name="multiclass-logistic-regression-module"></a>Çok sınıflı Lojistik regresyon Modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Birden çok değer tahmin etmek için kullanılan bir Lojistik regresyon modeli oluşturmak için bu modülü kullanın.

Sınıflandırma Lojistik regresyon kullanarak, denetimli öğrenme yöntemidir ve bu nedenle etiketlenmiş bir veri kümesi gerektirir. Model ve etiketli veri kümesini girdi bir modül olarak gibi sağlayarak modeli eğitmek [modeli eğitme](./train-model.md). Eğitim modeli, ardından yeni giriş örnekleri için değerleri tahmin etmek için kullanılabilir.

Azure Machine Learning de sağlayan bir [iki sınıflı Lojistik regresyon](./two-class-logistic-regression.md) modülü, ikili veya dichotomous değişkenlerin sınıflandırma için uygundur.

## <a name="about-multiclass-logistic-regression"></a>Çok sınıflı Lojistik regresyon hakkında

Lojistik regresyon sonucu olasılığını tahmin etmek için kullanılır ve sınıflandırma görevleri için popülerdir istatistikleri iyi bilinen bir yöntemidir. Algoritma, veri Lojistik işlevine sığdırma tarafından bir olayın oluşum olasılığını tahmin eder. 

Sınıflandırıcı çok sınıflı Lojistik regresyon içinde birden çok sonuçları tahmin etmek için kullanılabilir.

## <a name="configure-a-multiclass-logistic-regression"></a>Bir çok sınıflı Lojistik regresyon yapılandırın

1. Ekleme **veya çoklu sınıflar Lojistik regresyon** modülünü deneme.

2. Model, ayarlayarak düşünürler nasıl istediğinizi belirtmek **Oluştur trainer modu** seçeneği.

    + **Tek bir parametre**: Nasıl model yapılandırın ve bağımsız değişkenler olarak, belirli bir dizi sağlamak istediğinizi biliyorsanız, bu seçeneği kullanın.

    + **Parametre aralık**: En iyi parametreleri emin değilseniz ve bir parametre tarama kullanmak istiyorsanız bu seçeneği kullanın.

3. **En iyi duruma getirme dayanıklılık**, iyileştirici yakınsama için eşik değerini belirtin. Yinelemeleri arasındaki geliştirme eşik değerinden düşük ise, algoritma durdurur ve geçerli model döndürür.

4. **L1 Kurallaştırma ağırlık**, **L2 Kurallaştırma ağırlık**: L1 ve L2 Kurallaştırma parametrelerini kullanmak için bir değer yazın. Sıfır olmayan bir değer, her ikisi için önerilir.

    Kurallaştırma aşırı katsayısı değerlerle penalizing modelleri tarafından overfitting engelleyen bir yöntemdir. Kurallaştırma varsayım hata katsayısı değerlerle ilişkili cezası ekleyerek çalışır. Daha doğru bir model aşırı katsayısı değerlerle ceza, ancak daha az doğru bir modeli daha pasif değerlerle daha az ceza.

     L1 ve L2 Kurallaştırma farklı etkileri ve kullanır. Yüksek boyutlu verilerle çalışırken kullanışlı, L1 seyrek modelleri için uygulanabilir. Buna karşılık, L2 Kurallaştırma seyrek olmayan veriler için tercih edilir.  Bu algoritma L1 ve L2 Kurallaştırma değerleri doğrusal birleşimi destekler: diğer bir deyişle, `x = L1` ve `y = L2`, `ax + by = c` doğrusal aralık düzenleme koşullarını tanımlar.

     L1 ve L2 koşulları farklı doğrusal birleşimlerini çıkabilecek Lojistik regresyon modeli için gibi [esnek net Kurallaştırma](https://wikipedia.org/wiki/Elastic_net_regularization).

6. **Rastgele sayı kaynağı**: Sonuçları üzerinde çalışır tekrarlanabilir olmasını istiyorsanız algoritması çekirdek kullanmak üzere bir tamsayı değeri yazın. Aksi takdirde, bir sistem saati değeri aynı deneyde çalıştırmalarında biraz farklı sonuçlar üretebilir kaynağı kullanılır.

8. Etiketlenmiş bir veri kümesi ve eğitme modüllerinden birini bağlanın:

    + Ayarlarsanız **Oluştur trainer modu** için **tek parametre**, kullanın [modeli eğitme](./train-model.md) modülü.

9. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Alıştırma tamamlandıktan sonra modelin parametreler, eğitimleri öğrenilen özellik ağırlıkları özetini görmek, çıktısını sağ [modeli eğitme](./train-model.md) modülü ve select **Görselleştir**.


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 