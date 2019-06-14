---
title: Resource Manager şablonlarını kullanarak Azure Service Bus kaynakları oluşturma | Microsoft Docs
description: Service Bus kaynaklarının oluşturulmasını otomatik hale getirmek için Azure Resource Manager şablonlarını kullanma
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: 24f6a207-0fa4-49cf-8a58-963f9e2fd655
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 09/11/2018
ms.author: spelluru
ms.openlocfilehash: 196b00f1268eada20d0e35473dc6eb43c9e48df6
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66111131"
---
# <a name="create-service-bus-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Service Bus kaynakları oluşturma

Bu makalede, Azure Resource Manager şablonları, PowerShell ve Service Bus kaynak Sağlayıcısı'nı kullanarak Service Bus kaynakları oluşturup dağıtmayı açıklar.

Azure Resource Manager şablonları, bir çözümü dağıtmak ve parametreleri ve farklı ortamlar için değer girmenizi sağlayan değişkenleri belirtmek için kaynakları tanımlamanıza yardımcı olur. Şablon JSON biçiminde yazılır ve dağıtımınız için değerleri oluşturmada kullanabileceğiniz ifadeler oluşur. Azure Resource Manager şablonları ve şablon biçimi ayrıntılı bir yazma hakkında ayrıntılı bilgi için bkz: [yapısını ve Azure Resource Manager şablonları söz dizimini](../azure-resource-manager/resource-group-authoring-templates.md).

> [!NOTE]
> Bu makaledeki örneklerde, bir Service Bus ad alanı ve mesajlaşma varlığı (sıra) oluşturmak için Azure Resource Manager'ı kullanma gösterilmektedir. Diğer şablon örneklerinde, ziyaret [Azure hızlı başlangıç şablonları galeri] [ Azure Quickstart Templates gallery] araması **Service Bus**.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="service-bus-resource-manager-templates"></a>Service Bus Resource Manager şablonları

Bu Service Bus Azure Resource Manager şablonları, yükleme ve dağıtım için kullanılabilir. Her biri, github'da şablonları bağlantılarla ilgili ayrıntıları için aşağıdaki bağlantılara tıklayın:

* [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
* [Kuyruk ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-queue.md)
* [Konu ve abonelik ile Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
* [Kuyruk ve yetkilendirme kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
* [Konusu, aboneliği ve kuralı ile bir Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)

## <a name="deploy-with-powershell"></a>PowerShell ile dağıtma

Aşağıdaki yordamı, standart katman hizmet veri yolu ad alanı ve bu ad alanı içinde bir kuyruk oluşturan bir Azure Resource Manager şablonu dağıtmak için PowerShell kullanmayı açıklar. Bu örnekte dayanır [sırası ile Service Bus ad alanı oluşturma](https://github.com/Azure/azure-quickstart-templates/tree/master/201-servicebus-create-queue) şablonu. Yaklaşık iş akışı aşağıdaki gibidir:

1. PowerShell yükleyin.
2. Şablon ve (isteğe bağlı olarak) bir parametre dosyası oluşturun.
3. PowerShell'de, Azure hesabınızda oturum açın.
4. Bir mevcut değilse yeni bir kaynak grubu oluşturun.
5. Dağıtımı test etme.
6. İsterseniz, dağıtım modu ayarlayın.
7. Şablonu dağıtın.

Azure Resource Manager şablonlarını dağıtma hakkında tam bilgi için bkz. [kaynakları Azure Resource Manager şablonları ile dağıtma][Deploy resources with Azure Resource Manager templates].

### <a name="install-powershell"></a>PowerShell yükleme

' Ndaki yönergeleri takip ederek Azure PowerShell'i yükleme [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Şablon oluşturma

Depo veya kopyalama kopyalama [201-servicebus-oluşturma-kuyruk](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.json) github'dan şablon:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
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

### <a name="create-a-parameters-file-optional"></a>Bir parametre dosyası oluşturma (isteğe bağlı)

İsteğe bağlı parametreler dosyası kullanmak için kopyalayın [201-servicebus-oluşturma-kuyruk](https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/azuredeploy.parameters.json) dosya. Değiştirin `serviceBusNamespaceName` bu dağıtımı oluşturma ve değerini değiştirmek istediğiniz Service Bus ad alanı adı ile `serviceBusQueueName` ile oluşturmak istediğiniz Kuyruğun adı.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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

Daha fazla bilgi için [parametreleri](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) makalesi.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure'da oturum açın ve Azure aboneliğini ayarlayın

Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

```powershell
Connect-AzAccount
```

Azure hesabınızda oturum açmak için istenir. Oturum açtıktan sonra kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

```powershell
Get-AzSubscription
```

Bu komut, kullanılabilir Azure abonelikleri listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. Değiştirin `<YourSubscriptionId>` ile kullanmak istediğiniz Azure abonelik GUİD'i:

```powershell
Set-AzContext -SubscriptionID <YourSubscriptionId>
```

### <a name="set-the-resource-group"></a>Kaynak grubu

Mevcut bir kaynak grubu, yeni bir kaynak grubu oluşturun yoksa **yeni AzResourceGroup** komutu. Kullanmak istediğiniz konum ve kaynak grubu adını sağlayın. Örneğin:

```powershell
New-AzResourceGroup -Name MyDemoRG -Location "West US"
```

Başarılı olursa, yeni kaynak grubunun bir özeti gösterilir.

```powershell
ResourceGroupName : MyDemoRG
Location          : westus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
```

### <a name="test-the-deployment"></a>Dağıtımı test etme

Çalıştırarak dağıtımınızı doğrulama `Test-AzResourceGroupDeployment` cmdlet'i. Tam dağıtım yürütülürken gibi test etme ve dağıtım parametreleri belirtin.

```powershell
Test-AzResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="create-the-deployment"></a>Dağıtım oluşturma

Yeni dağıtım oluşturmak için çalıştırılması `New-AzResourceGroupDeployment` cmdlet'ini ve istendiğinde gerekli parametreleri belirtin. Kaynak grubunuzu ve yolu veya URL adı şablon dosyasına dağıtımınız için bir ad parametreleri içerir. Varsa **modu** parametresi belirtilmezse, varsayılan değerini **artımlı** kullanılır. Daha fazla bilgi için [artımlı ve tam dağıtımları](../azure-resource-manager/deployment-modes.md).

Aşağıdaki komutu PowerShell penceresine üç parametrelerinde ister:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

Dağıtım cmdlet'ini çalıştırdığınızda, satır içi parametreleri kullanabilirsiniz. Komutu aşağıdaki gibidir:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
```

Çalıştırılacak bir [tam](../azure-resource-manager/deployment-modes.md) dağıtımı, **modu** parametresi **tam**:

```powershell
New-AzResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
```

### <a name="verify-the-deployment"></a>Dağıtımı doğrulama
Kaynakları başarıyla dağıtılırsa, dağıtımın bir özeti PowerShell penceresinde görüntülenir:

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
Artık, bir Azure Resource Manager şablonu dağıtmak için komutları ve temel iş akışı gördünüz. Daha ayrıntılı bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Azure Resource Manager'a genel bakış][Azure Resource Manager overview]
* [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma][Deploy resources with Azure Resource Manager templates]
* [Azure Resource Manager şablonları yazma](../azure-resource-manager/resource-group-authoring-templates.md)
* [Microsoft.ServiceBus kaynak türleri](/azure/templates/microsoft.servicebus/allversions)

[Azure Resource Manager overview]: ../azure-resource-manager/resource-group-overview.md
[Deploy resources with Azure Resource Manager templates]: ../azure-resource-manager/resource-group-template-deploy.md
[Azure Quickstart Templates gallery]: https://azure.microsoft.com/documentation/templates/?term=service+bus
