---
title: Dağıtım şablonları oluşturmak için Azure Logic Apps | Microsoft Docs
description: Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: ecfan
ms.author: estfan
ms.reviewer: klam, LADocs
ms.topic: article
ms.assetid: 85928ec6-d7cb-488e-926e-2e5db89508ee
ms.date: 10/18/2016
ms.openlocfilehash: 624539557b0bf57e9d919a3a46337f1cf93a4f07
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58894243"
---
# <a name="create-azure-resource-manager-templates-for-deploying-logic-apps"></a>Mantıksal uygulamalar dağıtmak için Azure Resource Manager şablonları oluşturma

Mantıksal uygulama oluşturduğunuzda, mantıksal uygulamanızın tanımını içine genişletebilirsiniz bir [Azure Resource Manager şablonu](../azure-resource-manager/resource-group-overview.md). Ardından, kaynaklarını tanımlayarak dağıtımları otomatik hale getirmek için bu şablonu kullanabilirsiniz ve parametreleri istediğiniz dağıtım ve parametre değerlerini sağlamak için kullanılan bir [parametre dosyasını](../azure-resource-manager/resource-group-template-deploy.md#parameter-files).
Bu şekilde, mantıksal uygulamalar daha kolayca dağıtım yapabilir ve istediğiniz tüm ortam ya da Azure kaynak grubu. 

Azure Logic Apps sağlayan bir [önceden oluşturulmuş mantıksal uygulamaları Azure Resource Manager şablonunu](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json) , mantıksal uygulama oluşturma için yalnızca aynı zamanda dağıtım için kullanılacak parametreleri ve kaynakları tanımlamak için yeniden. Gereksinimlerinizi karşılamak için şablonu özelleştirmek ya da kendi iş senaryoları için bu şablonu kullanın. Daha fazla bilgi edinin [Resource Manager şablon yapısını ve söz dizimi](../azure-resource-manager/resource-group-authoring-templates.md). JSON söz dizimi ve özellikler için bkz: [Microsoft.Logic kaynak türleri](/azure/templates/microsoft.logic/allversions).

Azure Resource Manager şablonları hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)
* [Azure Resource Manager şablon bulut tutarlılık için geliştirme](../azure-resource-manager/templates-cloud-consistency.md)

## <a name="logic-app-structure"></a>Mantıksal uygulama yapısı

"Kod görünümü" için veya bir aracı gibi kullanarak mantıksal uygulama tanımınızı "Tasarımcı görünümü" geçerek görüntüleyebilirsiniz. Bu temel bölümlerini sahip [Azure kaynak Gezgini](http://resources.azure.com). Mantıksal uygulama tanımları Javascript nesne gösterimi (JSON), bu nedenle JSON söz dizimi ve özellikleri hakkında daha fazla bilgi için bkz [Microsoft.Logic kaynak türleri](/azure/templates/microsoft.logic/allversions).

* **Mantıksal uygulama kaynağı**: Mantıksal uygulamanızın konuma veya bölgeye, gibi bilgileri fiyatlandırma planı ve iş akışı tanımını açıklar.

* **İş akışı tanımı**: Mantıksal uygulamanızın tetikleyici ve Eylemler ve Azure Logic Apps hizmetinin iş akışını nasıl çalıştığını açıklar. Kod Görünümü'nde, iş akışı tanımında bulabilirsiniz `definition` bölümü.

* **Bağlantıları**: Yönetilen bağlayıcılar mantıksal uygulamanızda kullanırsanız `$connections` bölümü güvenli bir şekilde mantıksal uygulamanız ve diğer sistemler veya bağlantı dizelerini ve erişim belirteçleri gibi hizmetler arasında bu bağlantıları hakkındaki meta verileri depolayan diğer kaynaklara başvuruyor. Mantıksal uygulama tanımınızı içinde bu bağlantılar başvuruları, mantıksal uygulama tanımının içinde görünen `parameters` bölümü.

Azure kaynak grubu dağıtımı sorunlarını kullanabileceğiniz bir mantıksal uygulama şablonunu oluşturmak için kaynakları tanımlayan ve gerektikçe parametreleştirin. Geliştirme, test ve üretim ortamı dağıtıyorsanız, örneğin, büyük olasılıkla bir SQL veritabanı'na farklı bağlantı dizeleri her ortamda kullanmak istediğiniz.
Ya da farklı bir abonelik veya kaynak grupları dağıtmak isteyebilirsiniz.

## <a name="create-logic-app-deployment-templates"></a>Mantıksal uygulama dağıtım şablonları oluşturma

En kolay yolu için geçerli mantıksal uygulama dağıtım şablonu oluşturmak Visual Studio uzantısı için Visual Studio ve Azure Logic Apps araçlarını kullanın. Visual Studio ile mantıksal uygulamanızı Azure Portalı'ndan yükleyerek herhangi bir Azure aboneliği ve konumu ile kullandığınız geçerli bir dağıtım şablonu alın. Ayrıca, mantıksal uygulamanızı otomatik olarak yüklenmesini şablonda katıştırılmış mantıksal uygulama tanımını parametreleştiren.
Oluşturma ve yönetme Visual Studio'da logic apps hakkında daha fazla bilgi için bkz. [Visual Studio ile mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-logic-apps-with-visual-studio.md) ve [Visual Studio ile mantıksal uygulamaları yönetme](../logic-apps/manage-logic-apps-with-visual-studio.md).

Visual Studio veya el ile bu konudaki yönergeleri izleyerek, şablonunuzu ve gerekli parametreleri oluşturma dışında da kullanabilirsiniz [mantıksal uygulama şablonları oluşturmak için PowerShell Modülü](https://github.com/jeffhollan/LogicAppTemplateCreator). Bu açık kaynaklı modül ilk mantıksal uygulamanızı ve mantıksal uygulamanın kullandığı tüm bağlantıları değerlendirir. Modül, ardından dağıtım için gerekli parametrelerle şablon kaynakları oluşturur. Örneğin, bir Azure Service Bus kuyruktan bir ileti alır ve bir Azure SQL veritabanına veri ekler bir mantıksal uygulama olduğunu varsayalım. Modül aracı, tüm düzenleme mantığı korur ve dağıtım sırasında bu değerleri ayarlayabilmesi SQL ve Service Bus bağlantı dizeleri parametreleştiren.

> [!IMPORTANT]
> Bağlantılar, aynı mantıksal uygulamayı Azure kaynak grubunda mevcut olması gerekir.
> PowerShell modülü modülle Azure kiracısı ve abonelik erişim belirteci, kullanım ile çalışmak için [Azure Resource Manager istemci aracı](https://github.com/projectkudu/ARMClient). Daha fazla bilgi için bkz. Bu [Azure Resource Manager istemci aracı hakkında daha fazla makale](https://blog.davidebbo.com/2015/01/azure-resource-manager-client.html) ARMClient daha ayrıntılı olarak ele alınmaktadır.

### <a name="install-powershell-module-for-logic-app-templates"></a>Mantıksal uygulama şablonları için PowerShell modülünü yükleme

Modülünü yüklemek en kolay yolu için [PowerShell Galerisi](https://www.powershellgallery.com/packages/LogicAppTemplate/0.1), bu komutu kullanın:

`Install-Module -Name LogicAppTemplate`

PowerShell modülünü el ile de yükleyebilirsiniz:

1. En son sürümünü indirin [mantıksal uygulama şablonu Oluşturucu](https://github.com/jeffhollan/LogicAppTemplateCreator/releases).

1. Genellikle, PowerShell modülü klasörünü klasöre ayıklayın `%UserProfile%\Documents\WindowsPowerShell\Modules`.

### <a name="generate-logic-app-template---powershell"></a>Mantıksal uygulama şablonunu - PowerShell oluşturma

PowerShell'i yükledikten sonra aşağıdaki komutu kullanarak bir şablon oluşturabilirsiniz:

`armclient token $SubscriptionId | Get-LogicAppTemplate -LogicApp MyApp -ResourceGroup MyRG -SubscriptionId $SubscriptionId -Verbose | Out-File C:\template.json`

`$SubscriptionId` Azure abonelik kimliğidir. Bu satır, ilk bir erişim belirteci ARMClient alır sonra PowerShell betiğine kanallar aracılığıyla ve ardından şablonu bir JSON dosyası oluşturur.

## <a name="parameters-in-logic-app-templates"></a>Mantıksal uygulama şablonları parametreleri

Mantıksal uygulama şablonunu oluşturduktan sonra ekleme ve tüm gerekli parametreleri düzenleyin. Şablonunuzu birden fazla çeşit `parameters` bölümünde, örneğin: 

* Mantıksal uygulamanızın iş akışı tanımı kendi bölümüne sahiptir [ `parameters` bölümü](../logic-apps/logic-apps-workflow-definition-language.md#parameters) nerede tanımlayabilirsiniz mantıksal uygulamanızın dağıtım sırasında girişleri kabul etmek için kullandığı tüm parametreleri.

* Resource Manager şablonunuzu kendi bölümüne sahiptir [ `parameters` bölümü](../azure-resource-manager/resource-group-authoring-templates.md#parameters)mantıksal uygulamanızın ayrı `parameters` bölümü. Örneğin:

  [!INCLUDE [logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

Örneğin, mantıksal uygulama tanımınızı başvuran bir Azure işlevine veya bir iç içe geçmiş mantıksal uygulama iş akışı temsil eden bir kaynak kimliği ve bu kaynak kimliği tek bir dağıtım mantıksal uygulamanızla birlikte dağıtmak istediğinizi varsayalım. Ekleyebileceğiniz şablonunuzda bir kaynak olarak bu kimliği ve o kimlik Parametreleştirme Özel API'ler veya Openapı Uç noktalara başvuruları için bu yaklaşımı kullanabilirsiniz (eski adıyla "Swagger") her bir Azure kaynak grubu ile dağıtılmış istiyor.

Dağıtım şablonunuzda parametrelerini kullanırken, Logic Apps Tasarımcısı'nda bu parametreleri doğru şekilde gösterebilir için bu yönergeleri izleyin:

* Yalnızca bu tetikleyiciler ve Eylemler içinde parametrelerini kullanın:

  * Azure işlev uygulaması
  * İç içe geçmiş veya alt mantıksal uygulama iş akışı
  * API Management çağrısı
  * API bağlantısı çalışma zamanı URL'si
  * API bağlantı yolu

* Parametreler tanımladığınızda, varsayılan değerleri kullanarak girdiğinizden emin olun `defaultValue` özellik değeri, örneğin:

  ```json
  "parameters": {
     "IntegrationAccount": {
        "type":"string",
        "minLength": 1,
        "defaultValue": "/subscriptions/<Azure-subscription-ID>/resourceGroups/<Azure-resource=group-name>/providers/Microsoft.Logic/integrationAccounts/<integration-account-name>"
     }
  },
  ```

* Güvenli veya şablonlarında hassas bilgileri gizlemek için parametrelerinizi güvenli hale getirin. Daha fazla bilgi edinin [güvenli parametrelerini kullanma](../logic-apps/logic-apps-securing-a-logic-app.md#secure-parameters-workflow).

Azure Service Bus "İletisi gönder" eylemini nasıl parametreleştirebilirsiniz gösteren bir örnek aşağıda verilmiştir:

```json
"Send_message": {
   "type": "ApiConnection",
   "inputs": {
      "host": {
         "connection": {
            "name": "@parameters('$connections')['servicebus']['connectionId']"
         },
         // If the `host.runtimeUrl` property appears in your template, 
         // you can remove this property, which is optional, for example:
         "runtimeUrl": {}
      },
      "method": "POST",
      "path": "[concat('/@{encodeURIComponent(''', parameters('<Azure-Service-Bus-queue-name>'), ''')}/messages')]",
      "body": {
         "ContentData": "@{base64(triggerBody())}"
      },
      "queries": {
         "systemProperties": "None"
      }
   },
   "runAfter": {}
},
```

### <a name="reference-dependent-resources"></a>Başvuru bağımlı kaynakları

Mantıksal uygulamanızın bağımlı kaynaklara başvurular gerektiriyorsa, kullanabileceğiniz [Azure Resource Manager şablonu işlevleri](../azure-resource-manager/resource-group-template-functions.md) mantıksal uygulamanızın dağıtım şablonu olarak. Örneğin, bir Azure işlevine veya bir tümleştirme hesabı iş ortakları, sözleşmeler ve mantıksal uygulamanız ile birlikte dağıtılan istediğiniz diğer yapıtlar için tanımları ile başvurmak için mantıksal uygulamanızı istediğinizi varsayalım.
Resource Manager şablonu işlevleri gibi kullanabileceğiniz `parameters`, `variables`, `resourceId`, `concat`ve benzeri.

Bu parametreleri tanımlayarak Azure işlevi için kaynak Kimliğini nasıl değiştirebilir gösteren bir örnek aşağıda verilmiştir:

``` json
"parameters": {
   "<Azure-function-name>": {
      "type": "string",
      "minLength": 1,
      "defaultValue": "<Azure-function-name>"
   }
},
```

Azure işlevini başvururken bu parametreleri kullanma aşağıda verilmiştir:

```json
"MyFunction": {
   "type": "Function",
   "inputs": {
      "body": {},
      "function": {
         "id":"[resourceid('Microsoft.Web/sites/functions','<Azure-Functions-app-name>', parameters('<Azure-function-name>'))]"
      }
   },
   "runAfter": {}
},
```

## <a name="add-logic-app-to-resource-group-project"></a>Mantıksal uygulama için kaynak grubu projesi ekleme

Mevcut bir Azure kaynak grubu projesi varsa, JSON ana hattı penceresinin kullanarak mantıksal uygulamanız bu projeye ekleyebilirsiniz. Daha önce oluşturduğunuz uygulama yanı sıra başka bir mantıksal uygulama da ekleyebilirsiniz.

1. Çözüm Gezgini'nde açın `<template>.json` dosya.

2. Gelen **görünümü** menüsünde **diğer Windows** > **JSON ana hattı**.

3. Şablon dosyasına bir kaynak eklemek için **kaynak Ekle** JSON ana hattı penceresinin üst kısmındaki. JSON ana hattı penceresinin içinde sağ tıklatın **kaynakları**seçip **yeni kaynak Ekle**.

   ![JSON ana hattı penceresinin](./media/logic-apps-create-deploy-template/jsonoutline.png)

4. İçinde **kaynak Ekle** bulun ve seçin iletişim kutusu **mantıksal uygulama**. Mantıksal uygulamanızı adlandırın ve seçin **Ekle**.

   ![Kaynak ekle](./media/logic-apps-create-deploy-template/addresource.png)

## <a name="get-support"></a>Destek alın

Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Mantıksal uygulama şablonları dağıtma](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md)
