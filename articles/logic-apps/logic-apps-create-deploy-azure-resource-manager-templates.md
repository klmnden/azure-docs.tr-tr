---
title: "Logic apps Azure Resource Manager şablonlarını oluşturma | Microsoft Docs"
description: "Oluşturma ve Azure Resource Manager şablonları ile mantığı uygulama iş akışları dağıtma"
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
ms.date: 10/15/2017
ms.author: LADocs; mandia
ms.openlocfilehash: e30ed8b1b8e2241bbebab1d7c5f337fabf37e1dd
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2017
---
# <a name="create-and-deploy-logic-apps-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile mantığı uygulamalar oluşturma ve dağıtma

Azure mantıksal uygulamaları yalnızca iş akışlarını otomatikleştirmek için logic apps oluşturmak için aynı zamanda kaynakları ve dağıtım için kullanılan parametreleri tanımlamak için kullanabileceğiniz Azure Resource Manager şablonları sağlar. Kendi iş senaryoları için bu şablonu kullanabilir veya gereksinimlerinizi karşılamak için şablonunu özelleştirebilirsiniz. Daha fazla bilgi edinmek [logic apps için Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) ve [Azure Resource Manager şablonu yapısı ve sözdizimi](../azure-resource-manager/resource-group-authoring-templates.md).

## <a name="define-the-logic-app"></a>Mantıksal uygulama tanımlayın

Bu örnek mantıksal uygulama tanımını saatte bir kez çalıştırır ve belirttiğiniz konuma ping `testUri` parametresi.
Şablon parametre değerlerini mantığı uygulama adını kullanır (```logicAppName```) ve test etmek için ping işlemi yapmak için konum (```testUri```). Daha fazla bilgi edinmek [şablonunuzda bu parametreleri tanımlama](#define-parameters). Şablon mantıksal uygulama konumunu Azure kaynak grubu olarak aynı konuma da ayarlar. 

``` json
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
                "Recurrence": {
                    "type": "Recurrence",
                    "recurrence": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            },
            "actions": {
                "Http": {
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
``` 

<a name="define-parameters"></a>

### <a name="define-parameters"></a>parametrelerini tanımlayın

[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Şablondaki parametreler için açıklamalar şunlardır:

| Parametre | Açıklama | JSON tanımı örneği | 
| --------- | ----------- | ----------------------- | 
| `logicAppName` | Adı tanımlar mantıksal uygulama bu şablonu oluşturur. | "logicAppName": {"tür": "dize", "meta veri": {"Açıklama": "myExampleLogicAppName"}} |
| `testUri` | Test etmek için ping işlemi yapmak için konum tanımlar. | "testUri": {"tür": "dize", "defaultValue": "http://azure.microsoft.com/status/feed/"} | 
||||

Daha fazla bilgi edinmek [Logic Apps iş akışı tanımı ve özellikleri için REST API](https://docs.microsoft.com/rest/api/logic/workflows) ve [mantıksal uygulama tanımları JSON ile derleme](logic-apps-author-definitions.md).

## <a name="deploy-logic-apps-automatically"></a>Mantıksal uygulamalar otomatik olarak Dağıt

Oluşturma ve otomatik olarak bir mantıksal uygulama Azure'a dağıtmak için tercih **Azure'a Dağıt** burada:

[![Azure’a dağıtma](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Bu eylem, burada mantığınızı uygulama ayrıntılarını sağlamak ve değişiklik şablon veya parametreleri Azure portalında oturum açtığında. Örneğin, Azure portal için bu ayrıntıların ister:

* Azure abonelik adı
* Kullanmak istediğiniz kaynak grubu
* Mantıksal uygulama konumu
* Mantıksal uygulamanız için bir ad
* Bir test URI
* Belirtilen hüküm ve koşulların kabulü

## <a name="deploy-logic-apps-with-commands"></a>Logic apps komutları ile dağıtma

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

```
New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup
``` 

### <a name="azure-cli"></a>Azure CLI

```
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](http://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal Uygulamaları izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)