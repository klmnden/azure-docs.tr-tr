---
title: Azure Kubernetes Service (AKS) Jenkins ve mavi/yeşil dağıtım modelini kullanarak dağıtma
description: Azure Kubernetes Service (AKS) Jenkins ve mavi/yeşil dağıtım modelini kullanarak dağıtmayı öğrenin.
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
ms.openlocfilehash: 384681ae0ba212b485022ac81743528f96075ec8
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39716472"
---
# <a name="deploy-to-azure-kubernetes-service-aks-by-using-jenkins-and-the-bluegreen-deployment-pattern"></a>Azure Kubernetes Service (AKS) Jenkins ve mavi/yeşil dağıtım modelini kullanarak dağıtma

Azure Kubernetes Service (AKS), barındırılan Kubernetes ortamınızı hızla ve kapsayıcılı uygulamaları dağıtmak ve yönetmek kolay yönetir. Kapsayıcı düzenleme uzmanlığı gerekmez. AKS devam eden işlemler ve Bakım, sağlama, yükseltme ve ölçeklendirme kaynakları isteğe bağlı olarak yükünü de ortadan kaldırır. Uygulamalarınızı çevrimdışı duruma gerek yoktur. AKS hakkında daha fazla bilgi için bkz: [AKS belgelerini](/azure/aks/).

Mavi/yeşil dağıtım bir yeni (yeşil) sürüm dağıtılırken mevcut (mavi) bir sürümü canlı tutma yöntemini kullanır Azure DevOps, sürekli teslim modelidir. Genellikle, bu düzen, Yük Dengeleme kullanılarak yeşil dağıtıma trafiği miktarda artırma yönlendirmek için kullanır. İzleme bir olay bulunursa trafik, hala çalışıyor olan mavi dağıtıma yeniden yönlendirilebilir. Sürekli teslim hakkında daha fazla bilgi için bkz: [sürekli teslim nedir](/azure/devops/what-is-continuous-delivery).

Bu öğreticide, aşağıdaki görevleri nasıl gerçekleştireceğinizi öğrenin:

> [!div class="checklist"]
> * Mavi/yeşil dağıtım modeli hakkında bilgi edinin
> * Yönetilen bir Kubernetes kümesi oluşturma
> * Bir Kubernetes kümesi yapılandırmak için bir örnek betiği çalıştırma
> * El ile bir Kubernetes kümesi yapılandırma
> * Oluşturma ve bir Jenkins işi çalıştırma

## <a name="prerequisites"></a>Önkoşullar
- [GitHub hesabı](https://github.com) : örnek depoyu kopyalamak için bir GitHub hesabı gerekir.
- [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) : Kubernetes kümesi oluşturmak için Azure CLI 2. 0'ı kullanın.
- [Chocolatey](https://chocolatey.org): kubectl yüklemek için kullandığınız bir paket Yöneticisi.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): Kubernetes kümelerini karşı komutları çalıştırmak için kullandığınız bir komut satırı arabirimi.
- [jq](https://stedolan.github.io/jq/download/): basit, komut satırı bir JSON işlemci.

## <a name="clone-the-sample-app-from-github"></a>Github'dan örnek uygulamayı kopyalayın

Github'da Microsoft depoda, Jenkins ve mavi/yeşil desenini kullanarak AKS için dağıtmayı gösteren örnek bir uygulama bulabilirsiniz. Bu bölümde, GitHub, depo çatalı oluşturma ve yerel sisteminize uygulamasını kopyalayın.

1. İçin GitHub deposuna Gözat [todo uygulaması-java-üzerinde-azure](https://github.com/microsoft/todo-app-java-on-azure.git) örnek uygulaması.

    ![Microsoft GitHub deposuna örnek uygulamasının ekran görüntüsü](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Depo çatalı oluşturma seçerek **çatal** sağ sayfanın üst ve GitHub hesabınızda depo çatalı oluşturma için yönergeleri izleyin.

    ![Çatalınıza ekran görüntüsü, GitHub seçeneği](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Depo çatalı oluşturup sonra hesap adı için hesabınızın adını değiştirir ve burada depo çatallanmış gelen Not gösterir görürsünüz (Microsoft).

    ![Ekran görüntüsü, GitHub hesap adı ve Not](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Seçin **Kopyala veya indir**.

    ![Kopyala veya indir bir depo için ekran görüntüsü, GitHub seçeneği](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. İçinde **HTTPS ile kopya** penceresinde **kopyalama** simgesi.

    ![Ekran görüntüsü, GitHub seçeneği kopya URL'sini Panoya Kopyala](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Bir terminal veya Git Bash penceresini açın.

1. Dizinleri, deponun yerel kopyası (kopya) depolamak için istediğiniz istenilen konuma değiştirin.

1. Kullanarak `git clone` komutu, daha önce kopyaladığınız URL'yi kopyalayın.

    ![Git clone komutu Git Bash, ekran görüntüsü](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Kopyalama işlemini başlatmak için Enter tuşuna basın.

    ![Git clone komutu sonuçları Git Bash, ekran görüntüsü](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Dizinleri uygulama kaynak kopyasını içeren yeni oluşturulan dizine geçin.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Yönetilen bir Kubernetes kümesi oluşturmalı ve yapılandırmalısınız

Bu bölümde, aşağıdaki adımları gerçekleştirin:

- Yönetilen bir Kubernetes kümesi oluşturmak için Azure CLI 2. 0'ı kullanın.
- Küme Kurulum betiğini kullanarak ya da ayarlama hakkında bilgi edinin veya el ile.
- Azure Container Registry Hizmeti'nin bir örneğini oluşturun.

> [!NOTE]   
> AKS, şu anda Önizleme aşamasındadır. Önizleme için Azure aboneliğinizi etkinleştirme hakkında daha fazla bilgi için bkz. [hızlı başlangıç: Azure Kubernetes Service (AKS) kümesini dağıtma](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription).

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Yönetilen bir Kubernetes kümesi oluşturmak için Azure CLI 2.0 kullanma
İle yönetilen bir Kubernetes kümesi oluşturmak için [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), Azure CLI 2.0.25 sürümü kullandığınızdan emin olun veya üzeri.

1. Azure hesabınızda oturum açın. Aşağıdaki komutu girdikten sonra oturum açma işlemini tamamlamak anlatan yönergeler alırsınız. 
    
    ```bash
    az login
    ```

1. Çalıştırdığınızda `az login` komut önceki adımda (aboneliklerinin yanı sıra kimlikler) tüm Azure aboneliklerinizin listesi görüntülenir. Bu adımda, Azure aboneliği varsayılan ayarlayın. Değiştirin &lt;your-subscrıptıon-ID > yer tutucusunu istediğiniz Azure abonelik kimliği ile 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Bir kaynak grubu oluşturun. Değiştirin &lt;uygulamanızın kaynak-grup-adı > yer tutucusunu değiştirin ve yeni kaynak grubu adıyla &lt;your-location > yer tutucusunu konum. Komut `az account list-locations` tüm Azure konumları görüntüler. AKS Önizleme sırasında tüm konumları kullanılabilir. Şu anda geçersiz bir konum girin, hata iletisi kullanılabilir konumların listeler.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Kubernetes kümesi oluşturun. Değiştirin &lt;uygulamanızın kaynak-grup-adı > önceki adımda oluşturulan kaynak grubunun adı ile ve Değiştir &lt;uygulamanızın kubernetes-kümesi-adı > Yeni kümenize adı. (Bu işlemin tamamlanması birkaç dakika sürebilir.)

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Kubernetes kümesi oluşturma

Aks'deki bir mavi/yeşil dağıtım el ile ayarlayabilirsiniz veya kurulum ile sağlanan örnek komut dosyasını daha önce kopyalanabilir. Bu bölümde, her ikisini de yapmak nasıl bakın.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Örnek Kurulum betiği aracılığıyla Kubernetes kümesi oluşturma
1. Düzen **deploy/aks/setup/setup.sh** dosya, aşağıdaki yer tutucuları, ortamınız için uygun değerlerle değiştirin: 

    - **&lt;uygulamanızın kaynak-grup-adı >**
    - **&lt;Uygulamanızın kubernetes-kümesi-adı >**
    - **&lt;Uygulamanızın konumu >**
    - **&lt;Your-dns-adı-soneki >**

    ![Bazı yer tutucuları vurgulanmış olan bir bash setup.sh betikte ekran görüntüsü](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Kurulum betiğini çalıştırın.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>El ile bir Kubernetes kümesi ayarlama 
1. Kubernetes yapılandırma profili klasörünüze indirin.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Dizinine **dağıtma/aks/Kurulum** dizin. 

1. Aşağıdaki komutu çalıştırın **kubectl** hizmetler için genel bir uç nokta ve iki test uç noktaları ayarlamak için komutları.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Güncelleştirme genel kullanıma yönelik DNS adı ve test uç noktaları. Bir Kubernetes kümesi oluşturduğunuzda, ayrıca oluşturduğunuz bir [ek kaynak grubu](https://github.com/Azure/AKS/issues/3), adlandırma ile **MC_&lt;uygulamanızın kaynak-grup-adı >_ &lt; Uygulamanızın kubernetes-kümesi-adı >_&lt;your-location >**.

    Genel IP'ler, kaynak grubunu bulun.

    ![Kaynak grubundaki genel IP'lerin ekran görüntüsü](./media/jenkins-aks-blue-green-deployment/publicip.png)

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

    DNS adı, bir abonelikte benzersiz olması gerekir. Benzersiz olmasını sağlamak için kullanabileceğiniz `<your-dns-name-suffix>`.

### <a name="create-an-instance-of-container-registry"></a>Container Registry örneği oluşturma

1. Çalıştırma `az acr create` Container Registry örneği oluşturmak için komutu. Sonraki bölümde, ardından kullanabilirsiniz `login server` olarak Docker kayıt defteri URL'si.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Çalıştırma `az acr credential` , kapsayıcı kayıt defteri kimlik bilgilerini görüntülemek için komutu. Sonraki bölümde gerektiği Docker kayıt defteri kullanıcı adı ve parolayı unutmayın.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Jenkins sunucusunu hazırlama

Bu bölümde, test etmek için iyi bir yapıyı çalıştırmak için Jenkins sunucusunu hazırlama bakın. Ancak, kullanmanız gereken bir [Azure VM Aracısı](https://plugins.jenkins.io/azure-vm-agents) veya [Azure kapsayıcı Aracısı](https://plugins.jenkins.io/azure-container-agents) derlemelerinizi çalıştırmak için azure'da bir aracı aşamasında. Daha fazla bilgi için Jenkins makaleye bakın [yöneticisinde oluşturmanın güvenlikle ilgili etkileri](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master).

1. Dağıtım bir [Azure üzerinde Jenkins yöneticisini](https://aka.ms/jenkins-on-azure).

1. Sunucunun SSH aracılığıyla bağlanmak ve derleme araçları derleme çalıştırdığınız sunucuya yükleyin.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Docker yükleme](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Kullanıcı emin `jenkins` çalıştırma izni olan `docker` komutları.

1. [Kubectl yüklemek](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Jq indirme](https://stedolan.github.io/jq/download/).

1. Jq şu komutla yükleyin:

   ```bash
   sudo apt-get install jq
   ```
   
1. Eklenti, Jenkins Jenkins panosundan aşağıdaki adımları uygulayarak yükleyin:

    1. Seçin **Jenkins Yönet > Eklentileri Yönet > kullanılabilir**.
    1. Arayın ve Azure Container Service eklentisi yükleyin.

1. Azure'daki kaynakları yönetmek için kimlik bilgilerini ekleyin. Eklenti henüz yüklemediyseniz **Azure kimlik bilgileri** eklentisi.

1. Azure hizmet sorumlusu kimlik bilgilerinizi tür olarak eklemek **Microsoft Azure hizmet sorumlusu**.

1. Azure'da Docker kayıt defteri kullanıcı kimliğiniz ve parolanızı ("Container Registry örneği oluşturma" bölümünde edindiğiniz gibi) bir tür olarak eklemek **kullanıcı adıyla parola**.

## <a name="edit-the-jenkinsfile"></a>Jenkinsfile Düzenle

1. Kendi depoda Git `/deploy/aks/`açın `Jenkinsfile`.

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
    
    Kapsayıcı kayıt defteri kimlik güncelleştirin:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>İş oluşturma
1. Yeni bir proje türünde ekleme **işlem hattı**.

1. Seçin **işlem hattı** > **tanımı** > **işlem hattı SCM betikten**.

1. İle SCM depo URL'sini girin, &lt;çatallanmış bilgisayarınızı depo >.

1. Betik yolu olarak girin `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>İşi çalıştırma

1. Proje başarıyla yerel ortamınızda için çalıştırabildiğinizi doğrulayın. İşte nasıl: [proje yerel makinede çalıştırma](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it).

1. Jenkins işi çalıştırın. İşlem ilk çalıştırıldığında Jenkins varsayılan etkin olmayan ortamıdır mavi ortam todo uygulaması dağıtır. 

1. İşin çalıştırıldığını doğrulamak için bu URL'ler için gidin:
    - Genel uç noktası: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Mavi uç noktası- `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Yeşil uç noktası- `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

Varsayılan tomcat görüntüsünü gösterirken, yeşil uç noktası genel ve mavi test bitiş noktaları aynı güncelleştirme vardır. 

Derleme birden çok kez çalıştırırsanız, mavi ve yeşil dağıtımlar arasında geçiş yapar. Diğer bir deyişle, geçerli ortamı mavi ise, iş dağıtır ve yeşil ortamı için test eder. Ardından, testler iyidir, iş trafiği yönlendirmek için yeşil ortam uygulama genel uç nokta güncelleştirir.

## <a name="additional-information"></a>Ek bilgiler

Kesintisiz dağıtım hakkında daha fazla bilgi için bkz. Bu [Hızlı Başlangıç şablonu](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz kaynaklara artık ihtiyacınız olmadığında, bunları silebilirsiniz.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>Sorun giderme

Jenkins eklentileriyle ilgili hatalarla karşılaşırsanız [Jenkins JIRA](https://issues.jenkins-ci.org/) sayfasında söz konusu bileşenle ilgili sorun bildirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Jenkins ve mavi/yeşil dağıtım modelini kullanarak AKS'ye dağıtma öğrendiniz. Azure Jenkins sağlayıcısı hakkında daha fazla bilgi için Azure sitesindeki Jenkins bakın.

> [!div class="nextstepaction"]
> [Azure üzerinde Jenkins](/azure/jenkins/)