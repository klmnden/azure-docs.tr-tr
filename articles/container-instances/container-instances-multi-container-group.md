---
title: "Azure kapsayıcı örnekleri gruplarında çok kapsayıcı dağıtma"
description: "Azure kapsayıcı örnekleri birden çok kapsayıcı kapsayıcı grubuyla dağıtmayı öğrenin."
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5e1f23e20b001404d3f781e7e6deac87ede12684
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-a-container-group"></a>Kapsayıcı grubu dağıtma

Azure kapsayıcı örnekleri birden çok kapsayıcı kullanarak tek bir ana bilgisayar üzerine dağıtımını destekleyen bir *kapsayıcı grubu*. Bu günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken bir hizmetin ikinci bir bağlı işlem nerede ihtiyaç yararlıdır.

Bu belge, Azure Resource Manager şablonu kullanarak basit çok kapsayıcı sepet yapılandırma çalıştıran anlatılmaktadır.

## <a name="configure-the-template"></a>Şablon yapılandırma

Adlı bir dosya oluşturun `azuredeploy.json` ve aşağıdaki json dosyasını buraya kopyalayın.

Bu örnekte, bir kapsayıcı grubu iki kapsayıcıları ve genel bir IP adresi ile tanımlanır. İlk kapsayıcı grubunun Internet karşılıklı uygulamasını çalıştırır. İkinci kapsayıcı, sepet grubun yerel ağ aracılığıyla ana web uygulaması için bir HTTP isteği yapar.

Bu sepet örnek, bir HTTP yanıt kodu 200 dışında Tamam aldıysanız, bir uyarıyı tetiklemek için genişletilemiyor.

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

Bir özel kapsayıcı görüntü kayıt defterini kullanmak için json belgesi aşağıdaki biçime sahip bir nesne ekleyin.

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

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Şablonla dağıtmak [az grup dağıtımı oluşturmak](/cli/azure/group/deployment#create) komutu.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Birkaç saniye içinde Azure'dan ilk yanıt alırsınız.

## <a name="view-deployment-state"></a>Dağıtım durumunu görüntüle

Dağıtım durumunu görüntülemek için kullanın `az container show` komutu. Bu uygulama erişilebilen sağlanan genel IP adresini döndürür.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Çıktı:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Günlükleri görüntüle

Kullanarak bir kapsayıcı günlük çıktısını görüntüleyin `az container logs` komutu. `--container-name` Bağımsız değişkeni günlüklerini kapsayıcıyı belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Çıktı:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Yan araba kapsayıcısı için günlükleri görmek için ikinci kapsayıcı adı belirterek aynı komutu çalıştırın.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Çıktı:

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

Gördüğünüz gibi sepet bir HTTP isteği düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden yapılmasıdır.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede bir çok kapsayıcı Azure kapsayıcı örneği dağıtmak için gerekli olan adımları ele. Bir uçtan uca Azure kapsayıcı örnekleri deneyimi için Azure kapsayıcı örnekleri öğretici bakın.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri öğretici]:./container-instances-tutorial-prepare-app.md
