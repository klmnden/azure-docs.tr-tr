---
title: Otomatik ML algoritması seçme ve ayarlama
titleSuffix: Azure Machine Learning service
description: Nasıl Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçme ve ondan size zaman sağlamak için ölçütleri ve parametreleri kullanarak kaydetmek için bir model oluşturmak öğrenin modeliniz için en iyi algoritmayı seçin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/12/2018
ms.custom: seodec18
ms.openlocfilehash: 620dbd22613df37fdc3c20e34906684446b2251f
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546020"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Otomatik makine öğrenimi eğitim verileri ile tanımlanan hedefi özelliği ve algoritmaları ve otomatik olarak en iyi modeli için eğitim puanları temel alarak verilerinizi seçmek için özellik seçimleri birleşimlerini ile Yinelem işlemidir. Geleneksel makine öğrenme modeli geliştirme sürecinde yüksek kaynak kullanımı yoğun ve çalıştırmak ve modelleri onlarca sonuçları karşılaştırmak için önemli bir etki alanı bilgilerini ve saat yatırım gerektirir. Otomatik makine öğrenimi modelleri çalıştırmak deneme süresi gibi denemeniz için tanımlanan kısıtlamaları ve hedefler ayarlanmış veya kara listeye hangi modelleri oluşturarak bu işlemi basitleştirir.

## <a name="how-it-works"></a>Nasıl çalışır?

1. Makine öğrenimi sorunu çözmeye çalışıyorsanız türünü yapılandırın. Denetimli öğrenmede kategoriler desteklenir:
   + Sınıflandırma
   + Regresyon
   + Tahmin etme

   Genel kullanıma sunulan otomatik makine öğrenimi ederken **tahmin özelliği hala genel Önizleme aşamasındadır.**

   Bkz: [modelleri](how-to-configure-auto-train.md#select-your-experiment-type) Azure Machine Learning eğitimi deneyebilirsiniz.

1. Kaynak ve eğitim verileri biçimini belirtin. Veriler etiketlenmelidir ve geliştirme ortamınızda veya Azure Blob Depolama alanında depolanabilir. Veriler üzerinde geliştirme ortamınızı depolanmışsa, eğitim betikleriniz ile aynı dizinde olmalıdır. Bu dizin, eğitim için seçtiğiniz işlem hedefe kopyalanır.

    Eğitim betiğinizde veri Numpy diziler veya Pandas dataframe okunabilir.

    Eğitim ve doğrulama verileri seçmek için bölünmüş seçenekleri yapılandırabilirsiniz ya da ayrı eğitim ve doğrulama veri kümelerini belirtebilirsiniz.

1. Yapılandırma [hedef işlem](how-to-set-up-training-targets.md) modeli eğitmek için kullanılır.

1. Otomatik makine öğrenimi yapılandırması yapılandırın. Bu farklı modelleri, Hiper parametre ayarları, Azure Machine Learning yinelendiğinden kullanılan parametreleri denetler ve hangi ölçümleri ne zaman aramak için en iyi modeli belirleme

1. Çalıştıran bir eğitim gönderin.

Eğitim sırasında Azure Machine Learning hizmetinde bir dizi farklı algoritmaları ve parametrelerle deneyin işlem hatları oluşturur. Sağladığınız yineleme sınırına İsabetleri sonra veya belirttiğiniz ölçüm için hedef değer ulaştığında durdurur.

[![Otomatik makine öğrenimi](./media/how-to-automated-ml/automated-machine-learning.png)](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Çalıştırma sırasında toplanan ölçümleri içeren günlüğe kaydedilen çalışma bilgileri inceleyebilirsiniz. Eğitim da Python serileştirilmiş nesne çalıştırmanın (`.pkl` dosyası) modeli ve veri ön işleme içerir.


## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik Machine Learning kullanarak modelleri oluşturmayı öğrenin:

+ [Öğretici: Otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md)

+ [Not Defteri örnekleri](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)

+ [Uzak bir kaynağa otomatik eğitim kullanma](how-to-auto-train-remote.md)

+ [Otomatik eğitim ayarlarını yapılandırma](how-to-configure-auto-train.md)
