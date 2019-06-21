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
ms.openlocfilehash: acd873cd19cafb785f968fd3d8671640bcfafed8
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67163713"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Java işlevleri Geliştirici Kılavuzu

Azure işlevleri çalışma zamanı destekler [Java SE 8 LTS (zulu8.31.0.2 jre8.0.181 win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/). Bu kılavuzda, Azure işlevleri ile Java yazma ayrıntılı olarak incelenmektedir hakkında bilgi içerir.

Bir Java işlev, bir `public` yöntemi, ek açıklama ile donatılmış `@FunctionName`. Bu yöntem, bir Java işlev girişini tanımlar ve belirli bir paket içinde benzersiz olmalıdır. 

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). Ayrıca kullanarak ilk işlevinizi oluşturmak için işlevleri hızlı tamamlanır [Visual Studio Code](functions-create-first-function-vs-code.md) veya [Maven](functions-create-first-java-maven.md).

## <a name="programming-model"></a>Programlama modeli 

Kavramlarını [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) Azure işlevleri için zorunludur. Tetikleyiciler, kodunuzu yürütmeye başlayın. Bağlamaları, verileri aktarmak ve özel veri erişim kodu yazmak zorunda kalmadan bir işlevden dönüş verileri için bir yol sağlar.

## <a name="folder-structure"></a>klasör yapısı

Azure işlevleri Java projesi klasör yapısı şu şekildedir:

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

Kullanabileceğiniz bir paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için bir dosya. Her işlev, kendi kod dosyası (.java) ve bağlama yapılandırma dosyası (function.json) vardır.

Bir projede birden fazla işlev koyabilirsiniz. Ayrı jar dosyaları dışındaki işlevlerinizi eklemekten kaçının. `FunctionApp` Hedef ne işlev uygulamanızı azure'da dağıtılan dizinidir.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamaları

 İşlevleri HTTP isteği, bir zamanlayıcı ya da veri güncelleştirme gibi bir tetikleyici tarafından çağrılır. Bu tetikleyici ve bir veya birden çok çıktı üretmek için tüm diğer girişler, işlevinizi gerekmez.

İçindeki Java ek açıklamalarını kullanma [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) giriş ve çıkışları için yöntemlerinizi bağlamak için paket. Daha fazla bilgi için [Java başvuru belgeleri](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> Bir Azure depolama hesabında yapılandırmanız gerekir, [local.settings.json](/azure/azure-functions/functions-run-local#local-settings-file) Azure Blob Depolama, Azure kuyruk depolama veya Azure tablo depolama Tetikleyicileri yerel olarak çalıştırılacak.

Örnek:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

İşte oluşturulan karşılık gelen `function.json` tarafından [azure işlevleri maven plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin):

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
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

## <a name="jdk-runtime-availability-and-support"></a>JDK çalışma zamanı kullanılabilirliği ve Destek 

Java işlevi uygulamalarını yerel geliştirme için karşıdan yükleme ve kullanma [Azul Zulu Enterprise Azure](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) Java 8 JDK'den [Azul Systems](https://www.azul.com/downloads/azure-only/zulu/). Azure işlevleri, işlev uygulamalarınızı buluta dağıtırken Azul Java 8 JDK çalışma zamanı kullanır.

[Azure Destek](https://azure.microsoft.com/support/) JDK ve işlev ile ilgili sorunlar için uygulamalar, kullanılabilir bir [tam destek planı](https://azure.microsoft.com/support/plans/).

## <a name="customize-jvm"></a>JVM özelleştirme

İşlevleri, Java sanal makinesi (JVM) Java işlevlerinizi çalıştırmak için kullanılan özelleştirmenize olanak sağlar. [Aşağıdaki JVM seçenekleri](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) varsayılan olarak kullanılır:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Ek bağımsız değişkenler adlandırılmış ayarı bir uygulamada sağlayabilir `JAVA_OPTS`. İşlev uygulamanızı Azure portalında veya Azure CLI'yı Azure'a dağıtılmış uygulama ayarları ekleyebilirsiniz.

### <a name="azure-portal"></a>Azure portal

İçinde [Azure portalında](https://portal.azure.com), kullanın [uygulama ayarları sekmesinde](functions-how-to-use-azure-function-app-settings.md#settings) eklemek için `JAVA_OPTS` ayarı.

### <a name="azure-cli"></a>Azure CLI

Kullanabileceğiniz [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) ayarlamak için komutu `JAVA_OPTS`, aşağıdaki örnekte olduğu gibi:

```azurecli-interactive
az functionapp config appsettings set --name <APP_NAME> \
--resource-group <RESOURCE_GROUP> \
--settings "JAVA_OPTS=-Djava.awt.headless=true"
```
Bu örnekte gözetimsiz modunu etkinleştirir. Değiştirin `<APP_NAME>` işlev uygulamanızın adıyla ve `<RESOURCE_GROUP>` kaynak grubu ile.

> [!WARNING]  
> İçinde [tüketim planı](functions-scale.md#consumption-plan), eklemelisiniz `WEBSITE_USE_PLACEHOLDER` değeriyle ayarlama `0`.  
Bu ayar, Java işlevleri için soğuk başlangıç zamanlarını artırın.

## <a name="third-party-libraries"></a>Üçüncü taraf kitaplıklar 

Azure işlevleri, üçüncü taraf kitaplıkların kullanımını destekler. Varsayılan olarak, tüm bağımlılıkları belirtilen projenizde `pom.xml` dosya otomatik olarak sırasında paketlenmiş [ `mvn package` ](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) hedefi. Bağımlılık olarak belirtilmemiş kitaplıkları `pom.xml` dosya, yerleştirebilirsiniz bir `lib` işlevin kök dizininde dizin. Bağımlılıkları yerleştirildiğinde `lib` dizin sistemi sınıf yükleyicisi zamanında eklenir.

`com.microsoft.azure.functions:azure-functions-java-library` Bağımlılık sınıf üzerinde varsayılan olarak sağlanır ve dahil edilmesi gerekmez `lib` dizin. Ayrıca, [azure-işlevler-java-çalışan](https://github.com/Azure/azure-functions-java-worker) listelenen bağımlılıkları ekler [burada](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) için sınıf.

## <a name="data-type-support"></a>Veri türü desteği

Eski basit Java nesnelerini (Pojo'lar) kullanabilir, tanımlı türleri `azure-functions-java-library`, ya da giriş veya çıktı bağlaması için dize ve tamsayı bağlamak için gibi temel veri türleri.

### <a name="pojos"></a>Pojo'ları

POJO'ya, giriş verilerini dönüştürmek için [azure-işlevler-java-çalışan](https://github.com/Azure/azure-functions-java-worker) kullanan [gson](https://github.com/google/gson) kitaplığı. İşlevlere giriş olması gerektiği gibi kullanılan POJO'ya türleri `public`.

### <a name="binary-data"></a>İkili veriler

İkili giriş veya çıkış için bağlama `byte[]`, ayarlayarak `dataType` için function.json alanındaki `binary`:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Null değerler beklediğiniz kullanırsanız `Optional<T>`.

## <a name="bindings"></a>Bağlamalar

Giriş ve çıkış bağlamaları, kod içindeki verilere bağlanmak için bildirim temelli bir yöntemini sağlar. Bir işlev birden fazla giriş ve çıkış bağlamaları kullanabilirsiniz.

### <a name="input-binding-example"></a>Örnek Giriş bağlama

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Bir HTTP isteği ile bu işlevi çağırın. 
- HTTP isteği yükü olarak geçirilen bir `String` bağımsız değişkeni `inputReq`.
- Bir giriş tablo Depolama'yı alınır ve olarak geçirilen `TestInputData` bağımsız değişkene `inputData`.

Bir batch girişlerinin almak, adlarınıza bağlayabileceğiniz `String[]`, `POJO[]`, `List<String>`, veya `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Bu işlev, yapılandırılmış olay hub'ında yeni veri olduğunda tetiklenen. Çünkü `cardinality` ayarlanır `MANY`, işlev, olay hub'ından toplu iletiler alır. `EventData` Olay hub'ı için dönüştürüldüğünü `TestEventData` işlevi yürütme.

### <a name="output-binding-example"></a>Çıkış bağlaması örneği

Dönüş değerini kullanarak bir çıkış bağlaması bağlayabilirsiniz `$return`. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Dönüş değeri, birden çok çıkış bağlamaları varsa, bunlardan yalnızca biri için kullanın.

Birden çok çıkış değerleri göndermek için `OutputBinding<T>` tanımlanan `azure-functions-java-library` paket. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Bir HTTP isteği üzerinde bu işlevi çağırın. Birden çok değer kuyruk depolamaya yazar.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage ve HttpResponseMessage

 Bu tanımlı `azure-functions-java-library`. Bunlar HttpTrigger işlevleri ile çalışmaya yardımcı türleridir.

| Özel tür      |       Hedef        | Tipik kullanım                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP Tetikleyicisi     | Yöntemi, üst bilgiler veya sorgu alır |
| `HttpResponseMessage` | HTTP çıkış bağlaması | Durum 200 dışında döndürür   |

## <a name="metadata"></a>Meta Veriler

Birkaç tetikleyicilere göndermek [meta verileri tetikleme](/azure/azure-functions/functions-triggers-bindings) giriş verileriyle birlikte. Ek açıklama kullanabileceğiniz `@BindingName` meta verileri tetiklemek için bağlamak için.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
Önceki örnekte `queryValue` sorgu dizesi parametresine bağlı `name` http istek URL'sindeki `http://{example.host}/api/metadata?name=test`. Nasıl bağlanacağını gösteren başka bir örnek `Id` kuyruğu tetikleyici meta veriler.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Meta veri özelliğini eşleştirmek Ek açıklamada verilen adı gerekiyor.

## <a name="execution-context"></a>Yürütme bağlamı

`ExecutionContext`, tanımlanan `azure-functions-java-library`, İşlevler çalışma zamanı ile iletişim kurmak için yardımcı yöntemler içerir.

### <a name="logger"></a>Günlükçü

Kullanım `getLogger`içinde tanımlanmış `ExecutionContext`, işlev kodunu günlüklerini yazma izni.

Örnek:

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

Akış Java stdout ve stderr günlük yanı sıra, diğer uygulama günlükleri için Azure CLI'yı kullanabilirsiniz. 

Azure CLI kullanarak uygulama günlükleri yazmak için işlev uygulamanızın yapılandırma aşağıda verilmiştir:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

İşlev uygulamanız için günlük çıktısı, Azure CLI kullanarak akış, yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin için:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
[Az webapp log tail](/cli/azure/webapp/log) komutunu kullanarak çıkış filtreleme seçeneklerini içeren `--provider` seçeneği. 

Günlük dosyaları, Azure CLI kullanarak tek bir ZIP dosyası olarak karşıdan yüklemek için yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Bu komutu çalıştırmadan önce Azure portalında veya Azure CLI günlüğü dosya sistemi etkinleştirmiş olmanız gerekir.

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, [uygulama ayarları](https://docs.microsoft.com/azure/azure-functions/functions-app-settings), gibi hizmet bağlantısı dizeleri sunulur ortam değişkenleri olarak yürütme sırasında. Bu ayarları kullanarak erişebileceğiniz `System.getenv("AzureWebJobsStorage")`.

Örneğin, ekleyebileceğiniz [AppSetting](https://docs.microsoft.com/azure/azure-functions/functions-how-to-use-azure-function-app-settings), adıyla `testAppSetting` ve değer `testAppSettingValue`:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("testAppSetting "+ System.getenv("testAppSettingValue"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri Java geliştirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* Yerel geliştirme ve hata ayıklama ile [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [Intellij](functions-create-maven-intellij.md), ve [Eclipse](functions-create-maven-eclipse.md)
* [Uzaktan hata ayıklama Java Azure işlevleri ile Visual Studio kodu](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Azure işlevleri için maven eklentisi](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* İşlev oluşturma aracılığıyla kolaylaştırın `azure-functions:add` hedef ve hazırlamak için bir hazırlama dizin [ZIP dosyası dağıtım](deployment-zip-push.md).
