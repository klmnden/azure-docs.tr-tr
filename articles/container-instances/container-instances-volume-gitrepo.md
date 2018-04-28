---
title: GitRepo birim Azure kapsayıcı örnekleri
description: Kapsayıcı örneklerinizi bir Git deposuna kopyalamak için bir gitRepo birim öğrenin
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/16/2018
ms.author: marsma
ms.openlocfilehash: e40d841c07534c9c0074c038d1e3c6e435265564
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Azure kapsayıcı durumlarda gitRepo birim

Bağlama öğrenin bir *gitRepo* kapsayıcı örneklerinizi bir Git deposuna kopyalamak için birim.

> [!NOTE]
> Takma bir *gitRepo* birimdir Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="gitrepo-volume"></a>gitRepo birim

*GitRepo* birim bir dizin bağlar ve belirtilen Git deposu içine kapsayıcı başlangıçta klonlar. Kullanarak bir *gitRepo* kapsayıcı örneklerinizi bir birimde kaçının uygulamalarınızda bunu kod ekleme.

Bağlama ne zaman bir *gitRepo* birimi, birim yapılandırmak için üç özelliklerini ayarlayabilirsiniz:

| Özellik | Gerekli | Açıklama |
| -------- | -------- | ----------- |
| `repository` | Evet | Tam URL dahil olmak üzere `http://` veya `https://`, kopyalanma Git depo.|
| `directory` | Hayır | Dizin depo kopyalanma. Yolun gerekir değil içeremez veya ile başlayamaz "`..`".  Belirtirseniz "`.`", depo birimin dizinine kopyalanamıyor. Aksi halde, bir alt birimi dizini içinde verilen ad içine Git deposu kopyalandı. |
| `revision` | Hayır | Kopyalanacak düzeltme yürütme karmasını. Belirtilmezse, `HEAD` düzeltme kopyalanamıyor. |

## <a name="mount-gitrepo-volume-azure-cli"></a>Bağlama gitRepo birimi: Azure CLI

Kapsayıcı örnekleriyle dağıttığınızda gitRepo birim bağlamak için [Azure CLI](/cli/azure), sağlar `--gitrepo-url` ve `--gitrepo-mount-path` parametreleri [az kapsayıcı oluşturmak] [ az-container-create] komutu. İsteğe bağlı olarak içine kopyalamak için birimi dizininde belirtebilirsiniz (`--gitrepo-dir`) ve kopyalanma düzeltme yürütme karmasını (`--gitrepo-revision`).

Bu örnek komut klonlar [aci helloworld] [ aci-helloworld] örnek uygulamaya `/mnt/aci-helloworld` kapsayıcı örneği içinde:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image microsoft/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

GitRepo birimin bağlı doğrulamak için kapsayıcıyla kabuğunda başlatma [az kapsayıcı exec] [ az-container-exec] ve dizin listesinde:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Bağlama gitRepo birimi: Kaynak Yöneticisi

Kapsayıcı örnekleriyle dağıttığınızda gitRepo birim bağlamak için bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups), ilk doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümünü. Ardından, içinde gibi bağlamak kapsayıcı grubundaki her kapsayıcı için *gitRepo* birim, doldurmak `volumeMounts` içinde dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu tek bir kapsayıcının oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı tarafından belirtilen iki GitHub depolarının klonlar *gitRepo* birim bloklarında. İkinci birim için kopyalamak için bir dizin belirten ek özellikler ve kopyalamak için belirli bir düzeltmesini yürütme karmasını içerir.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Önceki şablonda tanımlanan iki kopyalanan depoları elde edilen dizin yapısını şöyledir:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Örnek bir Azure Resource Manager şablonu ile kapsayıcı örnek dağıtım görmek için bkz: [çok kapsayıcı grupları Azure kapsayıcı durumlarda dağıtmak](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı örnekleri diğer birim türlerinde bağlama öğrenin:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure kapsayıcı durumlarda gizli bir birim](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec