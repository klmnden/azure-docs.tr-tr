---
title: PowerShell ile Azure Resource Manager şablonları kullanılarak otomatik olarak laboratuvarı oluşturma veya değiştirme | Microsoft Docs
description: Laboratuvarı otomatik olarak geliştirme ve test laboratuvarı oluşturma veya değiştirme için PowerShell ile Azure Resource Manager şablonlarını kullanma hakkında bilgi edinin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: cb5a08730b47cb5df3116aa4a54554ef0ee6f260
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622466"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Azure Resource Manager şablonları ve PowerShell kullanarak otomatik olarak laboratuvarı oluşturma veya değiştirme

DevTest Labs, birçok Azure Resource Manager şablonları ve hızlı bir şekilde Yardım ve otomatik olarak yeni bir laboratuvar oluşturma veya mevcut labs değiştirebilir ve ardından bu kaynakları dağıtma PowerShell komut dosyaları sağlar.

Bu makalede oluşturulması, değiştirilmesi ve laboratuvarlarınızı dağıtımını otomatik hale getirmek için bu şablonları ve betikler kullanarak işlemi boyunca size rehberlik sağlar. Bu makalede ayrıca, DevTest Labs'de bazı genel görevleri gerçekleştirmek için PowerShell kullanma hakkında daha fazla bilgi bulabileceğiniz açıklanır.

## <a name="step-1-gather-your-templates-and-scripts"></a>1. Adım: Şablonları ve betikler toplayın
Önceden yapılan bulabilirsiniz [Azure Resource Manager şablonları](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) ve [PowerShell betikleri](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Scripts) ortak adresindeki [GitHub deposu](https://github.com/Azure/azure-devtestlab). Bunları olarak-olan veya gereksinimlerinize göre özelleştirin ve bunları kendi içinde depolamak [özel Git deposu](devtest-lab-add-artifact-repo.md).

## <a name="step-2-modify-your-azure-resource-manager-template"></a>2. Adım: Azure Resource Manager şablonunuzu değiştirme
Bölümündeki adımları takip edebilirsiniz [ilk Azure Resource Manager şablonunuzu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) hiçbir zaman önce bir şablon oluşturduysanız.

Ayrıca, [Azure Resource Manager şablonları oluşturmaya yönelik en iyi uygulamalar](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) birçok yönergeler ve öneriler, Azure Resource Manager şablonları oluşturmanıza yardımcı olması için, güvenilir ve kullanımı kolay sunar. Genellikle, bir yaklaşım veya sağlanan örnekleri çeşitlemesi kullanacağınızı ve gereksinimlerinize göre şablonunuzu değiştirmeniz.

## <a name="step-3-deploy-resources-with-powershell"></a>3. Adım: PowerShell ile kaynakları dağıtma
Şablonları ve betikler özelleştirdikten sonra gerekli adımları [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Makale, kaynakları Azure'a dağıtmak için Azure Resource Manager şablonları ile Azure PowerShell kullanma hakkında genel bilgiler sağlar.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Genel görevleri PowerShell kullanarak DevTest Labs'de gerçekleştirebilir.
PowerShell kullanarak otomatik hale getiren birçok diğer ortak görevler vardır. Aşağıdaki bölümlerde belgelerin bu görevleri gerçekleştirmek için gerekli adımları özetler.

* [PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [Laboratuvar depolama hesabına PowerShell kullanarak karşıya VHD dosyası yükleme](devtest-lab-upload-vhd-using-powershell.md)
* [PowerShell kullanarak Laboratuvar için bir dış kullanıcı ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell kullanarak Laboratuvar özel rol oluşturma](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Sonraki adımlar
* Oluşturmayı bir [özel Git deposu](devtest-lab-add-artifact-repo.md) depolayacağınız özelleştirilmiş şablonları veya betikler.
* Keşfedin [Azure hızlı başlangıç Şablon Galerisi, Azure Resource Manager şablonları](https://github.com/Azure/azure-quickstart-templates).
