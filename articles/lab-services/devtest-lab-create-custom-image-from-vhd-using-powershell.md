---
title: PowerShell kullanarak bir VHD dosyasından bir Azure DevTest Labs özel görüntü oluşturma | Microsoft Docs
description: Azure DevTest Labs PowerShell kullanarak bir VHD dosyasından özel görüntüsünü oluşturulmasını otomatikleştirin
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
ms.openlocfilehash: c1cdb64e4c8c99eeca4cc66c0d0ad2b755144917
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60201962"
---
# <a name="create-a-custom-image-from-a-vhd-file-using-powershell"></a>PowerShell kullanarak bir VHD dosyasından özel bir görüntü oluşturma

[!INCLUDE [devtest-lab-create-custom-image-from-vhd-selector](../../includes/devtest-lab-create-custom-image-from-vhd-selector.md)]

[!INCLUDE [devtest-lab-custom-image-definition](../../includes/devtest-lab-custom-image-definition.md)]

[!INCLUDE [devtest-lab-upload-vhd-options](../../includes/devtest-lab-upload-vhd-options.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, PowerShell kullanarak bir VHD dosyasından bir özel görüntü oluşturmada size yol:

1. Bir PowerShell komut isteminde aşağıdaki çağrısını Azure hesabınızla oturum açın **Connect AzAccount** cmdlet'i.

    ```powershell
    Connect-AzAccount
    ```

1.  Çağırarak istediğiniz Azure aboneliğini seçin **seçin AzSubscription** cmdlet'i. Aşağıdaki yer tutucusunu değiştirin **$subscriptionId** bir geçerli Azure abonelik kimliğinizle değişken

    ```powershell
    $subscriptionId = '<Specify your subscription ID here>'
    Select-AzSubscription -SubscriptionId $subscriptionId
    ```

1.  Laboratuvar nesnesi çağırarak alma **Get-AzResource** cmdlet'i. Aşağıdaki yer tutucularını değiştirin **$labRg** ve **$labName** değişkenleri ortamınız için uygun değerlerle.

    ```powershell
    $labRg = '<Specify your lab resource group name here>'
    $labName = '<Specify your lab name here>'
    $lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)
    ```

1.  Laboratuvar, laboratuvarı nesnesinden depolama hesabı ve Laboratuvar depolama hesap anahtarı değerleriyle alın.

    ```powershell
    $labStorageAccount = Get-AzResource -ResourceId $lab.Properties.defaultStorageAccount
    $labStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value
    ```

1.  Aşağıdaki yer tutucusunu değiştirin **$vhdUri** URI'yı kullanarak karşıya VHD dosyası için içeren değişken. VHD dosya URI'si, Azure portalında depolama hesabının blob dikey penceresinden alabilirsiniz.

    ```powershell
    $vhdUri = '<Specify the VHD URI here>'
    ```

1.  Özel görüntü kullanarak oluşturma **yeni AzResourceGroupDeployment** cmdlet'i. Aşağıdaki yer tutucularını değiştirin **$customImageName** ve **$customImageDescription** değişkenleri ortamınız için anlamlı adlar.

    ```powershell
    $customImageName = '<Specify the custom image name>'
    $customImageDescription = '<Specify the custom image description>'

    $parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

    New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
    ```

## <a name="powershell-script-to-create-a-custom-image-from-a-vhd-file"></a>Bir VHD dosyasından özel bir görüntü oluşturmak için PowerShell Betiği

Aşağıdaki PowerShell Betiği, bir VHD dosyasından özel bir görüntü oluşturmak için kullanılabilir. Gereksinimlerinize uygun değerlerle (başlangıç ve bitiş açılı ayraçlar ile) yer tutucularını değiştirin.

```powershell
# Log in to your Azure account.
Connect-AzAccount

# Select the desired Azure subscription.
$subscriptionId = '<Specify your subscription ID here>'
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab object.
$labRg = '<Specify your lab resource group name here>'
$labName = '<Specify your lab name here>'
$lab = Get-AzResource -ResourceId ('/subscriptions/' + $subscriptionId + '/resourceGroups/' + $labRg + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the lab storage account and lab storage account key values.
$labStorageAccount = Get-AzResource -ResourceId $lab.Properties.defaultStorageAccount
$labStorageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $labStorageAccount.ResourceGroupName -Name $labStorageAccount.ResourceName)[0].Value

# Set the URI of the VHD file.
$vhdUri = '<Specify the VHD URI here>'

# Set the custom image name and description values.
$customImageName = '<Specify the custom image name>'
$customImageDescription = '<Specify the custom image description>'

# Set up the parameters object.
$parameters = @{existingLabName="$($lab.Name)"; existingVhdUri=$vhdUri; imageOsType='windows'; isVhdSysPrepped=$false; imageName=$customImageName; imageDescription=$customImageDescription}

# Create the custom image.
New-AzResourceGroupDeployment -ResourceGroupName $lab.ResourceGroupName -Name CreateCustomImage -TemplateUri 'https://raw.githubusercontent.com/Azure/azure-devtestlab/master/samples/DevTestLabs/QuickStartTemplates/201-dtl-create-customimage-from-vhd/azuredeploy.json' -TemplateParameterObject $parameters
```

## <a name="related-blog-posts"></a>İlgili blog gönderileri

- [Özel görüntü veya formül?](https://blogs.msdn.microsoft.com/devtestlab/2016/04/06/custom-images-or-formulas/)
- [Azure DevTest Labs arasında özel görüntüleri kopyalama](https://www.visualstudiogeeks.com/blog/DevOps/How-To-Move-CustomImages-VHD-Between-AzureDevTestLabs#copying-custom-images-between-azure-devtest-labs)

## <a name="next-steps"></a>Sonraki adımlar

- [Laboratuvarınız için bir VM ekleme](devtest-lab-add-vm.md)
