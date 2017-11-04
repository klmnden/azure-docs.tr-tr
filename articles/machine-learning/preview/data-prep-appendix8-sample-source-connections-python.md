---
title: "Örnek ek kaynak veri bağlantıları Azure Machine Learning ile veri hazırlama olası | Microsoft Docs"
description: "Bu belgede Azure Machine Learning ile veri hazırlama olası kaynak veri bağlantıları örnekleri kümesi sağlar"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 9aaf4329b25cb189146949afed89cf15619ba592
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-custom-source-connections-python"></a>Özel kaynak bağlantıları (Python) örneği 
Bu ekte okumadan önce okuma [Python genişletilebilirlik genel bakış](data-prep-python-extensibility-overview.md).

## <a name="load-data-from-dataworld"></a>Data.world yük verileri

### <a name="prerequisites"></a>Ön koşullar

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

Yeni bir komut dosyası tabanlı veri akışı oluşturun. Ardından data.world verileri yüklemek için aşağıdaki komut dosyasını kullanın.

```python
#paths = df['Path'].tolist()

import datadotworld as dw

#Load the dataset.
lds = dw.load_dataset('data-society/the-simpsons-by-the-data')

#Load specific data frame from the dataset.
df = lds.dataframes['simpsons_episodes']

```

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Veri hazırlama Azure Cosmos DB veri yükleme

Yeni bir komut dosyası tabanlı veri akışı oluşturun ve ardından Azure Cosmos DB'den verileri yüklemek için aşağıdaki komut dosyasını kullanın. (Kitaplıkları önce yüklenmesi gerekir. Daha fazla bilgi için şu bağlantı önceki başvurusu belgesine bakın.)

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

import pandas as pd

config = { 
    'ENDPOINT': '<Endpoint>',
    'MASTERKEY': '<Key>',
    'DOCUMENTDB_DATABASE': '<DBName>',
    'DOCUMENTDB_COLLECTION': '<collectionname>'
};

# Initialize the Python DocumentDB client.
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})

# Read databases and take first since id should not be duplicated.
db = next((data for data in client.ReadDatabases() if data['id'] == config['DOCUMENTDB_DATABASE']))

# Read collections and take first since id should not be duplicated.
coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config['DOCUMENTDB_COLLECTION']))

docs = client.ReadDocuments(coll['_self'])

df = pd.DataFrame(list(docs))
```
