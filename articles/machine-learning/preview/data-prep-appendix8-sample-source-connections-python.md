---
title: "Örnek ek kaynak veri bağlantıları Azure Machine Learning ile veri hazırlama olası | Microsoft Docs"
description: "Bu belgede Azure Machine Learning ile veri hazırlama olası kaynak veri bağlantıları örnekleri kümesi sağlar"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 66c356d6d42254e7443b645bff3393daca67012b
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="sample-of-custom-source-connections-python"></a>Özel kaynak bağlantıları (Python) örneği 
Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Data.world yük verileri

### <a name="prerequisites"></a>Önkoşullar

#### <a name="register-yourself-at-dataworld"></a>Kendiniz data.world kaydetme
Bir API belirteci data.world Web sitesinden gerekir.

#### <a name="install-dataworld-library"></a>Data.World kitaplığını yükle

Azure Machine Learning çalışma ekranı komut satırı arabirimini seçerek açın **dosya** > **açık komut satırı arabirimi**.

```console
pip install git+git://github.com/datadotworld/data.world-py.git
```

Ardından, çalıştırın `dw configure` komut satırında, sizden belirtecinizi için. Ne zaman bir .dw belirtecinizi girin veya dizin giriş dizininize ve belirtecinizi oluşturulur orada saklanır.

```
API token (obtained at: https://data.world/settings/advanced): <enter API token here>
```
Artık data.world kitaplıkları içeri aktarma mümkün olması gerekir.

#### <a name="load-data-into-data-preparation"></a>Veri hazırlama içine veri yükleme

Bir dönüştürme veri akışı (komut) dönüştürme oluşturun. Ardından data.world verileri yüklemek için aşağıdaki komut dosyasını kullanın.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```
## <a name="azure-cosmos-db-as-a-data-source-connection"></a>Bir veri kaynağı bağlantısı olarak Azure Cosmos DB
Bir veri bağlantısı olarak Azure Cosmos DB örneği için okuma [yük Azure Cosmos DB kaynak veri bağlantısı olarak](data-prep-load-azure-cosmos-db.md)
