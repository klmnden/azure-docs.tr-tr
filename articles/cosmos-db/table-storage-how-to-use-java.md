---
title: Azure Table storage veya Azure Cosmos DB tablo API Java'dan nasıl kullanılacağını | Microsoft Docs
description: Bir NoSQL veri deposu olan Azure Table Storage kullanarak bulutta yapılandırılmış veri depolayın.
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 03/20/2018
ms.author: mimig
ms.openlocfilehash: b11faf56ac700399fc411c7feb9910ada355e952
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="how-to-use-azure-table-storage-or-azure-cosmos-db-table-api-from-java"></a>Azure Table storage veya Azure Cosmos DB tablo API Java'dan kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu makalede Azure Table depolama hizmeti ve Azure Cosmos DB kullanarak genel senaryolar gerçekleştirme gösterilmektedir. Java ve kullanım örnekleri yazılır [Java için Azure depolama SDK'sı][Azure Storage SDK for Java]. Kapsamdaki senaryolar da dahil **oluşturma**, **listeleme**, ve **silme** tabloları, yanı **ekleme**, **sorgulama**, **değiştirme**, ve **silme** bir tabloda varlıklar. Tabloları hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

> [!NOTE]
> Bir SDK'sı, Android cihazlarda Azure depolama kullanan geliştiriciler için kullanılabilir. Daha fazla bilgi için bkz: [Android için Azure depolama SDK'sı][Azure Storage SDK for Android].
>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a>Java uygulaması oluşturma
Bu kılavuzda, bir Java uygulamasında yerel olarak veya bir web rolü ya da Azure çalışan rolünde çalışan kod çalıştırabilirsiniz depolama özelliklerini kullanır.

Bu makaledeki örnekler kullanmak için Java Geliştirme Seti (JDK) yükleyin ve Azure aboneliğinizde bir Azure depolama hesabı oluşturun. Bunu yaptıktan sonra geliştirme sisteminizde içinde listelenen bağımlılıkları ve en düşük gereksinimleri karşıladığını doğrulayın [Java için Azure depolama SDK'sı] [ Azure Storage SDK for Java] github'daki. Sisteminiz bu gereksinimleri karşılıyorsa, indirin ve Java için Azure depolama kitaplıkları bu depoyu sisteminizden yüklemek için yönergeleri izleyebilirsiniz. Bu görevleri tamamladıktan sonra bu makaledeki örnekler kullanan bir Java uygulaması oluşturabilirsiniz.

## <a name="configure-your-application-to-access-table-storage"></a>Tablo depolama alanına erişmek için uygulamanızı yapılandırın
Azure depolama API'leri veya Azure Cosmos DB tablo API erişim tablolar için kullanmak istediğiniz Java dosyanın en üstüne aşağıdaki içeri aktarma deyimlerini ekleyin:

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="add-an-azure-storage-connection-string"></a>Bir Azure depolama bağlantı dizesi Ekle
Bir Azure storage istemci uç noktaları ve Veri Yönetimi Hizmetleri erişmek için kimlik bilgilerini depolamak için bir depolama bağlantı dizesi kullanır. Bir istemci uygulamasında çalıştırırken, depolama hesabınızın adını kullanarak depolama bağlantı dizesi şu biçimde sağlamanız gerekir ve depolama hesabı için birincil erişim anahtarını listelenen [Azure portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. 

Bu örnek, bağlantı dizesi tutmak için statik bir alana nasıl bildirebilir gösterir:

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

## <a name="add-an-azure-cosmos-db-connection-string"></a>Bir Azure Cosmos DB bağlantı dizesi Ekle
Tablo uç noktası ve kimlik bilgilerinizi depolamak için bir bağlantı dizesi bir Azure Cosmos DB hesabını kullanır. Bir istemci uygulamasında çalıştırırken, Azure Cosmos DB hesabınızın adını kullanarak Azure Cosmos DB bağlantı dizesi şu biçimde sağlamanız gerekir ve hesap için birincil erişim anahtarını listelenen [Azure portal](https://portal.azure.com) için *AccountName* ve *AccountKey* değerleri. 

Bu örnek, nasıl Azure Cosmos DB bağlantı dizesi tutmak için statik bir alana bildirebilir gösterir:

```java
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=https;" + 
    "AccountName=your_cosmosdb_account;" + 
    "AccountKey=your_account_key;" + 
    "TableEndpoint=https://your_endpoint;" ;
```

Çalışan bir uygulama içinde bir rolde Azure içinde hizmet yapılandırma dosyasında bu dize depolayabilir *ServiceConfiguration.cscfg*, ve çağrısıyla erişim  **RoleEnvironment.getConfigurationSettings** yöntemi. Bağlantı dizesi alma örneği bir **ayarı** adlı öğe *StorageConnectionString* hizmet yapılandırma dosyasında:

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

Ayrıca, bağlantı dizenizi projenizin config.properties dosyasında depolayabilirsiniz:

```java
StorageConnectionString = DefaultEndpointsProtocol=https;AccountName=your_account;AccountKey=your_account_key;TableEndpoint=https://your_table_endpoint/
```

Aşağıdaki örnekler, aşağıdaki yöntemlerden birini depolama bağlantı dizesini almak için kullanılan olduğunu varsayın.

## <a name="create-a-table"></a>Bir tablo oluşturma
A **CloudTableClient** nesne tabloları ve varlıkları için başvuru nesneleri almak olanak sağlar. Aşağıdaki kod oluşturur bir **CloudTableClient** nesne ve yeni bir oluşturmak için kullandığı **CloudTable** tablo temsil eden nesne adında "Kişiler". 

> [!NOTE]
> Oluşturmak için başka bir yolla **CloudStorageAccount** nesneleri; daha fazla bilgi için bkz: **CloudStorageAccount** içinde [Azure Storage istemci SDK'sı başvurusu]).
>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="list-the-tables"></a>Tablolar listesi
Tabloların bir listesini almak için arama **CloudTableClient.listTables()** iterable bir tablo adlarının listesini almak için yöntem.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Varlıkları özel bir sınıf uygulama kullanarak Java nesnelere eşleme **TableEntity**. Kolaylık sağlamak için **TableServiceEntity** uygulayan sınıf **TableEntity** ve yansıma özelliklerini adlı bir alıcı ve ayarlayıcıya yöntemlerine özellikleri eşlemek için kullanır. Tabloya bir varlık eklemek için önce varlığınızın özelliklerini tanımlayan bir sınıf oluşturun. Aşağıdaki kod bölüm anahtarı olarak müşterinin adını satır anahtarını ve Soyadı kullanan bir varlık sınıfı tanımlar. Birlikte, bir varlığın bölüm ve sıra anahtarı varlığı tabloda benzersiz şekilde tanımlar. Aynı bölüm anahtarına sahip varlıklar farklı bölüm anahtarlarının göre daha hızlı sorgulanabilir.

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

Varlıkları içeren tablo işlemleri gerektiren bir **TableOperation** nesnesi. Bu nesne ile yürütülebilir bir varlık üzerinde gerçekleştirilecek işlemi tanımlayan bir **CloudTable** nesnesi. Aşağıdaki kod yeni bir örneğini oluşturur **CustomerEntity** depolanması için bazı müşteri verileriyle sınıfı. Kodun sonraki çağrılar **TableOperation.insertOrReplace** oluşturmak için bir **TableOperation** bir tabloya bir varlık eklemek için nesne ve yeni ilişkilendirir **CustomerEntity** onunla. Son olarak, kod çağırır **yürütme** yöntemi **CloudTable** "Kişiler" tablosu ve yeni belirten nesnesini **TableOperation**, o "Kişiler" tablosuna yeni müşteri varlığı Ekle veya zaten varsa varlığı değiştirmek için depolama hizmetine bir istek gönderir.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-a-batch-of-entities"></a>Toplu işlem varlık yerleştirme
Toplu işlem varlık tablo hizmeti için bir yazma işlemi ekleyebilirsiniz. Aşağıdaki kod oluşturur bir **TableBatchOperation** nesne sonra üç ekleme işlemleri ona ekler. Her ekleme işlemi yeni bir varlık nesnesi oluşturma değerlerini ayarlama ve ardından çağırma eklenir **Ekle** yöntemi **TableBatchOperation** varlığı yeni bir ekleme işlemi ile ilişkilendirmek için nesne. Sonra çağrılır **yürütme** üzerinde **CloudTable** "Kişiler" Tablo belirten nesnesini ve **TableBatchOperation** tek bir istek depolama hizmetinde tablo işlemleri toplu gönderir nesnesi.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

Toplu işlem dikkat edilecek bazı noktalar:

* En fazla 100 INSERT işlemi, silme, birleştirme, Değiştir, eklemek veya birleştirme ve ekleyebilir veya tek bir toplu işteki herhangi bir birleşimini işlemlerinde değiştirin.
* Toplu işlemdeki tek işlem olması durumunda toplu işlem bir alma işlemi olabilir.
* Tek bir toplu işlemdeki tüm varlıkların bölüm anahtarları aynı olmalıdır.
* Bir toplu işlemi için 4 MB veri yükü sınırlıdır.

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Varlıkları bir bölüme için bir tabloyu sorgulamak için kullanabileceğiniz bir **TableQuery**. Çağrı **TableQuery.from** belirtilen sonuç türü döndüren belirli bir tablo üzerinde bir sorgu oluşturmak için. Aşağıdaki kod 'Smith' bölüm anahtarı olduğu varlıklar için bir filtre belirtir. **TableQuery.generateFilterCondition** sorgular için filtreleri oluşturmak için bir yardımcı yöntemi. Çağrı **nerede** tarafından döndürülen başvuru üzerinde **TableQuery.from** yöntemi sorguya filtresini uygulayın. Zaman sorgu yürütüldüğünde çağrısıyla **yürütme** üzerinde **CloudTable** döndürdüğü nesne bir **yineleyici** ile **CustomerEntity** sonuç türü belirtildi. Daha sonra **yineleyici** döndürülen bir sonuçları kullanmak her bir döngü için. Bu kod konsoluna sorgu sonuçlarındaki her varlığın alanlarını yazdırır.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Bir bölüme bir grup varlık alma
Bir bölümdeki tüm varlıkları sorgulamak istemiyorsanız, bir filtre Karşılaştırma işleçleri kullanarak bir aralığı belirtebilirsiniz. Aşağıdaki kod alfabesinde "satır anahtarı (ad) 'E' kadar bir harf ile başladığı Smith" bölümündeki tüm varlıkları almak için iki filtre birleştirir. Ardından sorgu sonuçlarını yazdırır. Kullanıyorsanız bu kılavuzun bölüm toplu tabloya eklenen varlık ekleme, yalnızca iki varlık bu saati (Ben ve Gamze Smith); döndürülür Jeff Smith dahil değildir.

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
Tek, belirli bir varlığı almak üzere bir sorgu yazabilirsiniz. Aşağıdaki kod çağrıları **TableOperation.retrieve** bölüm anahtarını ve satır müşteri belirtmek üzere parametreler "Jeff Smith" oluşturmak yerine anahtar bir **TableQuery** ve aynı şeyi yapmak için filtreleri kullanma. Çalıştırıldığında, alma işlemi bir koleksiyon yerine yalnızca bir varlık döndürür. **GetResultAsType** yöntemi çevirir atama hedef türü sonucu bir **CustomerEntity** nesnesi. Bu tür sorgu için belirtilen tür ile uyumlu değilse, bir özel durum oluşur. Bir tam bölüm ve satır anahtarını eşleşen hiçbir varlık varsa, bir null değeri döndürülür. Bir sorguda hem bölüm hem de satır anahtarını belirtmek Tablo hizmetinden tek bir varlık almanın en hızlı yoludur.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="modify-an-entity"></a>Bir varlığı değiştirme
Bir varlığı değiştirmek için tablo hizmetinden alın, varlık nesnede değişiklikleri yapın ve değişiklikleri değiştirin veya birleştirme işlemi ile tablo hizmetine geri kaydedin. Aşağıdaki kod mevcut bir müşterinin telefon numarasını değiştirir. Çağırmak yerine **TableOperation.insert** eklemek için yaptığımız gibi bu kod çağırır **TableOperation.replace**. **CloudTable.execute** tablo hizmeti yöntemini çağırır ve bu uygulama, alınan bu yana başka bir uygulama bu süre içinde değiştirmediyse varlık değiştirilir. Bu durum oluştuğunda bir özel durum ve varlık gerekir alınan, değiştirilecek ve yeniden kaydedildi. Bu iyimser eşzamanlılık yeniden deneme deseni, bir dağıtılmış depolama sistemi yaygındır.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="query-a-subset-of-entity-properties"></a>Giriş özellikleri alt kümesi sorgulama
Sorguda bir tabloya bir varlık birkaç özelliği alabilir. Projeksiyon olarak adlandırılan bu yöntem bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Aşağıdaki kodda sorgusu kullanan **seçin** tabloda yalnızca e-posta adresleri varlıkların döndürülecek yöntemi. Sonuçları bir koleksiyona öngörülen **dize** yardımıyla bir **Varlıkçözümleyicisi**, sunucudan döndürülen varlıklar üzerinde tür dönüştürme yapar. İçinde projeksiyon hakkında daha fazla bilgiyi [Azure tabloları: Tanıtımı Upsert ve sorgu projeksiyon][Azure Tables: Introducing Upsert and Query Projection]. Bu kod yalnızca tablo hizmetinde bir hesap kullanırken nedenle projeksiyon yerel depolama öykünücüsünde desteklenmez unutmayın.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="insert-or-replace-an-entity"></a>Eklemek veya bir varlığı değiştirme
Genellikle tabloda zaten varsa bilmeden tabloya bir varlık eklemek istiyorsunuz. Bir ekleme veya değiştirme işlemi, yok veya varolan bir aşması durumunda değiştirmek, varlık ekler tek bir isteği yapmanızı sağlar. Önceki örneklere oluşturma, aşağıdaki kod, ekler veya "Walter Harp için" varlığı değiştirir. Yeni bir varlık oluşturduktan sonra bu kodu çağırır **TableOperation.insertOrReplace** yöntemi. Daha sonra bu kodu çağırır **yürütme** üzerinde **CloudTable** nesne tablo ve ekleme veya tablo işlem parametreleri olarak değiştirin. Bir varlık yalnızca bir kısmını güncelleştirmek için **TableOperation.insertOrMerge** yöntemi yerine kullanılabilir. Bu kod yalnızca tablo hizmetinde bir hesap kullanarak olduğunda çalışması için ekleme veya değiştirme yerel depolama öykünücüsünde desteklenmez unutmayın. Ekleme veya değiştirme ve Ekle-veya-birleştirme bu hakkında daha fazla bilgi edinmek [Azure tabloları: Tanıtımı Upsert ve sorgu projeksiyon][Azure Tables: Introducing Upsert and Query Projection].

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlık, onu aldıktan sonra kolayca silebilirsiniz. Varlık alındıktan sonra arama **TableOperation.delete** varlıkla silin. ' I çağırın **yürütme** üzerinde **CloudTable** nesnesi. Aşağıdaki kod bir müşteri girişini alır ve siler.

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak, aşağıdaki kod, bir depolama hesabından bir tablo siler. Bir tablo sildikten sonra yaklaşık 40 saniye, onu yeniden oluşturulamaz. 

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Java'da Azure tablo hizmeti ile çalışmaya başlama](https://github.com/Azure-Samples/storage-table-java-getting-started)
* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
* [Azure depolama için Java SDK'sı][Azure Storage SDK for Java]
* [Azure Storage istemci SDK'sı başvurusu][Azure Storage istemci SDK'sı başvurusu]
* [Azure Storage REST API][Azure Storage REST API]
* [Azure depolama ekibi blogu][Azure Storage Team Blog]

Daha fazla bilgi için bkz. [Java geliştiricileri için Azure](/java/azure).

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage istemci SDK'sı başvurusu]: http://azure.github.io/azure-storage-java/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
