---
title: Azure Tablo Depolama ve Azure Cosmos DB Tablo API’sini Ruby ile kullanma
description: Azure Tablo Depolama veya Azure Cosmos DB Tablo API’sini kullanarak yapılandırılmış verileri bulutta depolayın.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: ruby
ms.topic: sample
ms.date: 04/05/2018
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: 3603455674485a505a7dbc969554a881947940ae
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130543"
---
# <a name="how-to-use-azure-table-storage-and-the-azure-cosmos-db-table-api-with-ruby"></a>Azure Tablo Depolama ve Azure Cosmos DB Tablo API’sini Ruby ile kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure Tablo hizmeti ve Azure Cosmos DB Tablo API’sini kullanarak genel senaryoları nasıl gerçekleştireceğinizi gösterir. Örnekler Ruby ile yazılmıştır ve [Ruby için Azure Depolama Tablo İstemcisi Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table)'nı kullanır. Senaryolarda **tablo oluşturma ve silme ve bir tablodaki varlıkları ekleme ve sorgulama** ele alınmıştır.

## <a name="create-an-azure-service-account"></a>Azure hizmet hesabı oluşturma
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="add-access-to-storage-or-azure-cosmos-db"></a>Depolama'ya veya Azure Cosmos DB'sine erişim ekleme
Azure Depolama'yı veya Azure Cosmos DB'yi kullanmak için Tablo REST hizmetleri ile iletişim kuran kolaylık sağlayıcı kitaplıklar içeren Ruby Azure paketini indirip kullanmalısınız.

### <a name="use-rubygems-to-obtain-the-package"></a>Paketi edinmek için RubyGems kullanma
1. **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (Unix) gibi bir komut satırı arabirimi kullanın.
2. Gem'i ve bağımlılıklarını yüklemek için komut satırı penceresine **gem install azure-storage-table** yazın.

### <a name="import-the-package"></a>Paketi içeri aktarma
Sık kullandığınız metin düzenleyiciyi kullanarak aşağıdakileri Storage kullanmak istediğiniz Ruby dosyasının en üstüne ekleyin:

```ruby
require "azure/storage/table"
```

## <a name="add-an-azure-storage-connection"></a>Azure Depolama bağlantısı ekleme
Azure Depolama modülü, Azure Depolama hesabınıza bağlanırken gereken bilgiler için **AZURE_STORAGE_ACCOUNT** ve **AZURE_STORAGE_ACCESS_KEY** ortam değişkenlerini okur. Bu ortam değişkenleri ayarlanmamışsa, **Azure::Storage::Table::TableService** kullanmadan önce aşağıdaki kod ile hesap bilgilerini belirtmelisiniz:

```ruby
Azure.config.storage_account_name = "<your Azure Storage account>"
Azure.config.storage_access_key = "<your Azure Storage access key>"
```

Bu değerleri Azure portalında bir klasik veya Kaynak Yöneticisi depolama hesabından edinmek için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabına gidin.
3. Sağdaki Ayarlar dikey penceresinde **Erişim Anahtarları**'na tıklayın.
4. Açılan Erişim anahtarları dikey penceresinde, 1. ve 2. erişim anahtarını göreceksiniz. Bunlardan birini kullanabilirsiniz.
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın.

## <a name="add-an-azure-cosmos-db-connection"></a>Azure Cosmos DB bağlantısını ekleme
Azure Cosmos DB'ye bağlanmak için, Azure portalından birincil bağlantı dizenizi kopyalayın ve kopyaladığınız bağlantı dizesini kullanarak bir **Client** nesnesi oluşturun. **Client** nesnesini bir **TableService** nesnesi oluştururken geçebilirsiniz:

```ruby
common_client = Azure::Storage::Common::Client.create(storage_account_name:'myaccount', storage_access_key:'mykey', storage_table_host:'mycosmosdb_endpoint')
table_client = Azure::Storage::Table::TableService.new(client: common_client)
```

## <a name="create-a-table"></a>Bir tablo oluşturma
**Azure::Storage::Table::TableService** nesnesi tablolar ve varlıklar ile çalışmanızı sağlar. Bir tablo oluşturmak için **create_table()** yöntemini kullanın. Aşağıdaki örnek, bir tablo oluşturur veya varsa hatayı yazdırır.

```ruby
azure_table_service = Azure::Storage::Table::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir varlık eklemek için ilk olarak varlık özelliklerinizi tanımlayan bir karma değer nesnesi oluşturun. Her varlık için bir **PartitionKey** ve **RowKey** belirtmeniz gerektiğini unutmayın. Bunlar, varlıklarınızın benzersiz tanımlayıcılarıdır ve diğer özelliklerinizden çok daha hızlı sorgulanabilen değerlerdir. Azure Depolama, tablonun varlıklarını çok sayıda depolama düğümüne otomatik olarak dağıtmak için **PartitionKey** değerini kullanır. Aynı **PartitionKey** değerine sahip varlıklar aynı düğüme depolanır. **RowKey**, varlığın ait olduğu bölümdeki benzersiz kimliğidir.

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a>Varlığı güncelleştirme
Mevcut bir varlığı güncelleştirmenin birçok yolu vardır:

* **update_entity():** Bunu değiştirerek var olan bir varlığa güncelleştirin.
* **merge_entity():** Mevcut varlığa yeni özellik değerlerini birleştirerek var olan bir varlığa güncelleştirir.
* **insert_or_merge_entity():** Bunu değiştirerek var olan bir varlığa güncelleştirir. Bir varlık yoksa, yenisi eklenir:
* **insert_or_replace_entity():** Mevcut varlığa yeni özellik değerlerini birleştirerek var olan bir varlığa güncelleştirir. Bir varlık yoksa, yenisi eklenir.

Aşağıdaki örnekte **update_entity()** kullanılarak bir varlığın güncelleştirilmesi gösterilmektedir:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

**update_entity()** ve **merge_entity()** yöntemlerinde, güncelleştirmekte olduğunuz varlık yoksa, güncelleştirme işlemi başarısız olur. Bu nedenle bir varlığı daha önceden mevcut olup olmadığına bakmaksızın depolamak istiyorsanız, **insert_or_replace_entity()** veya **insert_or_merge_entity()** kullanmalısınız.

## <a name="work-with-groups-of-entities"></a>Varlık gruplarıyla çalışma
Bazen, sunucu tarafından atomik olarak işlenmelerini sağlamak için birden fazla işlemin toplu bir işte bir arada gönderilmesi mantıklıdır. Bunun için önce bir **Batch** nesnesi oluşturmalı, sonra **TableService** üzerinde **execute_batch()** yöntemini kullanmalısınız. Aşağıdaki örnekte, iki varlığın RowKey 2 ve 3 ile toplu bir işte gönderilmesi gösterilmektedir. Bunun yalnızca aynı PartitionKey değerine sahip varlıklar için sonuç vereceğini unutmayın.

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a>Varlık sorgulama
Bir tablodaki bir varlığı sorgulamak için, tablo adını ve **PartitionKey** ve **RowKey** değerlerini geçerek **get_entity()** yöntemini kullanın.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Varlık kümesi sorgulama
Bir tablodaki bir varlık kümesini sorgulamak için, bir sorgu karma değer nesnesi oluşturun ve **query_entities()** yöntemini kullanın. Aşağıdaki örnekte aynı **PartitionKey** değerine sahip tüm nesnelerin alınması gösterilmektedir:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Sonuç kümesi tek bir sorgunun döndüremeyeceği kadar büyükse, sonraki sayfaları almak üzere kullanabileceğiniz bir devam belirteci döndürülür.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Tabloya gönderilen bir sorgu, bir varlıktan yalnızca birkaç özellik alabilir. "Projeksiyon" olarak adlandırılan bu teknik, bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Select yan tümcesini kullanın ve istemciye getirmek istediğiniz özelliklerin adlarını geçin.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı silmek için **delete_entity()** yöntemini kullanın. Varlığı içeren tablonun adını ve varlığın PartitionKey ve RowKey değerlerini geçin.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Bir tablo silme
Bir tablo silmek için **delete_table()** yöntemini kullanın ve silmek istediğiniz tablonun adını geçin.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Ruby Geliştirici Merkezi](https://azure.microsoft.com/develop/ruby/)
* [Ruby için Microsoft Azure Depolama Tablosu İstemci Kitaplığı](https://github.com/azure/azure-storage-ruby/tree/master/table) 

