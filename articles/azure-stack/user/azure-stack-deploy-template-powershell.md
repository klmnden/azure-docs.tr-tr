---
title: Şablonları PowerShell kullanarak Azure Stack'te dağıtma | Microsoft Docs
description: Bir şablonu PowerShell kullanarak Azure Stack'e dağıtma.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 4b254f9a4446a1b0ff400e0d63effe68fc4f82b4
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363675"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Bir şablonu PowerShell kullanarak Azure Stack'e dağıtma

*İçin geçerlidir: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager şablonlarını dağıtmak için PowerShell kullanabilirsiniz. Bu makalede, bir şablonu dağıtmak için PowerShell kullanmayı açıklar.

## <a name="run-azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'lerini çalıştırın

Bu örnek, AzureRM PowerShell cmdlet'leri ve Github'a depolanmış bir şablon kullanır. Şablon, Windows Server 2012 R2 Datacenter sanal makine oluşturur.

>[!NOTE]
>Bu örnekte denemeden önce emin olun [PowerShell yapılandırılmış](azure-stack-powershell-configure-user.md) Azure Stack kullanıcısı için.

1. Git [ http://aka.ms/AzureStackGitHub ](http://aka.ms/AzureStackGitHub) ve bulma **basit windows vm 101** şablonu. Şablonu bu konuma kaydedin: C:\\şablonları\\azuredeploy-101-basit-windows-vm.json.
2. Yükseltilmiş bir PowerShell komut istemi açın.
3. Değiştirin *kullanıcıadı* ve *parola* aşağıdaki komut, kullanıcı adı ve parola ve betiği çalıştırın.

   ```PowerShell
   # Set deployment variables
   $myNum = "001" #Modify this per deployment
   $RGName = "myRG$myNum"
   $myLocation = "local"
   
   # Create resource group for template deployment
   New-AzureRmResourceGroup -Name $RGName -Location $myLocation
   
   # Deploy simple IaaS template
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

   >[!IMPORTANT]
   >Bu betik komutunu her çalıştırdığınızda, değeri artırmak `$myNum` dağıtımınızı üzerine yazmasını engellemek için parametre.

4. Azure Stack portal, select açın **Gözat**ve ardından **sanal makineler** yeni sanal makinenize bulmak için (**myDeployment001**).

## <a name="next-steps"></a>Sonraki adımlar

[Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
