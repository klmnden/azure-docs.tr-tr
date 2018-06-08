---
title: Azure kapsayıcı örnekleri gruplarında çok kapsayıcı dağıtma
description: Azure kapsayıcı örnekleri birden çok kapsayıcı kapsayıcı grubuyla dağıtmayı öğrenin.
services: container-instances
author: neilpeterson
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: db3f616d85c21f01c751fd82532289593a6e7e45
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850578"
---
# <a name="deploy-a-container-group"></a>Kapsayıcı grubu dağıtma

Azure kapsayıcı örnekleri birden çok kapsayıcı kullanarak tek bir ana bilgisayar üzerine dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Bu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken bir hizmetin ikinci bir bağlı işlem nerede ihtiyaç yararlıdır.

Azure CLI kullanarak birden çok kapsayıcı grupları dağıtmak için iki yöntem vardır:

* Resource Manager şablonu dağıtımı (Bu makalede)
* [YAML dosya dağıtımı](container-instances-multi-container-yaml.md)

Ek Azure hizmet kaynakları (örneğin, bir Azure dosya paylaşımı) dağıtmak gerektiğinde bir Resource Manager şablonu ile dağıtım kapsayıcı örnek dağıtım zamanında önerilir. Dağıtımınızı içerdiğinde YAML formatı nedeniyle daha kısa yapısı, dağıtımı YAML dosya ile önerilir *yalnızca* kapsayıcı örnekleri.

> [!NOTE]
> Birden çok kapsayıcı grupları Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="configure-the-template"></a>Şablon yapılandırma

Bu makaledeki bölümler basit çok kapsayıcı sepet yapılandırma çalıştıran bir Azure Resource Manager şablonu dağıtarak yol.

Başlangıç adlı bir dosya oluşturarak `azuredeploy.json`, ardından aşağıdaki JSON dosyasını buraya kopyalayın.

Bu Resource Manager şablonunu iki kapsayıcı kapsayıcı grubuyla, bir ortak IP adresi ve iki gösterilen bağlantı noktalarını tanımlar. Gruptaki ilk kapsayıcı bir internet'e yönelik uygulama çalışır. İkinci kapsayıcı, sepet grubun yerel ağ aracılığıyla ana web uygulaması için bir HTTP isteği yapar.

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
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-04-01",
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

Bir özel kapsayıcı görüntü kayıt defterini kullanmak için JSON belgesi aşağıdaki biçime sahip bir nesne ekleyin. Bu yapılandırma örnek uygulaması için bkz: [ACI Resource Manager şablonu başvurusu] [ template-reference] belgeleri.

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

Şablonla dağıtmak [az grup dağıtımı oluşturmak] [ az-group-deployment-create] komutu.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
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
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Günlükleri görüntüleme

Kullanarak bir kapsayıcı günlük çıktısını görüntüleyin [az kapsayıcı günlükleri] [ az-container-logs] komutu. `--container-name` Bağımsız değişkeni günlüklerini kapsayıcıyı belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Çıktı:

```bash
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

```bash
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

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çok kapsayıcı Azure kapsayıcı örneği dağıtmak için gerekli olan adımları ele. Bir uçtan uca Azure kapsayıcı örnekleri deneyimi için Azure kapsayıcı örnekleri öğretici bakın.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
