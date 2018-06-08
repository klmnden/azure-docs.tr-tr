---
title: Azure kapsayıcı örnekleriyle Azure CLI ve YAML gruplarında çok kapsayıcı dağıtma
description: Azure CLI ve YAML dosyasını kullanarak Azure kapsayıcı örnekleri birden çok kapsayıcı kapsayıcı grubuyla dağıtmayı öğrenin.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: marsma
ms.openlocfilehash: 5dfee15e978d2dba0f50d1dc4b78953698389950
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851360"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Birden çok kapsayıcı kapsayıcı grubu YAML ile dağıtma

Azure kapsayıcı örneği kullanarak tek bir ana bilgisayar üzerine birden çok kapsayıcı dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Birden çok kapsayıcı kapsayıcı grupları günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken bir hizmetin ikinci bir bağlı işlem nerede ihtiyaç faydalıdır.

Azure CLI kullanarak birden çok kapsayıcı grupları dağıtmak için iki yöntem vardır:

* YAML dosya dağıtımını (Bu makalede)
* [Resource Manager şablonu dağıtımı](container-instances-multi-container-group.md)

Dağıtımınızı içerdiğinde YAML formatı nedeniyle daha kısa yapısı, dağıtımı YAML dosya ile önerilir *yalnızca* kapsayıcı örnekleri. Ek Azure hizmet kaynakları (örneğin, bir Azure dosya paylaşımı) kapsayıcı örnek dağıtım zamanında dağıtımı yapmanız gerekirse, Resource Manager şablonu dağıtım önerilir.

> [!NOTE]
> Birden çok kapsayıcı grupları Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılara getirmek için çalışıyoruz, ancak geçerli platform farklılıkları bulabilirsiniz [kotalar ve Azure kapsayıcı örnekleri için bölge kullanılabilirliği](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>YAML dosyasını yapılandırma

Birden çok kapsayıcı kapsayıcı grubu ile dağıtmak için [az kapsayıcı oluşturmak] [ az-container-create] YAML dosyasında kapsayıcı grubu yapılandırmasını belirtin sonra YAML dosyası olarak geçirmek Azure CLI komut bir komut parametresi.

Başlangıç adlı yeni bir dosyaya aşağıdaki YAML kopyalayarak **dağıtmak aci.yaml**.

Bu YAML dosyası iki kapsayıcı kapsayıcı grubuyla, bir ortak IP adresi ve iki gösterilen bağlantı noktalarını tanımlar. Gruptaki ilk kapsayıcı bir internet'e yönelik web uygulaması çalıştırır. İkinci kapsayıcı, sepet HTTP istekleri kapsayıcı grubun yerel ağ üzerinden ilk kapsayıcıda çalışan web uygulaması için düzenli aralıklarla yapar.

```YAML
apiVersion: 2018-06-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: microsoft/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: microsoft/aci-tutorial-sidecar
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

Bir kaynak grubu ile oluşturmak [az grubu oluşturma] [ az-group-create] komutu:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Kapsayıcı grubuyla dağıtmak [az kapsayıcı oluşturmak] [ az-container-create] komutu, YAML dosya bağımsız değişken olarak geçirme:

```azurecli-interactive
az container create --resource-group myResourceGroup --name myContainerGroup -f deploy-aci.yaml
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir.

## <a name="view-deployment-state"></a>Dağıtım durumunu görüntüle

Dağıtım durumunu görüntülemek için aşağıdakini kullanın [az kapsayıcı Göster] [ az-container-show] komutu:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Çalışan uygulama görüntülemek istiyorsanız, tarayıcınızın IP adresine gidin. Örneğin, IP. `52.168.26.124` Bu örnek çıkışı:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Günlükleri görüntüleme

Kullanarak bir kapsayıcı günlük çıktısını görüntüleyin [az kapsayıcı günlükleri] [ az-container-logs] komutu. `--container-name` Bağımsız değişkeni günlüklerini kapsayıcıyı belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Çıktı:

```console
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Yan araba kapsayıcısı için günlükleri görmek için ikinci kapsayıcı adı belirterek aynı komutu çalıştırın.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Çıktı:

```console
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

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
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Gördüğünüz gibi sepet bir HTTP isteği düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden yapılmasıdır. Bu sepet örnek, bir HTTP yanıt kodu 200 dışında Tamam aldıysanız, bir uyarıyı tetiklemek için genişletilemiyor.

## <a name="deploy-from-private-registry"></a>Özel kayıt defterinden dağıtma

Bir özel kapsayıcı görüntü kayıt defteri kullanmak için ortamınız için değişiklik değerlerle aşağıdaki YAML ekleyin:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

Örneğin, aşağıdaki YAML, görüntü bir özel Azure kapsayıcı "myregistry" adlı kayıt defterinden çekilir tek bir kapsayıcı kapsayıcı grubuyla dağıtır:

```YAML
apiVersion: 2018-06-01
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

## <a name="export-container-group-to-yaml"></a>Kapsayıcı grubu YAML için dışarı aktarma

Varolan bir kapsayıcı grubun yapılandırma Azure CLI komutunu kullanarak YAML dosyasına dışa aktarabilirsiniz [az kapsayıcı verme][az-container-export].

Yararlı bir kapsayıcı grubun yapılandırması koruma için dışa aktarma kapsayıcısı Grup yapılandırmalarınızı "yapılandırma" kod olarak için sürüm denetimi depolamanızı sağlar Veya YAML yeni bir yapılandırmada geliştirirken, dışarı aktarılan dosyayı bir başlangıç noktası olarak kullanın.

Daha önce oluşturduğunuz aşağıdaki vererek kapsayıcı grubunun yapılandırmasını dışarı aktarma [az kapsayıcı verme] [ az-container-export] komutu:

```azurecli-interactive
az container export --resource-group rg604 --name myContainerGroup --file deployed-aci.yaml
```

Çıktı komutu başarılı oldu, ancak sonuç görmek için dosyanın içeriğini görüntüleyebilir, görüntülenmez. Örneğin, ilk birkaç satırı ile `head`:

```console
$ head deployed-aci.yaml
apiVersion: 2018-02-01-preview
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: microsoft/aci-helloworld:latest
      ports:
```

> [!NOTE]
> Azure CLI Sürüm 2.0.34 itibariyle, var olan bir [bilinen sorun] [ cli-issue-6525] kapsayıcı grupları daha eski bir API sürümü belirtin, dışa aktarılan **2018-02-01-Önizleme** (önceki görülen JSON örnek çıktı). Dışarı aktarılan YAML dosyasını kullanarak yeniden dağıtmak istiyorsanız, güvenli bir şekilde güncelleştirebilirsiniz `apiVersion` dışarı aktarılan YAML dosyasındaki değer **2018-06-01**.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çok kapsayıcı Azure kapsayıcı örneği dağıtmak için gerekli olan adımları ele. Bir özel Azure kapsayıcı kayıt defteri kullanımı dahil olmak üzere bir uçtan uca Azure kapsayıcı örnekleri deneyimi için Azure kapsayıcı örnekleri öğretici bakın.

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
