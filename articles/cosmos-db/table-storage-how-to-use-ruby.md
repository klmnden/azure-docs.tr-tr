---
title: Azure Table Storage ve Azure Cosmos DB tablo API Ruby ile kullanma | Microsoft Docs
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 02/27/2018
ms.author: mimig
ms.openlocfilehash: 104d793826116462f71e4889386906256b2df8f8
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-ruby"></a>Azure Table Storage ve Azure Cosmos DB tablo API Ruby ile kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure Table hizmeti ve Azure Cosmos DB tablo API kullanarak genel senaryolar gerçekleştirme gösterir. Ruby ve kullanım örnekleri yazılır [Ruby için Azure Storage tablo istemci Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table). Kapsamdaki senaryolar dahil **oluşturma ve bir tablo silme ve ekleme ve bir tablo varlıkları sorgulama**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="add-access-to-storage-or-azure-cosmos-db"></a>Depolama veya Azure Cosmos DB erişim Ekle
Azure Storage veya Azure Cosmos DB kullanmak için indirme ve tablo REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Ruby Azure paketi kullanmanız gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paket elde etmek için RubyGems kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. Tür **gem yüklemek azure depolama tablosu** gem ve bağımlılıklarını yüklemek için komut penceresinde.

### <a name="import-the-package"></a>Paket alma
Sık kullandığınız metin düzenleyiciyi kullanın, burada depolama kullanmayı düşündüğünüz Söyleniş dosyasının üstüne aşağıdakileri ekleyin:

```ruby
require "azure/storage/table"
```

## <a name="add-an-azure-storage-connection"></a>Bir Azure depolama bağlantı Ekle
Azure Storage modül ortam değişkenleri okur **AZURE_STORAGE_ACCOUNT** ve **AZURE_STORAGE_ACCESS_KEY** Azure depolama hesabınıza bağlanmak için gerekli bilgileri için. Bu ortam değişkenleri ayarlanmamışsa, hesap bilgilerini kullanmadan önce belirtmelisiniz **Azure::Storage::Table::TableService** aşağıdaki kod ile:

```ruby
Azure.config.storage_account_name = "<your Azure Storage account>"
Azure.config.storage_access_key = "<your Azure Storage access key>"
```

Klasik veya Resource Manager depolama hesabı Azure portalında bu değerleri almak için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabınıza gidin.
3. Sağ taraftaki ayarları dikey penceresinde tıklayın **erişim tuşları**.
4. Görüntülenen erişim anahtarları dikey penceresinde, erişim tuşu 1 ve 2 erişim anahtarı görürsünüz. Bunlardan kullanabilirsiniz.
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın.

## <a name="add-an-azure-cosmos-db-connection"></a>Bir Azure Cosmos DB Bağlantısı Ekle
Azure Cosmos Veritabanına bağlanmak için birincil bağlantı dizenizi Azure Portal'dan kopyalayın ve oluşturma bir **istemci** kopyalanan bağlantı dizenizi kullanarak nesne. Geçirebilirsiniz **istemci** nesne oluşturduğunuzda bir **TableService** nesnesi:

```ruby
common_client = Azure::Storage::Common::Client.create(storage_account_name:'myaccount', storage_access_key:'mykey', storage_table_host:'mycosmosdb_endpoint')
table_client = Azure::Storage::Table::TableService.new(client: common_client)
```

## <a name="create-a-table"></a>Bir tablo oluşturma
**Azure::Storage::Table::TableService** nesnesi, tabloları ve varlıkları ile çalışmanıza olanak sağlar. Bir tablo oluşturmak için kullanmak **create_table()** yöntemi. Aşağıdaki örnek, bir tablo oluşturur veya hata varsa yazdırır.

```ruby
azure_table_service = Azure::Storage::Table::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir varlık eklemek için önce varlık özelliklerinizi tanımlayan bir karma nesnesi oluşturun. Her varlık için belirtmelisiniz Not bir **PartitionKey** ve **RowKey**. Bunlar, varlıklarınızı'nın benzersiz tanımlayıcıları ve çok diğer özelliklerinizi daha hızlı sorgulanabilir değerlerdir. Azure depolama kullanan **PartitionKey** tablonun varlıklar birçok depolama düğümleri üzerinde otomatik olarak dağıtmak için. Aynı varlıkla **PartitionKey** aynı düğümde depolanır. **RowKey** varlığın ait bölüm içinde benzersiz kimliğidir.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Bir varlığı güncelleştirir
Var olan bir varlığı güncelleştirmek için kullanılabilir birden çok yöntemi vardır:

* **update_entity():** var olan bir varlığı değiştirme tarafından güncelleştirin.
* **merge_entity():** varolan varlık kümesine yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir.
* **insert_or_merge_entity():** değiştirme tarafından var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir:
* **insert_or_replace_entity():** varolan varlık kümesine yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir.

Aşağıdaki örnek, bir varlık kullanarak güncelleştirme gösterir **update_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

İle **update_entity()** ve **merge_entity()**, güncelleştirdiğiniz varlık yoksa güncelleştirme işlemi başarısız olur. Bu nedenle, bir varlık zaten var olup olmadığını bağımsız olarak depolamak istiyorsanız, bunun yerine kullanmalısınız **insert_or_replace_entity()** veya **insert_or_merge_entity()**.

## <a name="work-with-groups-of-entities"></a>Varlıkları gruplarıyla çalışma
Bazen birden çok sunucu tarafından işleme atomik emin olmak için birlikte toplu iş işlemlerinde göndermek için mantıklıdır. Bunu yapmaya yönelik önce oluşturduğunuz bir **toplu** nesnesi ve ardından **execute_batch()** yöntemi **TableService**. Aşağıdaki örnek, iki varlık RowKey 2 ve 3 toplu gönderme gösterir. Yalnızca aynı PartitionKey sahip varlıklar için çalışır durumda olduğunu dikkat edin.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Bir varlık için sorgu
Bir tablodaki bir varlık sorgulamak için kullanın **get_entity()** tablo adı geçirerek yöntemi **PartitionKey** ve **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Varlık kümesi sorgulama
Bir tablodaki varlıkları kümesinin sorgulamak için bir sorgu karma nesnesi oluşturma ve kullanma **query_entities()** yöntemi. Aşağıdaki örnek, aynı tüm varlıkları alma gösterir **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Sonuç kümesi dönmek tek bir sorgu için çok büyük ise, bir devamlılık belirteci sonraki sayfalarda almak için kullanabileceğiniz döndürülür.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Sorguda bir tabloya bir varlık birkaç özelliği alabilir. "Projeksiyon," olarak adlandırılan bu teknik bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Select yan tümcesi kullanın ve istemciye Getir istediğiniz özellik adlarını geçirin.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı silmek için kullanın **delete_entity()** yöntemi. Varlık, PartitionKey ve RowKey varlık içeren bir tablo adına geçirin.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Bir tablo silme
Bir tablo silmek için kullanın **delete_table()** yöntemi ve silmek istediğiniz tabloyu adına geçirin.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Ruby Geliştirici Merkezi](https://azure.microsoft.com/develop/ruby/)
* [Ruby için Microsoft Azure depolama tablo istemci kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table) 

