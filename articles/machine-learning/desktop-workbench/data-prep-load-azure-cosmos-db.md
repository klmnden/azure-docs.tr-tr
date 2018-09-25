---
title: Azure cosmos DB veri kaynağı olarak Azure Machine Learning workbench'te bağlanma | Microsoft Docs
description: Bu belge, Azure Cosmos DB, Azure Machine Learning Workbench ile bağlanmak nasıl bir örnek sağlar.
services: machine-learning
author: cforbe
ms.author: cforbe
manager: mwinkle
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 09/11/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 9c4ea529e8ca6dbb9b7321dc24468fad93435705
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948205"
---
# <a name="connecting-to-azure-cosmos-db-as-a-data-source"></a>Veri kaynağı olarak Azure Cosmos DB'ye bağlanma

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)] 


Bu makale, bir python içerir, Azure Machine Learning workbench'te Cosmos DB'ye bağlanmak örnek olanak sağlar.

## <a name="load-azure-cosmos-db-data-into-data-preparation"></a>Veri hazırlama ile Azure Cosmos DB veri yükleme

Yeni bir komut dosyası tabanlı veri akışı oluşturun ve ardından Azure Cosmos DB'den verileri yüklemek için aşağıdaki betiği kullanın. 

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
