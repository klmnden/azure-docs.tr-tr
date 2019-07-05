---
title: Bir Azure işlevi dönüş değerini kullanarak
description: Azure işlevleri için dönüş değerleri yönetmeyi öğrenin
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 01/14/2019
ms.author: cshoe
ms.openlocfilehash: 03cf85ab12a8f64d639c09db5ea75002b258aa84
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480275"
---
# <a name="using-the-azure-function-return-value"></a>Azure işlev dönüş değeri kullanma

Bu makalede, nasıl iş dönüş değerleri açıklanmaktadır. bir işlev içinde.

Bir işlev dönüş değerine sahip dillerde bağlayabilirsiniz [çıktı bağlaması](./functions-triggers-bindings.md#binding-direction) dönüş değeri için:

* Bir C# sınıf kitaplığında, yöntemin dönüş değerini çıkış bağlama özniteliğini uygulayın.
* Diğer dillerde ayarlamak `name` özelliğinde *function.json* için `$return`.

Dönüş değeri, birden çok çıkış bağlamaları varsa, bunlardan yalnızca biri için kullanın.

C# ve C# betiği için bir çıkış bağlaması veri göndermek için alternatif yöntemler, `out` parametreleri ve [toplayıcısı nesneleri](functions-reference-csharp.md#writing-multiple-output-values).

Dönüş değeri kullanımını gösteren dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betiği (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)
* [Python](#python-example)

## <a name="c-example"></a>C# örneği

Dönüş değeri için zaman uyumsuz örneği tarafından izlenen bir çıkış bağlaması kullanan aşağıda verilmiştir; C# kodu:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="c-script-example"></a>C# betiği örneği

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Zaman uyumsuz örneği tarafından izlenen C# betik kodu, şu şekildedir:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

## <a name="f-example"></a>F#Örnek

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

İşte F# kod:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="javascript-example"></a>JavaScript örneği

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript'te, ikinci parametresinin, dönüş değeri gider `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

## <a name="python-example"></a>Python örnek

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Python kod aşağıdaki gibidir:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri bağlama hataları işleme](./functions-bindings-errors.md)
