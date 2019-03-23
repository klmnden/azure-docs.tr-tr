---
title: Azure Container ınstances'da bir gizli birimi
description: Kapsayıcı örneklerinizin erişim için hassas bilgileri depolamak için gizli bir birim bağlama hakkında bilgi edinin
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: danlep
ms.openlocfilehash: 3c1c83bb0c3e46a7eaab519050d9c556e2cc1a7a
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372293"
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Azure Container ınstances'da bir gizli birimi

Kullanım bir *gizli* kapsayıcı grubundaki kapsayıcı için hassas bilgileri sağlamak için birim. *Gizli* birim kapsayıcı grubundaki kapsayıcı tarafından erişilebilen birimin içindeki dosyaları dizilerinizin depolar. Gizli dizileri depolayarak bir *gizli* birimi önlemek uygulama kodunuz için SSH anahtarları veya veritabanı kimlik bilgileri gibi hassas verileri ekleme.

Tüm *gizli* birimleri tarafından desteklenen [tmpfs][tmpfs], RAM destekli filesystem; içerikleri, hiçbir zaman geçici olmayan depolama alanına yazılır.

> [!NOTE]
> *Gizli dizi* birimleri Linux kapsayıcıları için şu anda kısıtlı. Hem Windows hem de Linux kapsayıcıları için güvenli bir ortam değişkenlerini geçirin öğrenin [ortam değişkenlerini ayarlama](container-instances-environment-variables.md). Tüm özellikleri Windows kapsayıcılarına getirmek için çalışıyoruz, ancak geçerli platform farklılıklarını içinde bulabilirsiniz [kotaları ve Azure Container Instances için bölge kullanılabilirliği](container-instances-quotas.md).

## <a name="mount-secret-volume---azure-cli"></a>Gizli birimi - Azure CLI

Azure CLI kullanarak bir veya daha fazla gizli bir kapsayıcıyı dağıtmak için dahil `--secrets` ve `--secrets-mount-path` parametrelerinde [az kapsayıcı oluşturma] [ az-container-create] komutu. Bu örnekte bağlar bir *gizli* adresindeki "mysecret1" ve "mysecret2," olmak üzere iki gizli dizileri içeren toplu `/mnt/secrets`:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name secret-volume-demo \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --secrets mysecret1="My first secret FOO" mysecret2="My second secret BAR" \
    --secrets-mount-path /mnt/secrets
```

Aşağıdaki [az container exec] [ az-container-exec] çıktısı, içinde çalışmakta olan kapsayıcıyı bir kabuk açarak, gizli birimin içindeki dosyaları listelemenin ve ardından içeriklerini görüntüleme göstermektedir:

```console
$ az container exec --resource-group myResourceGroup --name secret-volume-demo --exec-command "/bin/sh"
/usr/src/app # ls -1 /mnt/secrets
mysecret1
mysecret2
/usr/src/app # cat /mnt/secrets/mysecret1
My first secret FOO
/usr/src/app # cat /mnt/secrets/mysecret2
My second secret BAR
/usr/src/app # exit
Bye.
```

## <a name="mount-secret-volume---yaml"></a>Gizli birimi - YAML

Azure CLI ile kapsayıcı grupları da dağıtım yapabilirsiniz ve [YAML şablonu](container-instances-multi-container-yaml.md). YAML şablonu tarafından dağıtma tercih edilen kapsayıcı grupları birden çok kapsayıcılardan oluşan dağıtırken yöntemidir.

Bir YAML şablonu ile dağıttığınızda, gizli anahtar değerleri olmalıdır **Base64 kodlamalı** şablondaki. Ancak, düz metin kapsayıcıdaki dosyaları içinde gizli değerleri görüntülenir.

Aşağıdaki YAML şablonu bağlar bir kapsayıcısını bir kapsayıcı grubu tanımlayan bir *gizli* birim `/mnt/secrets`. İki gizli dizileri, "mysecret1" ve "mysecret2." gizli birimi içeriyor

```yaml
apiVersion: '2018-06-01'
location: eastus
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

YAML şablonu ile dağıtmak için önceki YAML adlı bir dosyaya kaydedin `deploy-aci.yaml`, ardından yürütme [az kapsayıcı oluşturma] [ az-container-create] komutunu `--file` parametresi:

```azurecli-interactive
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

## <a name="mount-secret-volume---resource-manager"></a>Gizli birimi - Resource Manager bağlama

CLI ve YAML dağıtım yanı sıra Azure kullanarak kapsayıcı grubunu dağıtabileceğiniz [Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümü. Resource Manager şablonu ile dağıttığınızda, gizli anahtar değerleri olmalıdır **Base64 kodlamalı** şablondaki. Ancak, düz metin kapsayıcıdaki dosyaları içinde gizli değerleri görüntülenir.

İçinde istediğiniz bağlamak kapsayıcı grubundaki her kapsayıcı için sonraki *gizli* birim doldurmak `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü.

Aşağıdaki Resource Manager şablonu bağlar bir kapsayıcısını bir kapsayıcı grubu tanımlayan bir *gizli* birim `/mnt/secrets`. İki gizli dizileri, "mysecret1" ve "mysecret2." gizli birimi içeriyor

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->
[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Resource Manager şablonu ile dağıtmak için önceki JSON adlı bir dosyaya Kaydet `deploy-aci.json`, ardından yürütme [az grubu dağıtımı oluşturmak] [ az-group-deployment-create] komutunu `--template-file` parametresi:

```azurecli-interactive
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```

## <a name="next-steps"></a>Sonraki adımlar

### <a name="volumes"></a>Birimler

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure Container ınstances'da bir gitRepo birimi](container-instances-volume-gitrepo.md)

### <a name="secure-environment-variables"></a>Güvenli bir ortam değişkenleri

Kullanarak kapsayıcıları (Windows kapsayıcıları dahil) için hassas bilgileri sağlamak için başka bir yöntem olan [güvenli ortam değişkenlerini](container-instances-environment-variables.md#secure-values).

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
