---
title: Azure Stack'te Azure Resource Manager şablonlarını kullanma | Microsoft Docs
description: Azure Resource Manager şablonları Azure Stack'te sağlama kaynakları kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2018
ms.author: sethm
ms.reviewer: jeffgo
ms.openlocfilehash: d520a33fd6729f4a9d4778cfdb8d3380c9ac09b0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237851"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Stack'te Azure Resource Manager şablonlarını kullanma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Resource Manager şablonları, dağıtmak ve tek ve eşgüdümlü bir işlemle uygulamanıza yönelik tüm kaynakları sağlamak için kullanabilirsiniz. Ayrıca, bir kaynak grubundaki kaynaklar için değişiklik yapmak için şablonları yeniden dağıtabilirsiniz.

Bu şablonlar, Microsoft Azure Stack portal, PowerShell, komut satırı ve Visual Studio ile dağıtılabilir.

Aşağıdaki Hızlı Başlangıç şablonları mevcuttur [GitHub](https://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>SharePoint Server (yüksek kullanılabilirlik dağıtımı) dağıtın

Kullanmak için PowerShell DSC uzantısı [bir SharePoint Server 2013'ü grubu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/sharepoint-2013-non-ha) , aşağıdaki kaynakları içermektedir:

* Bir sanal ağ
* Üç depolama hesapları
* İki dış yük Dengeleyiciler
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan sanal makineye (VM)
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM
* Bir makine SharePoint Server 2013'ü grubu yapılandırılmış bir VM

## <a name="deploy-ad-non-high-availability-deployment"></a>AD (olmayan yüksek-kullanılabilirlik-dağıtımı) dağıtın

Kullanmak için PowerShell DSC uzantısı [bir AD etki alanı denetleyicisi sunucusuna oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/ad-non-ha) , aşağıdaki kaynakları içermektedir:

* Bir sanal ağ
* Bir depolama hesabı
* Bir dış yük dengeleyici
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan sanal makineye (VM)

## <a name="deploy-adsql-non-high-availability-deployment"></a>AD/SQL (olmayan yüksek-kullanılabilirlik-dağıtımı) dağıtın

Kullanmak için PowerShell DSC uzantısı [SQL Server 2014 tek başına bir sunucu oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/sql-2014-non-ha) , aşağıdaki kaynakları içermektedir:

* Bir sanal ağ
* İki depolama hesabı
* Bir dış yük dengeleyici
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan sanal makineye (VM)
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

PowerShell DSC uzantısı, var olan bir sanal makine yerel Configuration Manager'ı (LCM) yapılandırma ve bir Azure Otomasyon hesabı DSC çekme sunucusuna kaydetmek için kullanın.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Bir kullanıcı görüntüsünden sanal makine oluşturma

[Özel kullanıcı görüntüsünden sanal makine oluşturma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/101-vm-from-user-image). Bu şablonu ayrıca bir sanal ağla (DNS), genel IP adresi ve bir ağ arabirimi dağıtır.

## <a name="basic-virtual-machine"></a>Temel sanal makine

[Windows VM dağıtma](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/AzureStackTechnicalPreview1/101-simple-windows-vm) (DNS ile) bir sanal ağ, genel IP adresi ve bir ağ arabirimi içerir.

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımı iptal et

Çalışan bir şablon dağıtımı iptal etmek için kullanın [Stop-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/stop-azurermresourcegroupdeployment) PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları portal ile dağıtma](azure-stack-deploy-template-portal.md)
* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
