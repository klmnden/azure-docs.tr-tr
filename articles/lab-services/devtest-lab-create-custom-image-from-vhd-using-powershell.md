---
title: PowerShell kullanarak bir VHD'yi dosyasından Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs
description: Azure DevTest Labs PowerShell kullanarak bir VHD'yi dosyasından özel görüntü oluşturulmasını otomatik hale getirme
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 17679ee3a5cb50f78cad0f66cb3fffcc6d556087
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787503"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>PowerShell kullanarak bir VHD'yi dosyasından özel bir görüntü oluşturun

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, PowerShell kullanarak bir VHD'yi dosyasından özel görüntü oluşturmada size yol:

1. Bir PowerShell komut isteminde, aşağıdaki çağrıyı Azure hesabınızla oturum **Connect-AzureRmAccount** cmdlet'i.  
    
    ```PowerShell
    Connect-AzureRmAccount
    ```

1.  Çağırarak istediğiniz Azure aboneliğini seçin **Select-AzureRmSubscription** cmdlet'i. Değiştirmek için aşağıdaki yer tutucu **$subscriptionId** geçerli Azure aboneliği ID'ye sahip değişken 

    ```PowerShell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzureRmSubscription -SubscriptionId $subscriptionId
    ```

1.  Çağırarak Laboratuvar nesne get **Get-AzureRmResource** cmdlet'i. Aşağıdaki yer tutucularını değiştirin **$labRg** ve **$labName** değişkenleri ortamınız için uygun değerlerle. 

    ```PowerShell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```
 
1.  Laboratuvar, laboratuvarı nesnesinden depolama hesabı ve lab depolama hesabı anahtar değerleri alın. 

    ```PowerShell
    $labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
    $labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Değiştirmek için aşağıdaki yer tutucu **$vhdUri** karşıya yüklenen VHD dosyasına URI'yı içeren değişken. VHD dosyasının URI, Azure portalında depolama hesabının blob dikey penceresinden alabilirsiniz.

    ```PowerShell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  Özel görüntü kullanarak oluşturduğunuz **New-AzureRmResourceGroupDeployment** cmdlet'i. Aşağıdaki yer tutucularını değiştirin **$customImageName** ve **$customImageDescription** ortamınız için anlamlı adlar değişkenleri.

    ```PowerShell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasından özel bir görüntü oluşturmak için PowerShell Betiği

Aşağıdaki PowerShell betiğini bir VHD dosyasından özel bir görüntü oluşturmak için kullanılabilir. (Başlangıç ve köşeli parantez ile biten) yer tutucuları, gereksinimlerinize uygun değerlerle değiştirin. 

```PowerShell
# Log in to your Azure account.  
Connect-AzureRmAccount

# Select the desired Azure subscription. 
$subscriptionId = '<Specify your subscription ID here>'
Select-AzureRmSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzureRmResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the lab storage account and lab storage account key values.
$labStorageAccount = Get-AzureRmResource -ResourceId $lab.Properties.defaultStorageAccount 
$labStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set the URI of the VHD file.  
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image. 
New-AzureRmResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/Samples/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel resimler veya formüller?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel resimler kopyalama](http://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınızı için bir VM ekleme](devtest-lab-add-vm.md)
