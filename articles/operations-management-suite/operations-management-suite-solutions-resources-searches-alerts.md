---
title: "OMS çözümlerinde Kaydedilmiş aramaları ve Uyarıları | Microsoft Docs"
description: "OMS çözümlerinde genellikle Kaydedilmiş aramaları çözümü tarafından toplanan verileri çözümlemek için günlük analizi dahil edin.  Ayrıca kullanıcıya bildirmek için uyarılar tanımlayın olabilir veya otomatik olarak yanıt kritik bir sorun için adımları uygulayın.  Bu makalede, günlük yönetim çözümlerine dahil şekilde bir Resource Manager şablonunda kaydedilmiş aramaları ve Uyarıları analizi tanımlamak açıklar."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/16/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b2388626dd68ea1911cdfb3d6a84e70f6bf3cc6
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-oms-management-solution-preview"></a>Günlük analizi ekleme arar ve Uyarıları kaydedilmiş OMS yönetim çözümü (Önizleme)

> [!NOTE]
> Bu, şu anda önizlemede OMS yönetim çözümleri oluşturmak için başlangıç belgesidir. Aşağıda açıklanan herhangi bir şema değiştirilebilir ' dir.   


[OMS yönetim çözümlerine](operations-management-suite-solutions.md) genellikle içerecektir [kayıtlı aramalar](../log-analytics/log-analytics-log-searches.md) çözümü tarafından toplanan verileri çözümlemek için günlük analizi içinde.  Ayrıca tanımlayabilir [uyarıları](../log-analytics/log-analytics-alerts.md) kullanıcıya bildir veya otomatik olarak yanıt kritik bir sorun için adımları uygulayın.  Bu makalede nasıl günlük kayıtlı aramalar analizi tanımlayacağınızı açıklar ve Uyarıları gelen bir [kaynak yönetimi şablonu](../resource-manager-template-walkthrough.md) içinde eklenebilir şekilde [yönetim çözümleri](operations-management-suite-solutions-creating.md).

> [!NOTE]
> Bu makaledeki örnekler parametreleri ve gerekli olduğunu veya yönetim çözümleri için ortak olduğunu ve açıklanan değişkenleri kullanma [Operations Management Suite (OMS) yönetimi çözümleri oluşturma](operations-management-suite-solutions-creating.md)  

## <a name="prerequisites"></a>Ön koşullar
Bu makale, zaten nasıl hakkında bilgi sahibi olduğunuzu varsayar [bir yönetim çözümü oluşturma](operations-management-suite-solutions-creating.md) ve yapısı bir [Resource Manager şablonu](../resource-group-authoring-templates.md) ve çözüm dosya.


## <a name="log-analytics-workspace"></a>Günlük analizi çalışma alanı
Günlük analizi tüm kaynaklarında bulunan bir [çalışma](../log-analytics/log-analytics-manage-access.md).  Bölümünde açıklandığı gibi [OMS çalışma ve Automation hesabı](operations-management-suite-solutions.md#oms-workspace-and-automation-account), çalışma alanı yönetimi çözümünde dahil değildir ancak çözüm yüklenmeden önce mevcut olması gerekir.  Kullanılabilir değilse, çözüm yükleme başarısız olur.

Her günlük analizi kaynak adına çalışma adıdır.  Bu çözümle yapılır **çalışma** savedsearch kaynak aşağıdaki örnekteki gibi parametre.

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"

## <a name="log-analytics-api-version"></a>Günlük analizi API sürümü
Resource Manager şablonunda tanımlanan tüm günlük analizi kaynaklarını özelliğine sahip **apiVersion** kaynak kullanması gereken API sürümü tanımlar.  Bu kullanan kaynaklar için farklı bir sürümdür [eski ve yükseltilmiş sorgu dili](../log-analytics/log-analytics-log-search-upgrade.md).  

 Aşağıdaki tabloda, eski ve yükseltilmiş çalışma alanlarını ve her biri için farklı bir sözdizimi belirtmek için bir örnek sorgu için günlük analizi API sürümleri belirtir. 

| Çalışma alanında sürümü | API sürümü | Örnek sorgu |
|:---|:---|:---|
| V1 (eski)   | 2015-11-01-Önizleme | Tür olay EventLevelName = hata =             |
| v2 (yükseltme) | 2017-03-15-Önizleme | Olay &#124; Burada EventLevelName "Error" ==  |

Çalışma alanları farklı sürümleri tarafından desteklenir aşağıdakileri göz önünde bulundurun.

- Eski sorgu dili kullanmanızı şablonları yükseltilmiş veya eski bir çalışma alanında yüklenebilir.  Kullanıcı tarafından çalıştırdığınızda yükseltilen bir çalışma alanında yüklediyseniz, ardından sorguları yeni dil için kolay bir şekilde dönüştürülür.
- Yükseltilen sorgu dili kullanmanızı şablonları yükseltilmiş bir çalışma alanında yalnızca yüklenebilir.


## <a name="saved-searches"></a>Kaydedilen aramalar
Dahil [kayıtlı aramalar](../log-analytics/log-analytics-log-searches.md) çözümünüz tarafından toplanan sorgu veri kullanıcılarına izin vermek için bir çözüm içinde.  Kaydedilmiş aramaları altında görünen **Sık Kullanılanlar** OMS portalında ve **kayıtlı aramaları** Azure portalında.  Kaydedilmiş bir aramayı de her uyarı için gereklidir.   

[Günlük analizi kaydedilen arama](../log-analytics/log-analytics-log-searches.md) kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches` ve aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



Kaydedilmiş bir aramayı, her bir özellik aşağıdaki tabloda açıklanmıştır. 

| Özellik | Açıklama |
|:--- |:--- |
| category | Kayıtlı arama kategorisi.  Konsolunda birlikte gruplanır şekilde aynı çözümüne kaydedilmiş yapılan aramalar genellikle tek bir kategori paylaşır. |
| görünen adı | Portalı'nda kayıtlı arama için görüntülenecek adı. |
| sorgu | Çalıştırılacak sorgu. |

> [!NOTE]
> JSON olarak yorumlanabilecek karakterler içeriyorsa, sorguda kaçış karakterleri kullanmanız gerekebilir.  Örneğin, sorgu, **türü: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, çözüm dosyasındaki yazılmalıdır **türü: AzureActivity işlemadı:\" Microsoft.Compute/virtualMachines/write\"**.

## <a name="alerts"></a>Uyarılar
[Analytics uyarıları oturum](../log-analytics/log-analytics-alerts.md) düzenli aralıklarla kaydedilmiş bir aramayı çalıştırma uyarı kuralları tarafından oluşturulur.  Sorgu eşleşmesinin sonuçlarını ölçütleri belirtilmişse, bir uyarı kaydı oluşturulur ve bir veya daha fazla eylem çalıştırın.  

Uyarı kurallarında bir yönetim çözümü, aşağıdaki üç farklı kaynakları yapılır.

- **Kayıtlı arama.**  Çalıştırılan günlük arama tanımlar.  Birden çok uyarı kurallarını, tek bir kayıtlı arama paylaşabilirsiniz.
- **Zamanlama.**  Günlük arama ne sıklıkta çalıştırmak tanımlar.  Her uyarı kuralı bir ve yalnızca bir zamanlaması vardır.
- **Uyarı eylem.**  Her uyarı kuralı bir eylem kaynak türüne sahip olan **uyarı** bir uyarı kaydı oluşturulduğunda ve uyarının önem derecesi ölçütlerini gibi uyarı ayrıntılarını tanımlar.  Eylem kaynak isteğe bağlı olarak bir posta ve runbook yanıt tanımlayacaksınız.
- **Web kancası eylem (isteğe bağlı).**  Uyarı kuralı bir Web kancası çağırır sonra başka bir işlem kaynak türüne sahip gerektirir **Web kancası**.    

Kaynaklar, yukarıda açıklanan arama kaydedildi.  Diğer kaynaklar aşağıda açıklanmıştır.


### <a name="schedule-resource"></a>Zamanlama kaynak

Kaydedilmiş bir aramayı her zamanlamayı ayrı bir uyarı kuralı temsil eden ile bir veya daha fazla zamanlama olabilir. Ne sıklıkta arama çalıştırma ve verilerin alınacağı zaman aralığı olan zamanlamayı tanımlar.  Zamanlama kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` ve aşağıdaki yapı ayarlanmıştır. Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



Zamanlama kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Etkin       | Evet | Oluşturulduğunda uyarının etkin olup olmadığını belirtir. |
| interval      | Evet | Ne sıklıkta sorgu dakika içinde çalışır. |
| QueryTimeSpan | Evet | Sonuçları değerlendirileceği üzerinden dakika cinsinden süre uzunluğu. |

Zamanlama önce oluşturulan zamanlama kaynak kayıtlı arama üzerinde bağımlı olmalıdır.


### <a name="actions"></a>Eylemler
Belirtilen eylem kaynak iki tür vardır **türü** özelliği.  Bir zamanlama gerektiren **uyarı** uyarı kuralı ve bir uyarı oluşturulduğunda, hangi eylemleri alınır ayrıntılarını tanımlayan eylem.  Ayrıca içerebilir bir **Web kancası** eylem bir Web kancası uyarıdan çağrılmalıdır.  

Eylem kaynaklarınız türü `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.  

#### <a name="alert-actions"></a>Uyarı eylemleri

Her zamanlama varsa **uyarı** eylem.  Bu, uyarı ve isteğe bağlı olarak bildirim ve düzeltme eylemlerinin ayrıntılarını tanımlar.  Bir bildirim bir veya daha fazla adrese bir e-posta gönderir.  Bir düzeltme algılanan sorunu düzeltmek girişiminde Azure Otomasyonu'nda bir runbook başlatır.

Uyarı eylemleri aşağıdaki yapı ayarlanmıştır.  Kopyalayabilir ve çözüm dosyanıza Bu kod parçacığını yapıştırın ve parametre adlarını değiştirmek için bu ortak değişkenleri ve parametreleri içerir. 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

Uyarı eylemi kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Tür | Evet | Eylem türü.  Bu **uyarı** uyarı eylemleri için. |
| Ad | Evet | Uyarı görünen adı.  Bu uyarı kuralı için konsolunda görüntülenen addır. |
| Açıklama | Hayır | Uyarı isteğe bağlı bir açıklama. |
| Önem Derecesi | Evet | Aşağıdaki değerlerden uyarı kaydının önem derecesi:<br><br> **Kritik**<br>**Uyarı**<br>**Bilgilendirme** |


##### <a name="threshold"></a>Eşik
Bu bölüm gereklidir.  Uyarı eşiği özelliklerini tanımlar.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| işleci | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt büyük =<br>lt = küçüktür** |
| Değer | Evet | Sonuçları karşılaştırmak için değer. |


##### <a name="metricstrigger"></a>MetricsTrigger
Bu bölümde isteğe bağlıdır.  Bir ölçüm ölçüm uyarı içerir.

> [!NOTE]
> Ölçüm ölçüm uyarılar şu anda genel önizlemede. 

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| TriggerCondition | Evet | Eşik ihlallerini veya aşağıdaki değerlerden ardışık ihlallerini toplam sayısını olup olmadığını belirtir:<br><br>**Toplam<br>ardışık** |
| işleci | Evet | Aşağıdaki değerlerden karşılaştırma işleci:<br><br>**gt büyük =<br>lt = küçüktür** |
| Değer | Evet | Sayısı uyarıyı tetikleyecek ölçütler karşılanması gerekir. |

##### <a name="throttling"></a>Azaltma
Bu bölümde isteğe bağlıdır.  Bazı süreyi bir uyarı oluşturulduktan sonra için aynı kural uyarıları gizlemek istiyorsanız, bu bölüm içerir.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Dakika Cinsiden Süre | Dahil edilen öğesi azaltma, Evet | Aynı uyarı kuralı birinden oluşturulduktan sonra uyarıları gizlemek için dakika sayısı. |

##### <a name="emailnotification"></a>EmailNotification
 Bu bölümde isteğe bağlı bir veya daha fazla alıcıya posta göndermek için uyarı istiyorsanız ekleyin.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| Alıcıları | Evet | Bir uyarı aşağıdaki örnekte olduğu gibi oluşturulduğunda, bildirim göndermek için e-posta adreslerini virgülle ayrılmış listesi.<br><br>**[ "recipient1@contoso.com", "recipient2@contoso.com" ]** |
| Konu | Evet | Posta konu satırı. |
| Eki | Hayır | Ekleri şu anda desteklenmemektedir.  Bu öğe dahil ise olmalıdır **hiçbiri**. |


##### <a name="remediation"></a>Düzeltme
Bu bölümde isteğe yanıt olarak uyarı başlatmak üzere bir runbook'u istiyorsanız ekleyin. |

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| RunbookName | Evet | Başlatmak için runbook'un adı. |
| WebhookUri | Evet | Runbook için Web kancası URI'si. |
| Süre sonu | Hayır | Tarih ve saat düzeltme süresi dolar. |

#### <a name="webhook-actions"></a>Web kancası eylemleri

Web kancası eylemleri, bir URL çağırma ve isteğe bağlı olarak gönderilecek bir yükü sağlayarak bir işlem başlatın. Azure Otomasyon çalışma kitabı dışındaki işlemler çağırabilir Web kancası için amacı dışında düzeltme eylemleri benzerdir. Uzak işlem teslim edilecek bir yükü sağlama ek seçeneği de sağlar.

Uyarınız bir Web kancası çağırır sonra bir eylem kaynak türüne sahip gerekir **Web kancası** ek olarak **uyarı** eylem kaynak.  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

Web kancası eylem kaynaklar için özellikler aşağıdaki tabloda açıklanmıştır.

| Öğe adı | Gerekli | Açıklama |
|:--|:--|:--|
| type | Evet | Eylem türü.  Bu **Web kancası** Web kancası eylemleri için. |
| ad | Evet | Eylem görünen adı.  Bu konsolunda görüntülenmez. |
| wehookUri | Evet | Web kancası için URI. |
| CustomPayload | Hayır | Web kancası için gönderilecek özel yükü. Web kancası bekleniyor üzerinde biçimi bağlıdır. |




## <a name="sample"></a>Örnek

Aşağıdaki kaynakları içermektedir içeren bir çözüm örneği aşağıda verilmiştir:

- Kayıtlı arama
- Zamanlama
- Uyarı eylemi
- Web kancası eylemi

Örnek kullanır [standart çözüm parametreleri](operations-management-suite-solutions-solution-file.md#parameters) cmdlet'e kod değerleri kaynak tanımlarında aksine bir çözümde yaygın olarak kullanılacak değişkenleri.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for the email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


Aşağıdaki parametre dosyası bu çözüm için örnek değerleri sağlar.

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a>Sonraki adımlar
* [Görünümler ekleme](operations-management-suite-solutions-resources-views.md) Yönetimi çözümünüz için.
* [Otomasyon runbook'ları ve diğer kaynakları eklemek](operations-management-suite-solutions-resources-automation.md) Yönetimi çözümünüz için.

