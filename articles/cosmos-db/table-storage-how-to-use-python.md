---
title: Azure Table storage Python ile kullanma | Microsoft Docs
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.openlocfilehash: 0c46f04786ba4b62bd7ca22c5e25643123e6e136
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-in-python"></a>Tablo depolama Python içinde kullanma

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Bu kılavuz size nasıl Python kullanarak Azure Table depolama senaryoları gerçekleştirileceğini gösterir [Python için Microsoft Azure depolama SDK](https://github.com/Azure/azure-storage-python). Kapsamdaki senaryolar oluşturma ve bir tablo, silme ve ekleme ve varlıkları sorgulama içerir.

Çalışırken senaryoları aracılığıyla Bu öğreticide, başvurmak isteyebilirsiniz [Python API Başvurusu için Azure depolama SDK'sı](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a>Python için Azure depolama SDK'sını yükleyin

Bir depolama hesabı oluşturduktan sonra sonraki adımınız yüklemektir [Python için Microsoft Azure depolama SDK](https://github.com/Azure/azure-storage-python). SDK'sını yükleme hakkında daha fazla bilgi için başvurmak [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) dosyasında depolama SDK'sı Python deposu için github'da.

## <a name="create-a-table"></a>Bir tablo oluşturma

Python Azure tablo hizmetinde çalışmak için içeri aktarmanız gerekir [TableService] [ py_TableService] modülü. Tablo varlıklarla çalışmaya olduğundan, ayrıca gerekir [varlık] [ py_Entity] sınıfı. Bu kod ilk her ikisi de içe aktarmak için Python dosyanıza ekleyin:

```python
from azure.storage.table import TableService, Entity
```

Oluşturma bir [TableService] [ py_TableService] nesnesi, depolama hesabı adı ve hesap anahtarınızı geçirme. Değiştir `myaccount` ve `mykey` hesap adı ve anahtar ve çağrı [create_table] [ py_create_table] Azure Storage'da bir tablo oluşturmak için.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme

Bir varlık eklemek için önce varlığı temsil eder ve ardından nesneyi geçirmek bir nesne oluşturma [TableService][py_TableService].[ insert_entity] [ py_insert_entity] yöntemi. Varlık nesnesi bir sözlük veya türünde bir nesne olabilir [varlık][py_Entity]ve, varlığın özellik adlarını ve değerlerini tanımlar. Her varlık gerekli içermelidir [PartitionKey ve RowKey](#partitionkey-and-rowkey) varlık için tanımladığınız diğer özellikleri ek özellikler.

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
from azure.storage.table import TableBatch
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

Bir varlığın PartitionKey ve RowKey için geçirerek silme [delete_entity] [ py_delete_entity] yöntemi.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Bir tablo silme

Bir tablo veya varlıkları hiçbirini artık ihtiyacınız varsa, çağrı [delete_table] [ py_delete_table] Azure depolama biriminden tabloyu kalıcı olarak silmek için yöntem.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure depolama için SDK'sı Python API Başvurusu](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Azure depolama için Python SDK'sı](https://github.com/Azure/azure-storage-python)
* [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/)
* [Microsoft Azure Storage Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md): görsel olarak Windows, macOS ve Linux Azure Storage ile çalışmak için ücretsiz, platformlar arası bir uygulama.

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch
