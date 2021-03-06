---
title: Azure Container ınstances'da bir emptyDir birim bağlama
description: Azure Container ınstances'da bir kapsayıcı grubundaki kapsayıcı arasında veri paylaşımı için bir emptyDir birim bağlama hakkında bilgi edinin
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: danlep
ms.openlocfilehash: d91706da898e84effc6194a74dce69a66be0f4ac
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657625"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Azure Container ınstances'da bir emptyDir birim bağlama

Bağlama öğrenin bir *emptyDir* Azure Container ınstances'da bir kapsayıcı grubundaki kapsayıcı arasında veri paylaşımı için birim.

> [!NOTE]
> Bağlama bir *emptyDir* birim şu anda Linux kapsayıcıları için kısıtlanmış. Tüm özellikleri Windows kapsayıcılarına getirmek için çalışıyoruz, ancak geçerli platform farklılıklarını içinde bulabilirsiniz [genel bakış](container-instances-overview.md#linux-and-windows-containers).

## <a name="emptydir-volume"></a>emptyDir birim

*EmptyDir* birim kapsayıcı grubundaki her kapsayıcı için erişilebilir yazılabilir bir dizin sağlar. Grup kapsayıcılarında okuyup biriminde aynı dosyaları yazabilir ve her bir kapsayıcıdaki aynı veya farklı yolları kullanarak bağlanabilir.

Bazı örnek kullanım bir *emptyDir* birim:

* Boş alan
* Uzun süre çalışan görevleri sırasında denetim noktası oluşturma
* Sepet kapsayıcısı tarafından alınır ve bir uygulama kapsayıcısı tarafından sunulan data Store

Verileri bir *emptyDir* birim kapsayıcı kilitlenmeleri kalıcıdır. Kapsayıcılar yeniden başlatılır, ancak garanti edilmez verileri kalıcı hale getirmek için bir *emptyDir* birim.

## <a name="mount-an-emptydir-volume"></a>EmptyDir birim bağlama

Bir kapsayıcı örneğine bir emptyDir birimi bağlamak için kullanarak dağıtmalısınız bir [Azure Resource Manager şablonu](/azure/templates/microsoft.containerinstance/containergroups).

İlk olarak, doldurmak `volumes` kapsayıcı grubu dizisinde `properties` şablon bölümü. İçinde istediğiniz bağlamak kapsayıcı grubundaki her kapsayıcı için sonraki *emptyDir* birim doldurmak `volumeMounts` içindeki dizi `properties` kapsayıcı tanımının bölümü.

Örneğin, aşağıdaki Resource Manager şablonu oluşan iki kapsayıcı bir kapsayıcı grubu oluşturur her hangi takar *emptyDir* birim:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Kapsayıcı örneği dağıtımıyla bir Azure Resource Manager şablonu ile bir örneğini görmek için bkz: [Azure Container ınstances'da çok kapsayıcılı grupları dağıtma](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure Container ınstances'da diğer birim türleri bağlama işlemleri gerçekleştirmeyi öğreneceksiniz:

* [Azure kapsayıcı durumlarda bir Azure dosya paylaşımını bağlama](container-instances-volume-azure-files.md)
* [Azure Container ınstances'da bir gitRepo birimi](container-instances-volume-gitrepo.md)
* [Azure Container ınstances'da bir gizli birimi](container-instances-volume-secret.md)