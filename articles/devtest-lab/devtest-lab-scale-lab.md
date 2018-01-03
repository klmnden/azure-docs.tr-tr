---
title: "Ölçeklendirme kotalar ve sınırlar laboratuvarınızda Azure DevTest Labs | Microsoft Docs"
description: "Azure DevTest Labs'de Laboratuvar ölçeklendirmek öğrenin"
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: ae624155-9181-45fa-bd2b-1983339b7e0e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: v-craic
ms.openlocfilehash: 7ea022dfb39154ec86c26856030111beee672bd6
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="scale-quotas-and-limits-in-devtest-labs"></a>Kotalar ve sınırlar DevTest Labs'de ölçeklendirme
DevTest Labs'de çalışırken DevTest Labs hizmet etkileyebilecek bazı Azure kaynakları için belirli varsayılan sınırları olduğunu fark edebilirsiniz. Bu sınırlar denir **kotaları**.

> [!NOTE]
> DevTest Labs hizmet hiçbir kota zorunlu tuttukları değil. Karşılaşabileceğiniz kotaları varsayılan genel Azure aboneliğinin sınırlamalardır.

Kotasına ulaşana kadar her Azure kaynak kullanabilirsiniz. Her abonelik ayrı kotaları sahiptir ve abonelik başına kullanım izlenir.

Örneğin, her abonelik varsayılan kota 20 olarak belirlenen çekirdek sahiptir. Dört çekirdek ile laboratuvarınızda VM'ler oluşturuyorsanız, bu nedenle, daha sonra yalnızca beş VM'ler oluşturabilirsiniz. 

[Azure aboneliği ve hizmet sınırları](https://docs.microsoft.com/azure/azure-subscription-service-limits) bazı Azure kaynakları için en yaygın kotaları yer almaktadır. Kaynakları bir laboratuar ortamında en yaygın olarak kullanılan ve hangi karşılaşabileceğiniz için kotaları içerir VM çekirdek, genel IP adresleri, ağ arabirimi, yönetilen diskleri, RBAC rol ataması ve ExpressRoute bağlantı hatları.

## <a name="view-your-usage-and-quotas"></a>Kotalar ve kullanım görüntüleyin
Bu adımlar nasıl aboneliğinizde belirli Azure kaynakları için geçerli kotalar görüntülemek ve her kota yüzdesini kullanıldığını görmek için gösterir.

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Seçin **daha Hizmetleri**ve ardından **faturalama** listeden.
1. Faturalama dikey penceresinde bir abonelik seçin.
4. Seçin **kullanım + kotaları**.

   ![Kullanım ve kotaları düğmesi](./media/devtest-lab-scale-lab/devtestlab-usage-and-quotas.png)

   Kullanım + kotaları dikey penceresi görünür, listeyi farklı kaynaklar bu abonelikte ve kaynak başına kullanılan kota yüzdesi.

   ![Kotalar ve kullanım](./media/devtest-lab-scale-lab/devtestlab-view-quotas.png)

## <a name="requesting-more-resources-in-your-subscription"></a>Daha fazla kaynak aboneliğinizde isteme
Bir kota cap ulaştıysanız, bir kaynağın bir abonelik varsayılan sınır sınırına kadar açıklandığı gibi artırılabilir [Azure aboneliği ve hizmet sınırları](https://docs.microsoft.com/azure/azure-subscription-service-limits).

Bu adımları aracılığıyla kota artışı isteği göndermek üzere nasıl Göster [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Seçin **daha Hizmetleri**seçin **faturalama**ve ardından **kullanım + kotaları**.
1. Kullanım + kotaları dikey penceresinde, select **isteği artırmak** düğmesi.

   ![İstek artış düğmesi](./media/devtest-lab-scale-lab/devtestlab-request-increase.png)

1. Tamamlayıp isteği göndermek için gerekli bilgiler tüm üç sekmelerde doldurmak **yeni destek isteği** formu.

   ![İstek artış formu](./media/devtest-lab-scale-lab/devtestlab-support-form.png)

[Anlama Azure sınırları ve artar](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/) kota artışı isteği göndermek üzere Azure desteğine başvurarak hakkında daha fazla bilgi sağlar.



[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

### <a name="next-steps"></a>Sonraki adımlar
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
