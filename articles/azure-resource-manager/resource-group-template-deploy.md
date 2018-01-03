---
title: "PowerShell ve şablon kaynaklarla dağıtma | Microsoft Docs"
description: "Azure Resource Manager ve Azure PowerShell bir kaynakları Azure'a dağıtmak için kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2017
ms.author: tomfitz
ms.openlocfilehash: 1210b2da9126c24b59e8ef59b50742a17b2e740d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma

Bu makalede Azure PowerShell'i Resource Manager şablonları ile kaynakları Azure'a dağıtmak için nasıl kullanılacağı açıklanmaktadır. Dağıtma ile ilgili kavramları hakkında bilgi sahibi değilseniz ve Azure çözümlerinizi bkz [Azure Resource Manager'a genel bakış](resource-group-overview.md).

Resource Manager şablonu ya da makinenizde yerel bir dosya ya da GitHub gibi bir havuzda bulunan dış dosyası dağıtabilirsiniz. Bu makalede dağıttığınız şablonu kullanılabilir [örnek şablonu](#sample-template) bölümünde veya as [depolama hesabı şablonu github](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

Gerekirse, bulunan yönergeleri kullanarak Azure PowerShell modülünü yüklemek [Azure PowerShell Kılavuzu](/powershell/azure/overview)ve ardından çalıştırın `Login-AzureRmAccount` Azure ile bir bağlantı oluşturmak için.

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Yerel makinenizde bir şablondan dağıtma

Kaynakları Azure'a dağıtırken:

1. Azure hesabınızda oturum açın
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. En fazla 90 karakter olabilir. Bir nokta ile bitemez.
3. Kaynak grubu oluşturmak için kaynakları tanımlayan şablonu dağıtma

Bir şablon dağıtımı özelleştirmenize olanak sağlayan parametreler içerebilir. Örneğin, belirli bir ortamda (örneğin, geliştirme, test ve üretim) için uyarlanabilir değerler sağlayabilirsiniz. Örnek şablon SKU depolama hesabı için bir parametre tanımlar.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenize bir şablondan dağıtır:

```powershell
Login-AzureRmAccount

Select-AzureRmSubscription -SubscriptionName <yourSubscriptionName>
 
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuç içeren bir ileti görür:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Bir dış kaynaktan şablon dağıtma

Resource Manager şablonları yerel makinenizde depolamak yerine bir dış konuma depolamak tercih edebilirsiniz. Şablonları bir kaynak denetimi deponuza (örneğin, GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabı, kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için kullandığınız **TemplateUri** parametresi. URI örnekte github'dan örnek şablonu dağıtmak için kullanın.

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

Önceki örnekte şablonunuzu hassas bir veri içermemesi çünkü çoğu senaryo için çalışır ve şablona için genel olarak erişilebilir bir URI gerektirir. Hassas veriler (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre olarak geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısı depolayarak koruyabilirsiniz. Bir paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-powershell-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Bulut Kabuğu'nda aşağıdaki komutları kullanın:

```powershell
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzureRmResourceGroupDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile "C:\users\ContainerAdministrator\CloudDrive\templates\azuredeploy.json" -storageAccountType Standard_GRS
```

## <a name="parameter-files"></a>Parametre dosyaları

Satır içi değerler olarak komut parametreleri geçirme yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolay. Parametre dosyası şu biçimde olmalıdır:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Parametreler bölümünde şablonunuzda (storageAccountType) tanımlanan parametre eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyası parametresi için bir değer içerir. Bu değer, dağıtım sırasında şablona otomatik olarak geçirilir. Farklı dağıtım senaryoları için birden çok parametre dosyası oluşturun ve ardından uygun parametre dosyası geçirin. 

Önceki örnekte kopyalayın ve adlı bir dosya kaydedin `storage.parameters.json`.

Bir yerel parametre dosyası geçirmek için kullanmak **TemplateParameterFile** parametre:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Bir dış parametre dosyasına geçirmek için kullanmak **TemplateParameterUri** parametre:

```powershell
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

Satır içi parametreleri ve aynı dağıtım işlemi yerel parametre dosyasında kullanabilirsiniz. Örneğin, bazı değerler yerel parametresini belirtin ve dağıtım sırasında diğer değerleri satır içi ekleyin. Hem yerel parametre dosyası, hem de satır içi bir parametre için değer sağlarsanız, satır içi değer önceliklidir.

Bir dış parametre dosyası kullandığınızda, ancak, diğer değerler ya da satır içi geçiremezsiniz veya yerel bir dosya. Bir parametre dosyası belirttiğinizde **TemplateParameterUri** parametresi, tüm satır içi parametreleri yok sayılır. Dış dosyadaki tüm parametre değerlerini sağlayın. Şablonunuzu parametre dosyasında içeremez duyarlı bir değer içeriyorsa, bu değer bir anahtar Kasası'na ekleyin ya da dinamik olarak tüm parametre değerleri satır içi sağlayın.

Şablonunuzu PowerShell komutunda parametrelerden biri aynı ada sahip bir parametre içeriyorsa, PowerShell şablonunuzu parametresinden sonek ile sayısını gösterir. **FromTemplate**. Örneğin, adlı bir parametre **ResourceGroupName** ile şablonu çakışıyor **ResourceGroupName** parametresinde [New-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) cmdlet'i. İçin bir değer sağlamanız istenir **ResourceGroupNameFromTemplate**. Genel olarak, Karışıklığı önlemek için bu parametreleri dağıtım işlemleri için kullanılan parametreleri aynı ada sahip adlandırılmasıyla değil kaçınmalısınız.

## <a name="test-a-template-deployment"></a>Şablon dağıtımı test etme

Herhangi bir kaynağa dağıtmadan şablonu ve parametre değerlerini sınamak için kullanın [Test-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/test-azurermresourcegroupdeployment). 

```powershell
Test-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Hiçbir hata algılanırsa, komutu bir yanıt tamamlanır. Bir hata algılandığında, komutu bir hata iletisi döndürür. Örneğin, SKU, depolama hesabı için hatalı bir değer geçirmek çalışılırken aşağıdaki hata döndürür:

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Şablonunuzun söz dizimi hatası varsa, komut şablon ayrıştırılamadı belirten bir hata döndürür. İleti satır numarası ve ayrıştırma hatası konumunu gösterir.

```powershell
Test-AzureRmResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Tam modu kullanmak için `Mode` parametre:

```powershell
New-AzureRmResourceGroupDeployment -Mode Complete -Name ExampleDeployment `
  -ResourceGroupName ExampleResourceGroup -TemplateFile c:\MyTemplates\storage.json 
```

## <a name="sample-template"></a>Örnek şablonu

Aşağıdaki şablonu, bu makaledeki örnekler için kullanılır. Kopyalayın ve storage.json adlı bir dosya kaydedin. Bu şablonun nasıl oluşturulacağını anlamak için bkz: [, ilk Azure Resource Manager şablonu oluşturma](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Bu makaledeki örneklerde kaynakları varsayılan aboneliğinizde bir kaynak grubuna dağıtın. Farklı bir abonelik kullanmak için bkz: [birden çok Azure Aboneliklerini yönetmek](/powershell/azure/manage-subscriptions-azureps).
* Bir şablon dağıtan bir tam örnek betik için bkz: [Resource Manager şablonu dağıtım betiği](resource-manager-samples-powershell-deploy.md).
* Şablonunuzda parametrelerini tanımlamak nasıl anlamak için bkz: [yapısı ve Azure Resource Manager şablonları sözdizimini anlamanız](resource-group-authoring-templates.md).
* Genel dağıtım hatalarını giderme ipuçları için bkz: [ortak Azure dağıtım hataları Azure Resource Manager ile ilgili sorunları giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [dağıtma özel şablonu SAS belirteci ile](resource-manager-powershell-sas-token.md).
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](resource-manager-subscription-governance.md).

