---
title: "Azure’da Java ve Maven ile ilk işlevinizi oluşturma | Microsoft Docs"
description: "Java ve Maven ile basit bir HTTP tetiklemeli işlev oluşturup Azure’da yayımlayın."
services: functions
documentationcenter: na
author: rloutlaw
manager: justhe
keywords: "azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari"
ms.service: functions
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/07/2017
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 3762a6e267540ef79577c3bf94ce27b648bd3534
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2017
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Java ve Maven (Önizleme) ile ilk işlevinizi oluşturma

> [!NOTE] 
> Azure İşlevleri için Java şu anda önizleme aşamasındadır.

Bu hızlı başlangıç, Maven ile [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) işlev projesi oluşturma, bunu yerel olarak test etme ve Azure İşlevleri’ne dağıtma konusunda rehberlik sağlar. Hızlı başlangıcı bitirdiğinizde, Azure’da çalışan HTTP tetiklemeli bir işlev uygulamanız olur.

![cURL ile komut satırından bir Hello World işlevine erişme](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar
Java ile işlev uygulamaları geliştirebilmeniz için şunlar yüklü olmalıdır:

-  [.NET Core](https://www.microsoft.com/net/core), en son sürüm.
-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Node.js](https://nodejs.org/download/), sürüm 8.6 veya üzeri.

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools’u Yükleme

[Azure Functions Core Tools 2.0](https://www.npmjs.com/package/azure-functions-core-tools), Azure İşlevleri yazmak, çalıştırmak ve bunların hatalarını ayıklamak için yerel bir geliştirme ortamı sağlar. Araçları [Node.js](https://nodejs.org/)’ye dahil edilen [npm](https://www.npmjs.com/) ile yükleyin.

```
npm install -g azure-functions-core-tools@core
```

> [!NOTE]
> Azure Functions Core Tools 2.0 sürümünü yükleme sorunu yaşıyorsanız bkz. [Sürüm 2.x çalışma zamanı](/azure/azure-functions/functions-run-local#version-2x-runtime).

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

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Projede oluşturulan bu kod, isteğin gövdesini yankılayan [HTTP tetiklemeli](/azure/azure-functions/functions-bindings-http-webhook) basit bir işlevdir:

```java
public class Function {
    @FunctionName("hello")
    public String hello(@HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req,
                        ExecutionContext context) {
        return String.format("Hello, %s!", req);
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

İşlev çalışırken bu çıktıyı görürsünüz:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Komut satırından yeni bir terminalde curl kullanarak işlevi tetikleyin:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

İşlev kodunu durdurmak için terminalde `Ctrl-C` komutunu kullanın.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) ve sonra `azure-functions:deploy` Maven hedefini kullanarak kodunuzu yeni bir İşlev uygulamasına dağıtın.

```
az login
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

Azure’da çalışan işlev uygulamasını curl kullanarak test edin:

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP tetikleyicisine sahip bir Java işlev uygulaması oluşturdunuz ve Azure İşlevleri’ne dağıttınız.

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.
- Visual Studio Code ile işlevlerde yerel olarak hata ayıklayın. [Java uzantı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) yüklü ve İşlevler projeniz Visual Studio Code’da açık haldeyken, 5005 bağlantı noktasına [hata ayıklayıcıyı bağlayın](https://code.visualstudio.com/Docs/editor/debugging#_launch-configurations). Sonra düzenleyicide bir kesme noktası ayarlayın ve işleviniz yerel olarak çalışırken tetikleyin: ![Visual Studio Code’da işlevlerin hatalarını ayıklama](media/functions-create-java-maven/vscode-debug.png)



