---
title: Php'den Table storage kullanma | Microsoft Docs
description: "PHP tablo hizmetinden oluşturmak ve bir tablo silmek için nasıl kullanılacağını öğrenin ve ekleme, silme ve tablo sorgu."
services: cosmos-db
documentationcenter: php
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 1e57f371-6208-4753-b2a0-05db4aede8e3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: php
ms.topic: article
ms.date: 11/03/2017
ms.author: mimig
ms.openlocfilehash: 5cfdfc814d1dfc15149d7b100ba1f6b68e90bb36
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="how-to-use-azure-table-storage-from-php"></a>Php'den Azure Table storage kullanma
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a>Genel Bakış
Bu kılavuz Azure Table hizmetini kullanarak yaygın senaryolar gerçekleştirme gösterir. PHP ve kullanım örnekleri yazılır [PHP için Azure SDK][download]. Kapsamdaki senaryolar dahil **oluşturma ve bir tablo silme ve ekleme, silme ve bir tablo varlıkları sorgulama**. Azure tablo hizmeti hakkında daha fazla bilgi için bkz: [sonraki adımlar](#next-steps) bölümü.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-php-application"></a>PHP uygulaması oluşturma
Azure Table hizmete erişen bir PHP uygulaması oluşturmak için yalnızca Azure SDK'sındaki sınıfların PHP'nin için kodunuzu içinde başvuran gereksinimdir. Not Defteri dahil olmak üzere uygulamanızı oluşturmak için tüm geliştirme araçlarını kullanabilirsiniz.

Bu kılavuzda, bir PHP uygulamanızda yerel olarak veya bir Azure web rolü, çalışan rolü veya Web sitesi içinde çalışan kodu çağrılabilir tablo hizmet özelliklerini kullanın.

## <a name="get-the-azure-client-libraries"></a>Azure istemci kitaplıkları Al
[!INCLUDE [get-client-libraries](../../includes/get-client-libraries.md)]

## <a name="configure-your-application-to-access-the-table-service"></a>Tablo hizmete erişmek için uygulamanızı yapılandırın
Azure tablo hizmeti API'ları kullanmak için aktarmanız gerekir:

1. Otomatik Yükleyiciden kullanarak dosya başvuru [require_once] [ require_once] deyimi, ve
2. Kullanabileceğinize sınıfları başvuru.

Aşağıdaki örnek otomatik Yükleyiciden dosya ve başvuru dahil gösterilmektedir **ServicesBuilder** sınıfı.

> [!NOTE]
> Bu makaledeki örneklerde oluşturucu aracılığıyla Azure için PHP istemci kitaplıkları yüklü olduğunu varsayalım. Başvuruda bulunmanız kitaplıklarını el ile yüklediyseniz, <code>WindowsAzure.php</code> otomatik yükleyici dosyası.
>
>

```php
require_once 'vendor/autoload.php';
use WindowsAzure\Common\ServicesBuilder;
```

Aşağıdaki örneklerde `require_once` deyimi her zaman gösterilir, ancak yalnızca örnek yürütmek gerekli sınıfları başvurulur.

## <a name="set-up-an-azure-storage-connection"></a>Bir Azure depolama bağlantı kurma
Azure tablo hizmeti istemcisi örneği oluşturmak için öncelikle geçerli bir bağlantı dizesi olması gerekir. Tablo hizmeti bağlantı dizesini biçimdedir:

Canlı hizmetine erişmek için:

```php
DefaultEndpointsProtocol=[http|https];AccountName=[yourAccount];AccountKey=[yourKey]
```

Öykünücü depolama erişmek için:

```php
UseDevelopmentStorage=true
```

Herhangi bir Azure hizmeti istemcisi oluşturmak için kullanmanız gerekir **ServicesBuilder** sınıfı. Şunları yapabilirsiniz:

* doğrudan bağlantı dizesi geçirin veya
* kullanmak **CloudConfigurationManager (CCM)** bağlantı dizesi için dış kaynaklardan denetlemek için:
  * bir dış kaynak - ortam değişkenleri için destek ile birlikte varsayılan olarak,
  * genişleterek yeni kaynakları ekleyebilirsiniz **ConnectionStringSource** sınıfı

Burada özetlenen örnekler için bağlantı dizesi doğrudan geçirilir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;

$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);
```

## <a name="create-a-table"></a>Bir tablo oluşturma
A **TableRestProxy** nesne içeren bir tablo oluşturma olanak tanır **createTable** yöntemi. Bir tablo oluştururken, tablo hizmeti zaman aşımı ayarlayabilirsiniz. (Tablo hizmeti zaman aşımı hakkında daha fazla bilgi için bkz: [ayarı zaman aşımları tablo hizmeti işlemleri için][table-service-timeouts].)

```php
require_once 'vendor\autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Create table.
    $tableRestProxy->createTable("mytable");
}
catch(ServiceException $e){
    $code = $e->getCode();
    $error_message = $e->getMessage();
    // Handle exception based on error codes and messages.
    // Error codes and messages can be found here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
}
```

Tablo adları kısıtlamaları hakkında bilgi için bkz: [tablo hizmeti veri modelini anlama][table-data-model].

## <a name="add-an-entity-to-a-table"></a>Tabloya bir varlık ekleme
Bir tabloya bir varlık eklemek için yeni bir oluşturma **varlık** nesne ve ona geçirin **TableRestProxy -> insertEntity**. Bir varlık oluşturduğunuzda, belirtmeniz gerektiğini unutmayın bir `PartitionKey` ve `RowKey`. Bunlar bir varlık için benzersiz tanımlayıcı ve çok diğer varlık özellikleri daha hızlı sorgulanabilir değerlerdir. Sistem kullanır `PartitionKey` tablonun varlıklar birçok depolama düğümleri üzerinde otomatik olarak dağıtmak için. Aynı varlıkla `PartitionKey` aynı düğümde depolanır. (Birden çok varlık aynı düğümde depolanan işlemleri farklı düğümlere saklanan varlıkları üzerinde daha iyi.) `RowKey` Bir varlığın bölüm içinde benzersiz kimliğidir.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$entity = new Entity();
$entity->setPartitionKey("tasksSeattle");
$entity->setRowKey("1");
$entity->addProperty("Description", null, "Take out the trash.");
$entity->addProperty("DueDate",
                        EdmType::DATETIME,
                        new DateTime("2012-11-05T08:15:00-08:00"));
$entity->addProperty("Location", EdmType::STRING, "Home");

try{
    $tableRestProxy->insertEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
}
```

Tablo özellikleri ve türleri hakkında daha fazla bilgi için bkz: [tablo hizmeti veri modelini anlama][table-data-model].

**TableRestProxy** sınıfı varlıkları eklemek için iki alternatif yöntem sunar: **insertOrMergeEntity** ve **insertOrReplaceEntity**. Bu yöntemleri kullanmak için yeni bir oluşturma **varlık** ve her iki yöntem için parametre olarak geçirin. Henüz yoksa her yöntem varlık ekler. Varlık zaten varsa, **insertOrMergeEntity** özellikleri zaten mevcutsa özellik değerlerini güncelleştirir ve yeni özellikleri ekler Bunlar yoksa while **insertOrReplaceEntity** tamamen var olan bir varlığı değiştirir. Aşağıdaki örnekte nasıl kullanılacağını gösterir **insertOrMergeEntity**. Varsa varlıkla `PartitionKey` "tasksSeattle" ve `RowKey` "1" zaten mevcut değil, bu eklenir. Ancak, (yukarıdaki örnekte gösterildiği gibi) daha önce eklenmiş, `DueDate` özelliği güncelleştirilir ve `Status` özellik eklenir. `Description` Ve `Location` özellikleri de güncelleştirilmiş değerlerle, etkili bir şekilde bırakmak ancak bunları değişmeden. Bu ikinci iki özellik olmayan eklenen örnekte gösterildiği gibi ancak hedef varlık üzerinde var, mevcut değerlerine değişmeden kalır.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

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
    $tableRestProxy->insertOrMergeEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="retrieve-a-single-entity"></a>Tek bir varlık alma
**TableRestProxy -> getEntity** yöntemi tek bir varlık için sorgulayarak almanıza olanak tanır, `PartitionKey` ve `RowKey`. Bölüm anahtarı aşağıdaki örnekte `tasksSeattle` ve satır anahtarını `1` geçirilen **getEntity** yöntemi.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    $result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
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

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "PartitionKey eq 'tasksSeattle'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
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

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$filter = "Location eq 'Office' and DueDate lt '2012-11-5'";

try    {
    $result = $tableRestProxy->queryEntities("mytable", $filter);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
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
Varlık özelliklerinin bir alt sorgu alabilirsiniz. Adlı bu teknik *projeksiyon*, bant genişliğini azaltır ve özellikle büyük varlıklar için sorgu performansını iyileştirebilir. Alınacak bir özelliği belirtmek için özelliğin adını geçirmek **sorgu addSelectField ->** yöntemi. Daha fazla özellik eklemek için birden çok kez bu yöntemi çağırın. Yürütme sonrasında **TableRestProxy -> queryEntities**, döndürülen varlıkları yalnızca seçilen özellikleri sahip olur. (Bir alt tablo varlıkların dönmek istiyorsanız, bir filtre sorguları yukarıda gösterildiği gibi kullanın.)

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\QueryEntitiesOptions;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$options = new QueryEntitiesOptions();
$options->addSelectField("Description");

try    {
    $result = $tableRestProxy->queryEntities("mytable", $options);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
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
Var olan bir varlığı kullanarak güncelleştirilebilir **varlık setProperty ->** ve **varlık addProperty ->** varlık ve ardından arama yöntemleri **TableRestProxy -> updateEntity**. Aşağıdaki örnekte bir varlığı alır, bir özelliğini değiştirir, başka bir özellik kaldırır ve yeni bir özellik ekler. Bir özelliğin değerini ayarlayarak kaldırabileceğini unutmayın **null**.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

$result = $tableRestProxy->getEntity("mytable", "tasksSeattle", 1);

$entity = $result->getEntity();

$entity->setPropertyValue("DueDate", new DateTime()); //Modified DueDate.

$entity->setPropertyValue("Location", null); //Removed Location.

$entity->addProperty("Status", EdmType::STRING, "In progress"); //Added Status.

try    {
    $tableRestProxy->updateEntity("mytable", $entity);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="delete-an-entity"></a>Bir varlığı silme
Bir varlığı silmek için tablo adı ve varlığın geçmesi `PartitionKey` ve `RowKey` için **TableRestProxy -> deleteEntity** yöntemi.

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete entity.
    $tableRestProxy->deleteEntity("mytable", "tasksSeattle", "2");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

Not eşzamanlılık denetleyeceğini kullanarak silinecek bir varlık için Etag ayarlayabilirsiniz **DeleteEntityOptions -> setEtag** yöntemi ve geçirerek **DeleteEntityOptions** nesnesini **deleteEntity** dördüncü bir parametre olarak.

## <a name="batch-table-operations"></a>Toplu tablo işlemleri
**TableRestProxy -> Toplu** yöntemi birden çok işlem tek bir istekte yürütmesine olanak tanır. Ekleme işlemleri için desen burada içerir **BatchRequest** nesnesi ve ardından geçirme **BatchRequest** nesnesini **TableRestProxy -> Toplu** yöntemi. Bir işlem eklemek için bir **BatchRequest** nesne çağırabilirsiniz aşağıdaki yöntemlerden herhangi birini birden çok kez:

* **addInsertEntity** (bir insertEntity işlem ekler)
* **addUpdateEntity** (bir updateEntity işlem ekler)
* **addMergeEntity** (mergeEntity işlemi ekler)
* **addInsertOrReplaceEntity** (bir insertOrReplaceEntity işlem ekler)
* **addInsertOrMergeEntity** (bir insertOrMergeEntity işlem ekler)
* **addDeleteEntity** (deleteEntity işlemi ekler)

Aşağıdaki örnekte nasıl yürütüleceği gösterilmektedir **insertEntity** ve **deleteEntity** tek bir istek işlemleri:

```php
require_once 'vendor/autoload.php';

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;
use MicrosoftAzure\Storage\Table\Models\Entity;
use MicrosoftAzure\Storage\Table\Models\EdmType;
use MicrosoftAzure\Storage\Table\Models\BatchOperations;

    // Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

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
    $tableRestProxy->batch($operations);
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
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

use WindowsAzure\Common\ServicesBuilder;
use MicrosoftAzure\Storage\Common\ServiceException;

// Create table REST proxy.
$tableRestProxy = ServicesBuilder::getInstance()->createTableService($connectionString);

try    {
    // Delete table.
    $tableRestProxy->deleteTable("mytable");
}
catch(ServiceException $e){
    // Handle exception based on error codes and messages.
    // Error codes and messages are here:
    // http://msdn.microsoft.com/library/azure/dd179438.aspx
    $code = $e->getCode();
    $error_message = $e->getMessage();
    echo $code.": ".$error_message."<br />";
}
```

## <a name="next-steps"></a>Sonraki adımlar
Azure tablo hizmetinin öğrendiğinize göre daha karmaşık depolama görevleri hakkında bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.

* [PHP Geliştirici Merkezi](/develop/php/).

[download]: http://go.microsoft.com/fwlink/?LinkID=252473
[require_once]: http://php.net/require_once
[table-service-timeouts]: http://msdn.microsoft.com/library/azure/dd894042.aspx

[table-data-model]: http://msdn.microsoft.com/library/azure/dd179338.aspx
[filters]: http://msdn.microsoft.com/library/azure/dd894031.aspx
[entity-group-transactions]: http://msdn.microsoft.com/library/azure/dd894038.aspx
