---
title: Oluşturma ve Azure Machine Learning çalışma alanları yönetme
description: Oluşturun, görüntüleyin ve Azure Machine Learning çalışma alanları, Azure portalında silme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.openlocfilehash: 7d01a2e3ebd46315966c82a43a17ffc5b329b829
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954356"
---
# <a name="create-and-manage-azure-machine-learning-workspaces"></a>Oluşturma ve Azure Machine Learning çalışma alanları yönetme

Bu makalede, oluşturun, görüntüleyin, silin ve [ **Azure Machine Learning çalışma alanları** ](concept-azure-machine-learning-architecture.md#workspace) Azure Portalı'nda [Azure Machine Learning hizmeti](overview-what-is-azure-ml.md).  Ayrıca oluşturabilir ve çalışma alanlarını silen [CLI kullanarak](reference-azure-machine-learning-cli.md) veya [Python kodu ile](http://aka.ms/aml-sdk).

Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view-a-workspace"></a>Bir çalışma alanını görüntüle

1. Sol üst köşede Portalı'nın, seçin **tüm hizmetleri**. 

1. İçinde **tüm hizmetleri** filtre alanda, türü **Machine Learning çalışma alanı**.  

   ![Azure Machine Learning çalışma alanı arayın](media/how-to-manage-workspace/allservices-search1.png)

1. Filtre sonuçları'nda **Machine Learning çalışma alanı** çalışma alanlarınızın listesini görüntüleyin. 

   ![Azure Machine Learning çalışma alanı arayın](media/how-to-manage-workspace/allservices-search.PNG)

1. Çalışma alanı bulunamadı listesinde arayın. Bağlı aboneliği, kaynak grupları ve konumlarını filtreleyebilirsiniz.  

   ![Azure Machine Learning çalışma alanı listesi](media/how-to-manage-workspace/allservices_view_workspace.PNG)

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
