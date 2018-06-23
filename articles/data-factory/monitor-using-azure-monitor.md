---
title: İzleme Azure İzleyicisi'ni kullanarak veri fabrikaları | Microsoft Docs
description: Azure Data Factory bilgileriyle tanılama günlükleri'ni etkinleştirerek Data Factory işlem hatlarını izlemek için Azure İzleyici kullanmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: shlo
ms.openlocfilehash: e60f368115e91cbd8972af8dfa7f0f3d6ea8765b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2018
ms.locfileid: "36337600"
---
# <a name="alert-and-monitor-data-factories-using-azure-monitor"></a>Uyarı ve İzleyici veri fabrikaları Azure İzleyicisi'ni kullanma
Bulut uygulamalarını birçok taşıma bölümleriyle karmaşıktır. İzleme, uygulamanızı kurma kalmasını sağlamak için veri ve sağlıklı bir durumda çalışmasını sağlar. Ayrıca olası sorunları stave veya olanları sorun gidermeye yardımcı olur. Ayrıca, uygulamanız hakkında ayrıntılı Öngörüler elde etmek için izleme verilerini kullanabilirsiniz. Bu bilgi, uygulama performansı veya devamlılığını iyileştirmek için yardımcı veya aksi halde el ile müdahale gerektiren Eylemler otomatikleştirmek.

Azure İzleyicisi, çoğu Microsoft Azure hizmetlerini taban düzeyi altyapı ölçümleri ve günlükleri sağlar. Ayrıntılar için bkz [izlemeye genel bakış](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor). Azure tanılama günlüklerini bu kaynakla ilgili zengin, sık sık veri sağlayan bir kaynak tarafından gösterilen günlüklerin. Veri Fabrikası Azure İzleyicisi'nde tanılama günlüklerini çıkarır.

> [!NOTE]
> Bu makale şu anda önizleme sürümünde olan Data Factory sürüm 2 için geçerlidir. Genel olarak kullanılabilir (GA) Data Factory Hizmeti'ne 1 sürümünü kullanıyorsanız bkz [İzleyici ve ardışık düzenlerinde Data Factory version1 yönetmek](v1/data-factory-monitor-manage-pipelines.md).

## <a name="persist-data-factory-data"></a>Veri Fabrikası veri Sürdür
Veri Fabrikası yalnızca ardışık düzen veri 45 gün çalıştırmak depolar. Azure İzleyicisi'ni kullanarak birden fazla 45 gün için veri çalıştırmak ardışık düzen kalıcı hale getirmek istiyorsanız, size yalnızca çözümleme için tanılama günlükleri yönlendirebilir değil, sizin chossing süresi için Fabrika bilgilerine sahip olması için bunları bir depolama hesabına sürdürebilirsiniz.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

* Bunları kaydetmek bir **depolama hesabı** denetim veya el ile İnceleme için. Tanılama ayarları kullanarak bekletme süresi (gün) cinsinden belirtebilirsiniz.
* Bunları akış **olay hub'ları** bir üçüncü taraf hizmeti veya Powerbı gibi özel analiz çözümü tarafından alımı için.
* Bunları ile analiz **günlük analizi**

Günlükleri yayma kaynak ile aynı abonelikte değil bir depolama hesabı veya olay hub'ı ad alanını kullanabilirsiniz. Ayar yapılandıran kullanıcı her iki aboneliğin uygun rol tabanlı erişim denetimi (RBAC) erişimi olmalıdır.

## <a name="set-up-diagnostic-logs"></a>Tanılama günlüklerini ayarlayın

### <a name="diagnostic-settings"></a>Tanılama ayarları
Tanılama günlüklerini işlem dışı kaynaklar için tanılama ayarları kullanılarak yapılandırılır. Kaynak denetimi için tanılama ayarları:

* Burada tanılama günlükleri (depolama hesabı, olay hub'ları ve/veya günlük analizi) gönderilir.
* Günlük kategorilerini gönderilir.
* Her günlük kategori bir depolama hesabında ne kadar süre tutulacağını
* Sıfır gün bekletme günlükleri sonsuza kadar tutulur anlamına gelir. Aksi takdirde, değer 1 ile 2147483647 arasındaki gün herhangi bir sayıda olabilir.
* Bekletme ilkeleri ayarlanır ancak günlükleri bir depolama hesabına depolama devre dışı bırakıldı (örneğin, yalnızca olay hub'ları veya günlük analizi seçenekler seçilidir), bekletme ilkeleri herhangi bir etkisi yoktur.
* Bekletme ilkeleri uygulanan başına günlük, olduğundan, bir gün (UTC) dışında tutma sunulmuştur gün günlüklerinden sonunda İlkesi silinir. Örneğin, bir günlük bir Bekletme İlkesi nesneniz varsa, günün bugün başında dünden önceki gün günlüklerinden silinecek.

### <a name="enable-diagnostic-logs-via-rest-apis"></a>REST API'leri aracılığıyla tanılama günlüklerini etkinleştirme

Azure İzleyici REST API tanılama ayarında güncelle

**İstek**
```
PUT
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst Bilgiler**
* Değiştir `{api-version}` ile `2016-09-01`.
* Değiştir `{resource-id}` kendisi için gibi tanılama ayarlarını düzenlemek kaynak kaynak kimliği. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../azure-resource-manager/resource-group-portal.md).
* Ayarlama `Content-Type` başlığına `application/json`.
* Azure Active Directory'den elde bir JSON web belirteci için yetkilendirme üst ayarlayın. Daha fazla bilgi için bkz: [istekleri kimlik doğrulaması](../active-directory/develop/active-directory-authentication-scenarios.md).

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
| storageAccountId |Dize | Tanılama günlükleri göndermesini istediğiniz depolama hesabının kaynak kimliği |
| serviceBusRuleId |Dize | Olay hub'ın tanılama günlüklerini akış için oluşturulmuş olmasını istediğiniz hizmet veri yolu ad alanı service bus kuralı kimliği. Kimliktir biçimi kuralı: "{hizmet veri yolu kaynak kimliği} /authorizationrules/ {anahtar name}".|
| Workspaceıd | Karmaşık Tür | Ölçüm zaman grains ve bekletme ilkelerini dizisi. Bu özellik şu anda boştur. |
|metrics| Çağrılan ardışık düzene iletilecek parametre değerlerini ardışık çalıştırın| Parametre adları bağımsız değişken değeri için eşleştirme bir JSON nesnesi |
| günlükler| Karmaşık Tür| Bir kaynak türü için bir tanılama günlük kategori adı. Bir kaynak için tanılama günlük kategorileri listesini almak için önce bir GET tanılama ayarlarını işlemi gerçekleştirin. |
| category| Dize| Günlük kategorileri ve bekletme ilkelerini dizisi |
| timeGrain | Dize | ISO 8601 süre biçiminde yakalanır ölçümleri ayrıntı düzeyi. PT1M (bir dakika) olması gerekir|
| enabled| Boole | Ölçüm ya da günlük kategoriye koleksiyonu bu kaynak için etkin olup olmadığını belirtir|
| retentionPolicy| Karmaşık Tür| Ölçüm ya da günlük kategorisi için bekletme ilkesi açıklar. Yalnızca depolama hesabı seçeneği için kullanılır.|
| gün| Int| Ölçümleri veya günlükleri tutulacağı gün sayısı. 0 değeri günlükleri sonsuza kadar saklar. Yalnızca depolama hesabı seçeneği için kullanılır. |

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

Azure İzleyici REST API tanılama ayarı hakkında bilgi edinin

**İstek**
```
GET
https://management.azure.com/{resource-id}/providers/microsoft.insights/diagnosticSettings/service?api-version={api-version}
```

**Üst Bilgiler**
* Değiştir `{api-version}` ile `2016-09-01`.
* Değiştir `{resource-id}` kendisi için gibi tanılama ayarlarını düzenlemek kaynak kaynak kimliği. Daha fazla bilgi için kaynağını kullanarak Azure kaynaklarınızı yönetmek için gruplar.
* Ayarlama `Content-Type` başlığına `application/json`.
* Yetkilendirme üst bilgisi bir JSON Web, Azure Active Directory'den elde belirteci ayarlayın. Daha fazla bilgi için Authenticating istekleri bakın.

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
[Daha fazla bilgi](https://docs.microsoft.com/en-us/rest/api/monitor/diagnosticsettings)

## <a name="schema-of-logs--events"></a>Şema günlüklerini ve olayları

### <a name="activity-run-logs-attributes"></a>Etkinlik çalışma özniteliklerini günlüğe kaydeder

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
   "properties:"
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
| Düzey |Dize | Tanılama günlüklerini düzeyi. Düzey 4 etkinlik günlükleri için her zaman durumdur. | `4`  |
| correlationId |Dize | Bir özel talep uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Timespan, UTC biçiminde olay zamanı | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|activityRunId| Dize| Etkinlik Kimliği | `3a171e1f-b36e-4b80-8a54-5625394f4354` |
|pipelineRunId| Dize| Çalıştırma ardışık kimliği | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Dize | Veri Fabrikası kaynağın ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlüklerini kategorisi. Bu özelliği "Fabrikanız" olarak ayarlayın | `ActivityRuns` |
|düzey| Dize | Tanılama günlüklerini düzeyi. Bu özelliği "Bilgilendirici" olarak ayarlayın | `Informational` |
|operationName| Dize |Etkinlik durumu olan adı. Başlangıç sinyal durumundaysa olduğu `MyActivity -`. Son sinyal durumundaysa olduğu `MyActivity - Succeeded` son durumuna sahip | `MyActivity - Succeeded` |
|pipelineName| Dize | Ardışık Düzen adı | `MyPipeline` |
|activityName| Dize | Etkinlik adı | `MyActivity` |
|start| Dize | Timespan, UTC biçiminde etkinlik başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|end| Dize | Timespan, UTC biçiminde etkinliğin sona erer çalıştırın. Etkinlik değil bittiyse henüz (başlayarak bir etkinlik için tanılama günlük), varsayılan değer olarak `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |


### <a name="pipeline-run-logs-attributes"></a>Ardışık Düzen Çalıştır öznitelikleri günlüğe kaydeder

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
| Düzey |Dize | Tanılama günlüklerini düzeyi. Düzey 4 etkinlik günlükleri için geçerlidir. | `4`  |
| correlationId |Dize | Bir özel talep uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Timespan, UTC biçiminde olay zamanı | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|çalıştırma kodu| Dize| Çalıştırma ardışık kimliği | `9f6069d6-e522-4608-9f99-21807bfc3c70` |
|resourceId| Dize | Veri Fabrikası kaynağın ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlüklerini kategorisi. Bu özelliği "PipelineRuns" olarak ayarlayın | `PipelineRuns` |
|düzey| Dize | Tanılama günlüklerini düzeyi. Bu özelliği "Bilgilendirici" olarak ayarlayın | `Informational` |
|operationName| Dize |Durumuna sahip ardışık düzen adı. "Ardışık - başarılı oldu" Çalıştır ardışık düzeni tamamlandığında son durumuna sahip| `MyPipeline - Succeeded` |
|pipelineName| Dize | Ardışık Düzen adı | `MyPipeline` |
|start| Dize | Timespan, UTC biçiminde etkinlik başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|end| Dize | Timespan, UTC biçiminde Etkinliğin bitiş çalışır. Etkinlik değil bittiyse henüz (başlayarak bir etkinlik için tanılama günlük), varsayılan değer olarak `1601-01-01T00:00:00Z` ayarlanır.  | `2017-06-26T20:55:29.5007959Z` |
|durum| Dize | Son durum ardışık çalıştırın (başarılı veya başarısız) | `Succeeded`|


### <a name="trigger-run-logs-attributes"></a>Tetikleyici Çalıştır öznitelikleri günlüğe kaydeder

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
| Düzey |Dize | Tanılama günlüklerini düzeyi. Düzey 4 etkinlik günlükleri için ayarlayın. | `4`  |
| correlationId |Dize | Bir özel talep uçtan uca izlemek için benzersiz kimliği | `319dc6b4-f348-405e-b8d7-aafc77b73e77` |
| time | Dize | Timespan, UTC biçiminde olay zamanı | `YYYY-MM-DDTHH:MM:SS.00000Z` | `2017-06-28T21:00:27.3534352Z` |
|Tetikleyici No| Dize| Çalıştırma Tetikleyici kimliği | `08587023010602533858661257311` |
|resourceId| Dize | Veri Fabrikası kaynağın ilişkili kaynak kimliği | `/SUBSCRIPTIONS/<subID>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/FACTORIES/<dataFactoryName>` |
|category| Dize | Tanılama günlüklerini kategorisi. Bu özelliği "PipelineRuns" olarak ayarlayın | `PipelineRuns` |
|düzey| Dize | Tanılama günlüklerini düzeyi. Bu özelliği "Bilgilendirici" olarak ayarlayın | `Informational` |
|operationName| Dize |Son durum tetikleyiciyle adını olup başarıyla gönderildi. "MyTrigger - başarılı oldu" sinyal başarılı olduysa| `MyTrigger - Succeeded` |
|tetikleyiciadı| Dize | Tetikleyici adı | `MyTrigger` |
|triggerType| Dize | (El ile tetikleyici veya zamanlama tetikleme) tetikleyici türü | `ScheduleTrigger` |
|triggerEvent| Dize | Tetikleyici olayı | `ScheduleTime - 2017-07-06T01:50:25Z` |
|start| Dize | Tetikleyici yangın timespan, UTC biçiminde içinde başlangıcı | `2017-06-26T20:55:29.5007959Z`|
|durum| Dize | Olup tetikleyici başarıyla (başarılı veya başarısız) harekete son durumu | `Succeeded`|

## <a name="metrics"></a>Ölçümler

Azure İzleyicisi, performans ve sistem durumu, iş yüklerinin Azure üzerinde görünürlük elde etmek için telemetri kullanmasına olanak sağlar. En önemli Azure telemetri verileri çoğu Azure kaynaklar tarafından gösterilen (performans sayaçlarını olarak da bilinir) ölçümleri türüdür. Azure İzleyicisi'ni yapılandırma ve izleme ve sorun giderme için bu ölçümleri kullanmak için çeşitli yöntemler sağlar.

Aşağıdaki ölçümleri ADFV2 yayar

| **Ölçüm**           | **Ölçüm görünen adı**         | **Birim** | **Toplama türü** | **Açıklama**                                       |
|----------------------|---------------------------------|----------|----------------------|-------------------------------------------------------|
| PipelineSucceededRun | Ardışık Düzen çalıştırır ölçümleri başarılı oldu | Sayı    | Toplam                | Toplam işlem hatları çalıştıran başarılı dakika penceresi içinde |
| PipelineFailedRuns   | Ardışık Düzen çalıştırır ölçümleri başarısız oldu    | Sayı    | Toplam                | Toplam işlem hatları çalıştıran başarısız dakika penceresi içinde    |
| ActiviySucceededRuns | Etkinlik çalıştığında ölçümleri başarılı oldu | Sayı    | Toplam                | Toplam etkinlik dakika pencereye başarılı çalışır  |
| ActivityFailedRuns   | Etkinlik çalıştığında ölçümleri başarısız oldu    | Sayı    | Toplam                | Bir dakika pencere içinde toplam etkinlik başarısız çalışır     |
| TriggerSucceededRuns | Tetikleyici çalıştırır ölçümleri başarılı oldu  | Sayı    | Toplam                | Toplam tetikleyici dakika pencereye başarılı çalıştırır   |
| TriggerFailedRuns    | Tetikleyici çalıştırır ölçümleri başarısız oldu     | Sayı    | Toplam                | Toplam tetikleyici dakika pencereye başarısız çalıştırır      |

Ölçümleri erişmek için makalesindeki yönergeleri izleyin- https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-metrics

## <a name="alerts"></a>Uyarılar

Veri Fabrikası'nda desteklenen ölçümleri uyarılar oluşturabilir. Tıklatın **uyarıları** Data Factory düğmesinde **İzleyici** sayfası.

![Uyarıları seçeneği](media/monitor-using-azure-monitor/alerts_image1.png)

Bu, olanak sürer **uyarıları** sayfası.

![Uyarılar sayfası](media/monitor-using-azure-monitor/alerts_image2.png)

Ayrıca Azure portalında oturum açın ve'ı tıklatın **İzleyicisi -&gt; uyarıları** ulaşması **uyarıları** doğrudan sayfa.

![Portal menüsünde uyarıları](media/monitor-using-azure-monitor/alerts_image3.png)

### <a name="create-alerts"></a>Uyarı Oluştur

1.  Tıklatın **+ yeni uyarı kuralı** yeni bir uyarı oluşturmak için.

    ![Yeni uyarı kuralı](media/monitor-using-azure-monitor/alerts_image4.png)

2.  Tanımlamak **Uyarı koşulu**.

    > [!NOTE]
    > Seçtiğinizden emin olun **tüm** içinde **kaynak türüne göre filtre**.

    ![Koşul uyarı, 1 / 3 ekran](media/monitor-using-azure-monitor/alerts_image5.png)

    ![Koşul uyarı, 2 / 3 ekran](media/monitor-using-azure-monitor/alerts_image6.png)

    ![Koşul uyarı, 3 / 3 ekran](media/monitor-using-azure-monitor/alerts_image7.png)

3.  Tanımlamak **uyarı ayrıntıları**.

    ![Uyarı ayrıntıları](media/monitor-using-azure-monitor/alerts_image8.png)

4.  Tanımlamak **eylem grubu**.

    ![Eylem grubu, ekran 1 / 4](media/monitor-using-azure-monitor/alerts_image9.png)

    ![Eylem grubu, 4, 2 ekran](media/monitor-using-azure-monitor/alerts_image10.png)

    ![Eylem grubu, ekran 3 4](media/monitor-using-azure-monitor/alerts_image11.png)

    ![Eylem grubu, ekran 4 4](media/monitor-using-azure-monitor/alerts_image12.png)

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [İzleyici ve ardışık düzen programlı olarak yönetmek](monitor-programmatically.md) makalede izleme ve ardışık düzen çalıştırarak yönetme hakkında bilgi edinin.
