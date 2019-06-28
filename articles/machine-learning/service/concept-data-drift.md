---
title: Veri değişikliklerini izleme (Önizleme)
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti için veri değişikliklerini nasıl izleyebilirsiniz öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cody-dkdc
ms.author: copeters
ms.date: 06/20/2019
ms.openlocfilehash: 56761c32484d4f5b27800e56143c62d3731da852
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332960"
---
# <a name="what-is-data-drift-monitoring-preview"></a>Veri nedir, izleme (Önizleme) kayma?

Veri değişikliklerini veri dağıtımındaki farklıdır. Machine learning bağlamında, eğitilen makine öğrenimi modellerini düşürülmüş tahmini Performans nedeniyle kayması karşılaşabilirsiniz. Eğitim verilerini ve Öngörüler yapmak için kullanılan verileri arasında değişikliklerini izleme, performans sorunlarını belirlemenize yardımcı olabilir.

Makine öğrenimi modelleri yalnızca onları eğitmek için kullanılan veri olarak uygundur. Modelleri, kendi performans izleme olmadan Üretim dağıtımı sonrasında saptanmayan ve verebilirliğinde üzerindeki etkileri neden olabilir. Veri değişikliklerini izleme ile algılayın ve için veri değişikliklerini uyarlayabilirsiniz. 

## <a name="when-to-monitor-for-data-drift"></a>İçin veri değişikliklerini izlemek ne zaman?

İnşa ediyoruz ölçümler şunları içerir:

+ Büyüklüğünü kayması (kayması katsayısı)
+ Nedenini kayması (kayması katkı özelliği tarafından)
+ Uzaklık ölçümleri (Wasserstein, enerji, vb.)

Bu izleme ile uyarı veya Eylemler kayması algılanır ve veri bilimi insanı sorunun kök nedenini araştırmak ayarlanabilir. 

Dağıtılan modeliniz için girdi verilerini değişebilir düşünüyorsanız, veri değişikliklerini algılama kullanmayı düşünmeniz gerekir.

## <a name="how-data-drift-is-monitored-in-azure-machine-learning-service"></a>Azure Machine Learning hizmeti veri değişikliklerini nasıl izlenir

Kullanarak **Azure Machine Learning hizmeti**, veri değişikliklerini, veri kümeleri veya dağıtımları ile izlenir. İçin veri değişikliklerini izlemek için bir temel veri kümesi - genellikle eğitim veri kümesi için bir model - belirtilir. İkinci bir veri kümesi - genellikle bir dağıtımdan - model giriş verileri toplanan temel veri kümesinde sınanır. Her iki veri kümelerinin olduğunu [profili](how-to-create-dataset-snapshots.md) ve giriş verileri için farklı izleme hizmeti. İki veri kümesi arasındaki farkları algılamak için makine öğrenme modeli eğitilir. Modelin performans değişikliklerini katsayısı, iki veri kümesi arasında kayması büyüklüğünü ölçer dönüştürülür. Kullanarak [model interpretability](machine-learning-interpretability-explainability.md) kayması katsayısı için katkıda bulunan özellikler hesaplanır. Veri kümesi profilinden her özellik hakkında istatistiksel bilgi izlenir. 

## <a name="data-drift-metric-output"></a>Veri değişikliklerini ölçüm çıkış

Kayması ölçümleri görüntülemek için birden çok yolu vardır:

* Jupyter pencere öğesi kullanın.
* Kullanım `get_metrics()` herhangi işlevi `datadriftRun` nesne.
* Modelinizde Azure portalında ölçümleri görüntüleyin

Aşağıdaki ölçümler, veri değişikliklerini görevin çalışma her yinelemede kaydedilir:

|Ölçüm|Açıklama|
--|--|
wasserstein_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklığı.|
energy_distance|Tek boyutlu sayısal dağıtım için tanımlanan istatistiksel uzaklığı.|
datadrift_coefficient|Resmi olarak Matthews korelasyon katsayısı, 1 ile 1 arasında değişen bir gerçek sayı. Kayması bağlamında, 0 hiçbir kayması gösterir ve 1, en fazla kayması gösterir.|
datadrift_contribution|Katkıda bulunan farklı özellikler önemini özelliği.|

## <a name="next-steps"></a>Sonraki adımlar

Örneklere bakın ve için veri değişikliklerini izleme hakkında bilgi edinin:

+ [Azure Kubernetes Service (AKS) aracılığıyla dağıtılan modellerinde veri değişikliklerini izleme hakkında bilgi edinin](how-to-monitor-data-drift.md)
+ Denemenin [Jupyter not defteri örnekleri](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/data-drift/)