---
title: Bir Azure Linux sanal makinesi etiketleme | Microsoft Docs
description: Azure Resource Manager dağıtım modeli kullanılarak oluşturulan bir Azure Linux sanal makine etiketleme hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: 297d3bc201b4bc9d9db0b0bed7a364769fa72859
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62115983"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makinesi etiketleme
Bu makalede Resource Manager dağıtım modeliyle Azure'da bir Linux sanal makinesi etiketleme için farklı yollar açıklanmaktadır. Etiketler doğrudan bir kaynak veya kaynak grubu üzerinde yerleştirilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure, şu anda kaynak ve kaynak grubu başına en fazla 15 etiket destekler. Etiket oluşturma sırasında bir kaynakta yerleştirilen veya var olan bir kaynağı eklendi. Lütfen unutmayın etiketleri yalnızca Resource Manager dağıtım modeliyle oluşturulan kaynakları için desteklenir.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure CLI ile etiketleme

Başlamak için en son ihtiyacınız vardır. [Azure CLI](/cli/azure/install-azure-cli) yüklü ve bir Azure hesabı kullanarak oturum açmış [az login](/cli/azure/reference-index#az-login).

Belirtilen etiketler de dahil olmak üzere bu komutu kullanarak sanal makine için tüm özelliklerini görüntüleyebilirsiniz:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Azure CLI aracılığıyla yeni bir VM etiket eklemek için kullanabileceğiniz `azure vm update` tag parametresi ile birlikte komut **--ayarlamak**:

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Etiketleri kaldırmak için kullanabileceğiniz **--kaldırmak** parametresinde `azure vm update` komutu.

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Etiketleri kaynaklarımızın Azure CLI ve Portal için uyguladıysanız, fatura Portalı'nda etiketlerini görmek için kullanım ayrıntılarını bir göz atalım.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış] [ Azure Resource Manager Overview] ve [kullanarak Azure kaynaklarınızı düzenlemek için etiketleri] [ Using Tags to organize your Azure Resources].
* Etiketler, Azure kaynaklarının kullanımını yönetmenize nasıl yardımcı olabileceğini görmek için bkz. [Azure faturanızı anlama] [ Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketiminize öngörü] [Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
