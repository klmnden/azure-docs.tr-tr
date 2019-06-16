---
title: Görüntüleme ve bir sanal makinenin Azure Resource Manager şablonu kullanma | Microsoft Docs
description: Azure Resource Manager şablonu bir sanal makineden diğer sanal makineler oluşturmak için kullanmayı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 533770d98b146dea01e91e1249115c4b5c074b3c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62101582"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak sanal makineleri oluşturma 

Oluştururken bir sanal makine (VM) DevTest labs'deki [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040), sanal Makineyi kaydetmeden önce Azure Resource Manager şablonu görüntüleyebilirsiniz. Şablon, ardından aynı ayarlara sahip VM'ler daha fazla Laboratuvar oluşturmak için temel olarak kullanılabilir.

Bu makalede tek VM Resource Manager şablonları ile çoklu VM açıklar ve görüntüleme ve bir VM oluşturulurken bir şablon kaydetme işlemi gösterilmektedir.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Tek VM Resource Manager şablonları ile çoklu VM
Resource Manager şablonunu kullanarak DevTest Labs'de sanal makineler oluşturmak için iki yolu vardır: Microsoft.DevTestLab/labs/virtualmachines kaynak sağlamanız veya Microsoft.Compute/virtualmachines kaynak sağlayın. Her farklı senaryolarda kullanılır ve farklı izinleri gerektirir.

- ("Kaynak" özelliğinde şablonda bildirilen gibi) bir Microsoft.DevTestLab/labs/virtualmachines kaynak türünü kullanan resource Manager şablonları, tek tek Laboratuvar VM'ler sağlayabilirsiniz. Her VM ardından DevTest Labs sanal makineleri tek bir öğe olarak görünür:

   ![DevTest Labs sanal makineleri listedeki tek öğe olarak VM'lerin listesini](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Bu tür bir Resource Manager şablonu Azure PowerShell komutu aracılığıyla sağlanabilir **yeni AzResourceGroupDeployment** veya Azure CLI komutu aracılığıyla **az grubu dağıtım oluşturma**. DevTest Labs kullanıcı rolüyle ilişkilendirilen kullanıcılar dağıtım gerçekleştiremezler yönetici izinleri gerektirir. 

- Microsoft.Compute/virtualmachines kaynak türünü kullanan resource Manager şablonları, DevTest Labs sanal makineler listesi tek bir ortamda olarak birden çok VM sağlayabilirsiniz:

   ![DevTest Labs sanal makineleri listedeki tek öğe olarak VM'lerin listesini](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Aynı ortamda VM'ler birlikte yönetilebilir ve aynı yaşam döngüsünü. DevTest Labs kullanıcı rolüyle ilişkilendirilen kullanıcılar yönetici tarafından Laboratuvar bu şekilde yapılandırılmış olduğu sürece bu şablonları kullanarak ortamlar oluşturabilirsiniz.

Bu makalenin geri kalanında Microsoft.DevTestLab/labs/virtualmachines kullanan Resource Manager şablonları açıklar. Laboratuvar VM oluşturma (örneğin, talep edilebilir VM'ler) veya altın görüntü oluşturma (örneğin, görüntü Fabrika) otomatik hale getirmek için bunlar Laboratuvar yöneticileri tarafından kullanılır.

[Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) birçok yönergeler ve öneriler, Azure Resource Manager şablonları oluşturmanıza yardımcı olması için, güvenilir ve kullanımı kolay sunar.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Görüntüleme ve bir sanal makinenin Resource Manager şablonu kaydetme
1. Bölümündeki adımları [bir laboratuar ortamında ilk VM'nizi oluşturun](tutorial-create-custom-lab.md#add-a-vm-to-the-lab) bir sanal makine oluşturmaya başlayın.
1. Sanal makineniz için gerekli bilgileri girin ve bu VM için istediğiniz herhangi bir yapıt ekleyin.
1. Yapılandırma ayarları penceresinin en altında seçin **görünümü ARM şablonu**.

   ![Görünüm ARM şablonu düğmesi](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Kopyalayın ve daha sonra başka bir sanal makine oluşturmak için kullanmak üzere Resource Manager şablonu kaydedin.

   ![Daha sonra kullanmak üzere kaydetmek için resource Manager şablonu](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Resource Manager şablonu kaydettikten sonra kullanmadan önce şablon parametreleri bölümünü güncelleştirmeniz gerekir. Dışında gerçek Resource Manager şablon parametreleri yalnızca özelleştiren parameter.json oluşturabilirsiniz. 

![Bir JSON dosyası kullanarak parametrelerini özelleştirme](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

Resource Manager şablonu için kullanılmaya hazırdır [VM oluşturma](devtest-lab-create-environment-from-arm.md).

### <a name="next-steps"></a>Sonraki adımlar
* Bilgi edinmek için nasıl [Resource Manager şablonları ile çoklu VM ortamları oluşturma](devtest-lab-create-environment-from-arm.md).
* [Bir VM oluşturmak için Resource Manager şablonu dağıtma](devtest-lab-create-environment-from-arm.md#automate-deployment-of-environments)
* DevTest Labs Otomasyon için daha fazla hızlı başlangıç Resource Manager şablonları keşfedin [genel DevTest Labs GitHub deposunu](https://github.com/Azure/azure-quickstart-templates).
