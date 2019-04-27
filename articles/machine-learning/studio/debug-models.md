---
title: Modelinizi hata ayıklama
titleSuffix: Azure Machine Learning Studio
description: Azure Machine Learning Studio'da modeli eğitme ve Score Model modülleri tarafından oluşturulan hataları ayıklamak nasıl.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 03/14/2017
ms.openlocfilehash: 9c505262030e5b5aa13b8d221cf1e39c4a9c7833
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60751136"
---
# <a name="debug-your-model-in-azure-machine-learning-studio"></a>Azure Machine Learning Studio'da modelinizi hata ayıklama

Bir model çalıştırırken aşağıdaki hatalarla karşılaşırsanız çalıştırabilirsiniz:

* [modeli eğitme] [ train-model] modül bir hata oluşturur 
* [Score Model] [ score-model] modül hatalı sonuçlar oluşturur 

Bu makalede, bu hataların olası nedenleri açıklanmaktadır.


## <a name="train-model-module-produces-an-error"></a>Train Model modülü bir hata oluşturur.

![image1](./media/debug-models/train_model-1.png)

[Modeli eğitme] [ train-model] modülü, iki giriş bekler:

1. Machine learning modeli modeller Azure Machine Learning Studio tarafından sağlanan koleksiyonundan türü.
2. Eğitim verileri tahmin etmek için değişken belirten belirtilen etiket sütunu (diğer sütunları özellikleri olarak kabul edilir).

Bu modül, aşağıdaki durumlarda bir hata oluşturabilecek:

1. Etiket sütununda hatalı şekilde belirtildi. Bu, birden fazla sütun etiketi olarak seçilir ya da yanlış sütun dizini seçili oluşabilir. Örneğin, bir sütun dizini 30 yalnızca 25 sütuna sahip bir giriş veri kümesi ile kullanılıyorsa, ikinci koşul uygular.

2. Veri kümesi herhangi bir özellik sütunu içermiyor. Örneğin, giriş veri kümesi etiketi sütun olarak işaretlenmiş, yalnızca bir sütun varsa olacaktır, modeli oluşturmak üzere hiçbir özellik. Bu durumda, [modeli eğitme] [ train-model] modül bir hata oluşturur.

3. Giriş veri kümesi (Özellikler veya etiket), sonsuz bir değeri içerir.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model modülü hatalı sonuçlar üretir.

![image2](./media/debug-models/train_test-2.png)

Denetimli öğrenme için tipik bir eğitim ve test denemede [verileri bölme] [ split] modülü özgün veri kümesinden iki bölüme ayıran: bir kısım modeli eğitmek için kullanılır ve puanlamak için kullanılan bir bölümü nasıl Eğitim modeli de gerçekleştirir. Eğitilen modelin sonra sonuçları sonra model doğruluğunu belirlemek için değerlendirilir test verileri puanlamak için kullanılır.

[Score Model] [ score-model] modül iki giriş gerektiriyor:

1. Eğitilen modelin çıktısını [modeli eğitme] [ train-model] modülü.
2. Modeli eğitmek için kullanılan veri kümesinden farklı bir Puanlama veri kümesi.

Rağmen deneme başarılı olduğunu, mümkündür [Score Model] [ score-model] modül hatalı sonuçlar oluşturur. Birkaç senaryo gerçekleştirilecek bu soruna neden olabilir:

1. Belirtilen etiket kategorik veriler üzerinde bir regresyon modeli eğitilir, yanlış bir çıktı tarafından üretilen [Score Model] [ score-model] modülü. Regresyon sürekli yanıt değişkeni gerektirdiğinden budur. Bu durumda, bir sınıflandırma modelini kullanmak daha uygun olacaktır. 

2. Benzer şekilde, etiket sütununda kayan nokta numarası sahip bir veri kümesi üzerinde bir sınıflandırma modeli eğitilir, istenmeyen sonuçlara neden olabilir. Yalnızca bu aralık değerleri sınıflarının sınırlı ve küçük, küme üzerinde veren bir ayrık yanıt değişken Sınıflandırmayı gerektirmiyor olmasıdır.

3. Puanlama veri kümesi modeli eğitmek için kullanılan tüm özellikleri içermiyorsa [Score Model] [ score-model] bir hata oluşturur.

4. Puanlama veri kümesi bir satır eksik veya özellikleri, herhangi bir sonsuz değerini içeriyorsa [Score Model] [ score-model] o satırına karşılık gelen herhangi bir çıktı üretmez.

5. [Score Model] [ score-model] Puanlama kümesindeki tüm satırlar için aynı çıktı üretebilir. Bu, örneğin, örnek yaprak düğümü başına en düşük sayısı olması sayıdan fazla eğitim örnekleri kullanılabilir seçilirse karar ormanları kullanarak sınıflandırma çalışılırken gerçekleşebilir.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

