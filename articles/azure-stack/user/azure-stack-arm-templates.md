---
title: Azure Resource Manager şablonları Azure yığınında kullanın. | Microsoft Docs
description: Azure Resource Manager şablonları Azure yığınında sağlama kaynakları kullanmayı öğrenin.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 9c4d538f77ae056163fd17aa547162a4ad3eff63
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34301687"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Azure Resource Manager şablonları Azure yığınında kullanın.

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları dağıtmak ve tüm kaynakları tek ve eşgüdümlü bir işlemle uygulamanızda için sağlamak için kullanabilirsiniz. Ayrıca, bir kaynak grubunda kaynaklar değişiklikler yapmak üzere şablonları yeniden dağıtabilirsiniz.

Bu şablonlar, Microsoft Azure yığın portal, PowerShell, komut satırı ve Visual Studio ile dağıtılabilir.

Aşağıdaki Hızlı Başlangıç şablonlarını kullanılabilir [GitHub](http://aka.ms/azurestackgithub).

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>SharePoint Server (yüksek kullanılabilirlik dağıtımı) dağıtma

Aşağıdaki kaynakları içermektedir bir SharePoint Server 2013 grubu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* Üç depolama hesapları
* İki dış yük Dengeleyiciler
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış sanal makine (VM)
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM
* Bir makine SharePoint Server 2013 grubu yapılandırılan bir VM

## <a name="deploy-ad-non-high-availability-deployment"></a>AD (olmayan yüksek-kullanılabilirlik-dağıtım) dağıtma

Aşağıdaki kaynakları içeren bir AD etki alanı denetleyici sunucusu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* Bir depolama hesabı
* Bir dış yük dengeleyici
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış sanal makine (VM)

## <a name="deploy-adsql-non-high-availability-deployment"></a>AD/SQL (olmayan yüksek-kullanılabilirlik-dağıtım) dağıtma

Aşağıdaki kaynaklar içeren bir SQL Server 2014 tek başına sunucu oluşturmak için PowerShell DSC uzantısı kullanın:

* Bir sanal ağ
* İki depolama hesabı
* Bir dış yük dengeleyici
* Tek tek bir etki alanı ile yeni bir orman için etki alanı denetleyicisi olarak yapılandırılmış sanal makine (VM)
* SQL Server 2014 tek başına sunucu yapılandırılmış bir VM

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>VM-DSC-Extension-Azure-Automation-Pull-Server

PowerShell DSC uzantısı olan bir sanal makineyi yerel Configuration Manager (LCM'yi) yapılandırmak ve bir Azure Otomasyonu hesabı DSC çekme sunucusuna kaydetmek için kullanın.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Bir kullanıcı görüntüsünden sanal makine oluşturma

Bir sanal makineyi bir özel kullanıcı görüntüsünden oluşturun. Bu şablon, bir sanal ağla (DNS), genel IP adresi ve ağ arabirimi aynı zamanda dağıtır.

## <a name="basic-virtual-machine"></a>Temel sanal makine

Bir sanal ağla (DNS), genel IP adresi ve ağ arabirimi içeren bir Windows VM dağıtın.

## <a name="cancel-a-running-template-deployment"></a>Çalışan bir şablon dağıtımı iptal etme

Çalışan bir şablon dağıtımı iptal etmek için kullanın `Stop-AzureRmResourceGroupDeployment` PowerShell cmdlet'i.

## <a name="next-steps"></a>Sonraki adımlar

* [Şablonları portal ile dağıtma](azure-stack-deploy-template-portal.md)
* [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md)
