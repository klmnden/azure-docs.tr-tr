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
ms.openlocfilehash: a463ac9f9584cb13cadbcf79674d56b2f8e47c2c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66753084"
---
| Hedef işlem | Kullanım | GPU desteği | FPGA desteği | Açıklama |
| ----- | ----- | ----- | ----- | ----- |
| [Yerel web hizmeti](../articles/machine-learning/service/how-to-deploy-and-where.md#local) | Test/hata ayıklama | Belki de | &nbsp; | Sınırlı test etme ve sorun giderme için uygundur. Donanım hızlandırma yerel sistemde kitaplıkları kullanarak bağlıdır.
| [Azure Kubernetes Service'i (AKS)](../articles/machine-learning/service/how-to-deploy-and-where.md#aks) | Gerçek zamanlı çıkarımı |  [Evet](../articles/machine-learning/service/how-to-deploy-inferencing-gpus.md)  | [Evet](../articles/machine-learning/service/how-to-deploy-fpga-web-service.md)   |Büyük ölçekli üretim dağıtımları için idealdir. Otomatik ölçeklendirme ve hızlı yanıt süresi sağlar.  AKS için görsel arabirim kullanılabilir tek seçenektir. |
| [Azure Container Instances (ACI)](../articles/machine-learning/service/how-to-deploy-and-where.md#aci) | Test veya geliştirme | &nbsp;  | &nbsp; | Düşük ölçek gerektiren CPU tabanlı iş yükleri için iyi < 48 GB RAM |
| [Azure Machine Learning işlem](../articles/machine-learning/service/how-to-run-batch-predictions.md) | (Önizleme) Batch çıkarımı | evet | &nbsp;  | Toplu Puanlama sunucusuz bir işlem üzerinde çalıştırın. Normal veya düşük öncelikli sanal makineleri destekler. |
| [Azure IoT Edge](../articles/machine-learning/service/how-to-deploy-and-where.md#iotedge) | (Önizleme) IOT Modülü |  &nbsp; | &nbsp; | Dağıtma ve IOT cihazlarında ML modelleri hizmet. |
| [Azure Data Box Edge](../articles/databox-online/data-box-edge-overview.md)   | IOT Edge ile |  &nbsp; | evet | Dağıtma ve IOT cihazlarında ML modelleri hizmet. |