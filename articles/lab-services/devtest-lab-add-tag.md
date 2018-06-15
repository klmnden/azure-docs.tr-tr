---
title: Azure DevTest Labs laboratuvarda için etiketler ekleme | Microsoft Docs
description: Azure DevTest Labs laboratuarda bir etiket eklemek öğrenin
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
ms.openlocfilehash: 3d9a5b3c0ae0b6058d3e8ccf8cdb340bd1200edc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787419"
---
# <a name="add-tags-to-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs laboratuvarda için etiketler ekleme

Özel etiketler oluşturabilir ve bunları mantıksal olarak kaynaklarınızı kategorilere ayırmak için DevTest Labs kaynaklarınızı uygulayabilirsiniz. Daha sonra hızlı bir şekilde ve kolayca metnimizi sahip tüm kaynakları aboneliğinizde bakın. Faturalama veya Yönetim için kaynakları düzenlemek gerektiğinde etiketleri yardımcı olur.

Etiketler tarafından desteklenen kaynakları içerir

* Sanal makineleri işlem
* NIC’ler
* IP adresleri
* Yük dengeleyiciler
* Depolama hesapları
* Yönetilen diskler

Uygulayabileceğiniz ne zaman etiketleri, [Laboratuvar oluşturma](devtest-lab-create-lab.md) ve daha sonra yapılandırma ve ayarları altında etiketleri dikey aracılığıyla yönetebilirsiniz.

Her etiket oluşan bir **adı**/**değeri** çifti. Örneğin, bir etiket adıyla oluşturabilirsiniz *costcenter* değerine sahip *34543*. Bir etiketi gibi daha sonra bu yardımcı olabilecek, kuruluşunuzun belirli bu alana Faturalanabilir Laboratuvar kaynaklarını tanımlayın. Adları ve nasıl aboneliğinizi düzenlemek istediğiniz için anlamlı değerleri seçin alın.

## <a name="steps-to-manage-tags-in-an-existing-lab"></a>Varolan bir laboratuvar etiketlerinde yönetme adımları

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Gerekirse, seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden. Laboratuvarınızı altında bir Pano üzerinde zaten görüntülenebilir **tüm kaynakları**.
1. Labs listesinden eklemek veya etiketleri yönetmek istediğiniz Laboratuvar seçin.  
1. Laboratuvar 's üzerinde **genel bakış** alanında **yapılandırma ve ilkeleri**.  

    ![Yapılandırma ve ilkeleri düğmesi](./media/devtest-lab-add-tag/devtestlab-config-and-policies.png)

1. Solda'nin altında **Yönet**seçin **etiketleri**.
1. Bu Laboratuvar için yeni bir etiket oluşturmak için şunu girin bir **adı**/**değeri** eşleştirin ve seçin **kaydetmek**. Görüntülemek veya bu etiketi ile ilişkili tüm kaynakları yönetmek için listeden, varolan bir etiketi de seçebilirsiniz.

    ![Etiketleri yönet](./media/devtest-lab-add-tag/devtestlab-manage-tags.png)

## <a name="understanding-limitations-to-tags"></a>Etiketler için anlama kısıtlamaları

Etiketler için aşağıdaki sınırlamalar geçerlidir:

* Her kaynak veya kaynak grubu en fazla 15 etiket adı/değer çifti içerebilir. Bu sınırlama yalnızca kaynak grubu veya kaynağa doğrudan uygulanan etiketler için geçerlidir. Kaynak grupları, her biri 15 etiket adı/değer çiftine sahip çok sayıda kaynak içerebilir. 
* Etiket adı 512 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır. Depolama hesapları için etiket adı 128 karakter ile sınırlıdır ve etiket değeri 256 karakter ile sınırlıdır.
* Kaynak grubuna uygulanan etiketler, bu kaynak grubundaki kaynaklar tarafından devralınmaz.

[Azure kaynaklarınızı düzenleme için etiketler kullanın](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) etiketleri PowerShell veya Azure CLI kullanarak yönetme de dahil olmak üzere Azure'da etiketleri kullanma hakkında daha fazla ayrıntıları sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Özelleştirilmiş ilkeler kullanarak aboneliğinizi arasında kısıtlamaları ve kuralları uygulayabilirsiniz. Tanımladığınız bir ilke tüm kaynakların belirli bir etiket için bir değere sahip gerektirebilir. Daha fazla bilgi için bkz: [ayarlamak ilkeler ve zamanlamalar](devtest-lab-set-lab-policy.md).
* Araştır [DevTest Labs Azure Resource Manager hızlı başlangıç Şablon Galerisi](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
