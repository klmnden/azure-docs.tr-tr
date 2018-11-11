---
title: İçin ve Azure Blob depolamadan/depolamaya veri taşıma | Microsoft Docs
description: İçin ve Azure Blob depolamadan/depolamaya veri taşıma
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 7d0111b22df45577fccc3f4491f375ddd2e8b40f
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344476"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>İçin ve Azure Blob depolamadan/depolamaya veri taşıma

Team Data Science Process veri veya alınan farklı depolama ortamları işlenen ya da işlemin her aşamasında en uygun şekilde analiz için çeşitli yüklenmiş olmasını gerektirir.
Aşağıdaki makaleleri ve farklı teknolojileri kullanarak Azure Blob depolama alanından veri taşıma konusunda açıklanmaktadır.

* [Azure Depolama Gezgini](move-data-to-azure-blob-using-azure-storage-explorer.md)
* [AzCopy](move-data-to-azure-blob-using-azcopy.md)
* [Python](move-data-to-azure-blob-using-python.md)
* [SSIS](move-data-to-azure-blob-using-ssis.md)

Hangi sizin için en iyi bir yöntemdir, senaryoya bağlıdır. [Azure Machine learning'de Gelişmiş analiz senaryoları](plan-sample-scenarios.md) makale Gelişmiş analiz işleminde kullanılan veri bilimi iş akışları çeşitli için ihtiyacınız olan kaynakları belirlemenize yardımcı olur.

> [!NOTE]
> Azure blob depolama için bir tam giriş için başvurmak [Azure Blob Temelleri](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) ve [Azure Blob hizmeti](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Alternatif olarak, kullandığınız [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) için: 

* oluşturun ve Azure blob depolamadan veri yükleyen işlem hattı zamanlama 
* yayımlanan bir Azure Machine Learning web hizmeti ile geçirin, 
* Tahmine dayalı analiz sonuçlarını almak ve 
* Sonuçları depolama alanına yükleyin. 

Daha fazla bilgi için [Azure Data Factory ve Azure Machine Learning kullanarak öngörülebilir komut zincirleri oluşturma](../../data-factory/transform-data-using-machine-learning.md).

## <a name="prerequisites"></a>Önkoşullar
Bu belge, bir Azure aboneliği, bir depolama hesabı ve karşılık gelen depolama anahtarını ilgili hesabın sahibi olduğunuzu varsayar. Karşıya yükleme/veri indirmeden önce Azure depolama hesabı adını ve hesap anahtarınızı bilmeniz gerekir.

* Bir Azure aboneliğini ayarlama hakkında bilgi için bkz: [ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/).
* Bir depolama hesabı oluşturma hakkında yönergeler için ve hesap ve anahtar bilgilerini almak için bkz: [Azure depolama hesapları hakkında](../../storage/common/storage-create-storage-account.md).

