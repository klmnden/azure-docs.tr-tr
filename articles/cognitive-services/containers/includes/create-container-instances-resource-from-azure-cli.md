---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure CLI üzerinden bir Azure container örneği kaynak oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: 5e7a3d849f726ae4dbbd559d541464404e427775
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711741"
---
## <a name="create-an-azure-container-instance-resource-from-the-azure-cli"></a>Azure CLI üzerinden bir Azure Container Instance kaynağı oluşturun

Azure Container Instance kaynak aşağıdaki YAML tanımlar. Adlı yeni bir dosyaya içeriği yapıştırın `my-aci.yaml` ve açıklamalı değerleri kendi değerlerinizle değiştirin. Başvuru [şablon biçimine] [şablon formant] geçerli YAML için. Başvurmak [kapsayıcı depoları ve görüntüleri][repositories-and-images] için kullanılabilir görüntü adlarını ve bunların karşılık gelen bir depo.

```YAML
apiVersion: 2018-10-01
location: # < Valid location >
name: # < Container Group name >
imageRegistryCredentials:
  - server: containerpreview.azurecr.io
    username: # < The username for the preview container registry >
    password: # < The password for the preview container registry >
properties:
  containers:
  - name: # < Container name >
    properties:
      image: # < Repository/Image name >
      environmentVariables: # These env vars are required
        - name: eula
          value: accept
        - name: billing
          value: # < Service specific Endpoint URL >
        - name: apikey
          value: # < Service specific API key >
      resources:
        requests:
          cpu: 4 # Always refer to recommended minimal resources
          memoryInGb: 8 # Always refer to recommended minimal resources
      ports:
        - port: 5000
  osType: Linux
  restartPolicy: OnFailure
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: 5000
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

> [!NOTE]
> Tüm konumları aynı CPU ve bellek kullanılabilirlik vardır. Başvurmak [konum ve kaynakları][location-to-resource] konumu ve işletim sistemi başına kapsayıcılar için kullanılabilir kaynaklar listesi için tablo.

YAML dosyası için oluşturduğumuz bağımlı olduğumuz [ `az container create` ][azure-container-create] komutu. Azure CLI'dan yürütme `az container create` komut değiştirerek `<resource-group>` ile kendi. Bir YAML içinde değerleri güvenliğini sağlamak için dağıtım ek olarak, başvuru [güvenli değerleri][secure-values].

```azurecli
az container create -g <resource-group> -f my-aci.yaml
```

Komut çıktısı `Running...` geçerli süre sonra çıktı yeni oluşturulan ACI kaynağını temsil eden bir JSON dizesine değişip değişmediğini. Kapsayıcı görüntüsünü, büyük olasılıkla birden fazla bir süredir kullanılamaz, ancak kaynak artık dağıtılan değildir.

> [!TIP]
> YAML gerektiğinde konumu ile eşleşmesi için uygun şekilde ayarlanması Kapat genel önizlemede Azure Bilişsel hizmet teklifleri konumlarını dikkat edin.

[azure-container-create]: https://docs.microsoft.com/cli/azure/container?view=azure-cli-latest#az-container-create
[template-format]: https://docs.microsoft.com/azure/templates/Microsoft.ContainerInstance/2018-10-01/containerGroups#template-format

[repositories-and-images]: ../../cognitive-services-container-support.md#container-repositories-and-images
[location-to-resource]: ../../../container-instances/container-instances-region-availability.md#availability---general
[secure-values]: ../../../container-instances/container-instances-environment-variables.md#secure-values