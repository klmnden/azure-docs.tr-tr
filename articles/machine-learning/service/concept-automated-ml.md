---
title: Otomatik Machine Learning nedir
titleSuffix: Azure Machine Learning service
description: Parametreleri ve modeliniz için en iyi algoritmayı seçin sağladığınız ölçütleri kullanarak zaman nasıl Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçin ve bunu kaydetmek için bir model oluşturmak öğrenin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8c1641c975ff235b581f784f0965caf203a9562f
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53142537"
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

[ ![Otomatik makine öğrenimi](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Çalıştırma sırasında toplanan ölçümleri içeren günlüğe kaydedilen çalışma bilgileri inceleyebilirsiniz. Eğitim da Python serileştirilmiş nesne çalıştırmanın (`.pkl` dosyası) modeli ve veri ön işleme içerir.

## <a name="model-explainability"></a>Model explainability

Ortak bir durumu otomatik machine Learning, uçtan uca işlemi görmek için bağlanamama sorunudur. Azure Machine Learning modelleri arka uçta çalıştıran içine saydamlığı artırma hakkında ayrıntılı bilgi görüntülemek sağlar. Çıktı genel özellik önem modeli ayarlamak için özellikler tarafından sonuçları sıralama modelinizi en çok etkileyen gösterir. Ayrıca, sınıflandırma sorunları sınıfı başına özellik önem ve sıralamasını görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik Machine Learning kullanarak modelleri oluşturmayı öğrenin:

+ [Öğretici: Otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md)

+ [Otomatik eğitim ayarlarını yapılandırma](how-to-configure-auto-train.md)

+ [Uzak bir kaynağa otomatik eğitim kullanma](how-to-auto-train-remote.md) 