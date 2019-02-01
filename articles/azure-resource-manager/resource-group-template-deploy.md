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
ms.openlocfilehash: ff67474566737ca75206cd1237c89f873cb173a8
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55489861"
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-powershell"></a>Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma

Bu makalede, Azure PowerShell Resource Manager şablonları ile kaynaklarınızı Azure'a dağıtmak için kullanmayı açıklar. Dağıtma ile ilgili kavramları hakkında bilgi sahibi değilseniz ve Azure çözümlerinizi bkz [Azure Resource Manager'a genel bakış](resource-group-overview.md).

Resource Manager şablonu makinenizde yerel bir dosya veya GitHub gibi bir depoda bulunan bir dış dosya olabilir dağıtın. Bu makalede dağıttığınız şablon olarak kullanılabilir [depolama hesabı GitHub şablonunda](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

Gerekirse, [Azure PowerShell kılavuzunda](/powershell/azure/overview) bulunan yönergeleri kullanarak Azure PowerShell modülünü yükleyin ve sonra Azure bağlantısı oluşturmak için `Connect-AzAccount` komutunu çalıştırın.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Yerel makinenizden bir şablonu dağıtma

Kaynakları Azure'a dağıtırken:

1. Azure hesabınızda oturum açma
2. Dağıtılan kaynaklar için kapsayıcı görevi gören bir kaynak grubu oluşturun. Kaynak grubu adı yalnızca alfasayısal karakterler, nokta, alt çizgi, kısa çizgi ve parantez içerebilir. Bu, en fazla 90 karakter olabilir. Bu nokta ile bitemez.
3. Kaynak grubunu oluşturmak için gereken kaynakları tanımlayan şablonu dağıtma

Şablon dağıtımı özelleştirmenize olanak sağlayan parametreler ekleyebilir. Örneğin, belirli bir ortama (örneğin, geliştirme, test ve üretim gibi) için uygun değerler sağlayabilirsiniz. Örnek şablonu, depolama hesabı SKU'su için parametre tanımlar.

Aşağıdaki örnek, bir kaynak grubu oluşturur ve yerel makinenizden bir şablon dağıtır:

```powershell
Connect-AzAccount

Select-AzSubscription -SubscriptionName <yourSubscriptionName>
 
New-AzResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Dağıtımın tamamlanması birkaç dakika sürebilir. Tamamlandığında, sonuçları içeren bir ileti görürsünüz:

```powershell
ProvisioningState       : Succeeded
```

## <a name="deploy-a-template-from-an-external-source"></a>Bir dış kaynaktan bir şablonu dağıtma

Resource Manager şablonları, yerel makinenizde depolamak yerine dış bir konumda depolanması tercih edebilirsiniz. Şablonları bir kaynak denetim deposu (örneğin GitHub) depolayabilirsiniz. Veya, bunları paylaşılan erişim için bir Azure depolama hesabında kuruluşunuzda depolayabilirsiniz.

Dış bir şablonu dağıtmak için **TemplateUri** parametresi. URI örnekte, github'dan örnek şablonu dağıtmak için kullanın.

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json `
  -storageAccountType Standard_GRS
```

Yukarıdaki örnekte genel olarak erişilebilen bir URI şablonu için çoğu senaryo için şablonunuzu hassas verileri eklememelisiniz çünkü düşünülerek gerektirir. Hassas verileri (örneğin, bir yönetici parolası) belirtmeniz gerekiyorsa, bu değeri güvenli bir parametre geçirin. Ancak, şablonunuzu genel olarak erişilebilir olmasını istemiyorsanız, bir özel depolama kapsayıcısında depolayarak koruyabilirsiniz. Paylaşılan erişim imzası (SAS) belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-powershell-sas-token.md).

[!INCLUDE [resource-manager-cloud-shell-deploy.md](../../includes/resource-manager-cloud-shell-deploy.md)]

Cloud Shell'de aşağıdaki komutları kullanın:

```powershell
New-AzResourceGroup -Name ExampleResourceGroup -Location "South Central US"
New-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateUri <copied URL> `
  -storageAccountType Standard_GRS
```

## <a name="deploy-to-more-than-one-resource-group-or-subscription"></a>Birden fazla kaynak grubuna veya aboneliğe dağıtma

Genellikle, tüm kaynakları tek bir kaynak grubu için şablonunuzdaki dağıtın. Ancak, bir kaynak kümesini birlikte dağıtmak ancak farklı kaynak gruplarında ya da abonelik yerleştirmek istediğiniz senaryolar da vardır. Tek bir dağıtımda yalnızca beş kaynak gruplarına dağıtabilirsiniz. Daha fazla bilgi için [birden fazla abonelik veya kaynak grubu dağıtma Azure kaynaklarına](resource-manager-cross-resource-group-deployment.md).

<a id="parameter-file" />

## <a name="parameters"></a>Parametreler

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
  -TemplateFile c:\MyTemplates\storage.json `
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

## <a name="test-a-template-deployment"></a>Şablon dağıtımı test etme

Şablonu ve parametre değerleriniz tüm kaynakları gerçekten dağıtmadan test etmek için [Test-AzureRmResourceGroupDeployment](/powershell/module/az.resources/test-azresourcegroupdeployment). 

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName ExampleResourceGroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType Standard_GRS
```

Hata algılanırsa, komutu bir yanıt tamamlanır. Hata algılanırsa, komutu bir hata iletisi döndürür. Örneğin, depolama hesabının SKU, yanlış bir değere geçirme şu hatayı döndürür:

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName testgroup `
  -TemplateFile c:\MyTemplates\storage.json -storageAccountType badSku

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
* Bu makaledeki örneklerde, varsayılan aboneliğinizde bir kaynak grubunda kaynak dağıtın. Farklı bir aboneliği kullanmak için bkz: [birden çok Azure aboneliklerini yönetme](/powershell/azure/manage-subscriptions-azureps).
* Kaynak grubunda var, ancak şablonunda tanımlanmayan kaynakları nasıl ele alınacağını belirtmek için bkz: [Azure Resource Manager dağıtım modları](deployment-modes.md).
* Şablonunuzda parametreleri tanımlayan anlamak için bkz. [yapısını ve Azure Resource Manager şablonları söz dizimini anlamak](resource-group-authoring-templates.md).
* Sık karşılaşılan dağıtım hataları çözümleme hakkında daha fazla ipucu için bkz. [Azure Resource Manager ile yaygın Azure dağıtım hatalarını giderme](resource-manager-common-deployment-errors.md).
* Bir SAS belirteci gerektiren şablonu dağıtma hakkında daha fazla bilgi için bkz: [SAS belirteci ile özel şablonu Dağıt](resource-manager-powershell-sas-token.md).
* Güvenli bir şekilde, bir hizmetin ölçeğini birden fazla bölgeye toplamak için bkz: [Azure Deployment Manager](deployment-manager-overview.md).
