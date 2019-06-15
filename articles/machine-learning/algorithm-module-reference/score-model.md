---
title: 'Puanlama modeli: Modül başvurusu'
titleSuffix: Azure Machine Learning service
description: Model Puanlama modülü eğitilmiş bir sınıflandırma veya regresyon modelini kullanarak Öngörüler oluşturmak için Azure Machine Learning hizmetinde kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: f8f7bfcbbf013f2cf32957772086d7e44d31e310
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65029273"
---
# <a name="score-model-module"></a>Score Model (Model Puanlama) modülü

Bu makalede bir modül, Azure Machine Learning hizmeti için görsel arabirim (Önizleme).

Eğitilmiş bir sınıflandırma veya regresyon modelini kullanarak Öngörüler oluşturmak için bu modülü kullanın.

## <a name="how-to-use"></a>Nasıl kullanılır

1. Ekleme **Score Model** denemenizi modülü.

2. Eğitilen bir modelin ve yeni girdi verilerini içeren bir veri kümesi ekleyin. 

    Veriler, eğitilen modeli kullandığınız türü ile uyumlu bir biçimde olmalıdır. Giriş veri kümesi şemasını ayrıca genellikle modeli eğitmek için kullanılan veri şeması ile eşleşmesi gerekir.

3. Denemeyi çalıştırın.

## <a name="results"></a>Sonuçlar

Puanları kullanarak kümesi oluşturduktan sonra [Score Model](./score-model.md):

+ Ölçümleri modelin doğruluğunu (performans) değerlendirmesi için kullanılan bir dizi oluşturmak için.  puanlanmış veri kümesine bağlanıp [Evaluate Model](./evaluate-model.md), 
+ Modül sağ tıklayıp **Görselleştir** sonuçlarının bir örnek görmek için.
+ Bir veri kümesine sonuçları kaydedin.

Puan veya tahmin edilen değer, model ve girişinizi bağlı olarak birçok farklı biçimlerde olabilir:

- Sınıflandırma modelleri için [Score Model](./score-model.md) sınıfı yanı sıra tahmin edilen değerin olasılığı için tahmin edilen bir değer çıkarır.
- Regresyon modelleri için [Score Model](./score-model.md) yalnızca tahmin edilen sayısal değer oluşturur.
- Görüntü sınıflandırma modellerini için görüntü ya da belirli bir özellik bulundu olup olmadığını gösteren bir Boole değeri nesnenin sınıfını puanı olabilir.

## <a name="publish-scores-as-a-web-service"></a>Puanları bir web hizmeti olarak yayımlama

Puanlama yaygın kullanımı, Tahmine dayalı web hizmeti bir parçası olarak çıkış getirmektir. Daha fazla bilgi için Bu öğreticide bir Azure Machine Learning deneme temel bir web hizmeti oluşturma bakın:


## <a name="next-steps"></a>Sonraki adımlar

Bkz: [kullanılabilir modül kümesini](module-reference.md) Azure Machine Learning hizmetine. 