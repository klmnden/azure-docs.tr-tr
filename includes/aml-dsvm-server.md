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
ms.openlocfilehash: 02ef0d6c7c8ddc7088938d9c8ea379e3b97f3045
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66158735"
---
1. [Bir Azure Machine Learning hizmeti çalışma alanı oluşturma](../articles/machine-learning/service/setup-create-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bir çalışma alanı yapılandırma dosyası, bu yöntemlerden birini kullanarak kopyalanmış dizine ekleyin:

    * İçinde [Azure portalında](https://ms.portal.azure.com)seçin **config.json indirme** gelen **genel bakış** çalışma alanınızın bir bölümü. 

    ![Config.json dosyasını indir](./media/aml-dsvm-server/download-config.png)

    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) kopyalanan dizininizdeki dizüstü bilgisayar.


1. Kopyaladığınız dizinden notebook sunucusunu başlatın.
    
    ```shell
    jupyter notebook
    ```