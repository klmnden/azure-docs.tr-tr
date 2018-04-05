---
title: Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma
description: Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 03/29/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 2ab79e3a6308d01d836a82f356f43eccb6af9791
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="use-draft-with-azure-container-service-aks"></a>Taslak Azure kapsayıcı hizmeti (AKS) kullanın

Taslak içerir ve bir Kubernetes kümedeki geliştirme döngüsü--"İç döngü" konsantre geliştirme yoğunlaşmak boş bırakarak bu kapsayıcıları dağıtın yardımcı olan bir açık kaynak aracıdır. Taslak kodu geliştirilen gibi ancak sürüm Denetimi'ne gerçekleştirmeden önce çalışır. Kod değişiklikleri ortaya çıktığında taslak ile hızlı bir şekilde Kubernetes uygulamaya yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için bkz: [taslak belgeler Github üzerinde][draft-documentation].

Bu belge ayrıntıları AKS Kubernetes kümede taslak kullanarak.

## <a name="prerequisites"></a>Önkoşullar

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğeler gerekirse bkz [AKS quickstart][aks-quickstart].

Ayrıca özel Docker kayıt defteri Azure kapsayıcı kayıt defteri (ACR) gerekir. ACR örneğini dağıtma ile ilgili yönergeler için bkz: [Azure kapsayıcı kayıt defteri Quickstart][acr-quickstart].

Helm AKS kümenizdeki ayrıca yüklenmesi gerekir. Helm yükleme hakkında daha fazla bilgi için bkz: [kullanım Helm Azure kapsayıcı hizmeti (AKS) ile][aks-helm].

Son olarak, yüklemeniz gereken [Docker](https://www.docker.com).

## <a name="install-draft"></a>Taslak yükleyin

Taslak CLI geliştirme sisteminizde çalıştıran ve quicky için kod Kubernetes kümesine dağıttığınız sağlayan bir istemci olur. 

> [!NOTE] 
> Taslak sürümü 0.12 önce yüklediyseniz, Taslak, küme kullanımından silmeniz `helm delete --purge draft` ve yerel yapılandırmanızı çalıştırarak kaldırın `rm -rf ~/.draft`. MacOS üzerinde varsa, çalıştırabilirsiniz `brew upgrade draft`.

Bir Mac üzerinde taslak CLI yüklemek için `brew`. Ek yükleme seçenekleri için bkz, [taslak Yükleme Kılavuzu][install-draft].

```console
brew tap azure/draft
brew install draft
```

Şimdi taslak ile başlatma `draft init` komutu.

```console
draft init
```

## <a name="configure-draft"></a>Taslak yapılandırın

Taslak kapsayıcı görüntü yerel olarak oluşturur ve ardından ya da bunları (durumunda Minikube) yerel kayıt defterinden dağıtır veya görüntü kayıt defterini belirtmeniz gerekir. Bu örnek, Azure kapsayıcı kayıt defteri (ACR) kullanır, bu nedenle AKS kümenizi ve ACR kayıt defteri arasında bir güven ilişkisi kurmak ve kapsayıcı için ACR göndermeyi taslak yapılandırmanız gerekir.

### <a name="create-trust-between-aks-cluster-and-acr"></a>AKS küme ve ACR arasında güven oluşturma

AKS küme ve ACR kayıt defteri arasında güven oluşturmak için Azure Active Directory Hizmeti katkıda bulunan rolü ACR depo kapsamıyla ekleyerek AKS ile kullanılan asıl değiştirin. Bunu yapmak için değiştirme aşağıdaki komutları çalıştırın _&lt;aks rg adı&gt;_ ve _&lt;aks küme adı&gt;_ adını ve kaynak grubu ile AKS küme ve _&lt;acr rg adı&gt;_ ve _&lt;acr depo adı&gt;_ , ACR kaynak grubu ve depo adı güven oluşturmak istediğiniz deposu.

```console
export AKS_SP_ID=$(az aks show -g <aks-rg-name> -n <aks-cluster-name> --query "servicePrincipalProfile.clientId" -o tsv)
export ACR_RESOURCE_ID=$(az acr show -g <acr-rg-name> -n <acr-repo-name> --query "id" -o tsv)
az role assignment create --assignee $AKS_SP_ID --scope $ACR_RESOURCE_ID --role contributor
```

(Bu adımları ve ACR erişmek için diğer kimlik doğrulama mekanizmaları altındadır [ACR ile kimlik doğrulaması](../container-registry/container-registry-auth-aks.md).)

### <a name="configure-draft-to-push-to-and-deploy-from-acr"></a>Taslak iletin ve ACR dağıtmak için yapılandırma

Yoktur göre AKS ACR arasında bir güven ilişkisi, aşağıdaki adımları AKS kümenizi ACR kullanımdan etkinleştirin.
1. Taslak yapılandırma kümesi `registry` çalıştırarak değeri `draft config set registry <registry name>.azurecr.io`, burada _&lt;kayıt defteri adı&lt;_ ACR kaydınız adıdır.
2. Oturum çalıştırarak ACR kayıt defterini açın `az acr login -n <registry name>`. 

Şimdi yerel olarak ACR için oturum açmış ve AKS ve ACR ile bir güven ilişkisi oluşturan olduğundan, hiçbir parolaları veya gizli iletin veya ACR AKS çekmek için gereklidir. Kimlik doğrulaması Azure Active Directory'yi kullanarak Azure Resource Manager düzeyinde gerçekleşir. 

## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Taslak depo taslak gösteri için kullanılabilecek birkaç örnek uygulamaları içerir. Depodaki kopyalanan bir kopyasını oluşturun.

```console
git clone https://github.com/Azure/draft
```

Java örnekler dizine geçin.

```console
cd draft/examples/example-java/
```

Kullanım `draft create` işlemini başlatmak için komutu. Bu komut, bir Kubernetes kümede uygulamayı çalıştırmak için kullanılan yapılar oluşturur. Bu öğeler, bir Dockerfile Helm grafik içerir ve bir `draft.toml` taslak yapılandırma dosyası dosya.

```console
draft create
```

Çıktı:

```console
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Uygulama Kubernetes kümede çalıştırmak için kullandığınız `draft up` komutu. Bu komut bir kapsayıcı görüntüsü oluşturmak için Dockerfile oluşturur, görüntü için ACR iter ve son olarak AKS uygulamayı başlatmak için Helm grafik yükler.

Bu, ilk çalıştırıldığında, iletme ve kapsayıcı görüntü çekme biraz zaman alabilir; Temel katman önbelleğe alınan sonra geçen süre önemli ölçüde azalır.

```console
draft up
```

Çıktı:

```console
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (3.0007s)
example-java: Releasing Application: SUCCESS ⚓  (0.9322s)
example-java: Build ID: 01C9NPDYQQH2CZENDMZW7ESJAM
Inspect the logs with `draft logs 01C9NPDYQQH2CZENDMZW7ESJAM`
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulamayı test etmek için kullanmak `draft connect` komutu. Bu komut proxy'leri Kubernetes bağlantı pod güvenli bir yerel bağlantı izin verme. Tamamlandığında, uygulama üzerinde sağlanan URL erişilebilir.

Bazı durumlarda, yüklenmek üzere kapsayıcı görüntü için bir kaç dakika ve uygulamayı başlatmak için alabilir. Uygulama erişirken bir hata alırsanız, bağlantı yeniden deneyin.

```console
draft connect
```

Çıktı:

```console
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Göz atarak şimdi uygulamanızı test etmek http://localhost:46143 (önceki örneğin; bağlantı noktası farklı olabilir). Uygulama kullanımı test bittiğinde `Control+C` proxy bağlantı durdurmak için.

> [!NOTE]
> Aynı zamanda `draft up --auto-connect` oluşturmak ve uygulamanızı dağıtmak ve hemen yineleme yapmak için ilk çalışan kapsayıcıya bağlanmak için komut daha hızlı geçiş.

## <a name="expose-application"></a>Uygulama kullanıma sunma

Bir uygulama içinde Kubernetes sınarken, uygulamayı İnternette kullanılabilir yapmak isteyebilirsiniz. Bu yapılabilir bir türüyle Kubernetes hizmetini kullanarak [LoadBalancer] [ kubernetes-service-loadbalancer] veya bir [giriş denetleyicisi][kubernetes-ingress]. Bu belge ayrıntıları Kubernetes hizmetini kullanarak.


İlk olarak, Taslak paketi belirtmek için güncellenmelidir türüne sahip bir hizmet `LoadBalancer` oluşturulmalıdır. Bunu yapmak için hizmet türü güncelleştirme `values.yaml` dosya.

```console
vi charts/java/values.yaml
```

Bulun `service.type` özelliği ve değerini güncelleştirme `ClusterIP` için `LoadBalancer`.

```yaml
replicaCount: 2
image:
  repository: openjdk
  tag: 8-jdk-alpine
  pullPolicy: IfNotPresent
service:
  name: java
  type: LoadBalancer
  externalPort: 80
  internalPort: 4567
resources:
  limits:
    cpu: 100m
    memory: 128Mi
  requests:
    cpu: 100m
    memory: 128Mi
  ```

Çalıştırma `draft up` uygulamayı yeniden çalıştırın.

```console
draft up
```

Hizmetinin genel bir IP adresi dönmek birkaç dakika sürebilir. İlerleme kullanımı izlemek için `kubectl get service` bir izleme ile komutu.

```console
kubectl get service -w
```

Başlangıçta, *dış IP* olarak hizmet görünür `pending`.

```
example-java-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

EXTERNAL-IP adresi `pending` durumundan `IP address` değerine değiştiğinde kubectl izleme işlemini durdurmak için `Control+C` komutunu kullanın.

```
example-java-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Uygulamayı görmek için dış IP adresine gözatın.

```console
curl 52.175.224.118
```

Çıktı:

```
Hello World, I'm Java
```

## <a name="iterate-on-the-application"></a>Uygulaması üzerinde yineleme

Taslak yapılandırılmış ve uygulama Kubernetes içinde çalışan göre kod yineleme için ayarlanır. Her zaman gibi test etmek için güncelleştirilmiş kod, çalıştırmak `draft up` çalışan uygulama güncelleştirmesi için komutu.

Bu örnekte, Java hello world uygulamasının güncelleştirin.

```console
vi src/main/java/helloworld/Hello.java
```

Hello World metin güncelleştirin.

```java
package helloworld;

import static spark.Spark.*;

public class Hello {
    public static void main(String[] args) {
        get("/", (req, res) -> "Hello World, I'm Java in AKS!");
    }
}
```

Çalıştırma `draft up --auto-connect` bir pod yanıt hazır gittiği hemen sonra uygulamayı yeniden dağıtmak için komutu.

```console
draft up --auto-connect
```

Çıktı

```
Draft Up Started: 'example-java'
example-java: Building Docker Image: SUCCESS ⚓  (1.0003s)
example-java: Pushing Docker Image: SUCCESS ⚓  (4.0010s)
example-java: Releasing Application: SUCCESS ⚓  (1.1336s)
example-java: Build ID: 01C9NPMJP6YM985GHKDR2J64KC
Inspect the logs with `draft logs 01C9NPMJP6YM985GHKDR2J64KC`
Connect to java:4567 on localhost:39249
Your connection is still active.
Connect to java:4567 on localhost:39249
[java]: SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
[java]: SLF4J: Defaulting to no-operation (NOP) logger implementation
[java]: SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
[java]: == Spark has ignited ...
[java]: >> Listening on 0.0.0.0:4567

```

Son olarak, güncelleştirmeleri görmek için uygulamayı görüntüle.

```console
curl 52.175.224.118
```

Çıktı:

```
Hello World, I'm Java in AKS!
```

## <a name="next-steps"></a>Sonraki adımlar

Taslak kullanma hakkında daha fazla bilgi için Github'da taslak belgelerine bakın.

> [!div class="nextstepaction"]
> [Taslak belgeleri][draft-documentation]

<!-- LINKS - external -->
[draft-documentation]: https://github.com/Azure/draft/tree/master/docs
[install-draft]: https://github.com/Azure/draft/blob/master/docs/install.md
[kubernetes-ingress]: ./ingress.md
[kubernetes-service-loadbalancer]: https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer

<!-- LINKS - internal -->
[acr-quickstart]: ../container-registry/container-registry-get-started-azure-cli.md
[aks-helm]: ./kubernetes-helm.md
[aks-quickstart]: ./kubernetes-walkthrough.md
