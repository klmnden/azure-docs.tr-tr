---
title: "Azure Machine Learning çalışma ekranındaki Azure Cosmos DB veri kaynağı olarak bağlanma | Microsoft Docs"
description: "Bu belge, Azure Cosmos DB Azure Machine Learning çalışma ekranı aracılığıyla bağlanma hakkında bir örnek sağlar."
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: d36b394a528dc4bc1b6e0a9e0e5dbde728cbee1b
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2018
---
# <a name="connecting-to-azure-cosmos-db-as-a-data-source"></a>Bir veri kaynağı olarak Azure Cosmos Veritabanına bağlanma
Bu makalede bir python içeren örnek Azure Machine Learning çalışma ekranı Cosmos DB'de bağlanmanıza olanak sağlar.

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Veri hazırlama Azure Cosmos DB veri yükleme

Yeni bir komut dosyası tabanlı veri akışı oluşturun ve ardından Azure Cosmos DB'den verileri yüklemek için aşağıdaki komut dosyasını kullanın. 

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

## <a name="other-data-source-connections"></a>Diğer veri kaynağı bağlantıları
Diğer örnekler için okuma [örnek ek kaynak veri bağlantıları](data-prep-appendix8-sample-source-connections-python.md)
