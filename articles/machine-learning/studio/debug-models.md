---
title: Azure Machine learning'de modelinizi hata ayıklama | Microsoft Docs
description: Azure Machine Learning modeli eğitme ve Score Model modülleri tarafından oluşturulan hataları ayıklamak nasıl.
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: (previous ms.author=hshapiro, author=heatherbshapiro)
ms.author: amlstudiodocs
manager: hjerez
editor: cgronlun
ms.assetid: 629dc45e-ac1e-4b7d-b120-08813dc448be
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.openlocfilehash: fd81ff0ade0267d052862beed9d84649a7721ee9
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52263698"
---
# <a name="debug-your-model-in-azure-machine-learning"></a>Azure Machine Learning’de Model Hatalarını Ayıklama

Bu makalede, neden ya da aşağıdaki iki hataları bir model çalıştırırken karşılaşılabilecek olası nedenleri açıklanmaktadır:

* [modeli eğitme] [ train-model] modül bir hata oluşturur 
* [Score Model] [ score-model] modül hatalı sonuçlar oluşturur 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Train Model modülü bir hata oluşturur.

![image1](./media/debug-models/train_model-1.png)

[Modeli eğitme] [ train-model] modülü, iki giriş bekler:

1. Machine learning modeli modeller Azure Machine Learning tarafından sağlanan koleksiyonundan türü.
2. Eğitim verileri tahmin etmek için değişken belirten belirtilen etiket sütunu (diğer sütunları özellikleri olarak kabul edilir).

Bu modül, aşağıdaki durumlarda bir hata oluşturabilecek:

1. Etiket sütununda hatalı şekilde belirtildi. Bu, birden fazla sütun etiketi olarak seçilir ya da yanlış sütun dizini seçili oluşabilir. Örneğin, bir sütun dizini 30 yalnızca 25 sütunları olan bir giriş veri kümesi ile kullanılıyorsa, ikinci koşul uygular.

2. Veri kümesi herhangi bir özellik sütunu içermiyor. Örneğin, giriş veri kümesi etiketi sütun olarak işaretlenmiş, yalnızca bir sütun varsa olacaktır, modeli oluşturmak üzere hiçbir özellik. Bu durumda, [modeli eğitme] [ train-model] modül bir hata oluşturur.

3. Giriş veri kümesi (Özellikler veya etiket), sonsuz bir değeri içerir.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model modülü hatalı sonuçlar üretir.

![image2](./media/debug-models/train_test-2.png)

Denetimli öğrenme için tipik bir eğitim ve test denemede [verileri bölme] [ split] modülü özgün veri kümesinden iki bölüme ayıran: bir kısım modeli eğitmek için kullanılır ve puanlamak için kullanılan bir bölümü nasıl Eğitim modeli de gerçekleştirir. Eğitilen modelin sonra sonuçları sonra model doğruluğunu belirlemek için değerlendirilir test verileri puanlamak için kullanılır.

[Score Model] [ score-model] modül iki giriş gerektiriyor:

1. Eğitilen modelin çıktısını [modeli eğitme] [ train-model] modülü.
2. Modeli eğitmek için kullanılan veri kümesinden farklı bir Puanlama veri kümesi.

Rağmen deneme başarılı olduğunu, mümkündür [Score Model] [ score-model] modül hatalı sonuçlar oluşturur. Birkaç senaryoda bu oluşmasına neden olabilir:

1. Belirtilen etiket kategorik veriler üzerinde bir regresyon modeli eğitilir, yanlış bir çıktı tarafından üretilen [Score Model] [ score-model] modülü. Regresyon sürekli yanıt değişkeni gerektirdiğinden budur. Bu durumda, bir sınıflandırma modelini kullanmak daha uygun olacaktır. 

2. Benzer şekilde, etiket sütununda kayan nokta numarası sahip bir veri kümesi üzerinde bir sınıflandırma modeli eğitilir, istenmeyen sonuçlara neden olabilir. Yalnızca bu aralık değerleri sınıflarının sınırlı ve genellikle biraz küçük, küme üzerinde veren bir ayrık yanıt değişken Sınıflandırmayı gerektirmiyor olmasıdır.

3. Puanlama veri kümesi modeli eğitmek için kullanılan tüm özellikleri içermiyorsa [Score Model] [ score-model] bir hata oluşturur.

4. Puanlama veri kümesi bir satır eksik veya özellikleri, herhangi bir sonsuz değerini içeriyorsa [Score Model] [ score-model] o satırına karşılık gelen herhangi bir çıktı oluşturmaz.

5. [Score Model] [ score-model] Puanlama kümesindeki tüm satırlar için aynı çıktı üretebilir. Bu, örneğin, örnek yaprak düğümü başına en düşük sayısı olması sayıdan fazla eğitim örnekleri kullanılabilir seçilirse karar ormanları kullanarak sınıflandırma çalışılırken gerçekleşebilir.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

