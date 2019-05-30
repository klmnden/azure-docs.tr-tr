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
ms.date: 01/25/2019
ms.openlocfilehash: ce8b117a3cbe0e3a5c4265729ccf5c0264241013
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391771"
---
1. Bölümündeki yönergeleri kullanın [bir Azure Machine Learning hizmeti çalışma alanı oluşturma](../articles/machine-learning/service/setup-create-workspace.md#portal) aşağıdakileri yapmak için:
    * Miniconda ortamı oluşturma
    * Azure Machine için Python SDK'sı Learning yükleme
    * Çalışma alanı oluşturma
    * Bir çalışma alanı yapılandırma dosyasını yazma (**aml_config/config.json**).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```