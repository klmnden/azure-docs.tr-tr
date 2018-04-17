---
title: Birden çok NIC - Azure Resource Manager şablonu ile bir VM oluşturma | Microsoft Docs
description: Bir VM ile birden çok NIC bir Azure Resource Manager şablonu kullanarak oluşturun.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 85bfa264c6cf2b0586816a47b3ab72f3aee8ec96
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Bir şablon kullanarak birden çok NIC ile VM oluşturma
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure’da kaynak oluşturmak ve bunlarla çalışmak için iki farklı dağıtım modeli vardır:  [Resource Manager ve klasik](../resource-manager-deployment-model.md).  Bu makale, Microsoft’un çoğu yeni dağıtım için [klasik dağıtım modeli](virtual-network-deploy-multinic-classic-ps.md) yerine önerdiği Resource Manager dağıtım modelini açıklamaktadır.
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

Adlı bir kaynak grubu aşağıdaki adımları kullanın *IaaSStory* WEB sunucuları ve bir kaynak grubu için adlı *IaaSStory arka uç* DB sunucuları için.

## <a name="prerequisites"></a>Önkoşullar
DB sunucuları oluşturabilmeniz için önce oluşturmanız gerekir *IaaSStory* bu senaryo için gereken tüm kaynakların kaynak grubuyla. Bu kaynakları oluşturmak için aşağıdaki adımları tamamlayın:

1. Gidin [şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Şablon sayfasındaki sağ tarafındaki **üst kaynak grubu**, tıklatın **Azure'a Dağıt**.
3. Gerekirse, parametre değerlerini değiştirin, sonra kaynak grubu dağıtmak için Azure Önizleme Portalı'ndaki adımları izleyin.

> [!IMPORTANT]
> Depolama hesabı adları benzersiz olduğundan emin olun. Azure üzerinde yinelenen depolama hesabı adları sahip olamaz.
> 

## <a name="understand-the-deployment-template"></a>Dağıtım şablonu anlama
Bu belge ile sağlanan şablonunun dağıtmadan önce ne yaptığını anladığınızdan emin olun. Aşağıdaki adımlarda, şablon iyi bir genel bakış sunulmaktadır:

1. Gidin [şablon sayfasına](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Tıklatın **azuredeploy.json** şablon dosyasını açın.
3. Bildirim *osType* aşağıda listelenen parametre. Bu parametre hangi VM görüntüsü için veritabanı sunucusunu kullanmak üzere seçmek için kullanılır, birden çok işletim sistemi ile birlikte ilgili ayarları.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS to use for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Değişkenleri listesini aşağı kaydırın ve tanımını denetleyin **dbVMSetting** değişkenleri, aşağıda listelenen. İçinde yer alan dizi öğelerin birine aldığı **dbVMSettings** değişkeni. Yazılım geliştirme ifadeyle bilginiz varsa, görüntüleyebileceğiniz **dbVMSettings** değişken karma tablo veya bir sözlük olarak.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Windows arka uç SQL çalıştıran VM'ler dağıtmaya karar verdiğinizi varsayalım. Ardından değeri **osType** olacaktır *Windows*ve **dbVMSetting** değişkeni ilkdeğeritemsiledenaşağıdalistelenenöğeiçerebilir**dbVMSettings** değişkeni.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. Bildirim **vmSize** değeri içeren *Standard_DS3*. Yalnızca belirli VM boyutları için birden çok NIC kullanılmasına izin verin. Hangi VM boyutları okuyarak birden çok NIC destek doğrulayabilirsiniz [Windows VM boyutları](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux VM boyutları](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) makaleleri.

7. Ekranı aşağı kaydırarak **kaynakları** ve ilk öğe dikkat edin. Bir depolama hesabı açıklar. Bu depolama hesabını, her veritabanı VM tarafından kullanılan veri diskleri korumak için kullanılır. Bu senaryoda, her veritabanı VM normal depolamada depolanan bir işletim sistemi diski ve SSD (premium) depolamada depolanan iki veri diski var.

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Aşağıda listelenen sonraki kaynak aşağı kaydırın. Bu kaynak VM her veritabanında veritabanı erişimi için kullanılan NIC temsil eder. Kullanımına dikkat edin **kopya** bu kaynak işlevinde. Şablon, istediğiniz üzerinde birçok VM tabanlı olarak dağıtmanıza olanak tanır **dbCount** parametresi. Bu nedenle NIC ile aynı miktarda veritabanı erişim, her VM için bir tane oluşturmanız gerekir.

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Aşağıda listelenen sonraki kaynak aşağı kaydırın. Bu kaynak her veritabanındaki VM yönetimi için kullanılan NIC temsil eder. Bir kez daha, bu NIC'ler birini her veritabanı için VM gerekir. Bildirim **networkSecurityGroup** RDP/SSH yalnızca bu NIC'e erişmesini sağlayan bir NSG bağlama öğesi.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Aşağıda listelenen sonraki kaynak aşağı kaydırın. Bu kaynak, tüm veritabanı VM'ler tarafından paylaşılmak üzere bir kullanılabilirlik kümesi temsil eder. Bu şekilde, her zaman olacağını bir VM bakım sırasında çalışan kümesindeki garanti.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Sonraki kaynak aşağıya doğru kaydırın. Bu kaynak VM'ler, veritabanı ilk birkaç satırı aşağıda listelenen görülen temsil eder. Kullanımına dikkat edin **kopya** yeniden, birden çok VM temel alınarak oluşturulan sağlama işlev **dbCount** parametresi. Ayrıca fark **dependsOn** koleksiyonu. Kullanılabilirlik kümesi ve depolama hesabı yanı sıra VM dağıtılmadan önce oluşturulması için gerekli olan iki NIC listeler.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Aşağı kaydırın VM kaynak **networkProfile** aşağıda listelenen öğesi. Her VM için başvuru olan iki NIC olduğuna dikkat edin. Bir VM için birden çok NIC oluşturduğunuzda, ayarlamanız gerekir **birincil** özelliği için NIC'ler birinin *true*ve rest *false*.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-the-arm-template-by-using-click-to-deploy"></a>Tıklayarak dağıtma kullanarak ARM şablonu dağıtma

> [!IMPORTANT]
> İzlediğinizden emin olun [ön koşullar](#Pre-requisites) aşağıdaki yönergeleri izleyerek önce adımları.
> 

Genel depoda yer alan örnek şablonda, yukarıdaki senaryoyu oluşturmak için kullanılan varsayılan değerleri içeren parametre dosyası kullanılmaktadır. Dağıtmak için bir tıklamayla bu şablonu dağıtmak için izlemeniz [bu bağlantıyı](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), sağındaki **arka uç kaynak grubu (belgelere bakın)** tıklatın **Azure'a Dağıt**, Değiştir Varsayılan parametre değerleri gerekirse ve Portalı'ndaki yönergeleri izleyin.

Aşağıdaki şekilde dağıtımdan sonra yeni kaynak grubu içeriğini gösterir.

![Arka uç kaynak grubu](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-the-template-by-using-powershell"></a>PowerShell kullanarak şablonu dağıtma
İndirilen PowerShell kullanarak şablonu dağıtmak için PowerShell'i yükleme ve içindeki adımları tamamlayarak yapılandırma [PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview) makalesi ve ardından aşağıdaki adımları tamamlayın:

Çalıştırma **`New-AzureRmResourceGroup`** şablonu kullanarak bir kaynak grubu oluşturmak için cmdlet'i.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Beklenen çıktı:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-the-template-by-using-the-azure-cli"></a>Azure CLI kullanarak şablonu dağıtma
Azure CLI’yi kullanarak şablonu dağıtmak için aşağıdaki adımları uygulayın.

1. Hiç Azure CLI kullanmadıysanız bkz. [Azure CLI’yi Yükleme ve Yapılandırma](../cli-install-nodejs.md); sonra da, Azure hesabınızı ve aboneliğinizi seçtiğiniz noktaya kadar yönergeleri uygulayın.
2. Resource Manager moduna geçmek için **`azure config mode`** komutunu aşağıda gösterildiği gibi çalıştırın.

    ```azurecli
    azure config mode arm
    ```

    Beklenen çıktı aşağıdaki gibidir:

        info:    New mode is arm

3. Açık [parametre dosyası](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json)içeriğini seçin ve bilgisayarınızdaki bir dosyaya kaydedin. Bu örnekte parametre dosyasını *parameters.json* dosyasına kaydettik.
4. Yukarıda indirdiğiniz ve değiştirdiğiniz şablonu ve parametre dosyalarını kullanarak yeni VNet’i dağıtmak için **`azure group deployment create`** cmdlet’ini çalıştırın. Çıktıdan sonra gösterilen listede kullanılan parametreler açıklanmaktadır.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Beklenen çıktı:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

