---
title: Azure işlevleri tetikleyici ve bağlama örneği
description: Azure işlev bağlamaları yapılandırmayı öğrenin
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 8372becd614e35bead9e98e405e2694f3f16db4a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66472320"
---
# <a name="azure-functions-trigger-and-binding-example"></a>Azure işlevleri tetikleyici ve bağlama örneği

Bu makalede nasıl yapılandırılacağını gösteren bir [tetikleyicisini ve bağlamalarını](./functions-triggers-bindings.md) bir Azure işlev.

Azure kuyruk depolama alanında yeni bir iletinin göründüğü her durumda, Azure tablo depolama alanına yeni bir satır yazmak istediğiniz varsayalım. Bu senaryo, bir Azure kuyruğundaki kullanarak uygulanabilir depolama tetikleyicisi ve Azure tablo depolama çıkış bağlaması. 

İşte bir *function.json* bu senaryo için dosya. 

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

İçindeki ilk öğeyi `bindings` kuyruk depolama tetikleyicisi dizisidir. `type` Ve `direction` tetikleyici özelliklerini tanımlayın. `name` Özelliği kuyruk iletisi içeriğini alan işlev parametresi tanımlar. İzlemek için kuyruk adı yer `queueName`, ve uygulama ayarının tarafından tanımlanan bağlantı dizesidir `connection`.

İkinci öğe `bindings` dizidir Azure tablo depolama çıktı bağlaması. `type` Ve `direction` bağlama özelliklerini tanımlayın. `name` Dönüş değeri işlevini kullanarak bu durumda, özellik belirtir nasıl yeni bir tablo satırının işlevi sağlar. Tablo adı kullanılıyor `tableName`, ve uygulama ayarının tarafından tanımlanan bağlantı dizesidir `connection`.

İçeriğini görüntüleyip *function.json* Azure portalında **Gelişmiş Düzenleyici** seçeneğini **tümleştir** işlevinizin sekmesi.

> [!NOTE]
> Değerini `connection` değil bağlantı dizesinin kendisinin bağlantı dizesi içeren bir uygulama ayarı adı. Bağlamaları kullanın bağlantı dizeleri en iyi zorlamak için uygulama ayarlarında depolanan uygulama *function.json* hizmet gizli dizileri içermiyor.

## <a name="c-script-example"></a>C# betiği örneği

Aşağıda, bu tetikleyici ve bağlama ile çalışan bir C# kodu verilmiştir. Kuyruk iletisi içeriği sağlayan parametrenin adı olduğuna dikkat edin `order`; çünkü bu adı gereklidir `name` özellik değeri *function.json* olduğu `order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

## <a name="javascript-example"></a>JavaScript örneği

Aynı *function.json* dosya bir JavaScript işlevi ile kullanılabilir:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

## <a name="class-library-example"></a>Sınıf kitaplığı örnek

Bir sınıf kitaplığı, aynı tetikleyici ve bağlama bilgileri &mdash; kuyruk ve tablo adları, depolama hesapları, işlev giriş ve çıkış parametrelerini &mdash; öznitelikleri function.json dosyası tarafından sağlanır. Bir örneği aşağıda verilmiştir:

```csharp
public static class QueueTriggerTableOutput
{
    [FunctionName("QueueTriggerTableOutput")]
    [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
    public static Person Run(
        [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order,
        ILogger log)
    {
        return new Person() {
                PartitionKey = "Orders",
                RowKey = Guid.NewGuid().ToString(),
                Name = order["Name"].ToString(),
                MobileNumber = order["MobileNumber"].ToString() };
    }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

Artık bir Azure kuyruk tarafından tetiklenen ve Azure tablo Depolama'ına verileri çıkarır bir çalışma işlevi vardır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri bağlama ifade desenleri](./functions-bindings-expressions-patterns.md)
