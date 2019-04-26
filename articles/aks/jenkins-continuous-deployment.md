---
title: Öğretici - Azure Kubernetes Service'i (AKS) Jenkins ile Github'dan dağıtma
description: Jenkins'i sürekli tümleştirme (CI) için GitHub ve sürekli dağıtım (CD) Azure Kubernetes Service (AKS) ayarlama
services: container-service
ms.service: container-service
author: zr-msft
ms.author: zarhoads
ms.topic: article
ms.date: 01/09/2019
ms.openlocfilehash: 703aa081c8acf41f9206e2b0ccff45571367d2e8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465774"
---
# <a name="tutorial-deploy-from-github-to-azure-kubernetes-service-aks-with-jenkins-continuous-integration-and-deployment"></a>Öğretici: Github'dan Azure Kubernetes Service (AKS) Jenkins sürekli tümleştirme ve dağıtım ile dağıtma

Bu öğretici için github'dan örnek uygulama dağıtan bir [Azure Kubernetes Service (AKS)](/azure/aks/intro-kubernetes) kurarak sürekli tümleştirme (CI) ve Jenkins sürekli dağıtım (CD) kümesi. Github'a geçirdik, işlemeleri ileterek uygulamanızı güncelleştirdiğinizde bu şekilde, Jenkins otomatik olarak yeni bir kapsayıcı derlemesi çalıştırır, kapsayıcı görüntülerini Azure Container Registry (ACR) gönderir ve ardından uygulamanızı AKS çalıştırır. 

Bu öğreticide, bu görevleri tamamlamak:

> [!div class="checklist"]
> * Örnek Azure vote uygulaması için bir AKS kümesi dağıtın.
> * Temel bir Jenkins projesi oluşturun.
> * ACR ile etkileşim kurmak Jenkins için kimlik bilgilerini ayarlayın.
> * Bir Jenkins derleme işi ve otomatik yapılara için GitHub Web kancası oluşturun.
> * GitHub kod işlemelerine dayalı aks'deki bir uygulamayı güncelleştirmek için CI/CD işlem hattı test edin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için bu öğeler gerekir:

- Kubernetes, Git, CI/CD ve kapsayıcı görüntüleri ile ilgili temel bilgilere

- Bir [AKS kümesi] [ aks-quickstart] ve `kubectl` ile yapılandırılmış [AKS küme kimlik bilgilerini][aks-credentials]

- Bir [Azure Container Registry (ACR) kayıt defteri][acr-quickstart], ACR oturum açma sunucusu adını ve AKS küme için yapılandırılmış [ACR kayıt defteri ile kimlik doğrulaması][acr-authentication]

- Azure CLI Sürüm 2.0.46 veya daha sonra yüklenir ve yapılandırılır. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

- [Docker'ın yüklü] [ docker-install] geliştirme sisteminizde

- Bir GitHub hesabı [GitHub kişisel erişim belirteci][git-access-token]ve geliştirme sisteminizde yüklü Git istemcisi

- Jenkins dağıtmanın Bu örnek komut dosyası yerine kendi Jenkins örneği sağlarsanız, Jenkins gereksinimlerini örnek [Docker'ın yüklü ve yapılandırılmış] [ docker-install] ve [kubectl][kubectl-install].

## <a name="prepare-your-app"></a>Uygulamanızı hazırlama

Bu makalede, bir veya daha fazla pod'ların ve geçici veri depolama için Redis barındıran ikinci bir pod içinde barındırılan bir web arabirimi içeren örnek Azure vote uygulamasını kullanın. Otomatik dağıtım için Jenkins ve AKS tümleştirmeden önce ilk el ile hazırlamak ve AKS kümenizi Azure vote uygulamayı dağıtın. Bu el ile dağıtım, bir uygulamanın sürüm ve uygulamayı eylem görmenizi sağlar.

-Örnek uygulama için aşağıdaki GitHub depo çatalı oluşturma [ https://github.com/Azure-Samples/azure-voting-app-redis ](https://github.com/Azure-Samples/azure-voting-app-redis). Depo için GitHub hesabınızda çatal oluşturmak üzere sağ üst köşedeki **Fork** (Çatal Oluştur) düğmesini seçin.

Geliştirme sisteminizde çatal kopyalayın. Bu depoyu kopyalarken çatalınızın URL'sini kullandığınızdan emin olun:

```console
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Kopyalanan çatalınızın dizine değiştirin:

```console
cd azure-voting-app-redis
```

Örnek uygulama için gerekli kapsayıcı görüntülerini oluşturmak için kullanın *docker compose.yaml* ile dosya `docker-compose`:

```console
docker-compose up -d
```

Gerekli temel görüntüleri alınır ve uygulama kapsayıcıları üretilmiştir. Ardından [docker görüntüleri] [ docker-images] oluşturulan görüntüyü görmek için komutu. Üç görüntü indirilir veya oluşturulur. `azure-vote-front` görüntüsü uygulamayı içerir ve temel olarak `nginx-flask` görüntüsünü kullanır. `redis` Görüntüsü bir Redis örneği başlatmak için kullanılır:

```
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

Gönderebilmek için önce *azure-vote-front* , ACR için kapsayıcı görüntüsü alın, ACR oturum açma sunucusuyla [az acr listesi] [ az-acr-list] komutu. Aşağıdaki örnek, ACR oturum açma sunucusu adresi için bir kayıt defteri adlı kaynak grubunda alır *myResourceGroup*:

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanım [docker tag] [ docker-tag] komut görüntüyü ACR oturum açma sunucusu adını ve sürüm numarası ile etiketlemek için `v1`. Kendi sağlamak `<acrLoginServer>` adı, önceki adımda elde edilen:

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Son olarak, anında iletme *azure-vote-front* ACR kayıt defterinize görüntü. Yeniden değiştirin `<acrLoginServer>` kendi ACR kayıt defterinin oturum açma sunucusu adıyla gibi `myacrregistry.azurecr.io`:

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-the-sample-application-to-aks"></a>AKS için örnek uygulamayı dağıtma

AKS kümenizi örnek uygulamayı dağıtmak için Azure vote depo deponun kökünde Kubernetes bildirim dosyası kullanabilirsiniz. Açık *azure-vote-all-in-one-redis.yaml* gibi bildirim dosyası bir düzenleyiciyle `vi`. `microsoft` yerine ACR oturum açma sunucunuzun adını yazın. Satırda bu değer bulunana **47** bildirim dosyasının:

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Ardından, [kubectl uygulamak] [ kubectl-apply] AKS kümenizi uygulamayı dağıtmak için komut:

```console
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

Uygulamayı İnternette kullanıma sunmak için bir Kubernetes Yük Dengeleyici Hizmeti oluşturulur. Bu işlem birkaç dakika sürebilir. Yük Dengeleyici dağıtım ilerlemesini izlemek için kullanabilirsiniz [kubectl alma hizmeti] [ kubectl-get] komutunu `--watch` bağımsız değişken. *EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `Control + C` komutunu kullanın.

```console
$ kubectl get service azure-vote-front --watch

NAME               TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   LoadBalancer   10.0.215.27   <pending>     80:30747/TCP   22s
azure-vote-front   LoadBalancer   10.0.215.27   40.117.57.239   80:30747/TCP   2m
```

Uygulamayı iş başında görmek için hizmetin dış IP adresini bir web tarayıcısı açın. Azure vote uygulaması aşağıdaki örnekte gösterildiği gibi görüntülenir:

![AKS'de çalışan örnek azure vote uygulaması](media/aks-jenkins/azure-vote.png)

## <a name="deploy-jenkins-to-an-azure-vm"></a>Bir Azure VM'ye Jenkins dağıtma

Hızlı bir şekilde bu makaledeki kullanmak için Jenkins dağıtmak için bir Azure sanal makinesi dağıtma, ağ erişimi yapılandırma ve temel bir Jenkins yüklemesini tamamlamak için aşağıdaki betiği kullanabilirsiniz. Jenkins ve AKS kümesi arasında kimlik doğrulaması için betik, Kubernetes yapılandırma dosyası için Jenkins sistem geliştirme sisteminizden kopyalar.

> [!WARNING]
> Bu örnek betik bir Azure VM üzerinde çalışan bir Jenkins ortamı hızlıca sağlamak Tanıtım amaçlı var. Bir VM yapılandırın ve ardından gerekli kimlik bilgilerini görüntülemek için Azure özel betik uzantısı kullanır. *~/.Kube/config* Jenkins VM'ye kopyalanır.

Bir betik indirip çalıştırmak için aşağıdaki komutları çalıştırın. Bunu çalıştırılmadan önce herhangi bir betiğin içeriğini gözden geçirmeniz gereken- [ https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh ](https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh).

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

VM oluşturma ve Docker ve Jenkins için gerekli bileşenleri dağıtmak için birkaç dakika sürer. Betik tamamlandığında, aşağıdaki örnek çıktıda gösterildiği gibi bir adresi Jenkins sunucusunun ve Pano kilidini açmak için bir anahtar verir:

```
Open a browser to http://40.115.43.83:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

Görüntülenen URL için web tarayıcısını açın ve kilit açma anahtarı girin. İzleyin Jenkins yapılandırmasını tamamlamak için ekrandaki istemleri:

- Seçin **önerilen eklentileri yükle**
- İlk yönetici kullanıcıyı oluşturun. Bir kullanıcı adı gibi girin *azureuser*, daha sonra kendi güvenli parolanızı belirtin. Son olarak, bir tam ad ve e-posta adresi yazın.
- **Kaydet ve Bitir**’i seçin
- Jenkins hazır olduktan sonra **Jenkins kullanmaya başla**’yı seçin
    - Jenkins kullanmaya başladığınızda web tarayıcınız boş bir sayfa görüntülerse, Jenkins hizmetini yeniden başlatın. Hizmeti, SSH Jenkins örneği ve türdeki genel IP adresine yeniden `sudo service jenkins restart`. Hizmet yeniden başlatıldıktan sonra yenileme işlemini web tarayıcısı.
- Jenkins için kullanıcı adı ve yükleme işleminde oluşturduğunuz parola ile oturum açın.

## <a name="create-a-jenkins-environment-variable"></a>Jenkins ortam değişkeni oluşturma

Jenkins ortam değişkeni, ACR oturum açma sunucusu adını tutmak için kullanılır. Bu değişken, Jenkins derleme işi sırasında başvurulur. Bu ortam değişkenini oluşturmak için aşağıdaki adımları tamamlayın:

- Jenkins portalında sol taraftan **Jenkins'i Yönet** > **sistemini Yapılandır**
- Altında **genel özellikleri**seçin **ortam değişkenlerini**. Bu ada sahip bir değişken ekleyin `ACR_LOGINSERVER` ve değerini, ACR oturum açma sunucusu.

    ![Jenkins ortam değişkenleri](media/aks-jenkins/env-variables.png)

- Tamamlandığında, tıklayın **Kaydet** Jenkins yapılandırma sayfanın alt kısmındaki.

## <a name="create-a-jenkins-credential-for-acr"></a>ACR için Jenkins kimlik bilgisi oluşturma

Jenkins derleme ve ardından güncelleştirilmiş kapsayıcı görüntüleri ACR'ye gönderme izin vermek için ACR için kimlik bilgilerini belirtmeniz gerekir. Bu kimlik doğrulaması, Azure Active Directory Hizmet sorumluları kullanabilirsiniz. Önkoşullarda, ile AKS kümesi için hizmet sorumlusu yapılandırılmış *okuyucu* ACR kayıt defterinize izinleri. Bu izinler AKS kümeye izin *çekme* ACR kayıt defterinden görüntüleri. CI/CD işlem sırasında Jenkins uygulama güncelleştirmelerini temel alarak yeni bir kapsayıcı görüntüleri oluşturur ve ardından gereken *anında iletme* bu görüntüleri ACR kayıt defterine. Rolleri ve izinleri ayrılması artık Jenkins ile için hizmet sorumlusu yapılandırma *katkıda bulunan* ACR kayıt defterinize izinleri.

### <a name="create-a-service-principal-for-jenkins-to-use-acr"></a>Jenkins ACR kullanmak için hizmet sorumlusu oluşturma

İlk olarak kullanarak bir hizmet sorumlusu oluşturma [az ad sp create-for-rbac] [ az-ad-sp-create-for-rbac] komutu:

```azurecli
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "626dd8ea-042d-4043-a8df-4ef56273670f",
  "displayName": "azure-cli-2018-09-28-22-19-34",
  "name": "http://azure-cli-2018-09-28-22-19-34",
  "password": "1ceb4df3-c567-4fb6-955e-f95ac9460297",
  "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db48"
}
```

Not *AppID* ve *parola* çıkışınızda gösterilir. Bu değerler, aşağıdaki adımlarda kimlik bilgisi kaynak Jenkins yapılandırmak için kullanılır.

ACR kullanarak kayıt defteri kaynak Kimliğini alın [az acr show] [ az-acr-show] komutu ve bir değişkene depolayın. ACR adı ve kaynak grubu adı girin:

```azurecli
ACR_ID=$(az acr show --resource-group myResourceGroup --name <acrLoginServer> --query "id" --output tsv)
```

Artık hizmet sorumlusu atamak için rol ataması oluşturma *katkıda bulunan* ACR kayıt defteri hakları. Aşağıdaki örnekte, kendi sağlamak *AppID* hizmet sorumlusu oluşturmak için bir önceki komut çıktısında gösterilen:

```azurecli
az role assignment create --assignee 626dd8ea-042d-4043-a8df-4ef56273670f --role Contributor --scope $ACR_ID
```

### <a name="create-a-credential-resource-in-jenkins-for-the-acr-service-principal"></a>Bir kimlik bilgisi kaynak Jenkins için ACR hizmet sorumlusu oluşturma

Rol ataması Azure'da oluşturulan ACR kimlik bilgilerinizi bir Jenkins kimlik bilgisi nesnesi artık depolayın. Bu kimlik bilgileri, Jenkins derleme işi sırasında başvurulur.

Geri sol tarafında Jenkins portalında tıklayın **kimlik bilgilerini** > **Jenkins** > **(sınırsız) genel kimlik bilgileri**  >  **Kimlik bilgilerini ekleyin**

Kimlik bilgisi türü olduğundan emin olun **kullanıcı adıyla parola** ve aşağıdakileri girin:

- **Kullanıcı adı** - *AppID* , ACR kayıt defteri ile kimlik doğrulaması için oluşturduğunuz hizmet sorumlusunun.
- **Parola** - *parola* , ACR kayıt defteri ile kimlik doğrulaması için oluşturduğunuz hizmet sorumlusunun.
- **Kimliği** -tanımlayıcı gibi kimlik bilgisi *acr kimlik bilgileri*

Tamamlandığında, kimlik bilgilerini form aşağıdaki örnekteki gibi görünür:

![Bir Jenkins kimlik bilgisi nesnesi ile hizmet sorumlusu bilgilerini oluşturma](media/aks-jenkins/acr-credentials.png)

Tıklayın **Tamam** ve Jenkins portala geri dönün.

## <a name="create-a-jenkins-project"></a>Jenkins proje oluşturma

Seçin, Jenkins portalı giriş sayfasından **yeni öğe** sol taraftaki:

1. Girin *azure-vote* iş adı olarak. Seçin **Serbest tarzda proje**, ardından **Tamam**
1. Altında **genel** bölümünden **GitHub projesini** ve seçip çatalı oluşturulan deponuzun URL'nizi girin *https:\//github.com/\<your-github-hesabına\>/azure-voting-app-redis*
1. Altında **kaynak kodu Yönetimi** bölümünden **Git**, çatalı oluşturulan deponuzu girin *.git* URL gibi *https:\//github.com/\<your-github-hesabına\>/azure-voting-app-redis.git*

1. Altında **derleme Tetikleyicileri** bölümünden **Gıtscm yoklaması için GitHub kanca tetikleyicisi**
1. Altında **yapı ortamı**seçin **gizli metin veya dosyaları kullanma**
1. Altında **bağlamaları**seçin **Ekle** > **kullanıcı adı ve parola (ayrılmış)**
   - Girin `ACR_ID` için **kullanıcı adı değişkeni**, ve `ACR_PASSWORD` için **parola değişkeni**

     ![Jenkins bağlamaları](media/aks-jenkins/bindings.png)

1. Eklemek için bir **derleme adımı** türü **Kabuğu Yürüt** ve aşağıdaki metni kullanabilirsiniz. Bu betik yeni bir kapsayıcı görüntüsü oluşturur ve bu, ACR kayıt defterine iletir.

    ```bash
    # Build new image and push to ACR.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    docker build -t $WEB_IMAGE_NAME ./azure-vote
    docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
    docker push $WEB_IMAGE_NAME
    ```

1. Başka bir **derleme adımı** türü **Kabuğu Yürüt** ve aşağıdaki metni kullanabilirsiniz. Bu betik uygulaması dağıtımı'nda AKS yeni görüntüyü ACR ile güncelleştirir.

    ```bash
    # Update kubernetes deployment with new image.
    WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
    kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
    ```

1. Tamamlandığında, tıklayın **Kaydet**.

## <a name="test-the-jenkins-build"></a>Jenkins derleme test

GitHub işlemelerine dayalı iş otomatikleştirmeden önce ilk Jenkins derleme el ile test. Bu el ile derleme işi doğru şekilde yapılandırıldı, uygun Kubernetes kimlik doğrulaması dosyası yerinde olduğundan ve kimlik doğrulaması ACR ile çalıştığını doğrular.

Proje sol taraftaki menüden seçin **artık yapı**.

![Jenkins derleme test](media/aks-jenkins/test-build.png)

İki Docker görüntü katmanları olarak Jenkins sunucusuna aşağı çektiğiniz ya da ilk derleme bir dakika sürer. Ardışık derlemeler, derleme sürelerini iyileştirmek için önbelleğe alınan görüntü katmanları kullanabilirsiniz.

Derleme işlemi sırasında Jenkins derleme sunucusu için GitHub deposunu kopyalanmış olan. Yeni bir kapsayıcı görüntüsü oluşturulur ve ACR kayıt defterine gönderildi. Son olarak, yeni görüntüyü kullanarak AKS küme üzerinde çalışan Azure vote uygulaması güncelleştirilir. Uygulama koduna yapılan hiçbir değişiklik olduğundan, uygulama bir web tarayıcısında örnek uygulamasını görüntülerseniz değiştirilmez.

Derleme işi tamamlandıktan sonra tıklayarak **#1 derleme** altında derleme geçmişi. Seçin **konsol çıktısı** ve yapı işleminin çıktılarını görüntüleyin. Son satır başarılı bir derleme belirtmeniz gerekir.

## <a name="create-a-github-webhook"></a>Bir GitHub Web kancası oluştur

Başarılı elle bir yapı ile tam, GitHub ile Jenkins derleme artık tümleştirin. Bir Web kancasını Github'da her kod işlemesinde yapıldığında Jenkins derleme işi çalıştırmak için kullanılabilir. GitHub Web kancası oluşturmak için aşağıdaki adımları tamamlayın:

1. Bir web tarayıcısında çatalı oluşturulan GitHub deponuza göz atın.
1. Seçin **ayarları**, ardından **Web kancaları** sol taraftaki.
1. Tercih **Web kancası Ekle**. İçin *yük URL'si*, girin `http://<publicIp:8080>/github-webhook/`burada `<publicIp>` Jenkins sunucusunun IP adresidir. Sondaki eklediğinizden emin olun /. Diğer varsayılan içerik türü ve tetikleyici üzerinde bırakın *anında iletme* olayları.
1. Seçin **Web kancası Ekle**.

    ![Jenkins için GitHub Web kancası oluştur](media/aks-jenkins/webhook.png)

## <a name="test-the-complete-cicd-pipeline"></a>Eksiksiz bir CI/CD ardışık düzen test

Artık CI/CD işlem hattının tamamını test edebilirsiniz. GitHub için kod tamamlama gönderdiğinizde, aşağıdaki adımları gerçekleşir:

1. GitHub Web kancası Jenkins'e ulaşır.
1. Jenkins derleme işi başlatır ve en son kod tamamlama Github'dan çeker.
1. Docker derleme güncelleştirilmiş kod kullanılarak başlatılır ve en son yapı numarası ile yeni bir kapsayıcı görüntüsü etiketlendi.
1. Bu yeni bir kapsayıcı görüntüsü Azure Container Registry'ye gönderildi.
1. Azure Kubernetes hizmeti güncelleştirmelerle en yeni bir kapsayıcı görüntüsü Azure Container Registry kayıt defterinden dağıtılan uygulamanız.

Geliştirme makinenizde kopyalanan uygulamayı bir kod Düzenleyicisi ile açın. Altında */azure-vote/azure-vote* dosya adlı dizin, açık **config_file.cfg**. Oy bu dosyadaki bir şeye Kediler ve köpekler, dışında aşağıdaki örnekte gösterildiği gibi güncelleştirin:

```
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Güncelleştirildiğinde, dosyayı kaydedin, değişiklikleri ve bu GitHub deposunun çatalınıza gönderin. GitHub Web kancası bir yeni Jenkins derleme işi tetikler. Jenkins web Panoda derleme işlemini izleyin. En son kodu çekin, oluşturun ve güncelleştirilmiş görüntüyü gönderin ve aks'deki güncelleştirilmiş uygulamayı dağıtmak için birkaç saniye sürer.

Derleme tamamlandıktan sonra örnek Azure vote uygulaması, web tarayıcınızı yenileyin. Yaptığınız değişiklikleri aşağıdaki örnekte gösterildiği gibi görüntülenir:

![Örnek Azure oy aks'deki Jenkins derleme işi tarafından güncelleştirildi](media/aks-jenkins/azure-vote-updated.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Jenkins CI/CD çözümün bir parçası kullanmayı öğrendiniz. AKS tümleştirilebilir diğer CI/CD çözümleri ve Otomasyon araçları ile gibi [Azure DevOps projesi] [ azure-devops] veya [ansible'ı ile bir AKS kümesi oluşturma] [ aks-ansible].

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[docker-install]: https://docs.docker.com/install/
[kubectl-install]: https://kubernetes.io/docs/tasks/tools/install-kubectl/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az-acr-list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md#grant-aks-access-to-acr
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az-aks-get-credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli
[install-azure-cli]: /cli/azure/install-azure-cli
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-acr-show]: /cli/azure/acr#az-acr-show
[azure-devops]: ../devops-project/azure-devops-project-aks.md
[aks-ansible]: ../ansible/ansible-create-configure-aks.md
