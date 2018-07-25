---
title: Azure Kubernetes hizmeti (AKS'ye) Jenkins ve mavi/yeşil dağıtım modelini kullanarak dağıtma
description: Azure Kubernetes Service (Jenkins ve mavi/yeşil dağıtım modelini kullanarak AKS için) dağıtma hakkında bilgi edinin
services: app-service\web
documentationcenter: ''
author: tomarcher
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/23/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 472622f78303593b7a4d5d5136aa47f34a1f44b1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228189"
---
# <a name="deploy-to-azure-kubernetes-service-aks-using-jenkins-and-bluegreen-deployment-pattern"></a>Azure Kubernetes hizmeti (AKS'ye) Jenkins ve mavi/yeşil dağıtım modelini kullanarak dağıtma

Azure Kubernetes Hizmeti (AKS), barındırılan Kubernetes ortamınızı yöneterek kapsayıcılı uygulamaları, kapsayıcı yönetimi uzmanlığı gerekmeden hızla ve kolayca dağıtma olanağı sunar. AKS Süren işlemlerin ve bakımın yükünü sağlayarak, yükselterek ve uygulamalarınızı çevrimdışı duruma getirmeden kaynakları isteğe bağlı olarak ölçeklendirme tarafından da kaldırır. AKS hakkında daha fazla bilgi için bkz [AKS belgelerini](/azure/aks/).

Mavi/yeşil dağıtım, yeni bir (yeşil) sürüm dağıtılırken mevcut bir (mavi) sürümü etkin tutma yöntemini kullanan bir DevOps sürekli teslim (CD) modelidir. Genellikle, bu düzen, Yük Dengeleme kullanılarak yeşil dağıtıma trafiği miktarda artırma yönlendirmek için kullanır. İzleme bir olay bulunursa trafik çalışmakta olan mavi dağıtıma yeniden yönlendirilebilir. Makale için sürekli teslim hakkında daha fazla bilgi için bkz. [sürekli teslim nedir](/azure/devops/what-is-continuous-delivery).

Bu öğreticide, Jenkins ve mavi/yeşil dağıtım modelini kullanarak AKS'ye dağıtma öğrenmek aşağıdaki görevleri gerçekleştirmek nasıl öğrenin:

> [!div class="checklist"]
> * Mavi/yeşil dağıtım modeli hakkında bilgi edinin
> * Yönetilen bir Kubernetes kümesi oluşturun.
> * Bir Kubernetes kümesi yapılandırmak için bir örnek betiği çalıştırma
> * El ile bir Kubernetes kümesi yapılandırma
> * Oluşturma ve bir Jenkins işi çalıştırma

## <a name="prerequisites"></a>Önkoşullar
- [GitHub hesabı](https://github.com) : örnek depoyu kopyalamak için bir GitHub hesabı gerekir.
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) : Azure CLI 2.0, Kubernetes kümesini oluşturmak için kullanılır.
- [Chocolatey](https://chocolatey.org) -kubectl yüklemek için kullanılan bir paket Yöneticisi.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) : Kubernetes kümelerini karşı komutları çalıştırmak için bir komut satırı arabirimi.
- [jq](https://stedolan.github.io/jq/download/) : basit bir komut satırı JSON işlemcisine giden.

## <a name="clone-the-sample-app-from-github"></a>Github'dan örnek uygulamayı kopyalayın

Jenkins ve mavi/yeşil desenini kullanarak AKS için dağıtmayı gösteren örnek bir uygulama bir Microsoft GitHub deposunda açıktır. Bu bölümde, GitHub, depo çatalı oluşturma ve yerel sisteminize uygulamasını kopyalayın.

1. İçin GitHub deposuna Gözat [todo uygulaması-java-üzerinde-azure](https://github.com/microsoft/todo-app-java-on-azure.git) örnek uygulaması.

    ![Microsoft GitHub deposuna örnek uygulaması.](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Depo çatalı oluşturma seçerek **çatal** sağ sayfanın üst ve GitHub hesabınızda depo çatalı oluşturma için yönergeleri izleyin.

    ![Örnek uygulamasını GitHub hesabınızda çatal oluşturun.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Depo çatalı oluşturup sonra hesap adı için hesabınızın adını değiştirir ve burada depo çatallanmış gelen Not gösterir bakın (Microsoft).

    ![Başka bir GitHub hesabına çatallanmış sonra örnek uygulaması'nı tıklatın.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Seçme **Kopyala veya indir**.

    ![GitHub hızlı bir şekilde kopyalayın veya bir depo indirme sağlar.](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. İçinde **HTTPS ile kopya** penceresinde Kopyala simgesini seçin.

    ![Kopya URL'sini panoya kopyalayın.](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Bir terminal veya Bash penceresini açın.

1. Dizinleri, deponun yerel kopyası (kopya) depolamak için istediğiniz istenilen konuma değiştirin.

1. Kullanarak `git clone` komutu, daha önce kopyaladığınız URL'yi kopyalayın.

    !["Git clone" ve deponun bir kopyasını oluşturmak için kopya URL'si yazın.](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Seçin &lt;Enter > kopyalama işlemini başlatmak için anahtar.

    !["Git clone" komutunu kullanarak test edebilirsiniz depo kişisel bir kopyasını oluşturur](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Dizinleri uygulama kaynak kopyasını içeren yeni oluşturulan dizine geçin.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Yönetilen bir Kubernetes kümesi oluşturmalı ve yapılandırmalısınız

Bu bölümde, aşağıdaki adımları gerçekleştirin:

- Yönetilen bir Kubernetes kümesi oluşturmak için Azure CLI 2. 0'ı kullanın.
- Kurulum betiği kullanarak ya da bir küme ayarlama hakkında bilgi edinin veya el ile.
- Azure kapsayıcı kayıt defteri oluşturun.

> [!NOTE]   
> AKS, şu anda Önizleme aşamasındadır. Önizleme için Azure aboneliğinizi etkinleştirme hakkında daha fazla bilgi için. bkz: [hızlı başlangıç: Azure Kubernetes Service (AKS) kümesini dağıtma ](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription) daha fazla ayrıntı için.

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Yönetilen bir Kubernetes kümesi oluşturmak için Azure CLI 2.0 kullanma
İle yönetilen bir Kubernetes kümesi oluşturmak için [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), Azure CLI 2.0.25 sürümü kullandığınızdan emin olun veya üzeri.

1. Azure hesabınızda oturum açın. Aşağıdaki girdikten sonra `az login` komutu nasıl signın tamamlanacağını açıklayan yönergeler verilmiştir. 
    
    ```bash
    az login
    ```

1. Çalıştırdığınızda `az login` (aboneliklerinin yanı sıra kimlikler) komutu bir önceki adımda, tüm Azure aboneliklerinizin listesini görüntüler. Bu adımda, Azure aboneliği varsayılan ayarlayın. Değiştirin &lt;your-subscrıptıon-ID > yer tutucusunu istediğiniz Azure abonelik kimliği ile 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Bir kaynak grubu oluşturun. Değiştirin &lt;uygulamanızın kaynak-grup-adı > yer tutucusunu değiştirin ve yeni kaynak grubu adıyla &lt;your-location > yer tutucusunu konum. Komut `az account list-locations` tüm Azure konumları görüntüler. AKS Önizleme sırasında tüm konumları kullanılabilir. Şu anda geçersiz bir konum girin, hata iletisi kullanılabilir konumların listeler.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Kubernetes kümesi oluşturun. Değiştirin &lt;uygulamanızın kaynak-grup-adı > önceki adımda oluşturulan kaynak grubunun adı ile ve Değiştir &lt;,-kubernetes-kümesi-adı > Yeni kümenize adı. (Bu işlemin tamamlanması birkaç dakika sürebilir.)

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Aks'deki bir mavi/yeşil dağıtım ayarlama ya da daha önce veya el ile kopyalanan örnekte sağlanan bir kurulum komut dosyası ile yapılabilir. Bu bölümde, her ikisini de yapmak nasıl bakın.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Örnek Kurulum betiği aracılığıyla Kubernetes kümesi oluşturma
1. Düzen **deploy/aks/setup/setup.sh** dosya, aşağıdaki yer tutucuları, ortamınız için uygun değerlerle değiştirin: 

    - **&lt;uygulamanızın kaynak-grup-adı >**
    - **&lt;Uygulamanızın kubernetes-kümesi-adı >**
    - **&lt;Uygulamanızın konumu >**
    - **&lt;Your-dns-adı-soneki >**

    ![Setup.sh betik ortamınız için değiştirilebilir birkaç yer tutucular içerir.](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Kurulum betiğini çalıştırın.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>El ile bir Kubernetes kümesi ayarlama 
1. Kubernetes yapılandırma profili klasörünüze indirin.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Dizini **dağıtma/aks/Kurulum** dizin. 

1. Aşağıdaki komutu çalıştırın **kubectl** hizmetler için genel bir uç nokta ve iki test uç noktaları ayarlamak için komutları.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Güncelleştirme genel kullanıma yönelik DNS adı ve test uç noktaları. Bir Kubernetes kümesi oluşturulduğunda bir [ek kaynak grubu](https://github.com/Azure/AKS/issues/3) adlandırma desenini ile oluşturulan **MC_&lt;uygulamanızın kaynak-grup-adı > _&lt; Uygulamanızın kubernetes-kümesi-adı >_&lt;your-location >**.

    Genel IP'ler kaynak grubunu bulun

    ![Genel IP kaynak grubunda](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Aşağıdaki komutu çalıştırarak her hizmetin dış IP adresini bulun:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Aşağıdaki komutla karşılık gelen IP adresi için DNS adını güncelleştirin:

    ```bash
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Arama için yineleyin `todoapp-test-blue` ve `todoapp-test-green`:

    ```bash
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    DNS adı, bir abonelikte benzersiz olması gerekir. `<your-dns-name-suffix>` benzersiz olmasını sağlamak için kullanılabilir.

### <a name="create-azure-container-registry"></a>Azure Container Registry oluşturma

1. Çalıştırma `az acr create` komutunu bir Azure Container Registry oluşturun. Azure Container Registry oluşturulduktan sonra kullanın `login server` sonraki bölümde Docker kayıt defteri URL'si olarak.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Çalıştırma `az acr credential` Azure Container Registry kimlik bilgilerinizi gösterilecek komutu. Sonraki bölümde kullanıldığından Docker kayıt defteri kullanıcı adı ve parolayı unutmayın.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Jenkins sunucusunu hazırlama

Bu bölümde, test etmek için iyi bir yapıyı çalıştırmak için Jenkins sunucusunu hazırlama bakın. Ancak, anlatıldığı gibi Jenkins makalede [yöneticisinde oluşturmanın güvenlikle ilgili etkileri](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master), kullanmanız önerilir bir [Azure VM Aracısı](https://plugins.jenkins.io/azure-vm-agents) veya [Azure kapsayıcı Aracısı](https://plugins.jenkins.io/azure-container-agents) için derlemelerinizi çalıştırmak için azure'da bir aracı hızla çalıştırın. 

1. Dağıtım bir [Azure üzerinde Jenkins yöneticisini](https://aka.ms/jenkins-on-azure).

1. Sunucunun SSH aracılığıyla bağlanmak ve derleme araçları derleme çalıştırdığınız sunucuya yükleyin.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Docker yükleme](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Bir kullanıcı olun `jenkins` çalıştırma izni olan `docker` komutları.

1. [Kubectl yüklemek](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Jq indirme](https://stedolan.github.io/jq/download/).

1. Jq şu komutla yükleyin:

   ```bash
   sudo apt-get install jq
   ```
   
1. Eklenti, Jenkins Jenkins panosundan aşağıdaki adımları uygulayarak yükleyin:

    1. Seçin **Jenkins Yönet > Eklentileri Yönet > kullanılabilir**.
    1. Arayın ve Azure Container Service eklentisi yükleyin.

1. Azure'daki kaynakları yönetmek için kullanılacak kimlik bilgilerini eklemeniz gerekir. Eklenti zaten yoksa, yükleme **Azure kimlik bilgileri** eklentisi.

1. Tür/türü Azure hizmet sorumlusu kimlik bilgilerinizi ekleme **Microsoft Azure hizmet sorumlusu**.

1. Azure Docker kayıt defteri kullanıcı adı ve parola ekleyin (bölümünde elde edilmiş olarak **Azure Container Registry oluşturma**) tür/türü olarak **kullanıcı adıyla parola**.

## <a name="edit-the-jenkinsfile"></a>Jenkinsfile Düzenle

1. Kendi depoda gidin `/deploy/aks/` açın `Jenkinsfile`

2. Dosyanın aşağıdaki gibi güncelleştirin:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    Ve ACR kimlik bilgileri güncelleştirin:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>İş oluşturma
1. Yeni bir proje türünde ekleme **işlem hattı**.

1. Seçin **işlem hattı > tanımı > işlem hattı SCM betikten**.

1. İle SCM depo URL'sini girin, &lt;çatallanmış bilgisayarınızı depo >

1. Betik yolu olarak girin `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>İşi çalıştırma

1. Proje başarıyla yerel ortamınızda için çalıştırabildiğinizi doğrulayın. [Yerel makinede proje çalıştırma](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it)

1. Jenkins işi çalıştırın. Jenkins işi ilk kez çalıştırıldığında, Jenkins todo uygulaması varsayılan etkin olmayan ortamıdır mavi ortamına dağıtın. 

1. İşin çalıştırıldığını doğrulamak için URL'leri göz atın:
    - Genel uç noktası: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Mavi uç noktası- `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Yeşil uç noktası- `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

Varsayılan tomcat görüntüsünü gösterirken, yeşil uç noktası genel ve mavi test bitiş noktaları aynı güncelleştirme vardır. 

Derleme birden çok kez çalıştırırsanız, mavi ve yeşil dağıtımlar arasında geçiş yapar. Diğer bir deyişle, geçerli ortamı mavi ise, iş yeşil ortama dağıtmak ve test ve testiyle bir sorun yoksa sonra uygulama genel uç nokta trafiği yönlendirmek için yeşil ortamı güncelleştirin.

## <a name="additional-information"></a>Ek bilgiler

Kesintisiz dağıtım hakkında daha fazla bilgi için bu kontrol [Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu öğreticide oluşturduğunuz Azure kaynaklarını silin.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Sorun giderme

Tüm hatalar Jenkins eklentileri ile karşılaşırsanız, sorunu bildirin [Jenkins JIRA](https://issues.jenkins-ci.org/) belirli bileşeni.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Kubernetes Service (Jenkins ve mavi/yeşil dağıtım modelini kullanarak AKS için) dağıtmayı öğrendiniz. Azure Jenkins sağlayıcısı hakkında daha fazla bilgi için Azure sitesindeki Jenkins bakın.

> [!div class="nextstepaction"]
> [Azure üzerinde Jenkins](/azure/jenkins/)