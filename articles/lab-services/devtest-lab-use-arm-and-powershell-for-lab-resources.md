---
title: Oluşturma veya PowerShell ile otomatik olarak Azure Resource Manager şablonları kullanarak labs değiştirme | Microsoft Docs
description: Azure Resource Manager şablonları oluşturmak veya labs DevTest lab otomatik olarak değiştirmek için PowerShell ile kullanmayı öğrenin
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
ms.openlocfilehash: 7cf6e5569e9d47b2a279f39cf837a4c0558bf3c9
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33788623"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Oluşturun veya Azure Resource Manager şablonları ve PowerShell kullanılarak otomatik olarak labs değiştirin.

DevTest Labs birçok Azure Resource Manager şablonları ve hızlı bir şekilde Yardım ve otomatik olarak yeni labs oluşturmak veya var olan Laboratuarları değiştirebilir ve ardından bu kaynakları dağıtabilirsiniz PowerShell komut dosyaları sağlar.

Bu makalede oluşturulması, değiştirilmesi ve, labs dağıtımını otomatik hale getirmek için bu şablonları ve komut dosyaları kullanma sürecinde size rehberlik sağlar. Bu makalede ayrıca DevTest Labs'de bazı ortak görevleri gerçekleştirmek için PowerShell kullanma hakkında daha fazla bilgi bulabileceği yerler gösterilmektedir.

## <a name="step-1-gather-your-templates-and-scripts"></a>Adım 1: şablonları ve komut dosyaları toplama
Önceden yapılan bulabilir [Azure Resource Manager şablonları](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) ve [PowerShell komut dosyalarını](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) ortak adresindeki [Github deposunu](https://github.com/Azure/azure-devtestlab). Bunları kullanın-olan veya gereksinimlerinize göre özelleştirmek ve bunları kendi içinde depolamak [özel Git deposuna](devtest-lab-add-artifact-repo.md). 

## <a name="step-2-modify-your-azure-resource-manager-template"></a>2. adım: Azure Resource Manager şablonunu değiştirme
Bölümündeki adımları izleyin [, ilk Azure Resource Manager şablonu oluşturma](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) hiçbir zaman önce bir şablon oluşturduysanız.

Ayrıca, [en iyi uygulamalar Azure Resource Manager şablonları oluşturmak için](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) güvenilir ve kullanımı kolay pek çok kılavuzları ve Azure Resource Manager şablonları oluşturmanıza yardımcı olacak öneriler sunar. Genellikle, bir yaklaşım veya sağlanan örnekleri çeşitlemesi kullanın ve şablonunuzu gereksinimlerinize göre değiştirin.

## <a name="step-3-deploy-resources-with-powershell"></a>Adım 3: PowerShell kaynaklarla dağıtma
Şablonlar ve komut dosyalarınız özelleştirdikten sonra gerekli adımları [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtmak](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Makale için Azure kaynaklarınızı dağıtmak için Azure Resource Manager şablonları ile Azure PowerShell'i kullanma hakkında genel bilgi sağlar.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>PowerShell kullanarak DevTest Labs'de gerçekleştirdiğiniz ortak görevler
PowerShell kullanarak otomatikleştirebilirsiniz birçok diğer ortak görevler vardır. Aşağıdaki bölümlerde belgelerin bu görevleri gerçekleştirmek için gerekli adımlar verilmiştir.

* [PowerShell kullanarak bir VHD'yi dosyasından özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [PowerShell kullanarak Laboratuvar ait depolama hesabı için VHD dosyasını karşıya yükle](devtest-lab-upload-vhd-using-powershell.md)
* [PowerShell kullanarak Laboratuvar için bir dış kullanıcı ekleme](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [PowerShell kullanarak bir lab özel rolü oluşturun](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Sonraki adımlar
* Oluşturmayı öğrenin bir [özel Git deposu](devtest-lab-add-artifact-repo.md) depolayacağınız özelleştirilmiş şablonları veya komut dosyaları.
* Araştır [Azure Resource Manager şablonları Azure hızlı başlangıç Şablon Galerisinden](https://github.com/Azure/azure-quickstart-templates).
