---
title: Azure Table Storage ve Azure Cosmos DB C++ ile kullanma | Microsoft Docs
description: "Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jahogg
editor: tysonn
ms.assetid: f191f308-e4b2-4de9-85cb-551b82b1ea7c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: mimig
ms.openlocfilehash: 69d56c79320931419ff8d71373ec578af2dec921
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-c"></a>Azure Table depolama ve Azure Cosmos DB tablo API C++ ile kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure Table depolama hizmeti veya Azure Cosmos DB tablo API kullanarak yaygın senaryolar gerçekleştirmek nasıl yapacağınızı gösterir. C++ ve kullanım örnekleri yazılır [C++ için Azure Storage istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md). Kapsamdaki senaryolar dahil **oluşturma ve bir tablo silme** ve **tablo varlıklarla çalışmaya**.

> [!NOTE]
> Bu kılavuz, c++ sürümü 1.0.0 ve yukarıda Azure Storage istemci kitaplığı hedefler. Aracılığıyla kullanılabilir olan depolama istemci kitaplığı 2.2.0, önerilen sürümüdür [NuGet](http://www.nuget.org/packages/wastorage) veya [GitHub](https://github.com/Azure/azure-storage-cpp/).
> 
> 

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda, C++ uygulamasında çalıştırılabilir depolama özelliklerini kullanır. Bunu yapmak için Azure Storage istemci kitaplığı C++ için yükleme ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.  

C++ için Azure Storage istemci kitaplığı yüklemek için aşağıdaki yöntemleri kullanabilirsiniz:

* **Linux:** üzerinde verilen yönergeleri izleyin [C++ Benioku için Azure Storage istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sayfası.  
* **Windows:** Visual Studio'da sırasıyla **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**. Aşağıdaki komutu yazın [NuGet Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ve Enter tuşuna basın.  
  
     Install-Package wastorage

## <a name="configure-access-to-the-table-client-library"></a>Tablo istemci kitaplığı erişimi yapılandırma
Azure depolama API'leri tabloları erişmek için kullanmasını istediğiniz C++ dosyanın en üstüne deyimlerini şunlar ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

Bir Azure Storage istemcisi veya Cosmos DB istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir bağlantı dizesi kullanır. Bir istemci uygulaması çalıştırırken, depolama bağlantı dizesi veya Azure Cosmos DB bağlantı dizesi uygun biçimdeki sağlamanız gerekir.

## <a name="set-up-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi ayarlama
 Listelenen depolama hesabı için depolama hesabınız ve erişim anahtarı adını kullanmak [Azure Portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. Depolama hesapları ve erişim anahtarları hakkında daha fazla bilgi için bkz: [hakkında Azure depolama hesapları](../storage/common/storage-create-storage-account.md). Bu örnek, Azure depolama bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:  

```cpp
// Define the Storage connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="set-up-an-azure-cosmos-db-connection-string"></a>Bir Azure Cosmos DB bağlantı dizesi ayarlama
Azure Cosmos DB hesabınızı, birincil anahtar ve uç nokta listelenen adını kullanmak [Azure Portal](https://portal.azure.com) için *hesap adı*, *birincil anahtar*, ve  *Uç nokta* değerleri. Bu örnek, nasıl Azure Cosmos DB bağlantı dizesi tutmak için statik bir alana bildirebilir gösterir:

```cpp
// Define the Azure Cosmos DB connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_cosmos_db_account;AccountKey=your_cosmos_db_account_key;TableEndpoint=your_cosmos_db_endpoint"));
```


Yerel Windows tabanlı bilgisayarınızın uygulamanızı test etmek için Azure kullanabilirsiniz [depolama öykünücüsü](../storage/common/storage-use-emulator.md) ile yüklü [Azure SDK'sı](https://azure.microsoft.com/downloads/). Depolama öykünücüsü Azure Blob, kuyruk ve Tablo Hizmetleri, yerel geliştirme makinenizde kullanılabilir benzetim yapan bir yardımcı programdır. Aşağıdaki örnek, yerel depolama öykünücüsü için bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure storage öykünücüsü başlatmak için tıklatın **Başlat** düğmesine veya Windows tuşuna basın. Yazmaya başlayın **Azure Storage öykünücüsü**ve ardından **Microsoft Azure Storage öykünücüsü** uygulamalar listesinden.  

Aşağıdaki örnekler, bu iki yöntemden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayalım.  

## <a name="retrieve-your-connection-string"></a>Bağlantı dizesi alma
Kullanabileceğiniz **cloud_storage_account** depolama hesabı bilgileri temsil eden sınıf. Depolama bağlantı dizesi, depolama hesabı bilgilerini almak için kullanabileceğiniz **ayrıştırma** yöntemi.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Ardından, bir başvuru almak bir **cloud_table_client** sınıfı gibi tablolar ve tablo depolama hizmet içinde depolanan varlıklar için başvuru nesneleri alabilmenizi sağlar. Aşağıdaki kod oluşturur bir **cloud_table_client** biz alınan yukarıda depolama hesabı nesnesini kullanarak nesnesi:  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Bir tablo oluşturma
A **cloud_table_client** nesne tabloları ve varlıkları için başvuru nesneleri almak olanak sağlar. Aşağıdaki kod oluşturur bir **cloud_table_client** nesne ve yeni bir tablo oluşturmak için kullanır.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);  

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();  
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir tabloya bir varlık eklemek için yeni bir oluşturma **table_entity** nesne ve ona geçirin **table_operation::insert_entity**. Aşağıdaki kod, bölüm anahtarı olarak müşterinin adını satır anahtarını ve Soyadı kullanır. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlarının göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması daha fazla paralel işlem ölçeklenebilirlik sağlar. Daha fazla bilgi için bkz: [Microsoft Azure depolama performans ve ölçeklenebilirlik Yapılacaklar listesi](../storage/common/storage-performance-checklist.md).

Aşağıdaki kod yeni bir örneğini oluşturur **table_entity** depolanması için bazı müşteri verileri. Kodun sonraki çağrılar **table_operation::insert_entity** oluşturmak için bir **table_operation** bir tabloya bir varlık eklemek için nesne ve yeni tablo varlığı ile ilişkilendirir. Son olarak, kod üzerinde yürütme yöntemini çağırır **cloud_table** nesnesi. Ve yeni **table_operation** "Kişiler" tablosuna yeni müşteri varlık eklemek için tablo hizmetine bir istek gönderir.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Retrieve a reference to a table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table if it doesn't exist.
table.create_if_not_exists();

// Create a new customer entity.
azure::storage::table_entity customer1(U("Harp"), U("Walter"));

azure::storage::table_entity::properties_type& properties = customer1.properties();
properties.reserve(2);
properties[U("Email")] = azure::storage::entity_property(U("Walter@contoso.com"));

properties[U("Phone")] = azure::storage::entity_property(U("425-555-0101"));

// Create the table operation that inserts the customer entity.
azure::storage::table_operation insert_operation = azure::storage::table_operation::insert_entity(customer1);

// Execute the insert operation.
azure::storage::table_result insert_result = table.execute(insert_operation);
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Toplu işlem varlık tablo hizmeti için bir yazma işlemi ekleyebilirsiniz. Aşağıdaki kod oluşturur bir **table_batch_operation** nesne ve üç ekleme işlemleri ona ekler. Yeni bir varlık nesnesi oluşturarak, kendi değerlerini ayarlama ve üzerinde Insert yöntemini çağırmak her ekleme işlemi eklenen **table_batch_operation** varlığı yeni bir ekleme işlemi ile ilişkilendirmek için nesne. Ardından, **cloud_table.execute** işlemi yürütmek için çağrılır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define a batch operation.
azure::storage::table_batch_operation batch_operation;

// Create a customer entity and add it to the table.
azure::storage::table_entity customer1(U("Smith"), U("Jeff"));

azure::storage::table_entity::properties_type& properties1 = customer1.properties();
properties1.reserve(2);
properties1[U("Email")] = azure::storage::entity_property(U("Jeff@contoso.com"));
properties1[U("Phone")] = azure::storage::entity_property(U("425-555-0104"));

// Create another customer entity and add it to the table.
azure::storage::table_entity customer2(U("Smith"), U("Ben"));

azure::storage::table_entity::properties_type& properties2 = customer2.properties();
properties2.reserve(2);
properties2[U("Email")] = azure::storage::entity_property(U("Ben@contoso.com"));
properties2[U("Phone")] = azure::storage::entity_property(U("425-555-0102"));

// Create a third customer entity to add to the table.
azure::storage::table_entity customer3(U("Smith"), U("Denise"));

azure::storage::table_entity::properties_type& properties3 = customer3.properties();
properties3.reserve(2);
properties3[U("Email")] = azure::storage::entity_property(U("Denise@contoso.com"));
properties3[U("Phone")] = azure::storage::entity_property(U("425-555-0103"));

// Add customer entities to the batch insert operation.
batch_operation.insert_or_replace_entity(customer1);
batch_operation.insert_or_replace_entity(customer2);
batch_operation.insert_or_replace_entity(customer3);

// Execute the batch operation.
std::vector<azure::storage::table_result> results = table.execute_batch(batch_operation);
```

Toplu işlem dikkat edilecek bazı noktalar:  

* Tek bir toplu işteki herhangi bir arada en fazla 100 Ekle, Sil, birleştirme, Değiştir, ekleme veya birleştirme ve ekleme veya değiştirme işlemleri gerçekleştirebilir.  
* Toplu işlemdeki tek işlem olması durumunda toplu işlem bir alma işlemi olabilir.  
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.  
* Bir toplu işlemi için 4 MB veri yükü sınırlıdır.  

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Bir bölümdeki tüm varlıklar için bir tabloyu sorgulamak için kullanın bir **table_query** nesnesi. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.  

> [!NOTE]
> Bu yöntemler, C++ Azure Cosmos veritabanı için şu anda desteklenmemektedir.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Construct the query operation for all customer entities where PartitionKey="Smith".
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::generate_filter_condition(U("PartitionKey"), azure::storage::query_comparison_operator::equal, U("Smith")));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Print the fields for each customer.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

Bu örnekte sorgu, filtre ölçütüyle eşleşen tüm varlıkların getirir. Büyük tabloları varsa ve tablo varlıkları indirmek için genellikle öneririz, verilerinizi Azure storage bloblarında yerine depolar.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma
Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız bölüm anahtarı filtresi ile bir satır anahtarı filtresini birleştirerek bir aralık belirleyebilirsiniz. Aşağıdaki kod örneği, 'Smith' bölümünde, satır anahtarı (ad) alfabede 'E' harfinden önce gelen bir harfle başlayan tüm varlıkları almak için iki filtre kullanır, ardından sorgu sonuçlarını yazdırır.  

> [!NOTE]
> Bu yöntemler, C++ Azure Cosmos veritabanı için şu anda desteklenmemektedir.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create the table query.
azure::storage::table_query query;

query.set_filter_string(azure::storage::table_query::combine_filter_conditions(
    azure::storage::table_query::generate_filter_condition(U("PartitionKey"),
    azure::storage::query_comparison_operator::equal, U("Smith")),
    azure::storage::query_logical_operator::op_and,
    azure::storage::table_query::generate_filter_condition(U("RowKey"), azure::storage::query_comparison_operator::less_than, U("E"))));

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Loop through the results, displaying information about the entity.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    const azure::storage::table_entity::properties_type& properties = it->properties();

    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key()
        << U(", Property1: ") << properties.at(U("Email")).string_value()
        << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
}  
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod **table_operation::retrieve_entity** 'Jeff Smith' müşteri belirtmek için. Bu yöntem bir koleksiyon yerine yalnızca bir varlık döndürür ve döndürülen değer olarak **table_result**. Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.  

```cpp
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Output the entity.
azure::storage::table_entity entity = retrieve_result.entity();
const azure::storage::table_entity::properties_type& properties = entity.properties();

std::wcout << U("PartitionKey: ") << entity.partition_key() << U(", RowKey: ") << entity.row_key()
    << U(", Property1: ") << properties.at(U("Email")).string_value()
    << U(", Property2: ") << properties.at(U("Phone")).string_value() << std::endl;
```

## <a name="replace-an-entity"></a>Bir varlığı değiştirme
Bir varlığı değiştirmek için tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarası ve e-posta adresini değiştirir. Çağırmak yerine **table_operation::insert_entity**, bu kod **table_operation::replace_entity**. Bu, sunucu üzerindeki varlık alındığından beri değiştirilmemişse varlığın sunucu üzerinde tamamen değiştirilmesini sağlar, aksi takdirde işlem başarısız olur. Bu işlem, uygulamanızın başka bir bileşeninin alım ve güncelleştirme arasında gerçekleştirilen bir değişikliğin yanlışlıkla üzerine yazılmasını engellemek üzere başarısız olur. Varlığın yeniden alınması, (hala geçerli ise) değişiklikleri yapın ve ardından başka bir gerçekleştirmek için bu hatanın uygun işleme olan **table_operation::replace_entity** işlemi. Sonraki bölüm bu davranışı nasıl geçersiz kılacağınızı gösterecektir.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Replace an entity.
azure::storage::table_entity entity_to_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_replace = entity_to_replace.properties();
properties_to_replace.reserve(2);

// Specify a new phone number.
properties_to_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0106"));

// Specify a new email address.
properties_to_replace[U("Email")] = azure::storage::entity_property(U("JeffS@contoso.com"));

// Create an operation to replace the entity.
azure::storage::table_operation replace_operation = azure::storage::table_operation::replace_entity(entity_to_replace);

// Submit the operation to the Table service.
azure::storage::table_result replace_result = table.execute(replace_operation);
```

## <a name="insert-or-replace-an-entity"></a>Bir varlığı yerleştirme veya değiştirme
**table_operation::replace_entity** varlık sunucudan alındığından beri değiştirilmişse işlemleri başarısız olur. Ayrıca, varlığın sunucudan ilk sırada aldığınız gerekir **table_operation::replace_entity** başarılı olması için. Bazı durumlarda, Bununla birlikte, varlık sunucuda bulunduğundan ve içinde saklı geçerli değerlerin ilgisiz olup olmadığını bilmiyorsanız — güncelleştirmeniz tümünün üzerine yazmalıdır. Bunu gerçekleştirmek için kullanacağınız bir **table_operation::insert_or_replace_entity** işlemi. Bu işlem, varlık mevcut değilse varlığı yerleştirir, eğer varlık mevcutsa yapılan son güncelleştirmeden bağımsız olarak değiştirir. Aşağıdaki kod örneğinde Jeff Smith için müşteri varlığı hala alınabilir, ancak ardından sunucuya geri kaydedilir **table_operation::insert_or_replace_entity**. Varlığa alma ve güncelleştirme işlemi arasında yapılan güncelleştirmeler üzerine yazılır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Insert-or-replace an entity.
azure::storage::table_entity entity_to_insert_or_replace(U("Smith"), U("Jeff"));
azure::storage::table_entity::properties_type& properties_to_insert_or_replace = entity_to_insert_or_replace.properties();

properties_to_insert_or_replace.reserve(2);

// Specify a phone number.
properties_to_insert_or_replace[U("Phone")] = azure::storage::entity_property(U("425-555-0107"));

// Specify an email address.
properties_to_insert_or_replace[U("Email")] = azure::storage::entity_property(U("Jeffsm@contoso.com"));

// Create an operation to insert-or-replace the entity.
azure::storage::table_operation insert_or_replace_operation = azure::storage::table_operation::insert_or_replace_entity(entity_to_insert_or_replace);

// Submit the operation to the Table service.
azure::storage::table_result insert_or_replace_result = table.execute(insert_or_replace_operation);
```

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Sorguda bir tabloya bir varlık birkaç özelliği alabilir. Aşağıdaki kodda sorgusu kullanan **table_query::set_select_columns** tabloda yalnızca e-posta adresleri varlıkların döndürülecek yöntemi.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Define the query, and select only the Email property.
azure::storage::table_query query;
std::vector<utility::string_t> columns;

columns.push_back(U("Email"));
query.set_select_columns(columns);

// Execute the query.
azure::storage::table_query_iterator it = table.execute_query(query);

// Display the results.
azure::storage::table_query_iterator end_of_results;
for (; it != end_of_results; ++it)
{
    std::wcout << U("PartitionKey: ") << it->partition_key() << U(", RowKey: ") << it->row_key();

    const azure::storage::table_entity::properties_type& properties = it->properties();
    for (auto prop_it = properties.begin(); prop_it != properties.end(); ++prop_it)
    {
        std::wcout << ", " << prop_it->first << ": " << prop_it->second.str();
    }

    std::wcout << std::endl;
}
```

> [!NOTE]
> Bir varlıktaki birkaç özelliği sorgulama tüm özellikleri alınırken değerinden daha verimli bir işlemdir.
> 
> 

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlık, onu aldıktan sonra kolayca silebilirsiniz. Varlık alındıktan sonra arama **table_operation::delete_entity** varlıkla silin. ' I çağırın **cloud_table.execute** yöntemi. Aşağıdaki kod alır ve bir varlık bir bölüm anahtarı "Smith" ve "Jeff" satır anahtarı ile siler.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
azure::storage::table_operation retrieve_operation = azure::storage::table_operation::retrieve_entity(U("Smith"), U("Jeff"));
azure::storage::table_result retrieve_result = table.execute(retrieve_operation);

// Create an operation to delete the entity.
azure::storage::table_operation delete_operation = azure::storage::table_operation::delete_entity(retrieve_result.entity());

// Submit the delete operation to the Table service.
azure::storage::table_result delete_result = table.execute(delete_operation);  
```

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak aşağıdaki kod örneği bir depolama hesabından bir tablo siler. Silinen bir tablo, silme işleminin ardından yeniden oluşturma için belirli bir süre kullanılamayacaktır.  

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);

// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();

// Create a cloud table object for the table.
azure::storage::cloud_table table = table_client.get_table_reference(U("people"));

// Delete the table if it exists
if (table.delete_table_if_exists())
    {
        std::cout << "Table deleted!";
    }
    else
    {
        std::cout << "Table didn't exist";
    }
```

## <a name="troubleshooting"></a>Sorun giderme
* Derleme hataları Visual Studio 2017 Community Edition

  Projenizi içerme dosyaları storage_account.h ve table.h nedeniyle derleme hataları alırsa, kaldırma **/ izin veren-** derleyici anahtar. 
  - İçinde **Çözüm Gezgini**, projenize sağ tıklayın ve seçin **özellikleri**.
  - İçinde **özellik sayfaları** iletişim kutusunda, genişletin **yapılandırma özellikleri**, genişletin **C/C++**seçip **dil**.
  - Ayarlama **uyumluluk modu** için **Hayır**.
   
## <a name="next-steps"></a>Sonraki adımlar
Azure Storage ve Azure Cosmos veritabanı tablo API hakkında daha fazla bilgi için aşağıdaki bağlantıları izleyin: 

* [Tablo API giriş](table-introduction.md)
* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [C++'ta Azure Storage kaynakları listeler](../storage/common/storage-c-plus-plus-enumeration.md)
* [C++ başvurusu için depolama istemci kitaplığı](http://azure.github.io/azure-storage-cpp)
* [Azure Storage belgeleri](https://azure.microsoft.com/documentation/services/storage/)
