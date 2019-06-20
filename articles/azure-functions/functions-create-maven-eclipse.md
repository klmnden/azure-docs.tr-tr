---
title: Eclipse ile Java ile bir Azure işlev uygulaması oluşturma | Microsoft Docs
description: Java ile Eclipse için Azure işlevleri, sunucusuz uygulama oluşturma ve basit bir HTTP yayımlama ile ilgili nasıl yapılır Kılavuzu tetiklenir.
services: functions
documentationcenter: na
author: jeffhollan
manager: jpconnock
keywords: Azure işlevleri, İşlevler, olay işleme, işlem, sunucusuz mimari, java
ms.service: azure-functions
ms.devlang: java
ms.topic: conceptual
ms.date: 07/01/2018
ms.author: jehollan
ms.custom: mvc, devcenter
ms.openlocfilehash: 9dcc959e51aa42fd6ef3173dba2aec8d9970deb1
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67154564"
---
# <a name="create-your-first-function-with-java-and-eclipse"></a>Java ile Eclipse kullanarak ilk işlevinizi oluşturma 

Bu makalede nasıl oluşturulacağını gösterir bir [sunucusuz](https://azure.microsoft.com/solutions/serverless/) işlev projesi Eclipse IDE ve Apache Maven ile test ve hata ayıklamak ve ardından Azure işlevleri'ne dağıtın. 

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve Eclipse ile bir işlev uygulaması geliştirmek için aşağıdakilerin yüklü olması gerekir:

-  [Java Developer Kit](https://www.azul.com/downloads/zulu/), sürüm 8.
-  [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri.
-  [Eclipse](https://www.eclipse.org/downloads/packages/), Java ve Maven ile destekler.
-  [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT] 
> Bu hızlı başlangıcın tamamlanabilmesi için JAVA_HOME ortam değişkeni JDK’nin yükleme konumu olarak ayarlanmalıdır.

Ayrıca yükleme için önemle tavsiye edilir [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2), çalışan ve Azure işlevlerinde hata ayıklama için yerel bir ortam sağlar. 

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. Eclipse'te, seçin **dosya** menüsü, ardından **New -&gt; Maven projesi**. 
1. Varsayılan değerleri kabul **yeni bir Maven projesi** seçin ve iletişim kutusunda **sonraki**.
1. Seçin **ekleme Archetype** ve girdileri eklemek [azure işlevleri archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
    - Archetype grubu kimliği: com.microsoft.azure
    - Yapıt kimliği archetype: azure işlevleri archetype
    - Sürüm: En son sürümünü kullanın **1.22** gelen [merkezi depo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![Eclipse Maven oluşturma](media/functions-create-first-java-eclipse/functions-create-eclipse.png)  
1. Tıklayın **Tamam** ve ardından **sonraki** şu anlık görüntü gibi değerler girmek için (Lütfen farklı bir appName dışında kullanın **fabrikam işlevi 20170920120101928**), ve sonunda **son**.
    ![Eclipse Maven create2](media/functions-create-first-java-eclipse/functions-create-eclipse2.png)  

Maven, _artifactId_ adlı yeni bir dosyada proje dosyalarını oluşturur. Oluşturulan kod projesinde basit bir [HTTP ile tetiklenen](/azure/azure-functions/functions-bindings-http-webhook) tetikleme HTTP istek gövdesini yankılayan işlevi.

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE içinde yerel olarak çalıştırma

> [!NOTE]
> [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2) çalıştırın ve işlevleri yerel olarak hata ayıklama için yüklü olması gerekir.

1. Oluşturulan projeye sağ tıklayın ve ardından **Çalıştır** ve **Maven derleme**.
1. İçinde **yapılandırmasını Düzenle** iletişim kutusunda, Enter `package` içinde **hedefleri** ve **adı** alanlarını seçip **çalıştırma**. Derleme ve işlev kodunu paketi.
1. Derleme tamamlandıktan sonra yukarıdaki gibi kullanarak başka bir çalıştırma yapılandırması oluşturun `azure-functions:run` adını ve hedef olarak. Seçin **çalıştırma** işlevi IDE'de çalıştırmak için.

İşiniz bittiğinde, konsol penceresinde çalışma zamanı sonlandırma işlevinizi test etme. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

### <a name="debug-the-function-in-eclipse"></a>Hata ayıklayın eclipse'te işlevi

İçinde **Çalıştır** yapılandırma değişikliği önceki adımda ayarladığınız `azure-functions:run` için `azure-functions:run -DenableDebug` ve güncelleştirilmiş yapılandırmayı hata ayıklama modunda işlev uygulamasını başlatmak için çalıştırın.

Seçin **çalıştırma** menü ve açık **hata ayıklama yapılandırmaları**. Seçin **uzak Java uygulaması** ve yeni bir tane oluşturun. Yapılandırmanıza bir ad verin ve ayarlarını doldurun. Bağlantı noktası olan varsayılan işlevi konak tarafından açılmış hata ayıklama bağlantı noktası ile tutarlı olmalıdır `5005`. Kurulum sonrasında, tıklayarak `Debug` hata ayıklama başlatılamıyor.

![Eclipse işlevlerinde hata ayıklama](media/functions-create-first-java-eclipse/debug-configuration-eclipse.PNG)

Kesme noktaları ayarlayın ve nesneleri IDE kullanarak işlevinizi denetleyin. İşiniz bittiğinde hata ayıklayıcı ve çalışan işlevi konak durdurun. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

Azure İşlevleri’ne dağıtım işlemi, Azure CLI’dan hesap kimlik bilgilerini kullanır. [Azure CLI ile oturum açın](/cli/azure/authenticate-azure-cli?view=azure-cli-latest) bilgisayarınızın komut istemini kullanarak devam etmeden önce.

```azurecli
az login
```

Kodunuzu yeni işlev uygulama kullanarak bir dağıtın `azure-functions:deploy` Maven hedefi yeni **Çalıştır** yapılandırma.

Dağıtım tamamlandığında, Azure işlev uygulamanıza erişmek için kullanabileceğiniz URL’yi görürsünüz:

```output
[INFO] Successfully deployed Function App with package.
[INFO] Deleting deployment package from Azure Storage...
[INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
[INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
[INFO] ------------------------------------------------------------------------
```

## <a name="next-steps"></a>Sonraki adımlar

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- `azure-functions:add` Maven hedefini kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin.
