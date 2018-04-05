---
title: PowerShell ile Azure Application Insights otomatikleştirmek | Microsoft Docs
description: Bir Azure Resource Manager şablonu kullanarak PowerShell'de oluşturma kaynak, uyarı ve kullanılabilirlik testlerini otomatikleştirme.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 9f73b87f-be63-4847-88c8-368543acad8b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/02/2017
ms.author: mbullwin
ms.openlocfilehash: 46ba4ce992640e8a6d171ab839dd7cdb24e0b404
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
#  <a name="create-application-insights-resources-using-powershell"></a>PowerShell ile Application Insights kaynakları oluşturma
Bu makalede oluşturulması ve güncelleştirilmesini otomatikleştirmek gösterilmiştir [Application Insights](app-insights-overview.md) Azure kaynak yönetimi kullanarak otomatik olarak kaynakları. Örneğin, bir derleme işleminin parçası olarak bunu olabilir. Temel Application Insights kaynağı yanı sıra oluşturduğunuz [kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md), ayarlayın [uyarıları](app-insights-alerts.md)ayarlayın [düzeni fiyatlandırma](app-insights-pricing.md)ve diğer Azure kaynakları oluşturun .

Bu kaynakları oluşturmak için JSON şablonları anahtarıdır [Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md). Buna koysalar yordamdır: var olan kaynakların; JSON tanımları indirmek adları gibi belirli değerleri Parametreleştirme; ve yeni bir kaynak oluşturmak istediğinizde şablonu çalıştırın. Bazı kaynaklar birlikte paketini, bunları oluşturmak için tek - Örneğin, bir uygulama İzleyicisi kullanılabilirlik testleri, uyarılar ve sürekli dışa aktarma için depolama gidin. Burada açıklayacağız parameterizations bazıları için bazı subtleties vardır.

## <a name="one-time-setup"></a>Tek seferlik Kurulumu
Önce Azure aboneliğinizle PowerShell kullanmadıysanız:

Komut dosyalarını çalıştırmak istediğiniz Azure Powershell modülünü yükleyin:

1. Yükleme [Microsoft Web Platformu Yükleyicisi (v5 veya üzeri)](http://www.microsoft.com/web/downloads/platform.aspx).
2. Microsoft Azure PowerShell'i yüklemek için kullanın.

## <a name="create-an-azure-resource-manager-template"></a>Bir Azure Resource Manager şablonu oluşturma
Yeni bir .json dosyası oluşturma - şimdi çağrı `template1.json` Bu örnekte. Bu içerik bu dosyaya kopyalayın:

```JSON
    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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
                    "HockeyAppBridge",
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
                    "description": "1 = Basic, 2 = Enterprise"
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



## <a name="create-application-insights-resources"></a>Application Insights kaynakları oluşturun
1. PowerShell'de, Azure'da oturum açın:
   
    `Login-AzureRmAccount`
2. Bu gibi bir komutu çalıştırın:
   
    ```PS
   
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -appName myNewApp

    ``` 
   
   * `-ResourceGroupName` Yeni kaynak oluşturmak istediğiniz grubudur.
   * `-TemplateFile` Özel Parametreler önce gerçekleşmesi gerekir.
   * `-appName` Oluşturmak için kaynağın adı.

Diğer parametreler ekleyebilirsiniz - açıklamalarının Şablon Parametreler bölümünde bulabilirsiniz.

## <a name="to-get-the-instrumentation-key"></a>İzleme anahtarını almak için
Uygulama kaynağı oluşturduktan sonra izleme anahtarını isteyebilirsiniz: 

```PS
    $resource = Find-AzureRmResource -ResourceNameEquals "<YOUR APP NAME>" -ResourceType "Microsoft.Insights/components"
    $details = Get-AzureRmResource -ResourceId $resource.ResourceId
    $ikey = $details.Properties.InstrumentationKey
```


<a id="price"></a>
## <a name="set-the-price-plan"></a>Fiyat planını ayarlama

Ayarlayabileceğiniz [fiyat planı](app-insights-pricing.md).

Yukarıdaki şablonu kullanarak kurumsal fiyat planı ile bir uygulama kaynağı oluşturmak için:

```PS
        New-AzureRmResourceGroupDeployment -ResourceGroupName Fabrikam `
               -TemplateFile .\template1.json `
               -priceCode 2 `
               -appName myNewApp
```

|priceCode|plan|
|---|---|
|1|Temel|
|2|Enterprise|

* Yalnızca varsayılan temel fiyat planı kullanmak istiyorsanız, şablon CurrentBillingFeatures kaynaktan atlayabilirsiniz.
* Bileşen kaynak oluşturulduktan sonra fiyat planı değiştirmek isterseniz, "microsoft.insights/components" kaynak atlar bir şablon kullanabilirsiniz. Ayrıca, atlayın `dependsOn` fatura kaynak düğümden. 

Güncelleştirilmiş fiyat planı doğrulamak için bakmak **kullanım ve tahmini maliyetleri sayfa** dikey tarayıcı penceresinde. **Tarayıcı görünümünü yenileyin** en son durumunu gördüğünüzden emin olmak için.



## <a name="add-a-metric-alert"></a>Ölçüm uyarısı ekleme

Uygulama kaynağınız ile aynı zamanda bir ölçüm uyarısı ayarla şablon dosyasına aşağıdakine benzer bir kod Birleştir:

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

Şablon çağırdığınızda, bu parametre isteğe bağlı olarak ekleyebilirsiniz:

    `-responseTime 2`

Elbette, diğer alanlar Parametreleştirme. 

Tür adları ve diğer uyarı kurallarını yapılandırma ayrıntılarını öğrenmek için el ile bir kural oluşturmak ve bunu incelemek [Azure Resource Manager](https://resources.azure.com/). 


## <a name="add-an-availability-test"></a>Bir kullanılabilirlik testi ekleyin

Bu, bir ping sınaması (tek bir sayfayı test etmek) örneğidir.  

**İki bölümden oluşur** bir kullanılabilirlik testinde: test ve hatalar bildiren uyarı.

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
          "WebTest": "[concat('<WebTest   Name=\"', variables('pingTestName'), '\"   Enabled=\"True\"         CssProjectStructure=\"\"    CssIteration=\"\"  Timeout=\"120\"  WorkItemIds=\"\"         xmlns=\"http://microsoft.com/schemas/VisualStudio/TeamTest/2010\"         Description=\"\"  CredentialUserName=\"\"  CredentialPassword=\"\"         PreAuthenticate=\"True\"  Proxy=\"default\"  StopOnError=\"False\"         RecordedResultFile=\"\"  ResultsLocale=\"\">  <Items>  <Request Method=\"GET\"    Version=\"1.1\"  Url=\"', parameters('Url'),   '\" ThinkTime=\"0\"  Timeout=\"300\" ParseDependentRequests=\"True\"         FollowRedirects=\"True\" RecordResult=\"True\" Cache=\"False\"         ResponseTimeGoal=\"0\"  Encoding=\"utf-8\"  ExpectedHttpStatusCode=\"200\"         ExpectedResponseUrl=\"\" ReportingName=\"\" IgnoreHttpStatusCode=\"False\" />        </Items>  <ValidationRules> <ValidationRule  Classname=\"Microsoft.VisualStudio.TestTools.WebTesting.Rules.ValidationRuleFindText, Microsoft.VisualStudio.QualityTools.WebTestFramework, Version=10.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a\" DisplayName=\"Find Text\"         Description=\"Verifies the existence of the specified text in the response.\"         Level=\"High\"  ExectuionOrder=\"BeforeDependents\">  <RuleParameters>        <RuleParameter Name=\"FindText\" Value=\"',   parameters('pingText'), '\" />  <RuleParameter Name=\"IgnoreCase\" Value=\"False\" />  <RuleParameter Name=\"UseRegularExpression\" Value=\"False\" />  <RuleParameter Name=\"PassIfTextFound\" Value=\"True\" />  </RuleParameters> </ValidationRule>  </ValidationRules>  </WebTest>')]"
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

Diğer test konumları kodlarını Bul ya da daha karmaşık web testleri oluşturmayı otomatikleştirmek için bir örnek el ile oluşturup koddan Parametreleştirme [Azure Resource Manager](https://resources.azure.com/).

## <a name="add-more-resources"></a>Daha fazla kaynak ekleyin

Herhangi bir türde başka bir kaynak oluşturulmasını otomatik hale getirmek için bir örnek el ile oluşturun ve sonra kopyalayın ve onun koddan Parametreleştirme [Azure Resource Manager](https://resources.azure.com/). 

1. Açık [Azure Resource Manager](https://resources.azure.com/). Aşağı gezinmek `subscriptions/resourceGroups/<your resource group>/providers/Microsoft.Insights/components`, Uygulama kaynağı için. 
   
    ![Gezinti bölmesinde Azure kaynak Gezgini](./media/app-insights-powershell/01.png)
   
    *Bileşenleri* uygulamaları görüntülemek için temel Application Insights kaynaklardır. Kullanılabilirlik web testleri ve ilişkili uyarı kuralları için ayrı kaynak yok.
2. Bileşen JSON uygun yere kopyalayın `template1.json`.
3. Bu özellikleri silin:
   
   * `id`
   * `InstrumentationKey`
   * `CreationDate`
   * `TenantId`
4. Webtests ve alertrules bölümleri açın ve tek tek öğelerin JSON şablonunuza kopyalayın. (Webtests veya alertrules düğümünden kopyalama: bunları altındaki öğelerin uygulamasına gidin.)
   
    Her web testi ilişkili bir uyarı kuralı sahiptir, bu nedenle bunların her ikisi de kopyalamanız gerekir.
   
    Ölçümleri uyarılar de içerir. [Ölçüm adları](app-insights-powershell-alerts.md#metric-names).
5. Her bir kaynağın bu satırı ekleyin:
   
    `"apiVersion": "2015-05-01",`

### <a name="parameterize-the-template"></a>Şablon Parametreleştirme
Şimdi parametrelerle belirli adlarını değiştirmek zorunda. İçin [şablon Parametreleştirme](../azure-resource-manager/resource-group-authoring-templates.md), kullanarak ifadeler yazma bir [yardımcı işlevleri kümesi](../azure-resource-manager/resource-group-template-functions.md). 

Yalnızca bir dize parçası Parametreleştirme, kullanın `concat()` dizeleri oluşturmak için.

Olmak istersiniz değişimler örnekleri aşağıda verilmiştir. Her değiştirme birkaç kez vardır. Başkalarının şablonunuzda gerekebilir. Bu örnekler, şablon üst kısmındaki parametreler ve değişkenler tanımladığımız kullanın.

| Bul | değiştirin |
| --- | --- |
| `"hidden-link:/subscriptions/.../components/MyAppName"` |`"[concat('hidden-link:',`<br/>` resourceId('microsoft.insights/components',` <br/> ` parameters('appName')))]"` |
| `"/subscriptions/.../alertrules/myAlertName-myAppName-subsId",` |`"[resourceId('Microsoft.Insights/alertrules', variables('alertRuleName'))]",` |
| `"/subscriptions/.../webtests/myTestName-myAppName",` |`"[resourceId('Microsoft.Insights/webtests', parameters('webTestName'))]",` |
| `"myWebTest-myAppName"` |`"[variables(testName)]"'` |
| `"myTestName-myAppName-subsId"` |`"[variables('alertRuleName')]"` |
| `"myAppName"` |`"[parameters('appName')]"` |
| `"myappname"` (küçük harf) |`"[toLower(parameters('appName'))]"` |
| `"<WebTest Name=\"myWebTest\" ...`<br/>` Url=\"http://fabrikam.com/home\" ...>"` |`[concat('<WebTest Name=\"',` <br/> `parameters('webTestName'),` <br/> `'\" ... Url=\"', parameters('Url'),` <br/> `'\"...>')]"`<br/>Delete GUID ve kimliği. |

### <a name="set-dependencies-between-the-resources"></a>Kaynakları arasındaki bağımlılıkların ayarlama
Azure kaynakları katı sırayla ayarlamanız gerekir. Sonraki başlamadan önce bir kurulum tamamlandıktan emin olmak için bağımlılık satırları ekleyin:

* Kullanılabilirlik test kaynak:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/components', parameters('appName'))]"],`
* Uyarı kaynak kullanılabilirliği testi için:
  
    `"dependsOn": ["[resourceId('Microsoft.Insights/webtests', variables('testName'))]"],`



## <a name="next-steps"></a>Sonraki adımlar
Diğer Otomasyon makaleler:

* [Application Insights kaynağı oluşturma](app-insights-powershell-script-create-resource.md) -Şablon kullanmadan hızlı yöntem.
* [Uyarıları ayarlayın](app-insights-powershell-alerts.md)
* [Web testleri oluşturma](https://azure.microsoft.com/blog/creating-a-web-test-alert-programmatically-with-application-insights/)
* [Azure Tanılama verilerini Application Insights’a gönderme](app-insights-powershell-azure-diagnostics.md)
* [Github'dan Azure'a dağıtma](http://blogs.msdn.com/b/webdev/archive/2015/09/16/deploy-to-azure-from-github-with-application-insights.aspx)
* [Yayın ek açıklamaları oluşturma](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/API/CreateReleaseAnnotation.ps1)

