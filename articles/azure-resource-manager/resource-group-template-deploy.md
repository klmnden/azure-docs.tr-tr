---
title: PowerShell ve şablon kaynakları dağıtma | Microsoft Docs
description: Bir kaynakları Azure'a dağıtmak için Azure Resource Manager ve Azure PowerShell kullanın. Kaynaklar, bir Resource Manager şablonunda tanımlanır.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.assetid: 55903f35-6c16-4c6d-bf52-dbf365605c3f
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 18dc82880830b6f8d14a7fc01930f75e9e61e5b0
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/15/2019
ms.locfileid: "56300559"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma

Azure PowerShell Resource Manager şablonları ile kaynaklarınızı Azure'a dağıtmak için kullanmayı öğrenin. Azure çözümlerinizi dağıtıp kavramları hakkında daha fazla bilgi için bkz. [Azure Resource Manager'a genel bakış](resource-group-overview.md).

Bir şablonu dağıtmak için genellikle iki adım gerekir:

1. Bir kaynak grubu oluşturun. Kaynak grubu, dağıtılan kaynaklar için kapsayıcı görevi görür. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. Bu, en fazla 90 karakter olabilir. Bu nokta ile bitemez.
2. Bir şablonu dağıtın. Şablon oluşturmak için kaynakları tanımlar.  Dağıtım kaynakları belirtilen kaynak grubu oluşturulmaktadır.

Bu makalede kullanılan bu iki aşamalı dağıtım yöntemidir.  Diğer seçenek bir kaynak grubu ve kaynakları aynı anda dağıtmaktır.  Daha fazla bilgi için [kaynak grubu oluşturun ve kaynakları dağıtma](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="prerequisites"></a>Önkoşullar

Kullanmadığınız sürece [Azure Cloud Shell'i](#deploy-templates-from-azure-cloud-shell) şablonlarını dağıtmak için Azure PowerShell'i yükleme ve Azure'a bağlanmak için ihtiyaç duyduğunuz:
- **Azure PowerShell cmdlet'lerini yerel bilgisayarınıza yükleyin.** Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlayın](/powershell/azure/get-started-azureps).
- **Kullanarak Azure'a bağlanma [Connect AZAccount](/powershell/module/az.accounts/connect-azaccount.md)**. Birden çok Azure aboneliğiniz varsa, aynı zamanda çalıştırmak ihtiyacınız olabilecek [kümesi AzContext](/powershell/module/Az.Accounts/Set-AzContext.md). Daha fazla bilgi için [birden çok Azure aboneliği kullanın](/powershell/azure/manage-subscriptions-azureps).
- * İndirin ve kaydedin bir [Hızlı Başlangıç şablonu](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json) . Bu makalede kullanılan yerel dosya adı **c:\MyTemplates\azuredeploy.json**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="deploy-templates-stored-locally"></a>Yerel olarak depolanan şablonlarını dağıtma

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName `
  -TemplateFile c:\MyTemplates\azuredeploy.json
```

Not *c:\MyTemplates\azuredeploy.json* Hızlı Başlangıç şablonu.  [Ön koşullara](#prerequisites) bakın.

Dağıtımın tamamlanması birkaç dakika sürebilir.

## <a name="deploy-templates-stored-externally"></a>Harici olarak depolanan şablonlarını dağıtma

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

## <a name="deploy-templates-from-azure-cloud-shell"></a>Azure Cloud shell'den şablon dağıtma

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

## <a name="deploy-to-multiple-resource-groups-or-subscriptions"></a>Birden çok kaynak grubuna veya aboneliğe dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Tek bir dağıtımda yalnızca beş kaynak gruplarına dağıtabilirsiniz. Daha fazla bilgi için [dağıtma Azure kaynaklarına birden çok kaynak gruplarında ve Aboneliklerde](resource-manager-cross-resource-group-deployment.md).

## <a name="redeploy-when-deployment-fails"></a>Dağıtım başarısız olduğunda yeniden dağıtma

Bir dağıtım başarısız olduğunda, dağıtım geçmişinden eski, başarılı bir dağıtım otomatik olarak yeniden dağıtabilirsiniz. Yeniden dağıtım belirtmek için kullanın `-RollbackToLastDeployment` veya `-RollBackDeploymentName` dağıtım komut parametresi.

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
