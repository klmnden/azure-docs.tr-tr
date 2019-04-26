---
title: Azure İzleyicisi'ni kullanarak veri fabrikalarını izleme | Microsoft Docs
description: Azure Data Factory bilgileriyle tanılama günlükleri'ni etkinleştirerek Data Factory işlem hatlarını izlemek için Azure İzleyici'ı kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: shlo
ms.openlocfilehash: e96e462709ab0c715c831bd10c628869d5c617fe
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60319334"
---
# <a name="alert-and-monitor-data-factories-using-azure-monitor"></a>Azure İzleyicisi'ni kullanarak veri fabrikalarını izleme ve uyarı
Bulut uygulamaları ile birçok hareketli parçadan karmaşıktır. İzleme, uygulama güncel kalıp emin olmak için veri ve sağlam bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur. Ayrıca, uygulamanızı daha ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Bu bilgi bakım ya da uygulama performansı artırmak için yardımcı veya aksi halde el ile müdahale gerektiren eylemleri otomatikleştirme.

Azure İzleyici, çoğu Microsoft Azure Hizmetleri için temel düzeyde altyapı ölçüm ve günlükleri sağlar. Ayrıntılar için bkz [izlemeye genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Azure tanılama günlükleri, kaynak işlemiyle ilgili zengin, sık sık veri sağlayan bir kaynak tarafından gösterilen günlüklerdir. Data Factory, Azure İzleyici'de tanılama günlükleri çıkarır.

## <a name="persist-data-factory-data"></a>Veri Fabrikası verileri kalıcı hale
Veri Fabrikası işlem hattı 45 gün boyunca veri çalıştırması yalnızca depolar. Azure İzleyicisi'ni kullanarak işlem hattı 45 gün boyunca veri çalıştırması kalıcı hale getirmek istiyorsanız, analiz için tanılama günlükleri yalnızca Yönlendirme yapamazsınız, seçtiğiniz süre için Fabrika bilgi alacak şekilde, bunları bir depolama hesabına kalıcı hale getirebilirsiniz.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

* Kaydetmek için bir **depolama hesabı** denetim veya el ile İnceleme. Tanılama ayarlarını kullanarak elde tutma süresi (gün cinsinden) belirtebilirsiniz.
* Bunları Stream **Event Hubs** alımı bir üçüncü taraf hizmeti veya Power BI gibi özel bir analiz çözümü için.
* Bunları analiz **Log Analytics**

Günlükleri yayan kaynak ile aynı abonelikte değil bir depolama hesabına veya olay hub'ı ad alanını kullanabilirsiniz. Ayarı yapılandıran kullanıcının her iki aboneliğin uygun rol tabanlı erişim denetimi (RBAC) erişimi olması gerekir.

## <a name="set-up-diagnostic-logs"></a>Tanılama günlükleri ayarlama

### <a name="diagnostic-settings"></a>Tanılama ayarları
Olmayan işlem kaynakları için tanılama günlükleri, tanılama ayarları kullanılarak yapılandırılır. Kaynak denetimi için tanılama ayarları:

* Burada tanılama günlüklerini (depolama hesabı, Event Hubs veya Azure İzleyici günlüklerine) gönderilir.
* Hangi günlük kategorileri gönderilir.
* Günlük kategorileri bir depolama hesabında ne kadar süre tutulacağını.
* Bekletme günü sayısının sıfır günlükler süresiz olarak tutulur anlamına gelir. Aksi takdirde, değeri herhangi bir sayıda gün 1 ile 2147483647 arasında olabilir.
* Bekletme ilkeleri ayarlayın ancak günlükleri bir depolama hesabında Depolama'nın (örneğin, yalnızca Event Hubs veya Azure İzleyici günlükleri seçenek de belirlenmiştir) devre dışı bırakıldı, bekletme ilkeleri bir etkisi yoktur.
* Bekletme ilkeleri uygulanan günlük, olduğundan, bir günün (UTC), şu anda sonra saklama günü günlüklerinden sonunda İlkesi silindi. Örneğin, bir günlük bir bekletme ilkesi olsaydı, bugün günün başında dünden önceki gün kayıtları silinir.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>REST API'leri aracılığıyla tanılama günlüklerini etkinleştirme

Azure İzleyici REST API tanılama ayarında güncelle

**İstek**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst Bilgiler**
* `{api-version}` yerine `2016-09-01` yazın.
* Değiştirin `{resource-id}` kendisi için istediğiniz tanılama ayarlarını düzenlemek kaynak kaynak kimliği. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/manage-resource-groups-portal.md).
* Ayarlama `Content-Type` başlığına `application/json`.
* Yetkilendirme üst bilgisi, Azure Active Directory'den al bir JSON web belirteci ayarlayın. Daha fazla bilgi için [isteklerinde kimlik doğrulama](../active-directory/develop/authentication-scenarios.md).

**Gövde**
```json
{
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>/providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "metrics": [
        ],
        "logs": [
                {
                    "category": "PipelineRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "TriggerRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                },
                {
                    "category": "ActivityRuns",
                    "enabled": true,
                    "retentionPolicy": {
                        "enabled": false,
                        "days": 0
                    }
                }
            ]
    },
    "location": ""
}
```

| Özellik | Tür | Açıklama |
| --- | --- | --- |
| storageAccountId |String | Tanılama günlüklerini göndermek istediğiniz depolama hesabı kaynak kimliği |
| serviceBusRuleId |String | Event Hubs akış tanılama günlükleri için oluşturulmuş olmasını istediğiniz service bus ad alanı service bus kural kimliği. Kimliği biçimi kural: "{hizmet veri yolu kaynak kimliği} /authorizationrules/ {anahtar name}".|
| Çalışma alanı kimliği | Karmaşık Tür | Ölçüm zaman grains ve bekletme ilkelerini dizisi. Şu anda bu özellik boştur. |
|metrics| Çağrılan işlem hattına geçirilecek parametre değerleri işlem hattının çalıştırma| Bağımsız değişken değerleri parametre adlarını eşleme bir JSON nesnesi |
| günlükler| Karmaşık Tür| Bir kaynak türü için bir tanılama günlüğü kategori adı. Bir kaynak için tanılama günlük kategorileri listesi elde etmek için ilk tanılama ayarlarını alma işlemi gerçekleştirin. |
| category| String| Günlük kategorileri ve bunların bekletme ilkeleri |
| timeGrain | String | ISO 8601 süre biçiminde yakalanan ölçüm ayrıntı düzeyi. PT1M (bir dakika) olmalıdır|
| enabled| Boolean | Bu kaynak için ölçüm veya günlük kategori koleksiyonu etkin olup olmadığını belirtir|
| retentionPolicy| Karmaşık Tür| Ölçüm veya günlük kategorisi için bekletme ilkesi açıklar. Yalnızca depolama hesabı seçeneği kullanılır.|
| gün| Int| Günlükleri ve ölçümleri saklanacağı gün sayısı. 0 değeri, günlükler süresiz olarak korur. Yalnızca depolama hesabı seçeneği kullanılır. |

**Yanıt**

200 TAMAM


```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.Storage/storageAccounts/<storageAccountName>",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.EventHub/namespaces/<eventHubName>/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/<resourceGroupName>//providers/Microsoft.OperationalInsights/workspaces/<LogAnalyticsName>",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```

Azure İzleyici REST API'SİNDE tanılama ayarı hakkında bilgi edinin

**İstek**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst Bilgiler**
* `{api-version}` yerine `2016-09-01` yazın.
* Değiştirin `{resource-id}` kendisi için istediğiniz tanılama ayarlarını düzenlemek kaynak kaynak kimliği. Daha fazla bilgi için kaynak kullanarak Azure kaynaklarınızı yönetmek için gruplar.
* Ayarlama `Content-Type` başlığına `application/json`.
* Yetkilendirme üst bilgisi bir JSON Web Azure Active Directory'den edindiğiniz belirteci ayarlayın. Daha fazla bilgi için bkz: kimlik doğrulama istekleri.

**Yanıt**

200 TAMAM

```json
{
    "id": "/subscriptions/<subID>/resourcegroups/adf/providers/microsoft.datafactory/factories/shloadobetest2/providers/microsoft.insights/diagnosticSettings/service",
    "type": null,
    "name": "service",
    "location": null,
    "kind": null,
    "tags": null,
    "properties": {
        "storageAccountId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.Storage/storageAccounts/azmonlogs",
        "serviceBusRuleId": "/subscriptions/<subID>/resourceGroups/shloprivate/providers/Microsoft.EventHub/namespaces/shloeventhub/authorizationrules/RootManageSharedAccessKey",
        "workspaceId": "/subscriptions/<subID>/resourceGroups/ADF/providers/Microsoft.OperationalInsights/workspaces/mihaipie",
        "eventHubAuthorizationRuleId": null,
        "eventHubName": null,
        "metrics": [],
        "logs": [
            {
                "category": "PipelineRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "TriggerRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            },
            {
                "category": "ActivityRuns",
                "enabled": true,
                "retentionPolicy": {
                    "enabled": false,
                    "days": 0
                }
            }
        ]
    },
    "identity": null
}
```
[Daha fazla bilgi](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)

## <a name="schema-of-logs--events"></a>Şema günlükleri & olayları

### <a name="activity-run-logs-attributes"></a>Etkinlik çalıştırması öznitelikleri günlüğe kaydeder.

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "activityRunId":"",
   "pipelineRunId":"",
   "resourceId":"",
   "category":"ActivityRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "activityName":"",
   "start":"",
   "end":"",
   "properties":
       {
          "Input": "{
              "source": {
                "type": "BlobSource"
              },
              "sink": {
                "type": "BlobSink"
              }
           }",
          "Output": "{"dataRead":121,"dataWritten":121,"copyDuration":5,
               "throughput":0.0236328132,"errors":[]}",
          "Error": "{
              "errorCode": "null",
              "message": "null",
              "failureType": "null",
              "target": "CopyBlobtoBlob"
        }
    }
}
```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |String | Tanılama günlüklerini düzeyi. 4. düzey, her zaman etkinlik günlükleri çalıştırma için durum geçerlidir. | `4`  |
| correlationId |String | Bir belirli bir istek için uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | İçinde timespan, UTC biçiminde etkinliğin saati `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| String| Etkinlik çalıştırma kimliği | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| String| İşlem hattı çalıştırma kimliği | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Data factory kaynak için ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Tanılama günlükleri kategorisi. "ActivityRuns" için bu özelliği ayarlayın | `ActivityRuns` |
|düzey| String | Tanılama günlüklerini düzeyi. "Bilgilendirici" için bu özelliği ayarlayın | `Informational` |
|operationName| String |Etkinlik durumu adı. Durum başlangıç sinyal içindeyse `MyActivity -`. Son sinyal durumundaysa olduğu `MyActivity - Succeeded` son durumuna sahip | `MyActivity - Succeeded` |
|pipelineName| String | İşlem hattı adı | `MyPipeline` |
|activityName| String | Etkinliğin adı | `MyActivity` |
|start| String | Çalıştırma timespan, UTC biçiminde Etkinliğin başlangıç | `2017-06-26T20:55:29.5007959Z`|
|end| String | Timespan, UTC biçiminde etkinliğin sona erer çalıştırın. Etkinlik değil sona erdiyse henüz (bir etkinlik başlatmak için tanılama günlüğü), varsayılan değeri `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |

### <a name="pipeline-run-logs-attributes"></a>İşlem hattı çalıştırmasını öznitelikleri günlüğe kaydeder.

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "runId":"",
   "resourceId":"",
   "category":"PipelineRuns",
   "level":"Informational",
   "operationName":"",
   "pipelineName":"",
   "start":"",
   "end":"",
   "status":"",
   "properties":
    {
      "Parameters": {
        "<parameter1Name>": "<parameter1Value>"
      },
      "SystemParameters": {
        "ExecutionStart": "",
        "TriggerId": "",
        "SubscriptionId": ""
      }
    }
}
```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |String | Tanılama günlüklerini düzeyi. 4. düzey etkinlik günlükleri çalıştırma için geçerlidir. | `4`  |
| correlationId |String | Bir belirli bir istek için uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | İçinde timespan, UTC biçiminde etkinliğin saati `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|runId| String| İşlem hattı çalıştırma kimliği | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| String | Data factory kaynak için ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Tanılama günlükleri kategorisi. "PipelineRuns" için bu özelliği ayarlayın | `PipelineRuns` |
|düzey| String | Tanılama günlüklerini düzeyi. "Bilgilendirici" için bu özelliği ayarlayın | `Informational` |
|operationName| String |Durum ile işlem hattı adı. "İşlem hattı başarılı oldu -" ile işlem hattı çalıştırması tamamlandığında, son durum| `MyPipeline - Succeeded` |
|pipelineName| String | İşlem hattı adı | `MyPipeline` |
|start| String | Çalıştırma timespan, UTC biçiminde Etkinliğin başlangıç | `2017-06-26T20:55:29.5007959Z`|
|end| String | Timespan, UTC biçiminde Etkinliğin bitiş çalıştırır. Etkinlik değil sona erdiyse henüz (bir etkinlik başlatmak için tanılama günlüğü), varsayılan değeri `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |
|durum| String | (Başarılı veya başarısız) işlem hattı'nın son durumu | `Succeeded`|

### <a name="trigger-run-logs-attributes"></a>Tetikleyici çalıştırması öznitelikleri günlüğe kaydeder.

```json
{
   "Level": "",
   "correlationId":"",
   "time":"",
   "triggerId":"",
   "resourceId":"",
   "category":"TriggerRuns",
   "level":"Informational",
   "operationName":"",
   "triggerName":"",
   "triggerType":"",
   "triggerEvent":"",
   "start":"",
   "status":"",
   "properties":
   {
      "Parameters": {
        "TriggerTime": "",
       "ScheduleTime": ""
      },
      "SystemParameters": {}
    }
}

```

| Özellik | Tür | Açıklama | Örnek |
| --- | --- | --- | --- |
| Düzey |String | Tanılama günlüklerini düzeyi. Etkinlik günlükleri çalıştırma için 4 düzeyini ayarlayın. | `4`  |
| correlationId |String | Bir belirli bir istek için uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | String | İçinde timespan, UTC biçiminde etkinliğin saati `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|Tetikleyici No| String| Tetikleyici çalıştırması kimliği | `08587023010602533858661257311` |
|resourceId| String | Data factory kaynak için ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| String | Tanılama günlükleri kategorisi. "PipelineRuns" için bu özelliği ayarlayın | `PipelineRuns` |
|düzey| String | Tanılama günlüklerini düzeyi. "Bilgilendirici" için bu özelliği ayarlayın | `Informational` |
|operationName| String |Son durum ile tetikleyicinin adı olup başarıyla tetiklendi. "MyTrigger - başarılı oldu" sinyal başarılı olduysa| `MyTrigger - Succeeded` |
|triggerName| String | Tetikleyicinin adı | `MyTrigger` |
|triggerType| String | (El ile tetikleyici veya zamanlama tetikleyicisi) tetikleyici türü | `ScheduleTrigger` |
|triggerEvent| String | Olay tetikleyicisi | `ScheduleTime - 2017-07-06T01:50:25Z` |
|start| String | Tetikleyici ateşleyicisine timespan, UTC biçiminde içinde başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|durum| String | Olup tetikleyici başarıyla (başarılı veya başarısız) harekete son durumu | `Succeeded`|

## <a name="metrics"></a>Ölçümler

Azure İzleyici, performans ve sistem durumunu azure'da iş yüklerinizi görünürlük elde etmek için telemetri kullanmasına olanak tanır. Azure telemetri verilerinin en önemli ölçümleri (performans sayaçları olarak da bilinir) çoğu Azure kaynakları tarafından gösterilen türüdür. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanabilmeniz için birçok yol sağlar.

Aşağıdaki ölçümler ADFV2 yayar

| **Ölçüm**           | **Ölçüm görünen adı**         | **Birim** | **Toplama türü** | **Açıklama**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRun | İşlem hattı çalıştırmaları ölçümleri başarılı oldu | Sayı    | Toplam                | Toplam işlem hatlarını çalıştırır başarılı bir dakikalık bir pencere içinde |
| PipelineFailedRuns   | İşlem hattı çalıştırmaları ölçümleri başarısız oldu    | Sayı    | Toplam                | Toplam işlem hatlarını çalıştırır başarısız bir dakikalık bir pencere içinde    |
| ActivitySucceededRuns | Etkinlik çalıştırmaları ölçümlerinin başarılı oldu | Sayı    | Toplam                | Toplam etkinlik, bir dakikalık bir pencere içinde başarılı çalıştırır  |
| ActivityFailedRuns   | Etkinlik çalıştırmaları ölçümlerinin başarısız oldu    | Sayı    | Toplam                | Toplam etkinlik içinde bir dakika aralığında başarısız çalıştırmaları     |
| TriggerSucceededRuns | Tetikleyici çalıştırmaları ölçümleri başarılı oldu  | Sayı    | Toplam                | Bir dakika aralığında içinde toplam tetikleyici başarılı çalışır   |
| TriggerFailedRuns    | Tetikleyici çalıştırmaları ölçümleri başarısız oldu     | Sayı    | Toplam                | Toplam tetikleyici içinde bir dakika aralığında başarısız çalıştırmaları      |

Ölçümlere erişmek için makaledeki yönergeleri izleyin- https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics

## <a name="monitor-data-factory-metrics-with-azure-monitor"></a>Azure İzleyici ile veri fabrikası ölçümlerini izleme

Azure İzleyici ile Azure Data Factory tümleştirme, Azure İzleyici verileri yönlendirmek için kullanabilirsiniz. Bu tümleştirme, aşağıdaki senaryolarda kullanışlıdır:

1.  Azure İzleyici Data Factory'ye yayımlanır ölçümleri zengin bir dizi karmaşık sorgular yazmak istediğiniz. Ayrıca, Azure İzleyici aracılığıyla bu sorguları özel uyarılar oluşturabilirsiniz.

2.  Veri fabrikaları arasında izlemek istediğiniz. Veri için tek bir Azure İzleyici çalışma birden çok veri fabrikaları yönlendirebilirsiniz.

Yedi dakikalık bir giriş ve bu özelliği için şu videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Monitor-Data-Factory-pipelines-using-Operations-Management-Suite-OMS/player]

### <a name="configure-diagnostic-settings-and-workspace"></a>Tanılama ayarları ve çalışma alanını yapılandırma

Veri fabrikanızın tanılama ayarlarını etkinleştirme.

1.  Seçin **Azure İzleyici** -> **tanılama ayarları** -> data factory tanılamayı Aç -> seçin.

    ![monitor-oms-image1.png](media/data-factory-monitor-oms/monitor-oms-image1.png)

2.  Çalışma alanının yapılandırılması dahil olmak üzere tanılama ayarları sağlar.

    ![İzleyici oms image2.png](media/data-factory-monitor-oms/monitor-oms-image2.png)

### <a name="install-azure-data-factory-analytics-from-azure-marketplace"></a>Azure Data Factory Analytics'i Azure Market'ten yüklemeniz

![monitor-oms-image3.png](media/data-factory-monitor-oms/monitor-oms-image3.png)

![monitor-oms-image4.png](media/data-factory-monitor-oms/monitor-oms-image4.png)

Tıklayın **Oluştur** ve çalışma alanı ve çalışma alanı seçin ayarları.

![monitor-oms-image5.png](media/data-factory-monitor-oms/monitor-oms-image5.png)

### <a name="monitor-data-factory-metrics"></a>Data Factory ölçümlerini izleme

Yükleme **Azure Data Factory Analytics** şu ölçümler sağlayan görünümleri varsayılan kümesi oluşturur:

- Veri fabrikası tarafından ADF çalıştırır-1) işlem hattı çalıştırmaları

- Veri fabrikası tarafından etkinlik çalıştırmalarını ADF çalıştırır-2)

- Veri fabrikası tarafından ADF çalıştırır-3) tetikleyici çalıştırmaları

- ADF hataları-1) ilk 10 veri fabrikası işlem hattı hataları

- Veri fabrikası tarafından ADF hataları-2) ilk 10 etkinlik çalıştırmaları

- Veri fabrikası tarafından ADF hataları-3) ilk 10 tetikleyici hataları

- Türe göre etkinlik çalıştırmalarını ADF İstatistikleri-1)

- Türe göre ADF istatistikleri-2) tetikleyici çalıştırmaları

- ADF istatistikleri-3) en fazla işlem hattı süresi çalıştırır.

![monitor-oms-image6.png](media/data-factory-monitor-oms/monitor-oms-image6.png)

![monitor-oms-image7.png](media/data-factory-monitor-oms/monitor-oms-image7.png)

Yukarıdaki ölçümlerini görselleştirmek, bu ölçümleri arkasında sorguları bakmak, sorguları Düzenle, uyarı oluşturma ve VS.

![monitor-oms-image8.png](media/data-factory-monitor-oms/monitor-oms-image8.png)

## <a name="alerts"></a>Uyarılar

Azure portalında oturum açın ve tıklayın **İzleyicisi -&gt; uyarılar** uyarıları oluşturun.

![Portal menüsünde uyarıları](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Uyarı Oluştur

1.  Tıklayın **+ yeni uyarı kuralı** yeni bir uyarı oluşturmak için.

    ![Yeni uyarı kuralı](media/monitor-using-azure-monitor/alerts_image4.png)

2.  Tanımlama **Uyarı koşulu**.

    > [!NOTE]
    > Seçtiğinizden emin olun **tüm** içinde **kaynak türüne göre filtre**.

    ![Uyarı durumu, 1 / 3 ekran](media/monitor-using-azure-monitor/alerts_image5.png)

    ![Uyarı durumu, ekran 2 / 3](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Uyarı durumu, 3 / 3 ekran](media/monitor-using-azure-monitor/alerts_image7.png)

3.  Tanımlama **uyarı ayrıntıları**.

    ![Uyarı ayrıntıları](media/monitor-using-azure-monitor/alerts_image8.png)

4.  Tanımlama **eylem grubu**.

    ![Eylem grubu, ekran 1 / 4](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Eylem grubu, ekran 2 / 4](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Eylem grubu, ekran 3 / 4](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Eylem grubu, ekran 4 / 4](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Sonraki adımlar

Bkz: [izleme ve işlem hatlarını programlama yoluyla yönetme](monitor-programmatically.md) makalede izleme ve işlem hattı kodu ile yönetme hakkında bilgi edinmek için.
