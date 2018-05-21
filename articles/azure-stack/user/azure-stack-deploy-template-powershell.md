---
title: PowerShell kullanarak Azure yığınında şablonlarını dağıtma | Microsoft Docs
description: Bir şablonu Azure PowerShell kullanarak yığınına dağıtın.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: 12fe32d7-0a1a-4c02-835d-7b97f151ed0f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 4af82deef029120aa2699e7c69c501ae61a1e8bd
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Azure PowerShell kullanarak yığınına şablon dağıtma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Azure Resource Manager şablonları Azure yığınına dağıtmak için PowerShell kullanın. Bu makalede PowerShell bir şablonu dağıtmak için nasıl kullanılacağı gösterilmektedir.

## <a name="run-azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'lerini çalıştırın

Bu örnek AzureRM PowerShell cmdlet'leri ve GitHub üzerinde depolanan bir şablonu kullanır. Bir Windows Server 2012 R2 Datacenter sanal makine şablonu oluşturur.

>[!NOTE]
>Bu örnek çalışmadan önce açtığınızdan emin olun [PowerShell yapılandırılmış](azure-stack-powershell-configure-user.md) Azure yığın kullanıcı için.

1. Git <http://aka.ms/AzureStackGitHub> ve Bul **101-basit-windows-vm** şablonu. Şablonu bu konuma kaydedin: C:\\şablonları\\azuredeploy-101-basit-windows-vm.json.
2. Yükseltilmiş bir PowerShell komut istemi açın.
3. Değiştir *kullanıcıadı* ve *parola* aşağıdaki komut, kullanıcı adı ve parola ve komut dosyasını çalıştırın.

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

   >[!IMPORTANT]
   >Bu komut dosyasını çalıştırmak her dağıtımınızı üzerine yazılmasını engellemek için "$myNum" parametresinin değerini artırın.

4. Azure yığın portal, select açmak **Gözat**ve ardından **sanal makineleri** , yeni bir sanal makine bulmak için (*myDeployment001*).

## <a name="next-steps"></a>Sonraki adımlar

[Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
