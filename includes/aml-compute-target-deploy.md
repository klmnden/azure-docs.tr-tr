---
title: include dosyası
description: include dosyası
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/30/2019
ms.openlocfilehash: 59e5adb5d1ee35b983c9552a2076a373561c0b44
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67168315"
---
| Hedef işlem | Kullanım | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel&nbsp;web&nbsp;hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Test/hata ayıklama | Belki de | &nbsp; | Sınırlı test etme ve sorun giderme için uygundur. Donanım hızlandırma yerel sistemde kitaplıkları kullanarak bağlıdır.
| [Azure Kubernetes Service'i (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarımı |  [Evet](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Evet](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar.  AKS için görsel arabirim kullanılabilir tek seçenektir. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | Düşük ölçek gerektiren CPU tabanlı iş yükleri için iyi < 48 GB RAM |
| [Azure Machine Learning işlem](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Önizleme) Batch&nbsp;çıkarımı | evet | &nbsp;  | Toplu Puanlama sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli sanal makineleri destekler. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Önizleme) IOT&nbsp;Modülü |  &nbsp; | &nbsp; | Dağıtma ve IOT cihazlarında ML modelleri hizmet. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | IOT Edge ile |  &nbsp; | evet | Dağıtma ve IOT cihazlarında ML modelleri hizmet. |
