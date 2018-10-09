---
title: Azure Container ınstances'da çok kapsayıcılı grupları dağıtma
description: Birden çok kapsayıcı Azure Container ınstances'da bir kapsayıcı grubu dağıtmayı öğrenin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: adb284772291dc901dd5302124982948c1f37eea
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856488"
---
# <a name="deploy-a-container-group"></a>Kapsayıcı grubu dağıtma

Azure Container Instances kullanarak tek bir konak üzerine birden çok kapsayıcı dağıtımını destekleyen bir [kapsayıcı grubu](container-instances-container-groups.md). Günlüğe kaydetme, izleme veya başka bir yapılandırma için bir uygulama sepet oluştururken hizmet ikinci bir bağlı işlem gereken yere kullanışlıdır.

Azure CLI kullanarak çok kapsayıcılı grupları dağıtma için iki yöntem vardır:

* Resource Manager şablon dağıtımı (Bu makale)
* [YAML dosyası dağıtım](container-instances-multi-container-yaml.md)

Ek Azure hizmet kaynakları (örneğin, bir Azure dosya paylaşımı) dağıtmak gerektiğinde dağıtım bir Resource Manager şablonu ile kapsayıcı örneği dağıtımıyla zamanında önerilir. Dağıtımınız varsa YAML formatın daha kısa niteliği nedeniyle dağıtım ile bir YAML dosyası önerilir *yalnızca* kapsayıcı örnekleri.

> [!NOTE]
> Birden çok kapsayıcı grubunun şu anda Linux kapsayıcılarıyla kısıtlıdır. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="configure-the-template"></a>Şablon yapılandırma

Bu makaledeki bölümler, basit bir çoklu kapsayıcı sepet yapılandırma çalıştıran bir Azure Resource Manager şablonu dağıtarak yol.

Adlı bir dosya oluşturarak başlayın `azuredeploy.json`, içine aşağıdaki JSON kopyalayın.

Bu Resource Manager şablonu, bir kapsayıcı grubu iki kapsayıcı, bir genel IP adresi ve iki kullanıma sunulan bağlantı noktası tanımlar. İlk kapsayıcı grubunda Internet'e yönelik bir uygulama çalıştırır. İkinci kapsayıcıyı, sepet, grubun yerel ağ aracılığıyla ana web uygulamaya bir HTTP isteği yapar.

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
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Günlükleri görüntüleme

Kullanarak bir kapsayıcının günlük çıktısını görüntülemek [az kapsayıcı günlüklerini] [ az-container-logs] komutu. `--container-name` Kapsayıcı günlüklerini çekme yapılacağı bağımsız değişken belirtir. Bu örnekte, ilk kapsayıcı belirtilir.

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

Yan araba kapsayıcı için günlükleri görmek için ikinci kapsayıcının adını belirterek aynı komutu çalıştırın.

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

Gördüğünüz gibi sepet düzenli aralıklarla çalıştığından emin olmak için ana web uygulamasına grubun yerel ağ üzerinden bir HTTP istek yapıyor. Bir HTTP yanıt kodu 200 dışında Tamam aldığında, bir uyarı tetiklemek için bu sepet örneği genişletilemiyor.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir çoklu kapsayıcı Azure container örneği dağıtmak için gerekli olan adımları ele. Uçtan uca Azure Container Instances deneyimi için Azure Container Instances Öğreticisine bakın.

> [!div class="nextstepaction"]
> [Azure Container Instances öğreticisi][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
