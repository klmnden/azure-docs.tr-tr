---
title: Azure Machine Learning modelinizde hata ayıklama | Microsoft Docs
description: Azure Machine Learning Train Model ve Score Model modülleri tarafından oluşturulan hataları ayıklamak üzere nasıl.
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
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
ms.openlocfilehash: 144edca6d2e6fc8d0d8e59e6cff1d3f37eb233aa
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34834320"
---
# <a name="debug-your-model-in-azure-machine-learning"></a>Azure Machine Learning’de Model Hatalarını Ayıklama

Bu makalede neden aşağıdaki iki hataların ya da bir model çalıştırırken karşılaşılabilecek olası nedenleri açıklanmaktadır:

* [Train Model] [ train-model] modülünde bir hata oluşturur 
* [Score Model] [ score-model] modülü hatalı sonuçlar üretir 

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="train-model-module-produces-an-error"></a>Train Model modülünün bir hata oluşturur

![image1](./media/debug-models/train_model-1.png)

[Train Model] [ train-model] modülü iki girdi bekliyor:

1. Azure Machine Learning tarafından sağlanan modelleri koleksiyonundan makine öğrenimi modeline türü.
2. Eğitim verileri tahmin etmek için değişken belirten belirtilen etiket sütunu (diğer sütunların özellikleri olduğu varsayılır).

Bu modül aşağıdaki durumlarda hataya neden:

1. Etiket sütununda hatalı şekilde belirtildi. Bu, birden fazla sütun etiketi olarak seçilir ya da yanlış sütun dizini seçili meydana gelebilir. Örneğin, bir sütun dizini 30 yalnızca 25 sütunları olan bir giriş veri kümesi ile kullanılırsa, ikinci durum geçerli olur.

2. Veri kümesi herhangi bir özellik sütunu içermiyor. Örneğin, girdi veri kümesi etiket sütun işaretlendiğinden, yalnızca bir sütun varsa olurdu hangi model oluşturmak hiçbir özellik. Bu durumda, [Train Model] [ train-model] modülünde bir hata oluşturur.

3. Girdi veri kümesi (özellikleri veya etiketi) sonsuz bir değer içeriyor.

## <a name="score-model-module-produces-incorrect-results"></a>Score Model modülünün hatalı sonuçlar üretir

![image2](./media/debug-models/train_test-2.png)

Tipik bir eğitim ve sınama deneme denetimli öğrenme için de [bölünmüş veri] [ split] modülü iki bölüme özgün dataset böler: bir bölümü modeli eğitmek için kullanılır ve bir bölümü ne kadar iyi eğitilen model gerçekleştirir Puanlama amacıyla kullanılır. Eğitim modeli sonra daha sonra sonuçları modelin doğruluğunu belirlemek için değerlendirilen test verileri Puanlama amacıyla kullanılır.

[Score Model] [ score-model] modülü iki giriş gerektirir:

1. Eğitilen model çıktısını [Train Model] [ train-model] modülü.
2. Modeli eğitmek için kullanılan veri kümesinden farklı bir Puanlama veri kümesi.

Rağmen deneme başarılı olduğunu, mümkünse [Score Model] [ score-model] modülü hatalı sonuçlar üretir. Bazı senaryolarda bu gerçekleşecek şekilde neden olabilir:

1. Belirtilen etiket kategorik ise ve bir regresyon modeli verileri eğitildi, hatalı bir çıktı tarafından üretilen [Score Model] [ score-model] modülü. Regresyon sürekli yanıt değişkeni gerektirdiğinden budur. Bu durumda, bir sınıflandırma modelini kullanmak daha uygun olacaktır. 

2. Benzer şekilde, bir sınıflandırma modeli Etiket sütununda kayan nokta numarası olan bir veri kümesi üzerinde eğitildi, istenmeyen sonuçlara neden olabilir. Yalnızca bu aralık değerleri sınıfların sonlu ve genellikle biraz küçük, küme üzerinde veren bir ayrık yanıt değişken Sınıflandırmayı gerektirmiyor olmasıdır.

3. Puanlama veri kümesi modeli eğitmek için kullanılan tüm özellikleri içermiyorsa [Score Model] [ score-model] bir hata oluşturur.

4. Puanlama kümesindeki bir satır eksik veya kendi özelliklerinden herhangi birini sonsuz bir değerini içeriyorsa [Score Model] [ score-model] ilgili satır için karşılık gelen herhangi bir çıktı oluşturmaz.

5. [Score Model] [ score-model] Puanlama kümesindeki tüm satırların aynı çıktıları üretebilir. Bu, örneğin, örnekleri yaprak düğümü başına en az sayıda eğitim örnekleri kullanılabilir sayısından daha fazla olacak şekilde seçilirse karar ormanları kullanarak sınıflandırma çalışılırken gerçekleşebilir.

<!-- Module References -->
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/

