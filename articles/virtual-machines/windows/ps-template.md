---
title: Azure'da bir şablondan Windows VM oluşturma | Microsoft Docs
description: Kolayca yeni bir Windows VM oluşturmak için bir Resource Manager şablonu ve PowerShell kullanın.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 19129d61-8c04-4aa9-a01f-361a09466805
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/03/2019
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: fe3bbb3a998c0457cd504da5a785c805fc1b73e5
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977499"
---
# <a name="create-a-windows-virtual-machine-from-a-resource-manager-template"></a>Bir Resource Manager şablonundan bir Windows sanal makine oluşturma

Bu makalede, PowerShell kullanarak bir Azure Resource Manager şablonu dağıtma işlemini göstermektedir. Oluşturduğunuz şablonun yeni bir sanal ağda tek bir alt ağ ile Windows Server çalıştıran tek bir sanal makine dağıtır.

Sanal makine kaynağı ayrıntılı bir açıklaması için bkz. [sanal makineler bir Azure Resource Manager şablonunda](template-description.md). Bir şablonda tüm kaynaklar hakkında daha fazla bilgi için bkz. [Azure Resource Manager şablonu Kılavuzu](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Bu, bu makaledeki adımların tamamlanması yaklaşık beş dakika sürer.

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

PowerShell'i yerel olarak yükleyip kullanmayı tercih ederseniz bu öğretici, Azure PowerShell modülü 5.3 veya sonraki bir sürümü gerektirir. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-az-ps). PowerShell'i yerel olarak çalıştırıyorsanız, aynı zamanda çalıştırmak ihtiyacınız `Connect-AzAccount` Azure ile bir bağlantı oluşturmak için.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm kaynaklar içinde dağıtılmış olması gereken bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md).

1. Kaynakların oluşturulabileceği kullanılabilir konumların bir listesini alın.
   
    ```powershell   
    Get-AzLocation | sort-object DisplayName | Select DisplayName
    ```

2. Seçtiğiniz konumda kaynak grubu oluşturun. Bu örnek adlı bir kaynak grubu oluşturmayı gösterir **myResourceGroup** içinde **Batı ABD** konumu:

    ```powershell   
    New-AzResourceGroup -Name "myResourceGroup" -Location "West US"
    ```

## <a name="create-the-files"></a>Dosyaları oluşturma

Bu adımda, kaynakları dağıtan bir şablon dosyası ve şablon parametre değerlerini sağlayan bir parametre dosyası oluşturun. Azure Resource Manager işlemlerini gerçekleştirmek için kullanılan bir yetkilendirme dosyası oluşturabilir. 

1. Adlı bir dosya oluşturun *CreateVMTemplate.json* ve JSON kod ekleyin. Değiştirin `domainNameLabel` kendi benzersiz bir ada sahip.

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
        "adminUsername": { "type": "string" },
        "adminPassword": { "type": "securestring" }
      },
      "variables": {
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks','myVNet')]", 
        "subnetRef": "[concat(variables('vnetID'),'/subnets/mySubnet')]" 
      },
      "resources": [
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/publicIPAddresses",
          "name": "myPublicIPAddress",
          "location": "[resourceGroup().location]",
          "properties": {
            "publicIPAllocationMethod": "Dynamic",
            "dnsSettings": {
              "domainNameLabel": "myresourcegroupdns1"
            }
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/virtualNetworks",
          "name": "myVNet",
          "location": "[resourceGroup().location]",
          "properties": {
            "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
            "subnets": [
              {
                "name": "mySubnet",
                "properties": { "addressPrefix": "10.0.0.0/24" }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-03-30",
          "type": "Microsoft.Network/networkInterfaces",
          "name": "myNic",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/publicIPAddresses/', 'myPublicIPAddress')]",
            "[resourceId('Microsoft.Network/virtualNetworks/', 'myVNet')]"
          ],
          "properties": {
            "ipConfigurations": [
              {
                "name": "ipconfig1",
                "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "publicIPAddress": { "id": "[resourceId('Microsoft.Network/publicIPAddresses','myPublicIPAddress')]" },
                  "subnet": { "id": "[variables('subnetRef')]" }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2016-04-30-preview",
          "type": "Microsoft.Compute/virtualMachines",
          "name": "myVM",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Network/networkInterfaces/', 'myNic')]"
          ],
          "properties": {
            "hardwareProfile": { "vmSize": "Standard_DS1" },
            "osProfile": {
              "computerName": "myVM",
              "adminUsername": "[parameters('adminUsername')]",
              "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
              "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "2012-R2-Datacenter",
                "version": "latest"
              },
              "osDisk": {
                "name": "myManagedOSDisk",
                "caching": "ReadWrite",
                "createOption": "FromImage"
              }
            },
            "networkProfile": {
              "networkInterfaces": [
                {
                  "id": "[resourceId('Microsoft.Network/networkInterfaces','myNic')]"
                }
              ]
            }
          }
        }
      ]
    }
    ```

2. Adlı bir dosya oluşturun *Parameters.json* ve bu JSON kodunu ekleyin:

    ```json
    {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {
      "adminUserName": { "value": "azureuser" },
        "adminPassword": { "value": "Azure12345678" }
      }
    }
    ```

3. Yeni depolama hesabı ve kapsayıcı oluşturma:

    ```powershell
    $storageName = "st" + (Get-Random)
    New-AzStorageAccount -ResourceGroupName "myResourceGroup" -AccountName $storageName -Location "West US" -SkuName "Standard_LRS" -Kind Storage
    $accountKey = (Get-AzStorageAccountKey -ResourceGroupName myResourceGroup -Name $storageName).Value[0]
    $context = New-AzureStorageContext -StorageAccountName $storageName -StorageAccountKey $accountKey 
    New-AzureStorageContainer -Name "templates" -Context $context -Permission Container
    ```

4. Depolama hesabına dosya yükleme:

    ```powershell
    Set-AzureStorageBlobContent -File "C:\templates\CreateVMTemplate.json" -Context $context -Container "templates"
    Set-AzureStorageBlobContent -File "C:\templates\Parameters.json" -Context $context -Container templates
    ```

    Değişiklik dosyaların depolandığı konumun dosya yolları.

## <a name="create-the-resources"></a>Kaynakları oluşturma

Parametreleri kullanarak şablonu dağıtın:

```powershell
$templatePath = "https://" + $storageName + ".blob.core.windows.net/templates/CreateVMTemplate.json"
$parametersPath = "https://" + $storageName + ".blob.core.windows.net/templates/Parameters.json"
New-AzResourceGroupDeployment -ResourceGroupName "myResourceGroup" -Name "myDeployment" -TemplateUri $templatePath -TemplateParameterUri $parametersPath 
```

> [!NOTE]
> Ayrıca, şablon ve parametre yerel dosyalarından da dağıtabilirsiniz. Daha fazla bilgi için bkz. [Azure PowerShell kullanarak Azure depolama ile](../../storage/common/storage-powershell-guide-full.md).

## <a name="next-steps"></a>Sonraki Adımlar

- Dağıtımla ilgili sorunlar varsa, göz atın [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](../../resource-manager-common-deployment-errors.md).
- Oluşturma ve içinde bir sanal makineyi yönetmeyi öğrenin [oluşturun ve Azure PowerShell modülü ile Windows Vm'leri yönetme](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Şablonları oluşturma hakkında daha fazla bilgi için JSON söz dizimi ve dağıttığınız kaynak türleri için özellikleri görüntüleyin:

* [Microsoft.Network/publicIPAddresses](/azure/templates/microsoft.network/publicipaddresses)
* [Microsoft.Network/virtualNetworks](/azure/templates/microsoft.network/virtualnetworks)
* [Microsoft.Network/networkInterfaces](/azure/templates/microsoft.network/networkinterfaces)
* [Microsoft.Compute/virtualMachines](/azure/templates/microsoft.compute/virtualmachines)

