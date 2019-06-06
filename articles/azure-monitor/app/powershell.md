---
title: Azure Application ınsights'ı PowerShell ile otomatik hale getirin | Microsoft Docs
description: PowerShell kullanarak bir Azure Resource Manager şablonu oluşturma kaynağı, uyarı ve kullanılabilirlik testlerini otomatikleştirin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: mbullwin
ms.openlocfilehash: 07d52544b584adb02cc60790b7cb63c8aee1e366
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514469"
---
#  <a name="create-application-insights-resources-using-powershell"></a>PowerShell ile Application Insights kaynakları oluşturma

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Bu makalede oluşturma ve güncelleştirilmesini otomatikleştirmek gösterilmektedir [Application Insights](../../azure-monitor/app/app-insights-overview.md) Azure kaynak yönetimi kullanarak otomatik olarak kaynaklar. Örneğin, bir yapı işleminin parçası olarak bunu olabilir. Temel Application Insights kaynağını yanı sıra, oluşturduğunuz [kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md), ayarlayın [uyarılar](../../azure-monitor/app/alerts.md)ayarlayın [düzeni fiyatlandırma](pricing.md)ve diğer Azure kaynakları oluşturma .

Bu kaynakları oluşturmak için JSON şablonları anahtardır [Azure Resource Manager](../../azure-resource-manager/manage-resources-powershell.md). Buna koysalar yordam aynıdır: var olan kaynakların; JSON tanımları indirme adları gibi belirli değerleri Parametreleştirme; ve yeni bir kaynak oluşturmak istediğinizde şablonu çalıştırın. Çeşitli kaynaklar birlikte paket, bunları oluşturmak için tek - Örneğin, bir uygulama İzleyicisi kullanılabilirlik testleri, uyarılar ve depolama için sürekli dışarı aktarma ile gidin. Burada açıklayacağız parameterizations bazılarının bazı ıot'nin vardır.

## <a name="one-time-setup"></a>Bir kerelik Kurulum
Azure aboneliğiniz önce PowerShell kullanmadıysanız:

Azure Powershell modülü, komut dosyalarını çalıştırmak istediğiniz makineye yükleyin:

1. Yükleme [Microsoft Web Platformu Yükleyicisi (v5 veya üzeri)](https://www.microsoft.com/web/downloads/platform.aspx).
2. Microsoft Azure PowerShell'i yüklemek için kullanın.

## <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma
Yeni bir .json dosyası oluşturma - adlandıralım `template1.json` Bu örnekte. Bu içeriği dosyaya kopyalayın:

```JSON
    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "appName": {
                "type": "string",
                "metadata": {
                    "description": "Enter the application name."
                }
            },
            "appType": {
                "type": "string",
                "defaultValue": "web",
                "allowedValues": [
                    "web",
                    "java",
                    "other"
                ],
                "metadata": {
                    "description": "Enter the application type."
                }
            },
            "appLocation": {
                "type": "string",
                "defaultValue": "East US",
                "allowedValues": [
                    "South Central US",
                    "West Europe",
                    "East US",
                    "North Europe"
                ],
                "metadata": {
                    "description": "Enter the application location."
                }
            },
            "priceCode": {
                "type": "int",
                "defaultValue": 1,
                "allowedValues": [
                    1,
                    2
                ],
                "metadata": {
                    "description": "1 = Per GB (Basic), 2 = Per Node (Enterprise)"
                }
            },
            "dailyQuota": {
                "type": "int",
                "defaultValue": 100,
                "minValue": 1,
                "metadata": {
                    "description": "Enter daily quota in GB."
                }
            },
            "dailyQuotaResetTime": {
                "type": "int",
                "defaultValue": 24,
                "metadata": {
                    "description": "Enter daily quota reset hour in UTC (0 to 23). Values outside the range will get a random reset hour."
                }
            },
            "warningThreshold": {
                "type": "int",
                "defaultValue": 90,
                "minValue": 1,
                "maxValue": 100,
                "metadata": {
                    "description": "Enter the % value of daily quota after which warning mail to be sent. "
                }
            }
        },
        "variables": {
            "priceArray": [
                "Basic",
                "Application Insights Enterprise"
            ],
            "pricePlan": "[take(variables('priceArray'),parameters('priceCode'))]",
            "billingplan": "[concat(parameters('appName'),'/', variables('pricePlan')[0])]"
        },
        "resources": [
            {
                "type": "microsoft.insights/components",
                "kind": "[parameters('appType')]",
                "name": "[parameters('appName')]",
                "apiVersion": "2014-04-01",
                "location": "[parameters('appLocation')]",
                "tags": {},
                "properties": {
                    "ApplicationId": "[parameters('appName')]"
                },
                "dependsOn": []
            },
            {
                "name": "[variables('billingplan')]",
                "type": "microsoft.insights/components/CurrentBillingFeatures",
                "location": "[parameters('appLocation')]",
                "apiVersion": "2015-05-01",
                "dependsOn": [
                    "[resourceId('microsoft.insights/components', parameters('appName'))]"
                ],
                "properties": {
                    "CurrentBillingFeatures": "[variables('pricePlan')]",
                    "DataVolumeCap": {
                        "Cap": "[parameters('dailyQuota')]",
                        "WarningThreshold": "[parameters('warningThreshold')]",
                        "ResetTime": "[parameters('dailyQuotaResetTime')]"
                    }
                }
            }
        ]
    }
```



## <a name="create-application-insights-resources"></a>Create Application Insights kaynağı oluşturma
1. PowerShell'de, Azure'da oturum açın:
   
    `Connect-AzAccount`
2. Böyle bir komut çalıştırın:
   
    ```PS
   
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName` Yeni kaynaklar oluşturmak için istediğiniz grubudur.
   * `-TemplateFile` Özel Parametreler önce gerçekleşmelidir.
   * `-appName` Oluşturulacak kaynağın adı.

Diğer parametreler ekleyebilirsiniz - açıklamalarını şablon parametreleri bölümünde bulabilirsiniz.

## <a name="to-get-the-instrumentation-key"></a>İzleme anahtarını almak için
Uygulama kaynağı oluşturduktan sonra izleme anahtarını isteyeceksiniz: 

```PS
    $resource = Find-AzResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>Fiyat planı ayarlayın

Ayarlayabileceğiniz [fiyat planı](pricing.md).

Yukarıdaki şablonu kullanarak kurumsal fiyat planı ile bir uygulama kaynak oluşturmak için:

```PS
        New-AzResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|planı|
|---|---|
|1|Temel|
|2|Enterprise|

* Yalnızca varsayılan temel fiyat planı kullanmak istiyorsanız, şablonu CurrentBillingFeatures kaynak atlayabilirsiniz.
* Fiyat planı bileşen kaynak oluşturulduktan sonra değiştirmek istiyorsanız, "microsoft.insights/components" kaynak atlar bir şablon kullanabilirsiniz. Ayrıca, atlamak `dependsOn` fatura kaynak düğüm. 

Güncelleştirilmiş fiyat planı doğrulamak için bakmak **kullanım ve Tahmini maliyetler sayfasını** tarayıcı dikey penceresi. **Tarayıcı görünümü yenileyin** en son durumunu gördüğünüzden emin olmak için.



## <a name="add-a-metric-alert"></a>Bir ölçüm uyarısı Ekle

Uygulama kaynağınız ile aynı zamanda bir ölçüm uyarısı ayarlamak için şablon dosyasına aşağıdakine benzer bir kod birleştirme:

```JSON
{
    parameters: { ... // existing parameters ...
            "responseTime": {
              "type": "int",
              "defaultValue": 3,
              "minValue": 1,
              "metadata": {
                "description": "Enter response time threshold in seconds."
              }
    },
    variables: { ... // existing variables ...
      // Alert names must be unique within resource group.
      "responseAlertName": "[concat('ResponseTime-', toLower(parameters('appName')))]"
    }, 
    resources: { ... // existing resources ...
     {
      //
      // Metric alert on response time
      //
      "name": "[variables('responseAlertName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this resource is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('responseAlertName')]",
        "description": "response time alert",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/components', parameters('appName'))]",
            "metricName": "request.duration"
          },
          "threshold": "[parameters('responseTime')]", //seconds
          "windowSize": "PT15M" // Take action if changed state for 15 minutes
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Şablonu çağırdığınızda, bu parametre isteğe bağlı olarak ekleyebilirsiniz:

    `-responseTime 2`

Elbette, diğer alanları parametreleştirebilirsiniz. 

Tür adları ve diğer uyarı kuralları yapılandırma ayrıntılarını öğrenmek için el ile bir kural oluşturun ve içine inceleyin [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Kullanılabilirlik testi Ekle

Bu örnek, bir ping testi (tek bir sayfayı test etmek) içindir.  

**İki bölümden oluşur** bir kullanılabilirlik testi: test ve hata bildiren bir uyarı.

Uygulamayı oluşturan şablon dosyasına aşağıdaki kodu birleştirin.

```JSON
{
    parameters: { ... // existing parameters here ...
      "pingURL": { "type": "string" },
      "pingText": { "type": "string" , defaultValue: ""}
    },
    variables: { ... // existing variables here ...
      "pingTestName":"[concat('PingTest-', toLower(parameters('appName')))]",
      "pingAlertRuleName": "[concat('PingAlert-', toLower(parameters('appName')), '-', subscription().subscriptionId)]"
    },
    resources: { ... // existing resources here ...
    { //
      // Availability test: part 1 configures the test
      //
      "name": "[variables('pingTestName')]",
      "type": "Microsoft.Insights/webtests",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]",
      // Ensure this is created after the app resource:
      "dependsOn": [
        "[resourceId('Microsoft.Insights/components', parameters('appName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource"
      },
      "properties": {
        "Name": "[variables('pingTestName')]",
        "Description": "Basic ping test",
        "Enabled": true,
        "Frequency": 900, // 15 minutes
        "Timeout": 120, // 2 minutes
        "Kind": "ping", // single URL test
        "RetryEnabled": true,
        "Locations": [
          {
            "Id": "us-va-ash-azr"
          },
          {
            "Id": "emea-nl-ams-azr"
          },
          {
            "Id": "apac-jp-kaw-edge"
          }
        ],
        "Configuration": {
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExecutionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
        },
        "SyntheticMonitorId": "[variables('pingTestName')]"
      }
    },

    {
      //
      // Availability test: part 2, the alert rule
      //
      "name": "[variables('pingAlertRuleName')]",
      "type": "Microsoft.Insights/alertrules",
      "apiVersion": "2014-04-01",
      "location": "[parameters('appLocation')]", 
      "dependsOn": [
        "[resourceId('Microsoft.Insights/webtests', variables('pingTestName'))]"
      ],
      "tags": {
        "[concat('hidden-link:', resourceId('Microsoft.Insights/components', parameters('appName')))]": "Resource",
        "[concat('hidden-link:', resourceId('Microsoft.Insights/webtests', variables('pingTestName')))]": "Resource"
      },
      "properties": {
        "name": "[variables('pingAlertRuleName')]",
        "description": "alert for web test",
        "isEnabled": true,
        "condition": {
          "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.LocationThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
          "odata.type": "Microsoft.Azure.Management.Insights.Models.LocationThresholdRuleCondition",
          "dataSource": {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
            "resourceUri": "[resourceId('microsoft.insights/webtests', variables('pingTestName'))]",
            "metricName": "GSMT_AvRaW"
          },
          "windowSize": "PT15M", // Take action if changed state for 15 minutes
          "failedLocationCount": 2
        },
        "actions": [
          {
            "$type": "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
            "sendToServiceOwners": true,
            "customEmails": []
          }
        ]
      }
    }
}
```

Kodları diğer test konumları için keşfetmek için ya da daha karmaşık web testleri oluşturulmasını otomatik hale getirmek için el ile bir örnek oluşturun ve koddan sonra Parametreleştirme [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Daha fazla kaynak ekleyin

Herhangi bir türde başka bir kaynak oluşturulmasını otomatik hale getirmek için bir örnek el ile oluşturun ve daha sonra kopyalayın ve kendi koddan Parametreleştirme [Azure Resource Manager](https://resources.azure.com/). 

1. Açık [Azure Resource Manager](https://resources.azure.com/). Aşağı gezinebilirsiniz `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, uygulama kaynağınıza. 
   
    ![Azure kaynak Gezgini içinde gezinme](./media/powershell/01.png)
   
    *Bileşenleri* uygulamaları görüntülemek için temel Application Insights kaynaklarıdır. Kullanılabilirlik web testleri ve ilişkili uyarı kuralları için ayrı kaynaklar vardır.
2. Bileşenin JSON uygun yere kopyalayın `template1.json`.
3. Bu özellikler silin:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Web testleri ve alertrules bölümleri'ni açın ve tek tek öğelerin JSON şablonunuza kopyalayın. (Web testleri veya alertrules düğümünden bulundurmanıza gerek yoktur: bunları altındaki öğeleri uygulamasına gidin.)
   
    Her ikisi de kopyalamak zorunda her bir web testi ilişkili bir uyarı kuralı vardır.
   
    Ölçümler ile ilgili uyarılar de içerebilir. [Ölçüm adları](powershell-alerts.md#metric-names).
5. Bu satırı, her bir kaynak ekleyin:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Şablonu parametreleştirmek
Şimdi parametrelerle belirli adları değiştirmeniz gerekir. İçin [bir şablonu parametreleştirmek](../../azure-resource-manager/resource-group-authoring-templates.md), ifadeleri kullanarak yazdığınız bir [yardımcı işlevler kümesi](../../azure-resource-manager/resource-group-template-functions.md). 

Bir dize yalnızca bir parçasını Parametreleştirme, kullanın `concat()` dizeleri oluşturmak için.

Korunmalarını sağlamak isteyeceksiniz değişimler örnekleri aşağıda verilmiştir. Her değiştirme birkaç kez vardır. Başkalarının şablonunuzda gerekebilir. Bu örnekler, parametreler ve değişkenler tanımladığımız şablon üst kısmında kullanın.

| find | değiştirin |
| --- | --- |
| `"hidden-link:/subscriptions/.../../components/MyAppName"` |`"[concat('hidden-link:',`<br/>`resourceId('microsoft.insights/components',` <br/> `parameters('appName')))]"` |
| `"/subscriptions/.../../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (küçük) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>`Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>GUID ve Kimliği Sil |

### <a name="set-dependencies-between-the-resources"></a>Küme için kaynaklarınız arasındaki bağımlılıkları
Azure kaynakları katı bir sırayla ayarlamanız gerekir. Sonraki başlamadan önce bir kurulum tamamlandıktan emin olmak için bağımlılık satırları ekleyin:

* Kullanılabilirlik testi kaynağı:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* Bir kullanılabilirlik testi için uyarı kaynakta:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Sonraki adımlar
Diğer Otomasyon makaleler:

* [Bir Application Insights kaynağı oluşturma](powershell-script-create-resource.md) -şablon kullanarak olmadan hızlı yöntemi.
* [Uyarılar ayarlayın](powershell-alerts.md)
* [Web testleri oluşturun](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Azure Tanılama verilerini Application Insights’a gönderme](powershell-azure-diagnostics.md)
* [Azure'da Github'dan dağıtma](https://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Sürüm ek açıklamaları oluşturma](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)
