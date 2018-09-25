---
title: Paylaşılan bir ölçek kümesi Azure'da oluşturmak için VM görüntüleri kullan | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri dağıtmak için kullanılacak paylaşılan VM görüntüleri oluşturmak için Azure CLI'yı kullanmayı öğrenin.
services: virtual-machine-scale-sets
documentationcenter: ''
author: axayjo
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2018
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: 777f46a0bbe8deb4f8b207e099e4fd83d9fc8507
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47048297"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-cli-20"></a>Oluşturma ve Azure CLI 2.0 ile sanal makine ölçek kümeleri için paylaşılan görüntülerini kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. Paylaşılan görüntü Galerisi hizmeti kuruluşunuz genelinde paylaşımı özel görüntü büyük ölçüde kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Paylaşılan görüntü Galerisi oluşturma
> * Paylaşılan görüntü tanımı oluşturma
> * Paylaşılan görüntü sürümü oluşturma
> * Paylaşılan bir görüntüden VM oluşturma
> * Bir kaynakları silme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


>[!NOTE]
> Bu makalede, genelleştirilmiş bir yönetilen görüntüsü kullanma işlemi gösterilmektedir. Özelleştirilmiş bir sanal makine görüntüsünden ölçek kümesi oluşturulması desteklenmez.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLI Sürüm 2.0.46 çalıştırdığınız gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli).


[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]

## <a name="create-a-scale-set-from-the-custom-vm-image"></a>Özel bir sanal makine görüntüsünden ölçek kümesi oluşturma
[az vmss create](/cli/azure/vmss#az-vmss-create) komutunu kullanarak bir ölçek kümesi oluşturun. *UbuntuLTS* veya *CentOS* gibi bir platform görüntüsü yerine, özel sanal makine görüntünüzün adını belirtin. Aşağıdaki örnek, önceki adımda yer alan *myImage* adlı özel görüntüyü kullanan *myScaleSet* adlı bir ölçek kümesi oluşturur:

```azurecli-interactive
az vmss create \
  -g myGalleryRG \
  -n myScaleSet \
  --image "/subscriptions/<subscription ID>/resourceGroups/myGalleryRG/providers/Microsoft.Compute/galleries/myGallery/images/myImageDefinition/versions/1.0.0" \
  --admin-username azureuser \
  --generate-ssh-keys
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.


[!INCLUDE [virtual-machines-common-gallery-list-cli](../../includes/virtual-machines-common-gallery-list-cli.md)]


## <a name="clean-up-resources"></a>Kaynakları temizleme
Ölçek kümenizi ve ek kaynaklarınızı kaldırmak için [az group delete](/cli/azure/group#az_group_delete) komutunu kullanarak kaynak grubunu ve bu kaynak grubunun tüm kaynaklarını silin. `--no-wait` parametresi işlemin tamamlanmasını beklemeden denetimi komut istemine döndürür. `--yes` parametresi kaynakları ek bir komut istemi olmadan silmek istediğinizi onaylar.

```azurecli-interactive
az group delete --name myResourceGroup --no-wait --yes
```


## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla varsa [paylaşılan resim galerileri sorun giderme](troubleshooting-shared-images.md).