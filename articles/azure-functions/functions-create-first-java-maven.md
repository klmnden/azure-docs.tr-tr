---
title: Azure’da Java ve Maven ile ilk işlevinizi oluşturma | Microsoft Docs
description: Java ve Maven ile basit bir HTTP tetiklemeli işlev oluşturup Azure’da yayımlayın.
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: java
ms.topic: quickstart
ms.date: 08/10/2018
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: d25fbfc058337c7a96414cf41f321e039ebc2258
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61341461"
---
# <a name="create-your-first-function-with-java-and-maven"></a>Java ve Maven ile ilk işlevinizi oluşturma

Bu makalede, derleme ve Azure işlevleri için Java işlevi yayımlamak için Maven komut satırı aracını kullanarak aracılığıyla size yol gösterir. İşiniz bittiğinde, işlev kodunuzun çalıştığı [tüketim planı](functions-scale.md#consumption-plan) azure'da ve bir HTTP isteği kullanılarak tetiklenebilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Java kullanarak işlevleri geliştirmek için aşağıdakilerin yüklü olması gerekir:

- [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
- [Azure CLI](https://docs.microsoft.com/cli/azure)
- [Azure işlevleri temel araçları](functions-run-local.md#v2) (gerektirir **.NET Core 2.x SDK**)

> [!IMPORTANT]
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="generate-a-new-functions-project"></a>Yeni İşlevler projesi oluşturma

İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın.

### <a name="linuxmacos"></a>Linux/macOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

> [!NOTE]
> Komutu çalıştırmadaki sorunları yaşıyorsanız, ne göz atın `maven-archetype-plugin` sürümü kullanılır. Boş bir dizin yok ile'komutu çalıştırdığınızdan `.pom` , onu uygulanmaya çalışılıyor, eski sürümden bir eklentiyi `~/.m2/repository/org/apache/maven/plugins/maven-archetype-plugin` Maven eski bir sürümden yükselttiyseniz. Bu durumda, silmeyi deneyin `maven-archetype-plugin` dizin ve komutu yeniden çalıştırılıyor.

### <a name="windows"></a>Windows

```powershell
mvn archetype:generate `
    "-DarchetypeGroupId=com.microsoft.azure" `
    "-DarchetypeArtifactId=azure-functions-archetype"
```

```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven, proje oluşturma işleminin tamamlanması için gereken değerleri ister. _groupId_, _artifactId_ ve _version_ değerleri için [Maven adlandırma kuralları](https://maven.apache.org/guides/mini/guide-naming-conventions.html) başvurusuna bakın. _appName_ değerinin Azure genelinde benzersiz olması gerektiğinden, Maven bir varsayılan olarak daha önce girilen _artifactId_’yi temel alarak bir uygulama adı oluşturur. _packageName_ değeri, oluşturulan işlev kodu için Java paketini belirler.

Aşağıdaki `com.fabrikam.functions` ve `fabrikam-functions` tanımlayıcıları, örnek olarak ve bu hızlı başlangıcın ilerleyen kısımlarındaki adımların okunmasını kolaylaştırmak için kullanılır. Bu adımda Maven’e kendi değerlerinizi sağlamanız önerilir.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven, şu örnekte _artifactId_ adlı yeni bir klasörde proje dosyalarını oluşturur: `fabrikam-functions`. Projedeki çalıştırılmaya hazır olarak oluşturulan bu kod, isteğin gövdesini yankılayan [HTTP tetiklemeli](/azure/azure-functions/functions-bindings-http-webhook) basit bir işlevdir:

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage<String> hello(
            @HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponse(400, "Please pass a name on the query string or in the request body");
        } else {
            return request.createResponse(200, "Hello, " + name);
        }
    }
}

```

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Yeni oluşturulan proje klasörünün dizinine geçerek Maven ile işlevi derleyin ve çalıştırın:

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Java 9 ile `javax.xml.bind.JAXBException` özel durumunu yaşıyorsanız, [GitHub](https://github.com/jOOQ/jOOQ/issues/6477)’daki geçici çözüme bakın.

İşlev sisteminizde yerel olarak çalıştırılırken ve HTTP isteklerine yanıt vermeye hazır olduğunda şu çıktıyı görürsünüz:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Yeni bir terminal penceresinde curl kullanarak komut satırından işlevi tetikleyin:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

İşlev kodunu durdurmak için terminalde `Ctrl-C` komutunu kullanın.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

```azurecli
az login
```

`azure-functions:deploy` Maven hedefini kullanarak kodunuzu yeni bir İşlev uygulamasına dağıtın.

> [!NOTE]
> İşlev uygulamanızı dağıtmak için Visual Studio Code kullandığınızda unutmayın olmayan ücretsiz bir abonelik seçin veya bir hata alırsınız. IDE'nin sol tarafında aboneliğinizi izleyebilirsiniz.

```
mvn azure-functions:deploy
```

Dağıtım tamamlandığında, Azure işlev uygulamanıza erişmek için kullanabileceğiniz URL’yi görürsünüz:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Azure’da çalışan işlev uygulamasını `cURL` kullanarak test edin. Önceki adımdan kendi işlev uygulamanız için dağıtılan URL ile eşleşmek üzere aşağıdaki örnekten URL’yi değiştirmeniz gerekir.

> [!NOTE]
> Ayarladığınızdan emin olun **erişim hakları** için `Anonymous`. Varsayılan düzeyini seçtiğinizde `Function`, sunmak için gerekli [işlev anahtarı](../azure-functions/functions-bindings-http-webhook.md#authorization-keys) işlevi uç noktanızı erişmek için istek.

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="make-changes-and-redeploy"></a>Değişiklik yapma ve yeniden dağıtma

İşlev uygulamanız tarafından döndürülen metinde değişiklikler yapmak için oluşturulan projedeki `src/main.../Function.java` kaynak dosyasını düzenleyin. Bu satırı değiştirin:

```java
return request.createResponse(200, "Hello, " + name);
```

Şu şekilde:

```java
return request.createResponse(200, "Hi, " + name);
```

Önceki gibi terminalden `azure-functions:deploy` komutunu çalıştırarak değişiklikleri kaydedin ve yeniden dağıtın. İşlev uygulaması güncelleştirilir ve bu isteğin:

```bash
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

Çıkışı güncelleştirilir:

```Output
Hi, AzureFunctionsTest
```

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP tetikleyicisine sahip bir Java işlev uygulaması oluşturdunuz ve Azure İşlevleri’ne dağıttınız.

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.
- [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md) ve [Eclipse](functions-create-maven-eclipse.md) ile yerel olarak işlev yazın ve işlevlerde hata ayıklayın. 
- Visual Studio Code ile Azure’da dağıtılan işlevlerde hata ayıklayın. Yönergeler için [sunucusuz Java uygulamaları](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud) belgelerine bakın.
