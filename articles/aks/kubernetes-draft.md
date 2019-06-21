---
title: Azure Kubernetes Service'i (AKS) ile taslak üzerinde geliştirme
description: AKS ve Azure Container Registry ile taslak kullanma
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 06/20/2019
ms.author: zarhoads
ms.openlocfilehash: bd099b9d76e17eda36be1650ef5081e5aaa7e53a
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67303537"
---
# <a name="quickstart-develop-on-azure-kubernetes-service-aks-with-draft"></a>Hızlı Başlangıç: Azure Kubernetes Service'i (AKS) ile taslak üzerinde geliştirme

Taslak, paket yardımcı olur ve bir Kubernetes kümesinde uygulama kapsayıcıları çalıştırmak bir açık kaynak araçtır. Değişikliklerinizi sürüm denetimine işlemek zorunda kalmadan kod değişiklikleri ortaya çıktıkları taslak ile hızlı bir şekilde Kubernetes uygulamaya yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için bkz. [taslak belgeleri github'da][draft-documentation].

Bu makalede AKS'de bir uygulama çalıştırma ve taslak paket nasıl kullanılacağı gösterilmektedir.


## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz hesap](https://azure.microsoft.com/free) oluşturabilirsiniz.
* [Yüklü Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).
* Docker, yüklenir ve yapılandırılır. Docker üzerinde Docker'ı yapılandıran paketler sağlar bir [Mac][docker-for-mac], [Windows][docker-for-windows], veya [Linux][linux için docker] sistem.
* [Yüklü helm](https://github.com/helm/helm/blob/master/docs/install.md).
* [Taslak yüklü][draft-documentation].

## <a name="create-an-azure-kubernetes-service-cluster"></a>Azure Kubernetes Service kümesi oluşturma

AKS kümesi oluşturma. Aşağıdaki komutları MyResourceGroup ve MyAKS adlı bir AKS kümesi adlı bir kaynak grubu oluşturun.

```azurecli
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --generate-ssh-keys
```

## <a name="create-an-azure-container-registry"></a>Azure Container Registry oluşturma
AKS kümenizde uygulamanızı çalıştırmak için taslak kullanmak için bir Azure Container Registry, kapsayıcı görüntülerinizi depolamak için gerekir. Örnekte [az ACT create][az-acr-create] adlı bir ACR oluşturma *MyDraftACR* içinde *MyResourceGroup* kaynak grubuyla *temel* SKU. Kendi benzersiz kayıt defteri adı sağlamanız gerekir. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. *Temel* SKU, geliştirme amaçlı dağıtımlar için uygun maliyetli, depolama ve aktarım hızı açısından dengeli bir giriş noktasıdır.

```azurecli
az acr create --resource-group MyResourceGroup --name MyDraftACR --sku Basic
```

Çıktı aşağıdaki örneğe benzerdir. Not *loginServer* daha sonraki bir adımda kullanılacağından, ACR için değer. İçinde aşağıdaki örnekte, *mydraftacr.azurecr.io* olduğu *loginServer* için *MyDraftACR*.

```console
{
  "adminUserEnabled": false,
  "creationDate": "2019-06-11T13:35:17.998425+00:00",
  "id": "/subscriptions/<ID>/resourceGroups/MyResourceGroup/providers/Microsoft.ContainerRegistry/registries/MyDraftACR",
  "location": "eastus",
  "loginServer": "mydraftacr.azurecr.io",
  "name": "MyDraftACR",
  "networkRuleSet": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "MyResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```


ACR örneğini kullanması taslak için önce oturum açmalısınız. Kullanım [az acr login][az-acr-login] oturum açmak için komutu. Aşağıdaki örnekte adlı bir ACR oturum açacak *MyDraftACR*.

```azurecli
az acr login --name MyDraftACR
```

Komut tamamlandığında bir *Oturum Başarıyla Açıldı* iletisi döndürür.

## <a name="create-trust-between-aks-cluster-and-acr"></a>AKS kümesi ve ACR arasında güven oluşturma

AKS kümenizi, ayrıca kapsayıcı görüntülerini çekerken ve bunların, ACR erişmesi gerekir. Erişim ACR'ye AKS güven oluşturarak sağlar. Bir AKS kümesi ve ACR kayıt arasında güven oluşturmak için ACR kayıt defterine erişim için AKS kümesi tarafından kullanılan Azure Active Directory Hizmet sorumlusu için izinler verir. Aşağıdaki komutlar için hizmet sorumlusu izinleri *MyAKS* içinde küme *MyResourceGroup* için *MyDraftACR* , ACR  *MyResourceGroup*.

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group MyResourceGroup --name MyAKS --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group MyResourceGroup --name MyDraftACR --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

## <a name="connect-to-your-aks-cluster"></a>AKS kümenize bağlanın

Yerel bilgisayarınızdan Kubernetes kümesine bağlanmak için kullandığınız [kubectl][kubectl], Kubernetes komut satırı istemcisi.

Azure Cloud Shell'i kullanıyorsanız `kubectl` zaten yüklüdür. [az aks install-cli][] komutunu kullanarak da yerel ortama yükleyebilirsiniz:

```azurecli
az aks install-cli
```

Yapılandırmak için `kubectl` Kubernetes kümenize bağlanmak için [az aks get-credentials][] komutu. Aşağıdaki örnekte adlı AKS kümesi için kimlik bilgilerini alır *MyAKS* içinde *MyResourceGroup*:

```azurecli
az aks get-credentials --resource-group MyResourceGroup --name MyAKS
```

## <a name="create-a-service-account-for-helm"></a>Bir hizmet hesabı için Helm oluşturun

Bir hizmet hesabı ve rol bağlama bir RBAC özellikli AKS kümesinde Helm dağıtabilmeniz için önce Tiller hizmeti için gereklidir. Helm güvenliğini sağlama konusunda daha fazla bilgi için / Tiller bir RBAC, etkin küme, bkz: [Tiller, ad alanları ve RBAC][tiller-rbac]. AKS kümenizi RBAC etkin değilse, bu adımı atlayın.

Adlı bir dosya oluşturun `helm-rbac.yaml` aşağıdaki YAML'ye kopyalayın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Hizmet hesabı oluşturup rolü bağlamayla `kubectl apply` komutu:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="configure-helm"></a>Helm yapılandırın
Temel Tiller bir AKS kümesi dağıtmayı kullanın [helm init][helm-init] komutu. Kümenizi RBAC etkin değilse, kaldırma `--service-account` bağımsız değişkeni ve değer.

```console
helm init --service-account tiller --node-selectors "beta.kubernetes.io/os"="linux"
```

## <a name="configure-draft"></a>Draft'ı yapılandırma

Yerel makinenizde taslak yapılandırmadıysanız, çalıştırma `draft init`:

```console
$ draft init
Installing default plugins...
Installation of default plugins complete
Installing default pack repositories...
...
Happy Sailing!
```

Ayrıca taslak kullanmak için yapılandırmanız gereken *loginServer* , ACR biri. Aşağıdaki komutu kullanır `draft config set` kullanılacak `mydraftacr.azurecr.io` bir kayıt olarak.

```console
draft config set registry mydraftacr.azurecr.io
```

Taslak, ACR kullanmak için yapılandırdıysanız ve taslak, ACR için kapsayıcı görüntülerini gönderebilirsiniz. Taslak AKS kümenizin uygulamanız çalışırken, herhangi bir parola veya gizli dizileri gönderin veya ACR kayıt defterinden çekmek için gerekli değildir. AKS kümenizi, ACR arasında bir güven oluşturulduktan sonra kimlik doğrulaması Azure Active Directory'yi kullanarak Azure Resource Manager düzeyinde gerçekleşir.

## <a name="download-the-sample-application"></a>Örnek uygulamayı indirin:

Bu hızlı başlangıçta kullanılmaktadır [taslak GitHub deposundan örnek bir java uygulama][example-java]. Uygulamasını github'dan kopyalayın ve gidin `draft/examples/example-java/` dizin.

```console
git clone https://github.com/Azure/draft
cd draft/examples/example-java/
```

## <a name="run-the-sample-application-with-draft"></a>Draft ile örnek uygulamayı çalıştırma

Kullanım `draft create` uygulama hazırlamak için komutu.

```console
draft create
```

Bu komut, bir Kubernetes kümesinde uygulama çalıştırmak için kullanılan yapıtları oluşturur. Bu öğeler bir Dockerfile, Helm grafiği içerir ve bir *draft.toml* taslak yapılandırma dosyası olan bir dosya.

```console
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

AKS kümenizde örnek uygulamayı çalıştırmak için kullanın `draft up` komutu.

```console
draft up
```

Bu komut Dockerfile kapsayıcı görüntüsü oluşturma yapıları, görüntünün, ACR iter ve AKS uygulamayı başlatmak için Helm grafiği yükler. İlk kez bu komutu çalıştırdığınızda, gönderme ve kapsayıcı görüntüsünü çekme biraz zaman alabilir. Temel katman önbelleğe sonra uygulamayı dağıtmak için geçen süreyi önemli ölçüde azaltılır.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

## <a name="connect-to-the-running-sample-application-from-your-local-machine"></a>Bağlanmak için çalışan örnek uygulama, yerel makinenizde

Uygulamayı test etmek için `draft connect` komutu.

```console
draft connect
```

Bu Kubernetes pod'u güvenli bir bağlantı proxy'leri komutu. Tamamlandığında, uygulama üzerinde sağlanan URL erişilebilir.

```console
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Uygulama, bir tarayıcı kullanarak gidin `localhost` örnek uygulamayı görmek için url. Yukarıdaki örnekte URL'dir `http://localhost:49804`. Bağlantı kullanmayı `Ctrl+c`.

## <a name="access-the-application-on-the-internet"></a>İnternet üzerindeki bir uygulamaya erişim

Önceki adımda AKS kümenizde uygulama podunun Ara sunucu bağlantısı oluşturuldu. Geliştirir ve uygulamanızı test ederken, uygulamayı İnternette kullanılabilir yapmak isteyebilirsiniz. İnternet üzerindeki bir uygulamayı kullanıma sunmak için bir türü ile bir Kubernetes hizmeti oluşturabilirsiniz. [LoadBalancer][kubernetes-service-loadbalancer].

Güncelleştirme `charts/example-java/values.yaml` oluşturmak için bir *LoadBalancer* hizmeti. Değiştirin *service.type* gelen *ClusterIP* için *LoadBalancer*.

```yaml
...
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
...
```

Değişikliklerinizi kaydetmek için dosyayı kapatıp çalıştırma `draft up` uygulamayı yeniden çalıştırmak için.

```console
draft up
```

Hizmetin genel bir IP adresini döndürmek birkaç dakika sürer. İlerleme durumunu izlemek için kullanabilirsiniz `kubectl get service` komutunu *watch* parametresi:

```console
$ kubectl get service --watch

NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
...
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Yük dengeleyicinin uygulamanızın bir tarayıcı kullanarak gidin *EXTERNAL-IP* örnek uygulamayı görmek için. Yukarıdaki örnekte, IP olduğundan `52.175.224.118`.

## <a name="iterate-on-the-application"></a>Uygulama üzerinde yineleme

Uygulamanızı yerel olarak değişiklik yapmasını ve yeniden çalıştırılması yineleyebilirsiniz `draft up`.

Üzerinde döndürülen ileti güncelleştirme [src/main/java/helloworld/Hello.java 7 satır][example-java-hello-l7]

```java
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
```

Çalıştırma `draft up` uygulamayı yeniden dağıtmak için komut:

```console
$ draft up

Draft Up Started: 'example-java': 01CMZC9RF0TZT7XPWGFCJE15X4
example-java: Building Docker Image: SUCCESS ⚓  (25.0202s)
example-java: Pushing Docker Image: SUCCESS ⚓  (7.1457s)
example-java: Releasing Application: SUCCESS ⚓  (3.5773s)
Inspect the logs with `draft logs 01CMZC9RF0TZT7XPWGFCJE15X4`
```

Güncelleştirilmiş uygulamayı görmek için yeniden yük dengeleyicinizin IP adresine gidin ve görünen değişikliklerinizi doğrulayın.

## <a name="delete-the-cluster"></a>Küme silme

Küme artık gerekli değilse, [az grubu Sil][az-group-delete] kaynak grubunu, AKS kümesi, kapsayıcı kayıt defteri, kapsayıcı görüntülerini kaldırmak için depolanan vardır ve tüm ilgili kaynakları komutu.

```azurecli-interactive
az group delete --name MyResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz: [AKS hizmet sorumlusu hakkında önemli noktalar ve silme][sp-delete].

## <a name="next-steps"></a>Sonraki adımlar

Draft'ı kullanma hakkında daha fazla bilgi için Github'da taslak belgelerine bakın.

> [!div class="nextstepaction"]
> [Taslak belgeleri][draft-documentation]


[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-create]: /cli/azure/acr#az-acr-login
[az-group-delete]: /cli/azure/group#az-group-delete
[az aks get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az aks install-cli]: /cli/azure/aks#az-aks-install-cli
[kubernetes-ingress]: ./ingress-basic.md

[docker-for-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-for-mac]: https://docs.docker.com/docker-for-mac/
[docker-for-windows]: https://docs.docker.com/docker-for-windows/
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[example-java]: https://github.com/Azure/draft/tree/master/examples/example-java
[example-java-hello-l7]: https://github.com/Azure/draft/blob/master/examples/example-java/src/main/java/helloworld/Hello.java#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[helm-init]: https://docs.helm.sh/helm/#helm-init
[sp-delete]: kubernetes-service-principal.md#additional-considerations
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
