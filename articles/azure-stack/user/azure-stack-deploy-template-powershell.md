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
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: unknown
ms.lastreviewed: 01/04/2019
ms.openlocfilehash: d71df99096e58b3ac7adc920b91891b9a50cd6f7
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58479590"
---
# <a name="deploy-a-template-to-azure-stack-using-powershell"></a>Bir şablonu PowerShell kullanarak Azure Stack'e dağıtma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack için Azure Resource Manager şablonlarını dağıtmak için PowerShell kullanabilirsiniz. Bu makalede, bir şablonu dağıtmak için PowerShell kullanmayı açıklar.

## <a name="run-azurerm-powershell-cmdlets"></a>AzureRM PowerShell cmdlet'lerini çalıştırın

Bu örnekte **AzureRM** depolanan Github'da PowerShell cmdlet'leri ve bir şablon. Şablon, Windows Server 2012 R2 Datacenter sanal makine oluşturur.

>[!NOTE]
>Bu örnekte denemeden önce emin olun [PowerShell yapılandırılmış](azure-stack-powershell-configure-user.md) Azure Stack kullanıcısı için.

1. Git [ https://aka.ms/AzureStackGitHub ](https://aka.ms/AzureStackGitHub) ve bulma **basit windows vm 101** şablonu. Şablonu bu konuma kaydedin: `C:\templates\azuredeploy-101-simple-windows-vm.json`.
2. Yükseltilmiş bir PowerShell komut istemi açın.
3. Değiştirin `username` ve `password` aşağıdaki komut, kullanıcı adı ve parola ve betiği çalıştırın:

    ```powershell
    # Set deployment variables
    $myNum = "001" # Modify this per deployment
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

- [Şablonları Visual Studio ile dağıtma](azure-stack-deploy-template-visual-studio.md)
