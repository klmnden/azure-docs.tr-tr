---
title: Bir çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanma
titleSuffix: Azure Machine Learning service
description: Yeni bir Azure Machine Learning hizmeti çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanmayı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/16/2019
ms.custom: seoapril2019
ms.openlocfilehash: 4e0af3b395ec640fd037a1e76365408c10613340
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477007"
---
# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için bir çalışma alanı oluşturmak için bir Azure Resource Manager şablonu kullanma

Bu makalede, Azure Resource Manager şablonlarını kullanarak bir Azure Machine Learning hizmeti çalışma alanı oluşturmak için birkaç yol öğreneceksiniz. Resource Manager şablonu tek ve eşgüdümlü bir işlemle kaynaklarını oluşturmayı kolaylaştırır. Bir dağıtımı için gerekli kaynakları tanımlayan bir JSON belgesi şablonudur. Ayrıca, dağıtım parametreleri de belirtebilirsiniz. Parametreler, şablon kullanırken Giriş değerlerini sağlamak için kullanılır.

Daha fazla bilgi için [Azure Resource Manager şablonu ile uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md).

## <a name="prerequisites"></a>Önkoşullar

* Bir **Azure aboneliği**. Biri yoksa deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree).

* Bir şablondan bir CLI kullanmak için ya da ihtiyacınız [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="resource-manager-template"></a>Resource Manager şablonu

Aşağıdaki Resource Manager şablonu, bir Azure Machine Learning hizmeti çalışma alanında ve ilişkili Azure kaynaklarını oluşturmak için kullanılabilir:

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

Bu şablon, aşağıdaki Azure Hizmetleri oluşturur:

* Azure kaynak grubu
* Azure Depolama Hesabı
* Azure Key Vault
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning çalışma alanı

Kaynak grubu Hizmetleri tutan kapsayıcıdır. Çeşitli hizmetler, Azure Machine Learning çalışma alanı tarafından gereklidir.

Örnek şablon iki parametreye sahiptir:

* **Konumu** Hizmetleri ve kaynak grubunun oluşturulacağı.

    Şablon, seçtiğiniz konum için en fazla kaynak kullanır. Özel durum tüm diğer hizmetler konumlar kullanılamıyor Application Insights hizmetidir. Hizmet, nerede kullanılabilir değil bir konum seçin, Orta Güney ABD konumunda oluşturulur.

* **Çalışma alanı adı**, Azure Machine Learning çalışma alanının kolay adı.

    Diğer hizmetlerinin adları rastgele oluşturulur.

Şablonlar hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Resource Manager şablonları yazma](../../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablonları ile uygulama dağıtma](../../azure-resource-manager/resource-group-template-deploy.md)
* [Microsoft.MachineLearningServices kaynak türleri](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

## <a name="use-the-azure-portal"></a>Azure portalı kullanma

1. Bağlantısındaki [özel şablondan kaynakları dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template). Adresindeki geldiğinde __şablonu Düzen__ ekranında, bu belge şablonundan yapıştırın.
1. Seçin __Kaydet__ şablonu kullanmak için. Aşağıdaki bilgileri sağlayın ve listelenen hüküm ve koşulları kabul ediyorum:

   * Abonelik: Bu kaynaklar için kullanılacak Azure aboneliğini seçin.
   * Kaynak grubu: Hizmetleri içerecek bir kaynak grubu oluşturun veya seçin.
   * Çalışma alanı adı: Oluşturulacak Azure Machine Learning çalışma alanı için kullanılacak ad. Çalışma alanı adı 3-33 karakter arasında olmalıdır. Yalnızca alfasayısal karakterler içerebilir ve '-'.
   * Konum: Kaynakları oluşturulacağı konumu seçin.

     ![Azure portalında şablon parametreleri](media/how-to-create-workspace-template/template-parameters.png)

Daha fazla bilgi için [özel şablondan kaynakları dağıtma](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template).

## <a name="use-azure-powershell"></a>Azure PowerShell kullanma

Bu örnek adlı bir dosyaya kaydedilir şablonu varsayar `azuredeploy.json` geçerli dizin:

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace"
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../../azure-resource-manager/resource-group-template-deploy.md) ve [SAS belirteci ve Azure PowerShell ile özel Resource Manager şablonu dağıtma](../../azure-resource-manager/resource-manager-powershell-sas-token.md).

## <a name="use-azure-cli"></a>Azure CLI kullanma

Bu örnek adlı bir dosyaya kaydedilir şablonu varsayar `azuredeploy.json` geçerli dizin:

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus
```

Daha fazla bilgi için [kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-cli.md) ve [SAS belirteci ve Azure CLI ile özel Resource Manager şablonu dağıtma](../../azure-resource-manager/resource-manager-cli-sas-token.md).

## <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure anahtar kasası erişim ilkesi ve Azure Resource Manager şablonları

Çalışma alanı ve ilişkili kaynakları (Azure anahtar kasası dahil), birden çok kez oluşturmak için bir Azure Resource Manager şablonu kullandığınızda. Örneğin, şablon bir sürekli tümleştirme ve dağıtım işlem hattı bir parçası olarak aynı parametrelere sahip birden çok kez kullanma.

Şablonlar aracılığıyla çoğu kaynak oluşturma işlemleri bir kere etkili olur, ancak anahtar kasası erişim ilkeleri şablon kullanılan her zaman temizler. Key Vault kullandığı tüm mevcut bir çalışma alanı için erişim ilkeleri sonları erişimi temizleniyor. Örneğin, Azure not defterleri VM Stop/Create işlevlerini başarısız olabilir.  

Bu sorunu önlemek için aşağıdaki yaklaşımlardan birini önerilir:

*  Şablon, birden çok kez aynı parametreleri dağıtılmaz. Veya bunları yeniden oluşturmak için bu şablonu kullanmadan önce var olan kaynakları silin.
  
* Anahtar kasası erişim ilkelerini inceleyin ve sonra da şablonun accessPolicies özelliğini ayarlamak için bu ilkeleri kullanın.
* Key Vault kaynağı zaten mevcut olup olmadığını denetleyin. Varsa, şablonu aracılığıyla yeniden oluşturmayın. Örneğin, zaten varsa, anahtar kasası kaynak oluşturma devre dışı bırakmanıza olanak tanıyan bir parametre ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Kaynakları Resource Manager şablonları ve Resource Manager REST API'si ile dağıtma](../../azure-resource-manager/resource-group-template-deploy-rest.md).
* [Oluşturma ve Visual Studio aracılığıyla Azure kaynak grupları dağıtma](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).
