---
title: Azure Resource Manager şablonları kullanarak Azure zaman serisi Öngörüler ortamınızı yönetmek nasıl | Microsoft Docs
description: Bu makalede, Azure zaman serisi Öngörüler ortamınızı program aracılığıyla Azure Kaynak Yöneticisi'ni kullanarak yönetmek açıklar.
ms.service: time-series-insights
services: time-series-insights
author: sandshadow
ms.author: edett
manager: jhubbard
ms.reviewer: anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 12/08/2017
ms.openlocfilehash: 99aabc01132da60a1b09fcf65f439b8e084bbffa
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34651973"
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonları kullanarak zaman serisi Öngörüler kaynakları oluşturun

Bu makalede, Azure Resource Manager şablonları, PowerShell ve zaman serisi Öngörüler kaynak sağlayıcısı kullanılarak zaman serisi Öngörüler kaynakları oluşturup açıklar.

Zaman serisi Öngörüler aşağıdaki kaynaklara destekler:
   | Kaynak | Açıklama |
   | --- | --- |
   | Ortam | Olay aracıların okuyun, depolanır ve sorgu için kullanılabilir hale olaylar mantıksal bir gruplandırması bir zaman serisi Öngörüler ortamıdır. Daha fazla bilgi için bkz: [Azure zaman serisi Öngörüler ortamınızı planlama](time-series-insights-environment-planning.md) |
   | Olay Kaynağı | Bir olay kaynağı içinden zaman serisi Öngörüler okur ve ortama olayları alır olay aracısı için bir bağlantıdır. Şu anda desteklenen olay kaynakları IOT Hub ve Event Hub'dır. |
   | Başvuru veri kümesi | Başvuru veri kümelerini ortamında olaylar hakkında meta veri sağlar. Başvuru veri kümelerini meta verilerde olaylarla giriş sırasında katılması. Başvuru veri kümelerini kaynaklar olarak olay anahtar özellikleri tarafından tanımlanır. Başvuru veri ayarlama yapar gerçek meta veriler karşıya veya veri düzlemi API'leri değiştirilemiyor. |
   | Erişim İlkesi | Erişim ilkeleri, veri sorguları göndermek, ortamında başvuru verileri işlemek ve ortamla ilgili Perspektif ve kaydedilmiş sorguları paylaşmak için izinleri verin. Daha fazla bilgi için bkz: [Azure portalını kullanarak bir zaman serisi Öngörüler ortamı için veri erişim](time-series-insights-data-access.md) |

Resource Manager şablonu bir kaynak grubunda altyapı ve kaynakların yapılandırmasını tanımlayan bir JSON dosyasıdır. Daha fazla bilgi için aşağıdaki belgelere bakın:

- [Azure Resource Manager'a genel bakış - şablon dağıtımı](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)

[201-timeseriesinsights-ortamı-ile-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) hızlı başlatma şablonunu Github'da yayımlanır. Bu şablon bir zaman serisi Öngörüler ortamında, Event Hub'ındaki olayları kullanmak ve ortam verilerine erişim vermek ilkeleri erişmek için yapılandırılmış bir alt olay kaynağı oluşturur. Varolan bir Event Hub belirtilmezse, bir dağıtımı ile oluşturulur.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>PowerShell kullanarak yerel olarak hızlı başlangıç şablonu dağıtma

Aşağıdaki yordam, bir zaman serisi Öngörüler ortamı, Event Hub'ındaki olayları kullanmak ve erişim ilkeleri erişmek için yapılandırılmış bir alt olay kaynağı oluşturur bir Azure Resource Manager şablonu dağıtmak için PowerShell kullanmayı açıklar Ortam verileri. Varolan bir Event Hub belirtilmezse, bir dağıtımı ile oluşturulur.

Yaklaşık iş akışı aşağıdaki gibidir:

1. PowerShell yükleyin.
1. Şablonu ve parametre dosyası oluşturun.
1. PowerShell'de, Azure hesabınızda oturum açın.
1. Bir mevcut değilse yeni bir kaynak grubu oluşturun.
1. Dağıtımı test etme.
1. Şablon dağıtın.

### <a name="install-powershell"></a>PowerShell yükleme

Azure PowerShell yönergelerini takip ederek yükleyin [Azure PowerShell ile çalışmaya başlama](/powershell/azure/get-started-azureps).

### <a name="create-a-template"></a>Şablon oluşturma

Kopya veya kopya [201-timeseriesinsights-ortamı-ile-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) github'dan şablon.

### <a name="create-a-parameters-file"></a>Bir parametre dosyası oluşturma

Bir parametre dosyası oluşturmak için kopyalama [201-timeseriesinsights-ortamı-ile-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) dosya.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "eventHubNamespaceName": {
          "value": "GEN-UNIQUE"
      },
      "eventHubName": {
          "value": "GEN-UNIQUE"
      },
      "consumerGroupName": {
          "value": "GEN-UNIQUE"
      },
      "environmentName": {
        "value": "GEN-UNIQUE"
      },
      "eventSourceName": {
        "value": "GEN-UNIQUE"
      }
  }
}
```

#### <a name="required-parameters"></a>Gerekli Parametreler

   | Parametre | Açıklama |
   | --- | --- |
   | eventHubNamespaceName | Kaynak olay hub'ı ad alanı. |
   | eventHubName | Kaynak olay hub'ı adı. |
   | consumerGroupName | Zaman serisi Öngörüler hizmeti olay hub'ından veri okumak için kullanacağı tüketici grubu adı. **Not:** kaynak çekişmesini önlemek için bu tüketici grubu için zaman serisi Öngörüler hizmeti ayrılmış ve gerekir diğer okuyucularıyla paylaşılmaz. |
   | EnvironmentName | Ortam adı. Adı içeremez: ' <', ' >', '%', '&', ': ','\\','?', '/' ve herhangi bir denetim karakteri. Diğer tüm karakterler izin verilir.|
   | eventSourceName | Olay kaynağı alt kaynağın adı. Adı içeremez: ' <', ' >', '%', '&', ': ','\\','?', '/' ve herhangi bir denetim karakteri. Diğer tüm karakterler izin verilir. |

#### <a name="optional-parameters"></a>İsteğe bağlı parametreler

   | Parametre | Açıklama |
   | --- | --- |
   | existingEventHubResourceId | Olay kaynağı aracılığıyla zaman serisi Öngörüler ortamına bağlı bir olay Hub'ının bir isteğe bağlı kaynak kimliği. **Not:** şablon dağıtma kullanıcı olay hub'ındaki listkeys işlemi gerçekleştirmek için ayrıcalıkları olmalıdır. Herhangi bir değer iletilmezse, yeni bir olay hub'ı şablon tarafından oluşturulur. |
   | environmentDisplayName | Ortam adı yerine araç ya da kullanıcı arabirimleri göstermek için isteğe bağlı bir kolay ad. |
   | environmentSkuName | Sku adı. Daha fazla bilgi için bkz: [zaman serisi Öngörüler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
   | environmentSkuCapacity | Sku birim kapasitesi. Daha fazla bilgi için bkz: [zaman serisi Öngörüler Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/time-series-insights/).|
   | environmentDataRetentionTime | Minimum timespan ortamı olayları sorgu için kullanılabilir. Değer, örneğin "P30D" bir bekletme ilkesi için 30 günlük ISO 8601 biçiminde belirtilmelidir. |
   | eventSourceDisplayName | Olay kaynağı adı yerine araç ya da kullanıcı arabirimleri göstermek için isteğe bağlı bir kolay ad. |
   | eventSourceTimestampPropertyName | Olay kaynağının zaman damgası kullanılacak olay özelliği. TimestampPropertyName için bir değer belirtilmezse ya da null veya boş dize belirtilirse, olay oluşturma zamanı kullanılır. |
   | eventSourceKeyName | Zaman serisi Öngörüler hizmeti olay hub'ına bağlanmak için kullanacağı paylaşılan erişim anahtarı adı. |
   | accessPolicyReaderObjectIds | Nesne fazla kullanıcı ya da ortam okuyucu erişimi olması gereken Azure AD uygulamalarında kimlikleri listesi. Hizmet asıl objectID çağırarak elde edilebilir **Get-AzureRMADUser** veya **Get-AzureRMADServicePrincipal** cmdlet'leri. Azure AD grupları için bir erişim ilkesi oluşturma henüz desteklenmiyor. |
   | accessPolicyContributorObjectIds | Nesne fazla kullanıcı ya da ortam katkıda bulunan erişimi olması gereken Azure AD uygulamalarında kimlikleri listesi. Hizmet asıl objectID çağırarak elde edilebilir **Get-AzureRMADUser** veya **Get-AzureRMADServicePrincipal** cmdlet'leri. Azure AD grupları için bir erişim ilkesi oluşturma henüz desteklenmiyor. |

Örnek olarak, aşağıdaki parametreleri dosya bir ortamda ve var olan bir olay hub'ından olayları okur bir olay kaynağı oluşturmak için kullanılacak. Ayrıca, katkıda bulunan bir ortama erişim iki erişim ilkeleri oluşturur.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "eventHubNamespaceName": {
            "value": "tsiTemplateTestNamespace"
        },
        "eventHubName": {
            "value": "tsiTemplateTestEventHub"
        },
        "consumerGroupName": {
            "value": "tsiTemplateTestConsumerGroup"
        },
        "environmentName": {
          "value": "tsiTemplateTestEnvironment"
        },
        "eventSourceName": {
          "value": "tsiTemplateTestEventSource"
        },
        "existingEventHubResourceId": {
          "value": "/subscriptions/{yourSubscription}/resourceGroups/MyDemoRG/providers/Microsoft.EventHub/namespaces/tsiTemplateTestNamespace/eventhubs/tsiTemplateTestEventHub"
        },
        "accessPolicyContributorObjectIds": {
            "value": [
                "AGUID001-0000-0000-0000-000000000000",
                "AGUID002-0000-0000-0000-000000000000"
            ]
        }
    }
  }
```

Daha fazla bilgi için bkz: [parametreleri](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) makalesi.

### <a name="log-in-to-azure-and-set-the-azure-subscription"></a>Azure'da oturum açma ve Azure abonelik ayarlayın

Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

```powershell
Connect-AzureRmAccount
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

Grup, yeni bir kaynak grubu oluşturmak için mevcut bir kaynağı yoksa **New-AzureRmResourceGroup** komutu. Kullanmak istediğiniz konumu ve kaynak grubu adını sağlayın. Örneğin:

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
Test-AzureRmResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
```

### <a name="create-the-deployment"></a>Dağıtım oluşturma

Yeni dağıtım oluşturmak için çalıştırın `New-AzureRmResourceGroupDeployment` cmdlet'ini ve istendiğinde gerekli parametreleri belirtin. Parametreleri adına, kaynak grubu ve yolu veya URL'si şablon dosyası, dağıtımınız için bir ad içerir. Varsa **modu** parametresi belirtilmezse, varsayılan değeri **artımlı** kullanılır. Daha fazla bilgi için bkz: [artımlı ve tam dağıtımları](../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments).

Aşağıdaki komut beş gerekli parametrelerin PowerShell penceresinde ister:

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

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

Kaynakları başarıyla dağıtılmışsa, dağıtım özetini PowerShell penceresinde görüntülenir:

```powershell
DeploymentName          : azuredeploy
ResourceGroupName       : MyDemoRG
ProvisioningState       : Succeeded
Timestamp               : 12/9/2017 01:06:54
Mode                    : Incremental
TemplateLink            :
Parameters              :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          eventHubNamespaceName  String               <eventHubNamespaceName>
                          eventHubName     String                     <eventHubName>
                          consumerGroupName  String                   <consumerGroupName>
                          existingEventHubResourceId  String
                          environmentName  String                     <environmentName>
                          environmentDisplayName  String
                          environmentSkuName  String                  S1
                          environmentSkuCapacity  Int                 1
                          environmentDataRetentionTime  String        P30D
                          eventSourceName  String                     <eventSourceName>
                          eventSourceDisplayName  String
                          eventSourceTimestampPropertyName  String
                          eventSourceKeyName  String                  manage
                          accessPolicyReaderObjectIds  Array          []
                          accessPolicyContributorObjectIds  Array     [
                            "AGUID001-0000-0000-0000-000000000000",
                            "AGUID002-0000-0000-0000-000000000000"
                          ]

Outputs                 :
                          Name             Type                       Value
                          ===============  =========================  ==========
                          dataAccessFQDN   String
                          <guid>.env.timeseries.azure.com
```

## <a name="deploy-the-quickstart-template-through-the-azure-portal"></a>Azure portalı üzerinden Hızlı Başlangıç şablonu dağıtma

GitHub hızlı başlangıç şablonun giriş sayfasında da içeren bir **Azure'a Dağıt** düğmesi. Tıklatarak Azure Portalı'nda bir özel dağıtım sayfası açılır. Bu sayfadan girin veya her parametreler için değerler seçin [Gerekli Parametreler](time-series-insights-manage-resources-using-azure-resource-manager-template.md#required-parameters) veya [isteğe bağlı parametreler](time-series-insights-manage-resources-using-azure-resource-manager-template.md#optional-parameters) tabloları. Ayarları tıklayarak, doldurduktan sonra **satın alma** düğmesi şablon dağıtımı başlatmak.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

## <a name="next-steps"></a>Sonraki adımlar

- Program aracılığıyla REST API'lerini kullanarak zaman serisi Öngörüler kaynaklarını yönetme hakkında daha fazla bilgi için bkz: [zaman serisi Öngörüler Yönetimi](https://docs.microsoft.com/rest/api/time-series-insights-management/).
