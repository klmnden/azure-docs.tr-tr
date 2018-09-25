---
title: Modeller Azure Machine Learning hizmeti ile dağıtılacağı yeri | Microsoft Docs
description: Modellerinizi Azure Machine Learning hizmetini kullanarak üretime dağıtabileceğiniz farklı yolları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/29/2018
ms.openlocfilehash: 606e8aed42bce69d5b3210b4e97f8cbfeaaf104c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961017"
---
# <a name="deploy-models-with-the-azure-machine-learning-service"></a>Azure Machine Learning hizmeti ile modelleri dağıtma

Azure Machine Learning hizmeti eğitilen modelinizi dağıtabileceğiniz çeşitli yöntemler sağlar. Bu belgede, modelinizin Azure bulutta veya IOT edge cihazları için bir web hizmeti olarak dağıtmayı öğrenin.

Modelleri için aşağıdaki işlem hedeflerine dağıtabilirsiniz:

- Azure Container Instances (ACI)
- Azure Kubernetes Hizmeti (AKS)
- Azure IoT Edge
- Alanda programlanabilir kapı dizileri (FPGA)

Bu belgenin geri kalan ayrıntılı bu seçeneklerin her biri hakkında konuşuyor.

## <a name="azure-container-instances"></a>Azure Container Instances

Kullanım Azure Container Instances varsa, bir REST API uç noktası olarak Modellerinizi dağıtmak veya aşağıdaki koşullardan biri daha fazla bilgi için geçerlidir:
- Puanlama ve modelinizi doğrulama için hızlı dağıtım için bakıyorsunuz. ACI dağıtım, genellikle 5 dakikadan daha kısa bir süre içinde tamamlanır.
- Modelinizi bir geliştirme veya test ortamı verirsiniz. ACI, abonelik başına 20 kapsayıcı grubu dağıtmanıza olanak tanır. Daha fazla bilgi için [kotaları ve Azure Container Instances için bölge kullanılabilirliği](https://docs.microsoft.com/azure/container-instances/container-instances-quotas) belge.

Daha fazla bilgi için [model Azure Container Instances'a dağıtma](how-to-deploy-to-aci.md) belge.

## <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Büyük ölçekli üretim senaryoları için Azure Kubernetes Service (AKS) için model dağıtabilirsiniz. Mevcut bir AKS kümesi kullanmak veya Azure Machine Learning SDK'sı, CLI veya Azure portalını kullanarak yeni bir tane oluşturun.

Bir küme olduğundan AKS oluşturma işlemi için çalışma süresi. Oluşturulduktan sonra bu küme birden çok dağıtım için yeniden kullanabilirsiniz. Küme veya onu içeren kaynak grubunu silerseniz, sonraki açışınızda dağıtmanız gerekir. yeni bir küme oluşturmanız gerekir.

AKS'ye dağıtma, otomatik ölçeklendirme, günlüğe kaydetme, model veri koleksiyonu ve web hizmetleriniz için hızlı yanıt süresi sağlar. 

Bir AKS kümesi oluşturma işlemi yaklaşık 20 dakika sürer.

Daha fazla bilgi için [Azure Kubernetes Service için model dağıtma](how-to-deploy-to-aks.md) belge.

## <a name="azure-iot-edge"></a>Azure IoT Edge

IOT cihazları ile verileri buluta göndermeden ve verileri döndürmek için bulutta barındırılan model üzerinde bekleyen yerine cihazın puanlamaların gerçekleştirilmesinin anlatıldığı daha hızlıdır. Azure IOT Edge, uç cihazlarında modelinizi barındırabilirsiniz. Bir veya daha fazla aşağıdaki özellikleri gerekirse modelinizi IOT Edge dağıtma:
- Hatta bir bulut bağlantısı olmadan öncelikli görevlerin yerel olarak işleme
- Hızlı bir şekilde buluttan çekmek için çok büyük oluşturulan verilerle çalışın
- Gerçek zamanlı olarak veya yerel cihazları yakın zeka işlemeden etkinleştir
- Veri gizliliği ile ilgili gereksinimleri 

Daha fazla bilgi için [Azure IOT Edge dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning) belge.

IOT Edge hizmeti hakkında daha fazla bilgi için bkz. [Azure IOT Edge belgeleri](https://docs.microsoft.com/azure/iot-edge/).


## <a name="field-programmable-gate-arrays-fpga"></a>Alanda programlanabilir kapı dizileri (FPGA)

Project Brainwave tarafından desteklenen donanım hızlandırılmış modelleri, gerçek zamanlı çıkarım istekleri için çok düşük gecikme süresine ulaşmanız mümkün kılar. Project Brainwave alanda programlanabilir kapı dizileri Azure bulutta dağıtılan derin sinir ağı (DNN) hızlandırır. Yaygın olarak kullanılan Dnn'leri aktarımlı öğrenme için featurizers olarak kullanılabilir veya kendi verilerinizden özelleştirilebilir ağırlıklara sahip eğitilir.

Daha fazla bilgi için [bir FPGA Dağıt](how-to-deploy-fpga-web-service.md) belge.

## <a name="next-steps"></a>Sonraki adımlar

Belirli bir işlem hedefi için bir model dağıtma hakkında bilgi almak için aşağıdaki belgelere bakın:

* [Azure Container Instances'a model dağıtma](how-to-deploy-to-aci.md)
* [Azure Kubernetes Service için model dağıtma](how-to-deploy-to-aks.md)
* [Azure IOT Edge için model dağıtma](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-machine-learning)
* [FPGA için model dağıtma](how-to-deploy-fpga-web-service.md)
