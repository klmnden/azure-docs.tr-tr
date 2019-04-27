---
title: Oluşturma ve çalışma alanlarını yönetme
titleSuffix: Azure Machine Learning service
description: Oluşturun, görüntüleyin ve Azure Machine Learning hizmeti çalışma alanları Azure portalında silme hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: shipatel
author: shivp950
ms.date: 09/24/2018
ms.custom: seodec18
ms.openlocfilehash: e65f739a9641181381205c7255d0472325e8055c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60819596"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Oluşturma ve Azure Machine Learning hizmeti çalışma alanlarını yönetme

Bu makalede, oluşturun, görüntüleyin, silin ve [ **Azure Machine Learning hizmeti çalışma alanları** ](concept-azure-machine-learning-architecture.md#workspace) Azure Portalı'nda [Azure Machine Learning hizmeti](overview-what-is-azure-ml.md).  Ayrıca oluşturabilir ve çalışma alanlarını silen [CLI kullanarak](reference-azure-machine-learning-cli.md) veya [Python kodu ile](https://aka.ms/aml-sdk).

## <a name="create-a-workspace"></a>Çalışma alanı oluşturma 

Bir çalışma alanı oluşturmak için bir Azure aboneliğinizin olması gerekir. Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

## <a name="view"></a>Bir çalışma alanını görüntüle

1. Sol üst köşede Portalı'nın, seçin **tüm hizmetleri**. 

1. İçinde **tüm hizmetleri** filtre alanda, türü **machine learning hizmeti**.  

1. Seçin **Machine Learning hizmeti çalışma alanları**.

   ![Azure Machine Learning hizmeti çalışma alanında Ara](media/how-to-manage-workspace/all-services.png)

1. Çalışma alanı bulunamadı listesinde arayın. Bağlı aboneliği, kaynak grupları ve konumlarını filtreleyebilirsiniz.  

1. Özelliklerini görüntülemek istediğiniz çalışma alanını seçin.
   ![Çalışma alanı özellikleri](media/how-to-manage-workspace/allservices_view_workspace_full.PNG)

## <a name="delete-a-workspace"></a>Çalışma alanını silme

Silmek istediğiniz çalışma alanının üstündeki Sil düğmesini kullanın.

  ![Sil düğmesi](media/how-to-manage-workspace/delete-workspace.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir çalışma alanı oluşturmak, eğitmek ve modeller Azure Machine Learning hizmeti ile dağıtmak için nasıl kullanılacağını öğrenmek için eksiksiz bir öğreticiyi izleyin.

> [!div class="nextstepaction"]
> [Öğretici: Modelleri eğitme](tutorial-train-models-with-aml.md)
