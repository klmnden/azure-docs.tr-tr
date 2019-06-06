---
title: PowerShell ve şablon kaynakları dağıtma | Microsoft Docs
description: Kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure PowerShell kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tomfitz
ms.openlocfilehash: 63d729f19b0ef20d0e7a716d6857b4627095856b
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476990"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma

Azure PowerShell Resource Manager şablonları ile kaynaklarınızı Azure'a dağıtmak için kullanmayı öğrenin. Azure çözümlerinizi dağıtıp kavramları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](resource-group-overview.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deployment-scope"></a>Dağıtım kapsamı

Dağıtımınız için bir Azure aboneliğine veya abonelik içinde bir kaynak grubu hedefleyebilirsiniz. Çoğu durumda, bir kaynak grubuna dağıtım hedefi. İlkeleri ve rol atamalarını abonelik üzerinden uygulamak için abonelik dağıtımları'ı kullanın. Abonelik dağıtımları da bir kaynak grubu oluşturun ve kaynakları dağıtmak için kullanın. Dağıtım kapsamını bağlı olarak, farklı komutlarını kullanın.

Dağıtmak için bir **kaynak grubu**, kullanın [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment):

```azurepowershell
New-AzResourceGroupDeployment -ResourceGroupName <resource-group-name> -TemplateFile <path-to-template>
```

Dağıtmak için bir **abonelik**, kullanın [yeni AzDeployment](/powershell/module/az.resources/new-azdeployment):

```azurepowershell
New-AzDeployment -Location <location> -TemplateFile <path-to-template>
```

Şu anda, yönetim grubu dağıtımları yalnızca REST API aracılığıyla desteklenir. Bkz: [kaynakları Resource Manager şablonları ve Resource Manager REST API'si ile dağıtma](resource-group-template-deploy-rest.md).

Bu makaledeki örneklerde, kaynak grubu dağıtımı kullanın. Abonelik dağıtımları hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](deploy-to-subscription.md).

## <a name="prerequisites"></a>Önkoşullar

Bir şablonu dağıtmak için ihtiyacınız vardır. Zaten yoksa, indirin ve kaydedin bir [örnek şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) Azure hızlı başlangıç şablonları deposundan. Bu makalede kullanılan yerel dosya adı **c:\MyTemplates\azuredeploy.json**.

Azure Cloud shell şablonları dağıtmak için kullandığınız sürece, Azure PowerShell'i yükleme ve Azure'a bağlanmak gereken:

- **Azure PowerShell cmdlet'lerini yerel bilgisayarınıza yükleyin.** Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](/powershell/azure/get-started-azureps).
- **Kullanarak Azure'a bağlanma [Connect AZAccount](/powershell/module/az.accounts/connect-azaccount)** . Birden çok Azure aboneliğiniz varsa, aynı zamanda çalıştırmak ihtiyacınız olabilecek [kümesi AzContext](/powershell/module/Az.Accounts/Set-AzContext). Daha fazla bilgi için [birden çok Azure aboneliği kullanın](/powershell/azure/manage-subscriptions-azureps).

## <a name="deploy-local-template"></a>Yerel şablonu dağıtma

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. Bu, en fazla 90 karakter olabilir. Bu nokta ile bitemez.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürebilir.

## <a name="deploy-remote-template"></a>Uzak şablonu dağıtma

Resource Manager şablonları, yerel makinenizde depolamak yerine dış bir konumda depolanması tercih edebilirsiniz. Şablonları bir kaynak denetim deposu (örneğin GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabında kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için **TemplateUri** parametresi. URI örnekte, github'dan örnek şablonu dağıtmak için kullanın.

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Yukarıdaki örnekte genel olarak erişilebilen bir URI şablonu için çoğu senaryo için şablonunuzu hassas verileri eklememelisiniz çünkü düşünülerek gerektirir. Hassas verileri (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-powershell-sas-token.md). Bir öğreticiyi incelemek için bkz: [Öğreticisi: Resource Manager şablon dağıtımı Azure anahtar kasası tümleştirme](./resource-manager-tutorial-use-key-vault.md).

## <a name="deploy-from-azure-cloud-shell"></a>Azure Cloud shell'den dağıtma

Kullanabileceğiniz [Azure Cloud Shell](https://shell.azure.com) şablonunuzu dağıtmak için. Dış bir şablonu dağıtmak için şablon URI'si belirtin. Yerel bir şablonu dağıtmak için Cloud Shell için depolama hesabına şablonunuzu yüklemelisiniz. Dosyaları shell'e yüklemeniz için seçin **dosyaları karşıya yükleme/indirme** Kabuk penceresinden menüsü simgesi.

Cloud Shell'i açmak için Gözat [ https://shell.azure.com ](https://shell.azure.com), ya da seçin **deneyin-BT** aşağıdaki kod bölümünden:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
```

Kod shell'e yapıştırmak için kabuk içinde sağ tıklayın ve ardından **yapıştırın**.

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bu özellik olarak da bilinir *hatada geri alma*. Bir dağıtım başarısız olduğunda, dağıtım geçmişinden eski, başarılı bir dağıtım otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtım belirtmek için kullanın `-RollbackToLastDeployment` veya `-RollBackDeploymentName` dağıtım komut parametresi. Bu işlevsellik, altyapı dağıtımınız için iyi bilinen bir duruma var ve bu duruma geri dönmek istiyorsanız kullanışlıdır. Uyarılar ve kısıtlamaları vardır:

- Yeniden dağıtma işlemi ile aynı parametreleri daha önce tam olarak çalıştırıldığı olarak çalıştırılır. Parametreleri değiştiremezsiniz.
- Kullanarak önceki dağıtım çalıştırma [tam modda](./deployment-modes.md#complete-mode). Önceki dağıtıma dahil olmayan tüm kaynaklar silinir ve herhangi bir kaynak yapılandırmaları önceki durumlarına ayarlanır. Tam olarak anladığınızdan emin olun [dağıtım modları](./deployment-modes.md).
- Yeniden dağıtma işlemi, yalnızca kaynakları etkiler, tüm veri değişiklikleri etkilenmez.
- Bu özellik yalnızca kaynak grubu dağıtımlarında, abonelik düzeyinde dağıtımlar desteklenir. Abonelik düzeyi dağıtımı hakkında daha fazla bilgi için bkz. [oluşturma kaynak grubu ve kaynak abonelik düzeyinde](./deploy-to-subscription.md).

Bu seçeneği kullanmak için dağıtımlarınızı geçmişinde tanımlanan şekilde benzersiz adları olmalıdır. Benzersiz adlara sahip değilseniz, geçerli başarısız dağıtım geçmişini daha önce başarılı dağıtım üzerine yazılabilir. Bu gibi durumlarda, bu seçenek yalnızca kök düzey dağıtımlar kullanabilirsiniz. İç içe geçmiş şablon dağıtımları, yeniden dağıtım için kullanılamaz.

Son başarılı dağıtımı yeniden ekleyin `-RollbackToLastDeployment` parametre olarak bir bayrak.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollbackToLastDeployment
```

Belirli bir dağıtımı yeniden dağıtmak için kullanın `-RollBackDeploymentName` parametresi ve dağıtımın adını belirtin.

```azurepowershell-interactive
New-AzResourceGroupDeployment -Name ExampleDeployment02 `
  -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -RollBackDeploymentName ExampleDeployment01
```

Belirtilen dağıtım başarılı gerekir.

## <a name="pass-parameter-values"></a>Parametre değerlerini geçirmek

Parametre değerlerini geçirmek için satır içi parametre ya da bir parametre dosyası kullanabilirsiniz. Bu makalede Yukarıdaki örneklerde, satır içi parametreleri göster.

### <a name="inline-parameters"></a>Satır içi parametreleri

Satır içi parametreleri geçirmek için adlı parametre adlarını sağlayan `New-AzResourceGroupDeployment` komutu. Örneğin, bir dize ve dizi için bir şablon geçirmek için kullanın:

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString "inline string" `
  -exampleArray $arrayParam
```

Ayrıca, dosya içeriğini Al ve bu içeriği satır içi parametresi olarak sağlayın.

```powershell
$arrayParam = "value1", "value2"
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleString $(Get-Content -Path c:\MyTemplates\stringcontent.txt -Raw) `
  -exampleArray $arrayParam
```

Bir parametre değeri bir dosyadan alınırken, yapılandırma değerlerini sağlamak ihtiyacınız olduğunda yararlıdır. Örneğin, sağlayabilir [Linux sanal makinesi için cloud-init değerleri](../virtual-machines/linux/using-cloud-init.md).

Bir nesne dizisidir geçirmek gerekiyorsa, PowerShell'de karma tabloları oluşturma ve bunları bir dizisi olarak ekleyin. Bu dizi, dağıtım sırasında bir parametre olarak geçirirsiniz.

```powershell
$hash1 = @{ Name = "firstSubnet"; AddressPrefix = "10.0.0.0/24"}
$hash2 = @{ Name = "secondSubnet"; AddressPrefix = "10.0.1.0/24"}
$subnetArray = $hash1, $hash2
New-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\demotemplate.json `
  -exampleArray $subnetArray
```


### <a name="parameter-files"></a>Parametre dosyaları

Betiğinizde değerleri satır içi olarak parametre geçirme yerine parametre değerlerini içeren bir JSON dosyası kullanmak daha kolay. Parametre dosyasını yerel bir dosya veya bir dış dosya erişilebilir bir URI'ye sahip olabilir.

Parametre dosyasını şu biçimde olmalıdır:

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

Parametreler bölümü (storageAccountType), şablonunuzda tanımlanan parametre eşleşen bir parametre adı içerdiğine dikkat edin. Parametre dosyasını, parametre için bir değer içerir. Bu değer, şablona dağıtım sırasında otomatik olarak geçirilir. Birden fazla parametre dosyası oluşturun ve ardından uygun bir parametre dosyası senaryosu için geçirin.

Önceki örneği kopyalayabilir ve adlı bir dosya kaydedin `storage.parameters.json`.

Bir yerel parametre dosyası geçirmek için kullanmak **TemplateParameterFile** parametresi:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json `
  -TemplateParameterFile c:\MyTemplates\storage.parameters.json
```

Bir dış parametre dosyası geçirmek için kullanmak **TemplateParameterUri** parametresi:

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.parameters.json
```

### <a name="parameter-precedence"></a>Parametre önceliği

Satır içi parametreleri ve aynı dağıtım işlemi yerel parametre dosyasında kullanabilirsiniz. Örneğin, yerel dosyada bazı değerler belirtin ve diğer değerleri satır içi dağıtımı sırasında ekleyin. Hem yerel parametre dosyasında hem de satır içi parametre değerlerini sağlayın, satır içi değeri önceliklidir.

Bir dış parametre dosyası kullanmak, ancak diğer değerleri ya da satır içi geçiremezsiniz veya yerel bir dosyadan. Bir parametre dosyasında belirttiğinizde **TemplateParameterUri** parametre, tüm satır içi parametreleri yok sayılır. Dış dosya içinde tüm parametre değerlerini sağlayın. Şablon parametre dosyasında içeremez duyarlı bir değer içeriyorsa, bu değer için bir anahtar kasası ekleyin veya tüm parametre değerlerini satır içi dinamik olarak sağlayın.

### <a name="parameter-name-conflicts"></a>Parametre adı çakışıyor

Şablonunuzu PowerShell komutunu parametrelerinden biri olarak aynı ada sahip bir parametre içeriyorsa, PowerShell, şablon parametresi ile sonek sunar. **FromTemplate**. Örneğin, adında bir parametre **ResourceGroupName** ile şablon çakışıyor **ResourceGroupName** parametresinde [yeni AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) cmdlet'i. İçin bir değer sağlamanız istenir **ResourceGroupNameFromTemplate**. Genel olarak, dağıtım işlemleri için kullanılan parametreler olarak aynı ada sahip adlandırma parametreleri olmayan tarafından bu karışıklığı kaçınmanız gerekir.

## <a name="test-template-deployments"></a>Test şablon dağıtımları

Şablonu ve parametre değerleriniz tüm kaynakları gerçekten dağıtmadan test etmek için [Test-AzureRmResourceGroupDeployment](/powershell/module/az.resources/test-azresourcegroupdeployment). 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType Standard_GRS
```

Hata algılanırsa, komutu bir yanıt tamamlanır. Hata algılanırsa, komutu bir hata iletisi döndürür. Örneğin, depolama hesabının SKU, yanlış bir değere geçirme şu hatayı döndürür:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\azuredeploy.json -storageAccountType badSku

Code    : InvalidTemplate
Message : Deployment template validation failed: 'The provided value 'badSku' for the template parameter 'storageAccountType'
          at line '15' and column '24' is not valid. The parameter value is not part of the allowed value(s):
          'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.
Details :
```

Komut, şablonunuzun söz dizimi hatası varsa, şablon ayrıştırılamadı belirten bir hata döndürür. İleti satır numarasını ve ayrıştırma hatası konumunu gösterir.

```powershell
Test-AzResourceGroupDeployment : After parsing a value an unexpected character was encountered: 
  ". Path 'variables', line 31, position 3.
```

## <a name="next-steps"></a>Sonraki adımlar

- Güvenli bir şekilde, bir hizmetin ölçeğini birden fazla bölgeye toplamak için bkz: [Azure Deployment Manager](deployment-manager-overview.md).
- Kaynak grubunda var, ancak şablonunda tanımlanmayan kaynakları nasıl ele alınacağını belirtmek için bkz: [Azure Resource Manager dağıtım modları](deployment-modes.md).
- Şablonunuzda parametreleri tanımlayan anlamak için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
- Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-powershell-sas-token.md).
