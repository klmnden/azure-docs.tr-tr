---
title: Paylaşılan bir ölçek kümesi Azure'da oluşturmak için VM görüntüleri kullan | Microsoft Docs
description: Azure'da sanal makine ölçek kümeleri dağıtmak için kullanılacak paylaşılan VM görüntüleri oluşturmak için Azure PowerShell kullanmayı öğrenin.
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
ms.date: 04/25/2019
ms.author: akjosh; cynthn
ms.custom: ''
ms.openlocfilehash: ff8d94213e4e07b6597f6195126116a607c18bf7
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191740"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-powershell"></a>Oluşturma ve Azure PowerShell ile sanal makine ölçek kümeleri için paylaşılan görüntülerini kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. Paylaşılan görüntü Galerisi hizmeti kuruluşunuz genelinde paylaşımı özel görüntü büyük ölçüde kolaylaştırır. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. 

Paylaşılan görüntü Galerisi özel VM görüntülerinizi başkalarıyla kuruluşunuzdaki içinde veya bölgeler, bir AAD kiracısı arasında paylaşmanıza olanak tanır. Paylaşmak istediğiniz hangi görüntüleri seçin, bunları bulunan ve kendileriyle paylaşmak istediğiniz yapmak istediğiniz hangi bölgeleri. Paylaşılan görüntülerini mantıksal olarak gruplayabilirsiniz, böylece birden fazla galeri oluşturabilirsiniz. 

Galeri tam rol tabanlı erişim denetimi (RBAC) sağlayan bir üst düzey bir kaynaktır. Görüntüleri tutulan olabilir ve farklı bir Azure bölgesi kümesi için her görüntü sürümü çoğaltmak seçebilirsiniz. Galeri, yalnızca yönetilen görüntüleri ile çalışır. 

Paylaşılan görüntü Galerisi özelliği, birden çok kaynak türü vardır. Biz kullanarak veya kaldırılacak bunlar bu makaledeki oluşturma:

| Resource | Açıklama|
|----------|------------|
| **Yönetilen bir görüntü** | Bu, tek başına kullanılan veya oluşturmak için kullanılan bir temel görüntü, bir **görüntü sürümü** bir görüntü galerisinde. Yönetilen bir görüntü genelleştirilmiş sanal makinelerinden oluşturulur. Yönetilen bir görüntü, birden çok sanal makine sağlamak için kullanılabilir ve artık paylaşılan görüntü sürümlerini oluşturmak için kullanılan VHD özel türüdür. |
| **Görüntü Galerisi** | Azure Marketi gibi bir **görüntü Galerisi** yönetmek ve görüntüler, ancak kimlerin erişebildiğini siz denetlersiniz paylaşımı için bir depodur. |
| **Görüntü tanımı** | Görüntüleri bir galeri içindeki tanımlanır ve görüntü ve dahili olarak kullanma gereksinimleri hakkında bilgi Yürüt. Bu, görüntünün Windows veya Linux, sürüm notları ve minimum ve maksimum bellek gereksinimleri olup olmadığını içerir. Görüntü türü bir tanımıdır. |
| **Görüntü sürümü** | Bir **görüntü sürümü** bir galeri kullanırken bir VM oluşturmak için kullanın. Görüntünün birden çok sürümü, ortamınız için gerektiği şekilde olabilir. Yönetilen bir görüntü kullanırken gibi bir **görüntü sürümü** bir VM oluşturmak için görüntü sürümü sanal makine için yeni bir disk oluşturmak için kullanılır. Yansıma sürümü birden çok kez kullanılabilir. |

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [updated-for-az.md](../../includes/updated-for-az.md)]

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki adımlarda, mevcut bir VM'yi alıp yeni VM örnekleri oluşturmak için kullanabileceğiniz yeniden kullanılabilir bir özel görüntüye dönüştürme işlemi ayrıntılı olarak açıklanmıştır.

Bu makalede örneği tamamlamak için var olan yönetilen bir görüntü olması gerekir. İzleyebileceğiniz [Öğreticisi: Oluşturma ve Azure PowerShell ile sanal makine ölçek kümeleri için özel görüntü kullanma](tutorial-use-custom-image-powershell.md) gerekirse oluşturmak için. Yönetilen bir görüntü veri diski varsa, veri disk boyutu 1 TB'den fazla olamaz.

Makale üzerinden geçmeden değiştirdiğinizde kaynak grubu ve VM adlarını gerektiğinde.


[!INCLUDE [virtual-machines-common-shared-images-ps](../../includes/virtual-machines-common-shared-images-powershell.md)]

## <a name="create-a-scale-set-from-the-shared-image-version"></a>Ölçek kümesi paylaşılan görüntü sürümü oluşturma

Bir sanal makine ölçek kümesi oluşturma [yeni AzVmss](/powershell/module/az.compute/new-azvmss). Aşağıdaki örnek, Batı ABD veri merkezinde yeni görüntü sürümünden bir ölçek oluşturur. Sanal ağ, genel IP adresi ve yük dengeleyici için Azure ağ kaynakları otomatik olarak oluşturulur. İstendiğinde ölçek kümesindeki VM örnekleri için yönetici kendi kimlik bilgilerini ayarlayın:

```azurepowershell-interactive
New-AzVmss `
  -ResourceGroupName myVMSSRG `
  -Location 'South Central US' `
  -VMScaleSetName 'myScaleSet' `
  -VirtualNetworkName 'myVnet' `
  -SubnetName 'mySubnet'`
  -PublicIpAddressName 'myPublicIPAddress' `
  -LoadBalancerName 'myLoadBalancer' `
  -UpgradePolicyMode 'Automatic' `
  -ImageName $imageVersion.Id
```

Tüm ölçek kümesi kaynaklarının ve VM'lerin oluşturulup yapılandırılması birkaç dakika sürer.

[!INCLUDE [virtual-machines-common-gallery-list-ps](../../includes/virtual-machines-common-gallery-list-ps.md)]

[!INCLUDE [virtual-machines-common-shared-images-update-delete-ps](../../includes/virtual-machines-common-shared-images-update-delete-ps.md)]


## <a name="next-steps"></a>Sonraki adımlar

Paylaşılan görüntü Galerisi kaynak şablonlarını kullanarak da oluşturabilirsiniz. Çeşitli Azure hızlı başlangıç şablonları mevcuttur: 

- [Paylaşılan bir görüntü Galerisi oluşturma](https://azure.microsoft.com/resources/templates/101-sig-create/)
- [Paylaşılan bir görüntü galerisinde bir görüntü tanımı oluşturun](https://azure.microsoft.com/resources/templates/101-sig-image-definition-create/)
- [Paylaşılan bir görüntü galerisinde görüntü sürümü oluşturma](https://azure.microsoft.com/resources/templates/101-sig-image-version-create/)
- [Resmi sürümden bir VM oluşturma](https://azure.microsoft.com/resources/templates/101-vm-from-sig/)

Paylaşılan resim galerileri hakkında daha fazla bilgi için bkz: [genel bakış](shared-image-galleries.md). Sorun yaşarsanız bkz [sorun giderme paylaşılan resim galerileri](troubleshooting-shared-images.md).
