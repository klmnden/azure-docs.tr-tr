---
title: "Azure işlevleri için Azure tablo depolama bağlamaları"
description: "Azure Table depolama bağlamaları Azure işlevlerini kullanmak nasıl anlayın."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 5cfb968b201f49d5b7029a0b677e3ce2a8aa6cb9
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="azure-table-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure tablo depolama bağlamaları

Bu makalede Azure Table depolama bağlamaları Azure işlevlerinde ile nasıl çalışılacağını açıklar. Giriş ve Azure tablo depolaması için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="input"></a>Girdi

Bir tablodaki bir Azure Storage hesabı okumak için Azure Table depolama giriş bağlama kullanın.

## <a name="input---example"></a>Girişi - örnek

Dile özgü örneğe bakın:

* [C# bir varlığı okuma](#input---c-example-1)
* [C# birden çok varlık okuma](#input---c-example-2)
* [C# betik - bir varlığı okuma](#input---c-script-example-1)
* [C# betik - birden çok varlık okuma](#input---c-script-example-2)
* [F#](#input---f-example-2)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example-1"></a>Giriş - C# Örnek 1

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir tablo satırı okur. 

Satır anahtar değeri "{queueTrigger}" satır anahtarını kuyruk iletisi dizeden geldiğini belirtir.

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
        TraceWriter log)
    {
        log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
    }
}
```

### <a name="input---c-example-2"></a>Giriş - C# Örnek 2

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) birden çok tablo satırı okur. Unutmayın `MyPoco` sınıfı türer `TableEntity`.

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
        TraceWriter log)
    {
        foreach (MyPoco poco in pocos)
        {
            log.Info($"PK={poco.PartitionKey}, RK={poco.RowKey}, Text={poco.Text}";
        }
    }
}
```

### <a name="input---c-script-example-1"></a>Giriş - C# betik örnek 1

Aşağıdaki örnek, bir tablo giriş bağlama gösterir bir *function.json* dosya ve [C# betik](functions-reference-csharp.md) bağlama kullanan kod. İşlevi bir sıra tetikleyici tek bir tablo satırı okumak için kullanır. 

*Function.json* dosyayı belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarını kuyruk iletisi dizeden geldiğini gösterir.

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

C# betik kod aşağıdaki gibidir:

```csharp
public static void Run(string myQueueItem, Person personEntity, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    log.Info($"Name in Person entity: {personEntity.Name}");
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
}
```

### <a name="input---c-script-example-2"></a>Giriş - C# betik örnek 2

Aşağıdaki örnek, bir tablo giriş bağlama gösterir bir *function.json* dosya ve [C# betik](functions-reference-csharp.md) bağlama kullanan kod. İşlev varlıklar için bir kuyruk iletisi içinde belirtilen bir bölüm anahtarı okur.

Burada *function.json* dosyası:

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

C# betik kodu, Azure depolama SDK'sına bir başvuru ekler, böylece varlık türü öğesinden türetilen `TableEntity`:

```csharp
#r "Microsoft.WindowsAzure.Storage"
using Microsoft.WindowsAzure.Storage.Table;

public static void Run(string myQueueItem, IQueryable<Person> tableBinding, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    foreach (Person person in tableBinding.Where(p => p.PartitionKey == myQueueItem).ToList())
    {
        log.Info($"Name: {person.Name}");
    }
}

public class Person : TableEntity
{
    public string Name { get; set; }
}
```

### <a name="input---f-example"></a>Giriş - F # örnek

Aşağıdaki örnek, bir tablo giriş bağlama gösterir bir *function.json* dosya ve [F # betiği](functions-reference-fsharp.md) bağlama kullanan kod. İşlevi bir sıra tetikleyici tek bir tablo satırı okumak için kullanır. 

*Function.json* dosyayı belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarını kuyruk iletisi dizeden geldiğini gösterir.

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

F # kod aşağıdaki gibidir:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(myQueueItem: string, personEntity: Person) =
    log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
    log.Info(sprintf "Name in Person entity: %s" personEntity.Name)
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

Aşağıdaki örnek, bir tablo giriş bağlama gösterir bir *function.json* dosya ve [JavaScript kodu] bağlama kullanır (işlevleri başvuru node.md). İşlevi bir sıra tetikleyici tek bir tablo satırı okumak için kullanır. 

*Function.json* dosyayı belirtir bir `partitionKey` ve `rowKey`. `rowKey` Değeri "{queueTrigger}" satır anahtarını kuyruk iletisi dizeden geldiğini gösterir.

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

## <a name="input---attributes"></a>Giriş - öznitelikleri
 
İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), tablo giriş bağlama yapılandırmak için aşağıdaki öznitelikler kullanın:

* [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), NuGet paketi tanımlanan [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

  Özniteliğin Oluşturucusu tablo adı, bölüm anahtarını ve satır anahtarını alır. Aşağıdaki örnekte gösterildiği gibi bir out parametresi ya da işlevin dönüş değeri kullanılabilmesi için:

  ```csharp
  [FunctionName("TableInput")]
  public static void Run(
      [QueueTrigger("table-items")] string input, 
      [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, 
      TraceWriter log)
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
      TraceWriter log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz: [giriş - C# örnek](#input---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs), NuGet paketi tanımlı [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs)

  Depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu depolama bağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

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

Depolama hesabı şu sırayla belirlenir:

* `Table` Özniteliğin `Connection` özelliği.
* `StorageAccount` Aynı parametre olarak uygulanan öznitelik `Table` özniteliği.
* `StorageAccount` İşlevi için uygulanan öznitelik.
* `StorageAccount` Sınıfına uygulanan öznitelik.
* İşlev uygulaması ("AzureWebJobsStorage" uygulama ayarı) için varsayılan depolama hesabı.

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | ayarlanmalıdır `table`. Azure portalında bağlama oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | ayarlanmalıdır `in`. Azure portalında bağlama oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | Tablo veya işlev kodu varlığı temsil eden değişken adı. | 
|**tableName** | **TableName** | Tablonun adı.| 
|**partitionKey** | **PartitionKey** |İsteğe bağlı. Okunacak tablo varlığın bölüm anahtarı. Bkz: [kullanım](#input---usage) bölüm bu özelliği kullanmak nasıl hakkında yönergeler için.| 
|**rowKey** |**RowKey** | İsteğe bağlı. Okunacak tablo varlığın satır anahtarı. Bkz: [kullanım](#input---usage) bölüm bu özelliği kullanmak nasıl hakkında yönergeler için.| 
|**Al** |**Al** | İsteğe bağlı. JavaScript'te okumak için varlıklar maksimum sayısı. Bkz: [kullanım](#input---usage) bölüm bu özelliği kullanmak nasıl hakkında yönergeler için.| 
|**Filtre** |**Filtre** | İsteğe bağlı. Bir OData filtre ifadesi JavaScript'te giriş tablosu. Bkz: [kullanım](#input---usage) bölüm bu özelliği kullanmak nasıl hakkında yönergeler için.| 
|**bağlantı** |**Bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

Tablo depolama giriş bağlama aşağıdaki senaryoları destekler:

* **C# veya C# komut satırındaki okuma**

  Ayarlama `partitionKey` ve `rowKey`. Tablo verisi yöntemi parametresini kullanarak erişim `T <paramName>`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. `T`genellikle uygulayan bir tür olduğundan `ITableEntity` veya türetilen `TableEntity`. `filter` Ve `take` özellikleri bu senaryoda kullanılmaz. 

* **C# ya da C# komut dosyasında bir veya daha fazla satır okuma**

  Tablo verisi yöntemi parametresini kullanarak erişim `IQueryable<T> <paramName>`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. `T`uygulayan bir tür olmalıdır `ITableEntity` veya türetilen `TableEntity`. Kullanabileceğiniz `IQueryable` tüm gerekli filtreleme yapmak için yöntemleri. `partitionKey`, `rowKey`, `filter`, Ve `take` özellikleri bu senaryoda kullanılmaz.  

> [!NOTE]
> `IQueryable`işe yaramazsa şekilde .NET Core içinde çalışmıyor [işlevleri v2 çalışma zamanı](functions-versions.md).

  Alternatif kullanmaktır bir `CloudTable paramName` Azure depolama SDK'sını kullanarak tabloyu okumak için yöntem parametresi.

* **JavaScript bir veya daha fazla satır okuma**

  Ayarlama `filter` ve `take` özellikleri. Ayarlamazsanız `partitionKey` veya `rowKey`. Giriş tablosu varlık (veya varlıklar) kullanarak erişim `context.bindings.<name>`. Seri durumdan çıkarılmış nesneler sahip `RowKey` ve `PartitionKey` özellikleri.

## <a name="output"></a>Çıktı

Bir Azure depolama hesabındaki bir tablo varlıkları yazılacak bağlama Azure Table depolama çıktı kullanın.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

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
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }
}
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama tablo çıktısı gösterir bir *function.json* dosya ve [C# betik](functions-reference-csharp.md) bağlama kullanan kod. İşlev birden çok tablo varlıkları yazar.

Burada *function.json* dosyası:

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

C# betik kod aşağıdaki gibidir:

```csharp
public static void Run(string input, ICollector<Person> tableBinding, TraceWriter log)
{
    for (int i = 1; i < 10; i++)
        {
            log.Info($"Adding Person entity {i}");
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

### <a name="output---f-example"></a>Çıktı - F # örnek

Aşağıdaki örnek, bağlama tablo çıktısı gösterir bir *function.json* dosya ve [F # betiği](functions-reference-fsharp.md) bağlama kullanan kod. İşlev birden çok tablo varlıkları yazar.

Burada *function.json* dosyası:

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

F # kod aşağıdaki gibidir:

```fsharp
[<CLIMutable>]
type Person = {
  PartitionKey: string
  RowKey: string
  Name: string
}

let Run(input: string, tableBinding: ICollector<Person>, log: TraceWriter) =
    for i = 1 to 10 do
        log.Info(sprintf "Adding Person entity %d" i)
        tableBinding.Add(
            { PartitionKey = "Test"
              RowKey = i.ToString()
              Name = "Name" + i.ToString() })
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama tablo çıktısı gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev birden çok tablo varlıkları yazar.

Burada *function.json* dosyası:

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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [TableAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), NuGet paketi tanımlanan [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs).

Özniteliğin Oluşturucusu tablo adını alır. Üzerinde kullanılabilir bir `out` parametresi veya aşağıdaki örnekte gösterildiği gibi işlevinin dönüş değeri:

```csharp
[FunctionName("TableOutput")]
[return: Table("MyTable")]
public static MyPoco TableOutput(
    [HttpTrigger] dynamic input, 
    TraceWriter log)
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
    TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

Kullanabileceğiniz `StorageAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde depolama hesabı belirtin. Daha fazla bilgi için bkz: [giriş - öznitelikleri](#input---attributes).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Table` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | ayarlanmalıdır `table`. Azure portalında bağlama oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | ayarlanmalıdır `out`. Azure portalında bağlama oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | Tablo veya varlığı temsil eden işlevi kod içinde kullanılan değişken adı. Kümesine `$return` işlevi dönüş değeri başvurmak için.| 
|**tableName** |**TableName** | Tablonun adı.| 
|**partitionKey** |**PartitionKey** | Yazılacak tablo varlığın bölüm anahtarı. Bkz: [kullanımı bölümü](#output---usage) nasıl bu özelliği kullanmak hakkında yönergeler için.| 
|**rowKey** |**RowKey** | Yazılacak tablo varlığın satır anahtarı. Bkz: [kullanımı bölümü](#output---usage) nasıl bu özelliği kullanmak hakkında yönergeler için.| 
|**bağlantı** |**Bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Table storage bağlama destekler aşağıdaki senaryolarda çıktı:

* **Herhangi bir dilde bir satır yazın**

  Yöntem parametresi gibi kullanarak çıktı tablosu varlık, C# ve C# betik erişim `out T paramName` veya işlev dönüş değeri. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. `T`Bölüm anahtarı ve satır anahtarı tarafından sağlanan herhangi bir seri hale getirilebilir türü olabilir *function.json* dosya veya `Table` özniteliği. Aksi takdirde, `T` içeren bir tür olmalıdır `PartitionKey` ve `RowKey` özellikleri. Bu senaryoda, `T` genellikle uygulayan `ITableEntity` veya türetilen `TableEntity`, ancak gerekli değildir.

* **C# veya C# içinde bir veya daha fazla satır yazma**

  C# ve C# betik çıktı tablosu varlık yöntemi parametresini kullanarak erişim `ICollector<T> paramName` veya `ICollectorAsync<T> paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. `T`eklemek istediğiniz varlıklar şeması belirtir. Genellikle, `T` türetilen `TableEntity` veya uygulayan `ITableEntity`, ancak gerekli değildir. Bölüm anahtarı ve satır anahtarı değerlerini *function.json* veya `Table` öznitelik oluşturucunun Bu senaryoda kullanılmaz.

  Alternatif kullanmaktır bir `CloudTable paramName` yöntem parametresi Azure depolama SDK'sını kullanarak tabloya yazabilirsiniz.

* **Bir veya daha fazla satır yazmanıza**

  JavaScript işlevleri kullanarak çıktıyı tabloya erişim `context.bindings.<name>`.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
