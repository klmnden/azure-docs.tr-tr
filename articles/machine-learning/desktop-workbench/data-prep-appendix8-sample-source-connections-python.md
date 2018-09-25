---
title: Örnek ek kaynak veri bağlantıları ile Azure Machine Learning veri hazırlama olası | Microsoft Docs
description: Bu belge, Azure Machine Learning veri hazırlama ile olası kaynak veri bağlantıları örnekleri sunmaktadır
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ROBOTS: NOINDEX
ms.openlocfilehash: ed0e6bc4c506535fcb679fc2b4015a61274a77f3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967702"
---
# <a name="sample-of-custom-source-connections-python"></a>Özel kaynak bağlantıları (Python) örneği 

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Data.world'den veri yükleme

### <a name="prerequisites"></a>Önkoşullar

#### <a name="register-yourself-at-dataworld"></a>Kendinize data.world kaydetme
API belirteci data.world Web sitesinden ihtiyacınız vardır.

#### <a name="install-dataworld-library"></a>Data.World kitaplığını yükle

Azure Machine Learning Workbench'te komut satırı arabirimi seçerek açın **dosya** > **açık komut satırı arabirimi**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Ardından, çalıştırma `dw configure` komut satırında, sizden belirtecinizin. Ne zaman bir .dw belirtecinizi girin / directory giriş dizininizin ve belirtecinizi oluşturulan burada depolanır.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Artık data.world kitaplıkları içeri aktarma mümkün olması gerekir.

#### <a name="load-data-into-data-preparation"></a>Veri hazırlama ile veri yükleme

Dönüşüm veri akışı (betik) dönüşüm oluşturun. Ardından verileri Data.world'den yüklemek için aşağıdaki betiği kullanın.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Azure Cosmos DB veri kaynağı bağlantısı olarak
Azure Cosmos DB'nin bir veri bağlantısı olarak Örneğin, okuma [yük Azure Cosmos DB veri kaynağı bağlantısı olarak](data-prep-load-azure-cosmos-db.md)
