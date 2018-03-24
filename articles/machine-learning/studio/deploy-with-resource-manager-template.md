---
title: Bir Machine Learning çalışma alanı Azure Resource Manager ile dağıtma | Microsoft Docs
description: Azure Resource Manager şablonu kullanarak Azure Machine Learning için bir çalışma alanı dağıtma
services: machine-learning
documentationcenter: ''
author: heatherbshapiro
ms.author: hshapiro
manager: hjerez
editor: cgronlun
ms.assetid: 4955ac4d-ff99-4908-aa27-69b6bfcc8e85
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/05/2018
ms.openlocfilehash: 4ba75b1d1740486649cc8d4e012c3f780488cbe0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="deploy-machine-learning-workspace-using-azure-resource-manager"></a>Azure Resource Manager’ı Kullanarak Machine Learning Çalışma Alanını Dağıtma
## <a name="introduction"></a>Giriş
Dağıtım şablonu, ölçeklenebilir bir şekilde sağlayarak kaydeder bir Azure Kaynak Yöneticisi'ni kullanarak bir doğrulama birbirine bağlı bileşenlerle dağıtın ve mekanizması yeniden deneyin. Azure Machine Learning çalışma alanlarınızı ayarlamak için örneğin, ilk Azure storage hesabı yapılandırın ve sonra çalışma alanınızda dağıtmak gerekir. El ile çalışma alanları yüzlerce için bunu düşünün. Daha kolay bir Azure Machine Learning çalışma alanı ve tüm bağımlılıklarını dağıtmak için bir Azure Resource Manager şablonu kullanmaya alternatiftir. Bu makalede bu işlemi adım adım alır. Bir harika Azure Kaynak Yöneticisi'nin için bkz: genel bakış [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Adım adım: bir Machine Learning çalışma alanı oluşturma
Biz bir Azure kaynak grubu oluşturun ve ardından yeni bir Azure depolama hesabı ve yeni bir Azure Machine Learning Resource Manager şablonu kullanarak çalışma alanı dağıtın. Dağıtım tamamlandıktan sonra biz (birincil anahtar, Workspaceıd ve çalışma URL'sine) oluşturulmuş çalışma alanları hakkında önemli bilgiler yazdırır.

### <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma
Bir Machine Learning çalışma alanı bağlı veri kümesi depolamak için bir Azure depolama hesabı gerektirir.
Depolama hesabı adı oluşturmak için kaynak grubunun adını ve çalışma alanı adı aşağıdaki şablonu kullanır.  Ayrıca depolama hesabı adı bir özellik olarak çalışma alanına oluştururken kullanır.

```
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "variables": {
        "namePrefix": "[resourceGroup().name]",
        "location": "[resourceGroup().location]",
        "mlVersion": "2016-04-01",
        "stgVersion": "2015-06-15",
        "storageAccountName": "[concat(variables('namePrefix'),'stg')]",
        "mlWorkspaceName": "[concat(variables('namePrefix'),'mlwk')]",
        "mlResourceId": "[resourceId('Microsoft.MachineLearning/workspaces', variables('mlWorkspaceName'))]",
        "stgResourceId": "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "storageAccountType": "Standard_LRS"
    },
    "resources": [
        {
            "apiVersion": "[variables('stgVersion')]",
            "name": "[variables('storageAccountName')]",
            "type": "Microsoft.Storage/storageAccounts",
            "location": "[variables('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "apiVersion": "[variables('mlVersion')]",
            "type": "Microsoft.MachineLearning/workspaces",
            "name": "[variables('mlWorkspaceName')]",
            "location": "[variables('location')]",
            "dependsOn": ["[variables('stgResourceId')]"],
            "properties": {
                "UserStorageAccountId": "[variables('stgResourceId')]"
            }
        }
    ],
    "outputs": {
        "mlWorkspaceObject": {"type": "object", "value": "[reference(variables('mlResourceId'), variables('mlVersion'))]"},
        "mlWorkspaceToken": {"type": "string", "value": "[listWorkspaceKeys(variables('mlResourceId'), variables('mlVersion')).primaryToken]"},
        "mlWorkspaceWorkspaceID": {"type": "string", "value": "[reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId]"},
        "mlWorkspaceWorkspaceLink": {"type": "string", "value": "[concat('https://studio.azureml.net/Home/ViewWorkspace/', reference(variables('mlResourceId'), variables('mlVersion')).WorkspaceId)]"}
    }
}

```
Bu şablon, c:\temp\ altında mlworkspace.json dosyası olarak kaydedin.

### <a name="deploy-the-resource-group-based-on-the-template"></a>Şablona göre kaynak grubunu dağıtma
* Open PowerShell
* Azure Resource Manager ve Azure hizmet yönetimi için modüllerini yükleyin  

```
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module AzureRM -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Bu adımları indirin ve geri kalan adımları tamamlamak için gerekli modüllerini yükleyin. Bu yalnızca bir kez burada PowerShell komutlarını çalıştırma ortamında yapılmalıdır.   

* Azure için kimlik doğrulaması  

```
# Authenticate (enter your credentials in the pop-up window)
Add-AzureRmAccount
```
Bu adımı her oturum için yinelenmesi gerekir. Kimliği doğrulanmış sonra abonelik bilgilerinizi görüntülenmesi gerekir.

![Azure hesabı][1]

Biz erişiminiz Azure, kaynak grubu oluşturabiliriz.

* Kaynak grubu oluşturma

```
$rg = New-AzureRmResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Kaynak grubu doğru biçimde sağlandığından emin olun. **ProvisioningState** "Başarılı."
Kaynak grubu adı, depolama hesabı adı oluşturmak için şablon tarafından kullanılır. Depolama hesabı adı 3 ile 24 karakter uzunluğunda olmalıdır ve rakamlar ve yalnızca küçük harfler kullanmanız gerekir.

![Kaynak Grubu][2]

* Kaynak grubu dağıtım kullanarak, yeni bir Machine Learning çalışma alanı dağıtın.

```
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzureRmResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Dağıtım tamamlandıktan sonra dağıttığınız çalışma özelliklerini erişim için basit bir iştir. Örneğin, birincil anahtar belirteci erişebilir.

```
# Access Azure ML Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Varolan çalışma alanının belirteçleri almak için başka bir yol Invoke-AzureRmResourceAction komutunu kullanmaktır. Örneğin, tüm çalışma alanları birincil ve ikincil belirteçleri listeleyebilirsiniz.

```  
# List the primary and secondary tokens of all workspaces
Get-AzureRmResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |% { Invoke-AzureRmResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}  
```
Çalışma alanı sağlandıktan sonra kullanarak birçok Azure Machine Learning Studio görevleri de otomatik hale getirebilirsiniz [Azure Machine Learning için PowerShell Modülü](http://aka.ms/amlps).

## <a name="next-steps"></a>Sonraki Adımlar
* Daha fazla bilgi edinmek [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md). 
* Bakın [Azure hızlı başlangıç şablonlarını deposuna](https://github.com/Azure/azure-quickstart-templates). 
* İlgili bu videoyu izleyin [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39). 

<!--Image references-->
[1]: ./media/deploy-with-resource-manager-template/azuresubscription.png
[2]: ./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png


<!--Link references-->
