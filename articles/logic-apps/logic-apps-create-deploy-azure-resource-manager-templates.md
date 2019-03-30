---
title: Azure Resource Manager şablonları - Azure Logic Apps ile mantıksal uygulamalar oluşturma | Microsoft Docs
description: Oluşturma ve mantıksal uygulama iş akışlarını Azure Logic Apps, Azure Resource Manager şablonları ile dağıtma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.date: 10/15/2017
ms.openlocfilehash: 8ad70c5d22ca73258fa9e6501d03d5409a4e45d8
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652493"
---
# <a name="create-and-deploy-logic-apps-with-azure-resource-manager-templates"></a>Azure Resource Manager şablonları ile mantıksal uygulamaları oluşturun ve dağıtın

Azure Logic Apps, yalnızca iş akışlarını otomatik hale getirmek için mantıksal uygulamalar oluşturmak için aynı zamanda dağıtım için kullanılan parametreleri ve kaynakları tanımlamak için kullanabileceğiniz Azure Resource Manager şablonları sağlar.
Gereksinimlerinizi karşılamak için şablonu özelleştirmek ya da kendi iş senaryoları için bu şablonu kullanın. Daha fazla bilgi edinin [logic apps için Resource Manager şablonu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) ve [Azure Resource Manager şablon yapısını ve söz dizimi](../azure-resource-manager/resource-group-authoring-templates.md). JSON söz dizimi ve özellikler için bkz: [Microsoft.Logic kaynak türleri](/azure/templates/microsoft.logic/allversions).

## <a name="define-the-logic-app"></a>Mantıksal uygulama tanımlayın
Bu örnek mantıksal uygulama tanımını saatte bir çalışır ve belirttiğiniz konuma göre ping atan `testUri` parametresi.
Şablon parametre değerleri için mantıksal uygulama adını kullanır (```logicAppName```) ve test etmek için ping konumu (```testUri```). Daha fazla bilgi edinin [Bu parametreler, şablonunuzda tanımlama](#define-parameters).
Şablon, Azure kaynak grubu olarak aynı konuma mantıksal uygulamanın konumu da ayarlanır.

```json
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
         "$schema": "https://schema.management.azure.com/schemas/2016-06-01/Microsoft.Logic.json",
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

### <a name="define-parameters"></a>Parametreleri tanımlayın

[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Şablon parametreleri için açıklamalar şunlardır:

| Parametre | Açıklama | JSON tanımı örneği |
| --------- | ----------- | ----------------------- |
| `logicAppName` | Adını tanımlayan Bu şablon mantıksal uygulama oluşturur. | "logicAppName": {"type": "dize", "metadata": {"description": "myExampleLogicAppName"}} |
| `testUri` | Test etmek için ping konumu tanımlar. | "testUri": {"type": "dize", "defaultValue": "https://azure.microsoft.com/status/feed/"} |
||||

Daha fazla bilgi edinin [Logic Apps iş akışının tanımını ve özellikleri için REST API](https://docs.microsoft.com/rest/api/logic/workflows) ve [JSON ile mantıksal uygulama tanımları üzerinde oluşturma](logic-apps-author-definitions.md).

## <a name="deploy-logic-apps-automatically"></a>Mantıksal uygulamalar otomatik olarak Dağıt

Oluşturma ve otomatik olarak bir mantıksal uygulamayı Azure'a dağıtmak için seçin **azure'a Dağıt** burada:

[![Azure’a dağıtma](./media/logic-apps-create-deploy-azure-resource-manager-templates/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

Bu eylem, burada, mantıksal uygulama'nın ayrıntılarını sağlayın ve değişiklik şablon veya parametreleri için Azure portalında oturum açar.
Örneğin, Azure portalı için bu Ayrıntılar ister:

* Azure abonelik adı
* Kullanmak istediğiniz kaynak grubu
* Mantıksal uygulama konumu
* Mantıksal uygulamanız için bir ad
* Bir test URI'si
* Belirtilen hüküm ve koşulların kabulü

## <a name="deploy-logic-apps-with-commands"></a>Logic apps komutları ile dağıtma

[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

```powershell
New-AzResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup
```

### <a name="azure-cli"></a>Azure CLI'si

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal Uygulamaları izleme](../logic-apps/logic-apps-monitor-your-logic-apps.md)
