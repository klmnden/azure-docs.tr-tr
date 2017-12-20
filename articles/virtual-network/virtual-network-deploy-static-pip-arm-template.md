---
title: "Bir statik genel IP adresi ile - Azure Resource Manager şablonu bir VM oluşturma | Microsoft Docs"
description: "Bir Azure Resource Manager şablonu kullanarak bir statik genel IP adresi ile VM oluşturmayı öğrenin."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2f503aa60fdd9b7cf66ef482a1041e34c88e5c01
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu kullanarak bir statik genel IP adresiyle bir VM oluşturma

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Şablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (Klasik)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md). Bu makalede, Klasik dağıtım modeli yerine en yeni dağıtımlar için Microsoft önerir Resource Manager dağıtım modelini kullanarak yer almaktadır.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Bir şablon dosyasındaki ortak IP adresi kaynakları
Görüntüleyin ve indirme [örnek şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

Aşağıdaki bölümde yukarıdaki senaryoyu temel genel IP kaynağı tanımını gösterir:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Bildirim **Publicıpallocationmethod** ayarlamak için özellik *statik*. Bu özellik ya da olabilir *dinamik* (varsayılan değer) veya *statik*. Atanan genel IP adresi hiçbir zaman değiştirecek statik garanti ayarlama.

Aşağıdaki bölümde, bir ağ arabirimi genel IP adresi ilişkilendirmesini gösterilmektedir:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Bildirim **Publicıpaddress** işaret eden özelliği **kimliği** adlı bir kaynağın **variables('webVMSetting').pipName**. Yukarıda gösterilen ortak IP kaynak adının olmasıdır.

Son olarak, ağ arabiriminin yukarıdaki listede **networkProfile** oluşturulan VM özelliği.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-the-template-by-using-click-to-deploy"></a>Tıklayarak dağıtma kullanarak şablonu dağıtma

Genel depoda yer alan örnek şablonda, yukarıdaki senaryoyu oluşturmak için kullanılan varsayılan değerleri içeren parametre dosyası kullanılmaktadır. Dağıtmak için bir tıklamayla bu şablonu dağıtmak için **Azure'a Dağıt** Readme.md dosyasında [statik PIP VM](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) şablonu. İstenirse, varsayılan parametre değerlerini değiştirin ve boş parametreler için değerler girin.  Statik bir genel IP adresine sahip bir sanal makine oluşturmak için Portalı'ndaki yönergeleri izleyin.

## <a name="deploy-the-template-by-using-powershell"></a>PowerShell kullanarak şablonu dağıtma

PowerShell kullanarak yüklediğiniz şablonu dağıtmak için aşağıdaki adımları izleyin.

1. Azure PowerShell'i hiç kullanmadıysanız, bölümündeki adımları tamamlamanız [yükleme ve yapılandırma Azure PowerShell](/powershell/azure/overview) makalesi.
2. Bir PowerShell konsolunda çalıştırın `New-AzureRmResourceGroup` gerekiyorsa, yeni bir kaynak grubu oluşturmak için cmdlet'i. Oluşturulan bir kaynak grubu zaten varsa, 3. adıma gidin.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Beklenen çıktı:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. Bir PowerShell konsolunda çalıştırın `New-AzureRmResourceGroupDeployment` şablonu dağıtmak için cmdlet.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Beklenen çıktı:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Azure CLI kullanarak şablonu dağıtma
Azure CLI kullanarak şablonu dağıtmak için aşağıdaki adımları tamamlayın:

1. Hiç Azure CLI kullanmadıysanız, adımları [Azure CLI'yi yükleme ve yapılandırma](../cli-install-nodejs.md) makalenin yükleyin ve yapılandırın.
2. Çalıştırma `azure config mode` aşağıda gösterildiği gibi Resource Manager moduna geçmek için komutu.

    ```azurecli
    azure config mode arm
    ```

    Yukarıdaki komut için beklenen çıktı:

        info:    New mode is arm

3. Açık [parametre dosyası](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json)içeriğini seçin ve bilgisayarınızdaki bir dosyaya kaydedin. Bu örnekte, parametreleri adlı bir dosyaya kaydedilir *parameters.json*. İsterseniz dosya içinde parametre değerlerini değiştirmek ancak en azından Admınpassword parametresinin değeri için benzersiz ve karmaşık bir parola değiştirme önerilir.
4. Çalıştırma `azure group deployment create` , yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre kullanarak yeni Vnet'i dağıtmak için komut dosyaları. Aşağıdaki komutta, <path> yolu dosyasına kaydedilir. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Beklenen çıktı: (kullanılan parametre değerlerini listeler)

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

