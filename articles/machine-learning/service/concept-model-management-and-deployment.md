---
title: 'MLOps: Yönetme, dağıtma ve izleme ML modelleri'
titleSuffix: Azure Machine Learning service
description: 'Azure Machine Learning hizmeti için MLOps kullanmayı öğrenin: dağıtma, yönetme ve izleme Modellerinizi sürekli olarak iyileştirin. Yerel makinenizde veya diğer kaynaklardan Azure Machine Learning hizmeti ile eğitilmiş modeller dağıtabilirsiniz.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: chris-lauren
ms.author: clauren
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 416bebc070cfcad52c6180e65f0066c46c826cbe
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65849639"
---
# <a name="mlops-manage-deploy-and-monitor-models-with-azure-machine-learning-service"></a>MLOps: Yönetin, dağıtın ve modeller Azure Machine Learning hizmeti ile izleme

Bu makalede, Azure Machine Learning hizmeti dağıtma, yönetme ve sürekli olarak geliştirmek için Modellerinizi izlemek için nasıl kullanılacağını öğrenebilirsiniz. Azure Machine Learning ile yerel makinenizde veya diğer kaynaklardan eğitilmiş modeller dağıtabilirsiniz. 

Tam dağıtım iş akışı aşağıdaki diyagramda gösterilmektedir: [![Azure Machine Learning için dağıtım iş akışı](media/concept-model-management-and-deployment/deployment-pipeline.png)](media/concept-model-management-and-deployment/deployment-pipeline.png#lightbox)

MLOps / dağıtımı iş akışı, aşağıdaki adımları içerir:
1. **Modeli kaydetmeyi** , Azure Machine Learning hizmeti çalışma alanında barındırılan bir kayıt defterinde
1. **Kullanım** bir web hizmeti bulutta, IOT cihazında veya Power BI ile analiz için model.
1. **İzleme ve veri toplama**
1. **Güncelleştirme** yeni görüntüyü kullanarak bir dağıtım.

Her adım, bağımsız olarak veya tek bir komutun parçası olarak gerçekleştirilebilir. Ayrıca, oluşturabileceğiniz bir **CI/CD iş akışı** Bu grafikte gösterildiği gibi.

[!['Azure Machine Learning sürekli tümleştirme/sürekli dağıtım (CI/CD) döngüsü'](media/concept-model-management-and-deployment/model-ci-cd.png)](media/concept-model-management-and-deployment/model-ci-cd.png#lightbox)

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2X1GX]

## <a name="step-1-register-model"></a>1. Adım: Modeli kaydetme

Model kaydı, depolamanızı ve sürüm Modellerinizi çalışma alanınızda Azure bulutunda sağlar. Model kayıt defterini düzenlemek ve eğitilen Modellerinizi izlemek kolaylaştırır.

> [!TIP]
> Azure Machine Learning hizmetinin dışında eğitilen modelleri de kaydedebilirsiniz.
 
Kayıtlı modelleri, ada ve sürüme göre tanımlanır. Mevcut bir aynı ada sahip bir model her kaydettirdiğinizde, kayıt defteri sürüm artırır. Ek meta veri etiketleri aramak modellerinde kullanılabilir kayıt sırasında de sağlayabilirsiniz. Azure Machine Learning hizmeti yüklenen Python 3.5.2 kullanarak ya da daha yüksek olabilir herhangi bir modelini destekler.

Etkin bir dağıtımda kullanılmayan modelleri nelze odstranit.

Daha fazla bilgi için kayıt modeli bölümüne bakın. [modelleri dağıtma](how-to-deploy-and-where.md#registermodel).

Bir model pickle biçiminde depolanan kaydetme ilişkin bir örnek için bkz [Öğreticisi: Bir görüntü sınıflandırma modeli eğitme](tutorial-deploy-models-with-aml.md).

## <a name="step-2-use-the-model"></a>2. Adım: Kullanım modeli

Makine öğrenimi modelleri, Power BI gibi hizmetlerden analiz veya IOT Edge cihazlarında bir web hizmeti olarak kullanılabilir.

### <a name="web-service"></a>Web hizmeti

Modellerinizi içinde kullanabileceğiniz **web Hizmetleri** hedefleri ile aşağıdaki işlem:

* Azure Container Örneği
* Azure Kubernetes Service

Bir web hizmeti olarak modeli dağıtacağız için aşağıdakileri sağlamanız gerekir:

* Model veya modellerin topluluğu.
* Model kullanmak için gerekli bağımlılıkları. Örneğin, istekleri kabul eder ve çağıran conda bağımlılıklarını, modeli bir betik vb.
* Nasıl ve nerede açıklayan bir dağıtım yapılandırması model dağıtma.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="iot-edge-devices"></a>IoT Edge cihazları

Modeller IOT cihazları ile kullanabileceğiniz **Azure IOT Edge modülleri**. IOT Edge modülleri çıkarım ya da, cihazda Puanlama modeli sağlayan donanım cihazlarına dağıtılır.

Daha fazla bilgi için [modelleri dağıtma](how-to-deploy-and-where.md).

### <a name="analytics"></a>Analiz

Microsoft Power BI, verileri analiz için makine öğrenimi modelleri kullanarak destekler. Daha fazla bilgi için [Power BI (Önizleme) Azure Machine Learning tümleştirme](https://docs.microsoft.com/power-bi/service-machine-learning-integration).

## <a name="step-3-monitor-models-and-collect-data"></a>3. adım: İzleyici modeller ve veri toplama

İzleme, hangi veri modeliniz ve döndürdüğü Öngörüler gönderildiğini anlamanıza olanak tanır.

Bu bilgiler, modelinizi nasıl kullanıldığını anlamanıza yardımcı olur. Toplanan giriş veri modelinin eğitim gelecekteki sürümlerinde yararlı olabilir.

Daha fazla bilgi için [model verileri toplamayı etkinleştirme](how-to-enable-data-collection.md).

## <a name="step-4-update-the-deployment"></a>4. Adım: Güncelleştirme dağıtımı

Dağıtımları açıkça güncelleştirilmesi gerekir. Daha fazla bilgi için güncelleştirme bölümünü [modelleri dağıtma](how-to-deploy-and-where.md#update).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [nasıl ve nerede modelleri dağıtma](how-to-deploy-and-where.md) Azure Machine Learning hizmeti ile. Dağıtım örneği için bkz: [Öğreticisi: Azure Container ınstances'da bir görüntü sınıflandırma modeli dağıtma](tutorial-deploy-models-with-aml.md).

Oluşturmayı [sürekli tümleştirme ve dağıtım ML modelleri Azure işlem hatları ile](/azure/devops/pipelines/targets/azure-machine-learning). 

İstemci uygulamaları oluşturmayı öğrenin ve Hizmetleri [bir web hizmeti olarak modeli kullanma](how-to-consume-web-service.md).
