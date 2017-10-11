---
title: "Jenkins CI/CD Azure kapsayıcı Hizmeti'nde Kubernetes ile | Microsoft Docs"
description: "Dağıtıp Kubernetes Azure kapsayıcı Hizmeti'nde bir kapsayıcılı uygulamasını yükseltme Jenkins CI/CD işlemine otomatikleştirme"
services: container-service
documentationcenter: 
author: chzbrgr71
manager: johny
editor: 
tags: acs, azure-container-service, jenkins
keywords: "Docker, kapsayıcıları, Kubernetes, Azure, Jenkins"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: briar
ms.custom: mvc
ms.openlocfilehash: 2078d0694fc4dd6e83ecd2792588b4254980cd78
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="jenkins-integration-with-azure-container-service-and-kubernetes"></a>Azure kapsayıcı hizmeti ve Kubernetes Jenkins tümleştirme 
Bu öğreticide, Azure kapsayıcı hizmeti Jenkins platformu kullanılarak Kubernetes çok kapsayıcı uygulamasına, sürekli tümleştirme ayarlamak için işlem size rehberlik eder. İş akışı Docker hub'a kapsayıcı görüntüyü güncelleştirir ve dağıtımı sunumu kullanarak Kubernetes pod'ları yükseltir. 

## <a name="high-level-process"></a>Yüksek düzeyli işlem
Bu makalede ayrıntılı temel adımlar şunlardır: 
- Kapsayıcı Hizmeti'nde bir Kubernetes küme yükleyin
- Jenkins ayarlayın ve kapsayıcı hizmeti erişimi yapılandırma
- Jenkins iş akışı oluşturma
- CI/CD işlemi uçtan uca test edin

## <a name="install-a-kubernetes-cluster"></a>Kubernetes küme yükleyin
    
Aşağıdaki adımları kullanarak Azure kapsayıcı hizmeti Kubernetes kümede dağıtın. Tam belgelerine bulunduğu [burada](container-service-kubernetes-walkthrough.md).

### <a name="step-1-create-a-resource-group"></a>1. adım: bir kaynak grubu oluşturma
```azurecli
RESOURCE_GROUP=my-resource-group
LOCATION=westus

az group create --name=$RESOURCE_GROUP --location=$LOCATION
```

### <a name="step-2-deploy-the-cluster"></a>Adım 2: küme dağıtma
> [!NOTE]
> Aşağıdaki adımları ~/.ssh klasöründe bulunan yerel bir SSH ortak anahtarı gerektirir.
>

```azurecli
RESOURCE_GROUP=my-resource-group
DNS_PREFIX=some-unique-value
CLUSTER_NAME=any-acs-cluster-name

az acs create \
--orchestrator-type=kubernetes \
--resource-group $RESOURCE_GROUP \
--name=$CLUSTER_NAME \
--dns-prefix=$DNS_PREFIX \
--ssh-key-value ~/.ssh/id_rsa.pub \
--admin-username=azureuser \
--master-count=1 \
--agent-count=5 \
--agent-vm-size=Standard_D1_v2
```

## <a name="set-up-jenkins-and-configure-access-to-container-service"></a>Jenkins ayarlayın ve kapsayıcı hizmeti erişimi yapılandırma

### <a name="step-1-install-jenkins"></a>1. adım: Jenkins yükleme
1. Ubuntu 16.04 ile bir Azure VM oluşturma LTS.  Sonraki adımlarda yerel makinenizde bash kullanarak bu VM bağlanmak gerekli olduğundan, 'kimlik doğrulama türü' 'SSH ortak anahtarını' olarak ayarlayın ve ~/.ssh klasörünüzde yerel olarak depolanan SSH ortak anahtarını yapıştırın.  Ayrıca, 'kullanıcı adı' not alın bu kullanıcı adı Jenkins panoyu görüntülemek için gerekli olduğundan, belirttiğiniz ve daha sonraki adımlarda Jenkins VM bağlanmak için.
2. Jenkins yüklemeniz [yönergeleri](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Ubuntu). Daha ayrıntılı bir öğretici altındadır [howtoforge.com](https://www.howtoforge.com/tutorial/how-to-install-jenkins-with-apache-on-ubuntu-16-04).
3. Yerel makinenizde Jenkins panoyu görüntülemek için bağlantı noktası 8080 bağlantı noktası 8080 erişimi veren bir gelen kuralı ekleyerek izin vermek için Azure ağ güvenlik grubu güncelleştirin.  Alternatif olarak, şu komutu çalıştırarak bağlantı noktası iletme Kurulum:`ssh -i ~/.ssh/id_rsa -L 8080:localhost:8080 <your_jenkins_user>@<your_jenkins_public_ip`
4. Genel IP giderek tarayıcı kullanarak Jenkins sunucunuza bağlanmak (http:// < your_jenkins_public_ip >: 8080) ve ilk yönetici parolası ile ilk kez Jenkins Pano kilidini açın.  Yönetici parolası Jenkins VM üzerinde /var/lib/jenkins/secrets/initialAdminPassword konumunda depolanır.  Jenkins VM içine SSH için bu parolayı almak için kolay bir yoludur.: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Ardından, çalıştırın: `sudo cat /var/lib/jenkins/secrets/initialAdminPassword`.
5. Docker bu Jenkins makinesine yükleyin [yönergeleri](https://docs.docker.com/cs-engine/1.13/#install-on-ubuntu-1404-lts-or-1604-lts). Bu Jenkins işleri çalıştırılacak Docker komut sağlar.
6. Docker uç noktasına erişmek Jenkins izin vermek için Docker izinlerini yapılandırın.

    ```bash
    sudo chmod 777 /run/docker.sock
    ```
8. Yükleme `kubectl` CLI Jenkins üzerinde. Daha fazla ayrıntı altındadır [yükleme ve kubectl ayarlama](https://kubernetes.io/docs/tasks/kubectl/install/).  Jenkins işleri 'kubectl' Kubernetes kümesine dağıtmak ve yönetmek için kullanır.

    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl

    chmod +x ./kubectl

    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

### <a name="step-2-set-up-access-to-the-kubernetes-cluster"></a>2. adım: Kubernetes kümesine erişimi ayarlama

> [!NOTE]
> Aşağıdaki adımlar işlemi gerçekleştirmek için birden çok yaklaşım vardır. Sizin için en kolay yaklaşım kullanın.
>

1. Kopya `kubectl` yapılandırma dosyası Jenkins makineye böylece Jenkins işleri Kubernetes kümesine erişimi. Bu yönergeler, kullandığınız varsayılır bash Jenkins VM daha farklı bir makineden ve yerel bir SSH ortak anahtarı makinenin ~/.ssh klasöründe depolanır.

```bash
export KUBE_MASTER=<your_cluster_master_fqdn>
export JENKINS_USER=<your_jenkins_user>
export JENKINS_SERVER=<your_jenkins_public_ip>
sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir -m 777 /home/$JENKINS_USER/.kube/ \
&& sudo ssh $JENKINS_USER@$JENKINS_SERVER sudo mkdir /var/lib/jenkins/.kube/ \
&& sudo scp -3 -i ~/.ssh/id_rsa azureuser@$KUBE_MASTER:.kube/config $JENKINS_USER@$JENKINS_SERVER:~/.kube/config \
&& sudo ssh -i ~/.ssh/id_rsa $JENKINS_USER@$JENKINS_SERVER sudo cp /home/$JENKINS_USER/.kube/config /var/lib/jenkins/.kube/config \
```
        
2. Jenkins Kubernetes küme erişilebilir olduğunu doğrulayın.  Bu, SSH Jenkins VM'de oturum yapmak için: `ssh <your_jenkins_user>@<your_jenkins_public_ip>`.  Ardından, Jenkins başarıyla kümenize bağlanmak doğrulayın: `kubectl cluster-info`.
    

## <a name="create-a-jenkins-workflow"></a>Jenkins iş akışı oluşturma

### <a name="prerequisites"></a>Ön koşullar

- Kod deposu için GitHub hesabı.
- Depolamak ve görüntülerini güncelleştirmek için docker hub'a hesabı.
- Yeniden ve güncelleştirilmiş kapsayıcılı uygulama. Bu örnek kapsayıcı uygulaması Golang içinde yazılmış kullanabilirsiniz: https://github.com/chzbrgr71/go-web 

> [!NOTE]
> Kendi GitHub hesabı aşağıdaki adımlar gerçekleştirilmelidir. Yukarıdaki depoyu kopyalama çekinmeyin ancak Jenkins erişmek ve Web kancalarını yapılandırmak için kendi hesabı kullanmanız gerekir.
>

### <a name="step-1-deploy-initial-v1-of-application"></a>1. adım: uygulamanın ilk v1 dağıtma
1. Aşağıdaki komutlarla Geliştirici makineden uygulaması oluşturma. Değiştir `myrepo` kendi değerlerinizle.
    
    ```bash
    git clone https://github.com/chzbrgr71/go-web.git
    cd go-web
    docker build -t myrepo/go-web .
    ```

2. Görüntü Docker hub'a iletin.

    ```bash
    docker login
    docker push myrepo/go-web
    ```

3. Kubernetes kümeye dağıtın.
    
    > [!NOTE] 
    > Düzen `go-web.yaml` kapsayıcı görüntü ve depo güncelleştirmek için dosya.
    >
        
    ```bash
    kubectl create -f ./go-web.yaml --record
    ```
### <a name="step-2-configure-jenkins-system"></a>2. adım: Jenkins sistem yapılandırma
1. Tıklatın **Jenkins yönetmek** > **sistem yapılandırma**.
2. Altında **GitHub**seçin **GitHub Sunucu Ekle**.
3. Bırakın **API URL** varsayılan olarak.
4. Altında **kimlik bilgileri**, Jenkins kimlik bilgilerini kullanarak eklemek **gizli metin**. GitHub kullanıcı hesap ayarlarınızda yapılandırılmış GitHub kişisel erişim belirteçleri kullanmanızı öneririz. Bunun hakkında daha fazla ayrıntı [burada.](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)
5. Tıklatın **Bağlantıyı Sına** bunu doğru şekilde yapılandırılmış emin olmak için.
6. Altında **genel özellikleri**, bir ortam değişkeni ekleyin `DOCKER_HUB` ve Docker hub'a parolanızı girin. (Bu gösteride kullanışlıdır ancak bir üretim senaryosu daha güvenli bir yöntem gerektirir.)
7. Kaydedin.

![Jenkins GitHub erişim](./media/container-service-kubernetes-jenkins/jenkins-github-access.png)

### <a name="step-3-create-the-jenkins-workflow"></a>3. adım: Jenkins iş akışı oluşturma
1. Jenkins öğesi oluşturun.
2. Bir ad girin (örneğin, "Web'de Git") seçip **Freestyle proje**. 
3. Denetleme **GitHub proje** ve GitHub depo URL'si sağlayın.
4. İçinde **kaynak kodu Yönetimi**, GitHub deposuna URL ve kimlik bilgilerini sağlayın. 
5. Ekleme bir **derleme adımı** türü **Kabuk yürütme** ve aşağıdaki metni kullanın:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME .
    docker login -u <your-dockerhub-username> -p ${DOCKER_HUB}
    docker push $WEB_IMAGE_NAME
    ```

6. Başka bir tane eklemek **derleme adımı** türü **Kabuk yürütme** ve aşağıdaki metni kullanın:

    ```bash
    WEB_IMAGE_NAME="myrepo/go-web:kube${BUILD_NUMBER}"
    kubectl set image deployment/go-web go-web=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

![Jenkins derleme adımları](./media/container-service-kubernetes-jenkins/jenkins-build-steps.png)
    
7. Jenkins öğeyi kaydetmek ve test **şimdi yapı**.

### <a name="step-4-connect-github-webhook"></a>4. adım: GitHub Web kancası bağlanma
1. Oluşturduğunuz Jenkins öğeyi tıklatın **yapılandırma**.
2. Altında **yapı tetikleyicileri**seçin **GITScm yoklama için GitHub kanca tetikleyici** ve **kaydetmek**. Bu, GitHub Web kancası otomatik olarak yapılandırır.
3. Web Git, GitHub deposuna içinde tıklatın **ayarlar > Web Kancalarını**.
4. Jenkins Web kancası URL'si başarıyla eklendiğini doğrulayın. URL "github-Web kancası" bitmelidir.

![Jenkins Web kancası yapılandırma](./media/container-service-kubernetes-jenkins/jenkins-webhook.png)

## <a name="test-the-cicd-process-end-to-end"></a>CI/CD işlemi uçtan uca test edin

1. Depodaki ve anında iletme/eşitlemesi için kod GitHub deposuyla güncelleştirin.
2. Jenkins konsolundan denetleyin **yapı geçmiş** ve iş çalıştırıldığını doğrulayın. Ayrıntıları görmek için görünümü konsol çıkışı.
3. Kubernetes yükseltilmiş dağıtım ayrıntılarını görüntüleyin:

    ```bash
    kubectl rollout history deployment/go-web
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Azure kapsayıcı kayıt defteri dağıtın ve güvenli bir depoda görüntüleri saklamak. Bkz: [Azure kapsayıcı kayıt defteri belgeleri](https://docs.microsoft.com/azure/container-registry).
- Yan yana dağıtım ve otomatikleştirilmiş testleri Jenkins içeren daha karmaşık bir iş akışı oluşturma.
- CI/CD Jenkins ve Kubernetes hakkında daha fazla bilgi için bkz: [Jenkins blog](https://jenkins.io/blog/2015/07/24/integrating-kubernetes-and-jenkins/).
