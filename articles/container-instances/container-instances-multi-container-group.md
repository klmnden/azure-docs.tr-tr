---
title: "Azure kapsayıcı örnekleri gruplarında çok kapsayıcı dağıtma"
description: "Azure kapsayıcı örnekleri birden çok kapsayıcı kapsayıcı grubuyla dağıtmayı öğrenin."
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: dc1bd6502a5362bebd845f3938ab6502e0d91c74
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/20/2017
---
# <a name="deploy-a-container-group"></a>Kapsayıcı grubu dağıtma

Azure kapsayıcı örnekleri birden çok kapsayıcı kullanarak tek bir ana bilgisayar üzerine dağıtımını destekleyen bir *kapsayıcı grubu*. Bu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken bir hizmetin ikinci bir bağlı işlem nerede ihtiyaç yararlıdır.

Bu belge, Azure Resource Manager şablonu dağıtarak basit çok kapsayıcı sepet yapılandırmasını çalıştırma açıklanmaktadır.

## <a name="configure-the-template"></a>Şablon yapılandırma

Adlı bir dosya oluşturun `azuredeploy.json` ve aşağıdaki JSON dosyasını buraya kopyalayın.

Bu örnekte, bir kapsayıcı grubu iki kapsayıcıları ve genel bir IP adresi ile tanımlanır. Gruptaki ilk kapsayıcı bir internet'e yönelik uygulama çalışır. İkinci kapsayıcı, sepet grubun yerel ağ aracılığıyla ana web uygulaması için bir HTTP isteği yapar.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
    "resources": [
      {
        "name": "myContainerGroup",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2017-08-01-preview",
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
              }
            ]
          }
        }
      }
    ],
    "outputs": {
      "containerIPv4Address": {
        "type": "string",
        "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
      }
    }
  }
```

Bir özel kapsayıcı görüntü kayıt defterini kullanmak için JSON belgesi aşağıdaki biçime sahip bir nesne ekleyin.

```json
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
az group deployment create --resource-group myResourceGroup --name myContainerGroup --template-file azuredeploy.json
```

Birkaç saniye içinde Azure'dan ilk yanıt almanız gerekir.

## <a name="view-deployment-state"></a>Dağıtım durumunu görüntüle

Dağıtım durumunu görüntülemek için kullanın [az kapsayıcı Göster] [ az-container-show] komutu. Bu uygulama tarafından erişilebilen sağlanan genel IP adresini döndürür.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Çıktı:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Günlükleri görüntüle

Kullanarak bir kapsayıcı günlük çıktısını görüntüleyin [az kapsayıcı günlükleri] [ az-container-logs] komutu. `--container-name` Bağımsız değişkeni günlüklerini kapsayıcıyı belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Çıktı:

```bash
listening on port 80
::1 - - [18/Dec/2017:21:31:08 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [18/Dec/2017:21:31:11 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [18/Dec/2017:21:31:15 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Yan araba kapsayıcısı için günlükleri görmek için ikinci kapsayıcı adı belirterek aynı komutu çalıştırın.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Çıktı:

```bash
Every 3s: curl -I http://localhost                          2017-12-18 23:19:34

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
Date: Mon, 18 Dec 2017 23:19:34 GMT
Connection: keep-alive
```

Gördüğünüz gibi sepet bir HTTP isteği düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden yapılmasıdır. Bu sepet örnek, bir HTTP yanıt kodu 200 dışında Tamam aldıysanız, bir uyarıyı tetiklemek için genişletilemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çok kapsayıcı Azure kapsayıcı örneği dağıtmak için gerekli olan adımları ele. Bir uçtan uca Azure kapsayıcı örnekleri deneyimi için Azure kapsayıcı örnekleri öğretici bakın.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri öğretici]: container-instances-tutorial-prepare-app.md

<!-- LINKS - Internal -->
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
