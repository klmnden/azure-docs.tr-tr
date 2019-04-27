---
title: Azure Resource Manager şablonları - Azure Logic Apps ile mantıksal uygulamaları dağıtma
description: Logic apps, Azure Resource Manager şablonlarını kullanarak dağıtma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: 21603ff213bc8babb4145209a76aee0b4150cc12
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60681770"
---
# <a name="deploy-logic-apps-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile mantıksal uygulamaları dağıtma

Mantıksal uygulamanızın dağıtılacağı bir Azure Resource Manager şablonu oluşturduktan sonra şablonunuzu bu yolla dağıtabilirsiniz:

* [Azure portal](#portal)
* [Azure PowerShell](#powershell)
* [Azure CLI](#cli)
* [Azure Resource Manager REST API](../azure-resource-manager/resource-group-template-deploy-rest.md)
* [Azure DevOps Azure işlem hatları](#azure-pipelines)

<a name="portal"></a>

## <a name="deploy-through-azure-portal"></a>Azure Portalı aracılığıyla dağıtma

Otomatik olarak bir mantıksal uygulama şablonunu Azure'a dağıtmak için aşağıdaki seçebilirsiniz **azure'a Dağıt** , Azure portalında oturum açtıktan ve mantıksal uygulamanızla ilgili daha fazla bilgi ister düğmesi. Ardından, mantıksal uygulama şablonunu veya parametre gerekli değişiklikleri hale getirebilirsiniz.

[![Azure’a dağıtma](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Örneğin, Azure portalında oturum açtıktan sonra bu bilgiyi istenir:

* Azure abonelik adı
* Kullanmak istediğiniz kaynak grubu
* Mantıksal uygulama konumu
* Mantıksal uygulamanızın adı
* Bir test URI'si
* Belirtilen hüküm ve koşulların kabulü

Daha fazla bilgi için [Azure Resource Manager şablonları ve Azure portalı ile kaynakları dağıtma](../azure-resource-manager/resource-group-template-deploy-portal.md).

## <a name="authorize-oauth-connections"></a>OAuth bağlantıları yetkilendirin

Dağıtımdan sonra mantıksal uygulama için uçtan uca geçerli parametreler ile çalışır. Ancak, yine de geçerli erişim belirteci oluşturmak için OAuth bağlantıları yetkilendirmeniz gerekir. Otomatik dağıtımlar için bu gibi her bir OAuth bağlantısı için onay bir komut dosyası kullanabilirsiniz [örnek betiği GitHub LogicAppConnectionAuth projedeki](https://github.com/logicappsio/LogicAppConnectionAuth). OAuth bağlantılar Azure Portalı aracılığıyla ya da Visual Studio'da Logic Apps Tasarımcısı'nda mantıksal uygulamanızı açarak ayrıca yetki verebilir.

<a name="powershell"></a>

## <a name="deploy-with-azure-powershell"></a>Azure PowerShell ile dağıtma

Belirli bir dağıtmak için *Azure kaynak grubu*, bu komutu kullanın:

```powershell
New-AzResourceGroupDeployment -ResourceGroupName <Azure-resource-group-name> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

Belirli bir Azure aboneliğinize dağıtmak için bu komutu kullanın:

```powershell
New-AzDeployment -Location <location> -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json 
```

* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)
* [`New-AzResourceGroupDeployment`](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment)
* [`New-AzDeployment`](/powershell/module/az.resources/new-azdeployment)

<a name="cli"></a>

## <a name="deploy-with-azure-cli"></a>Azure CLI ile dağıtma

Belirli bir dağıtmak için *Azure kaynak grubu*, bu komutu kullanın:

```azurecli
az group deployment create -g <Azure-resource-group-name> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Belirli bir Azure aboneliğinize dağıtmak için bu komutu kullanın:

```azurecli
az deployment create --location <location> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json
```

Daha fazla bilgi için şu konulara bakın: 

* [Kaynakları Resource Manager şablonları ve Azure CLI ile dağıtma](../azure-resource-manager/resource-group-template-deploy-cli.md) 
* [`az group deployment create`](https://docs.microsoft.com/cli/azure/group/deployment?view=azure-cli-latest#az-group-deployment-create)
* [`az deployment create`](https://docs.microsoft.com/cli/azure/deployment?view=azure-cli-latest#az-deployment-create)

<a name="azure-pipelines"></a>

## <a name="deploy-with-azure-devops"></a>Azure DevOps ile dağıtma

Mantıksal uygulama şablonları dağıtma ve ortamları yönetmek için yaygın olarak bir aracı gibi kullanın [Azure işlem hatları](https://docs.microsoft.com/azure/devops/pipelines/get-started/what-is-azure-pipelines) içinde [Azure DevOps](https://docs.microsoft.com/azure/devops/user-guide/what-is-azure-devops-services). Azure işlem hatları sağlayan bir [Azure kaynak grubu dağıtımı görev](https://github.com/Microsoft/azure-pipelines-tasks/tree/master/Tasks/AzureResourceGroupDeploymentV2) eklemek için herhangi bir derleme veya yayın işlem hattı.
Dağıtma ve yayın işlem hattı oluşturmak yetkilendirme için ayrıca bir Azure Active Directory (AD) ihtiyacınız [hizmet sorumlusu](../active-directory/develop/app-objects-and-service-principals.md). Daha fazla bilgi edinin [Azure işlem hatları ile hizmet sorumlularını kullanma](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

Azure işlem hatları kullanarak için genel üst düzey adımlar şunlardır:

1. Azure işlem hatlarında, boş bir işlem hattı oluşturun.

1. Mantıksal uygulama şablonunu ve el ile veya yapı işleminin bir parçası olarak oluşturduğunuz şablon parametreleri dosyaları gibi işlem hattı için ihtiyacınız olan kaynakları seçin.

1. Aracı işinizi bulun ve ekleyin **Azure kaynak grubu dağıtımı** görev.

   !["Azure kaynak grubu dağıtımı" görevi ekleyin](./media/logic-apps-create-deploy-template/add-azure-resource-group-deployment-task.png)

1. Yapılandırma ile bir [hizmet sorumlusu](https://docs.microsoft.com/azure/devops/pipelines/library/connect-to-azure). 

1. Mantıksal uygulama şablonunu ve şablon parametreleri dosya başvuruları ekleyin.

1. Diğer bir ortam, otomatikleştirilmiş test veya gerektiğinde onaylayanlar için yayın işlemindeki adımları oluşturmak için devam edin.

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal Uygulamaları izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)
