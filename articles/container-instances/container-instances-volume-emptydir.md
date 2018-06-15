---
title: Azure kapsayıcı durumlarda emptyDir birim
description: Azure kapsayıcı örnekleri kapsayıcı grubunda kapsayıcılarında arasında veri paylaşımı için bir emptyDir birim öğrenin
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 89289a7a0bb5c486c662d528c5014bdbd8eebaca
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32164899"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Azure kapsayıcı durumlarda emptyDir birim

Bağlama öğrenin bir *emptyDir* birim kapsayıcıları Azure kapsayıcı örnekleri kapsayıcı grubunda arasında veri paylaşır.

> [!NOTE]
> Takma bir *emptyDir* birimdir Linux kapsayıcılara şu anda kısıtlı. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışmamız esnasında, geçerli platform farklılıklarını [Azure Kapsayıcı Örnekleri için kotalar ve bölge kullanılabilirliği](container-instances-quotas.md) bölümünde bulabilirsiniz.

## <a name="emptydir-volume"></a>emptyDir birim

*EmptyDir* birim kapsayıcı grubundaki her kapsayıcı için erişilebilir bir yazılabilir dizin sağlar. Grup kapsayıcılarında okuma ve birimin aynı dosyaları yazma ve her bir kapsayıcıdaki aynı veya farklı yollarını kullanarak bağlanabilir.

Bazı örnek kullanan bir *emptyDir* birimi:

* Boş alan
* Uzun süre çalışan görevler sırasında denetim noktası oluşturma
* Sepet kapsayıcı tarafından alınır ve bir uygulama kapsayıcısı tarafından sunulan veri depolama

Verileri bir *emptyDir* birim kapsayıcı çökme (Crash) kalıcıdır. Yeniden başlatılır kapsayıcıları ancak garanti edilmez verilerde sürdürmek için bir *emptyDir* birim.

## <a name="mount-an-emptydir-volume"></a>Bir emptyDir birim

EmptyDir birim kapsayıcı örneğinde kullanarak dağıtmanız gerekir bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümünü. İçinde istediğiniz bağlama kapsayıcı grubundaki her kapsayıcı için sonraki *emptyDir* birim, doldurmak `volumeMounts` içinde dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu iki kapsayıcıları için oluşan bir kapsayıcı grubu oluşturur her hangi başlatmalar *emptyDir* birimi:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Örnek bir Azure Resource Manager şablonu ile kapsayıcı örnek dağıtım görmek için bkz: [çok kapsayıcı grupları Azure kapsayıcı durumlarda dağıtmak](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure kapsayıcı örnekleri diğer birim türlerinde bağlama öğrenin:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure kapsayıcı durumlarda gitRepo birim](container-instances-volume-gitrepo.md)
* [Azure kapsayıcı durumlarda gizli bir birim](container-instances-volume-secret.md)