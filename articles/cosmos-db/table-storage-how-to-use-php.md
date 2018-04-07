---
title: Azure depolama tablo hizmeti veya Azure Cosmos DB tablo API php'den nasıl kullanılacağını | Microsoft Docs
description: Php'den Table hizmeti API'si oluşturma ve bir tablo silme kullanmayı öğrenin ve ekleme, silme ve tablo sorgu.
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 02/22/2018
ms.author: mimig
ms.openlocfilehash: 69fa1f8f9717d1ea4ca5081a45317cff8d752809
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-use-azure-storage-table-service-or-cosmos-db-table-api-from-php"></a>Azure depolama tablo hizmeti veya php'den Cosmos DB tablo API kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure depolama tablo hizmeti ve Azure Cosmos DB tablo API kullanarak genel senaryolar gerçekleştirme gösterir. PHP ve kullanım örnekleri yazılır [Azure Storage tablo PHP istemci Kitaplığı][download]. Kapsamdaki senaryolar dahil **oluşturma ve bir tablo silme**, ve **ekleme, silme ve bir tablo varlıkları sorgulama**. Azure tablo hizmeti hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="create-an-azure-service-account"></a>Bir Azure hizmet hesabı oluşturma

Azure Table storage veya Azure Cosmos DB tablo API kullanarak tabloları ile çalışabilirsiniz. Okuyarak Hizmetleri arasındaki farklar hakkında daha fazla bilgiyi [tablo teklifleri](table-introduction.md#table-offerings). Hizmeti için kullanacağınız bir hesap oluşturmanız gerekir. 

### <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma

İlk Depolama hesabınızı oluşturmak için en kolay yolu kullanmaktır [Azure portal](https://portal.azure.com). Daha fazla bilgi için bkz. [Depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md#create-a-storage-account).

Kullanarak bir depolama hesabı oluşturabilirsiniz [Azure PowerShell](../storage/common/storage-powershell-guide-full.md) veya [Azure CLI](../storage/common/storage-azure-cli.md).

Bir depolama hesabı şu anda oluşturmamayı tercih ederseniz, çalıştırmak ve yerel bir ortamda kodunuzu test etmek için Azure Storage öykünücüsü de kullanabilirsiniz. Daha fazla bilgi için bkz. [Geliştirme ve test için Azure depolama öykünücüsünü kullanma](../storage/common/storage-use-emulator.md).

### <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Bir Azure Cosmos DB hesap oluşturma ile ilgili yönergeler için bkz: [tablo API hesabı oluşturma](create-table-dotnet.md#create-a-database-account).

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma

Azure depolama tablosu SDK'sı sınıflarda PHP'nin için kodunuzu içinde başvurmak için depolama tablo hizmeti veya Azure Cosmos DB tablo API erişmek için bir PHP uygulaması oluşturmak için tek gereksinim olmasıdır. Not Defteri dahil olmak üzere uygulamanızı oluşturmak için tüm geliştirme araçlarını kullanabilirsiniz.

Bu kılavuzda, depolama tablo hizmeti veya PHP uygulamanızda yerel olarak veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kodu çağrılabilir Azure Cosmos DB özellikleri kullanın.

## <a name="get-the-client-library"></a>İstemci Kitaplığı Al

1. Projenizi kök Composer.JSON adlı bir dosya oluşturun ve aşağıdaki kodu ekleyin:
```json
{
  "require": {
    "microsoft/azure-storage-table": "*"
  }
}
```
2. Karşıdan [composer.phar](http://getcomposer.org/composer.phar) , kök. 
3. Bir komut istemi açın ve proje kök dizininde aşağıdaki komutu yürütün:
```
php composer.phar install
```
Alternatif olarak, Git [Azure Storage tablo PHP istemci Kitaplığı](https://github.com/Azure/azure-storage-php/tree/master/azure-storage-table) kaynak kodunu kopyalama github'da.


## <a name="add-required-references"></a>Gerekli başvurular ekleyin
Depolama tablo hizmeti ya da Azure Cosmos DB API'leri kullanmak için yapmanız gerekir:

* Otomatik Yükleyiciden kullanarak dosya başvuru [require_once] [ require_once] deyimi, ve
* Kullandığınız tüm sınıflar başvuru.

Aşağıdaki örnek otomatik Yükleyiciden dosya ve başvuru dahil gösterilmektedir **TableRestProxy** sınıfı.

```php
require_once 'vendor/autoload.php';
use MicrosoftAzure\Storage\Table\TableRestProxy;
```

Aşağıdaki örneklerde `require_once` deyimi her zaman gösterilir, ancak yalnızca örnek yürütmek gerekli sınıfları başvurulur.

## <a name="add-a-storage-table-service-connection"></a>Bir depolama tablo Hizmeti Bağlantısı Ekle
Depolama tablo hizmeti istemcisi örneği oluşturmak için öncelikle geçerli bir bağlantı dizesi olması gerekir. Depolama tablo hizmeti bağlantı dizesini biçimdedir:

```php
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"
```

## <a name="add-an-azure-cosmos-db-connection"></a>Bir Azure Cosmos DB Bağlantısı Ekle
Bir Azure Cosmos DB tablosu istemci örneği oluşturmak için önce geçerli bir bağlantı dizesi olması gerekir. Azure Cosmos DB bağlantı dizesi biçimi şöyledir:

```php
$connectionString = "DefaultEndpointsProtocol=[https];AccountName=[myaccount];AccountKey=[myaccountkey];TableEndpoint=[https://myendpoint/]";
```

## <a name="add-a-storage-emulator-connection"></a>Depolama öykünücüsü Bağlantı Ekle
Öykünücü depolama erişmek için:

```php
UseDevelopmentStorage = true
```

Bir Azure tablo hizmeti istemcisi veya Azure Cosmos DB istemci oluşturmak için kullanmanız gerekir **TableRestProxy** sınıfı. Şunları yapabilirsiniz:

* doğrudan bağlantı dizesi geçirin veya
* Kullanım **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * Varsayılan olarak, bir dış kaynak - ortam değişkenleri için destek ile gelir.
  * Genişleterek yeni kaynakları ekleyebilirsiniz `ConnectionStringSource` sınıfı.

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;

$tableClient = TableRestProxy::createTableService($connectionString);
```

## <a name="create-a-table"></a>Bir tablo oluşturma
A **TableRestProxy** nesne içeren bir tablo oluşturma olanak tanır **createTable** yöntemi. Bir tablo oluştururken, tablo hizmeti zaman aşımı ayarlayabilirsiniz. (Tablo hizmeti zaman aşımı hakkında daha fazla bilgi için bkz: [ayarı zaman aşımları tablo hizmeti işlemleri için][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create Table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Create table.
    $tableClient->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
}
```

Tablo adları kısıtlamaları hakkında bilgi için bkz: [tablo hizmeti veri modelini anlama][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir tabloya bir varlık eklemek için yeni bir oluşturma **varlık** nesne ve ona geçirin **TableRestProxy -> insertEntity**. Bir varlık oluşturduğunuzda, belirtmeniz gerektiğini unutmayın bir `PartitionKey` ve `RowKey`. Bunlar bir varlık için benzersiz tanımlayıcı ve çok diğer varlık özellikleri daha hızlı sorgulanabilir değerlerdir. Sistem kullanır `PartitionKey` tablonun varlıklar birçok depolama düğümleri üzerinde otomatik olarak dağıtmak için. Aynı varlıkla `PartitionKey` aynı düğümde depolanır. (Birden çok varlık aynı düğümde depolanan işlemleri farklı düğümlere saklanan varlıkları üzerinde daha iyi.) `RowKey` Bir varlığın bölüm içinde benzersiz kimliğidir.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableClient->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Tablo özellikleri ve türleri hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama][table-data-model].

**TableRestProxy** sınıfı varlıkları eklemek için iki alternatif yöntem sunar: **insertOrMergeEntity** ve **insertOrReplaceEntity**. Bu yöntemleri kullanmak için yeni bir oluşturma **varlık** ve her iki yöntem için parametre olarak geçirin. Henüz yoksa her yöntem varlık ekler. Varlık zaten varsa, **insertOrMergeEntity** özellikleri zaten mevcutsa özellik değerlerini güncelleştirir ve yeni özellikleri ekler Bunlar yoksa while **insertOrReplaceEntity** tamamen var olan bir varlığı değiştirir. Aşağıdaki örnekte nasıl kullanılacağını gösterir **insertOrMergeEntity**. Varsa varlıkla `PartitionKey` "tasksSeattle" ve `RowKey` "1" zaten mevcut değil, bu eklenir. Ancak, (yukarıdaki örnekte gösterildiği gibi) daha önce eklenmiş, `DueDate` özelliği güncelleştirilir ve `Status` özellik eklenir. `Description` Ve `Location` özellikleri de güncelleştirilmiş değerlerle, etkili bir şekilde bırakmak ancak bunları değişmeden. Bu ikinci iki özellik olmayan eklenen örnekte gösterildiği gibi ancak hedef varlık üzerinde var, mevcut değerlerine değişmeden kalır.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

//Create new entity.
$entity = new Entity();

// PartitionKey and RowKey are required.
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");

// If entity exists, existing properties are updated with new values and
// new properties are added. Missing properties are unchanged.
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate", EdmType::DATETIME, new DateTime()); // Modified the DueDate field.
$entity->addProperty("Location", EdmType::STRING, "Home");
$entity->addProperty("Status", EdmType::STRING, "Complete"); // Added Status field.

try    {
    // Calling insertOrReplaceEntity, instead of insertOrMergeEntity as shown,
    // would simply replace the entity with PartitionKey "tasksSeattle" and RowKey "1".
    $tableClient->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
**TableRestProxy -> getEntity** yöntemi tek bir varlık için sorgulayarak almanıza olanak tanır, `PartitionKey` ve `RowKey`. Bölüm anahtarı aşağıdaki örnekte `tasksSeattle` ve satır anahtarını `1` geçirilen **getEntity** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    $result = $tableClient->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entity = $result->getEntity();

echo $entity->getPartitionKey().":".$entity->getRowKey();
```

## <a name="retrieve-all-entities-in-a-partition"></a>Tüm varlıkları bir bölüme alma
Varlık sorguları filtreleri kullanarak oluşturulur (daha fazla bilgi için bkz: [sorgulama tabloları ve varlıkları][filters]). Bölümdeki tüm varlıkları almak için filtre kullanma "PartitionKey eq *bölüm_adı*". Aşağıdaki örnek, tüm varlıkları almak gösterilmiştir `tasksSeattle` filtre geçirerek bölüm **queryEntities** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entities-in-a-partition"></a>Bir alt kümesini varlıkları bir bölüme alma
Önceki örnekte kullanılan aynı düzeni, bunların bir alt kümesini varlıkları bir bölüme almak için kullanılabilir. Aldığınız varlıkların alt kullandığınız Filtresi tarafından belirlenir (daha fazla bilgi için bkz: [sorgulama tabloları ve varlıkları][filters]). Aşağıdaki örnekte belirli bir ile tüm varlıkları almak için bir filtre kullanmayı gösterir `Location` ve `DueDate` belirtilen bir tarih küçüktür.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableClient->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

$entities = $result->getEntities();

foreach($entities as $entity){
    echo $entity->getPartitionKey().":".$entity->getRowKey()."<br />";
}
```

## <a name="retrieve-a-subset-of-entity-properties"></a>Varlık özellikleri kümesini Al
Varlık özelliklerinin bir alt sorgu alabilirsiniz. Adlı bu teknik *projeksiyon*, bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Almak üzere bir özelliğe belirtmek için özelliğin adını geçirmek **sorgu addSelectField ->** yöntemi. Daha fazla özellik eklemek için birden çok kez bu yöntemi çağırın. Yürütme sonrasında **TableRestProxy -> queryEntities**, döndürülen varlıkları yalnızca seçilen özellikleri sahip olur. (Bir alt tablo varlıkların dönmek istiyorsanız, bir filtre sorguları yukarıda gösterildiği gibi kullanın.)

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableClient->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}

// All entities in the table are returned, regardless of whether
// they have the Description field.
// To limit the results returned, use a filter.
$entities = $result->getEntities();

foreach($entities as $entity){
    $description = $entity->getProperty("Description")->getValue();
    echo $description."<br />";
}
```

## <a name="update-an-entity"></a>Bir varlığı güncelleştirir
Var olan bir varlığı güncelleştirebilirsiniz **varlık setProperty ->** ve **varlık addProperty ->** varlık ve ardından arama yöntemleri **TableRestProxy updateEntity->**. Aşağıdaki örnekte bir varlığı alır, bir özelliğini değiştirir, başka bir özellik kaldırır ve yeni bir özellik ekler. Bir özelliğin değerini ayarlayarak kaldırabileceğini unutmayın **null**.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

$result = $tableClient->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();
$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.
$entity->setPropertyValue("Location", null); //Removed Location.
$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableClient->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı silmek için tablo adı ve varlığın geçmesi `PartitionKey` ve `RowKey` için **TableRestProxy -> deleteEntity** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete entity.
    $tableClient->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Eşzamanlılık denetleyeceğini kullanarak silinecek bir varlık için Etag ayarlayabilirsiniz **DeleteEntityOptions -> setEtag** yöntemi ve geçirerek **DeleteEntityOptions** nesnesini  **deleteEntity** dördüncü bir parametre olarak.

## <a name="batch-table-operations"></a>Toplu tablo işlemleri
**TableRestProxy -> Toplu** yöntemi birden çok işlem tek bir istekte yürütmesine olanak tanır. Ekleme işlemleri için desen burada içerir **BatchRequest** nesnesi ve ardından geçirme **BatchRequest** nesnesini **TableRestProxy -> Toplu** yöntemi. Bir işlem eklemek için bir **BatchRequest** nesne çağırabilirsiniz aşağıdaki yöntemlerden herhangi birini birden çok kez:

* **addInsertEntity** (bir insertEntity işlem ekler)
* **addUpdateEntity** (bir updateEntity işlem ekler)
* **addMergeEntity** (mergeEntity işlemi ekler)
* **addInsertOrReplaceEntity** (bir insertOrReplaceEntity işlem ekler)
* **addInsertOrMergeEntity** (adds an insertOrMergeEntity operation)
* **addDeleteEntity** (deleteEntity işlemi ekler)

Aşağıdaki örnekte nasıl yürütüleceği gösterilmektedir **insertEntity** ve **deleteEntity** tek bir istek işlemleri. 

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

// Configure a connection string for Storage Table service.
$connectionString = "DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]"

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

// Create list of batch operation.
$operations = new BatchOperations();

$entity1 = new Entity();
$entity1->setPartitionKey("tasksSeattle");
$entity1->setRowKey("2");
$entity1->addProperty("Description", null, "Clean roof gutters.");
$entity1->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity1->addProperty("Location", EdmType::STRING, "Home");

// Add operation to list of batch operations.
$operations->addInsertEntity("mytable", $entity1);

// Add operation to list of batch operations.
$operations->addDeleteEntity("mytable", "tasksSeattle", "1");

try    {
    $tableClient->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Toplu tablo işlemleri hakkında daha fazla bilgi için bkz: [varlık Grup işlemleri gerçekleştirme][entity-group-transactions].

## <a name="delete-a-table"></a>Bir tablo silme
Son olarak, bir tabloyu silmek için tablo adını geçirmek **TableRestProxy -> deleteTable** yöntemi.

```php
require_once 'vendor/autoload.php';

use MicrosoftAzure\Storage\Table\TableRestProxy;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableClient = TableRestProxy::createTableService($connectionString);

try    {
    // Delete table.
    $tableClient->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // https://docs.microsoft.com/rest/api/storageservices/Table-Service-Error-Codes
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure tablo hizmeti ve Azure Cosmos DB öğrendiğinize göre daha fazla bilgi için aşağıdaki bağlantıları izleyin.

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

* [PHP Geliştirici Merkezi](/develop/php/).

[download]: https://packagist.org/packages/microsoft/azure-storage-table
[require_once]: http://php.net/require_once
[table-service-timeouts]: https://docs.microsoft.com/rest/api/storageservices/setting-timeouts-for-table-service-operations

[table-data-model]: https://docs.microsoft.com/rest/api/storageservices/Understanding-the-Table-Service-Data-Model
[filters]: https://docs.microsoft.com/rest/api/storageservices/Querying-Tables-and-Entities
[entity-group-transactions]: https://docs.microsoft.com/rest/api/storageservices/Performing-Entity-Group-Transactions
