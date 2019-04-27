---
title: Ölçeklendirme kotalar ve sınırlar laboratuvarınızda Azure DevTest Labs | Microsoft Docs
description: Azure DevTest Labs'de bir laboratuvar ölçeklendirmeyi öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: ec79f6a9b255d44e66b901a0aae263c8dbbf2863
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60623516"
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Kotalar ve sınırlar DevTest Labs'de ölçeklendirin
DevTest Labs'de çalışırken, DevTest Labs hizmet etkileyebilecek bazı Azure kaynakları için bazı varsayılan limitler olduğunu fark edebilirsiniz. Bu sınırlar denir **kotalar**.

> [!NOTE]
> DevTest Labs hizmeti herhangi bir kota koymaz. Karşılaşabileceğiniz herhangi bir kota genel Azure aboneliğinin varsayılan kısıtlamalar var.

Her Azure kaynağı kotasına ulaşana kadar kullanabilirsiniz. Her abonelik ayrı bir kotası bulunur ve abonelik başına kullanım izlenir.

Örneğin, her aboneliğin, varsayılan olarak 20 çekirdek kotası vardır. Laboratuvarınızda dört çekirdek içeren VM'ler oluşturuyorsanız, bu nedenle, daha sonra yalnızca beş VM'ler oluşturabilirsiniz.

[Azure abonelik ve hizmet sınırlamaları](https://docs.microsoft.com/azure/azure-subscription-service-limits) Azure kaynakları için en yaygın kotalar bazıları listelenmektedir. Kaynakları bir laboratuar ortamında en yaygın olarak kullanılan ve hangi karşılaşabileceğiniz için kotaları dahil sanal makine çekirdeklerine, genel IP adresleri, ağ arabirimi, yönetilen diskler, RBAC rolü ataması ve ExpressRoute bağlantı hatları.

## <a name="view-your-usage-and-quotas"></a>Kullanım ve kotalar görüntüleyin
Bu adımlar, aboneliğinizdeki belirli Azure kaynakları için geçerli kotalar görüntülemek için ve kullandığınız her kota yüzdesini görmek için nasıl gösterir.

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **diğer hizmetler**ve ardından **faturalama** listeden.
1. Faturalama dikey penceresinde bir abonelik seçin.
4. Seçin **kullanım ve kotalar**.

   ![Kullanım ve kotalar düğmesi](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Kullanım ve kotalar, listeyi farklı kaynaklar bu abonelikte ve kaynak başına kullanılan kota yüzdesi kullanılabilir dikey penceresi görünür.

   ![Kotalar ve kullanım](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Daha fazla kaynak, aboneliğinizdeki isteme
Bir kota üst sınırına ulaşırsanız, kaynağın bir abonelikte varsayılan sınır sınırına kadar açıklanan şekilde artırılabilir [Azure aboneliği ve hizmet sınırları](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Bu adımlarda aracılığıyla bir kota artırım talebinde bulunmak gösterilmektedir [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Seçin **diğer hizmetler**seçin **faturalama**ve ardından **kullanım ve kotalar**.
1. Kullanım ve kotalar, dikey penceresinde **istek artırmak** düğmesi.

   ![İstek artışı düğmesi](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. Üç sekmenin tamamında gerekli bilgileri doldurun tamamlamak ve isteği göndermek için **yeni destek isteği** formu.

   ![İstek artışı formu](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Understanding Azure sınırları ve arttıkça](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) bir kota artırım talebinde bulunmak Azure destek ile iletişim kurarak hakkında daha fazla bilgi sağlar.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Sonraki adımlar
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates).
