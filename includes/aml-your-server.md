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
ms.openlocfilehash: 18ba86ce7876ba8275eb4853e4fc9ea0f35fa186
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55302230"
---
1. Tamamlamak [Azure Machine Learning Python hızlı](../articles/machine-learning/service/quickstart-create-workspace-with-python.md) SDK'sını yükleyin ve bir çalışma alanı oluşturun.  Atlayabilirsiniz **not defterini kullanma** istiyorsanız bölümü.
1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bu yöntemlerden birini kullanarak bir çalışma alanı yapılandırma dosyası ekleyin:
    * Kopyalama **aml_config\config.json** kopyalanmış dizine önkoşul Hızlı Başlangıç'ı kullanarak oluşturduğunuz dosya.
    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).
1. Kopyaladığınız dizinden notebook sunucusunu başlatın.
    
    ```shell
    jupyter notebook
    ```