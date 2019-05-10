---
title: Azure Resource Manager şablonlarını kullanarak Azure zaman serisi görüşleri ortamınızı yönetmek nasıl | Microsoft Docs
description: Bu makalede, Azure Resource Manager kullanarak program aracılığıyla Azure Time Series Insights ortamınızı yönetmek açıklar.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 05/08/2019
ms.custom: seodec18
ms.openlocfilehash: f5e350e8a9093936f1e747afda7c3192b4d8368d
ms.sourcegitcommit: 399db0671f58c879c1a729230254f12bc4ebff59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65471722"
---
# <a name="create-time-series-insights-resources-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Time Series Insights kaynakları oluşturma

Bu makalede, Azure Resource Manager şablonları, PowerShell ve zaman serisi görüşleri kaynak sağlayıcısını kullanarak zaman serisi görüşleri kaynakları oluşturup dağıtmayı açıklar.

Time Series Insights, aşağıdaki kaynakları destekler:

   | Resource | Açıklama |
   | --- | --- |
   | Ortam | Zaman serisi görüşleri ortamına olayları olay aracıları okuyun, depolanır ve sorgu için kullanılabilir hale mantıksal bir gruplandırmasıdır. Daha fazla bilgi için [Azure Time Series Insights ortamınızı planlama](time-series-insights-environment-planning.md) |
   | Olay Kaynağı | Olay kaynağı, zaman serisi görüşleri okur ve ortama olayları alır bir olay aracısından yapılan bir bağlantıdır. Şu anda desteklenen olay kaynakları, IOT Hub ve Event Hub ' dir. |
   | Başvuru veri kümesi | Başvuru veri kümelerini ortamında olaylar hakkında meta veriler sağlar. Başvuru veri kümesi meta verileri ile olayları sırasında giriş katılır. Başvuru veri kümesi kaynaklar olay anahtar özellikleri tarafından tanımlanır. Başvuru veri kümesi ' sağlayan gerçek meta veriler yüklendiğinde veya veri düzlemi API'leri değişiklik. |
   | Erişim İlkesi | Erişim ilkeleri, veri sorguları gönderme, ortamdaki başvuru verilerini işleme ve kaydedilen sorguları ve Perspektifleri ortamla ilişkilendirilmiş paylaşım izni verin. Daha fazla bilgi için okuma [Azure portalını kullanarak zaman serisi görüşleri ortamına veri erişimi verme](time-series-insights-data-access.md) |

Resource Manager şablonu bir kaynak grubunda altyapı ve kaynakların yapılandırmasını tanımlayan bir JSON dosyasıdır. Aşağıdaki belgeler, şablon dosyaları daha ayrıntılı açıklanmıştır:

- [Azure Resource Manager'a genel bakış - şablon dağıtımı](../azure-resource-manager/resource-group-overview.md#template-deployment)
- [Kaynakları Resource Manager şablonları ve Azure PowerShell ile dağıtma](../azure-resource-manager/resource-group-template-deploy.md)
- [Microsoft.TimeSeriesInsights kaynak türleri](/azure/templates/microsoft.timeseriesinsights/allversions)

[201-timeseriesinsights-ortam-ile-eventhub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-timeseriesinsights-environment-with-eventhub) Hızlı Başlangıç şablonu, Github'da yayımlanır. Bu şablon, bir zaman serisi görüşleri ortamı, Event Hub'ındaki olayları kullanma ve erişim ortamın verilerine erişim izni ilkeleri için yapılandırılmış bir alt olay kaynağı oluşturur. Mevcut bir olay hub'ı belirtilmezse, bir dağıtım ile oluşturulur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="specify-deployment-template-and-parameters"></a>Dağıtım şablonu ve parametreleri belirtin

Aşağıdaki yordam, bir zaman serisi görüşleri ortamı, Event Hub'ındaki olayları kullanma ve erişim için erişim ilkeleri için yapılandırılmış bir alt olay kaynağı oluşturan bir Azure Resource Manager şablonu dağıtmak için PowerShell kullanmayı açıklar ortamının verileri. Mevcut bir olay hub'ı belirtilmezse, bir dağıtım ile oluşturulur.

1. ' Ndaki yönergeleri takip ederek Azure PowerShell'i yükleme [Azure PowerShell'i kullanmaya başlama](/powershell/azure/get-started-azureps).

1. Kopyala veya kopyalama [201-timeseriesinsights-ortam-ile-eventhub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.json) github'dan şablon.

   * Bir parametre dosyası oluşturma

     Bir parametre dosyası oluşturmak için kopyalama [201-timeseriesinsights-ortam-ile-eventhub](https://github.com/Azure/azure-quickstart-templates/blob/master/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json) dosya.

      [!code-json[deployment-parameters](~/quickstart-templates/201-timeseriesinsights-environment-with-eventhub/azuredeploy.parameters.json)]

    <div id="required-parameters"></div>

   * Gerekli Parametreler

     | Parametre | Açıklama |
     | --- | --- |
     | eventHubNamespaceName | Kaynak olay hub'ı ad alanı. |
     | eventHubName | Kaynak olay hub'ı adı. |
     | consumerGroupName | Time Series Insights hizmeti, verileri olay hub'ından okumak için kullanacağı tüketici grubunun adı. **NOT:** Kaynak çekişmesinden kaçınmak için bu tüketici grubunun Time Series Insights hizmeti için ayrılmış olmalıdır ve diğer okuyucularıyla paylaşılmaz. |
     | EnvironmentName | Ortam adı. Adı içeremez: `<`, `>`, `%`, `&`, `:`, `\\`, `?`, `/`, ve herhangi bir denetim karakterini. Diğer tüm karakterlere izin verilir.|
     | eventSourceName | Olay kaynağı alt kaynak adı. Adı içeremez: `<`, `>`, `%`, `&`, `:`, `\\`, `?`, `/`, ve herhangi bir denetim karakterini. Diğer tüm karakterlere izin verilir. |

    <div id="optional-parameters"></div>

   * İsteğe bağlı parametreler

     | Parametre | Açıklama |
     | --- | --- |
     | existingEventHubResourceId | Olay kaynağı aracılığıyla zaman serisi görüşleri ortamına bağlı bir olay Hub'ının bir isteğe bağlı kaynak kimliği. **NOT:** Şablonu dağıtarak kullanıcı olay hub'ı listkeys'i işlemi gerçekleştirmek için ayrıcalıkları olmalıdır. Hiçbir değer iletilmezse, yeni bir olay hub'ı şablon tarafından oluşturulur. |
     | environmentDisplayName | Ortam adı yerine araçları veya kullanıcı arabirimi göstermek için isteğe bağlı bir kolay ad. |
     | environmentSkuName | Sku'nun adı. Daha fazla bilgi için [zaman serisi öngörüleri Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/time-series-insights/).  |
     | environmentSkuCapacity | Birim kapasitesi Sku'ların. Daha fazla bilgi için [zaman serisi öngörüleri Fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/time-series-insights/).|
     | environmentDataRetentionTime | En düşük timespan ortamın olayları sorgu için kullanılabilir. Değer ISO 8601 biçiminde örneğin belirtilmelidir `P30D` 30 günlük bir bekletme ilkesi için. |
     | eventSourceDisplayName | Araç veya kullanıcı arabirimi yerine olay kaynağı adını göstermek için isteğe bağlı bir kolay ad. |
     | eventSourceTimestampPropertyName | Olay kaynağının zaman damgası kullanılacak olay özelliği. TimestampPropertyName için bir değer belirtilmezse veya null veya boş dize belirtilirse, olay oluşturulma zamanı kullanılır. |
     | eventSourceKeyName | Time Series Insights hizmetinin, olay hub'ına bağlanmak için kullanacağı bir paylaşılan erişim anahtarı adı. |
     | accessPolicyReaderObjectIds | Nesne kimlikleri bir kullanıcı ya da ortam okuyucu erişimi olması gereken Azure AD uygulamaları listesi. Hizmet sorumlusu nesne kimliği çağrılarak alınabilir **Get-AzADUser** veya **Get-AzADServicePrincipal** cmdlet'leri. Azure AD grupları için bir erişim ilkesi oluşturma henüz desteklenmiyor. |
     | accessPolicyContributorObjectIds | Nesne kimlikleri bir kullanıcı ya da ortam katkıda bulunan erişimi olması gereken Azure AD uygulamaları listesi. Hizmet sorumlusu nesne kimliği çağrılarak alınabilir **Get-AzADUser** veya **Get-AzADServicePrincipal** cmdlet'leri. Azure AD grupları için bir erişim ilkesi oluşturma henüz desteklenmiyor. |

   * Örneğin, aşağıdaki parametre dosyasını ortamı ve var olan bir olay hub'ından olayları yazan olay kaynağını oluşturmak için kullanılacak. Ayrıca, katkıda bulunan bir ortama erişim iki erişim ilkesi oluşturur.

     ```json
     {
         "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
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
  
    * Daha fazla bilgi için [parametreleri](../azure-resource-manager/resource-group-template-deploy.md#parameter-files) makalesi.

## <a name="deploy-the-quickstart-template-locally-using-powershell"></a>PowerShell kullanarak yerel olarak hızlı başlangıç şablonu dağıtma

> [!IMPORTANT]
> Aşağıda gösterilen komut satırı işlemleri açıklayan [Az PowerShell Modülü](https://docs.microsoft.com/powershell/azure/overview).

1. PowerShell'de, Azure hesabınızda oturum açın.

    * Bir PowerShell isteminden aşağıdaki komutu çalıştırın:

      ```powershell
      Connect-AzAccount
      ```

    * Azure hesabınızda oturum açmak için istenir. Oturum açtıktan sonra kullanılabilir aboneliklerinizi görüntülemek için aşağıdaki komutu çalıştırın:

      ```powershell
      Get-AzSubscription
      ```

    * Bu komut, kullanılabilir Azure abonelikleri listesini döndürür. Aşağıdaki komutu çalıştırarak geçerli oturum için bir abonelik seçin. Değiştirin `<YourSubscriptionId>` ile kullanmak istediğiniz Azure abonelik GUİD'i:

      ```powershell
      Set-AzContext -SubscriptionID <YourSubscriptionId>
      ```

1. Bir mevcut değilse yeni bir kaynak grubu oluşturun.

   * Mevcut bir kaynak grubu, yeni bir kaynak grubu oluşturun yoksa **yeni AzResourceGroup** komutu. Kullanmak istediğiniz konum ve kaynak grubu adını sağlayın. Örneğin:

     ```powershell
     New-AzResourceGroup -Name MyDemoRG -Location "West US"
     ```

   * Başarılı olursa, yeni kaynak grubunun bir özeti gösterilir.

     ```powershell
     ResourceGroupName : MyDemoRG
     Location          : westus
     ProvisioningState : Succeeded
     Tags              :
     ResourceId        : /subscriptions/<GUID>/resourceGroups/MyDemoRG
     ```

1. Dağıtımı test etme.

   * Çalıştırarak dağıtımınızı doğrulama `Test-AzResourceGroupDeployment` cmdlet'i. Tam dağıtım yürütülürken gibi test etme ve dağıtım parametreleri belirtin.

     ```powershell
     Test-AzResourceGroupDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
     ```

1. Dağıtım oluşturma

    * Yeni dağıtım oluşturmak için çalıştırılması `New-AzResourceGroupDeployment` cmdlet'ini ve istendiğinde gerekli parametreleri belirtin. Kaynak grubunuzu ve yolu veya URL adı şablon dosyasına dağıtımınız için bir ad parametreleri içerir. Varsa **modu** parametresi belirtilmezse, varsayılan değerini **artımlı** kullanılır. Daha fazla bilgi için [artımlı ve tam dağıtımları](../azure-resource-manager/deployment-modes.md).

    * Aşağıdaki komutu PowerShell penceresine beş gerekli parametrelerin ister:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json 
      ```

    * Bunun yerine bir parametre dosyası belirtmek için aşağıdaki komutu kullanın:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -TemplateParameterFile <path to parameters file>\azuredeploy.parameters.json
      ```

    * Dağıtım cmdlet'ini çalıştırdığınızda, satır içi parametreleri kullanabilirsiniz. Komutu aşağıdaki gibidir:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json -parameterName "parameterValue"
      ```

    * Çalıştırılacak bir [tam](../azure-resource-manager/deployment-modes.md) dağıtımı, **modu** parametresi **tam**:

      ```powershell
      New-AzResourceGroupDeployment -Name MyDemoDeployment -Mode Complete -ResourceGroupName MyDemoRG -TemplateFile <path to template file>\azuredeploy.json
      ```

1. Dağıtımı doğrulama

    * Kaynakları başarıyla dağıtılırsa, dağıtımın bir özeti PowerShell penceresinde görüntülenir:

      ```powershell
       DeploymentName          : MyDemoDeployment
       ResourceGroupName       : MyDemoRG
       ProvisioningState       : Succeeded
       Timestamp               : 5/8/2019 10:28:34 PM
       Mode                    : Incremental
       TemplateLink            :
       Parameters              :
                                 Name                                Type                       Value
                                 ==================================  =========================  ==========
                                 eventHubNewOrExisting               String                     new
                                 eventHubResourceGroup               String                     MyDemoRG
                                 eventHubNamespaceName               String                     tsiquickstartns
                                 eventHubName                        String                     tsiquickstarteh
                                 consumerGroupName                   String                     tsiquickstart
                                 environmentName                     String                     tsiquickstart
                                 environmentDisplayName              String                     tsiquickstart
                                 environmentSkuName                  String                     S1
                                 environmentSkuCapacity              Int                        1
                                 environmentDataRetentionTime        String                     P30D
                                 eventSourceName                     String                     tsiquickstart
                                 eventSourceDisplayName              String                     tsiquickstart
                                 eventSourceTimestampPropertyName    String
                                 eventSourceKeyName                  String                     manage
                                 accessPolicyReaderObjectIds         Array                      []
                                 accessPolicyContributorObjectIds    Array                      []
                                 location                            String                     westus

       Outputs                 :
                                  Name              Type                       Value
                                  ================  =========================  ==========
                                  dataAccessFQDN    String
                                  11aa1aa1-a1aa-1a1a-a11a-aa111a111a11.env.timeseries.azure.com

       DeploymentDebugLogLevel :
      ```

1. Azure portalı üzerinden Hızlı Başlangıç şablonu dağıtma

   * GitHub Hızlı Başlangıç şablonu giriş sayfasında da içeren bir **azure'a Dağıt** düğmesi. Tıklayarak Azure Portalı'nda bir özel dağıtım sayfası açılır. Bu sayfadan girin veya her parametreler için değerler seçin [Gerekli Parametreler](#required-parameters) veya [isteğe bağlı parametreler](#optional-parameters) tablolar. Tıklayarak ayarlarını doldurduktan sonra **satın alma** düğmesi şablon dağıtımı başlatır.
    </br>
    </br>
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-timeseriesinsights-environment-with-eventhub%2Fazuredeploy.json" target="_blank">
       <img src="https://azuredeploy.net/deploybutton.png"/>
    </a>

## <a name="next-steps"></a>Sonraki adımlar

- Time Series Insights kaynakları REST API'lerini kullanarak programlama yoluyla yönetme hakkında daha fazla bilgi için bkz: [zaman serisi öngörüleri Yönetimi](https://docs.microsoft.com/rest/api/time-series-insights-management/).
