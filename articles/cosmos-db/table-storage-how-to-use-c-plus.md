---
title: Azure tablo depolama ve Azure Cosmos DB tablo API'si ile C++ kullanma
description: Azure Tablo Depolama veya Azure Cosmos DB Tablo API’sini kullanarak yapılandırılmış verileri bulutta depolayın.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: cpp
ms.topic: sample
ms.date: 04/05/2018
author: wmengmsft
ms.author: wmeng
ms.openlocfilehash: 40b84a56f93ad670a26eb876a18820e0d4037f63
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130560"
---
# <a name="how-to-use-azure-table-storage-and-azure-cosmos-db-table-api-with-c"></a>Azure Tablo Depolama ve Azure Cosmos DB Tablo API’sini C++ ile kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-applies-to-storagetable-and-cosmos](../../includes/storage-table-applies-to-storagetable-and-cosmos.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuzda Azure Tablo depolama hizmeti ve Azure Cosmos DB Tablo API’si kullanılarak genel senaryoların nasıl uygulanacağı gösterilir. Örnekler C++ dilinde yazılmıştır ve [C++ için Azure Depolama İstemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md)’nı kullanır. **Tablo oluşturma ve silme** ile **tablo varlıkları ile çalışma** senaryoları ele alınmaktadır.

> [!NOTE]
> Bu kılavuz C++ için Azure Depolama İstemci Kitaplığı sürüm 1.0.0 ve üzerini hedefler. Önerilen sürüm, [NuGet](https://www.nuget.org/packages/wastorage) ya da [GitHub](https://github.com/Azure/azure-storage-cpp/) üzerinden ulaşılabilen Depolama İstemci Kitaplığı 2.2.0’dır.
> 

## <a name="create-an-azure-service-account"></a>Azure hizmet hesabı oluşturma
[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma
[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Azure Cosmos DB Tablo API’si hesabı oluşturma
[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="create-a-c-application"></a>C++ uygulaması oluşturma
Bu kılavuzda bir C++ uygulamasında çalıştırılabilen depolama özelliklerini kullanacaksınız. Bunu yapmak için, C++ için Azure Depolama İstemci Kitaplığı’nı yüklemeniz ve Azure aboneliğinizde bir Azure depolama hesabı oluşturmanız gerekir.  

C++ için Azure Depolama İstemci Kitaplığı’nı aşağıdaki yöntemleri kullanarak yükleyebilirsiniz:

* **Linux:** Şirket yönergelerini izleyin [C++ Benioku için Azure depolama istemci Kitaplığı](https://github.com/Azure/azure-storage-cpp/blob/master/README.md) sayfası.  
* **Windows:** Visual Studio'da, **Araçlar > NuGet Paket Yöneticisi > Paket Yöneticisi Konsolu**’na tıklayın. Aşağıdaki komutu [NuGet Paket Yöneticisi konsoluna](/nuget/tools/package-manager-console) yazıp Enter’a basın.  
  
     Install-Package wastorage

## <a name="configure-access-to-the-table-client-library"></a>Tablo istemci kitaplığına erişimi yapılandırma
Tablolara erişmek üzere Azure depolama API’lerini kullanmak istediğiniz C++ dosyasının üstüne aşağıdaki deyimleri ekleyin:  

```cpp
#include <was/storage_account.h>
#include <was/table.h>
```

Azure Depolama istemcisi veya Cosmos DB istemcisi, veri yönetimi hizmetlerine erişmek üzere uç noktaları ve kimlik bilgilerini depolamak için bir bağlantı dizesi kullanır. Bir istemci uygulama çalıştırırken, depolama bağlantı dizesini veya Azure Cosmos DB bağlantı dizesini doğru biçimde sağlamanız gerekir.

## <a name="set-up-an-azure-storage-connection-string"></a>Azure Depolama bağlantı dizesini ayarlama
 *AccountName* ve *AccountKey* değerleri için [Azure Portalında](https://portal.azure.com) listelenen Depolama hesabınızın adını ve Depolama hesabına ait erişim anahtarını kullanın. Depolama hesapları ve erişim anahtarları hakkında bilgi için bkz. [Azure Depolama hesapları hakkında](../storage/common/storage-create-storage-account.md). Bu örnekte Azure Depolama bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the Storage connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_storage_account;AccountKey=your_storage_account_key"));
```

## <a name="set-up-an-azure-cosmos-db-connection-string"></a>Azure Cosmos DB bağlantı dizesini ayarlama
Azure Cosmos DB hesabınızın adını, birincil anahtarınızı ve *Hesap Adı*, *Birincil Anahtar* ve *Uç Nokta* değerleri için [Azure Portalında](https://portal.azure.com) listelenen uç noktayı kullanın. Bu örnekte Azure Cosmos DB bağlantı dizesini tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:

```cpp
// Define the Azure Cosmos DB connection string with your values.
const utility::string_t storage_connection_string(U("DefaultEndpointsProtocol=https;AccountName=your_cosmos_db_account;AccountKey=your_cosmos_db_account_key;TableEndpoint=your_cosmos_db_endpoint"));
```


Uygulamanızı yerel Windows tabanlı bilgisayarınızda test etmek için, [Azure SDK](https://azure.microsoft.com/downloads/) ile birlikte yüklenen Azure [depolama öykünücüsünü](../storage/common/storage-use-emulator.md) kullanabilirsiniz. Depolama öykünücüsü, yerel geliştirme makinenizde mevcut olan Azure Blob, Kuyruk ve Tablo hizmetlerini benzeten bir yardımcı programdır. Aşağıdaki örnekte bağlantı dizesini yerel depolama öykünücünüzde tutmak için nasıl statik bir alan bildirebileceğiniz gösterilmektedir:  

```cpp
// Define the connection string with Azure storage emulator.
const utility::string_t storage_connection_string(U("UseDevelopmentStorage=true;"));  
```

Azure Depolama öykünücüsünü başlatmak için **Başlat** düğmesine tıklayın veya Windows tuşuna basın. **Azure Depolama Öykünücüsü** yazmaya başlayın ve sonra uygulama listesinden **Microsoft Azure Depolama Öykünücüsü**’nü seçin.  

Aşağıdaki örnekler, depolama bağlantı dizesini almak için bu iki yöntemden birini kullandığınızı varsayar.  

## <a name="retrieve-your-connection-string"></a>Bağlantı dizenizi alma
Depolama hesabınızın bilgilerini temsil etmesi için **cloud_storage_account** sınıfını kullanabilirsiniz. Depolama bağlantı dizesinden depolama hesabı bilgilerini almak için **parse** yöntemini kullanabilirsiniz.

```cpp
// Retrieve the storage account from the connection string.
azure::storage::cloud_storage_account storage_account = azure::storage::cloud_storage_account::parse(storage_connection_string);
```

Ardından, Tablo depolama hizmeti içinde tablo ve varlıklar için başvuru nesneleri almanıza olanak tanıyan bir **cloud_table_client** sınıfına başvuru alın. Aşağıdaki kod, yukarıda aldığımız depolama hesabı nesnesini kullanarak bir **cloud_table_client** nesnesi oluşturur:  

```cpp
// Create the table client.
azure::storage::cloud_table_client table_client = storage_account.create_cloud_table_client();
```

## <a name="create-a-table"></a>Bir tablo oluşturma
**cloud_table_client** nesnesi, tablo ve varlıklar için başvuru nesneleri almanıza olanak tanır. Aşağıdaki kod bir **cloud_table_client** nesnesi oluşturur ve yeni bir tablo oluşturmak için bu nesneyi kullanır.

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
Bir tabloya varlık eklemek için yeni bir **table_entity** nesnesi oluşturun ve **table_operation::insert_entity** nesnesine geçirin. Aşağıdaki kod, satır anahtarı olarak müşterinin adını, bölüm anahtarı olarak soyadını kullanır. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlı varlıklara göre daha hızlı sorgulanabilir ancak farklı bölüm anahtarlarının kullanılması paralel işlemler için daha büyük ölçeklendirme sağlar. Daha fazla bilgi için bkz. [Microsoft Azure depolama performansı ve ölçeklenebilirliği yapılacaklar listesi](../storage/common/storage-performance-checklist.md).

Aşağıdaki kod, depolanacak bazı müşteri verileri ile birlikte **table_entity** sınıfının yeni bir örneğini oluşturur. Kod daha sonra **table_operation::insert_entity** yöntemini çağırarak bir tabloya varlık eklemek üzere bir **table_operation** nesnesi oluşturur ve onunla yeni tabloyu ilişkilendirir. Son olarak, kod **cloud_table** nesnesi üzerinde execute yöntemini çağırır. Yeni **table_operation** ise Tablo hizmetine yeni müşteri varlığını "people" tablosuna eklemeye yönelik bir istek gönderir.  

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
Tablo hizmetine tek bir yazma işlemiyle çok sayıda varlık ekleyebilirsiniz. Aşağıdaki kod bir **table_batch_operation** nesnesi oluşturur, ardından nesneye üç insert işlemi ekler. Bir insert işlemi, varlığı yeni bir insert işlemi ile ilişkilendirmek üzere yeni bir varlık nesnesi oluşturularak, değerleri ayarlanarak ve sonra **table_batch_operation** nesnesi üzerinde insert yöntemi çağrılarak eklenir. Ardından, işlemi yürütmek için **cloud_table.execute** çağrılır.  

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

Toplu işlemlerde dikkat edilecek bazı noktalar:  

* Tek bir toplu işlemdeki herhangi bir birleşimde en fazla 100 insert, delete, merge, replace, insert-or-merge ve insert-or-replace işlemi gerçekleştirebilirsiniz.  
* Bir toplu işlem toplu iş içindeki tek işlemse bir retrieve işlemi içerebilir.  
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.  
* Toplu işlem 4 MB veri yükü ile sınırlıdır.  

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Bir bölümdeki tüm varlıklar için bir tabloyu sorgulamak üzere bir **table_query** nesnesi kullanın. Aşağıdaki kod örneği, ‘Smith’in bölüm anahtarı olduğu varlıklar için bir filtre belirtir. Bu örnek sorgu sonuçlarındaki her varlığın alanlarını konsola yazdırır.  

> [!NOTE]
> Bu yöntemler şu anda Azure Cosmos DB’de C++ için desteklenmemektedir.

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

Bu örnekteki sorgu, filtre ölçütleriyle eşleşen tüm varlıkları getirir. Büyük tablolarınız varsa ve tablo varlıklarını sık indirmeniz gerekiyorsa, verilerinizi bunun yerine Azure depolama bloblarında depolamanız önerilir.

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma
Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız bölüm anahtarı filtresi ile bir satır anahtarı filtresini birleştirerek bir aralık belirleyebilirsiniz. Aşağıdaki kod örneği, 'Smith' bölümünde, satır anahtarı (ad) alfabede 'E' harfinden önce gelen bir harfle başlayan tüm varlıkları almak için iki filtre kullanır, ardından sorgu sonuçlarını yazdırır.  

> [!NOTE]
> Bu yöntemler şu anda Azure Cosmos DB’de C++ için desteklenmemektedir.

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
Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod 'Jeff Smith' müşterisini belirtmek için **table_operation::retrieve_entity** kullanır. Bu yöntem bir koleksiyon yerine yalnızca bir varlık döndürür ve döndürülen değer **table_result** içindedir. Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.  

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
Bir varlığı değiştirmek için Tablo hizmetinden alın, varlık nesnesini değiştirin ve değişiklikleri Tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını ve e-posta adresini değiştirir. Bu kod **table_operation::insert_entity** nesnesini çağırmak yerine **table_operation::replace_entity** nesnesini kullanır. Bu, sunucu üzerindeki varlık alındığından beri değiştirilmemişse varlığın sunucu üzerinde tamamen değiştirilmesini sağlar, aksi takdirde işlem başarısız olur. Bu işlem, uygulamanızın başka bir bileşeninin alım ve güncelleştirme arasında gerçekleştirilen bir değişikliğin yanlışlıkla üzerine yazılmasını engellemek üzere başarısız olur. Bu başarısız işlem, varlığın yeniden alınması, (hala geçerli ise) değişikliklerin yapılması ve yeni bir **table_operation::replace_entity** işleminin gerçekleştirilmesiyle uygun şekilde ele alınır. Sonraki bölüm bu davranışı nasıl geçersiz kılacağınızı gösterecektir.  

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
Varlık sunucudan alındığından beri değiştirilmişse, **table_operation::replace_entity** işlemleri başarısız olacaktır. Dahası, **table_operation::replace_entity** işleminin başarılı olması için ilk olarak varlığın sunucudan alınması gerekir. Buna karşın bazı durumlarda varlığın sunucuda olup olmadığını ve içinde saklı geçerli değerlerin ilgisiz olup olmadığını bilemeyebilirsiniz. Güncelleştirmeniz bunların tümünün üzerine yazılmalıdır. Bunu gerçekleştirmek için bir **table_operation::insert_or_replace_entity** işlemi kullanırsınız. Bu işlem, varlık mevcut değilse varlığı yerleştirir, eğer varlık mevcutsa yapılan son güncelleştirmeden bağımsız olarak değiştirir. Aşağıdaki kod örneğinde Jeff Smith için müşteri varlığı hala alınabilir, ancak ardından **table_operation::insert_or_replace_entity** ile sunucuya geri kaydedilir. Varlığa alma ve güncelleştirme işlemleri arasında yapılan tüm güncelleştirmelerin üzerine yazılacaktır.  

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
Tabloya gönderilen bir sorgu, bir varlıktan yalnızca birkaç özellik alabilir. Aşağıdaki kodda yer alan sorgu, **table_query::set_select_columns** yöntemini kullanarak yalnızca tablodaki varlıkların e-posta adreslerini döndürür.  

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
> Bir varlıktaki birkaç özelliğin sorgulanması, tüm özelliklerin alınmasından daha verimli bir işlemdir.
> 
> 

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı aldıktan sonra kolayca silebilirsiniz. Varlık alındıktan sonra, silinecek varlıkla birlikte **table_operation::delete_entity** yöntemini çağırın. Sonra **cloud_table.execute** yöntemini çağırın. Aşağıdaki kod, "Smith" bölüm anahtarı ve "Jeff" satır anahtarı ile bir varlığı alır ve siler.  

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
* Visual Studio 2017 Community Edition derleme hataları

  Projeniz include files storage_account.h ve table.h nedeniyle derleme hataları alırsa, **/permissive-** derleme anahtarını kaldırın. 
  - **Çözüm Gezgini**’nde projenize sağ tıklayın ve **Özellikler**’i seçin.
  - **Özellik Sayfaları** iletişim kutusunda **Yapılandırma Özellikleri**’ni, **C/C++**’yi genişletin ve **Dil**’i seçin.
  - **Uyumluluk modu**’nu **Hayır** olarak ayarlayın.
   
## <a name="next-steps"></a>Sonraki adımlar
Azure Depolama ve Azure Cosmos DB Tablo API’si hakkında daha fazla bilgi almak için şu bağlantıları izleyin: 

* [Tablo API’sine giriş](table-introduction.md)
* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Azure Depolama Kaynaklarını C++ dilinde listeleme](../storage/common/storage-c-plus-plus-enumeration.md)
* [C++ başvurusu için Depolama İstemci Kitaplığı](https://azure.github.io/azure-storage-cpp)
* [Azure Depolama belgeleri](https://azure.microsoft.com/documentation/services/storage/)
