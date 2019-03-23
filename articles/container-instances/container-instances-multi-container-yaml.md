---
title: Azure Container Instances ile Azure CLI ve YAML çok kapsayıcılı grupları dağıtma
description: Azure CLI ve bir YAML dosyası kullanarak birden çok kapsayıcı Azure Container ınstances'da bir kapsayıcı grubu dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/21/2019
ms.author: danlep
ms.openlocfilehash: 10f2340bd85da3dabcd50d51a4dd56d58d31675b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372446"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Bir YAML çok kapsayıcılı kapsayıcı grubu dağıtma

Azure Container Instances kullanarak tek bir ana bilgisayar üzerine birden çok kapsayıcı dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Çok kapsayıcılı kapsayıcı grupları günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken hizmet ikinci bir bağlı işlem gereken yere yararlıdır.

Azure CLI kullanarak çok kapsayıcılı grupları dağıtma için iki yöntem vardır:

* YAML dosyası dağıtımı (Bu makale)
* [Resource Manager şablon dağıtımı](container-instances-multi-container-group.md)

Dağıtımınız varsa YAML formatın daha kısa niteliği nedeniyle dağıtım ile bir YAML dosyası önerilir *yalnızca* kapsayıcı örnekleri. Kapsayıcı örneği dağıtımıyla zamanında ek Azure hizmeti kaynaklarına (örneğin, bir Azure dosya paylaşımı) dağıtmanız gerekiyorsa, Resource Manager şablon dağıtımı önerilir.

> [!NOTE]
> Birden çok kapsayıcı grubunun şu anda Linux kapsayıcılarıyla kısıtlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışıyoruz, ancak geçerli platform farklılıklarını içinde bulabilirsiniz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>YAML dosyası yapılandırma

Bir çoklu kapsayıcı kapsayıcı grubu dağıtmak için [az kapsayıcı oluşturma] [ az-container-create] kapsayıcı grubu yapılandırması bir YAML dosyası belirtin. ardından YAML dosyası olarak geçirmek Azure CLI, komut bir komut parametresi.

Başlangıç adlı yeni bir dosyaya aşağıdaki YAML kopyalayarak **dağıtma aci.yaml**.

Bu YAML dosyası iki kapsayıcı "myContainerGroup", genel bir IP adresi ve iki kullanıma sunulan bağlantı noktası adlı bir kapsayıcı grubunu tanımlar. Kapsayıcıları, genel Microsoft görüntülerden dağıtılır. İlk kapsayıcı grubunda bir internet'e yönelik web uygulaması çalışır. İkinci kapsayıcıyı, sepet düzenli aralıklarla kapsayıcı grubun yerel ağ üzerinden ilk kapsayıcıda çalıştırılan web uygulaması için HTTP isteklerini gönderir.

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: mcr.microsoft.com/azuredocs/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="deploy-the-container-group"></a>Kapsayıcı grubu dağıtma

Bir kaynak grubu oluşturun [az grubu oluşturma] [ az-group-create] komutu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kapsayıcı grubu dağıtma [az kapsayıcı oluşturma] [ az-container-create] komutu, YAML dosyası bağımsız değişken geçirme:

```azurecli-interactive
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir.

## <a name="view-deployment-state"></a>Görünüm dağıtım durumu

Dağıtım durumunu görüntülemek için aşağıdakileri kullanın [az container show] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Çalışan uygulamayı görüntülemek istiyorsanız, tarayıcınızda IP adresine gidin. Örneğin, IP olduğundan `52.168.26.124` , bu örnek çıktı:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Günlükleri görüntüleme

Kullanarak bir kapsayıcının günlük çıktısını görüntülemek [az kapsayıcı günlüklerini] [ az-container-logs] komutu. `--container-name` Kapsayıcı günlüklerini çekme yapılacağı bağımsız değişken belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Çıkış:

```console
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Yan araba kapsayıcı için günlükleri görmek için ikinci kapsayıcının adını belirterek aynı komutu çalıştırın.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Çıkış:

```console
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Gördüğünüz gibi sepet düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden bir HTTP istek yapıyor. Bir HTTP yanıt kodu 200 dışında Tamam aldığında, bir uyarı tetiklemek için bu sepet örneği genişletilemiyor.

## <a name="deploy-from-private-registry"></a>Özel kayıt defterinden dağıtma

Özel kapsayıcı görüntüsünü kayıt defteri kullanmak için ortamınız için değişiklik değerlerle aşağıdaki YAML ekleyin:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

Örneğin, aşağıdaki YAML, görüntü "myregistry" adlı özel Azure kapsayıcısı Kayıt Defteri'nden çekilen tek bir kapsayıcı bir kapsayıcı grubu dağıtır:

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup2
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: myregistry.azurecr.io/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
  imageRegistryCredentials:
  - server: myregistry.azurecr.io
    username: myregistry
    password: REGISTRY_PASSWORD
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="export-container-group-to-yaml"></a>YAML için kapsayıcı grubunu dışarı aktarma

Azure CLI komutunu kullanarak var olan bir kapsayıcı grubu yapılandırmasını bir YAML dosyasına aktarabilirsiniz [az container dışarı aktarma][az-container-export].

Bir kapsayıcı grubun yapılandırması koruma için kullanışlı, dışarı aktarma kapsayıcı grubu yapılandırmalarınızı "yapılandırma" kod olarak sürüm denetiminde depolamanıza olanak tanır Veya yeni bir YAML yapılandırmasında geliştirirken dışarı aktarılan dosya başlangıç noktası olarak kullanın.

Daha önce oluşturduğunuz aşağıdaki göndererek kapsayıcı grubu için yapılandırmayı dışarı aktarmak [az container dışarı aktarma] [ az-container-export] komutu:

```azurecli-interactive
az container export --resource-group myResourceGroup --name myContainerGroup --file deployed-aci.yaml
```

Çıkış, komut başarılı olur, ancak sonuç görmek için dosyanın içeriğini görüntüleyebilirsiniz görüntülenmez. Örneğin, ilk birkaç satırı ile `head`:

```console
$ head deployed-aci.yaml
additional_properties: {}
apiVersion: '2018-06-01'
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çoklu kapsayıcı Azure container örneği dağıtmak için gerekli olan adımları ele. Özel Azure kapsayıcısı kayıt defteri kullanma dahil olmak üzere bir uçtan uca Azure Container Instances deneyimi için Azure Container Instances Öğreticisine bakın.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi][aci-tutorial]

<!-- LINKS - External -->
[cli-issue-6525]: https://github.com/Azure/azure-cli/issues/6525

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-export]: /cli/azure/container#az-container-export
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
