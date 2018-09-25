---
title: Machine Learning otomatik - Azure Machine Learning nedir
description: Bu makalede, otomatik machine learning hakkında bilgi edinebilirsiniz. Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçin ve bir model oluşturmak. Machine learning yardımcı size zaman kazandırır, parametreleri ve modeliniz için en iyi algoritmayı seçin sağladığınız ölçütleri kullanarak otomatik.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: krishnan
author: krishnaanumalasetty
ms.date: 9/24/2018
ms.openlocfilehash: 2a9c05b68d05102fab80b2aa8fb1c1dad8a367ea
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960048"
---
# <a name="what-is-automated-machine-learning"></a>Nedir, makine öğrenimi otomatik?

Bu makalede, otomatik machine learning hakkında bilgi edinebilirsiniz. Azure Machine Learning hizmeti otomatik olarak sizin için bir algoritma seçin ve bir model oluşturmak. Machine learning yardımcı size zaman kazandırır, parametreleri ve modeliniz için en iyi algoritmayı seçin sağladığınız ölçütleri kullanarak otomatik.

## <a name="how-it-works"></a>Nasıl çalışır?

1. Makine öğrenimi sorunu çözmeye çalışıyorsanız türünü yapılandırın. Denetimli öğrenmede iki kategoriye desteklenir:
   + Sınıflandırma
   + Regresyon

   Bkz: [modelleri](how-to-configure-auto-train.md#select-your-experiment-type) Azure Machine Learning eğitimi deneyebilirsiniz.

1. Kaynak ve eğitim verileri biçimini belirtin. Veriler etiketlenmelidir ve geliştirme ortamınızda veya Azure Blob Depolama alanında depolanabilir. Veriler üzerinde geliştirme ortamınızı depolanmışsa, eğitim betikleriniz ile aynı dizinde olmalıdır. Bu dizin, eğitim için seçtiğiniz işlem hedefe kopyalanır.

    Eğitim betiğinizde veri Numpy diziler veya Pandas dataframe okunabilir.

    Eğitim ve doğrulama verileri seçmek için bölünmüş seçenekleri yapılandırabilirsiniz ya da ayrı eğitim ve doğrulama veri kümelerini belirtebilirsiniz.

1. Yapılandırma [hedef işlem](how-to-set-up-training-targets.md) modeli eğitmek için kullanılır.

1. Otomatik makine öğrenimi yapılandırması yapılandırın. Bu farklı modelleri, Hiper parametre ayarları, Azure Machine Learning yinelendiğinden kullanılan parametreleri denetler ve hangi ölçümleri ne zaman aramak için en iyi modeli belirleme. 

1. Çalıştıran bir eğitim gönderin.

Eğitim sırasında Azure Machine Learning hizmetinde bir dizi farklı algoritmaları ve parametrelerle deneyin işlem hatları oluşturur. Sağladığınız yineleme sınırına İsabetleri sonra veya belirttiğiniz ölçüm için hedef değer ulaştığında durdurur.

[ ![Otomatik makine öğrenimi](./media/how-to-automated-ml/automated-machine-learning.png) ](./media/how-to-automated-ml/automated-machine-learning.png#lightbox)

Çalıştırma sırasında toplanan ölçümleri içeren günlüğe kaydedilen çalışma bilgileri inceleyebilirsiniz. Eğitim çalıştırma modeli ve veri ön işleme içeren bir Python serileştirilmiş nesne (.pkl dosyası) da oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve otomatik Machine Learning kullanarak modelleri oluşturmayı öğrenin:

+ [Öğretici: Otomatik olarak bir Azure Machine Learning otomatik sınıflandırma modeli eğitme](tutorial-auto-train-models.md)

+ [Otomatik eğitim ayarlarını yapılandırma](how-to-configure-auto-train.md)

+ [Uzak bir kaynağa otomatik eğitim kullanma](how-to-auto-train-remote.md) 
