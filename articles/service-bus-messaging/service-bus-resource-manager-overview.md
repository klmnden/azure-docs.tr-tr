---
title: "Resource Manager şablonları kullanarak Azure Service Bus kaynakları oluşturun | Microsoft Docs"
description: "Service Bus kaynaklarını oluşturmayı otomatikleştirmek için Azure Resource Manager şablonlarını kullanma"
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/10/2017
ms.author: sethm
ms.openlocfilehash: 0ceeb138a7432e51cabe2597c680cb01ea9eac4a
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak Service Bus kaynakları oluşturun

Bu makalede, Azure Resource Manager şablonları, PowerShell ve Service Bus kaynak sağlayıcısı kullanarak Service Bus kaynaklarını oluşturup açıklar.

Azure Resource Manager şablonları bir çözümü dağıtmak ve parametreleri ve farklı ortamlar için değer girmesini sağlayan değişkenleri belirtmek için kaynakları tanımlamanıza yardımcı. Şablon JSON'de yazılır ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler oluşur. Azure Resource Manager şablonları ve şablon biçimi tartışması yazma hakkında ayrıntılı bilgi için bkz: [yapısı ve Azure Resource Manager şablonları sözdizimini](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> Bu makaledeki örnekler Azure Resource Manager bir hizmet veri yolu ad alanı ve mesajlaşma varlığıyla (kuyruk) oluşturmak için nasıl kullanılacağını gösterir. Diğer şablon örnekler için ziyaret [Azure hızlı başlangıç Şablon Galerisi] [ Azure Quickstart Templates gallery] arayın ve **Service Bus**.
>
>

## <a name="service-bus-resource-manager-templates"></a>Hizmet veri yolu Resource Manager şablonları

Bu hizmet veri yolu Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir. Her biri, GitHub şablonlar için bağlantılar ile birlikte hakkında ayrıntılı bilgi için aşağıdaki bağlantıları tıklatın:

* [Hizmet veri yolu ad alanı oluşturma](service-bus-resource-manager-namespace.md)
* [Sıra ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
* [Hizmet veri yolu ad alanı konu ve abonelik oluşturma](service-bus-resource-manager-namespace-topic.md)
* [Kuyruk ve yetkilendirme kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
* [Konu, abonelik ve kuralı ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Aşağıdaki yordam, standart katmanı Service Bus ad alanı ve bu ad alanı içindeki bir kuyruk oluşturur bir Azure Resource Manager şablonu dağıtmak için PowerShell kullanmayı açıklar. Bu örnek dayanır [sıra ile Service Bus ad alanı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) şablonu. Yaklaşık iş akışı aşağıdaki gibidir:

1. PowerShell yükleyin.
2. Şablon ve (isteğe bağlı) bir parametre dosyası oluşturun.
3. PowerShell'de, Azure hesabınızda oturum açın.
4. Bir mevcut değilse yeni bir kaynak grubu oluşturun.
5. Dağıtımı test etme.
6. İsterseniz, dağıtım modu ayarlayın.
7. Şablon dağıtın.

Azure Resource Manager şablonları dağıtma hakkında tam bilgi için bkz: [kaynakları Azure Resource Manager şablonları ile dağıtma][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>PowerShell yükleme

Azure PowerShell yönergelerini takip ederek yükleyin [Azure PowerShell ile çalışmaya başlama](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Şablon oluşturma

Kopya veya kopya [201-servicebus--kuyruk oluşturma](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) github'dan şablon:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "serviceBusNamespaceName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Service Bus namespace"
      }
    },
    "serviceBusQueueName": {
      "type": "string",
      "metadata": {
        "description": "Name of the Queue"
      }
    }
  },
  "variables": {
    "defaultSASKeyName": "RootManageSharedAccessKey",
    "authRuleResourceId": "[resourceId('Microsoft.ServiceBus/namespaces/authorizationRules', parameters('serviceBusNamespaceName'), variables('defaultSASKeyName'))]",
    "sbVersion": "2017-04-01"
  },
  "resources": [
    {
      "apiVersion": "2017-04-01",
      "name": "[parameters('serviceBusNamespaceName')]",
      "type": "Microsoft.ServiceBus/Namespaces",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "Standard"
      },
      "properties": {},
      "resources": [
        {
          "apiVersion": "2017-04-01",
          "name": "[parameters('serviceBusQueueName')]",
          "type": "Queues",
          "dependsOn": [
            "[concat('Microsoft.ServiceBus/namespaces/', parameters('serviceBusNamespaceName'))]"
          ],
          "properties": {
            "lockDuration": "PT5M",
            "maxSizeInMegabytes": "1024",
            "requiresDuplicateDetection": "false",
            "requiresSession": "false",
            "defaultMessageTimeToLive": "P10675199DT2H48M5.4775807S",
            "deadLetteringOnMessageExpiration": "false",
            "duplicateDetectionHistoryTimeWindow": "PT10M",
            "maxDeliveryCount": "10",
            "autoDeleteOnIdle": "P10675199DT2H48M5.4775807S",
            "enablePartitioning": "false",
            "enableExpress": "false"
          }
        }
      ]
    }
  ],
  "outputs": {
    "NamespaceConnectionString": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryConnectionString]"
    },
    "SharedAccessPolicyPrimaryKey": {
      "type": "string",
      "value": "[listkeys(variables('authRuleResourceId'), variables('sbVersion')).primaryKey]"
    }
  }
}
```

### <a name="create-a-parameters-file-optional"></a>Bir parametre dosyası (isteğe bağlı) oluşturun

Bir isteğe bağlı parametreler dosyası kullanmak için kopyalamanız [201-servicebus--kuyruk oluşturma](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) dosya. Değerini `serviceBusNamespaceName` bu dağıtımda oluşturun ve değerini değiştirmek istediğiniz hizmet veri yolu ad alanı ile `serviceBusQueueName` oluşturmak istediğiniz kuyruk adı.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "serviceBusNamespaceName": {
            "value": "<myNamespaceName>"
        },
        "serviceBusQueueName": {
            "value": "<myQueueName>"
        },
        "serviceBusApiVersion": {
            "value": "2017-04-01"
        }
    }
}
```

Daha fazla bilgi için bkz: [parametreleri](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) makalesi.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure'da oturum açma ve Azure abonelik ayarlayın

Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

```powershell
Login-AzureRmAccount
```

Azure hesabınızda oturum açmak için istenir. Oturum açtıktan sonra kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzureRMSubscription
```

Bu komut kullanılabilir Azure Aboneliklerin listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. Değiştir `<YourSubscriptionId>` kullanmak istediğiniz Azure aboneliği için GUID ile:

```powershell
Set-AzureRmContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Kaynak grubu

Grup, yeni bir kaynak grubu oluşturmak için mevcut bir kaynağı yoksa ** New-AzureRmResourceGroup ** komutu. Kullanmak istediğiniz konumu ve kaynak grubu adını sağlayın. Örneğin:

```powershell
New-AzureRmResourceGroup -Name MyDemoRG -Location "West US"
```

Başarılı olursa, yeni kaynak grubu bir özeti görüntülenir.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Dağıtımı test etme

Çalıştırarak, dağıtımınızı doğrulama `Test-AzureRmResourceGroupDeployment` cmdlet'i. Tam dağıtım yürütülürken gibi dağıtım sınarken parametreleri sağlar.

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Dağıtım oluşturma

Yeni dağıtım oluşturmak için çalıştırın `New-AzureRmResourceGroupDeployment` cmdlet'ini ve istendiğinde gerekli parametreleri belirtin. Parametreleri adına, kaynak grubu ve yolu veya URL'si şablon dosyası, dağıtımınız için bir ad içerir. Varsa **modu** parametresi belirtilmezse, varsayılan değeri **artımlı** kullanılır. Daha fazla bilgi için bkz: [artımlı ve tam dağıtımları](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

Aşağıdaki komutu PowerShell penceresinde üç parametre ister:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Dağıtım cmdlet'ini çalıştırdığınızda, satır içi parametreleri de kullanabilirsiniz. Komut aşağıdaki gibidir:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Çalıştırmak için bir [tam](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) dağıtımı, **modu** parametresi **tam**:

```powershell
New-AzureRmResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a>Dağıtımı doğrulama
Kaynakları başarıyla dağıtılmışsa, dağıtım özetini PowerShell penceresinde görüntülenir:

```powershell
DeploymentName    : MyDemoDeployment
ResourceGroupName : MyDemoRG
ProvisioningState : Succeeded
Timestamp         : 4/19/2017 10:38:30 PM
Mode              : Incremental
TemplateLink      :
Parameters        :
                    Name             Type                       Value
                    ===============  =========================  ==========
                    serviceBusNamespaceName  String             <namespaceName>
                    serviceBusQueueName  String                 <queueName>
                    serviceBusApiVersion  String                2017-04-01

```

## <a name="next-steps"></a>Sonraki adımlar
Şimdi bir Azure Resource Manager şablonunu dağıtmak için komutları ve temel iş akışı gördünüz. Daha ayrıntılı bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Azure Resource Manager'a genel bakış][Azure Resource Manager overview]
* [Resource Manager şablonları ve Azure PowerShell ile kaynakları dağıtma][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
