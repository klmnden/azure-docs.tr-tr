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
ms.openlocfilehash: 16d6dd6a589256ad98a37465e64e847778d0cc7e
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092602"
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Java ve Maven (Önizleme) ile ilk işlevinizi oluşturma

[!INCLUDE [functions-java-preview-note](../../includes/functions-java-preview-note.md)]

Bu hızlı başlangıç, Maven ile [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev projesi oluşturma, bunu yerel olarak test etme ve Azure İşlevleri’ne dağıtma konusunda rehberlik sağlar. Hızlı başlangıcı bitirdiğinizde, Azure’da çalışan HTTP tetiklemeli bir işlev uygulamanız olur.

![cURL ile komut satırından bir Hello World işlevine erişme](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar
Java ile işlev uygulamaları geliştirebilmeniz için şunlar yüklü olmalıdır:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

Azure İşlevleri Temel Araçları, terminalden veya komut isteminden Azure İşlevleri yazmak, çalıştırmak ve bunların hatalarını ayıklamak için yerel bir geliştirme ortamı sağlar. 

Devam etmeden önce yerel bilgisayarınıza [Temel Araçların 2. sürümünü](functions-run-local.md#v2) yükleyin.

## <a name="generate-a-new-functions-project"></a>Yeni İşlevler projesi oluşturma

İşlevler projesini bir [Maven arketipinden](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html) oluşturmak için boş bir klasörde aşağıdaki komutu çalıştırın.

### <a name="linuxmacos"></a>Linux/MacOS

```bash
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

### <a name="windows-cmd"></a>Windows (CMD)
```cmd
mvn archetype:generate ^
    -DarchetypeGroupId=com.microsoft.azure ^
    -DarchetypeArtifactId=azure-functions-archetype
```

Maven, proje oluşturma işleminin tamamlanması için gereken değerleri ister. _groupId_, _artifactId_ ve _version_ değerleri için [Maven adlandırma kuralları](https://maven.apache.org/guides/mini/guide-naming-conventions.html) başvurusuna bakın. _appName_ değerinin Azure genelinde benzersiz olması gerektiğinden, Maven bir varsayılan olarak daha önce girilen _artifactId_’yi temel alarak bir uygulama adı oluşturur. _packageName_ değeri, oluşturulan işlev kodu için Java paketini belirler.

`appRegion` değeri dağıtılan İşlev uygulamasının çalıştırılacağı [Azure bölgesini](https://azure.microsoft.com/global-infrastructure/regions/) belirtir. Bölge adı değerlerinin listesini Azure CLI'den `az account list-locations` komutuyla alabilirsiniz. `resourceGroup` değeri, işlev uygulamasının oluşturulacağı Azure kaynak grubunu belirtir.

Aşağıdaki `com.fabrikam.functions` ve `fabrikam-functions` tanımlayıcıları, örnek olarak ve bu hızlı başlangıcın ilerleyen kısımlarındaki adımların okunmasını kolaylaştırmak için kullanılır. Bu adımda Maven’e kendi değerlerinizi sağlamanız önerilir.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Define value for property 'appRegion' westus : 
Define value for property 'resourceGroup' java-functions-group: 
Confirm properties configuration: Y
```

Maven, şu örnekte _artifactId_ adlı yeni bir klasörde proje dosyalarını oluşturur: `fabrikam-functions`. Projedeki çalıştırılmaya hazır olarak oluşturulan bu kod, isteğin gövdesinin "Hello," dizesinin sonrasını yankılayan [HTTP tetiklemeli](/azure/azure-functions/functions-bindings-http-webhook) basit bir işlevdir.

```java
public class Function {
    @FunctionName("HttpTrigger-Java")
    public HttpResponseMessage HttpTriggerJava(
    @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");

        // Parse query parameter
        String query = request.getQueryParameters().get("name");
        String name = request.getBody().orElse(query);

        if (name == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
        } else {
            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        }
    }
}
```

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Yeni oluşturulan proje klasörünün dizinine geçerek Maven ile işlevi derleyin ve çalıştırın:

```
cd fabrikam-functions
mvn clean package 
mvn azure-functions:run
```

> [!NOTE]
> Java 9 ile `javax.xml.bind.JAXBException` özel durumunu yaşıyorsanız, [GitHub](https://github.com/jOOQ/jOOQ/issues/6477)’daki geçici çözüme bakın.

İşlev sisteminizde yerel olarak çalıştırılırken ve HTTP isteklerine yanıt vermeye hazır olduğunda şu çıktıyı görürsünüz:

```Output
Listening on http://0.0.0.0:7071/
Hit CTRL-C to exit...

Http Functions:

        HttpTrigger-Java: http://localhost:7071/api/HttpTrigger-Java
```

Yeni bir terminal penceresinde curl kullanarak komut satırından işlevi tetikleyin:

```
curl -w '\n' -d LocalFunctionTest http://localhost:7071/api/HttpTrigger-Java
```

```Output
Hello, LocalFunctionTest
```

İşlev kodunu durdurmak için terminalde `Ctrl-C` komutunu kullanın.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. Devam etmeden önce [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

```azurecli
az login
```

`azure-functions:deploy` Maven hedefini kullanarak kodunuzu yeni bir İşlev uygulamasına dağıtın.

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

```
curl -w '\n' -d AzureFunctionsTest https://fabrikam-functions-20170920120101928.azurewebsites.net/api/HttpTrigger-Java
```

```Output
Hello, AzureFunctionsTest
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
