---
title: Jenkins sürekli dağıtımı Azure Kubernetes hizmetindeki Kubernetes ile
description: Jenkins dağıtmak ve Azure Kubernetes hizmetindeki Kubernetes kapsayıcılı bir uygulamasını yükseltmek için sürekli dağıtım işlemine otomatikleştirme
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 03/26/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 376d3b916c4e01ea6111e6c1db63e976dd1ea320
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="continuous-deployment-with-jenkins-and-azure-kubernetes-service"></a>Jenkins ve Azure Kubernetes hizmetiyle sürekli dağıtım

Bu belge, temel sürekli dağıtımı iş akışı Jenkins ve Azure Kubernetes hizmet (AKS) kümesini arasında kurmak gösterilmiştir.

Örnek iş akışı aşağıdaki adımları içerir:

> [!div class="checklist"]
> * Kubernetes kümenize Azure oy uygulamayı dağıtın.
> * Sürekli dağıtım işlemi başlatan bir GitHub deposuna, Gönder ve Azure oy uygulama kodunu güncelleştirin.
> * Jenkins depo klonlar ve güncelleştirilmiş kod ile yeni bir kapsayıcı görüntüsü oluşturur.
> * Bu görüntü, Azure kapsayıcı kayıt defteri (ACR) gönderilir.
> * AKS kümede çalışan uygulama ile yeni bir kapsayıcı imaj güncelleştirilir.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki öğeler, bu makaledeki adımları tamamlamak için gerekir.

- Temel bilgilere Kubernetes, Git, CI/CD ve Azure kapsayıcı kayıt defteri (ACR).
- Bir [Azure Kubernetes hizmet (AKS) küme] [ aks-quickstart] ve [AKS kimlik bilgileriyle] [ aks-credentials] geliştirme sisteminizde.
- Bir [Azure kapsayıcı kayıt defteri (ACR) kayıt defteri][acr-quickstart], ACR oturum açma sunucusu adı ve [ACR kimlik] [ acr-authentication] anında iletme ve çekme erişim.
- Azure CLI geliştirme sisteminizde yüklü.
- Geliştirme sisteminizde docker.
- GitHub hesabı [GitHub kişisel erişim belirteci][git-access-token]ve Git istemci geliştirme sisteminizde yüklü.

## <a name="prepare-application"></a>Uygulama hazırlama

Bu belge boyunca kullanılan Azure oy uygulama bir veya daha fazla pod'ları ve geçici veri depolama için Redis barındırma ikinci bir pod barındırılan bir web arabirimi içerir.

Jenkins derleme önce / AKS tümleştirmesi, hazırlamak ve AKS kümenizi Azure oy uygulamayı dağıtın. Bu uygulama biri sürümü düşünün.

Aşağıdaki GitHub depoyu çatallaştırmanız.

```
https://github.com/Azure-Samples/azure-voting-app-redis
```

Çatalı oluşturulduktan sonra geliştirme sisteminizde klonlayın. Bu depodaki kopyalarken, çatalı URL'sini kullanıyorsanız emin olun.

```bash
git clone https://github.com/<your-github-account>/azure-voting-app-redis.git
```

Kopyalanan dizinden çalışabilmeniz için dizinleri değiştirin.

```bash
cd azure-voting-app-redis
```

Çalıştırma `docker-compose.yaml` oluşturmak için dosya `azure-vote-front` kapsayıcı görüntü ve başlangıç uygulaması.

```bash
docker-compose up -d
```

Tamamlandığında kullanmak [docker görüntüleri] [ docker-images] oluşturulan görüntü görmek için komutu.

İndirilen veya oluşturulan üç görüntü olduğunu göz önünde bulundurun. `azure-vote-front` görüntüsü uygulamayı içerir ve temel olarak `nginx-flask` görüntüsünü kullanır. `redis` görüntüsü bir Redis örneği başlatmak için kullanılır.

```console
$ docker images

REPOSITORY                   TAG        IMAGE ID            CREATED             SIZE
azure-vote-front             latest     9cc914e25834        40 seconds ago      694MB
redis                        latest     a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask      788ca94b2313        9 months ago        694MB
```

ACR oturum açma sunucusu ile [az acr listesi] [ az-acr-list] komutu. Kaynak grubu adı ACR kaydınız barındırma kaynak grubu ile güncelleştirdiğinizden emin olun.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanım [docker etiketi] [ docker-tag] oturum açma sunucusu adı ve sürüm numarası olan görüntü etiketi için komutu `v1`.

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

ACR oturum açma sunucusu değeri, ACR oturum açma sunucusu adı ve anında iletme ile güncelleştirme `azure-vote-front` kayıt defterine görüntü.

```bash
docker push <acrLoginServer>/azure-vote-front:v1
```

## <a name="deploy-application-to-kubernetes"></a>Kubernetes uygulamayı dağıtma

Bildirim dosyası olabilir Kubernetes Azure oy depo kök dizininde bulunan ve Kubernetes kümenizi uygulamayı dağıtmak için kullanılır.

İlk olarak, güncelleştirme **azure-vote-all-in-one-redis.yaml** ACR kayıt defteri konumu ile bildirim dosyası. Dosya ile herhangi bir metin düzenleyicisinde açın ve değiştirin `microsoft` ACR oturum açma sunucu adına sahip. Bu değer, bildirim dosyasının **47**. satırında bulunur.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Ardından, kullanın [kubectl uygulamak] [ kubectl-apply] uygulamayı çalıştırmak için komutu. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur.

```bash
kubectl apply -f azure-vote-all-in-one-redis.yaml
```

A [Kubernetes hizmet] [ kubernetes-service] internet uygulamasını kullanıma sunmak için oluşturulur. Bu işlem birkaç dakika sürebilir.

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

Bir komut dosyası, bir sanal makineyi dağıtmak için ağ erişimi yapılandırıp Jenkins temel yüklemesini tamamlamak için önceden oluşturulmuş olmuştur. Ayrıca, komut dosyası, Kubernetes yapılandırma dosyanızı geliştirme sisteminizden Jenkins sisteme kopyalar. Bu dosya Jenkins AKS küme arasındaki kimlik doğrulaması için kullanılır.

Karşıdan yüklemek ve komut dosyasını çalıştırmak için aşağıdaki komutları çalıştırın. URL komut dosyasının içeriğini gözden geçirmek için de kullanılabilir.

```console
curl https://raw.githubusercontent.com/Azure-Samples/azure-voting-app-redis/master/jenkins-tutorial/deploy-jenkins-vm.sh > azure-jenkins.sh
sh azure-jenkins.sh
```

Komut tamamlandığında, Jenkins sunucu için bir adres Jenkins kilidini açmak için iyi bir anahtar olarak çıkarır. URL'ye gidin, anahtarı girin ve ardından Jenkins yapılandırmasını tamamlamak için ekrandaki ister.

```console
Open a browser to http://52.166.118.64:8080
Enter the following to Unlock Jenkins:
667e24bba78f4de6b51d330ad89ec6c6
```

## <a name="jenkins-environment-variables"></a>Jenkins ortam değişkenleri

Jenkins ortam değişkeni, Azure kapsayıcı kayıt defteri (ACR) oturum açma sunucusu adını tutmak için kullanılır. Bu değişken Jenkins sürekli dağıtımı sırasında başvurulur.

Jenkins Yönetici portalı olduğu sırada, tıklayın **yönetmek Jenkins** > **yapılandırma sistem**.

Altında **genel özellikleri**seçin **ortam değişkenleri**ve ada sahip bir değişken Ekle `ACR_LOGINSERVER` ve ACR oturum açma sunucunuzun bir değer.

![Jenkins ortam değişkenleri](media/aks-jenkins/env-variables.png)

Tamamlandığında, tıklayın **kaydetmek** Jenkins yapılandırma sayfasında.

## <a name="jenkins-credentials"></a>Jenkins kimlik bilgileri

Şimdi ACR kimlik bilgilerinizi Jenkins kimlik bilgisi nesnesinde depolar. Bu kimlik bilgileri Jenkins derleme işi sırasında başvurulur.

Jenkins Yönetim Portalı'üzerinde geri tıklatın **kimlik bilgileri** > **Jenkins** > **(sınırsız) genel kimlik bilgileri**  >   **Kimlik bilgileri Ekle**.

Kimlik bilgisi türü olduğundan emin olun **kullanıcı adı parola ile** ve aşağıdakileri girin:

- **Kullanıcı adı** -ACR kaydınız kimlik doğrulaması için hizmet asıl kullanımı kimliği.
- **Parola** -gizli ACR kaydınız kimlik doğrulaması için hizmet asıl kullanımı.
- **Kimliği** -tanımlayıcı gibi kimlik bilgisi `acr-credentials`.

Tamamlandığında, kimlik bilgileri form bu görüntüsüne benzer görünmelidir:

![ACR kimlik bilgileri](media/aks-jenkins/acr-credentials.png)

Tıklatın **Tamam** ve Jenkins Yönetici portalına geri dönün.

## <a name="create-jenkins-project"></a>Jenkins projesi oluşturma

Jenkins Yönetim Portalı'ndan tıklatın **yeni öğe**.

Proje Örneğin, bir ad verin `azure-vote`seçin **Freestyle proje**, tıklatıp **Tamam**.

![Jenkins proje](media/aks-jenkins/jenkins-project.png)

Altında **genel**seçin **GitHub proje** ve Azure oy GitHub proje sayfanızı çatala URL'yi girin.

![GitHub proje](media/aks-jenkins/github-project.png)

Altında **kaynak kodu Yönetimi**seçin **Git**, Azure oy GitHub depo sayfanızı çatala URL'sini girin.

Kimlik bilgilerini tıklayın ve **Ekle** > **Jenkins**. Altında **türü**seçin **gizli metin** ve girin, [GitHub kişisel erişim belirteci] [ git-access-token] gizli olarak.

Seçin **Ekle** bittiğinde.

![GitHub kimlik bilgileri](media/aks-jenkins/github-creds.png)

Altında **yapı tetikleyicileri**seçin **GITScm yoklama için GitHub kanca tetikleyici**.

![Jenkins Tetikleyicileri oluşturma](media/aks-jenkins/build-triggers.png)

Altında **yapı ortamı**seçin **gizli metni veya dosyaları kullanmanız**.

![Jenkins yapı ortamı](media/aks-jenkins/build-environment.png)

Altında **bağlamaları**seçin **Ekle** > **kullanıcı adı ve parolası (ayrılmış)**.

Girin `ACR_ID` için **kullanıcı adı değişkeni**, ve `ACR_PASSWORD` için **parola değişkenini**.

![Jenkins bağlamaları](media/aks-jenkins/bindings.png)

Ekleme bir **derleme adımı** türü **Kabuk yürütme** ve aşağıdaki metni kullanın. Bu komut dosyasını yeni bir kapsayıcı görüntüsü oluşturur ve ACR kaydınız iter.

```bash
# Build new image and push to ACR.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
docker build -t $WEB_IMAGE_NAME ./azure-vote
docker login ${ACR_LOGINSERVER} -u ${ACR_ID} -p ${ACR_PASSWORD}
docker push $WEB_IMAGE_NAME
```

Başka bir tane eklemek **derleme adımı** türü **Kabuk yürütme** ve aşağıdaki metni kullanın. Bu komut dosyası Kubernetes dağıtım güncelleştirir.

```bash
# Update kubernetes deployment with new image.
WEB_IMAGE_NAME="${ACR_LOGINSERVER}/azure-vote-front:kube${BUILD_NUMBER}"
kubectl set image deployment/azure-vote-front azure-vote-front=$WEB_IMAGE_NAME --kubeconfig /var/lib/jenkins/config
```

Tamamlandığında, tıklatın **kaydetmek**.

## <a name="test-the-jenkins-build"></a>Jenkins derleme test

Devam etmeden önce Jenkins yapı sınayın. Bu derleme işi yapılandırıldığını, uygun Kubernetes kimlik doğrulama dosyasını yerinde olduğundan ve doğru ACR kimlik bilgilerini sağladığınızdan emin doğrular.

Tıklatın **şimdi yapı** projenin sol menüde.

![Yapı Jenkins test](media/aks-jenkins/test-build.png)

Bu işlem sırasında GitHub deposuna Jenkins yapı sunucusuna kopyalanması. Yeni bir kapsayıcı görüntüsü oluşturulur ve ACR kayıt defterine gönderilir. Son olarak, yeni görüntüyü kullanarak AKS küme üzerinde çalışan Azure oy uygulama güncelleştirilir. Uygulama kodu herhangi bir değişiklik yapılmıştır çünkü uygulama değiştirilmez.

İşlem tamamlandıktan sonra tıklayın **#1 yapı** altında geçmişi yapı ve seçin **konsol çıktısı** yapı işlemi gelen tüm çıkış görmek için. Son satırı başarılı bir derlemede belirtmeniz gerekir.

## <a name="create-github-webhook"></a>GitHub web kancası oluşturma

Ardından, böylece tüm yürütme üzerinde yeni bir yapı tetiklenir Jenkins yapı sunucusuna uygulama havuzu bağlayın.

1. Forked GitHub deposuna göz atın.
2. **Ayarlar**’ı seçip sol taraftan **Tümleştirmeler ve hizmetler**’i seçin.
3. Seçin **Hizmet Ekle**, girin `Jenkins (GitHub plugin)` filtre kutusuna ve eklenti seçin.
4. URL Jenkins kanca için girin `http://<publicIp:8080>/github-webhook/` nerede `publicIp` Jenkins sunucunun IP adresidir. Sondaki eklediğinizden emin olun /.
5. Ekle hizmeti seçin.

![GitHub web kancası](media/aks-jenkins/webhook.png)

## <a name="test-cicd-process-end-to-end"></a>CI/CD işlemi uçtan uca test edin

Geliştirme makinenizde kopyalanan uygulama ile bir kod düzenleyicisini açın.

Altında **/azure-vote/azure-vote** dizin, bir dosya adındaki bulun **config_file.cfg**. Bu dosyadaki oy değerleri için bir şeyler kediler ve köpekler dışında güncelleştirin.

Aşağıdaki örnek gösterir ve güncelleştirilmiş **config_file.cfg** dosya.

```bash
# UI Configurations
TITLE = 'Azure Voting App'
VOTE1VALUE = 'Blue'
VOTE2VALUE = 'Purple'
SHOWHOST = 'false'
```

Tamamlandığında, dosyayı kaydedin, değişiklikleri ve bunları sizin çatalı GitHub deposunun itme... Yürütme tamamlandıktan sonra GitHub Web kancası kapsayıcı görüntü ve AKS dağıtım güncelleştirmeleri yeni bir Jenkins yapı tetikler. Jenkins Yönetici konsolunda yapı işlemini izleyin.

Yapı tamamlandıktan sonra yeniden değişiklikleri izlemek için uygulama uç noktasına göz atın.

![Güncelleştirilmiş azure oy](media/aks-jenkins/azure-vote-updated-safari.png)

Bu noktada, bir basit sürekli dağıtım işlemi tamamlandı. Bu örnekte gösterilen yapılandırmaları ve adımları, daha sağlam ve üretime hazır sürekli yapı Otomasyon oluşturmak için kullanılabilir.

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