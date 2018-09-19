---
title: Azure işlevleri için Java Geliştirici Başvurusu | Microsoft Docs
description: Java işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: routlaw
ms.openlocfilehash: f6c5eb4a3ace1fcca1bbbef321371d55a0ce8da9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46123496"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Java işlevleri Geliştirici Kılavuzu

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

## <a name="programming-model"></a>Programlama modeli 

Azure işlevinizin giriş işleyen ve çıktıyı üretir bir durum bilgisi olmayan sınıf yöntemi olmalıdır. Örnek yöntemler yazabilirsiniz olsa da, işlevinizi sınıfının örneği alanlara bağlı olmamalı. Tüm işlev yöntemleri olmalıdır bir `public` erişim değiştiricisi.

## <a name="folder-structure"></a>klasör yapısı

Bir Java projesi klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

İşlev uygulamasını yapılandırmak için kullanılan bir paylaşılan [host.json] (işlevler-host-json.md) dosyası yoktur. Her işlev, kendi kod dosyası (.java) ve bağlama yapılandırma dosyası (function.json) vardır.

Bir projede birden fazla işlev koyabilirsiniz. Ayrı jar dosyaları dışındaki işlevlerinizi eklemekten kaçının. Hedef dizinde FunctionApp ne işlev uygulamanızı azure'da dağıtılan ' dir.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamaları

 Azure işlevleri, bir HTTP isteği, bir zamanlayıcı ya da veri güncelleştirme gibi bir tetikleyici tarafından çağrılır. Bu tetikleyici ve bir veya birden çok çıktı üretmek için başka bir giriş işlevinizi gerekmez.

İçindeki Java ek açıklamalarını kullanma [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) giriş ve çıkışları için yöntemlerinizi bağlamak için paket. Ek açıklamalar kullanarak örnek kodu kullanılabilir [Java başvuru belgeleri](/java/api/com.microsoft.azure.functions.annotation) her eklenti için ve biri gibi Azure işlevleri bağlama başvuru belgelerinde [HTTP Tetikleyicileri](/azure/azure-functions/functions-bindings-http-webhook).

Tetikleyici giriş ve çıkış olarak da tanımlanabilir [function.json](/azure/azure-functions/functions-reference#function-code) yerine işlevinizin ek açıklamalar aracılığıyla. Kullanarak `function.json` yerine bu şekilde ek açıklamalarda önerilmez.

> [!IMPORTANT] 
> Bir Azure depolama hesabında yapılandırmanız gerekir, [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure depolama Blob, kuyruk veya tablo Tetikleyiciler yerel olarak çalıştırın.

Örnek: ek açıklamalarını kullanma

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
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

## <a name="third-party-libraries"></a>Üçüncü taraf kitaplıklar 

Azure işlevleri, üçüncü taraf kitaplıkların kullanımını destekler. Varsayılan olarak, tüm bağımlılıkları belirtilen projenizde `pom.xml` dosya otomatik olarak toplanmış sırasında `mvn package` hedefi. Bağımlılık olarak belirtilmemiş kitaplıkları `pom.xml` dosya, yerleştirebilirsiniz bir `lib` işlevin kök dizininde dizin. Bağımlılıkları yerleştirildiğinde `lib` dizin sistemi sınıf yükleyicisi zamanında eklenecektir.

## <a name="data-type-support"></a>Veri türü desteği

Yerel türler içeren giriş ve çıkış verileri için herhangi bir veri türü Java'da kullanabilirsiniz; Java türleri ve özel Azure türleri içinde tanımlanan özelleştirilmiş `azure-functions-java-library` paket. Azure işlevleri çalışma zamanı çalışır, kodunuz tarafından istenen türüne içine alınan girişi dönüştürür.

### <a name="strings"></a>Dizeler

İşlevi yöntemlere geçirilen değerler değerine dizeleri için karşılık gelen giriş parametre türü işlev türü ise `String`. 

### <a name="plain-old-java-objects-pojos"></a>Eski basit Java nesnelerini (Pojo'lar)

İşlev giriş imzası Java türü bekliyorsa JSON biçimli dizeler için Java türleri dönüştürme. Bu dönüştürme, JSON'da geçirin ve Java türleriyle çalışmak sağlar.

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

Boş giriş değerleri olabilir `null` işlevleri bağımsız değişkeniniz ancak boş değerler kullanmaktır olası ile uğraşmak için önerilen bir yöntem olarak `Optional<T>`.


## <a name="function-method-overloading"></a>İşlev aşırı yüklemesi yöntemi

İşlev yöntemleri aynı adı taşıyan ancak farklı türleri ile aşırı yüklemeye izin verilir. Örneğin, her ikisi de olabilir `String echo(String s)` ve `String echo(MyType s)` sınıfında. Azure işlevleri çağırmak için hangi yöntemi giriş türüne göre karar (HTTP giriş, MIME türü için `text/plain` doğurur `String` sırada `application/json` temsil `MyType`).

## <a name="inputs"></a>Girişler

Giriş, Azure işlevleri'nde iki kategoriye ayrılmıştır: Tetikleyici girişi biridir ve diğer ek girişi. Farklı olsa `function.json`, kullanım, Java kodu aynıdır. Aşağıdaki kod parçacığını bir örnek olarak alalım:

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String in,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") MyObject obj
    ) {
        return "Hello, " + in + " and " + obj.getKey() + ".";
    }

    public static class MyObject {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
}
```

Bu işlev tetiklendiğinde, HTTP isteği tarafından işlev geçirilir `String in`. Bir giriş getirelim olarak yapılan ve yönlendirme URL'si kimliği temel Azure tablo depolama alanından alınan `obj` işlev gövdesindeki.

## <a name="outputs"></a>Çıkışlar

Çıkışlar hem dönüş değeri veya çıktı parametresi olarak ifade edilebilir. Yalnızca bir çıkış varsa, dönüş değeri kullanmak için önerilir. Çıktı parametreleri kullanmak zorunda birden çok çıkış için.

Dönüş değeri çıkış en basit biçimidir yalnızca herhangi bir tür değeri döndürmesi ve Azure işlevleri çalışma zamanı (örneğin, bir HTTP yanıtının) gerçek tür dön sıralamanız dener.  Çıktı ek açıklamaları (ek açıklama adı özniteliğinin sahip $return olmasını) işlevi yöntemi uygulayabilirsiniz dönüş değeri çıktısını tanımlamak için.

Birden çok çıktı değeri üretmek için kullanmak `OutputBinding<T>` içinde tanımlanan tür `azure-functions-java-library` paket. Bir HTTP yanıtı ve bir kuyruğuna bir ileti gönderme gerekiyorsa, benzer bir şey yazabilirsiniz:

Örneğin, bir blob içeriği işlevi kopyalama aşağıdaki kodun da tanımlanabilir. `@StorageAccount` ek açıklama buraya önlemek için her ikisi de bağlantı özelliği çoğaltma için kullanılan `@BlobTrigger` ve `@BlobOutput`.

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class MyClass {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Kullanma `OutputBinding<byte[]`> çıkış (parametre); değeri bir ikili yapmak için dönüş değerleri için kullanmanız yeterlidir `byte[]`.

## <a name="specialized-types"></a>Özel türleri

Bazen bir işlev giriş ve çıkışları denetime ayrıntılı olması gerekir. Özelleştirilmiş türlerinde `azure-functions-java-core` paket isteği bilgiyi işleyebilir ve bir HTTP tetikleyicisi dönüş durumu uyarlamak için sağlanır:

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
import com.microsoft.azure.functions.annotation.*;


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

## <a name="execution-context"></a>Yürütme bağlamı

Azure işlevleri yürütme ortamı etkileşim `ExecutionContext` içinde tanımlanan nesne `azure-functions-java-library` paket. Kullanma `ExecutionContext` çağırma ve İşlevler çalışma zamanı bilgileri, kodunuzda kullanılacak nesne.

### <a name="custom-logging"></a>Özel günlük

İşlevler çalışma zamanı Günlükçü erişimi aracılığıyla `ExecutionContext` nesne. Bu Günlükçü için Azure İzleyici bağlıdır ve bayrağını uyarılar ve işlev yürütme sırasında karşılaşılan hataları sağlar.

Alınan istek gövdesi boş olduğunda, aşağıdaki kod örneği bir uyarı iletisi günlüğe kaydeder.

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Günlükleri görüntüleme ve izleme

Diğer uygulama günlüğü yanı sıra stream Java standart çıkış ve hata günlüğü için Azure CLI'yı kullanabilirsiniz. İlk olarak, Azure CLI kullanarak uygulama günlükleri yazmak için işlev uygulamanızı yapılandırın:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Azure CLI kullanarak işlev uygulamanız için günlük çıktısı akış, yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin için:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
[Az webapp log tail](/cli/azure/webapp/log) komutunu kullanarak çıkışının filtreleme seçeneklerini içeren `--provider` seçeneği. 

Günlük dosyaları, Azure CLI kullanarak tek bir ZIP dosyası olarak karşıdan yüklemek için yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Bu komutu çalıştırmadan önce Azure Portal veya Azure CLI günlüğü dosya sistemi etkinleştirmiş olmanız gerekir.

## <a name="environment-variables"></a>Ortam değişkenleri

Anahtarları veya güvenlik nedenleriyle, kaynak kodunun dışında belirteçleri gibi gizli bilgiler tutun. Anahtarları ve belirteçleri ortam değişkenlerinden okuyarak işlev kodunuzu kullanın.

Azure işlevleri çalıştırırken ortam değişkenlerini ayarlamak için yerel olarak, bu değişkenler local.settings.json dosyasına eklemek seçebilirsiniz. Bir işlev projenizin kök dizininde mevcut değilse, bir tane oluşturabilirsiniz. İşte dosyanın aşağıdaki gibi görünmelidir:

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
> Bu yaklaşım alınmışsa olması emin local.settings.json eklemek için deponuzu dosyasına yoksayma, taahhüt olmaması.

Kodunuzu test etme, yerel olarak hem de Azure'da dağıtılan eşdeğer işlevlerinin artık bu ortam değişkenlerine bağlı olarak, kod ile aynı anahtarı / değer çiftlerini, işlev uygulaması ayarları Azure portalında oturum açabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevi Java geliştirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
- Yerel geliştirme ve hata ayıklama ile [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [Intellij](functions-create-maven-intellij.md), ve [Eclipse](functions-create-maven-eclipse.md). 
* [Uzaktan hata ayıklama Java Azure işlevleri ile Visual Studio kodu](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Azure işlevleri için maven plugin](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) -kolaylaştırın ile işlevi oluşturma `azure-functions:add` hedef ve hazırlamak için bir hazırlama dizin [ZIP dosyası dağıtım](deployment-zip-push.md).
