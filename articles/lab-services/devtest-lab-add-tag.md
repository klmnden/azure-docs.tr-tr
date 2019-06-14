---
title: Azure DevTest labs'deki bir laboratuvara etiket ekleme | Microsoft Docs
description: Azure DevTest labs'deki bir laboratuvara etiket ekleme hakkında bilgi edinin
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: dc5b327a-62e4-41bc-80ef-deb3c23d51b2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: e4d9aeb527461cc7292235fef1de0abdfa4242bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311378"
---
# <a name="add-tags-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara etiket ekleme

Özel etiketler oluşturabilir ve bunları kaynaklarınızı mantıksal olarak kategorilere ayırmak için DevTest Labs kaynaklarınızı uygulayabilirsiniz. Daha sonra hızla ve kolayca aboneliğinizde bu etikete sahip tüm kaynaklara bakın. Faturalandırma veya yönetim kaynakları düzenlemek gerektiğinde etiketleri yararlı olur.

Etiketlere göre desteklenen kaynakları içerir

* Vm'leri işlem
* NIC’ler
* IP adresleri
* Yük dengeleyiciler
* Depolama hesapları
* Yönetilen diskler

Uygulayabileceğiniz ne zaman etiketler, [Laboratuvar oluşturma](devtest-lab-create-lab.md) ve daha sonra yapılandırma ve ayarlar altında etiketleri dikey penceresinde yönetebilirsiniz.

Her etiket oluşan bir **adı**/**değer** çifti. Örneğin, adında bir etiket oluşturabilirsiniz *costcenter* değerine sahip *34543*. Bir etiket gibi daha sonra bu yardımcı olabilecek, kuruluşunuzun belirli bu alana Faturalanabilir Laboratuvar kaynaklarını tanımlayın. Adları ve aboneliğinizi düzenlemek istediğiniz için anlamlı değerleri seçmek alın.

## <a name="steps-to-manage-tags-in-an-existing-lab"></a>Mevcut bir laboratuvar etiketleri yönetme adımları

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. Laboratuvarınızı zaten altında Panoda görüntülenebilir **tüm kaynakları**.
1. Labs listesinden eklemek veya etiketleri yönetmek istediğiniz Laboratuvar seçin.
1. Laboratuvar'ın **genel bakış** alanında **yapılandırması ve ilkelerini**.

    ![Yapılandırması ve ilkelerini düğmesi](./media/devtest-lab-add-tag/devtestlab-config-and-policies.png)

1. Soldaki altında **Yönet**seçin **etiketleri**.
1. Bu Laboratuvar için yeni bir etiket oluşturmak için girin bir **adı**/**değer** eşleştirebilir ve seçin **Kaydet**. Ayrıca, bu etiketle ilişkili kaynakları yönetmek veya görüntülemek için listeden varolan bir etiketi seçebilirsiniz.

    ![Etiketleri yönet](./media/devtest-lab-add-tag/devtestlab-manage-tags.png)

## <a name="understanding-limitations-to-tags"></a>Etiketlerle sınırlamaları anlama

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir.
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

[Azure kaynaklarınızı düzenlemek için etiketleri kullanma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) etiketleri PowerShell veya Azure CLI kullanarak yönetme de dahil olmak üzere Azure'da etiketler kullanma hakkında daha fazla ayrıntıları sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş ilkeler kullanarak, aboneliğinizi arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Tanımladığınız bir ilke, tüm kaynakların belirli bir etiket için bir değer olmasını gerektirebilir. Daha fazla bilgi için [ilke ve zamanlamalar ayarlama](devtest-lab-set-lab-policy.md).
* Keşfedin [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates).
