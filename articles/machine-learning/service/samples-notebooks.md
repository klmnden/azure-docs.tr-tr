---
title: Örnek Jupyter Not Defterleri
titleSuffix: Azure Machine Learning service
description: Bul ve Azure Machine Learning hizmeti Python SDK'sını keşfetmek için örnek Jupyter not defterleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 05/29/2019
ms.custom: seodec18
ms.openlocfilehash: ea4d5a807c25ea0406b49dac8a83ef1a34e0e8b3
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391767"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Azure Machine Learning hizmeti keşfetmek için Jupyter not defterleri kullanma

[Azure Machine Learning not defterlerini depo](https://github.com/azure/machinelearningnotebooks) en son Azure Machine Learning Python SDK'sı örnekleri içerir. Bu Juypter dizüstü bilgisayarlar, SDK'sı keşfedin ve projeleri kendi makine öğrenimi modellerini görevi gören yardımcı olmak için tasarlanmıştır.

Bu makalede, aşağıdaki ortamlardan depoya erişmek nasıl gösterir:

- [Azure Machine Learning not defteri VM](#azure-machine-learning-notebook-vm)
- [Kendi not defteri sunucusu Getir](#bring-your-own-jupyter-notebook-server)
- [Veri bilimi sanal makinesi](#data-science-virtual-machine)
- [Azure Notebooks](#azure-notebooks)

> [!NOTE]
> Depo kopyalandı sonra öğretici not defterlerinde bulabilirsiniz **öğreticiler** klasörü ve özelliğe özgü not defterlerinde **Yardım-How-to-kullanın-azureml** klasör.

## <a name="azure-machine-learning-notebook-vm"></a>Azure Machine Learning not defteri VM

Örnek ile kullanmaya başlamak için en kolay yolu tamamlamaktır [bulut tabanlı bir not defteri hızlı](quickstart-run-cloud-notebook.md). Tamamlandığında, SDK ve örnek depoyu ile önceden yüklenmiş bir adanmış notebook sunucusu gerekir. İndirme veya yükleme gerekli yok.

## <a name="bring-your-own-jupyter-notebook-server"></a>Kendi Jupyter Notebook sunucusu Getir

Not Defteri sunucusu kendi yerel geliştirme için getirmek istiyorsanız, aşağıdaki adımları izleyin:

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Bu yönergeler, hızlı ve öğretici not defterleri için gerekli temel SDK paketlerini yükleyin. Diğer örnek not defterleri ek bileşenleri yüklemek gerekli kılabiliriz. Daha fazla bilgi için [Python için Azure Machine Learning SDK'sını yükleme](https://docs.microsoft.com/python/api/overview/azure/ml/install).

## <a name="data-science-virtual-machine"></a>Veri Bilimi Sanal Makinesi

Veri bilimi sanal makinesi (DSVM) veri bilimi yapmak için özel olarak oluşturulmuş bir makine görüntüsüdür. Varsa, [bir DSVM oluşturma](how-to-configure-environment.md#dsvm), SDK ve not defteri sunucusu yüklenir ve sizin için yapılandırılır. Ancak, bir çalışma alanı oluşturma ve örnek depoyu kopyalamak yine de gerekir.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="azure-notebooks"></a>Azure Notebooks

Üzerinde [Azure not defterleri](https://notebooks.azure.com/), SDK ve not defteri sunucusu yüklenir ve sizin için yapılandırılır. Azure not defterleri için keşfetmek için bir tam olarak yönetilen ve basit bir dizüstü bilgisayar ortamı sağlar.

Örnek depoyu Azure Not erişmek için Azure Machine Learning çalışma alanını kullanarak gidin [Azure portalında](https://portal.azure.com). Gelen **genel bakış** bölümünden **Get Started Azure not defterlerinde**.

## <a name="next-steps"></a>Sonraki adımlar

Keşfedin [örnek not defterleri](https://aka.ms/aml-notebooks) hangi Azure Machine Learning bulmak için hizmet yapabilir, veya bu öğreticileri deneyin:

- [Eğitmek ve bir görüntü sınıflandırma modeli MNIST ile dağıtma](tutorial-train-models-with-aml.md)

- [Veri hazırlama ve NYC taksi veri kümesiyle bir regresyon modeli eğitmek için otomatik makine öğrenimi kullanıyor](tutorial-data-prep.md)