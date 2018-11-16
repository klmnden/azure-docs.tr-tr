---
title: Oluşturma ve Azure Machine Learning hizmeti çalışma alanlarını yönetme
description: Oluşturun, görüntüleyin ve Azure Machine Learning hizmeti çalışma alanları Azure portalında silme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.openlocfilehash: 1565c54779278b440cfe631951e964921cc85720
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2018
ms.locfileid: "51709891"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Oluşturma ve Azure Machine Learning hizmeti çalışma alanlarını yönetme

Bu makalede, oluşturun, görüntüleyin, silin ve [ **Azure Machine Learning hizmeti çalışma alanları** ](concept-azure-machine-learning-architecture.md#workspace) Azure Portalı'nda [Azure Machine Learning hizmeti](overview-what-is-azure-ml.md).  Ayrıca oluşturabilir ve çalışma alanlarını silen [CLI kullanarak](reference-azure-machine-learning-cli.md) veya [Python kodu ile](https://aka.ms/aml-sdk).

Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Bir çalışma alanını görüntüle

1. Sol üst köşede Portalı'nın, seçin **tüm hizmetleri**. 

1. İçinde **tüm hizmetleri** filtre alanda, türü **Machine Learning hizmeti çalışma alanında**.  

   ![Azure Machine Learning hizmeti çalışma alanında Ara](media/how-to-manage-workspace/allservices-search1.png)

1. Filtre sonuçları'nda **Machine Learning hizmeti çalışma alanında** çalışma alanlarınızın listesini görüntüleyin. 

   ![Azure Machine Learning hizmeti çalışma alanında Ara](media/how-to-manage-workspace/allservices-search.PNG)

1. Çalışma alanı bulunamadı listesinde arayın. Bağlı aboneliği, kaynak grupları ve konumlarını filtreleyebilirsiniz.  

   ![Azure Machine Learning hizmeti çalışma alanı listesi](media/how-to-manage-workspace/allservices_view_workspace.PNG)

1. Özelliklerini görüntülemek için oluşturduğunuz çalışma alanını seçin.

   ![PNG](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Silmek istediğiniz çalışma alanının üstündeki Sil düğmesini kullanın.

  ![PNG](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir çalışma alanı oluşturmak, eğitmek ve modeller Azure Machine Learning hizmeti ile dağıtmak için nasıl kullanılacağını öğrenmek için eksiksiz bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Eğitme modelleri](tutorial-train-models-with-aml.md)
