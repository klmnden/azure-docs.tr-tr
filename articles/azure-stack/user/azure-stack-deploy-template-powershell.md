---
title: "PowerShell Azure yığınında şablonlarıyla dağıtma | Microsoft Docs"
description: "Resource Manager şablonu ve PowerShell kullanarak bir sanal makine dağıtmayı öğrenin."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: e4837be016b569dbd0b4bf8e071e6381b8daa85f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-templates-in-azure-stack-using-powershell"></a>Azure PowerShell kullanarak yığınında şablonlarını dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığın Geliştirme Seti dağıtmak için PowerShell kullanın.  Resource Manager şablonları dağıtın ve tüm kaynakları tek ve eşgüdümlü bir işlemle uygulamanızda sağlayın.

## <a name="run-azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'lerini çalıştırın
Bu örnekte, Resource Manager şablonu kullanarak bir sanal makineyi Azure yığın Geliştirme Seti dağıtmak için bir komut dosyasını çalıştırın.  Devam etmeden önce olduğundan emin olun [PowerShell yapılandırılır](azure-stack-powershell-configure-user.md)  

Bu örnek şablonda kullanılan VHD Windows Server 2012 R2 Datacenter ' dir.

1. Git <http://aka.ms/AzureStackGitHub>, arama **101-basit-windows-vm** şablonu ve şu konuma kaydedin: c:\\şablonları\\ azuredeploy-101-basit-windows-vm.json.
2. PowerShell'de aşağıdaki dağıtım komut dosyasını çalıştırın. Değiştir *kullanıcıadı* ve *parola* kullanıcı adı ve parola. Değeri sonraki kullanımlar üzerinde Artır *$myNum* dağıtımınızı üzerine yazılmasını engellemek için parametre.
   
   ```PowerShell
       # Set Deployment Variables
       $myNum = "001" #Modify this per deployment
       $RGName = "myRG$myNum"
       $myLocation = "local"
   
       # Create Resource Group for Template Deployment
       New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
       # Deploy Simple IaaS Template
       New-AzureRmResourceGroupDeployment `
           -Name myDeployment$myNum `
           -ResourceGroupName $RGName `
           -TemplateFile c:\templates\azuredeploy-101-simple-windows-vm.json `
           -NewStorageAccountName mystorage$myNum `
           -DnsNameForPublicIP mydns$myNum `
           -AdminUsername <username> `
           -AdminPassword ("<password>" | ConvertTo-SecureString -AsPlainText -Force) `
           -VmName myVM$myNum `
           -WindowsOSVersion 2012-R2-Datacenter
   ```
3. Azure yığın portal açmak **Gözat**, tıklatın **sanal makineler**ve yeni sanal makineniz için bakın (*myDeployment001*).


## <a name="next-steps"></a>Sonraki adımlar
[Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)

