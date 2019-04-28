---
title: Intellij ile Java ile Azure işlevi oluşturma | Microsoft Docs
description: Oluşturma ve azure'da Java ve Intellij ile basit bir HTTP tetiklemeli, sunucusuz uygulama yayımlama hakkında bilgi edinin.
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
ms.openlocfilehash: da93c60b52edf509900adf89fb688a0596d9763b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342246"
---
# <a name="create-your-first-azure-function-with-java-and-intellij"></a>Intellij ile Java ile ilk Azure işlevinizi oluşturma

Bu makalede, gösterir:
- Oluşturma bir [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) Intellij Idea ve Apache Maven ile işlevi projesi
- Adımları test ve hata ayıklama kendi bilgisayarınıza tümleşik geliştirme ortamında (IDE) işlevi
- Azure işlevleri işlev projesi dağıtmak için yönergeler

<!-- TODO ![Access a Hello World function from the command line with cURL](media/functions-create-java-maven/hello-azure.png) -->

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı kurma

Java ve Intellij ile bir işlev geliştirmek için aşağıdaki yazılımları yükleyin:

- [Java Developer Kit](https://www.azul.com/downloads/zulu/) (JDK), sürüm 8
- [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üstü
- [Intellij Idea](https://www.jetbrains.com/idea/download), Maven ile topluluk veya Ultimate sürümleri
- [Azure CLI](https://docs.microsoft.com/cli/azure)

> [!IMPORTANT]
> JAVA_HOME ortam değişkeni JDK'nin yükleme konumu için bu makaledeki adımları tamamlayabilmeniz için ayarlamanız gerekir.

 Yüklemenizi öneririz [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2). Yazma, çalıştırma ve Azure işlevlerinde hata ayıklama için bir yerel geliştirme ortamı sağlar.

## <a name="create-a-functions-project"></a>İşlevler projesi oluşturma

1. Intellij Idea içinde seçin **yeni proje oluştur**.  
1. İçinde **yeni proje** penceresinde **Maven** sol bölmeden.
1. Seçin **archetype Oluştur** onay kutusunu işaretleyin ve ardından **ekleme Archetype** için [azure işlevleri archetype](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype).
1. İçinde **ekleme Archetype** penceresinde alanları aşağıdaki gibi doldurun:
    - _GroupId_: com.microsoft.azure
    - _Artifactıd_: azure işlevleri archetype
    - _Sürüm_: En son sürümü kullan [merkezi depo](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-archetype)
    ![archetype Intellij Idea içinde bir Maven projesi oluşturun](media/functions-create-first-java-intellij/functions-create-intellij.png)  
1. Seçin **Tamam**ve ardından **sonraki**.
1. Geçerli proje için ayrıntılarınızı girin ve seçin **son**.

Maven ile aynı ada sahip yeni bir klasörde proje dosyalarını oluşturur _Artifactıd_ değeri. Projenin oluşturulan kod basit bir [HTTP ile tetiklenen](/azure/azure-functions/functions-bindings-http-webhook) tetikleme HTTP istek gövdesini yankılayan işlevi.

## <a name="run-functions-locally-in-the-ide"></a>İşlevleri IDE içinde yerel olarak çalıştırma

> [!NOTE]
> Çalıştırın ve işlevleri yerel olarak hata ayıklama için yüklediğinizden emin olun [Azure işlevleri temel araçları, sürüm 2](functions-run-local.md#v2).

1. Etkinleştirmek veya değişiklikleri el ile içeri [otomatik olarak içeri aktarma](https://www.jetbrains.com/help/idea/creating-and-optimizing-imports.html).
1. Açık **Maven projeleriyle** araç çubuğu.
1. Genişletin **yaşam döngüsü**ve ardından açın **paket**. Çözümü oluşturulur ve yeni oluşturulan hedef dizinde paketlenir.
1. Genişletin **eklentileri** > **azure işlevleri** açın **azure-İşlevler: çalışma** Azure işlevleri yerel çalışma zamanı'nı başlatmak için.  
  ![Azure işlevleri için maven araç çubuğu](media/functions-create-first-java-intellij/functions-intellij-java-maven-toolbar.png)  

1. İşiniz bittiğinde Çalıştır iletişim kutusunu kapatın, işlevinizi test etme. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

## <a name="debug-the-function-in-intellij"></a>Intellij işlevde hata ayıklama

1. Ekleme işlevi konak hata ayıklama modunda başlatmak için **- DenableDebug** işlevinizi çalıştırdığınızda bağımsız değişken olarak. Yapılandırmada ya da değiştirebilirsiniz [maven hedefleri](https://www.jetbrains.com/help/idea/maven-support.html#run_goal) veya bir terminal penceresinde aşağıdaki komutu çalıştırın:  

   ```
   mvn azure-functions:run -DenableDebug
   ```

   Bu komut, 5005 sırasında bir hata ayıklama bağlantı noktasını açmak işlev konak neden olur.

1. Üzerinde **çalıştırma** menüsünde **yapılandırmalarını Düzenle**.
1. Seçin **(+)** eklemek için bir **uzak**.
1. Tamamlamak _adı_ ve _ayarları_ alanları tıklayın ve ardından **Tamam** yapılandırmayı kaydetmek için.
1. Kurulum sonrasında seçin **< uzak yapılandırma adı > hata ayıklama** veya klavyenizde hata ayıklamayı başlatmak için Shift + F9 tuşuna basın.

   ![Intellij işlevlerinde hata ayıklama](media/functions-create-first-java-intellij/debug-configuration-intellij.PNG)

1. İşlemi tamamladığınızda, hata ayıklayıcı ve çalışan işlemi durdurun. İşlevi yalnızca bir ana bilgisayar etkin olduğu ve çalıştırıldığı yerel olarak bir zaman olabilir.

## <a name="deploy-the-function-to-azure"></a>İşlevi Azure’a dağıtma

1. İşlevinizi Azure'a dağıtmadan önce şunları yapmalısınız [oturum açmak için Azure CLI kullanarak](/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

   ``` azurecli
   az login
   ```

1. Kullanarak kodunuzu yeni bir işlev dağıtın `azure-functions:deploy` Maven hedefini. Belirleyebilirsiniz **azure işlevleri: dağıtma** Maven projeleriyle penceresinde seçeneği.

   ```
   mvn azure-functions:deploy
   ```

1. İşlev başarıyla dağıtıldıktan sonra Azure CLI çıktı işleviniz için URL'yi bulun.

   ``` output
   [INFO] Successfully deployed Function App with package.
   [INFO] Deleting deployment package from Azure Storage...
   [INFO] Successfully deleted deployment package fabrikam-function-20170920120101928.20170920143621915.zip
   [INFO] Successfully deployed Function App at https://fabrikam-function-20170920120101928.azurewebsites.net
   [INFO] ------------------------------------------------------------------------
   ```

## <a name="next-steps"></a>Sonraki adımlar

- Java işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Java İşlevleri geliştirici kılavuzunu](functions-reference-java.md) gözden geçirin.
- Kullanarak projenize farklı tetikleyicilere sahip ek işlevler ekleyin `azure-functions:add` Maven hedefini.
