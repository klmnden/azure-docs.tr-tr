---
title: Öğretici - Azure Container Instances - YAML içinde çok kapsayıcılı bir grup dağıtma
description: Bu öğreticide, Azure CLI ile bir YAML dosyası kullanarak birden çok kapsayıcı Azure Container ınstances'da bir kapsayıcı grubu dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.openlocfilehash: a0a91ece4f219cf822673cd457c064c326b89478
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59006177"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-yaml-file"></a>Öğretici: Bir YAML dosyası kullanarak çok kapsayıcılı bir grup dağıtma

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)
>

Azure Container Instances kullanarak tek bir konak üzerine birden çok kapsayıcı dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Kapsayıcı grubu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken hizmet ikinci bir bağlı işlem gereken yere yararlı olur.

Bu öğreticide, Azure CLI kullanarak bir YAML dosyası dağıtarak basit iki kapsayıcı sepet yapılandırma çalıştırmak için adımları izleyin. Bir YAML dosyası örneği ayarlarını belirtmek için bir kısa biçimi sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir YAML dosyası yapılandırma
> * Kapsayıcı grubu dağıtma
> * Kapsayıcı günlüklerini görüntüleme

> [!NOTE]
> Birden çok kapsayıcı grubunun şu anda Linux kapsayıcılarıyla kısıtlıdır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-yaml-file"></a>Bir YAML dosyası yapılandırma

Çok kapsayıcılı bir grup ile dağıtmak için [az kapsayıcı oluşturma] [ az-container-create] komutu Azure CLI'dan bir YAML dosyası içinde kapsayıcı grubu yapılandırmasını belirtmeniz gerekir. Ardından YAML dosyası komutuna parametre olarak geçirirsiniz.

Başlangıç adlı yeni bir dosyaya aşağıdaki YAML kopyalayarak **dağıtma aci.yaml**. Azure Cloud Shell'de, çalışma dizininizde dosya oluşturma için Visual Studio Code kullanabilirsiniz:

```
code deploy-aci.yaml
```

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

Özel kapsayıcı görüntüsünü kayıt defteri kullanmak için ekleme `imageRegistryCredentials` ortamınız için değiştirilmiş değerlere sahip kapsayıcı grubuna özelliği:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
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

## <a name="view-container-logs"></a>Kapsayıcı günlüklerini görüntüleme

Kullanarak bir kapsayıcının günlük çıktısını görüntülemek [az kapsayıcı günlüklerini] [ az-container-logs] komutu. `--container-name` Kapsayıcı günlüklerini çekme yapılacağı bağımsız değişken belirtir. Bu örnekte, `aci-tutorial-app` kapsayıcısı belirtildi.

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

Sepet kapsayıcı için günlükleri görmek için belirten benzer bir komut çalıştırın `aci-tutorial-sidecar` kapsayıcı.

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

Gördüğünüz gibi sepet düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden bir HTTP istek yapıyor. Bir HTTP yanıt kodu dışında aldıysanız, uyarı tetiklemek için bu sepet örneği genişletilemiyor `200 OK`.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container ınstances'da çok kapsayıcılı bir grup dağıtmak için bir YAML dosyası kullanılır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir YAML dosyası çok kapsayıcılı bir grup için yapılandırma
> * Kapsayıcı grubu dağıtma
> * Kapsayıcı günlüklerini görüntüleme

Kullanarak çok kapsayıcılı grup belirtebilirsiniz bir [Resource Manager şablonu](container-instances-multi-container-group.md). Kapsayıcı grubu ile ek Azure hizmet kaynaklarını dağıtmak için ihtiyacınız olduğunda Resource Manager şablonu kolayca senaryoları için uyarlanabilir.

<!-- LINKS - External -->


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
