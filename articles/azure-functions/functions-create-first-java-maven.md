---
title: "Azure Java ve Maven içinde ilk işlevinizi oluşturma | Microsoft Docs"
description: "Oluşturma ve basit bir HTTP tetiklenen işlevi Java ve Maven ile azure'a yayımlama."
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
ms.date: 10/03/2017
ms.author: routlaw, glenga
ms.custom: mvc, devcenter
ms.openlocfilehash: 50fd59c288312c7aa5ffe6abf1318a5ec2f406e6
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="create-your-first-function-with-java-and-maven-preview"></a>Java ve Maven (Önizleme) ilk işlevinizi oluşturma

Bu hızlı başlangıç kılavuzları oluşturmada size bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) yerel olarak test etme ve Azure işlevleri dağıtma Maven işlevi projeyle. İşiniz bittiğinde, Azure'da çalışan bir HTTP tetiklemeli işlevin uygulamanız.

 ![Hello World işlevi cURL ile komut satırından erişim](media/functions-create-java-maven/hello-azure.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar
Java ile işlevleri uygulama geliştirmek için aşağıdakilerin yüklü olması gerekir:

-  [.NET core](https://www.microsoft.com/net/core), en son sürümü.
-  [Java Geliştirme Seti](https://www.azul.com/downloads/zulu/), sürüm 1,8.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üstü.
-  [Node.js](https://nodejs.org/download/), 8,6 veya daha yüksek bir sürümü.

> [!IMPORTANT] 
> Bu hızlı başlangıç tamamlamak için JAVA_HOME ortam değişkeni JDK yükleme konumuna ayarlanması gerekir.

## <a name="install-the-azure-functions-core-tools"></a>Azure işlevleri çekirdek Araçları'nı yükleme

[Azure işlevleri çekirdek araçları 2.0](https://www.npmjs.com/package/azure-functions-core-tools) yazma, çalıştırma ve Azure işlevlerinde hata ayıklama için bir yerel geliştirme ortamı sağlar. Araçlarla Yükle [npm](https://www.npmjs.com/)ile birlikte sunulan [Node.js](https://nodejs.org/).

```
npm install -g azure-functions-core-tools@core
```

> [!NOTE]
> Azure işlevleri çekirdek Araçları sürüm 2.0 yükleme konusunda sorun yaşıyorsanız, bkz: [sürüm 2.x çalışma zamanı](/azure/azure-functions/functions-run-local#version-2x-runtime).

## <a name="generate-a-new-functions-project"></a>Yeni işlevleri projesi oluştur

Boş bir klasöre işlevleri projeden oluşturmak için aşağıdaki komutu çalıştırın bir [Maven archetype](https://maven.apache.org/guides/introduction/introduction-to-archetypes.html).

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

Maven projesi oluşturma tamamlamak için gereken değeri ister. İçin _GroupID_, _Artifactıd_, ve _sürüm_ değerler, bakın [Maven adlandırma kuralları](https://maven.apache.org/guides/mini/guide-naming-conventions.html) başvuru. _AppName_ Maven üzerinde önceden girilen tabanlı bir uygulama adı oluşturur şekilde değeri Azure arasında benzersiz olmalıdır _Artifactıd_ varsayılan olarak. _PackageName_ değeri oluşturulan işlev kodu için Java Paketi belirler.

```Output
Define value for property 'groupId': com.fabrikam.functions
Define value for property 'artifactId' : fabrikam-functions
Define value for property 'version' 1.0-SNAPSHOT : 
Define value for property 'package': com.fabrikam.functions
Define value for property 'appName' fabrikam-functions-20170927220323382:
Confirm properties configuration: Y
```

Maven adıyla yeni bir klasör içinde proje dosyalarını oluşturur _Artifactıd_. Basit bir projedeki oluşturulan kodu [tetiklenen HTTP](/azure/azure-functions/functions-bindings-http-webhook) istek gövdesi görüntülemektedir işlevi:

```java
public class Function {
    @FunctionName("hello")
    public String hello(@HttpTrigger(name = "req", methods = {"get", "post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req,
                        ExecutionContext context) {
        return String.format("Hello, %s!", req);
    }
}
```

## <a name="run-the-function-locally"></a>İşlev yerel olarak çalıştırma

Yeni oluşturulan proje klasörüne dizini değiştirin ve oluşturup işlevi ile Maven çalıştırın:

```
cd fabrikam-function
mvn clean package 
mvn azure-functions:run
```

İşlev çalıştığında bu Çıktıyı görürsünüz:

```Output
Listening on http://localhost:7071
Hit CTRL-C to exit...

Http Functions:

   hello: http://localhost:7071/api/hello
```

Yeni bir terminal curl kullanarak komut satırından işlevi tetikler:

```
curl -w '\n' -d LocalFunction http://localhost:7071/api/hello
```

```Output
Hello LocalFunction!
```

Kullanım `Ctrl-C` işlev kodu durdurmak için Terminal.

## <a name="deploy-the-function-to-azure"></a>İşlev Azure'a dağıtma

Azure işlevleri dağıtma işlemi, Azure CLI hesap kimlik bilgilerini kullanır. [Oturum ile Azure CLI](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) ve kodunuzu kullanarak yeni bir işlev uygulaması uygulamasına dağıtırsınız `azure-functions:deploy` Maven hedef.

```
az login
mvn azure-functions:deploy
```

Dağıtım tamamlandığında, Azure işlevi uygulamanıza erişmek için kullanabileceğiniz URL bakın:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

Curl kullanarak Azure üzerinde çalışan işlev uygulaması test edin:

```
curl -w '\n' https://fabrikam-function-20170920120101928.azurewebsites.net/api/hello -d AzureFunctions
```

```Output
Hello AzureFunctions!
```

## <a name="next-steps"></a>Sonraki adımlar

Java işlev uygulaması ile basit bir HTTP tetikleyicisi oluşturduğunuz ve Azure işlevleri için Dağıtılmış.

- Gözden geçirme [Java işlevleri Geliştirici Kılavuzu](functions-reference-java.md) Java işlevleri geliştirme hakkında daha fazla bilgi için.
- Kullanarak proje farklı tetikleyici içeren ek işlevler eklemek `azure-functions:add` Maven hedef.
- Visual Studio Code ile yerel olarak işlevlerinde hata ayıklama. İle [Java uzantısı paketi](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack) yüklü ve işlevleri projenizi açın Visual Studio Code [hata ayıklayıcısını](https://code.visualstudio.com/Docs/editor/debugging#_launch-configurations) 5005 numaralı bağlantı noktasına. Ardından Düzenleyicisi'nde bir kesme noktası ayarlayın ve yerel olarak çalışırken tetikleyicileri, işlevinizi tetiklemeleri: ![Visual Studio Code işlevlerinde hata ayıklama](media/functions-create-java-maven/vscode-debug.png)



