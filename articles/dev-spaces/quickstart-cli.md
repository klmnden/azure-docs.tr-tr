---
title: Azure geliştirme boşluklarla kubernetes'te uygulama dağıtma
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 07/08/2019
ms.topic: quickstart
description: Bir mikro hizmet Azure geliştirme alanları ile AKS dağıtma
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, kapsayıcılar, Helm, hizmet kafes, ağ hizmeti Yönlendirme, kubectl, k8s
manager: gwallace
ms.openlocfilehash: 39fb7658140a2eda948cd0dc0e58d71b0b9a053b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706268"
---
# <a name="quickstart-develop-an-application-on-kubernetes-using-azure-dev-spaces"></a>Hızlı Başlangıç: Azure geliştirme boşluklarla kubernetes'te uygulama geliştirme
Bu kılavuzda şunların nasıl yapıldığını öğreneceksiniz:

- Azure’da yönetilen bir Kubernetes ile Azure Dev Spaces’ı ayarlayın.
- Geliştirin ve kapsayıcıları komut satırını kullanarak kod çalıştırın.

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
- [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

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

Github'dan uygulama kopyalamak ve gidin *geliştirme-alanları/samples/nodejs/alma-çalışmaya/webfrontend* dizini:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/nodejs/getting-started/webfrontend
```

## <a name="prepare-the-application"></a>Uygulama hazırlama

Azure üzerinde geliştirme alanları uygulamanızı çalıştırmak için bir docker dosyası ve Helm grafiği gerekir. Bazı diller için gibi [Java][java-quickstart], [.NET core][netcore-quickstart], ve [Node.js][nodejs-quickstart], Azure geliştirme alanları istemci araçları ihtiyacınız olan tüm varlıklar oluşturabilir. Git, PHP ve Python gibi çok sayıda diğer dilleri, araçları istemci geçerli bir Dockerfile sağladığınız sürece Helm grafiği oluşturabilirsiniz.

Kubernetes kullanarak uygulamayı çalıştırmak için Docker ve Helm grafiği varlıkları oluşturmak `azds prep` komutu:

```cmd
azds prep --public
```

Çalıştırmalısınız `prep` komutunu *geliştirme-alanları/samples/nodejs/alma-çalışmaya/webfrontend* doğru Docker ve Helm grafiği varlıkları oluşturmak için dizin.

## <a name="build-and-run-code-in-kubernetes"></a>Kubernetes'de kodu oluşturma ve çalıştırma

Derleme ve AKS kullanarak kodunuzu çalıştırmak `azds up` komutu:

```cmd
$ azds up
Using dev space 'default' with target 'MyAKS'
Synchronizing files...2s
Installing Helm chart...2s
Waiting for container image build...2m 25s
Building container image...
Step 1/8 : FROM node
Step 2/8 : ENV PORT 80
Step 3/8 : EXPOSE 80
Step 4/8 : WORKDIR /app
Step 5/8 : COPY package.json .
Step 6/8 : RUN npm install
Step 7/8 : COPY . .
Step 8/8 : CMD ["npm", "start"]
Built container image in 6m 17s
Waiting for container...13s
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Service 'webfrontend' port 80 (http) is available at http://localhost:54256
...
```

Çıktıda gösterilen ortak URL'yi açarak çalışan hizmeti gördüğünüz `azds up` komutu. Bu örnekte genel URL: *http://webfrontend.1234567890abcdef1234.eus.azds.io/* .

Durdurursanız `azds up` komutunu kullanarak *Ctrl + c*, hizmet, AKS içinde çalışmaya devam eder ve genel URL sunulmaya devam edecektir.

## <a name="update-code"></a>Kodu güncelleştirme

Hizmetinizi güncelleştirilmiş bir sürümünü dağıtmak için projenizdeki herhangi bir dosyayı güncelleştirmek ve yeniden `azds up` komutu. Örneğin:

1. Varsa `azds up` hala, çalışıyor basın *Ctrl + c*.
1. Güncelleştirme [13'te satır `server.js` ](https://github.com/Azure/dev-spaces/blob/master/samples/nodejs/getting-started/webfrontend/server.js#L13) için:
    
    ```javascript
        res.send('Hello from webfrontend in Azure');
    ```

1. Yaptığınız değişiklikleri kaydedin.
1. Yeniden çalıştırılan `azds up` komutu:

    ```cmd
    $ azds up
    Using dev space 'default' with target 'MyAKS'
    Synchronizing files...1s
    Installing Helm chart...3s
    Waiting for container image build...
    ...    
    ```

1. Çalışan hizmetinize gidin ve yaptığınız değişiklikleri gözlemleyin.
1. Tuşuna *Ctrl + c* durdurmak için `azds up` komutu.

## <a name="clean-up-your-azure-resources"></a>Azure kaynaklarınızı temizleme

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Sonraki adımlar

Azure geliştirme alanları, birden çok kapsayıcıya daha karmaşık uygulamalar geliştirmenize nasıl yardımcı olur ve farklı sürümleri ya da farklı alanları kodunuzun dalları birlikte çalışarak işbirliğine dayalı geliştirme nasıl basitleştirebileceğinizi öğrenin.

> [!div class="nextstepaction"]
> [Azure geliştirme alanları takım geliştirme][team-quickstart]

[java-quickstart]: quickstart-java.md
[nodejs-quickstart]: quickstart-nodejs.md
[netcore-quickstart]: quickstart-netcore.md
[team-quickstart]: quickstart-team-development.md
[supported-regions]: about.md#supported-regions-and-configurations