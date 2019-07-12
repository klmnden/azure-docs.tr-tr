---
title: Azure geliştirme alanları kullanarak Kubernetes üzerinde Java ile geliştirme
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: quickstart
description: Kapsayıcılar, mikro hizmetler ve Azure üzerinde Java hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Java, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: b3e199f38f6f57cf10991f7e03757b8b603f74ad
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706873"
---
# <a name="quickstart-develop-with-java-on-kubernetes-using-azure-dev-spaces"></a>Hızlı Başlangıç: Azure geliştirme alanları kullanarak Kubernetes üzerinde Java ile geliştirme

Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
- Visual Studio Code kullanarak kapsayıcıları kodda yinelemeli olarak geliştirin.
- Visual Studio Code geliştirme alanınızdan kodunda hata ayıklayın.


## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Hesabınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Visual Studio Code'u](https://code.visualstudio.com/download).
- [Azure geliştirme alanları](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) ve [Azure geliştirme alanları için Java hata ayıklayıcı](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) Visual Studio Code için Uzantılar.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Yüklenmiş ve yapılandırılmış maven](https://maven.apache.org).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Service kümesi oluşturma

Bir AKS kümesinde oluşturmak gereken bir [bölge desteklenen][supported-regions]. Aşağıdaki komutları adlı bir kaynak grubu oluşturacaksınız *MyResourceGroup* ve adlı bir AKS kümesi *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
```

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Azure geliştirme alanları AKS kümenizde etkinleştirme

Kullanım `use-dev-spaces` komutunu kullanarak AKS kümenizde geliştirme alanları etkinleştirme ve yönergeleri izleyin. Geliştirme alanları aşağıdaki komutu etkinleştirir *MyAKS* içinde küme *MyResourceGroup* grup ve oluşturan bir *varsayılan* geliştirme alanı.

```cmd
$ az aks use-dev-spaces -g MyResourceGroup -n MyAKS

'An Azure Dev Spaces Controller' will be created that targets resource 'MyAKS' in resource group 'MyResourceGroup'. Continue? (y/N): y

Creating and selecting Azure Dev Spaces Controller 'MyAKS' in resource group 'MyResourceGroup' that targets resource 'MyAKS' in resource group 'MyResourceGroup'...2m 24s

Select a dev space or Kubernetes namespace to use as a dev space.
 [1] default
Type a number or a new name: 1

Kubernetes namespace 'default' will be configured as a dev space. This will enable Azure Dev Spaces instrumentation for new workloads in the namespace. Continue? (Y/n): Y

Configuring and selecting dev space 'default'...3s

Managed Kubernetes cluster 'MyAKS' in resource group 'MyResourceGroup' is ready for development in dev space 'default'. Type `azds prep` to prepare a source directory for use with Azure Dev Spaces and `azds up` to run.
```

## <a name="get-sample-application-code"></a>Örnek uygulama kodunu alma

Bu makalede, kullandığınız [Azure geliştirme alanları örnek uygulama](https://github.com/Azure/dev-spaces) Azure geliştirme alanları göstermek için.

Uygulamasını github'dan kopyalayın.

```cmd
git clone https://github.com/Azure/dev-spaces
```

## <a name="prepare-the-sample-application-in-visual-studio-code"></a>Örnek uygulamayı Visual Studio Code hazırlayın

Visual Studio Code'u açın, *dosya* ardından *Aç...* , gitmek *geliştirme-alanları/samples/java/alma-çalışmaya/webfrontend* dizin seçeneğine tıklayıp *açık*.

Artık *webfrontend* proje Visual Studio Code'da açın. Uygulama geliştirme alanınızda çalıştırmak için komut paletinden Azure geliştirme alanları uzantısını kullanarak Docker ve Helm grafiği varlıklar oluşturun.

Komut paletini Visual Studio Code'da açmak için *görünümü* ardından *komut paleti*. Yazmaya başlayın `Azure Dev Spaces` tıklayın `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

![Azure geliştirme alanları için yapılandırma dosyalarını hazırlama](./media/common/command-palette.png)

Visual Studio Code Ayrıca, temel görüntüleri, kullanıma sunulan bağlantı noktası ve genel bir uç nokta yapılandırma istediğinde seçin `Azul Zulu OpenJDK for Azure (Free LTS)` temel görüntü `8080` kullanıma sunulan bağlantı noktası ve `Yes` genel bir uç noktayı etkinleştirme.

![Temel görüntü seçin](media/get-started-java/select-base-image.png)

![Kullanıma sunulan bağlantı noktası seçin](media/get-started-java/select-exposed-port.png)

![Genel bir uç nokta seçin](media/get-started-java/select-public-endpoint.png)

Bu komut bir docker dosyası ve Helm grafiği oluşturarak Azure geliştirme alanlarında çalıştırmak üzere projenizi hazırlar. Ayrıca oluşturur bir *.vscode* projenizin kökünde yapılandırmasını hata ayıklama dizin.

## <a name="build-and-run-code-in-kubernetes-from-visual-studio"></a>Derleme ve kod Kubernetes'te Visual Studio'dan çalıştırma

Tıklayarak *hata ayıklama* simgesine tıklayın ve sol *başlatma Java programı'nı (AZDS)* en üstünde.

![Java Program başlatma](media/get-started-java/debug-configuration.png)

Bu komut, oluşturur ve hizmetinizi Azure geliştirme alanlarında çalıştırır. *Terminal* penceresinin altındaki hizmetiniz için Azure geliştirme alanları çalıştıran yapı çıkışını ve URL'leri gösterir. *Hata ayıklama konsolunu* günlük çıktısını gösterir.

> [!Note]
> Tüm Azure geliştirme alanları komutlarında görmüyorsanız, *komut paleti*, yüklediğinizden emin olun [Azure geliştirme alanları için Visual Studio Code uzantısı](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Ayrıca, açtığınız doğrulayın *geliştirme-alanları/samples/java/alma-çalışmaya/webfrontend* Visual Studio code'da dizin.

Genel URL açarak çalışan hizmeti görebilirsiniz.

Tıklayın *hata ayıklama* ardından *hata ayıklamayı Durdur* hata ayıklayıcıyı durdurmak için.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizi güncelleştirilmiş bir sürümünü dağıtmak için projenizdeki herhangi bir dosyayı güncelleştirmek ve yeniden *başlatma Java programı'nı (AZDS)* . Örneğin:

1. Uygulamanızı hala çalışıyorsa tıklayın *hata ayıklama* ardından *hata ayıklamayı Durdur* durdurmak ister.
1. Güncelleştirme [satır 19 ' `src/main/java/com/ms/sample/webfrontend/Application.java` ](https://github.com/Azure/dev-spaces/blob/master/samples/java/getting-started/webfrontend/src/main/java/com/ms/sample/webfrontend/Application.java#L19) için:
    
    ```java
    return "Hello from webfrontend in Azure!";
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Yeniden çalıştırılan *başlatma Java Program (AZDS)* .
1. Çalışan hizmetinize gidin ve yaptığınız değişiklikleri gözlemleyin.
1. Tıklayın *hata ayıklama* ardından *hata ayıklamayı Durdur* uygulamanızı durdurmak için.

## <a name="setting-and-using-breakpoints-for-debugging"></a>Ayarlama ve hata ayıklama için kesme noktaları kullanma

Hizmeti kullanmaya başlayın *başlatma Java programı'nı (AZDS)* . Bu ayrıca, hizmet hata ayıklama modunda çalıştırır.

Geri gidin *Gezgini* tıklayarak görünümü *görünümü* ardından *Gezgini*. Açık `src/main/java/com/ms/sample/webfrontend/Application.java` ve imlecinizi buraya koymanız-19. satırda bir yere tıklayın. Bir kesme noktası ayarlamak için isabet *F9* veya *hata ayıklama* ardından *iki durumlu kesme noktası*.

Hizmetinizi bir tarayıcıda açın ve herhangi bir ileti görüntülenir dikkat edin. Visual Studio Code için geri dönün ve satır 19 vurgulanır gözlemleyin. Kesme satır 19 hizmeti duraklatıldı. Hizmeti sürdürmek için isabet *F5* veya *hata ayıklama* ardından *devam*. Tarayıcınıza dönün ve ileti görüntülenecektir dikkat edin.

Bir hata ayıklayıcısı ekli Kubernetes'te hizmetinizi çalışırken, çağrı yığını, yerel değişkenleri ve özel durum bilgileri gibi bilgileri hata ayıklamak için tam erişime sahip olursunuz.

Kesme noktası 19 satırında imleci yerleştirerek kaldırmak `src/main/java/com/ms/sample/webfrontend/Application.java` ve ulaşmaktan *F9*.

## <a name="update-code-from-visual-studio-code"></a>Visual Studio code'dan kodunu güncelleştirme

Hizmet hata ayıklama modunda çalışırken, satır 19 güncelleştirme `src/main/java/com/ms/sample/webfrontend/Application.java`. Örneğin:
```java
return "Hello from webfrontend in Azure while debugging!";
```

Dosyayı kaydedin. Tıklayın *hata ayıklama* ardından *yeniden hata ayıklama* veya *hata ayıklama araç çubuğu*, tıklayın *yeniden hata ayıklama* düğmesi.

![Hata ayıklama Yenile](media/common/debug-action-refresh.png)

Hizmetinizi bir tarayıcıda açın ve güncelleştirilmiş iletinizi görüntülenen dikkat edin.

Yeniden oluşturma ve kod düzenleme yapılan her zaman, yeni bir kapsayıcı görüntüsü Azure geliştirme alanları artımlı olarak dağıtarak yerine daha hızlı bir düzenleme/hata ayıklama döngüsü sağlamak için mevcut kapsayıcı içindeki kodu yeniden derler.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure geliştirme alanları, birden çok kapsayıcıya daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olur ve farklı sürümleri ya da farklı alanları kodunuzun dalları birlikte çalışarak işbirliğine dayalı geliştirme nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Birden çok kapsayıcı ve takım geliştirme ile çalışma](multi-service-java.md)


[supported-regions]: about.md#supported-regions-and-configurations