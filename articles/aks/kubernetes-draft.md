---
title: "Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma"
description: "Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 803d9e9ea7411c6de4dd15670f495fa8e169a989
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2018
---
# <a name="use-draft-with-azure-container-service-aks"></a>Taslak Azure kapsayıcı hizmeti (AKS) kullanın

Taslak paketi yardımcı olur ve kod Kubernetes kümede çalışacak bir açık kaynak aracıdır. Taslak geliştirme yineleme döngüsünde yöneliktir; kod geliştirilmiş, ancak sürüm Denetimi'ne gerçekleştirmeden önce. Kod değişiklikleri ortaya çıktığında taslak ile hızlı bir şekilde Kubernetes uygulamaya yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için bkz: [taslak belgeler Github üzerinde][draft-documentation].

Bu belge ayrıntıları AKS Kubernetes kümede taslak kullanarak.

## <a name="prerequisites"></a>Önkoşullar

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğeler gerekirse bkz [AKS quickstart][aks-quickstart].

Ayrıca özel Docker kayıt defteri Azure kapsayıcı kayıt defteri (ACR) gerekir. ACR örneğini dağıtma ile ilgili yönergeler için bkz: [Azure kapsayıcı kayıt defteri Quickstart][acr-quickstart].

Helm AKS kümenizdeki ayrıca yüklenmesi gerekir. Helm yükleme hakkında daha fazla bilgi için bkz: [kullanım Helm Azure kapsayıcı hizmeti (AKS) ile][aks-helm].

## <a name="install-draft"></a>Taslak yükleyin

Taslak CLI geliştirme sisteminizde çalıştıran ve quicky için kod Kubernetes kümesine dağıttığınız sağlayan bir istemci olur.

Bir Mac üzerinde taslak CLI yüklemek için `brew`. Ek yükleme seçenekleri için bkz, [taslak Yükleme Kılavuzu][install-draft].

```console
brew install draft
```

Çıktı:

```
==> Installing draft from azure/draft
==> Downloading https://azuredraft.blob.core.windows.net/draft/draft-v0.7.0-darwin-amd64.tar.gz
Already downloaded: /Users/neilpeterson/Library/Caches/Homebrew/draft-0.7.0.tar.gz
==> /usr/local/Cellar/draft/0.7.0/bin/draft init --client-only
🍺  /usr/local/Cellar/draft/0.7.0: 6 files, 61.2MB, built in 1 second
```

## <a name="configure-draft"></a>Taslak yapılandırın

Taslak yapılandırırken, bir kapsayıcı kayıt defteri belirtilmesi gerekiyor. Bu örnekte, Azure kapsayıcı kayıt defteri kullanılır.

Ad ve oturum açma sunucusu ACR örneğinizin adını almak için aşağıdaki komutu çalıştırın. Komut ACR örneğinizi içeren kaynak grubunun adını güncelleştirin.

```console
az acr list --resource-group <resource group> --query "[].{Name:name,LoginServer:loginServer}" --output table
```

ACR örneği parola de gereklidir.

ACR parolasını döndürmek için aşağıdaki komutu çalıştırın. Komut ACR örneğinin adını güncelleştirin.

```console
az acr credential show --name <acr name> --query "passwords[0].value" --output table
```

Taslak ile başlatma `draft init` komutu.

```console
draft init
```

Bu işlem sırasında kapsayıcı kayıt defteri kimlik bilgileri istenir. Azure kapsayıcı kayıt defteri kullanırken, kayıt defteri URL'dir ACR oturum açma sunucusu adı, kullanıcı adı ACR örneği adı belirtin ve parola ACR paroladır.

```console
1. Enter your Docker registry URL (e.g. docker.io/myuser, quay.io/myuser, myregistry.azurecr.io): <ACR Login Server>
2. Enter your username: <ACR Name>
3. Enter your password: <ACR Password>
```

Tamamlandıktan sonra taslak Kubernetes kümede yapılandırılmış ve kullanıma hazırdır.

```
Draft has been installed into your Kubernetes Cluster.
Happy Sailing!
```

## <a name="run-an-application"></a>Bir uygulamayı çalıştırma

Taslak depo taslak gösteri için kullanılabilecek birkaç örnek uygulamaları içerir. Depodaki kopyalanan bir kopyasını oluşturun.

```console
git clone https://github.com/Azure/draft
```

Java örnekler dizine geçin.

```console
cd draft/examples/java/
```

Kullanım `draft create` işlemini başlatmak için komutu. Bu komut, bir Kubernetes kümede uygulamayı çalıştırmak için kullanılan yapılar oluşturur. Bu öğeler, bir Dockerfile Helm grafik içerir ve bir `draft.toml` taslak yapılandırma dosyası dosya.

```console
draft create
```

Çıktı:

```
--> Draft detected the primary language as Java with 92.205567% certainty.
--> Ready to sail
```

Uygulama Kubernetes kümede çalıştırmak için kullandığınız `draft up` komutu. Bu komut Kubernetes kümeye uygulama kodu ve yapılandırma dosyalarını yükler. Ardından bir kapsayıcı görüntüsü oluşturmak için Dockerfile çalıştırır, görüntünün kapsayıcı kayıt defterine iter ve son olarak uygulamayı başlatmak için Helm grafik çalıştırır.

```console
draft up
```

Çıktı:

```
Draft Up Started: 'open-jaguar'
open-jaguar: Building Docker Image: SUCCESS ⚓  (28.0342s)
open-jaguar: Pushing Docker Image: SUCCESS ⚓  (7.0647s)
open-jaguar: Releasing Application: SUCCESS ⚓  (4.5056s)
open-jaguar: Build ID: 01BW3VVNZYQ5NQ8V1QSDGNVD0S
```

## <a name="test-the-application"></a>Uygulamayı test etme

Uygulamayı test etmek için kullanmak `draft connect` komutu. Bu komut proxy'leri Kubernetes bağlantı pod güvenli bir yerel bağlantı izin verme. Tamamlandığında, uygulama üzerinde sağlanan URL erişilebilir.

Bazı durumlarda, yüklenmek üzere kapsayıcı görüntü için bir kaç dakika ve uygulamayı başlatmak için alabilir. Uygulama erişirken bir hata alırsanız, bağlantı yeniden deneyin.

```console
draft connect
```

Çıktı:

```
Connecting to your app...SUCCESS...Connect to your app on localhost:46143
Starting log streaming...
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
== Spark has ignited ...
>> Listening on 0.0.0.0:4567
```

Uygulama kullanımı test bittiğinde `Control+C` proxy bağlantı durdurmak için.

## <a name="expose-application"></a>Uygulama kullanıma sunma

Bir uygulama içinde Kubernetes sınarken, uygulamayı İnternette kullanılabilir yapmak isteyebilirsiniz. Bu yapılabilir bir türüyle Kubernetes hizmetini kullanarak [LoadBalancer] [ kubernetes-service-loadbalancer] veya bir [giriş denetleyicisi][kubernetes-ingress]. Bu belge ayrıntıları Kubernetes hizmetini kullanarak.


İlk olarak, Taslak paketi belirtmek için güncellenmelidir türüne sahip bir hizmet `LoadBalancer` oluşturulmalıdır. Bunu yapmak için hizmet türü güncelleştirme `values.yaml` dosya.

```console
vi chart/java/values.yaml
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
deadly-squid-java   10.0.141.72   <pending>     80:32150/TCP   14m
```

EXTERNAL-IP adresi `pending` durumundan `IP address` değerine değiştiğinde kubectl izleme işlemini durdurmak için `Control+C` komutunu kullanın.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
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
        get("/", (req, res) -> "Hello World, I'm Java - Draft Rocks!");
    }
}
```

Çalıştırma `draft up` uygulama dağıtmak için komutu.

```console
draft up
```

Çıktı

```
Draft Up Started: 'deadly-squid'
deadly-squid: Building Docker Image: SUCCESS ⚓  (18.0813s)
deadly-squid: Pushing Docker Image: SUCCESS ⚓  (7.9394s)
deadly-squid: Releasing Application: SUCCESS ⚓  (6.5005s)
deadly-squid: Build ID: 01BWK8C8X922F5C0HCQ8FT12RR
```

Son olarak, güncelleştirmeleri görmek için uygulamayı görüntüle.

```console
curl 52.175.224.118
```

Çıktı:

```
Hello World, I'm Java - Draft Rocks!
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