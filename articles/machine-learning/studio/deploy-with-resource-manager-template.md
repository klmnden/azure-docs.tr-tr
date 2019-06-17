---
title: Studio çalışma alanına Azure Resource Manager ile dağıtma
titleSuffix: Azure Machine Learning Studio
description: Bir çalışma alanı için Azure Machine Learning Azure Resource Manager şablonu kullanarak Studio dağıtma
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 02/05/2018
ms.openlocfilehash: 91413aa461261824782717ae4edacc2757ad5405
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66121310"
---
# <a name="deploy-azure-machine-learning-studio-workspace-using-azure-resource-manager"></a>Azure Machine Learning Studio çalışma alanına Azure Resource Manager kullanarak dağıtma

Kullanarak bir Azure Resource Manager dağıtım şablonu, ölçeklenebilir bir şekilde vererek, zaman tasarrufu sağlar, birbirine bağlı bileşenleri ile bir doğrulama dağıtmak ve yeniden deneme mekanizması. Azure Machine Learning Studio çalışma alanları ayarlamak için örneğin, önce bir Azure depolama hesabı yapılandırın ve ardından çalışma alanınızı dağıtmanız gerekir. Çalışma alanları yüzlerce için el ile bunu hayal edin. Daha kolay bir alternatif bir Studio çalışma alanına ve tüm bağımlılıklarını dağıtmak için bir Azure Resource Manager şablonu kullanmaktır. Bu makalede bu işlemi adım adım alır. Harika bir genel bakış, Azure Resource Manager için bkz: [Azure Resource Manager'a genel bakış](../../azure-resource-manager/resource-group-overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="step-by-step-create-a-machine-learning-workspace"></a>Adım adım: bir Machine Learning çalışma alanı oluşturma
Biz bir Azure kaynak grubu oluşturun ve ardından yeni bir Azure depolama hesabı ve yeni Azure Machine Learning Studio Resource Manager şablonu kullanarak bir çalışma alanı dağıtın. Dağıtım tamamlandıktan sonra biz (birincil anahtar, çalışma alanı kimliği ve çalışma alanı URL) oluşturulan çalışma alanları hakkında önemli bilgileri yazdırır.

### <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma

Bir Machine Learning çalışma alanı, bağlantılı veri kümesine depolamak için bir Azure depolama hesabı gerektirir.
Depolama hesabı adı oluşturmak için kaynak grubu adını ve çalışma alanı adı şu şablonu kullanır.  Ayrıca depolama hesabı adı bir özellik olarak çalışma alanını oluştururken kullanır.

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

* PowerShell’i açın
* Azure Resource Manager ve Azure hizmet yönetimi için modülleri yükleme

```powershell
# Install the Azure Resource Manager modules from the PowerShell Gallery (press “A”)
Install-Module Az -Scope CurrentUser

# Install the Azure Service Management modules from the PowerShell Gallery (press “A”)
Install-Module Azure -Scope CurrentUser
```

   Bu adımlar, indirin ve kalan adımları tamamlamak için gerekli modüllerini yükleyin. Bu yalnızca burada PowerShell komutlarını çalıştırma ortama bir kez gerçekleştirilmesi gerekir.

* Azure'da kimlik doğrulaması

```powershell
# Authenticate (enter your credentials in the pop-up window)
Connect-AzAccount
```
Bu adımı her oturum için yinelenmesi gerekir. Kimlik doğrulandıktan sonra abonelik bilgilerinizi görüntülenmesi gerekir.

![Azure hesabı](./media/deploy-with-resource-manager-template/azuresubscription.png)

Azure'a erişimi sahibiz, kaynak grubunu oluşturabiliriz.

* Kaynak grubu oluşturma

```powershell
$rg = New-AzResourceGroup -Name "uniquenamerequired523" -Location "South Central US"
$rg
```

Kaynak grubu doğru şekilde sağlandığından emin olun. **ProvisioningState** "Başarılı olması."
Kaynak grubu adı, depolama hesabı adı oluşturmak için şablon tarafından kullanılır. Depolama hesabı adı 3 ila 24 karakter uzunluğunda olmalı ve sayı ve yalnızca küçük harflerden oluşmalıdır.

![Kaynak Grubu](./media/deploy-with-resource-manager-template/resourcegroupprovisioning.png)

* Kaynak grubu dağıtımı'nı kullanarak yeni bir Machine Learning çalışma alanı dağıtın.

```powershell
# Create a Resource Group, TemplateFile is the location of the JSON template.
$rgd = New-AzResourceGroupDeployment -Name "demo" -TemplateFile "C:\temp\mlworkspace.json" -ResourceGroupName $rg.ResourceGroupName
```

Dağıtım tamamlandıktan sonra dağıttığınız çalışma özelliklerine erişmek için basit bir iştir. Örneğin, birincil anahtar belirteci erişebilirsiniz.

```powershell
# Access Azure Machine Learning studio Workspace Token after its deployment.
$rgd.Outputs.mlWorkspaceToken.Value
```

Mevcut bir çalışma alanının belirteçlerini almak için başka bir yolu, Invoke-AzResourceAction komutunu kullanmaktır. Örneğin, birincil ve ikincil belirteçleri tüm çalışma alanları listeleyebilirsiniz.

```powershell
# List the primary and secondary tokens of all workspaces
Get-AzResource |? { $_.ResourceType -Like "*MachineLearning/workspaces*"} |ForEach-Object { Invoke-AzResourceAction -ResourceId $_.ResourceId -Action listworkspacekeys -Force}
```
Çalışma alanı sağlandıktan sonra kullanarak birçok Azure Machine Learning Studio görevleri de otomatik hale getirebilirsiniz [Azure Machine Learning Studio için PowerShell Modülü](https://aka.ms/amlps).

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md).
* Göz atın [Azure hızlı başlangıç şablonları depo](https://github.com/Azure/azure-quickstart-templates).
* Bu videoyu hakkında [Azure Resource Manager](https://channel9.msdn.com/Events/Ignite/2015/C9-39).
* Bkz: [Resource Manager şablon Başvurusu Yardımı](https://docs.microsoft.com/azure/templates/microsoft.machinelearning/allversions)

<!--Link references-->
