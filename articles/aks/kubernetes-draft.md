---
title: AKS ve Azure Container Registry ile taslak kullanma
description: AKS ve Azure Container Registry ile taslak kullanma
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 08/15/2018
ms.author: zarhoads
ms.openlocfilehash: 462cfd6ec0a6b25f85dda0245dd4f5feed7cb712
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755658"
---
# <a name="use-draft-with-azure-kubernetes-service-aks"></a>Azure Kubernetes Service'i (AKS) ile taslak kullanma

Taslak, paket yardımcı olur ve geliştirme döngüsü - konsantre geliştirme "İç döngü" odaklanmak boş bırakın, bir Kubernetes kümesinde uygulama kapsayıcıları dağıtma bir açık kaynak araçtır. Taslak, kod geliştirilen gibi ancak sürüm denetimine gerçekleştirmeden önce çalışır. Kod değişiklikleri ortaya çıktıkları taslak ile hızlı bir şekilde Kubernetes uygulamaya yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için bkz. [taslak belgeleri github'da][draft-documentation].

Bu makalede bir Kubernetes kümesinde AKS ile taslak kullanma gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede ayrıntılı adımlarda bir AKS kümesi oluşturduğunuz ve belirledik varsayılır bir `kubectl` kümeyle bağlantı. Bu öğelere gereksiniminiz varsa bkz [AKS hızlı başlangıçları][aks-quickstart].

Özel Docker kayıt defteri Azure Container Registry (ACR) ihtiyacınız vardır. ACR örneği oluşturma adımları için bkz: [Azure Container Registry hızlı][acr-quickstart].

Helm, AKS kümenizin ayrıca yüklenmesi gerekir. Yükleme ve Helm yapılandırma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) ile kullanım Helm][aks-helm].

Son olarak, yüklemeniz gereken [Docker](https://www.docker.com).

## <a name="install-draft"></a>Draft'ı yükleme

Taslak CLI geliştirme sisteminizde çalışır ve kod bir Kubernetes kümesine dağıtmak için izin veren bir istemcidir. Mac bilgisayarlarda taslak CLI'yı yüklemek için kullanın `brew`. Ek yükleme seçenekleri için bkz [taslak Yükleme Kılavuzu][draft-documentation].

> [!NOTE]
> Taslak sürümü 0,12 önce yüklü değilse, taslağı, küme kullanarak silin `helm delete --purge draft` ve ardından çalıştırarak yerel yapılandırmanızı kaldırın `rm -rf ~/.draft`. Macos'ta ise çalıştırın, ardından `brew upgrade draft`.

```console
brew tap azure/draft
brew install draft
```

Artık taslak ile başlatma `draft init` komutu:

```console
draft init
```

## <a name="configure-draft"></a>Draft'ı yapılandırma

Taslak, yerel olarak kapsayıcı görüntüleri oluşturur ve sonra da dağıtır bunları yerel kayıt defterinden (gibi Minikube ile), veya belirttiğiniz bir görüntü kayıt defteri kullanır. Bu makalede, Azure Container Registry (ACR) kullanılmıştır, bu yüzden, bir AKS kümesi ve ACR kayıt defteri arasında bir güven ilişkisi oluşturmanız gerekir kapsayıcı görüntülerinizi ACR'ye gönderin taslak ardından yapılandırın.

### <a name="create-trust-between-aks-cluster-and-acr"></a>AKS kümesi ve ACR arasında güven oluşturma

Bir AKS kümesi ve ACR kayıt arasında güven oluşturmak için ACR kayıt defterine erişim için AKS kümesi tarafından kullanılan Azure Active Directory Hizmet sorumlusu için izinler verir. Aşağıdaki komutlar, kendi sağlamak `<resourceGroupName>`, değiştirin `<aksName>` adıyla değiştirin ve AKS kümesi `<acrName>` ACR kayıt defterinizin adıyla:

```azurecli
# Get the service principal ID of your AKS cluster
AKS_SP_ID=$(az aks show --resource-group <resourceGroupName> --name <aksName> --query "servicePrincipalProfile.clientId" -o tsv)

# Get the resource ID of your ACR instance
ACR_RESOURCE_ID=$(az acr show --resource-group <resourceGroupName> --name <acrName> --query "id" -o tsv)

# Create a role assignment for your AKS cluster to access the ACR instance
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

ACR erişmek için bu adımları hakkında daha fazla bilgi için bkz. [ACR ile kimlik doğrulaması](../container-registry/container-registry-auth-aks.md).

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Taslak gönderin ve ACR'den dağıtmak için yapılandırma

AKS kümenizi ACR kullanımdan yoktur, ACR ile AKS arasında bir güven ilişkisi etkinleştirin.

1. Taslak yapılandırma kümesi *kayıt defteri* değeri. Aşağıdaki komutlar, değiştirin `<acrName>` ACR kayıt defterinizin adıyla:

    ```console
    draft config set registry <acrName>.azurecr.io
    ```

1. ACR kayıt defteri ile oturum [az acr oturum açma][az-acr-login]:

    ```azurecli
    az acr login --name <acrName>
    ```

AKS ile ACR arasında bir güven oluşturulduğu gibi herhangi bir parola veya gizli dizileri gönderin veya ACR kayıt defterinden çekmek için gerekli değildir. Kimlik doğrulaması, Azure Active Directory'yi kullanarak Azure Resource Manager düzeyinde gerçekleşir.

## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Taslak nasıl çalıştığını görmek için bir örnek uygulamadan dağıtalım [Draft deposunda][draft-repo]. İlk olarak, depoyu kopyalama:

```console
git clone https://github.com/Azure/draft
```

Java örnekleri dizine değiştirin:

```console
cd draft/examples/example-java/
```

Kullanım `draft create` işlemini başlatmak için komutu. Bu komut, bir Kubernetes kümesinde uygulama çalıştırmak için kullanılan yapıtları oluşturur. Bu öğeler bir Dockerfile, Helm grafiği içerir ve bir *draft.toml* taslak yapılandırma dosyası olan bir dosya.

```
$ draft create

--> Draft detected Java (92.205567%)
--> Ready to sail
```

AKS kümenizde örnek uygulamayı çalıştırmak için kullanın `draft up` komutu. Bu komut bir kapsayıcı görüntüsü oluşturmak için bir Dockerfile oluşturur, görüntüyü ACR'ye gönderir ve son olarak uygulamayı AKS başlatmak için Helm grafiği yükler.

Bu komut bir ilk çalıştırıldığında, gönderme ve kapsayıcı görüntüsünü çekme biraz zaman alabilir. Temel katman önbelleğe sonra uygulamayı dağıtmak için geçen süreyi önemli ölçüde azaltılır.

```
$ draft up

Draft Up Started: 'example-java': 01CMZAR1F4T1TJZ8SWJQ70HCNH
example-java: Building Docker Image: SUCCESS ⚓  (73.0720s)
example-java: Pushing Docker Image: SUCCESS ⚓  (19.5727s)
example-java: Releasing Application: SUCCESS ⚓  (4.6979s)
Inspect the logs with `draft logs 01CMZAR1F4T1TJZ8SWJQ70HCNH`
```

Docker görüntüsü gönderiliyor sorunlarla karşılaşırsanız, başarıyla ACR kayıt defterinize ile açtıktan emin olun [az acr oturum açma][az-acr-login], daha sonra deneyin `draft up` yeniden komutu.

## <a name="test-the-application-locally"></a>Uygulamayı yerel olarak test etme

Uygulamayı test etmek için `draft connect` komutu. Bu Kubernetes pod'u güvenli bir bağlantı proxy'leri komutu. Tamamlandığında, uygulama üzerinde sağlanan URL erişilebilir.

> [!NOTE]
> Kapsayıcı görüntüsünün indirilmesi birkaç dakika ve uygulamayı başlatmak için beklemeniz gerekebilir. Uygulamaya erişirken bir hata alırsanız, bağlantı yeniden deneyin.

```
$ draft connect

Connect to java:4567 on localhost:49804
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See https://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567
```

Uygulamanıza erişmek için belirtilen bağlantı noktası ve adresi için bir web tarayıcısı açın `draft connect` çıkış, gibi `http://localhost:49804`. 

![Draft ile çalışan örnek Java uygulama](media/kubernetes-draft/sample-app.png)

Kullanım `Control+C` Ara sunucu bağlantısı durdurmak için.

> [!NOTE]
> Ayrıca `draft up --auto-connect` oluşturup uygulamanızı dağıtmak ardından hemen ilk çalışan kapsayıcıya bağlanmak için komutu.

## <a name="access-the-application-on-the-internet"></a>İnternet üzerindeki bir uygulamaya erişim

Önceki adımda AKS kümenizde uygulama podunun Ara sunucu bağlantısı oluşturuldu. Geliştirir ve uygulamanızı test ederken, uygulamayı İnternette kullanılabilir yapmak isteyebilirsiniz. Bir türü ile bir Kubernetes hizmeti oluşturduğunuz internet üzerindeki bir uygulamayı kullanıma sunmak için [LoadBalancer][kubernetes-service-loadbalancer], veya bir [giriş denetleyicisine] [ kubernetes-ingress]. Oluşturalım bir *LoadBalancer* hizmeti.

İlk olarak, güncelleştirme *values.yaml* paketi belirtmek için bir türle bir hizmet taslak *LoadBalancer* oluşturulmalıdır:

```console
vi charts/java/values.yaml
```

Bulun *service.type* özelliği ve değerini güncelleştirme *ClusterIP* için *LoadBalancer*sıkıştırılmış aşağıdaki örnekte gösterildiği gibi:

```yaml
[...]
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
[...]
```

Kaydedin ve dosyayı kapatın ve ardından kullanmak `draft up` uygulamayı yeniden çalıştırmak için:

```console
draft up
```

Hizmetin genel bir IP adresini döndürmek birkaç dakika sürer. İlerleme durumunu izlemek için kullanabilirsiniz `kubectl get service` komutunu *watch* parametresi:

```console
kubectl get service --watch
```

Başlangıçta *EXTERNAL-IP* hizmet olarak görünür *bekleyen*:

```
NAME                TYPE          CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
example-java-java   LoadBalancer  10.0.141.72   <pending>     80:32150/TCP   2m
```

EXTERNAL-IP adresi değiştiğinde *bekleyen* için IP adresi, `Control+C` durdurmak için `kubectl` işlem izleyin:

```
NAME                TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)        AGE
example-java-java   LoadBalancer   10.0.141.72   52.175.224.118  80:32150/TCP   7m
```

Uygulamayı görmek için dış IP adresi ile yük dengeleyicinizin gözatın `curl`:

```
$ curl 52.175.224.118

Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Uygulama üzerinde yineleme

Taslak yapılandırıldı ve uygulamayı Kubernetes'te çalışan göre kod yinelemesi için ayarlanır. Her zaman, istediğiniz test etmek için güncelleştirilmiş kod, çalıştırma `draft up` çalışan uygulamayı güncelleştirmek için komutu.

Bu örnekte, görünen metni değiştirmek için Java örnek uygulamayı güncelleştirin. Açık *Hello.java* dosyası:

```console
vi src/main/java/helloworld/Hello.java
```

Çıkış metnini görüntülemek için güncelleştirme *Hello World ben aks'deki Java!*:

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
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

Güncelleştirilmiş uygulamayı görüntülemek için IP adresini yük dengeleyicinizin curl:

```
$ curl 52.175.224.118

Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Sonraki adımlar

Draft'ı kullanma hakkında daha fazla bilgi için Github'da taslak belgelerine bakın.

> [!div class="nextstepaction"]
> [Taslak belgeleri][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer
[draft-repo]: https://github.com/Azure/draft

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[kubernetes-ingress]: ./ingress-basic.md
[aks-quickstart]: ./kubernetes-walkthrough.md
[az-acr-login]: /cli/azure/acr#az-acr-login
