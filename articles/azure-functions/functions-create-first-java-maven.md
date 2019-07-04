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
ms.openlocfilehash: 66a7caddc499d32a4d836dcb60bc940c1ebc8a9e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67444566"
---
# <a name="create-your-first-function-with-java-and-maven"></a>Java ve Maven ile ilk işlevinizi oluşturma

Bu makalede, derleme ve Azure işlevleri için Java işlevi yayımlamak için Maven komut satırı aracını kullanarak aracılığıyla size yol gösterir. İşiniz bittiğinde, işlev kodunuzun çalıştığı [tüketim planı](functions-scale.md#consumption-plan) azure'da ve bir HTTP isteği kullanılarak tetiklenebilir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Java kullanarak işlevleri geliştirmek için aşağıdakilerin yüklü olması gerekir:

- [Java Developer Kit](https://aka.ms/azure-jdks), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri
- [Azure CLI](https://docs.microsoft.com/cli/azure)
- [Azure işlevleri temel araçları](./functions-run-local.md#v2) 2.6.666 sürümü veya üzeri

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
    "-DarchetypeGroupId=com.microsoft.azure" ^
    "-DarchetypeArtifactId=azure-functions-archetype"
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

Maven, şu örnekte _artifactId_ adlı yeni bir klasörde proje dosyalarını oluşturur: `fabrikam-functions`. Projede oluşturulan kodu çalıştırmak için hazır bir [HTTP ile tetiklenen](/azure/azure-functions/functions-bindings-http-webhook) isteğin gövdesini yankılayan işlevi:

```java
public class Function {
    /**
     * This function listens at endpoint "/api/hello". Two ways to invoke it using "curl" command in bash:
     * 1. curl -d "HTTP Body" {your host}/api/hello
     * 2. curl {your host}/api/hello?name=HTTP%20Query
     */
    @FunctionName("hello")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = { HttpMethod.GET, HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
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

## <a name="enable-extension-bundles"></a>Uzantı paketleri etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

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
curl -w "\n" http://localhost:7071/api/hello -d LocalFunction
```

```Output
Hello LocalFunction!
```

İşlev kodunu durdurmak için terminalde `Ctrl-C` komutunu kullanın.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. [Azure CLI ile oturum](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) devam etmeden önce.

```azurecli
az login
```

`azure-functions:deploy` Maven hedefini kullanarak kodunuzu yeni bir İşlev uygulamasına dağıtın. Bunu gerçekleştiren bir [dağıtma Zip paketini çalıştırın](functions-deployment-technologies.md#zip-deploy) modu etkin.

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
curl -w "\n" https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
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

Değişiklikleri kaydedin. Temiz çalışma mvn paket ve çalıştırarak yeniden dağıtma `azure-functions:deploy` terminalden önceki gibi. İşlev uygulaması güncelleştirilir ve bu isteğin:

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
