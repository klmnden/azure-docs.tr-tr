---
title: Azure Kubernetes Service'teki Kubernetes ile Jenkins sürekli dağıtım
description: Nasıl bir Azure Kubernetes Service'teki kubernetes, kapsayıcılı bir uygulama dağıtın ve Jenkins ile sürekli dağıtım sürecini otomatikleştirin
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 246943b7e3df955394a6a79f9b3130633fe4ec50
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39186622"
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Jenkins ve Azure Kubernetes hizmeti ile sürekli dağıtım

Bu belgede, Jenkins ve Azure Kubernetes Service (AKS) kümesi arasında basit sürekli dağıtım iş akışı ayarlama gösterilmiştir.

Örnek iş akışı, aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Kubernetes kümenize Azure vote uygulaması dağıtın.
> * Azure vote uygulaması kodunu güncelleştirme ve sürekli dağıtım işlemi başlatır ve GitHub deposu için gönderin.
> * Jenkins, depo klonlar ve güncelleştirilmiş kod ile yeni bir kapsayıcı görüntüsü oluşturur.
> * Bu görüntü Azure Container Registry (ACR) gönderilir.
> * AKS kümesinde çalışan uygulama yeni bir kapsayıcı görüntüsü ile güncelleştirilir.

## <a name="prerequisites"></a>Önkoşullar

Bu makaledeki adımları tamamlamak için aşağıdakiler gerekir.

- Kubernetes, Git, CI/CD ve Azure Container Registry (ACR) ilgili temel bilgilere.
- Bir [Azure Kubernetes Service (AKS) kümesini] [ aks-quickstart] ve [AKS kimlik bilgileriyle] [ aks-credentials] geliştirme sisteminizde.
- Bir [Azure Container Registry (ACR) kayıt defteri][acr-quickstart], ACR oturum açma sunucusu adını ve [ACR kimlik bilgilerini] [ acr-authentication] gönderme ve çekme erişim.
- Azure CLI geliştirme sisteminizde yüklü.
- Docker geliştirme sisteminizde yüklü.
- GitHub hesabı [GitHub kişisel erişim belirteci][git-access-token]ve Git istemci geliştirme sisteminizde yüklü.

## <a name="prepare-application"></a>Uygulama hazırlama

Bu belge boyunca kullanılan Azure vote uygulaması bir veya daha fazla pod'ların ve geçici veri depolama için Redis barındıran ikinci bir pod içinde barındırılan bir web arabirimi içerir.

Jenkins derlemeden önce / AKS tümleştirme, hazırlama ve AKS kümenizi Azure vote uygulamayı dağıtın. Bunu, sürüm bir uygulama düşünün.

Aşağıdaki GitHub depo çatalı oluşturma.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Çatal oluşturulduktan sonra geliştirme sisteminizde klonlayın. Bu depoyu kopyalarken çatalınızın URL'sini kullandığınızdan olun.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Kopyalanan dizinden çalışabilmeniz için dizinleri değiştirin.

```bash
cd azure-voting-app-redis
```

Çalıştırma `docker-compose.yaml` oluşturmak için dosya `azure-vote-front` kapsayıcı görüntüsü ve başlangıç uygulaması.

```bash
docker-compose up -d
```

Tamamlandığında, kullanın [docker görüntüleri] [ docker-images] oluşturulan görüntüyü görmek için komutu.

İndirilen veya oluşturulan üç görüntü olduğunu göz önünde bulundurun. `azure-vote-front` görüntüsü uygulamayı içerir ve temel olarak `nginx-flask` görüntüsünü kullanır. `redis` görüntüsü bir Redis örneği başlatmak için kullanılır.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

ACR oturum açma sunucusuyla alma [az acr listesi] [ az-acr-list] komutu. Kaynak grubu adı ile kaynak grubu, ACR kayıt defterini barındıran güncelleştirdiğinizden emin olun.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanım [docker tag] [ docker-tag] oturum açma sunucusu adını ve sürüm numarası ile görüntüyü etiketlemek için komut `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

ACR oturum açma sunucusu değerini, ACR oturum açma sunucusu adını ve anında iletme ile güncelleştirmek `azure-vote-front` görüntüsünü kayıt defterine.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Kubernetes uygulamasını dağıtma

Bir Kubernetes bildirim dosyası olabilir, Azure vote deponun kök dizininde bulunan ve uygulama Kubernetes kümenize dağıtmak için kullanılabilir.

İlk olarak, güncelleştirme **azure-vote-all-in-one-redis.yaml** bildirim dosyası, ACR kayıt defterinizin konum. Dosyayı herhangi bir metin düzenleyicisi ile açın ve Değiştir `microsoft` ACR oturum açma sunucusu adıyla. Bu değer, bildirim dosyasının **47**. satırında bulunur.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Ardından, [kubectl uygulamak] [ kubectl-apply] uygulamayı çalıştırmak için komutu. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur.

```bash
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

A [Kubernetes hizmeti] [ kubernetes-service] uygulamayı İnternette kullanıma sunmak için oluşturulur. Bu işlem birkaç dakika sürebilir.

İlerleme durumunu izlemek için [kubectl get service][kubectl-get] komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```bash
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.

```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

*EXTERNAL-IP* adresi *pending* durumundan *IP address* değerine değiştiğinde kubectl izleme işlemini durdurmak için `control+c` komutunu kullanın.

```
azure-vote-front   10.0.34.242   13.90.150.118   80:30676/TCP   2m
```

Uygulamayı görmek için dış IP adresine gözatın.

![Azure’da Kubernetes kümesinin görüntüsü](media/aks-jenkins/azure-vote-safari.png)

## <a name="deploy-jenkins-to-vm"></a>Jenkins VM dağıtma

Bir betik, bir sanal makine dağıtma, ağ erişimi yapılandırma ve temel bir Jenkins yüklemesini tamamlamak için önceden oluşturulmuştur. Ayrıca, betik, Kubernetes yapılandırma dosyanızı geliştirme sisteminizden Jenkins sistemine kopyalar. Bu dosya, Jenkins ve AKS kümesi arasında kimlik doğrulaması için kullanılır.

Bir betik indirip çalıştırmak için aşağıdaki komutları çalıştırın. Aşağıdaki URL'yi komut dosyasının içeriğini gözden geçirmek için de kullanılabilir.

> [!WARNING]
> Bu örnek betik bir Azure VM üzerinde çalışan bir Jenkins ortamı hızlıca sağlamak Tanıtım amaçlı var. Bir VM yapılandırın ve ardından gerekli kimlik bilgilerini görüntülemek için Azure özel betik uzantısı kullanır. *~/.Kube/config* Jenkins VM'ye kopyalanır.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Betik tamamlandığında, Jenkins sunucusu için bir adres Jenkins kilidini açmak için iyi bir anahtar olarak çıkarır. Aşağıdaki URL'ye gidin ve anahtarı girin Jenkins yapılandırmasını tamamlamak için ekrandaki ister.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Jenkins ortam değişkenleri

Jenkins ortam değişkeni, Azure Container Registry (ACR) oturum açma sunucusu adını tutmak için kullanılır. Bu değişken, Jenkins sürekli dağıtım işi sırasında başvurulur.

Jenkins Yönetici portalında sırada, tıklayarak **Jenkins'i Yönet** > **yapılandırma sistemi**.

Altında **genel özellikleri**seçin **ortam değişkenlerini**ve ada sahip bir değişken ekleyin `ACR_LOGINSERVER` ve değerini, ACR oturum açma sunucusu.

![Jenkins ortam değişkenleri](media/aks-jenkins/env-variables.png)

Tamamlandığında, tıklayın **Kaydet** Jenkins yapılandırma sayfasında.

## <a name="jenkins-credentials"></a>Jenkins kimlik bilgileri

Artık ACR kimlik bilgilerinizi bir Jenkins kimlik bilgisi nesnesini saklar. Bu kimlik bilgileri, Jenkins derleme işi sırasında başvurulur.

Jenkins Yönetici portalı'üzerinde geri tıklayın **kimlik bilgilerini** > **Jenkins** > **(sınırsız) genel kimlik bilgileri**  >   **Kimlik bilgileri ekleme**.

Kimlik bilgisi türü olduğundan emin olun **kullanıcı adıyla parola** ve aşağıdakileri girin:

- **Kullanıcı adı** -kimliği, ACR kayıt defteri ile kimlik doğrulaması için hizmet sorumlusu kullanın.
- **Parola** -hizmet sorumlusu kullanımı, ACR kayıt defteri ile kimlik doğrulaması için istemci gizli anahtarı.
- **Kimliği** -tanımlayıcı gibi kimlik bilgisi `acr-credentials`.

Tamamlandığında, kimlik bilgilerini formun Bu resme benzer görünmelidir:

![ACR kimlik bilgileri](media/aks-jenkins/acr-credentials.png)

Tıklayın **Tamam** ve Jenkins yönetim portalına dönün.

## <a name="create-jenkins-project"></a>Jenkins projesi oluşturma

Jenkins yönetici portalından tıklayın **yeni öğe**.

Proje, örneğin, bir ad verin `azure-vote`seçin **Freestyle proje**, tıklatıp **Tamam**.

![Jenkins proje](media/aks-jenkins/jenkins-project.png)

Altında **genel**seçin **GitHub projesini** çatalınıza Azure vote GitHub proje URL'sini girin.

![GitHub proje](media/aks-jenkins/github-project.png)

Altında **kaynak kodu Yönetimi**seçin **Git**, çatalınıza Azure Vote GitHub deposunun URL'sini girin.

Kimlik bilgileri için tıklayın ve **Ekle** > **Jenkins**. Altında **tür**seçin **gizli metin** girin, [GitHub kişisel erişim belirteci] [ git-access-token] gizli dizi olarak.

Seçin **Ekle** işiniz bittiğinde.

![GitHub kimlik](media/aks-jenkins/github-creds.png)

Altında **derleme Tetikleyicileri**seçin **Gıtscm yoklaması için GitHub kanca tetikleyicisi**.

![Jenkins derleme Tetikleyicileri](media/aks-jenkins/build-triggers.png)

Altında **yapı ortamı**seçin **gizli metin veya dosyaları kullanmanız**.

![Jenkins derleme ortamı](media/aks-jenkins/build-environment.png)

Altında **bağlamaları**seçin **Ekle** > **kullanıcı adı ve parola (ayrılmış)**.

Girin `ACR_ID` için **kullanıcı adı değişkeni**, ve `ACR_PASSWORD` için **parola değişkenini**.

![Jenkins bağlamaları](media/aks-jenkins/bindings.png)

Ekleme bir **derleme adımı** türü **Kabuğu Yürüt** ve aşağıdaki metni kullanabilirsiniz. Bu betik yeni bir kapsayıcı görüntüsü oluşturur ve bu, ACR kayıt defterine iletir.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Başka bir **derleme adımı** türü **Kabuğu Yürüt** ve aşağıdaki metni kullanabilirsiniz. Bu betik, Kubernetes dağıtımı güncelleştirir.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Tamamlandığında, tıklayın **Kaydet**.

## <a name="test-the-jenkins-build"></a>Jenkins derleme test

Devam etmeden önce Jenkins derleme test edin. Bu derleme işi doğru şekilde yapılandırıldı, uygun Kubernetes kimlik doğrulaması dosyası yerinde olduğundan ve uygun ACR kimlik bilgilerini sağladığınızdan emin doğrular.

Tıklayın **artık yapı** soldaki menüden proje.

![Jenkins derleme test](media/aks-jenkins/test-build.png)

Bu işlem sırasında GitHub deposunu Jenkins derleme sunucusuna kopyalanmış olan. Yeni bir kapsayıcı görüntüsü oluşturulur ve ACR kayıt defterine gönderildi. Son olarak, yeni görüntüyü kullanarak AKS küme üzerinde çalışan Azure vote uygulaması güncelleştirilir. Uygulama koduna yapılan herhangi bir değişiklik olmadığından uygulama değiştirilmez.

İşlem tamamlandıktan sonra tıklayarak **#1 derleme** altında geçmişi oluşturun ve seçin **konsol çıktısı** yapı işleminin tüm çıktısını görmek için. Son satır başarılı bir derleme belirtmeniz gerekir.

## <a name="create-github-webhook"></a>GitHub web kancası oluşturma

Ardından, herhangi bir kaydetme üzerinde yeni bir derleme tetiklenmesi uygulama deposu Jenkins derleme sunucusuna bağlayın.

1. Çatalı oluşturulan GitHub deposuna göz atın.
2. Seçin **ayarları**, ardından **Web kancaları** sol taraftaki.
3. Tercih **Web kancası Ekle**. İçin *yük URL'si*, girin `http://<publicIp:8080>/github-webhook/` burada `publicIp` Jenkins sunucusunun IP adresidir. Sondaki eklediğinizden emin olun /. Diğer varsayılan içerik türü ve tetikleyici üzerinde bırakın *anında iletme* olayları.
4. Seçin **Web kancası Ekle**.

    ![GitHub web kancası](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>CI/CD işlem baştan sona test edin

Geliştirme makinenizde kopyalanan uygulamayı bir kod Düzenleyicisi ile açın.

Altında **/azure-vote/azure-vote** dizin Bul'adlı dosyayı **config_file.cfg**. Bu dosyadaki oy değerleri bir şeye Kediler ve köpekler dışında güncelleştirin.

Aşağıdaki örnek, gösterir ve güncelleştirilmiş **config_file.cfg** dosya.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Tamamlandığında, dosyayı kaydetmek, değişiklikleri ve bu GitHub deposunun çatalınıza gönderin... İşleme tamamlandığında, GitHub Web kancası kapsayıcı görüntüsünü ve AKS dağıtımı güncelleştirmeleri yeni bir Jenkins derleme tetikler. Jenkins yönetici konsolundaki derleme işlemini izleyin.

Yeniden derleme tamamlandıktan sonra değişiklikleri gözlemlemek için uygulama uç noktası için göz atın.

![Azure vote güncelleştirildi](media/aks-jenkins/azure-vote-updated-safari.png)

Bu noktada, bir basit sürekli dağıtım işlemi tamamlandı. Bu örnekte gösterilen yapılandırmaları ve adımları daha sağlam ve üretime hazır bir sürekli derleme otomasyonu oluşturmak için kullanılabilir.

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[git-access-token]: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/

<!-- LINKS - internal -->
[az-acr-list]: /cli/azure/acr#az_acr_list
[acr-authentication]: ../container-registry/container-registry-auth-aks.md
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-credentials]: /cli/azure/aks#az_aks_get_credentials
[aks-quickstart]: kubernetes-walkthrough.md
[azure-cli-install]: /cli/azure/install-azure-cli