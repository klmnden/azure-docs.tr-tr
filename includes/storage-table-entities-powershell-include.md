---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/27/2019
ms.author: tamram
ms.openlocfilehash: 9a60c624b181a1efd2f6deebd349daa82214a8a4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66159756"
---
<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tablo varlıklarını yönetme

Bir tablonuz olduğuna göre varlıklar veya tablosundaki satırları yönetme göz atalım. 

Varlıkları üç sistem özelliği dahil olmak üzere, en fazla 255 özelliklere sahiptir: **PartitionKey**, **RowKey**, ve **zaman damgası**. Ekleme ve güncelleştirme değerlerini sorumlu **PartitionKey** ve **RowKey**. Sunucu değeri yönetir **zaman damgası**, hangi değiştirilemez. Birlikte **PartitionKey** ve **RowKey** tablo içindeki her bir varlık benzersiz olarak tanımlanabilmesi.

* **PartitionKey**: Varlık içinde depolanır bölüm belirler.
* **RowKey**: Varlığı bölüm içinde benzersiz olarak tanımlar.

Bir varlık için özel en çok 252 özellik tanımlayabilir. 

### <a name="add-table-entities"></a>Tablo varlık ekleme

Varlıkları kullanarak bir tablo ekleyin **Ekle AzTableRow**. Bu örneklerde değerlerle bölüm anahtarları `partition1` ve `partition2`, ve satır anahtarı için durum kısaltmalar eşit. Her bir varlıkta özellikleri `username` ve `userid`. 

```powershell
$partitionKey1 = "partition1"
$partitionKey2 = "partition2"

# add four rows 
Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("CA") -property @{"username"="Chris";"userid"=1}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("NM") -property @{"username"="Jessie";"userid"=2}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey1 `
    -rowKey ("WA") -property @{"username"="Christine";"userid"=3}

Add-AzTableRow `
    -table $cloudTable `
    -partitionKey $partitionKey2 `
    -rowKey ("TX") -property @{"username"="Steven";"userid"=4}
```

### <a name="query-the-table-entities"></a>Tablo varlıkları sorgulama

Bir tablodaki varlıkları kullanarak sorgulayabilirsiniz **Get-AzTableRow** komutu.

> [!NOTE]
> Cmdlet'ler **Get-AzureStorageTableRowAll**, **Get-AzureStorageTableRowByPartitionKey**, **Get-AzureStorageTableRowByColumnName**, ve  **Get-AzureStorageTableRowByCustomFilter** kullanım dışıdır ve gelecekteki sürümüne güncelleştirmede kaldırılacak.

#### <a name="retrieve-all-entities"></a>Tüm varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable | ft
```

Bu komut için aşağıdaki tabloda benzer sonuçlar verir:

| userid | username | bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |
| 2 | Jessie | Bölüm2 | NM |
| 4 | Steven | Bölüm2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Belirli bir bölüm için varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable -partitionKey $partitionKey1 | ft
```

Sonuçları aşağıdaki tabloda benzer görünmelidir:

| userid | username | bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Belirli bir değeri belirli bir sütundaki varlıkları alma

```powershell
Get-AzTableRow -table $cloudTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Bu sorgu, bir kaydı alır.

|Alan|value|
|----|----|
| userid | 1 |
| username | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Özel bir filtre kullanarak varlıkları alma 

```powershell
Get-AzTableRow `
    -table $cloudTable `
    -customFilter "(userid eq 1)"
```

Bu sorgu, bir kaydı alır.

|Alan|value|
|----|----|
| userid | 1 |
| username | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Varlıkları güncelleştirme 

Varlıkları güncelleştirme için üç adım vardır. İlk olarak değiştirmek için varlık alın. İkinci olarak, değişikliği yapın. Üçüncü olarak, değişiklik kullanarak işleme **güncelleştirme AzTableRow**.

Kullanıcı adıyla varlığı Güncelleştirme 'kullanıcı adı için Jessie' = 'Jessie2' =. Bu örnek ayrıca .NET türlerini kullanarak özel bir filtre oluşturmak için başka bir yol gösterir.

```powershell
# Create a filter and get the entity to be updated.
[string]$filter = `
    [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
    [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie")
$user = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter

# Change the entity.
$user.username = "Jessie2"

# To commit the change, pipe the updated record into the update cmdlet.
$user | Update-AzTableRow -table $cloudTable

# To see the new record, query the table.
Get-AzTableRow -table $cloudTable `
    -customFilter "(username eq 'Jessie2')"
```

Sonuçları Jessie2 kaydı gösterir.

|Alan|value|
|----|----|
| userid | 2 |
| username | Jessie2 |
| PartitionKey | Bölüm2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tablo varlıklarını silme

Bir varlık veya tablodaki tüm varlıkları silebilirsiniz.

#### <a name="deleting-one-entity"></a>Bir varlığı silme

Tek bir varlığı silmek için bu varlığa bir başvuru almak ve içine kanal **Remove-AzTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.Azure.Cosmos.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.Azure.Cosmos.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzTableRow `
    -table $cloudTable `
    -customFilter $filter
$userToDelete | Remove-AzTableRow -table $cloudTable

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzTableRow -table $cloudTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Tablodaki tüm varlıklarını silme

Tablodaki tüm varlıkları silmek için bunları alın ve sonuçları Kaldır cmdlet'e kanal oluşturarak. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzTableRow `
    -table $cloudTable | Remove-AzTableRow -table $cloudTable 

# List entities in the table (there won't be any).
Get-AzTableRow -table $cloudTable | ft
```
