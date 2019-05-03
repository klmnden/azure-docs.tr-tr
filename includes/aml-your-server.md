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
ms.openlocfilehash: 8d21e41ad487ad17598f2320fab5eebae02309e8
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65021719"
---
1. Bölümündeki yönergeleri kullanın [Azure Machine Learning hizmeti çalışma alanı oluşturma](../articles/machine-learning/service/setup-create-workspace.md#portal) için:
    * Miniconda ortamı oluşturma
    * Azure Machine için Python SDK'sı Learning yükleme
    * Çalışma alanı oluşturma
    * Bir çalışma alanı yapılandırma dosyasını yazma (**aml_config/config.json**).
    
1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bu yöntemlerden birini kullanarak bir çalışma alanı yapılandırma dosyası ekleyin:
    * Kopyalama **aml_config/config.json** kopyalanmış dizine 1. adımda oluşturulan dosya.

    * İçinde [Azure portalında](https://ms.portal.azure.com)seçin **config.json indirme** gelen **genel bakış** çalışma alanınızın bir bölümü. 

    ![Config.json dosyasını indir](./media/aml-dsvm-server/download-config.png)

    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb).

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.
    
    ```shell
    jupyter notebook
    ```