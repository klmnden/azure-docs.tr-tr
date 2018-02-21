---
title: "GitRepo birim Azure kapsayıcı örnekleri"
description: "Kapsayıcı örneklerinizi bir Git deposuna kopyalamak için bir gitRepo birim öğrenin"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
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

## <a name="mount-a-gitrepo-volume"></a>GitRepo birim

Bağlanacak bir *gitRepo* bir kapsayıcı örnek bir birimde dağıtmanız gerekir kullanarak bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümünü. İçinde istediğiniz bağlama kapsayıcı grubundaki her kapsayıcı için sonraki *gitRepo* birim, doldurmak `volumeMounts` içinde dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu tek bir kapsayıcının oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı tarafından belirtilen iki GitHub depolarının klonlar *gitRepo* birim bloklarında. İkinci birim için kopyalamak için bir dizin belirten ek özellikler ve kopyalamak için belirli bir düzeltmesini yürütme karmasını içerir.

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
