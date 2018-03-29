---
title: Verileri için Azure Blob depolama biriminden getirip | Microsoft Docs
description: Azure Blob Depolamadan/Depolamaya Veri Taşıma
services: machine-learning,storage
documentationcenter: ''
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 16988d7dd466eb0f893dc0b1f8fdc6511115d0a7
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Azure Blob Storage gelen ve veri taşıma
[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

Hangi sizin için en iyi bir yöntemdir, senaryoya bağlıdır. [Senaryoları Azure Machine Learning, gelişmiş analizler için](plan-sample-scenarios.md) makale gelişmiş analizler işleminde kullanılan veri bilimi iş akışları çeşitli için ihtiyacınız olan kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure blob depolama tam bir giriş için bkz [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) için: 

* oluşturun ve veriler Azure blob depolama alanından yükler ardışık zamanlama 
* yayımlanan bir Azure Machine Learning web hizmetine geçirmek, 
* Tahmine dayalı analiz sonuçlarını almak ve 
* Sonuçları depolama alanına yükleme. 

Daha fazla bilgi için bkz: [Azure Data Factory ve Azure Machine Learning kullanarak Tahmine dayalı işlem hatlarını oluşturmak](../../data-factory/v1/data-factory-azure-ml-batch-execution-activity.md).

## <a name="prerequisites"></a>Önkoşullar
Bu belge, Azure aboneliği, bir depolama hesabı ve o hesap için karşılık gelen depolama anahtarı sahip olduğunuzu varsayar. Karşıya yükleme/veri yüklemeden önce Azure depolama hesabı adı ve hesap anahtarınızın bilmeniz gerekir.

* Bir Azure aboneliği ayarlamak için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler ve hesabı ve anahtarı bilgilerini almak için bkz: [Azure storage hesapları hakkında](../../storage/common/storage-create-storage-account.md).

