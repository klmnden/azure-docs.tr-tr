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
ms.openlocfilehash: 8d8b314965253dc00b39d0b068b1d6fb3e4aa471
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58395553"
---
1. Bölümündeki yönergeleri kullanın [Azure Machine Learning hizmeti çalışma alanı oluşturma](../articles/machine-learning/service/setup-create-workspace.md#portal) Miniconda ortamı oluşturmak için bir çalışma alanı oluşturma ve bir çalışma alanı yapılandırma dosyasını yazma (**aml_config/config.json**) .

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bu yöntemlerden birini kullanarak bir çalışma alanı yapılandırma dosyası ekleyin:
    * Kopyalama **aml_config/config.json** kopyalanmış dizine önkoşul Hızlı Başlangıç'ı kullanarak oluşturduğunuz dosya.
    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Kopyaladığınız dizinden notebook sunucusunu başlatın.
    
    ```shell
    jupyter notebook
    ```