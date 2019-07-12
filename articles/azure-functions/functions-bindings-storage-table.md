---
title: Azure işlevleri için Azure tablo depolama bağlamaları
description: Azure işlevleri'nde Azure tablo depolama bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.openlocfilehash: b815ce95da24b20ff18ea03d637ad85bfe72cb00
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67654259"
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure tablo depolama bağlamaları

Bu makalede, Azure işlevleri'nde Azure tablo depolama bağlamaları ile nasıl çalışılacağı açıklanmaktadır. Giriş ve çıkış bağlamaları Azure tablo depolama için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Tablo depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Table) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Tablo depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="input"></a>Girdi

Azure tablo depolama giriş bağlama, bir Azure depolama hesabındaki bir tabloda okumak için kullanın.

## <a name="input---example"></a>Giriş - örnek

Dile özgü örneğe bakın:

* [C# bir varlığı okuma](#input---c-example---one-entity)
* [C# Iqueryable bağlama](#input---c-example---iqueryable)
* [C# CloudTable bağlama](#input---c-example---cloudtable)
* [C# betiği bir varlığı okuma](#input---c-script-example---one-entity)
* [Iqueryable C# betik bağlama](#input---c-script-example---iqueryable)
* [C# betik bağlama CloudTable](#input---c-script-example---cloudtable)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)
* [Java](#input---java-example)

### <a name="input---c-example---one-entity"></a>-C# örneği - bir varlık giriş

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir tablo satırı okur. 

Satır anahtarı değeri "{queueTrigger}", satır anahtarı, kuyruk iletisi dizeden geldiğini gösterir.

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition", "{queueTrigger}")] MyPoco poco, 
        ILogger log)
    {
        log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
    }
}
```

### <a name="input---c-example---iqueryable"></a>Giriş - C# örneği - Iqueryable

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , birden çok tablo satırları okur. Unutmayın `MyPoco` sınıf türetilir `TableEntity`.

```csharp
public class TableStorage
{
    public class MyPoco : TableEntity
    {
        public string Text { get; set; }
    }

    [FunctionName("TableInput")]
    public static void TableInput(
        [QueueTrigger("table-items")] string input, 
        [Table("MyTable", "MyPartition")] IQueryable<MyPoco> pocos, 
        ILogger log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.LogInformation($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}");
        }
    }
}
```

### <a name="input---c-example---cloudtable"></a>Giriş - C# örneği - CloudTable

`IQueryable` içinde desteklenmeyen [işlevleri v2 çalışma zamanı](functions-versions.md). Kullanmaya alternatiftir bir `CloudTable` Azure depolama SDK'sını kullanarak tablo okumak için yöntem parametresi. Bir Azure işlevleri günlüğü tablosu sorgular 2.x işlevinin bir örnek aşağıda verilmiştir:

```csharp
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;

namespace FunctionAppCloudTable2
{
    public class LogEntity : TableEntity
    {
        public string OriginalName { get; set; }
    }
    public static class CloudTableDemo
    {
        [FunctionName("CloudTableDemo")]
        public static async Task Run(
            [TimerTrigger("0 */1 * * * *")] TimerInfo myTimer, 
            [Table("AzureWebJobsHostLogscommon")] CloudTable cloudTable,
            ILogger log)
        {
            log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

            TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
                TableQuery.CombineFilters(
                    TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
                        "FD2"),
                    TableOperators.And,
                    TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
                        "t")));

            // Execute the query and loop through the results
            foreach (LogEntity entity in 
                await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
            {
                log.LogInformation(
                    $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
            }
        }
    }
}
```

CloudTable kullanma hakkında daha fazla bilgi için bkz. [Azure tablo depolama ile çalışmaya başlama](../cosmos-db/table-storage-how-to-use-dotnet.md).

Bağlanılacak çalışırsanız `CloudTable` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---c-script-example---one-entity"></a>-C# betiği örneği - bir varlık giriş

Aşağıdaki örnek, bir tablo giriş bağlama gösterir. bir *function.json* dosya ve [C# betiği](functions-reference-csharp.md) bağlama kullanan kod. İşlevi, tek bir tablo satırı okumak için bir kuyruk tetikleyicisi kullanır. 

*Function.json* dosyasını belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarı, kuyruk iletisi dizeden geldiğini gösterir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
public static void Run(string myQueueItem, Person personEntity, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    log.LogInformation($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---iqueryable"></a>Giriş - C# betiği örneği - Iqueryable

Aşağıdaki örnek, bir tablo giriş bağlama gösterir. bir *function.json* dosya ve [C# betiği](functions-reference-csharp.md) bağlama kullanan kod. İşlevin varlıklar için bir kuyruk iletisinde belirtilen bir bölüm anahtarı okur.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnectionAppSetting",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodu Azure depolama SDK'sına bir başvuru ekler; böylece varlık türü türeyebilir `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using Microsoft.Extensions.Logging;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.LogInformation($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---c-script-example---cloudtable"></a>Giriş - C# betiği örneği - CloudTable

`IQueryable` içinde desteklenmeyen [işlevleri v2 çalışma zamanı](functions-versions.md). Kullanmaya alternatiftir bir `CloudTable` Azure depolama SDK'sını kullanarak tablo okumak için yöntem parametresi. Bir Azure işlevleri günlüğü tablosu sorgular 2.x işlevinin bir örnek aşağıda verilmiştir:

```json
{
  "bindings": [
    {
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in",
      "schedule": "0 */1 * * * *"
    },
    {
      "name": "cloudTable",
      "type": "table",
      "connection": "AzureWebJobsStorage",
      "tableName": "AzureWebJobsHostLogscommon",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;
using System;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static async Task Run(TimerInfo myTimer, CloudTable cloudTable, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");

    TableQuery<LogEntity> rangeQuery = new TableQuery<LogEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, 
            "FD2"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.GreaterThan, 
            "a")));

    // Execute the query and loop through the results
    foreach (LogEntity entity in 
    await cloudTable.ExecuteQuerySegmentedAsync(rangeQuery, null))
    {
        log.LogInformation(
            $"{entity.PartitionKey}\t{entity.RowKey}\t{entity.Timestamp}\t{entity.OriginalName}");
    }
}

public class LogEntity : TableEntity
{
    public string OriginalName { get; set; }
}
```

CloudTable kullanma hakkında daha fazla bilgi için bkz. [Azure tablo depolama ile çalışmaya başlama](../cosmos-db/table-storage-how-to-use-dotnet.md).

Bağlanılacak çalışırsanız `CloudTable` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

### <a name="input---f-example"></a>Giriş - F# örneği

Aşağıdaki örnek, bir tablo giriş bağlama gösterir. bir *function.json* dosya ve [ F# betik](functions-reference-fsharp.md) bağlama kullanan kod. İşlevi, tek bir tablo satırı okumak için bir kuyruk tetikleyicisi kullanır. 

*Function.json* dosyasını belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarı, kuyruk iletisi dizeden geldiğini gösterir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

İşte F# kod:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.LogInformation(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.LogInformation(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

Aşağıdaki örnek, bir tablo giriş bağlama gösterir. bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlama kullanan. İşlevi, tek bir tablo satırı okumak için bir kuyruk tetikleyicisi kullanır. 

*Function.json* dosyasını belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarı, kuyruk iletisi dizeden geldiğini gösterir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "personEntity",
      "type": "table",
      "tableName": "Person",
      "partitionKey": "Test",
      "rowKey": "{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

### <a name="input---java-example"></a>Giriş - Java örnek

Aşağıdaki örnek, belirli bir bölüme tablo depolamasında toplam öğe sayısını döndüren bir HTTP ile tetiklenen işlev gösterir.

```java
@FunctionName("getallcount")
public int run(
   @HttpTrigger(name = "req",
                 methods = {HttpMethod.GET},
                 authLevel = AuthorizationLevel.ANONYMOUS) Object dummyShouldNotBeUsed,
   @TableInput(name = "items",
                tableName = "mytablename",  partitionKey = "myparkey",
                connection = "myconnvarname") MyItem[] items
) {
    return items.length;
}
```


## <a name="input---attributes"></a>Giriş - öznitelikleri
 
İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), bir tablo giriş bağlama yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs)

  Özniteliğin Oluşturucusu tablo adı, bölüm anahtarını ve satır anahtarı alır. Aşağıdaki örnekte gösterildiği gibi bir out parametresi veya işlevin dönüş değeri kullanılabilmesi için:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}", Connection = "StorageConnectionAppSetting")] MyPoco poco, 
      ILogger log)
  {
      ...
  }
  ```

  Giriş - tam bir örnek için bkz. C# örnek.

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu bir depolama bağlantı dizesi içeren bir uygulama ayarı adı alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("TableInput")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `Table` Özniteliğin `Connection` özelliği.
* `StorageAccount` Özniteliği aynı parametresine uygulanan `Table` özniteliği.
* `StorageAccount` İşleve uygulanmış bir öznitelik.
* `StorageAccount` Sınıfına uygulanan bir öznitelik.
* İşlev uygulaması (uygulama ayarı "AzureWebJobsStorage") için varsayılan depolama hesabı.

## <a name="input---java-annotations"></a>Giriş - Java ek açıklamaları

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@TableInput` ek açıklama parametreleri değerini Table storage'gelmesi.  Bu ek açıklama yerel Java türler, pojo'ları veya isteğe bağlı kullanarak boş değer atanabilir değer ile kullanılabilir<T>. 

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `table`. Bu özellik, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | Ayarlanmalıdır `in`. Bu özellik, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**name** | yok | Tablo veya varlık işlev kodunu temsil eden değişken adı. | 
|**TableName** | **TableName** | Tablonun adı.| 
|**partitionKey** | **partitionKey** |İsteğe bağlı. Okunacak tablo varlığın bölüm anahtarı. Bkz: [kullanım](#input---usage) bölümü bu özelliği kullanmak hakkında yönergeler için.| 
|**RowKey** |**RowKey** | İsteğe bağlı. Okunacak tablo varlığın satır anahtarı. Bkz: [kullanım](#input---usage) bölümü bu özelliği kullanmak hakkında yönergeler için.| 
|**take** |**sınav zamanı** | İsteğe bağlı. Varlıkları JavaScript'te okunacak maksimum sayısı. Bkz: [kullanım](#input---usage) bölümü bu özelliği kullanmak hakkında yönergeler için.| 
|**Filtre** |**Filtre** | İsteğe bağlı. Bir OData filtre ifadesi JavaScript'te giriş tablosu. Bkz: [kullanım](#input---usage) bölümü bu özelliği kullanmak hakkında yönergeler için.| 
|**bağlantı** |**bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

Tablo depolama giriş bağlamasına aşağıdaki senaryoları destekler:

* **C# veya C# betiği bir satırı okuyun**

  Ayarlama `partitionKey` ve `rowKey`. Bir yöntem parametresi kullanarak tablo verilerini erişim `T <paramName>`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. `T` genellikle uygulayan bir tür olan `ITableEntity` veya ondan türetilmiş `TableEntity`. `filter` Ve `take` özellikleri, bu senaryoda kullanılmaz. 

* **C# veya C# betiği bir veya daha fazla satır okuma**

  Bir yöntem parametresi kullanarak tablo verilerini erişim `IQueryable<T> <paramName>`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. `T` uygulayan bir tür olmalıdır `ITableEntity` veya ondan türetilmiş `TableEntity`. Kullanabileceğiniz `IQueryable` gerekli filtreleme yapmak için yöntemleri. `partitionKey`, `rowKey`, `filter`, Ve `take` özellikleri, bu senaryoda kullanılmaz.  

  > [!NOTE]
  > `IQueryable` içinde desteklenmeyen [işlevleri v2 çalışma zamanı](functions-versions.md). Alternatif [CloudTable paramName yöntemi parametresini kullanın](https://stackoverflow.com/questions/48922485/binding-to-table-storage-in-v2-azure-functions-using-cloudtable) Azure depolama SDK'sını kullanarak tablo okumak için. Bağlanılacak çalışırsanız `CloudTable` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

* **JavaScript içinde bir veya daha fazla satır okuma**

  Ayarlama `filter` ve `take` özellikleri. Ayarlamamanız `partitionKey` veya `rowKey`. Giriş tablosu varlık (veya varlıklar) kullanarak erişmek `context.bindings.<name>`. Seri durumdan çıkarılmış nesne sahip `RowKey` ve `PartitionKey` özellikleri.

## <a name="output"></a>Output

Bir Azure tablo depolama çıkış bir Azure depolama hesabındaki bir tabloda varlıklar yazılacak bağlaması kullanın.

> [!NOTE]
> Bu çıkış bağlaması, var olan varlıkları güncelleştirilmesini desteklemiyor. Kullanım `TableOperation.Replace` işlemi [Azure depolama SDK'sı gelen](https://docs.microsoft.com/azure/cosmos-db/tutorial-develop-table-dotnet#delete-an-entity) var olan bir varlığı güncelleştirmek için.   

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir tablo satırı yazmak için bir HTTP tetikleyicisi kullanan. 

```csharp
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, ILogger log)
    {
        log.LogInformation($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bir tablo çıkışına bağlama gösterir. bir *function.json* dosya ve [C# betiği](functions-reference-csharp.md) bağlama kullanan kod. İşlevi, birden çok tablo varlıkları yazar.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, ILogger log)
{
    for (int i = 1; i < 10; i++)
        {
            log.LogInformation($"Adding Person entity {i}");
            tableBinding.Add(
                new Person() { 
                    PartitionKey = "Test", 
                    RowKey = i.ToString(), 
                    Name = "Name" + i.ToString() }
                );
        }

}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}

```

### <a name="output---f-example"></a>Çıkış - F# örneği

Aşağıdaki örnek, bir tablo çıkışına bağlama gösterir. bir *function.json* dosya ve [ F# betik](functions-reference-fsharp.md) bağlama kullanan kod. İşlevi, birden çok tablo varlıkları yazar.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

İşte F# kod:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: ILogger) =
    for i = 1 to 10 do
        log.LogInformation(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bir tablo çıkışına bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, birden çok tablo varlıkları yazar.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "manualTrigger",
      "direction": "in"
    },
    {
      "tableName": "Person",
      "connection": "MyStorageConnectionAppSetting",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context) {

    context.bindings.tableBinding = [];

    for (var i = 1; i < 10; i++) {
        context.bindings.tableBinding.push({
            PartitionKey: "Test",
            RowKey: i.toString(),
            Name: "Name " + i
        });
    }
    
    context.done();
};
```

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Tables/TableAttribute.cs).

Özniteliğin Oluşturucusu tablo adını alır. Üzerinde kullanılabilir bir `out` parametre veya dönüş değeri işlevin, aşağıdaki örnekte gösterildiği gibi:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable", Connection = "StorageConnectionAppSetting")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

Kullanabileceğiniz `StorageAccount` sınıf, yöntem ya da parametre düzeyinde depolama hesabı belirtmek için özniteliği. Daha fazla bilgi için [girişi - öznitelikleri](#input---attributes).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `table`. Bu özellik, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | Ayarlanmalıdır `out`. Bu özellik, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**name** | yok | Tablo veya varlıktan temsil eden işlevi kod içinde kullanılan değişken adı. Kümesine `$return` işlev dönüş değeri başvurmak için.| 
|**TableName** |**TableName** | Tablonun adı.| 
|**partitionKey** |**partitionKey** | Tablo varlığı yazmak için bölüm anahtarı. Bkz: [kullanım bölümüne](#output---usage) bu özelliği kullanmak hakkında yönergeler için.| 
|**RowKey** |**RowKey** | Yazılacak tablo varlığın satır anahtarı. Bkz: [kullanım bölümüne](#output---usage) bu özelliği kullanmak hakkında yönergeler için.| 
|**bağlantı** |**bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

Tablo depolama, bağlamayı desteklediği aşağıdaki senaryolarda çıktı:

* **Herhangi bir dilde bir satır yazın**

  Bir yöntem parametresi gibi kullanarak çıkış Tablo varlığı, C# ve C# betiği erişim `out T paramName` veya işlevin dönüş değeri. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. `T` Bölüm anahtarını ve satır anahtarı tarafından sağlanan serializable bir tür olabilir *function.json* dosya veya `Table` özniteliği. Aksi takdirde, `T` içeren bir tür olmalıdır `PartitionKey` ve `RowKey` özellikleri. Bu senaryoda, `T` genellikle uygulayan `ITableEntity` veya ondan türetilmiş `TableEntity`, ancak bu gerekli değildir.

* **Bir veya daha fazla satır yazmak C# veya C# betiği**

  Bir yöntem parametresi kullanarak çıkış Tablo varlığı, C# ve C# betiği erişim `ICollector<T> paramName` veya `IAsyncCollector<T> paramName`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. `T` eklemek istediğiniz varlıkların şema belirtir. Genellikle, `T` türetildiği `TableEntity` veya uygulayan `ITableEntity`, ancak bu gerekli değildir. Bölüm anahtarını ve satır anahtarı değerleri *function.json* veya `Table` öznitelik oluşturucusunda Bu senaryoda kullanılmaz.

  Kullanmaya alternatiftir bir `CloudTable` tablosuna Azure depolama SDK'sını kullanarak yazmak için yöntem parametresi. Bağlanılacak çalışırsanız `CloudTable` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x). Bağlamalarını kaydetmek için kod örneği için `CloudTable`, giriş bağlama örnekler için bkz: [C#](#input---c-example---cloudtable) veya [C# betiği](#input---c-script-example---cloudtable) bu makalenin üst kısmındaki.

* **JavaScript'te bir veya daha fazla satır yazma**

  JavaScript işlevleri'nde çıkış kullanma tabloya erişim `context.bindings.<name>`.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Tablo | [Tablo hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/table-service-error-codes) |
| BLOB, tablo, kuyruk | [Depolama hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tablo, kuyruk | [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
