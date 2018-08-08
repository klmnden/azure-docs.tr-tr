---
title: Azure işlevleri için Java Geliştirici Başvurusu | Microsoft Docs
description: Java işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari, java
ms.service: functions
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw
ms.openlocfilehash: 65964372cf2a0aa42be967f7c93749c58a9f56dd
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39621778"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Java işlevleri Geliştirici Kılavuzu

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programlama modeli 

Azure işlevinizin giriş işleyen ve çıktıyı üretir bir durum bilgisi olmayan sınıf yöntemi olmalıdır. Örnek yöntemleri yazmak için izin verse de, işlevinizi sınıfının örneği alanlara bağlı olmamalı. Tüm işlev yöntemleri olmalıdır bir `public` erişim değiştiricisi.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamaları

Genellikle bir Azure işlevi, bir dış tetikleyiciyi nedeniyle çağrılır. Bu tetikleyici ve ilişkili girişleri işlemek ve bir veya daha fazla çıktı üretmek, işlevinizi gerekir.

Java ek açıklamalar dahil edilecek `azure-functions-java-core` giriş ve çıkışları için yöntemlerinizi bağlamak için paket. Aşağıdaki tabloda, desteklenen giriş tetikleyiciler ve çıkış bağlaması ek açıklamalar dahil edilmiştir:

Bağlama | Ek Açıklama
---|---
CosmosDB | Yok
HTTP | <ul><li>`HttpTrigger`</li><li>`HttpOutput`</li></ul>
Mobile Apps | Yok
Notification Hubs | Yok
Depolama Blobu | <ul><li>`BlobTrigger`</li><li>`BlobInput`</li><li>`BlobOutput`</li></ul>
Depolama Kuyruğu | <ul><li>`QueueTrigger`</li><li>`QueueOutput`</li></ul>
Depolama tablosu | <ul><li>`TableInput`</li><li>`TableOutput`</li></ul>
Zamanlayıcı | <ul><li>`TimerTrigger`</li></ul>
Twilio | Yok

Tetikleyici giriş ve çıkış olarak da tanımlanabilir [function.json](/azure/azure-functions/functions-reference#function-code) uygulamanız için.

> [!IMPORTANT] 
> Bir Azure depolama hesabında yapılandırmanız gerekir, [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure depolama Blob, kuyruk veya tablo Tetikleyiciler yerel olarak çalıştırın.

Örnek: ek açıklamalarını kullanma

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Yazılan ek açıklamalar olmadan aynı işlev:

```java
package com.example;

public class MyClass {
    public static String echo(String in) {
        return in;
    }
}
```

Buna karşılık gelen ile `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="data-types"></a>Veri Türleri

Yerel türler içeren giriş ve çıkış verileri için Java tüm veri türlerinde ücretsiz; Java türleri ve özel Azure türleri içinde tanımlanan özelleştirilmiş `azure-functions-java-core` paket. Azure işlevleri çalışma zamanı çalışır, kodunuz tarafından istenen türüne içine alınan girişi dönüştürür.

### <a name="strings"></a>Dizeler

İşlevi yöntemlere geçirilen değerler değerine dizeleri için karşılık gelen giriş parametre türü işlev türü ise `String`. 

### <a name="plain-old-java-objects-pojos"></a>Eski basit Java nesnelerini (Pojo'lar)

İşlev yöntemi giriş Java türü bekliyorsa JSON biçimli dizeler için Java türleri dönüştürme. Bu dönüştürme JSON girişler işlevlerinizi geçirmek ve kendi kodunuzda dönüştürme uygulamak zorunda kalmadan kodunuzdaki türleri Java ile çalışmanıza olanak sağlar.

Aynı işlevlere giriş gerekir olarak kullanılan POJO'ya türleri `public` bunlar kullanıldığı içinde işlevi yöntemler olarak erişim değiştiricisi. POJO'ya sınıfı alanlar bildirmeniz gerekmez `public`. Örneğin, bir JSON dizesi `{ "x": 3 }` aşağıdaki POJO'ya türe dönüştürülüp yapabilir:

```Java
public class MyData {
    private int x;
}
```

### <a name="binary-data"></a>İkili veriler

İkili veri olarak temsil edilir bir `byte[]` Azure işlevleri kodunuzda. İkili giriş veya çıkış ayarlayarak, işlevlerine bağlama `dataType` için function.json alanındaki `binary`:

```json
 {
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "blob",
      "name": "content",
      "direction": "in",
      "dataType": "binary",
      "path": "container/myfile.bin",
      "connection": "ExampleStorageAccount"
    },
  ]
}
```

Ardından işlev kodunuzu kullanın:

```java
// Class definition and imports are omitted here
public static String echoLength(byte[] content) {
}
```

Kullanım `OutputBinding<byte[]>` ikili çıkış bağlaması yapma türü.


## <a name="function-method-overloading"></a>İşlev aşırı yüklemesi yöntemi

İşlev yöntemleri aynı adı taşıyan ancak farklı türleri ile aşırı yüklemeye izin verilir. Örneğin, her ikisi de olabilir `String echo(String s)` ve `String echo(MyType s)` bir sınıf ve Azure işlevleri çalışma zamanı tarafından çağrılacak hangisinin inceleyin gerçek giriş türü karar (HTTP giriş, MIME türü için `text/plain` doğurur `String` sırada `application/json` temsil eden `MyType`).

## <a name="inputs"></a>Girişler

Giriş, Azure işlevleri'nde iki kategoriye ayrılmıştır: Tetikleyici girişi biridir ve diğer ek girişi. Farklı olsa `function.json`, kullanım, Java kodu aynıdır. Aşağıdaki kod parçacığını bir örnek olarak alalım:

```java
package com.example;

import com.microsoft.azure.serverless.functions.annotation.BindingName;
import java.util.Optional;

public class MyClass {
    public static String echo(Optional<String> in, @BindingName("item") MyObject obj) {
        return "Hello, " + in.orElse("Azure") + " and " + obj.getKey() + ".";
    }

    private static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

`@BindingName` Ek açıklaması kabul eden bir `String` bağlama/tetikleyici adını temsil eden özellik tanımlanmış `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "put" ],
      "route": "items/{id}"
    },
    {
      "type": "table",
      "name": "item",
      "direction": "in",
      "tableName": "items",
      "partitionKey": "Example",
      "rowKey": "{id}",
      "connection": "ExampleStorageAccount"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```

Bu işlev çağrıldığında, HTTP isteği yükü isteğe bağlı olarak geçirir, böylece `String` bağımsız değişkeni `in` ve Azure tablo depolama `MyObject` geçirilen bağımsız değişken türü `obj`. Kullanım `Optional<T>` null olabilen, işlevlere giriş işlemek için türü.

## <a name="outputs"></a>Çıkışlar

Çıkışlar hem dönüş değeri veya çıktı parametresi olarak ifade edilebilir. Yalnızca bir çıkış varsa, dönüş değeri kullanmak için önerilir. Çıktı parametreleri kullanmak zorunda birden çok çıkış için.

Dönüş değeri çıkış en basit biçimidir yalnızca herhangi bir tür değeri döndürmesi ve Azure işlevleri çalışma zamanı (örneğin, bir HTTP yanıtının) gerçek tür dön sıralamanız dener. İçinde `functions.json`, kullandığınız `$return` çıkış bağlaması adı olarak.

Birden çok çıktı değeri üretmek için kullanmak `OutputBinding<T>` içinde tanımlanan tür `azure-functions-java-core` paket. Bir HTTP yanıtı ve bir kuyruğuna bir ileti gönderme gerekiyorsa, benzer bir şey yazabilirsiniz:

```java
package com.example;

import com.microsoft.azure.serverless.functions.OutputBinding;
import com.microsoft.azure.serverless.functions.annotation.BindingName;

public class MyClass {
    public static String echo(String body, 
    @QueueOutput(queueName = "messages", connection = "AzureWebJobsStorage", name = "queue") OutputBinding<String> queue) {
        String result = "Hello, " + body + ".";
        queue.setValue(result);
        return result;
    }
}
```

Çıkış bağlaması içinde tanımlayan `function.json`:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.MyClass.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "queue",
      "name": "queue",
      "direction": "out",
      "queueName": "messages",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}
```
## <a name="specialized-types"></a>Özel türleri

Bazen bir işlev giriş ve çıkışları denetime ayrıntılı olması gerekir. Özelleştirilmiş türlerinde `azure-functions-java-core` paket isteği bilgiyi işleyebilir ve bir HTTP tetikleyici dönüş durumu uyarlamak için sağlanır:

| Özel tür      |       Hedef        | Tipik kullanım                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP tetikleyicisi     | Yöntemi, üst bilgiler veya sorgu Al |
| `HttpResponseMessage<T>` | HTTP çıkış bağlaması | 200 dışında dönüş durumu   |

> [!NOTE] 
> Ayrıca `@BindingName` HTTP üst bilgileri ve sorguları almak için ek açıklama. Örneğin, `@BindingName("name") String query` HTTP isteği üstbilgileri ve sorguları yinelenir ve bu değer yönteme geçirin. Örneğin, `query` olacaktır `"test"` istek URL'si ise `http://example.org/api/echo?name=test`.

### <a name="metadata"></a>Meta Veriler

Meta veri gelen HTTP üst bilgileri, HTTP sorgular gibi farklı kaynaklardan gelen ve [meta verileri tetikleme](/azure/azure-functions/functions-triggers-bindings#trigger-metadata-properties). Kullanım `@BindingName` ek açıklama değerini almak için meta veri adı ile birlikte.

Örneğin, `queryValue` aşağıdaki kod parçacığı olacak `"test"` istenen URL `http://{example.host}/api/metadata?name=test`.

```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.serverless.functions.annotation.*;

public class MyClass {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```

## <a name="functions-execution-context"></a>İşlev yürütme bağlamı

Azure işlevleri yürütme ortamı ile etkileşim `ExecutionContext` içinde tanımlanan nesne `azure-functions-java-core` paket. Kullanma `ExecutionContext` çağırma ve İşlevler çalışma zamanı bilgileri, kodunuzda kullanılacak nesne.

### <a name="logging"></a>Günlüğe kaydetme

İşlevler çalışma zamanı Günlükçü erişimi aracılığıyla `ExecutionContext` nesne. Bu Günlükçü için Azure İzleyici bağlıdır ve bayrağını uyarılar ve işlev yürütme sırasında karşılaşılan hataları sağlar.

Alınan istek gövdesi boş olduğunda, aşağıdaki kod örneği bir uyarı iletisi günlüğe kaydeder.

```java
import com.microsoft.azure.serverless.functions.annotation.HttpTrigger;
import com.microsoft.azure.serverless.functions.ExecutionContext;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="environment-variables"></a>Ortam değişkenleri

Genellikle, güvenlik nedenleriyle kaynak kodundan gizli bilgi ayıklamak için tercih edilir. Bu kod, kaynak kod deposu için diğer geliştiriciler için kimlik bilgilerini sağlamadan yanlışlıkla yayımlanmasını sağlar. Bu, Azure işlevleri yerel olarak çalıştırılırken hem işlevlerinizi Azure'a dağıtırken ortam değişkenleri yalnızca kullanılarak gerçekleştirilebilir.

Kolayca Azure işlevleri çalıştırırken ortam değişkenlerini ayarlamak için yerel olarak, bu değişkenler local.settings.json dosyasına eklemek seçebilirsiniz. Bir işlev projenizin kök dizininde mevcut değilse oluşturun çekinmeyin. İşte dosyanın aşağıdaki gibi görünmelidir:

```xml
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "",
    "AzureWebJobsDashboard": ""
  }
}
```

Her anahtar / değer eşlemede `values` harita oluşturulacak kullanılabilir çalışma zamanında çağrılarak erişilebilen bir ortam değişkeni olarak `System.getenv("<keyname>")`, örneğin, `System.getenv("AzureWebJobsStorage")`. Ekleme ek anahtar / değer çiftlerini kabul edilir ve önerilen uygulama.

> [!NOTE]
> Bu yaklaşım alınmışsa olması emin local.settings.json ekleme olup olmadığını göz önünde bulundurmanız deponuzda dosya yoksayma, taahhüt olmaması.

Kodunuzu test etme, yerel olarak hem de Azure'da dağıtılan eşdeğer işlevlerinin artık bu ortam değişkenlerine bağlı olarak, kod ile aynı anahtarı / değer çiftlerini, işlev uygulaması ayarları Azure portalında oturum açabilir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Uzaktan hata ayıklama Java Azure işlevleri ile Visual Studio kodu](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
