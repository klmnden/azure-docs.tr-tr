---
title: "Azure işlevleri depolama tablosu bağlamaları | Microsoft Docs"
description: "Azure Storage bağlamaları Azure işlevlerini kullanmak nasıl anlayın."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: 65b3437e-2571-4d3f-a996-61a74b50a1c2
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/28/2016
ms.author: chrande
ms.openlocfilehash: 486b7c31c914ba7bb2d75e3f83ccf346a09104e8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-storage-table-bindings"></a>Azure işlevleri depolama tablo bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod Azure Storage tablo bağlamaları Azure işlevlerinde açıklanmaktadır. Giriş ve Azure Storage tablolarının bağlantılarında çıktı Azure işlevleri destekler.

Depolama Tablo Bağlama aşağıdaki senaryoları destekler:

* **C# veya Node.js işlevi tek bir satırda okuma** - ayarlanmış `partitionKey` ve `rowKey`. `filter` Ve `take` özellikleri bu senaryoda kullanılmaz.
* **C# işlevinde birden çok satır okuma** -işlevler çalışma zamanı sağlayan bir `IQueryable<T>` nesnesi tabloya bağlı. Tür `T` öğesinden türetilmelidir `TableEntity` veya uygulayan `ITableEntity`. `partitionKey`, `rowKey`, `filter`, Ve `take` özellikler bu senaryoda kullanılmaz; kullanabilirsiniz `IQueryable` tüm gerekli filtreleme yapmak için nesne. 
* **Bir düğüm işlevinde birden çok satır okuma** - ayarlanmış `filter` ve `take` özellikleri. Ayarlamazsanız `partitionKey` veya `rowKey`.
* **C# işlevinde bir veya daha fazla satır yazma** -işlevler çalışma zamanı sağlar bir `ICollector<T>` veya `IAsyncCollector<T>` tabloya bağlı olduğu `T` eklemek istediğiniz varlıklar şeması belirtir. Genellikle, yazın `T` türetilen `TableEntity` veya uygulayan `ITableEntity`, ancak gerekli değildir. `partitionKey`, `rowKey`, `filter`, Ve `take` özellikleri bu senaryoda kullanılmaz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="input"></a>

## <a name="storage-table-input-binding"></a>Depolama tablo giriş bağlama
Azure Storage tablo giriş bağlama depolama tablosu, işlevinde kullanmanıza olanak sağlar. 

Bir işlev depolama tablo girişi aşağıdaki JSON nesneleri kullanan `bindings` function.json dizisi:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "in",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to read - see below>",
    "rowKey": "<RowKey of table entity to read - see below>",
    "take": "<Maximum number of entities to read in Node.js - optional>",
    "filter": "<OData filter expression for table input in Node.js - optional>",
    "connection": "<Name of app setting - see below>",
}
```

Şunlara dikkat edin: 

* Kullanım `partitionKey` ve `rowKey` birlikte tek bir varlık okunamıyor. Bu özellikleri isteğe bağlıdır. 
* `connection`Depolama bağlantı dizesi içeren bir uygulama ayarı adı içermelidir. Standart Düzenleyici'de Azure portalında **tümleştir** sekmesinde, bir depolama alanı oluşturduğunuzda, hesap ya da mevcut bir seçer için bu uygulama ayarı yapılandırır. Ayrıca [bu uygulamayı el ile ayarlama yapılandırma](functions-how-to-use-azure-function-app-settings.md#settings).  

<a name="inputusage"></a>

## <a name="input-usage"></a>Giriş kullanımı
C# işlevlerde, giriş tablosu varlık (veya varlıklar) adlandırılmış bir parametre gibi işlevi imzanız kullanarak bağladığınız `<T> <name>`.
Burada `T` veri türü, verileri seri durumdan istediğiniz olduğunda ve `paramName` , belirtilen adı [bağlama giriş](#input). Node.js işlevlerde kullanarak giriş tablosu varlık (veya varlıklar) erişim `context.bindings.<name>`.

Giriş verisi Node.js veya C# işlevlerde serisi. Seri durumdan çıkarılmış nesneler sahip `RowKey` ve `PartitionKey` özellikleri.

C# işlevleri, şu türlerden birine de bağlayabilirsiniz ve işlevleri çalışma zamanı türü kullanarak tablo verileri seri durumdan dener:

* Uygulayan herhangi bir türü`ITableEntity`
* `IQueryable<T>`

<a name="inputsample"></a>

## <a name="input-sample"></a>Giriş örneği
Tek bir tablo satırı okumak için bir sıra tetikleyici kullanır aşağıdaki function.json sahip olması. JSON belirtir `PartitionKey`  
 `RowKey`. `"rowKey": "{queueTrigger}"`Satır anahtarını kuyruk iletisi dizeden geldiğini belirtir.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
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
      "connection": "MyStorageConnection",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Tek tablo varlığı okur dile özgü örneğe bakın.

* [C#](#inputcsharp)
* [F#](#inputfsharp)
* [Node.js](#inputnodejs)

<a name="inputcsharp"></a>

### <a name="input-sample-in-c"></a>C# giriş örneği #
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

<a name="inputfsharp"></a>

### <a name="input-sample-in-f"></a>F # giriş örneği #
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

<a name="inputnodejs"></a>

### <a name="input-sample-in-nodejs"></a>Node.js giriş örneği
```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);
    context.log('Person entity name: ' + context.bindings.personEntity.Name);
    context.done();
};
```

<a name="output"></a>

## <a name="storage-table-output-binding"></a>Bağlama depolama tablo çıktısı
Etkinleştirir bağlama Azure Storage tablo çıktısı, varlıklar bir depolama alanına yazmak için işlevinde tablo. 

Bir işlev aşağıdaki JSON nesneleri kullanan çıktısı depolama tablosu `bindings` function.json dizisi:

```json
{
    "name": "<Name of input parameter in function signature>",
    "type": "table",
    "direction": "out",
    "tableName": "<Name of Storage table>",
    "partitionKey": "<PartitionKey of table entity to write - see below>",
    "rowKey": "<RowKey of table entity to write - see below>",
    "connection": "<Name of app setting - see below>",
}
```

Şunlara dikkat edin: 

* Kullanım `partitionKey` ve `rowKey` birlikte tek bir varlık yazmak için. Bu özellikleri isteğe bağlıdır. Ayrıca belirtebilirsiniz `PartitionKey` ve `RowKey` işlevi kodunuzda varlık nesnesi oluşturduğunuzda.
* `connection`Depolama bağlantı dizesi içeren bir uygulama ayarı adı içermelidir. Standart Düzenleyici'de Azure portalında **tümleştir** sekmesinde, bir depolama alanı oluşturduğunuzda, hesap ya da mevcut bir seçer için bu uygulama ayarı yapılandırır. Ayrıca [bu uygulamayı el ile ayarlama yapılandırma](functions-how-to-use-azure-function-app-settings.md#settings). 

<a name="outputusage"></a>

## <a name="output-usage"></a>Çıktı kullanımı
C# İşlevler, tablo çıktısı adlandırılmış kullanarak bağladığınız `out` işlevi imzanız parametresinde ister `out <T> <name>`, burada `T` veri türü, verileri seri hale getirmek istediğiniz olduğunda ve `paramName` , belirtilen adı [bağlama çıktı](#output). Node.js işlevlerde kullanarak çıktıyı tabloya erişim `context.bindings.<name>`.

Node.js veya C# işlevleri nesneleri seri hale getirebilir. C# işlevlerde, aşağıdaki türlerine bağlayabilirsiniz:

* Uygulayan herhangi bir türü`ITableEntity`
* `ICollector<T>`(birden çok varlık çıkarmak için. Bkz: [örnek](#outcsharp).)
* `IAsyncCollector<T>`(zaman uyumsuz sürümü `ICollector<T>`)
* `CloudTable`(Azure depolama SDK'sını kullanma. Bkz: [örnek](#readmulti).)

<a name="outputsample"></a>

## <a name="output-sample"></a>Çıkış örneği
Aşağıdaki *function.json* ve *run.csx* örnek nasıl birden çok tablo varlıkları yazılacağını gösterir.

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
      "connection": "MyStorageConnection",
      "name": "tableBinding",
      "type": "table",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Birden çok tablo varlıkları oluşturur dile özgü örneğe bakın.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>C# çıktı örneği #
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
<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>F # çıktı örneği #
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

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Node.js çıktı örneği
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

<a name="readmulti"></a>

## <a name="sample-read-multiple-table-entities-in-c"></a>Örnek: C# birden çok tablo varlıkları okuma  #
Aşağıdaki *function.json* ve C# kod örneği sıra iletide belirtilen bir bölüm anahtarı için varlıklar okur.

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "tableBinding",
      "type": "table",
      "connection": "MyStorageConnection",
      "tableName": "Person",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

C# kodu, Azure depolama SDK'sına bir başvuru ekler, böylece varlık türü öğesinden türetilen `TableEntity`.

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

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

