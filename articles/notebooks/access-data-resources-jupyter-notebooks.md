---
title: Azure üzerinde Jupyter Notebook erişim veri kaynakları
description: Jupyter not defteri erişimi dosyalara, REST API'leri, veritabanları ve farklı Azure depolama kaynaklarını nasıl.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: ee867303-a5e5-4686-b2da-8a0108247d18
ms.service: azure-notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: 14a4191612a5d42836ae4be3ff902ca47a6b06d4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60634360"
---
# <a name="access-cloud-data-in-a-notebook"></a>Bir not defteri bulut verilerine erişim

İlginç bir Jupyter not defteri iş verileri gerektiriyor. Aslında, dizüstü bilgisayarlar, lifeblood verilerdir.

Verilerle yapabilecekleriniz [projeye veri dosyaları alma](work-with-project-data-files.md)olsa bile gibi komutları kullanarak `curl` gelen içinde doğrudan bir dosyayı indirmek için bir not defteri. Ancak, REST API'ler, ilişkisel veritabanları gibi dosya olmayan kaynaklardan kullanılabilir daha kapsamlı verilerle çalışın ve bulut depolama gibi Azure tabloları gerektiğini, olasıdır.

Bu makalede, bu farklı seçenekler kısaca özetler. Veri erişim en iyi eylem görüneceğinden, çalıştırılabilir kod içinde bulabilirsiniz [Azure not defterleri örnekleri - veri erişimi](https://github.com/Microsoft/AzureNotebooks/blob/master/Samples/Access%20your%20data%20in%20Azure%20Notebooks.ipynb).

## <a name="rest-apis"></a>REST API'leri

Genel olarak bakıldığında, yüksek miktarda veri Internet'ten kullanılabilir dosyaları üzerinden değil, ancak REST API'leri aracılığıyla erişilir. Neyse ki, bir not defteri hücre istediğiniz herhangi bir kod içerebileceğinden, JSON veri istekleri göndermek ve almak için kod kullanabilirsiniz. Ardından, JSON, bir pandas dataframe gibi kullanmak istediğiniz her biçimine dönüştürebilirsiniz.

REST API kullanarak verilere erişmek için başka bir uygulamada kullandığınız bir not defterinin kod hücreleri aynı kodu kullanın. İstekleri kitaplığını kullanarak genel yapısı şu şekildedir:

```python
import pandas
import requests

# New York City taxi data for 2014
data_url = 'https://data.cityofnewyork.us/resource/gkne-dk5s.json'

# General data request; include other API keys and credentials as needed in the data argument
response = requests.get(data_url, data={"limit" : "20"})

if response.status_code == 200:
    dataframe_rest2 = pandas.DataFrame.from_records(response.json())
    print(dataframe_rest2)
```

## <a name="azure-sql-databases"></a>Azure SQL veritabanları

SQL Server veritabanlarını pyodbc veya pymssql kitaplıkları yardıma erişebilir.

[Bir Azure SQL veritabanını sorgulamak için Python kullanma](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-python) AdventureWorks verilerini içeren bir veritabanı oluşturma hakkında yönergeler verir ve bu verileri Sorgulama işlemi gösterilmektedir. Bu makalede örnek not defterinde aynı kod gösterilmektedir.

## <a name="azure-storage"></a>Azure Storage

Azure depolama, ilişkisel olmayan depolama, sahip olduğunuz veriler ve nasıl erişim sağlaması gereken türüne bağlı olarak birkaç farklı türde sağlar:

- Tablo depolama: toplanan algılayıcı günlükler, tanılama günlükleri ve benzeri gibi tablo biçimindeki veriler için düşük maliyetli, yüksek hacimli depolama sağlar.
- BLOB Depolama: her türden veriyi dosya benzeri depolama sağlar.

Örnek Not Defteri hem tabloları ve bloblar için salt okunur erişime izin vermek için paylaşılan erişim imzası kullanma dahil olmak üzere, bloblar ile çalışma gösterir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Cosmos DB tamamen dizinli bir NoSQL deposu için JSON belgelerini sağlar). Aşağıdaki makaleler birkaç python'dan Cosmos DB ile çalışmak için farklı yollar sunar:

- [Python ile uygulama oluşturma bir SQL API'si](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-python)
- [MongoDB için Azure Cosmos DB'nin API'si ile bir Flask uygulaması derleme](https://docs.microsoft.com/azure/cosmos-db/create-mongodb-flask)
- [Python ve Gremlin API kullanarak bir grafik veritabanı oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-graph-python)
- [Python ve Azure Cosmos DB ile Cassandra uygulaması derleme](https://docs.microsoft.com/azure/cosmos-db/create-cassandra-python)
- [API uygulaması, Python ve Azure Cosmos DB ile tablo oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-table-python)

Cosmos DB ile çalışırken kullanabileceğiniz [azure cosmosdb tablo](https://pypi.org/project/azure-cosmosdb-table/) kitaplığı.

## <a name="other-azure-databases"></a>Diğer Azure veritabanları

Azure, bir dizi kullanabileceğiniz diğer veritabanı türleri sağlar. Aşağıdaki makaleleri veritabanlarınızdaki Python'dan erişmeye yönelik rehberlik sağlar:

- [PostgreSQL için Azure veritabanı: Bağlanmak ve veri sorgulamak için Python kullanma](https://docs.microsoft.com/azure/postgresql/connect-python)
- [Hızlı Başlangıç: Python ile Azure Redis Cache kullanma](https://docs.microsoft.com/azure/redis-cache/cache-python-get-started)
- [MySQL için Azure veritabanı: Bağlanmak ve veri sorgulamak için Python kullanma](https://docs.microsoft.com/azure/mysql/connect-python)
- [Azure Data Factory](https://azure.microsoft.com/services/data-factory/)
  - [Azure Data Factory Kopyalama Sihirbazı](https://azure.microsoft.com/updates/code-free-copy-wizard-for-azure-data-factory/)

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: Proje veri dosyalarıyla çalışma](work-with-project-data-files.md)
