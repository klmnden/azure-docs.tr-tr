---
title: "Bir şablonu kullanarak Azure'da bir mantıksal uygulama oluşturma | Microsoft Docs"
description: "İş akışları tanımlamak için bir mantıksal uygulama dağıtmak için bir Azure Resource Manager şablonunu kullanın."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 161adeacd6da2b15225c8a4ddae171e19e539967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-logic-app-using-a-template"></a>Şablon kullanarak Logic Apps uygulaması oluşturma
Şablonları, bir mantıksal uygulama içinde farklı bağlayıcılarını kullanmak için hızlı bir yol sağlar. Mantıksal uygulamalar iş iş akışları tanımlamak için kullanılan bir mantıksal uygulama oluşturmak için Azure Resource Manager şablonları içerir. Hangi kaynağın dağıtılan ve mantıksal uygulamanızı dağıttığınızda parametrelerini tanımlamak nasıl tanımlayabilirsiniz. Bu şablon için kendi iş senaryoları kullanın veya gereksinimlerinizi karşılayacak şekilde özelleştirin.

Mantıksal uygulama özellikleri hakkında daha fazla bilgi için bkz: [mantığı uygulama iş akışı yönetimi API](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Tanımı bir örnekleri için bkz: [Yazar mantıksal uygulama tanımları](logic-apps-author-definitions.md). 

Şablonları oluşturma hakkında daha fazla bilgi için bkz: [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md).

Tam şablon için bkz: [mantıksal uygulama şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Ne dağıtma
Bu şablon kullanılarak bir mantıksal uygulama dağıtın.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeye seçin:  

[![Azure’a dağıtma](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametreler
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a>Dağıtılacak kaynaklar
### <a name="logic-app"></a>Mantıksal uygulama
Mantıksal uygulama oluşturur.

Şablonlar mantığı uygulama adı için bir parametre değeri kullanır. Kaynak grubu olarak aynı konuma mantıksal uygulama konumunu ayarlar. 

Bu belirli tanımını saatte bir kez çalıştırır ve belirttiğiniz konuma ping **testUri** parametresi. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Dağıtımı çalıştırma komutları
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



