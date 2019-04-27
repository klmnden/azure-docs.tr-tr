---
title: Azure PowerShell ile DevTest labs'deki bir sanal makine oluşturun. | Microsoft Docs
description: Oluşturma ve Azure PowerShell ile sanal makineleri yönetmek için Azure DevTest Labs'i kullanmayı öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/02/2019
ms.author: spelluru
ms.openlocfilehash: a9629cd14c71a163612c2c4ba3c7b109a52b91ad
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60622448"
---
# <a name="create-a-virtual-machine-with-devtest-labs-using-azure-powershell"></a>Azure PowerShell kullanarak DevTest Labs ile bir sanal makine oluşturun
Bu makalede Azure PowerShell kullanarak Azure DevTest Labs'de sanal makine oluşturma işlemini gösterir. Azure DevTest labs'deki bir laboratuvara sanal makinelerin oluşturmayı otomatikleştirmek için PowerShell komut dosyalarını kullanabilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Başlamadan önce:

- [Bir laboratuvar oluşturma](devtest-lab-create-lab.md) komutları ve komut dosyası, bu makaledeki test etmek için var olan bir laboratuvar kullanmak istemiyorsanız. 
- [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps?view=azps-1.7.0) veya Azure portalında tümleşik Azure Cloud Shell kullanın. 

## <a name="powershell-script"></a>PowerShell betiği
Bu bölümde örnek betikte [Invoke-AzResourceAction](/powershell/module/az.resources/invoke-azresourceaction?view=azps-1.7.0) cmdlet'i.  Bu cmdlet Laboratuvar kaynak kimliği, gerçekleştirilecek eylem adını alır (`createEnvironment`), ve gerekli parametreleri bu eylemi gerçekleştirin. Tüm sanal makine açıklama özellikleri içeren bir karma tablo parametrelerdir. 

```powershell
[CmdletBinding()]

Param(
[Parameter(Mandatory = $false)]  $SubscriptionId,
[Parameter(Mandatory = $true)]   $LabResourceGroup,
[Parameter(Mandatory = $true)]   $LabName,
[Parameter(Mandatory = $true)]   $NewVmName,
[Parameter(Mandatory = $true)]   $UserName,
[Parameter(Mandatory = $true)]   $Password
)
 
pushd $PSScriptRoot

try {
    if ($SubscriptionId -eq $null) {
        $SubscriptionId = (Get-AzContext).Subscription.SubscriptionId
    }

    $API_VERSION = '2016-05-15'
    $lab = Get-AzResource -ResourceId "/subscriptions/$SubscriptionId/resourceGroups/$LabResourceGroup/providers/Microsoft.DevTestLab/labs/$LabName"

    if ($lab -eq $null) {
       throw "Unable to find lab $LabName resource group $LabResourceGroup in subscription $SubscriptionId."
    }

    #For this example, we are getting the first allowed subnet in the first virtual network
    #  for the lab.
    #If a specific virtual network is needed use | to find it. 
    #ie $virtualNetwork = @(Get-AzResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION) | Where-Object Name -EQ "SpecificVNetName"

    $virtualNetwork = @(Get-AzResource -ResourceType  'Microsoft.DevTestLab/labs/virtualnetworks' -ResourceName $LabName -ResourceGroupName $lab.ResourceGroupName -ApiVersion $API_VERSION)[0]

    $labSubnetName = $virtualNetwork.properties.allowedSubnets[0].labSubnetName

    #Prepare all the properties needed for the createEnvironment
    # call used to create the new VM.
    # The properties will be slightly different depending on the base of the vm
    # (a marketplace image, custom image or formula).
    # The setup of the virtual network to be used may also affect the properties.
    # This sample includes the properties to add an additional disk under dataDiskParameters
    
    $parameters = @{
       "name"      = $NewVmName;
       "location"  = $lab.Location;
       "properties" = @{
          "labVirtualNetworkId"     = $virtualNetwork.ResourceId;
          "labSubnetName"           = $labSubnetName;
          "notes"                   = "Windows Server 2016 Datacenter";
          "osType"                  = "windows"
          "galleryImageReference"   = @{
             "offer"     = "WindowsServer";
             "publisher" = "MicrosoftWindowsServer";
             "sku"       = "2016-Datacenter";
             "osType"    = "Windows";
             "version"   = "latest"
          };
          "size"                    = "Standard_DS2_v2";
          "userName"                = $UserName;
          "password"                = $Password;
          "disallowPublicIpAddress" = $true;
          "dataDiskParameters" = @(@{
            "attachNewDataDiskOptions" = @{
                "diskName" = "adddatadisk"
                "diskSizeGiB" = "1023"
                "diskType" = "Standard"
                }
          "hostCaching" = "ReadWrite"
          })
       }
    }
    
    #The following line is the same as invoking
    # https://azure.github.io/projects/apis/#!/Labs/Labs_CreateEnvironment rest api

    Invoke-AzResourceAction -ResourceId $lab.ResourceId -Action 'createEnvironment' -Parameters $parameters -ApiVersion $API_VERSION -Force -Verbose
}
finally {
   popd
}
```

Yukarıdaki komut sanal makinenin özelliklerini işletim sistemi olarak Windows Server 2016 DataCenter ile bir sanal makine oluşturmak olanak tanır. Her sanal makine türü için bu özellikleri biraz farklı olacaktır. [Sanal makineyi tanımlama](#define-virtual-machine) bölümü, bu komut dosyasını kullanmak için hangi özellikleri belirlemek nasıl gösterir.

Aşağıdaki komutu bir dosya adı kaydedilmiş betik çalıştıran bir örnek sağlar: Oluştur-LabVirtualMachine.ps1. 

```powershell
 PS> .\Create-LabVirtualMachine.ps1 -ResourceGroupName 'MyLabResourceGroup' -LabName 'MyLab' -userName 'AdminUser' -password 'Password1!' -VMName 'MyLabVM'
```

## <a name="define-virtual-machine"></a>Sanal makineyi tanımlama
Bu bölümde, oluşturmak istediğiniz sanal makine türüne özgü özelliklerini alma işlemini göstermektedir. 

### <a name="use-azure-portal"></a>Azure portalı kullanma
Azure portalında bir VM oluştururken, bir Azure Resource Manager şablonu oluşturabilirsiniz. VM oluşturma işleminin tamamlanması gerekmez. Şablon görene kadar yalnızca adımları izleyin. Oluşturulan VM'yi bir laboratuvara zaten yoksa, gerekli JSON açıklama almak için en iyi yolu budur. 

1. [Azure portalına](https://portal.azure.com) gidin.
2. Seçin **tüm hizmetleri** sol gezinti menüsünde.
3. Arayın ve seçin **DevTest Labs** hizmetler listesinden. 
4. Üzerinde **DevTest Labs** sayfasında, laboratuvarınızda Laboratuvar listesinde seçin.
5. Laboratuvarınız için giriş sayfasında, seçin **+ Ekle** araç. 
6. Seçin bir **temel görüntü** VM için. 
7. Seçin **Otomasyon seçenekleri** yukarıdaki sayfanın alt kısmındaki **Gönder** düğmesi. 
8. Gördüğünüz **Azure Resource Manager şablonu** sanal makine oluşturma. 
9. JSON kesimdeki **kaynakları** bölümünde daha önce seçilen görüntü türüne ilişkin tanımı bulunur. 

    ```json
    {
      "apiVersion": "2018-10-15-preview",
      "type": "Microsoft.DevTestLab/labs/virtualmachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "labVirtualNetworkId": "[variables('labVirtualNetworkId')]",
        "notes": "Windows Server 2019 Datacenter",
        "galleryImageReference": {
          "offer": "WindowsServer",
          "publisher": "MicrosoftWindowsServer",
          "sku": "2019-Datacenter",
          "osType": "Windows",
          "version": "latest"
        },
        "size": "[parameters('size')]",
        "userName": "[parameters('userName')]",
        "password": "[parameters('password')]",
        "isAuthenticationWithSshKey": false,
        "labSubnetName": "[variables('labSubnetName')]",
        "disallowPublicIpAddress": true,
        "storageType": "Standard",
        "allowClaim": false,
        "networkInterface": {
          "sharedPublicIpAddressConfiguration": {
            "inboundNatRules": [
              {
                "transportProtocol": "tcp",
                "backendPort": 3389
              }
            ]
          }
        }
      }
    }
    ```

Bu örnekte, nasıl Azure Marketi görüntü tanımını almak bkz. Özel bir görüntü, bir formül olarak ayarlayın veya bir ortam tanımı aynı şekilde alabilirsiniz. Sanal makine için gerekli tüm yapıtlar ekleyin ve gerekli tüm Gelişmiş ayarlar. Gerekli alanları ve isteğe bağlı alanları için değerleri girdikten sonra önce seçerek **Otomasyon seçenekleri** düğmesi.

### <a name="use-azure-rest-api"></a>Azure REST API kullanma
Aşağıdaki yordam, REST API kullanarak görüntü özelliklerini almak için adımları sunar: Bu adımlar, yalnızca bir laboratuvarda var olan bir VM için geçerlidir. 

1. Gidin [sanal makineler - liste](/rest/api/dtl/virtualmachines/list) sayfasında **deneyin** düğmesi. 
2. **Azure aboneliğinizi** seçin.
3. Girin **Laboratuvar için kaynak grubu**.
4. Girin **Laboratuvar adı**. 
5. **Çalıştır**'ı seçin.
6. Gördüğünüz **görüntü özelliklerini** VM oluşturulduğu üzerinde temel. 



## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki içeriğe bakın: [Azure DevTest Labs için Azure PowerShell belgeleri](/powershell/module/az.devtestlabs/)
