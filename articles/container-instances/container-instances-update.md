---
title: Azure Container ınstances'da kapsayıcı güncelleştir
description: Azure Container Instances'a kapsayıcı gruplarınızı içinde çalışan kapsayıcıları güncelleştirme hakkında bilgi edinin.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 08/01/2018
ms.author: danlep
ms.openlocfilehash: 2df6a2724cbdcd6bbb6c6ca6636256b7e399da8e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60686900"
---
# <a name="update-containers-in-azure-container-instances"></a>Azure Container ınstances'da kapsayıcı güncelleştir

Kapsayıcı örneklerinizin normal işlem sırasında bir kapsayıcı grubundaki kapsayıcı güncelleştirmek gerekli bulabilirsiniz. Örneğin, görüntü sürümü güncelleştirme, bir DNS adını değiştirmek, ortam değişkenlerini güncelleştirmeniz veya bir kapsayıcı, uygulaması kilitlenen durumunu yenilemek isteyebilir.

## <a name="update-a-container-group"></a>Bir kapsayıcı grubunu güncelleştir

Bir kapsayıcı grubundaki kapsayıcı değiştirilmiş en az bir özellik ile varolan bir grubu yeniden dağıtmaya gerek güncelleştirin. Kapsayıcı grubu güncelleştirdiğinizde, gruptaki tüm çalışan kapsayıcıları yeniden yerinde.

Create komutu (veya Azure portalını kullanın) vererek var olan bir kapsayıcı grubunu yeniden dağıtın ve mevcut bir grubun adını belirtin. Yeniden dağıtım tetiklemek için create komutu verdiğinizde grubunun en az bir geçerli özellik değiştirin. Tüm kapsayıcı grubu özelliklerini, yeniden dağıtım için geçerlidir. Bkz: [silme için gereken özellikleri](#properties-that-require-container-delete) desteklenmeyen özelliklerin bir listesi için.

Aşağıdaki Azure CLI örnek, bir kapsayıcı grubu yeni bir DNS ad etiketi ile güncelleştirir. DNS ad etiketi özelliği grubuna değiştiğinden kapsayıcı grubunun imzalanmasını ve kapsayıcılarında yeniden başlatıldı.

İlk dağıtımı DNS ad etiketi ile *myapplication hazırlama*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Kapsayıcı grubu ile yeni bir DNS ad etiketi, güncelleştirme *myapplication*:

```azurecli-interactive
# Update container group (restarts container)
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Güncelleştirme avantajları

Var olan bir kapsayıcı grubu güncelleştiriliyor birincil avantajı, daha hızlı dağıtımıdır. Var olan bir kapsayıcı grubunu yeniden dağıtırken, kapsayıcı görüntüsü katmanları arasından önceki bir dağıtım tarafından önbelleğe alınır. Tüm görüntü katmanlarını yeni bir kayıt defteri ile yeni dağıtımlar bitti olarak çekmek yerine, yalnızca değiştirilen katmanları (varsa) alınır.

Windows Server Core yerine güncelleştirdiğinizde dağıtım hızını önemli bir iyileştirme görebilirsiniz gibi daha büyük kapsayıcı görüntülerini temel alan uygulamaları silin ve yeni dağıtın.

## <a name="limitations"></a>Sınırlamalar

Bir kapsayıcı grubunun tüm özelliklerini güncelleştirmelerini destekler. Bir kapsayıcı grubunun bazı özelliklerini değiştirmek için önce silin, ardından gerekir grubuna yeniden dağıtın. Ayrıntılar için bkz [kapsayıcı gerektiren özellikleri Sil](#properties-that-require-container-delete).

Kapsayıcı grubu güncelleştirdiğinizde, bir kapsayıcı grubundaki tüm kapsayıcılar yeniden başlatılır. Bir güncelleştirme veya yerinde yeniden belirli bir kapsayıcı bir çoklu kapsayıcı grubunda gerçekleştirilemiyor.

Bir kapsayıcının IP adresi arasında güncelleştirmeleri genellikle değiştirmez, ancak aynı kalması garantili. Kapsayıcı grubu, aynı temel alınan ana bilgisayarına dağıtılır sürece, kapsayıcı grubu IP adresini tutar. Ender olsa ve Azure Container Instances aynı ana bilgisayara dağıtmanız için her türlü çabayı yaparken, farklı bir konağa yeniden dağıtma işlemi neden olabilecek bazı Azure iç olaylar vardır. Bu sorunu gidermek için her zaman kapsayıcı örneklerinizin bir DNS ad etiketi kullanın.

Sonlandırılan ya da silinen kapsayıcı grubu güncelleştirilemiyor. Kapsayıcı grubu durdurulduğunda (bulunduğu *kesildi* durumu) veya silindi, gruba yeni olarak dağıtılır.

## <a name="properties-that-require-container-delete"></a>Kapsayıcı için gereken özellikleri Sil

Daha önce bahsedildiği gibi tüm kapsayıcı grubu özelliklerini güncelleştirilebilir. Örneğin, yeniden başlatma ilkesi kapsayıcı ya da bağlantı noktalarını değiştirmek için önce kapsayıcı grubunu silin, ardından gerekir yeniden oluşturun.

Kapsayıcı grubu silme işlemini yeniden dağıtım öncesinde bu özellikleri gerektirir:

* İşletim sistemi türü
* CPU
* Bellek
* Yeniden başlatma ilkesi
* Bağlantı Noktaları

Bir kapsayıcı grubunu silin ve yeniden oluşturun, bunu değil "imzalanmasını" olsa da, yeni oluşturulan. Tüm görüntü katmanlarını yeni alınır kayıt defterinden, değil, önceki bir dağıtım tarafından önbelleğe. Kapsayıcının IP adresini de temel alınan farklı bir konağa dağıtılan nedeniyle değişebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede birkaç kez olduğundan belirtilen **kapsayıcı grubu**. Her kapsayıcıyı Azure Container ınstances'da bir kapsayıcı grubunda dağıtılır ve kapsayıcı grupları, birden fazla kapsayıcı içerebilir.

[Azure Container Instances’taki kapsayıcı grupları](container-instances-container-groups.md)

[Çok kapsayıcılı bir grup dağıtma](container-instances-multi-container-group.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
