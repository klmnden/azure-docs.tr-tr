---
title: Örnek Jupyter Not Defterleri
titleSuffix: Azure Machine Learning service
description: Bulup Python Azure Machine Learning hizmetinde keşfetmek için örnek Jupyter not defterleri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: cd88fd85ce6d18287c700a54e42b6237a42ea5c9
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035373"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Azure Machine Learning hizmeti keşfetmek için Jupyter not defterleri kullanma

Kolaylık olması için Jupyter Python not defterleri, Azure Machine Learning hizmeti keşfetmek için kullanabileceğiniz bir dizi geliştirdik. 

Bu sitedeki belgelere hizmeti kullanmak ve bunları kendi durumunuza özelleştirmek için bu not defterlerini kullanma hakkında bilgi edinin. 

Bu örnek not defterleri ile bir not defteri sunucusu çalıştırmak için aşağıdaki yollardan birini kullanın.  Sunucu çalıştırıldıktan sonra öğretici not defterlerinde Bul **öğreticiler** klasöründe veya farklı özellikleri keşfedin **Yardım-How-to-kullanın-azureml** klasör.

## <a name="a-managed-cloud-notebook-server"></a>Yönetilen bir bulut not defteri sunucusu

Kendi bulut tabanlı bir not defteri sunucusu ile çalışmaya başlama daha kolaydır. Örnek Not Defteri ve [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) zaten yüklü ve bu bulut kaynağı oluşturduktan sonra sizin için yapılandırılır.  

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Örnekler, Not Defteri sayfasında kullanılabilir.

## <a name="a-data-science-virtual-machine-dsvm"></a>Bir veri bilimi sanal makinesi (DSVM)

[Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) ve not defteri sunucusu zaten yüklü ve sizin için bir DSVM üzerinde yapılandırılır. 

Çalıştırdıktan sonra [bir DSVM oluşturma](how-to-configure-environment.md#dsvm), not defterlerini çalıştırmak için DSVM üzerinde aşağıdaki adımları kullanın.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="your-own-jupyter-notebook-server"></a>Kendi Jupyter Notebook sunucusu

Bilgisayarınızda yerel bir Jupyter not defteri sunucusu oluşturmak için aşağıdaki adımları kullanın.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Kurulum yönergelerini, hızlı ve öğretici not defterlerini çalıştırmak için gereken paketleri yükler.  Diğer örnek not defterleri, ek bileşen yüklenmesini gerektirebilir.  Bu bileşenler hakkında daha fazla bilgi için bkz. [Python için Azure Machine Learning SDK'sını yükleme](https://docs.microsoft.com/python/api/overview/azure/ml/install).

## <a name="azure-notebooks"></a>Azure Notebooks

Örnek Not Defterleri ve [Python için Azure Machine Learning SDK](https://aka.ms/aml-sdk) zaten yüklü ve sizin için yapılandırılmış [Azure not defterleri](https://notebooks.azure.com/). Yükleme ve gelecekteki güncelleştirmelerin otomatik olarak Azure Hizmetleri yönetilir.

Kullanım [Azure portalında](https://portal.azure.com) Azure not defterleri ile kullanmaya başlamak için.  Çalışma alanını açın ve **genel bakış** bölümünden **Get Started Azure not defterlerinde**.

## <a name="next-steps"></a>Sonraki adımlar

+ Azure Machine Learning hizmetinde bu GitHub deposundan örnek not defterleri keşfedin: https://aka.ms/aml-notebooks

Bu öğreticileri deneyin:
+ [Eğitmek ve bir görüntü sınıflandırma modeli MNIST ile dağıtma](tutorial-train-models-with-aml.md)

+ [Veri hazırlama ve NYC taksi veri kümesiyle bir regresyon modeli eğitmek için otomatik makine öğrenimi kullanıyor](tutorial-data-prep.md)