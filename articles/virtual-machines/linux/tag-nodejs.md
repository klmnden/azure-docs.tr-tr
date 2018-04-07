---
title: Bir Azure Linux sanal makine etiketlemek nasıl | Microsoft Docs
description: Azure Resource Manager dağıtım modeli kullanılarak oluşturulmuş bir Azure Linux sanal makinenin etiketleme hakkında bilgi edinin.
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
ms.openlocfilehash: 103784d97301313379b2fd336624e5cda591c5a6
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Azure'da bir Linux sanal makine etiketlemek nasıl
Bu makalede Resource Manager dağıtım modeli aracılığıyla Azure'da bir Linux sanal makine etiketlemek için farklı yollar açıklanmaktadır. Etiketler doğrudan bir kaynağa veya bir kaynak grubu yerleştirilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure şu anda kaynak ve kaynak grubu başına en fazla 15 etiketlerini destekler. Etiketler oluşturma sırasında bir kaynağa yerleştirilmiş veya mevcut bir kaynağı eklendi. Lütfen unutmayın, Resource Manager dağıtım modeli yalnızca oluşturulan kaynaklar için etiketler desteklenir.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure CLI ile etiketleme
Başlamak için [Azure CLI'yi yükleme ve yapılandırma](../../xplat-cli-azure-resource-manager.md) ve Kaynak Yöneticisi modunda olduğundan emin olun (`azure config mode arm`).

Bir verilen etiketleri de dahil olmak üzere bu komutu kullanarak sanal makine için tüm özelliklerini görüntüleyebilirsiniz:

        azure vm show -g MyResourceGroup -n MyTestVM

Azure CLI aracılığıyla yeni bir VM etiket eklemek için kullanabileceğiniz `azure vm set` tag parametresini birlikte komutu **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Tüm etiketleri kaldırmak için kullanabileceğiniz **– T** parametresinde `azure vm set` komutu.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Etiketler KAYNAKLARIMIZI Azure CLI ve Portal için uyguladıysanız, fatura portalı etiketlerinde görmek için kullanım ayrıntılarını bir göz atalım.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış] [ Azure Resource Manager Overview] ve [etiketleri kullanarak Azure kaynaklarınızı düzenleme][Using Tags to organize your Azure Resources].
* Etiketleri kullanımınızı Azure kaynaklarını yönetmek nasıl yardımcı olabileceğini görmek için bkz: [Azure faturanızı anlamak] [ Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketimini Öngörüler elde][Gain insights into your Microsoft Azure resource consumption].

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/resource-group-overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/resource-group-using-tags.md
[Understanding your Azure Bill]: ../../billing/billing-understand-your-bill.md
[Gain insights into your Microsoft Azure resource consumption]: ../../billing/billing-usage-rate-card-overview.md
