---
title: Öğretici - Azure Container ınstances'da - çok kapsayıcılı bir grup dağıtma şablonu
description: Bu öğreticide, Azure CLI ile bir Azure Resource Manager şablonu kullanarak birden çok kapsayıcı Azure Container ınstances'da bir kapsayıcı grubu dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f769beda1654dc9f58ecff733741fb1ab9118031
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59006908"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>Öğretici: Resource Manager şablonu kullanarak çok kapsayıcılı bir grup dağıtma

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)

Azure Container Instances kullanarak tek bir konak üzerine birden çok kapsayıcı dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Kapsayıcı grubu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken hizmet ikinci bir bağlı işlem gereken yere yararlı olur.

Bu öğreticide, Azure CLI kullanarak bir Azure Resource Manager şablonu dağıtarak basit iki kapsayıcı sepet yapılandırma çalıştırmak için adımları izleyin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir çok kapsayıcılı grup şablonu yapılandırma
> * Kapsayıcı grubu dağıtma
> * Kapsayıcı günlüklerini görüntüleme

Kapsayıcı grubu ek Azure hizmeti kaynaklarına (örneğin, bir Azure dosya paylaşımı veya bir sanal ağ) dağıtmak için ihtiyacınız olduğunda Resource Manager şablonu kolayca senaryoları için uyarlanabilir. 

> [!NOTE]
> Birden çok kapsayıcı grubunun şu anda Linux kapsayıcılarıyla kısıtlıdır. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>Bir şablonu yapılandırma

Başlangıç adlı yeni bir dosya aşağıdaki JSON kopyalayarak `azuredeploy.json`. Azure Cloud Shell'de, çalışma dizininizde dosya oluşturma için Visual Studio Code kullanabilirsiniz:

```
code azuredeploy.json
```

Bu Resource Manager şablonu, bir kapsayıcı grubu iki kapsayıcı, bir genel IP adresi ve iki kullanıma sunulan bağlantı noktası tanımlar. İlk kapsayıcı grubunda bir internet'e yönelik web uygulaması çalışır. İkinci kapsayıcıyı, sepet, grubun yerel ağ aracılığıyla ana web uygulamaya bir HTTP isteği yapar.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Özel kapsayıcı görüntüsünü kayıt defteri kullanmak için JSON belgesi aşağıdaki biçime sahip bir nesne ekleyin. Bu yapılandırma için bir örnek uygulaması, bakın [ACI Resource Manager şablon başvurusu] [ template-reference] belgeleri.

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>Şablonu dağıtma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Şablon ile dağıtım [az grubu dağıtım oluşturma] [ az-group-deployment-create] komutu.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
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

```bash
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

```bash
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

Bu öğreticide, Azure Container ınstances'da çok kapsayıcılı bir grup dağıtmak için bir Azure Resource Manager şablonu kullanılır. Şunları öğrendiniz:

> [!div class="checklist"]
> * Bir çok kapsayıcılı grup şablonu yapılandırma
> * Kapsayıcı grubu dağıtma
> * Kapsayıcı günlüklerini görüntüleme

Ek şablon örnekleri için bkz [Azure Container Instances için Azure Resource Manager şablonları](container-instances-samples-rm.md).

Kullanarak çok kapsayıcılı grup belirtebilirsiniz bir [YAML dosyası](container-instances-multi-container-yaml.md). Dağıtımınız yalnızca container Instances içerdiğinde YAML formatın daha kısa niteliği nedeniyle bir YAML dosyası ile dağıtımı iyi bir seçimdir.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
