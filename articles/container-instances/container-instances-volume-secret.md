---
title: "Azure kapsayıcı durumlarda gizli bir birim"
description: "Kapsayıcı örnekleri tarafından erişim için hassas bilgileri depolamak için gizli bir birim öğrenin"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 6f8e1b6faac11b668a143f8013a198831a428c51
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Azure kapsayıcı durumlarda gizli bir birim

Bağlama öğrenin bir *gizli* kapsayıcı örneklerinizi depolama ve alma kapsayıcı gruplarınızı kapsayıcılarında tarafından hassas bilgilerin biriminde.

> [!NOTE]
> Takma bir *gizli* birimdir Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="secret-volume"></a>Gizli birim

Kullanabileceğiniz bir *gizli* bir kapsayıcı grubu kapsayıcılarında hassas bilgiler sağlamak için birim. *Gizli* birim kapsayıcı grubunuzun kapsayıcılarında daha sonra erişebilirsiniz birimin içindeki dosyaları belirtilen sırlarınızı depolar. Gizli anahtarları kullanarak bir *gizli* birim, kaçının uygulama kodunuzda SSH anahtarları veya veritabanı kimlik bilgileri gibi hassas verileri yerleştirme.

Tüm *gizli* birimleri tarafından desteklenen [tmpfs][tmpfs], RAM destekli filesystem; içeriklerini hiçbir zaman geçici olmayan depolama alanına yazılır.

## <a name="mount-a-secret-volume"></a>Gizli bir birim

Bağlanacak bir *gizli* bir kapsayıcı örnek bir birimde dağıtmanız gerekir kullanarak bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümünü. İçinde istediğiniz bağlama kapsayıcı grubundaki her kapsayıcı için sonraki *gizli* birim, doldurmak `volumeMounts` içinde dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu tek bir kapsayıcının oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı başlatmalar bir *gizli* iki Base64 ile kodlanmış gizlilikleri oluşan birim.

[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Örnek bir Azure Resource Manager şablonu ile kapsayıcı örnek dağıtım görmek için bkz: [çok kapsayıcı grupları Azure kapsayıcı durumlarda dağıtmak](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı örnekleri diğer birim türlerinde bağlama öğrenin:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure kapsayıcı durumlarda emptyDir birim](container-instances-volume-emptydir.md)
* [Azure kapsayıcı durumlarda gitRepo birim](container-instances-volume-gitrepo.md)

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs