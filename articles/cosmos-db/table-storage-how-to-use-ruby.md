---
title: Azure Table Storage ruby'den kullanma | Microsoft Docs
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 372bc89f75ad4730f0defbf9d6f9f041ae5ce1bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-ruby"></a>Ruby'den Azure Table Storage kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure Table hizmetini kullanarak yaygın senaryolar gerçekleştirme gösterir. Örnekler, Ruby API kullanılarak yazılır. Kapsamdaki senaryolar dahil **oluşturma ve bir tablo, ekleme ve bir tablo varlıkları sorgulama**.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a>Ruby uygulaması oluşturma
Yönergeler için Ruby uygulaması oluşturma, bkz: [Azure VM'de rayları Web uygulaması üzerinde Söyleniş](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).

## <a name="configure-your-application-to-access-storage"></a>Depolama alanına erişmek için uygulamanızı yapılandırın
Azure Storage kullanmak için indirme ve depolama REST Hizmetleri ile iletişim kuran bir dizi kolaylık içeren Söyleniş azure paketini kullanmak gerekir.

### <a name="use-rubygems-to-obtain-the-package"></a>Paket elde etmek için RubyGems kullanın
1. Bir komut satırı arabirimi gibi kullandığınız **PowerShell** (Windows), **Terminal** (Mac) veya **Bash** (UNIX).
2. Tür **gem yükleme azure** gem ve bağımlılıklarını yüklemek için komut penceresinde.

### <a name="import-the-package"></a>Paket alma
Sık kullandığınız metin düzenleyiciyi kullanın, burada depolama kullanmayı düşündüğünüz Söyleniş dosyasının üstüne aşağıdakileri ekleyin:

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Azure modülü ortam değişkenleri okur **AZURE\_depolama\_hesap** ve **AZURE\_depolama\_erişim\_anahtar** Azure depolama hesabınıza bağlanmak için gerekli bilgileri için. Bu ortam değişkenleri ayarlanmamışsa, hesap bilgilerini kullanmadan önce belirtmelisiniz **Azure::TableService** aşağıdaki kod ile:

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

Klasik veya Resource Manager depolama hesabı Azure portalında bu değerleri almak için:

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Kullanmak istediğiniz depolama hesabınıza gidin.
3. Sağ taraftaki ayarları dikey penceresinde tıklayın **erişim tuşları**.
4. Görüntülenen erişim anahtarları dikey penceresinde, erişim tuşu 1 ve 2 erişim anahtarı görürsünüz. Bunlardan kullanabilirsiniz.
5. Anahtarı panoya kopyalamak için Kopyala simgesine tıklayın.

## <a name="create-a-table"></a>Bir tablo oluşturma
**Azure::TableService** nesnesi, tabloları ve varlıkları ile çalışmanıza olanak sağlar. Bir tablo oluşturmak için kullanmak **oluşturma\_table()** yöntemi. Aşağıdaki örnek, bir tablo oluşturur veya hata varsa yazdırır.

```ruby
azure_table_service = Azure::TableService.new
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

* **Güncelleştirme\_entity():** var olan bir varlığı değiştirme tarafından güncelleştirin.
* **birleştirme\_entity():** varolan varlık kümesine yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir.
* **INSERT\_veya\_birleştirme\_entity():** değiştirme tarafından var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir:
* **INSERT\_veya\_Değiştir\_entity():** varolan varlık kümesine yeni özellik değerlerinin birleştirerek var olan bir varlığı güncelleştirir. Hiçbir varlık varsa, yeni bir tane eklenir.

Aşağıdaki örnek, bir varlık kullanarak güncelleştirme gösterir **güncelleştirme\_entity()**:

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

İle **güncelleştirme\_entity()** ve **birleştirme\_entity()**, güncelleştirdiğiniz varlık yoksa güncelleştirme işlemi başarısız olur. Bu nedenle olup zaten var. bakılmaksızın bir varlık depolamak istiyorsanız, bunun yerine kullanmalısınız **Ekle\_veya\_Değiştir\_entity()** veya **Ekle\_veya\_birleştirme\_entity()**.

## <a name="work-with-groups-of-entities"></a>Varlıkları gruplarıyla çalışma
Bazen birden çok sunucu tarafından işleme atomik emin olmak için birlikte toplu iş işlemlerinde göndermek için mantıklıdır. Bunu yapmaya yönelik önce oluşturduğunuz bir **toplu** nesnesi ve ardından **yürütme\_batch()** yöntemi **TableService**. Aşağıdaki örnek, iki varlık RowKey 2 ve 3 toplu gönderme gösterir. Yalnızca aynı PartitionKey sahip varlıklar için çalışır durumda olduğunu dikkat edin.

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
Bir tablodaki bir varlık sorgulamak için kullanın **almak\_entity()** tablo adı geçirerek yöntemi **PartitionKey** ve **RowKey**.

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a>Varlık kümesi sorgulama
Bir tablodaki varlıkları kümesinin sorgulamak için bir sorgu karma nesnesi oluşturma ve kullanma **sorgu\_entities()** yöntemi. Aşağıdaki örnek, aynı tüm varlıkları alma gösterir **PartitionKey**:

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> Sonuç kümesi döndürmek sonraki sayfalarda almak için kullanabileceğiniz bir devamlılık belirteci döndürülecek tek bir sorgu için çok büyük.
>
>

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Sorguda bir tabloya bir varlık birkaç özelliği alabilir. "Projeksiyon" olarak adlandırılan bu teknik bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Select yan tümcesi kullanın ve istemciye Getir istediğiniz özellik adlarını geçirin.

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı silmek için kullanın **silmek\_entity()** yöntemi. Varlık, varlığın PartitionKey ve RowKey varlık içeren tablo adına geçmesi gerekir.

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a>Bir tablo silme
Bir tablo silmek için kullanın **silmek\_table()** yöntemi ve silmek istediğiniz tabloyu adına geçirin.

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a>Sonraki adımlar

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Ruby için Azure SDK](http://github.com/WindowsAzure/azure-sdk-for-ruby) github'daki

