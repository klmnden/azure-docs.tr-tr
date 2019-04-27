---
title: Jenkins Azure işlevleri eklentiyi kullanarak, Azure işlevleri için dağıtma
description: Jenkins Azure işlevleri eklentiyi kullanarak, Azure işlevleri için dağıtmayı öğrenin
ms.service: jenkins
keywords: jenkins, azure, devops, java, azure işlevleri
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 02/23/2019
ms.openlocfilehash: bd8fa10ca0a9809891efc67ff930ab01d502eda9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60640971"
---
# <a name="deploy-to-azure-functions-using-the-jenkins-azure-functions-plugin"></a>Jenkins Azure işlevleri eklentiyi kullanarak, Azure işlevleri için dağıtma

[Azure işlevleri](/azure/azure-functions/) bir sunucusuz işlem hizmetidir. Azure işlevleri'ni kullanarak, sağlama veya altyapı yönetmenize gerek kalmadan kodu isteğe bağlı çalıştırabilirsiniz. Bu öğreticide, Azure işlevleri plugin kullanarak Azure işlevleri'ne bir Java işlev dağıtmayı gösterir.

## <a name="prerequisites"></a>Önkoşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- **Jenkins sunucusu**: Yüklü bir Jenkins sunucusu yoksa bu makaleye bakın [Azure'da bir Jenkins sunucusu oluşturma](./install-jenkins-solution-template.md).

  > [!TIP]
  > Bu öğreticide kullanılan kaynak kodu bulunan [Visual Studio Çin GitHub deposunu](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java).

## <a name="create-a-java-function"></a>Bir Java işlev oluşturma

Java Çalışma zamanı yığını ile bir Java işlev oluşturmak için kullanın [Azure portalında](https://portal.azure.com) veya [Azure CLI](/cli/azure/?view=azure-cli-latest).

Aşağıdaki adımlar, Azure CLI kullanarak bir Java işlev oluşturma işlemini gösterir:

1. Bir kaynak grubu oluşturmak yerine  **&lt;resource_group >** kaynak grubunuzun adı ile yer tutucu.

    ```cli
    az group create --name <resource_group> --location eastus
    ```

1. Yer tutucuları uygun değerlerle değiştirerek bir Azure depolama hesabı oluşturun.
 
    ```cli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. Yer tutucuları uygun değerlerle değiştirerek test işlev uygulaması oluşturun.

    ```cli
    az functionapp create --resource-group <resource_group> --consumption-plan-location eastus --name <function_app> --storage-account <storage_account>
    ```
    
1. Yer tutucuları uygun değerlerle değiştirerek sürüm 2.x çalışma zamanı için güncelleştirin.

    ```cli
    az functionapp config appsettings set --name <function_app> --resource-group <resource_group> --settings FUNCTIONS_EXTENSION_VERSION=~2
    ```

## <a name="prepare-jenkins-server"></a>Jenkins sunucusunu hazırlama

Aşağıdaki adımlarda, Jenkins sunucusu hazırlama açıklanmaktadır:

1. Dağıtım bir [Jenkins sunucusu](https://aka.ms/jenkins-on-azure) azure'da. Zaten yüklü, Jenkins sunucusu makale örneği yoksa [Azure'da bir Jenkins sunucusu oluşturma](./install-jenkins-solution-template.md) işleminde size yol gösterir.

1. SSH ile Jenkins örneği için oturum açın.

1. Jenkins örneği üzerinde aşağıdaki komutu kullanarak maven'i yükleyin:

    ```terminal
    sudo apt install -y maven
    ```

1. Jenkins örneği üzerinde yükleme [Azure işlevleri çekirdek Araçları](/azure/azure-functions/functions-run-local) terminal bir komut isteminde aşağıdaki komutları göndererek:

    ```terminal
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools
    ```

1. Jenkins panosunda, aşağıdaki eklentiler yükleyin:

    - Azure işlev eklentisi
    - EnvInject eklentisi

1. Jenkins, kimlik doğrulaması ve Azure kaynaklarına erişmek için bir Azure hizmet sorumlusu gerekir. Başvurmak [Azure App Service'e dağıtma](./tutorial-jenkins-deploy-web-app-azure-app-service.md) adım adım yönergeler için.

1. "Microsoft Azure hizmet sorumlusu" kimlik bilgisi türü, Azure hizmet sorumlusu kullanarak Jenkins ekleyin. Başvurmak [Azure App Service'e dağıtma](./tutorial-jenkins-deploy-web-app-azure-app-service.md#add-service-principal-to-jenkins) öğretici.

## <a name="fork-the-sample-github-repo"></a>Örnek GitHub depo çatalı oluşturma

1. [Oturum açmak için tek veya hatta örnek uygulama için GitHub deposunu](https://github.com/VSChina/odd-or-even-function.git).

1. Github'da sağ üst köşedeki seçin **çatal**.

1. GitHub hesabınızı seçin ve çatal tamamlamak için yönergeleri izleyin.

## <a name="create-a-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma

Bu bölümde, oluşturduğunuz [Jenkins işlem hattı](https://jenkins.io/doc/book/pipeline/).

1. Jenkins panosunda bir işlem hattı oluşturursunuz.

1. Etkinleştirme **çalıştırma için ortam hazırlama**.

1. Aşağıdaki ortam değişkenleri eklemek **özellikleri içerik**, yer tutucuları ortamınız için uygun değerlerle değiştirin:

    ```
    AZURE_CRED_ID=<service_principal_credential_id>
    RES_GROUP=<resource_group>
    FUNCTION_NAME=<function_name>
    ```
    
1. İçinde **Ardışık Düzen -> tanımı** bölümünden **işlem hattı SCM betikten**.

1. Kullanmak için GitHub çatalınızın URL'sini ve betik yolu ("doc/kaynak/jenkins/JenkinsFile") girin [JenkinsFile örnek](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile).

   ```
   node {
    stage('Init') {
        checkout scm
        }

    stage('Build') {
        sh 'mvn clean package'
        }

    stage('Publish') {
        azureFunctionAppPublish appName: env.FUNCTION_NAME, 
                                azureCredentialsId: env.AZURE_CRED_ID, 
                                filePath: '**/*.json,**/*.jar,bin/*,HttpTrigger-Java/*', 
                                resourceGroup: env.RES_GROUP, 
                                sourceDirectory: 'target/azure-functions/odd-or-even-function-sample'
        }
    }
    ```

## <a name="build-and-deploy"></a>Derleme ve dağıtma

Bu, artık Jenkins işini çalıştırmak için zamanı geldi.

1. Yetkilendirme anahtarı'ndaki yönergeleri aracılığıyla edinip [Azure işlevleri HTTP Tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook#authorization-keys) makalesi.

1. Tarayıcınızda, uygulamanın URL'sini girin. Yer tutucuları uygun değerlerle değiştirin ve için sayısal bir değer belirtin  **&lt;input_number >** Java işlevi için giriş olarak.

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. (Tek sayı - 365 - bir test olarak kullanıldığı) aşağıdaki örnek çıktıya benzer sonuçlar görürsünüz:

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımda oluşturduğunuz kaynakları silin:

```cli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure işlevleri hakkında daha fazla bilgi için aşağıdaki kaynağa bakın:
> [!div class="nextstepaction"]
> [Azure işlevleri belgeleri](/azure/azure-functions/)
