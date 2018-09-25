---
title: Azure CLI ile paylaşılan VM görüntüleri oluşturma | Microsoft Docs
description: Bu makalede, Azure'da bir sanal makinenin paylaşılan bir görüntü oluşturmak için Azure CLI'yı kullanmayı öğrenin.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: axayjo
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/19/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 2f8ddae05b97b3fa6f1d3f65681defdd4e5a38b7
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048394"
---
# <a name="preview-create-a-shared-image-gallery-with-the-azure-cli"></a>Önizleme: Azure CLI ile bir paylaşılan görüntü Galerisi oluşturma

Paylaşılan görüntü Galerisi, kuruluşunuz genelinde paylaşımı özel görüntü büyük ölçüde kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır.

Bu makalede, kendi galerisini ve özel görüntüleri bir Azure sanal makine oluşturun. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Paylaşılan görüntü Galerisi oluşturma
> * Paylaşılan görüntü tanımı oluşturma
> * Paylaşılan görüntü sürümü oluşturma
> * Paylaşılan bir görüntüden VM oluşturma
> * Bir kaynakları silme


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.46 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).

[!INCLUDE [virtual-machines-common-shared-images-cli](../../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-vm"></a>VM oluşturma

Sürüm kullanarak görüntüden VM oluşturma [az vm oluşturma](/cli/azure/vm#az-vm-create).

```azurecli-interactive 
az vm create\
   -g myGalleryRG \
   -n myVM \
   --image "/subscriptions/<subscription-ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
   --generate-ssh-keys
```

[!INCLUDE [virtual-machines-common-gallery-list-cli](../../../includes/virtual-machines-common-gallery-list-cli.md)]



## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [genel bakış](shared-image-galleries.md).
