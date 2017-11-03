---
title: "Azure Resource Manager şablonları Azure yığınında kullanın. | Microsoft Docs"
description: "Azure Resource Manager şablonları Azure yığınında sağlama kaynakları kullanmayı öğrenin."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: 7648855011e8f77c35713d2d2ae50f2e474a08a6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager şablonları Azure yığınında kullanın.

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları dağıtın ve tek ve eşgüdümlü bir işlemle uygulamanızda için tüm kaynakları sağlayın. Ayrıca kaynak grubundaki kaynaklar değişiklikler yapmak üzere şablonları yeniden dağıtabilirsiniz.

Bu şablonlar, Microsoft Azure yığın portal, PowerShell, komut satırı ve Visual Studio ile dağıtılabilir.

Aşağıdaki Hızlı Başlangıç şablonlarını kullanılabilir [GitHub](http://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-non-high-availability"></a>SharePoint (yüksek olmayan kullanılabilirlik) dağıtma
Aşağıdaki kaynakları içermektedir bir SharePoint 2013 grubu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* Üç depolama hesapları
* İki dış yük Dengeleyiciler
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan bir VM
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM
* Bir makine SharePoint 2013 grubu yapılandırılan bir VM

## <a name="deploy-ad-non-high-availability"></a>AD (yüksek olmayan kullanılabilirlik) dağıtma
Aşağıdaki kaynakları içeren bir AD etki alanı denetleyici sunucusu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* Bir depolama hesabı
* Bir dış yük dengeleyici
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan bir VM

## <a name="deploy-adsql-non-high-availability"></a>AD/SQL (yüksek olmayan kullanılabilirlik) dağıtma
Aşağıdaki kaynaklar içeren bir SQL Server 2014 tek başına sunucu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* İki depolama hesabı
* Bir dış yük dengeleyici
* Tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılan bir VM
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server
PowerShell DSC uzantısı olan bir sanal makineyi yerel Configuration Manager (LCM'yi) yapılandırmak ve bir Azure Otomasyonu hesabı DSC çekme sunucusuna kaydetmek için kullanın.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Bir kullanıcı görüntüsünden sanal makine oluşturma
Bir sanal makineyi bir özel kullanıcı görüntüsünden oluşturun. Bu şablon, bir sanal ağla (DNS), genel IP adresi ve ağ arabirimi aynı zamanda dağıtır.

## <a name="simple-vm"></a>Basit VM
Bir sanal ağla (DNS), genel IP adresi ve ağ arabirimi içeren bir Windows VM dağıtın.

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımı iptal etme
Çalışan bir şablon dağıtımı iptal etmek için kullanın `Stop-AzureRmResourceGroupDeployment` PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar
[Şablonları portal ile dağıtma](azure-stack-deploy-template-portal.md)

[Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)

