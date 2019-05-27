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
ms.date: 05/10/2019
ms.custom: seodec18
ms.openlocfilehash: 7f0806a1d68cd2cede1ae51f0a50a8125c1e7c8b
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "66016536"
---
# <a name="create-and-manage-azure-machine-learning-service-workspaces"></a>Oluşturma ve Azure Machine Learning hizmeti çalışma alanlarını yönetme

Bu makalede, oluşturun, görüntüleyin, silin ve [ **Azure Machine Learning hizmeti çalışma alanları** ](concept-workspace.md) Azure Portalı'nda [Azure Machine Learning hizmeti](overview-what-is-azure-ml.md).  Ayrıca oluşturabilir ve çalışma alanlarını silen [CLI kullanarak](reference-azure-machine-learning-cli.md), [Python kodu ile](https://aka.ms/aml-sdk) veya [VS Code uzantısı aracılığıyla](how-to-vscode-tools.md#get-started-with-azure-machine-learning).

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
