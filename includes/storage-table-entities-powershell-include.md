---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: e8c5bf8e3c4cd63b7eec278c480527e95455140d
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50166055"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tablo varlıklarını yönetme

Bir tablonuz olduğuna göre varlıklar veya tablosundaki satırları yönetme göz atalım. 

Bir varlığın 3 sistem özelliği dahil olmak üzere, en fazla 255 özellikleri olabilir: **PartitionKey**, **RowKey**, ve **zaman damgası**. Ekleme ve güncelleştirme değerlerini sorumludur **PartitionKey** ve **RowKey**. Sunucu değeri yönetir **zaman damgası**, hangi değiştirilemez. Birlikte **PartitionKey** ve **RowKey** tablo içindeki her bir varlık benzersiz olarak tanımlanabilmesi.

* **PartitionKey**: Varlık depolanan bölüm belirler.
* **RowKey**: varlığı bölüm içinde benzersiz şekilde tanımlar.

Bir varlık için özel en çok 252 özellik tanımlayabilir. 

### <a name="add-table-entities"></a>Tablo varlık ekleme

Varlıkları kullanarak bir tablo ekleyin **Ekle StorageTableRow**. Bu örnekler, bölüm anahtarı değerleri "Bölüm1" ve "bölüm2" ve satır anahtarı için durum kısaltmalar eşit kullanın. Her varlık özellikleri, kullanıcı adı ve kullanıcı kimliği ' dir. 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-StorageTableRow `
    -table $storageTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Tablo varlıkları sorgulama

Bir tablodaki varlıkları sorgulamak için çeşitli yollar vardır.

#### <a name="retrieve-all-entities"></a>Tüm varlıkları alma

Tüm varlıkları almak için kullanın **Get-AzureStorageTableRowAll**.

```powershell
Get-AzureStorageTableRowAll -table $storageTable | ft
```

Bu komut için aşağıdaki tabloda benzer sonuçlar verir:

| Kullanıcı Kimliği | kullanıcı adı | bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |
| 2 | Jessie | Bölüm2 | NM |
| 4 | Steven | Bölüm2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Belirli bir bölüm için varlıkları alma

Belirli bir bölümdeki tüm varlıkları almak için kullanın **Get-AzureStorageTableRowByPartitionKey**.

```powershell
Get-AzureStorageTableRowByPartitionKey -table $storageTable -partitionKey $partitionKey1 | ft
```
Sonuçları aşağıdaki tabloda benzer görünmelidir:

| Kullanıcı Kimliği | kullanıcı adı | bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Belirli bir değeri belirli bir sütundaki varlıkları alma

Burada belirli bir sütundaki değer belirli bir değere eşittir varlıkları almak için kullanın **Get-AzureStorageTableRowByColumnName**.

```powershell
Get-AzureStorageTableRowByColumnName -table $storageTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Bu sorgu, bir kaydı alır.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 1 |
| kullanıcı adı | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Özel bir filtre kullanarak varlıkları alma 

Özel bir filtre kullanarak varlıkları almak için kullanın **Get-AzureStorageTableRowByCustomFilter**.

```powershell
Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter "(userid eq 1)"
```

Bu sorgu, bir kaydı alır.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 1 |
| kullanıcı adı | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Varlıkları güncelleştirme 

Varlıkları güncelleştirme için üç adım vardır. İlk olarak değiştirmek için varlık alın. İkinci olarak, değişikliği yapın. Üçüncü olarak, değişiklik kullanarak işleme **güncelleştirme AzureStorageTableRow**.

Kullanıcı adıyla varlığı Güncelleştirme 'kullanıcı adı için Jessie' = 'Jessie2' =. Bu örnek ayrıca .NET türlerini kullanarak özel bir filtre oluşturmak için başka bir yol gösterir. 

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2" 

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzureStorageTableRow -table $storageTable 

# To see the new record, query the table.
Get-AzureStorageTableRowByCustomFilter -table $storageTable `
    -customFilter "(username eq 'Jessie2')"
```

Sonuçları Jessie2 kaydı gösterir.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 2 |
| kullanıcı adı | Jessie2 |
| PartitionKey | Bölüm2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tablo varlıklarını silme

Bir varlık veya tablodaki tüm varlıkları silebilirsiniz.

#### <a name="deleting-one-entity"></a>Bir varlığı silme

Tek bir varlığı silmek için bu varlığa bir başvuru almak ve içine kanal **Remove-AzureStorageTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter $filter
$userToDelete | Remove-AzureStorageTableRow -table $storageTable 

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzureStorageTableRowAll -table $storageTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Tablodaki tüm varlıklarını silme 

Tablodaki tüm varlıkları silmek için bunları alın ve sonuçları Kaldır cmdlet'e kanal oluşturarak. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzureStorageTableRowAll `
    -table $storageTable | Remove-AzureStorageTableRow -table $storageTable 

# List entities in the table (there won't be any).
Get-AzureStorageTableRowAll -table $storageTable | ft
```
