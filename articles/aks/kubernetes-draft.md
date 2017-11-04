---
title: "Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma | Microsoft Docs"
description: "Taslak AKS ve Azure kapsayıcı kayıt defteri ile kullanma"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: draft, helm, aks, azure-container-service
keywords: "Docker, Kapsayıcılar, mikro hizmetler, Kubernetes, Draft, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3e607a6ce5662f6ff597fafbcec8c864f25ff54c
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="use-draft-with-azure-container-service-aks"></a>Taslak Azure kapsayıcı hizmeti (AKS) kullanın

Taslak paketi yardımcı olur ve kod Kubernetes kümede çalışacak bir açık kaynak aracıdır. Taslak geliştirme yineleme döngüsünde yöneliktir; kod geliştirilmiş, ancak sürüm Denetimi'ne gerçekleştirmeden önce. Kod değişiklikleri ortaya çıktığında taslak ile hızlı bir şekilde Kubernetes uygulamaya yeniden dağıtabilirsiniz. Taslak hakkında daha fazla bilgi için bkz: [taslak belgeler Github üzerinde](https://github.com/Azure/draft/tree/master/docs).

Bu belge ayrıntıları AKS Kubernetes kümede taslak kullanarak.

## <a name="prerequisites"></a>Ön koşullar

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğeler gerekirse bkz [AKS quickstart](./kubernetes-walkthrough.md).

Ayrıca özel Docker kayıt defteri Azure kapsayıcı kayıt defteri (ACR) gerekir. ACR örneğini dağıtma ile ilgili yönergeler için bkz: [Azure kapsayıcı kayıt defteri Quickstart](../container-registry/container-registry-get-started-azure-cli.md).

## <a name="install-helm"></a>Helm yükleyin

Helm CLI geliştirme sisteminizde çalıştıran ve başlatma, durdurma ve Helm grafiklerle uygulamaları yönetmenize olanak sağlayan bir istemci olur.

Mac üzerinde Helm CLI yüklemek için kullandığınız `brew`. Ek yükleme seçenekleri için bkz [yükleme Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md).

```console
brew install kubernetes-helm
```

Çıktı:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="install-draft"></a>Taslak yükleyin

Taslak CLI geliştirme sisteminizde çalıştıran ve quicky için kod Kubernetes kümesine dağıttığınız sağlayan bir istemci olur.

Bir Mac üzerinde taslak CLI yüklemek için `brew`. Ek yükleme seçenekleri için bkz, [taslak Yükleme Kılavuzu](https://github.com/Azure/draft/blob/master/docs/install.md).

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

## <a name="run-an-application"></a>Bir uygulamayı çalıştırın

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

Bir uygulama içinde Kubernetes sınarken, uygulamayı İnternette kullanılabilir yapmak isteyebilirsiniz. Bu yapılabilir bir türüyle Kubernetes hizmetini kullanarak [LoadBalancer](https://kubernetes.io/docs/concepts/services-networking/service/#type-loadbalancer) veya bir [giriş denetleyicisi](https://kubernetes.io/docs/concepts/services-networking/ingress/). Bu belge ayrıntıları Kubernetes hizmetini kullanarak.


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

DIŞ IP adresi değiştiğinden sonra `pending` için bir `IP address`, kullanın `Control+C` kubectl izleme işlemi durdurmak için.

```
deadly-squid-java   10.0.141.72   52.175.224.118   80:32150/TCP   17m
```

Uygulama görmek için dış IP adresine göz atın.

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
> [Taslak belgeleri](https://github.com/Azure/draft/tree/master/docs)