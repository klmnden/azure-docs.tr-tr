---
title: "Python kullanarak Azure Table storage'ı kullanmaya başlama | Microsoft Docs"
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/08/2018
ms.author: mimig
ms.openlocfilehash: 2c8c7dc6d3bdb6ba34818d7e36739297cffbe2d2
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="get-started-with-azure-table-storage-using-python"></a>Python kullanarak Azure Table storage'ı kullanmaya başlama

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Tablo Depolama, yapılandırılmış NoSQL verilerini bulutta depolayan ve şemasız bir tasarım ile anahtar/öznitelik deposu sağlayan bir hizmettir. Table Storage şemasız olduğu için uygulamanızın ihtiyaçları geliştikçe verilerinizi kolayca uyarlayabilirsiniz. Tablo Depolama verilerine erişim, çoğu uygulama türü için hızlı ve ekonomik olmanın yanı sıra benzer veri hacimleri için geleneksel SQL’e kıyasla genellikle daha düşük maliyetlidir.

Web uygulamaları için kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetiniz için gerekli olan tüm diğer meta veri türleri gibi esnek veri kümelerini Tablo Depolama ile depolayabilirsiniz. Bir tabloda istediğiniz kadar varlık depolayabilirsiniz ve bir depolama hesabı kapasite limitini dolduracak kadar tablo içerebilir.

### <a name="about-this-tutorial"></a>Bu öğretici hakkında
Bu öğretici nasıl kullanılacağını gösterir [Python için Azure Cosmos DB tablo SDK](https://pypi.python.org/pypi/azure-cosmosdb-table/) ortak Azure Table depolama senaryolarında. Azure Cosmos DB için kullanılır, ancak hem Azure Cosmos DB ile çalışır ve Azure tablo depolama, her hizmetin yalnızca benzersiz bir uç nokta sahip SDK adını gösterir. Bu senaryolar gösterilmektedir Python örnekler kullanarak keşfedilen nasıl yapılır:
* Oluşturma ve tabloları silin
* INSERT ve sorgu varlıklar
* Varlıkları değiştirme

Çalışırken senaryoları aracılığıyla Bu öğreticide, başvurmak isteyebilirsiniz [Python API Başvurusu için Azure Cosmos DB SDK](https://azure.github.io/azure-cosmosdb-python/).

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi başarıyla tamamlamak için aşağıdakiler gerekir:

- [Python](https://www.python.org/downloads/) 2.7, 3.3, 3.4, 3.5 veya 3.6
- [Python için Azure Cosmos DB tablo SDK 1.01](https://pypi.python.org/pypi/azure-cosmosdb-table/). Bu SDK, Azure Table depolama ve Azure Cosmos DB tablo API ile bağlanır.
- [Azure depolama hesabı](https://docs.microsoft.com/en-us/azure/storage/common/storage-create-storage-account#create-a-storage-account) veya [Azure Cosmos DB hesabı](https://azure.microsoft.com/en-us/try/cosmosdb/)

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Bir Azure hizmet hesabı oluşturma

Azure Table storage veya Azure Cosmos DB kullanarak tabloları ile çalışabilirsiniz. Okuyarak Hizmetleri arasındaki farklar hakkında daha fazla bilgiyi [tablo teklifleri](table-introduction.md#table-offerings). Hizmeti için kullanacağınız bir hesap oluşturmanız gerekir. 

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
İlk Azure depolama hesabınızı oluşturmanın en kolay yolu [Azure Portalı](https://portal.azure.com)’nı kullanmaktır. Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Kullanarak bir Azure depolama hesabı oluşturabilirsiniz [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) veya [Azure CLI](../storage/common/storage-azure-cli.md).

Şu anda bir depolama hesabı oluşturmayı tercih ediyorsanız, yerel bir ortamda kodunuzu çalıştırıp sınamak için Azure Storage öykünücüsü de kullanabilirsiniz. Daha fazla bilgi için bkz. [Geliştirme ve Sınama için Azure Storage Öykünücüsünü Kullanma](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Bir Azure Cosmos DB tablo API hesabı oluşturma

Bir Azure Cosmos DB tablo API hesap oluşturma ile ilgili yönergeler için bkz: [tablo API hesabı oluşturma](create-table-dotnet.md#create-a-database-account).

## <a name="install-the-azure-cosmos-db-table-sdk-for-python"></a>Python için Azure Cosmos DB tablosu SDK yükleme

Bir depolama hesabı oluşturduktan sonra sonraki adımınız yüklemektir [Python için Microsoft Azure Cosmos DB tablo SDK](https://pypi.python.org/pypi/azure-cosmosdb-table/). SDK'sını yükleme hakkında daha fazla bilgi için başvurmak [README.rst](https://github.com/Azure/azure-cosmosdb-python/blob/master/azure-cosmosdb-table/README.rst) Cosmos DB tablo SDK'sı github'da dosyasına Python depo.

## <a name="import-the-tableservice-and-entity-classes"></a>TableService ve varlık sınıflarını içe aktarın

Python Azure tablo hizmetinde varlıklarda çalışmak için kullandığınız [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) ve [varlık] [ py_Entity] sınıfları. Bu kod ilk her ikisi de içe aktarmak için Python dosyanıza ekleyin:

```python
from azure.cosmosdb.table.tableservice import TableService
from azure.cosmosdb.table.models import Entity
```

## <a name="connect-to-azure-table-service"></a>Azure Table hizmetine bağlanmak

Azure depolama tablosu hizmetine bağlanmak için oluşturma bir [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) nesnesi ve depolama hesabı adı ve hesap anahtarınızı geçirin. Değiştir `myaccount` ve `mykey` , hesap adı ve anahtarınız ile.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')
```

## <a name="connect-to-azure-cosmos-db"></a>Azure Cosmos DB’ye bağlanma

Azure Cosmos Veritabanına bağlanmak için birincil bağlantı dizenizi Azure Portal'dan kopyalayın ve oluşturma bir [TableService](https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html) kopyalanan bağlantı dizenizi kullanarak nesnesi:

```python
table_service = TableService(connection_string='DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;TableEndpoint=myendpoint;)
```

## <a name="create-a-table"></a>Bir tablo oluşturma

Çağrı [create_table] [ py_create_table] tablo oluşturmak için.

```python
table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Bir varlık eklemek için önce varlığı temsil eder ve ardından nesneyi geçirmek bir nesne oluşturma [TableService.insert_entity yöntemi][py_TableService]. Varlık nesnesi bir sözlük veya türünde bir nesne olabilir [varlık][py_Entity]ve, varlığın özellik adlarını ve değerlerini tanımlar. Her varlık gerekli içermelidir [PartitionKey ve RowKey](#partitionkey-and-rowkey) varlık için tanımladığınız diğer özellikleri ek özellikler.

Bu örnek, bir sözlük nesnesi oluşturur. bir varlığı temsil eden daha sonra geçirir kendisine [insert_entity] [ py_insert_entity] yöntemi tabloya eklemek için:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Bu örnekte bir [varlık] [ py_Entity] nesnesi, ardından buna ileten [insert_entity] [ py_insert_entity] yöntemi tabloya eklemek için:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey ve RowKey

Her ikisini de belirtmelisiniz bir **PartitionKey** ve **RowKey** özelliği her varlık için. Bunlar, varlıklarınızı'nın benzersiz tanımlayıcıları olarak birbirine bir varlığın birincil anahtarı oluşturur. Bu değerleri yalnızca bu özellikleri dizine olduğundan başka bir varlık özellikleri sorgulayabilirsiniz çok daha hızlı kullanarak sorgulayabilirsiniz.

Tablo hizmeti kullandığı **PartitionKey** tablo varlıkları akıllıca depolama düğümleri arasında dağıtılacak. Aynı olan varlıkları **PartitionKey** aynı düğümde depolanır. **RowKey** varlığın ait bölüm içinde benzersiz kimliğidir.

## <a name="update-an-entity"></a>Bir varlığı güncelleştirir

Bir varlığın özellik değerlerinin tümü güncelleştirmek için arama [update_entity] [ py_update_entity] yöntemi. Bu örnek, var olan bir varlığı güncelleştirilmiş bir sürümle gösterilmektedir:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Güncelleştirilen varlık zaten mevcut değilse, güncelleştirme işlemi başarısız olur. Bir varlık depolamak istiyorsanız, bunu var olup olmadığını, kullanın [insert_or_replace_entity][py_insert_or_replace_entity]. Aşağıdaki örnekte, ilk çağrıda varolan varlık yerini alır. İkinci çağrı hiçbir varlık belirtilen PartitionKey ile bu yana yeni bir varlık ekler ve RowKey tablosunda yok.

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> [Update_entity] [ py_update_entity] yöntemi, tüm özellikler ve var olan bir varlık özellikleri kaldırmak için de kullanabilirsiniz olan bir varlığa değerlerini değiştirir. Kullanabileceğiniz [merge_entity] [ py_merge_entity] tamamen varlık değiştirmeden yeni veya değiştirilmiş özellik değerleri ile var olan bir varlığı güncelleştirmek için yöntem.

## <a name="modify-multiple-entities"></a>Birden çok varlık değiştirme

Tablo hizmeti tarafından bir istek atomik işlenmesini sağlamak için birden çok işlemlerinde birlikte bir toplu gönderebilirsiniz. İlk olarak, kullanın [TableBatch] [ py_TableBatch] birden çok işlem tek bir toplu eklemek için sınıfı. Ardından, çağrı [TableService][py_TableService].[ commit_batch] [ py_commit_batch] atomik bir işlem işlemlerinde göndermek için. Toplu işlemde değiştirilecek tüm varlıklar aynı bölümde olmalıdır.

Bu örnek iki varlık bir toplu işlemde toplar:

```python
from azure.cosmosdb.table.tablebatch import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Toplu da İçerik Yöneticisi sözdizimiyle kullanılabilir:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Bir varlık için sorgu

Bir tablodaki bir varlık için sorgulamak üzere kendi PartitionKey ve RowKey için geçirmek [TableService][py_TableService].[ get_entity] [ py_get_entity] yöntemi.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Varlık kümesi sorgulama

Varlık kümesi için bir filtre dizesi ile sağlayarak Sorgulayabileceğiniz **filtre** parametresi. Bu örnekte, tüm görevler PartitionKey üzerinde filtre uygulayarak Seattle bulur:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama

Sorgudaki her bir varlık için döndürülen hangi özelliklerin de kısıtlayabilirsiniz. Adlı bu teknik *projeksiyon*, bant genişliğini azaltır ve özellikle büyük varlıklar veya sonuç kümesi için sorgu performansını iyileştirebilir. Kullanım **seçin** parametre ve istediğiniz özellik adlarını istemciye döndürülen geçirin.

Aşağıdaki kod sorguda yalnızca varlıklar açıklamalarını tablodaki döndürür.

> [!NOTE]
> Aşağıdaki kod parçacığını yalnızca Azure depolama karşı çalışır. Depolama öykünücüsü tarafından desteklenmiyor.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Bir varlığı silme

Bir varlığı geçirerek silme kendi **PartitionKey** ve **RowKey** için [delete_entity] [ py_delete_entity] yöntemi.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo veya varlıkları hiçbirini artık ihtiyacınız varsa, çağrı [delete_table] [ py_delete_table] Azure depolama biriminden tabloyu kalıcı olarak silmek için yöntem.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Sonraki adımlar

* [SSS - API tabloyla geliştirin](https://docs.microsoft.com/en-us/azure/cosmos-db/faq#develop-with-the-table-api)
* [Python API Başvurusu için Azure Cosmos DB SDK](https://azure.github.io/azure-cosmosdb-python/)
* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md): görsel olarak Windows, macOS ve Linux Azure Storage ile çalışmak için ücretsiz, platformlar arası bir uygulama.
* [Python Visual Studio (Windows) ile çalışma](https://docs.microsoft.com/en-us/visualstudio/python/overview-of-python-tools-for-visual-studio)

[py_commit_batch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_create_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_delete_table]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_Entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.models.html
[py_get_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_insert_or_replace_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableService]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_TableBatch]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tablebatch.html
[py_merge_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
[py_update_entity]: https://azure.github.io/azure-cosmosdb-python/ref/azure.cosmosdb.table.tableservice.html
