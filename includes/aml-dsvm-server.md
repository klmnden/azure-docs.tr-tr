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
ms.openlocfilehash: a882a874574395095e98079cd0f8aa4a4987c749
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391775"
---
1. [Bir Azure Machine Learning hizmeti çalışma alanı oluşturma](../articles/machine-learning/service/setup-create-workspace.md).

1. [GitHub deposunu](https://aka.ms/aml-notebooks) kopyalayın.

    ```CLI
    git clone https://github.com/Azure/MachineLearningNotebooks.git
    ```

1. Bir çalışma alanı yapılandırma dosyası, bu yöntemlerden birini kullanarak kopyalanmış dizine ekleyin:

    * İçinde [Azure portalında](https://ms.portal.azure.com)seçin **config.json indirme** gelen **genel bakış** çalışma alanınızın bir bölümü. 

    ![Config.JSON indirin](./media/aml-dsvm-server/download-config.png)

    * Kod kullanarak yeni bir çalışma alanı oluşturma [configuration.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/configuration.ipynb) kopyalanan dizininizdeki dizüstü bilgisayar.

1. Kopyaladığınız dizinden notebook sunucusunu başlatın.

    ```shell
    jupyter notebook
    ```