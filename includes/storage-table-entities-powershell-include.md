<!--created by Robin Shahan to go in the articles for table storage w/powershell.
    There is one for Azure Table Storage and one for Azure Cosmos DB Table API -->

## <a name="managing-table-entities"></a>Tablo varlıkları yönetme

Bir tablo sahip olduğunuza göre varlık veya tablosundaki satırları yönetme bakalım. 

Bir varlık 3 sistem özelliği de dahil olmak üzere en fazla 255 özelliklere sahip olabilir: **PartitionKey**, **RowKey**, ve **zaman damgası**. Ekleme ve değerlerini güncelleştirme sorumlu **PartitionKey** ve **RowKey**. Sunucu değeri yöneten **zaman damgası**, hangi değiştirilemez. Birlikte **PartitionKey** ve **RowKey** tablo içindeki her varlık benzersiz şekilde tanımlar.

* **PartitionKey**: Varlık depolanan bölüm belirler.
* **RowKey**: varlığın bölüm içinde benzersiz şekilde tanımlar.

Bir varlık için en çok 252 özel özellikler tanımlayabilir. 

### <a name="add-table-entities"></a>Tablo varlıkları ekleyin

Kullanarak bir tablo varlıkları ekleyin **Ekle StorageTableRow**. Bu örnekler bölüm anahtarlarını değerlerle "Bölüm1" ve "bölüm2" ve satır anahtarları durumu kısaltmalar eşit kullanın. Her varlıkta kullanıcı adı ve kullanıcı kimliği özelliklerdir. 

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

Bir tablodaki varlıkları sorgulamak için birkaç farklı yolu vardır.

#### <a name="retrieve-all-entities"></a>Tüm varlıkları almak

Tüm varlıkları almak kullanın **Get-AzureStorageTableRowAll**.

```powershell
Get-AzureStorageTableRowAll -table $storageTable | ft
```

Bu komut, aşağıdaki tabloda benzer sonuçlar verir:

| Kullanıcı Kimliği | kullanıcı adı | Bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |
| 2 | Jessie | Bölüm2 | NM |
| 4 | Steven | Bölüm2 | TX |

#### <a name="retrieve-entities-for-a-specific-partition"></a>Belirli bir bölüm için varlık alma

Belirli bir bölümdeki tüm varlıkları almak kullanın **Get-AzureStorageTableRowByPartitionKey**.

```powershell
Get-AzureStorageTableRowByPartitionKey -table $storageTable -partitionKey $partitionKey1 | ft
```
Sonuçlar aşağıdaki tabloya benzer görünür:

| Kullanıcı Kimliği | kullanıcı adı | Bölüm | rowkey |
|----|---------|---------------|----|
| 1 | Chris | Bölüm1 | CA |
| 3 | Christine | Bölüm1 | WA |

#### <a name="retrieve-entities-for-a-specific-value-in-a-specific-column"></a>Belirli bir sütunda belirli bir değeri varlıkları alma

Burada belirli bir sütunun değeri eşittir belirli bir değeri varlık almaya kullanın **Get-AzureStorageTableRowByColumnName**.

```powershell
Get-AzureStorageTableRowByColumnName -table $storageTable `
    -columnName "username" `
    -value "Chris" `
    -operator Equal
```

Bu sorgu, bir kayıt alır.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 1 |
| kullanıcı adı | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

#### <a name="retrieve-entities-using-a-custom-filter"></a>Özel bir filtre kullanarak varlıkları alma 

Özel bir filtre kullanarak varlıkları almak kullanın **Get-AzureStorageTableRowByCustomFilter**.

```powershell
Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter "(userid eq 1)"
```

Bu sorgu, bir kayıt alır.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 1 |
| kullanıcı adı | Chris |
| PartitionKey | Bölüm1 |
| RowKey      | CA |

### <a name="updating-entities"></a>Varlıkları güncelleştirme 

Varlıkları güncelleştirme için üç adım vardır. İlk olarak, değiştirmek için varlığı alır. İkinci olarak, değişikliği yapın. Üçüncü değişikliği kullanarak yürütme **güncelleştirme AzureStorageTableRow**.

Kullanıcı adı ile varlığı Güncelleştirme 'kullanıcı adına sahip Jessie' = 'Jessie2' =. Bu örnek ayrıca .NET türlerini kullanarak bir özel filtre oluşturmak için başka bir yol gösterir. 

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

Sonuçları Jessie2 kaydını gösterir.

|Alan|değer|
|----|----|
| Kullanıcı Kimliği | 2 |
| kullanıcı adı | Jessie2 |
| PartitionKey | Bölüm2 |
| RowKey      | NM |

### <a name="deleting-table-entities"></a>Tablo varlıkları silme

Bir varlık veya tablodaki tüm varlıkların silebilirsiniz.

#### <a name="deleting-one-entity"></a>Bir varlığı silme

Tek bir varlık silmek için bu varlığa bir başvuru almak ve içine kanal **Kaldır AzureStorageTableRow**.

```powershell
# Set filter.
[string]$filter = `
  [Microsoft.WindowsAzure.Storage.Table.TableQuery]::GenerateFilterCondition("username",`
  [Microsoft.WindowsAzure.Storage.Table.QueryComparisons]::Equal,"Jessie2")

# Retrieve entity to be deleted, then pipe it into the remove cmdlet.
$userToDelete = Get-AzureStorageTableRowByCustomFilter `
    -table $storageTable `
    -customFilter 
$userToDelete | Remove-AzureStorageTableRow -table $storageTable 

# Retrieve entities from table and see that Jessie2 has been deleted.
Get-AzureStorageTableRowAll -table $storageTable | ft
```

#### <a name="delete-all-entities-in-the-table"></a>Tablodaki tüm varlıklarını silme 

Tablosundaki tüm varlıkları silmek için bunları alın ve sonuçları Kaldır cmdlet'ine kanal. 

```powershell
# Get all rows and pipe the result into the remove cmdlet.
Get-AzureStorageTableRowAll `
    -table $storageTable | Remove-AzureStorageTableRow -table $storageTable 

# List entities in the table (there won't be any).
Get-AzureStorageTableRowAll -table $storageTable | ft
```