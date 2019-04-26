---
title: Olay hub'ına Stream Azure tanılama günlükleri
description: Azure tanılama günlükleri Olay hub'ına akışı yapmayı öğrenin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: johnkem
ms.subservice: ''
ms.openlocfilehash: b5299af375646e7759d0770139df2cd6d7ce105c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60237682"
---
# <a name="stream-azure-diagnostic-logs-to-an-event-hub"></a>Olay hub'ına Stream Azure tanılama günlükleri
**[Azure tanılama günlükleri](diagnostic-logs-overview.md)**  portalında veya Azure aracılığıyla bir tanılama ayarını olay hub'ı yetkilendirme kuralı kimliği etkinleştirerek yerleşik "Dışarı aktarmak için Event Hubs" seçeneğini kullanarak herhangi bir uygulama için neredeyse gerçek zamanlı akış PowerShell cmdlet'leri veya Azure CLI.

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Tanılama günlükleri ve Event Hubs ile yapabilecekleriniz
Akış özelliği için tanılama günlüklerini kullanabilir birkaç yolu vardır:

* **3. taraf günlüğe kaydetme ve telemetri sistemleri günlüklerine Stream** – tüm tanılama günlüklerinizi kanal günlük verilerini bir üçüncü taraf SIEM veya log analytics aracı için tek bir olay hub'ına akış.
* **Power BI'a "sıcak yol" veri akışı tarafından hizmet durumunu görüntüle** – kullanarak Event Hubs, Stream Analytics ve Power BI, sizin kolayca dönüştürüp tanılama verilerinizi neredeyse gerçek zamanlı Öngörüler, Azure Hizmetleri için. [Bu belge makalesi nasıl Event hubs'ı ayarlayın, Stream Analytics ile verileri işlemek ve Power BI çıkış olarak kullanmak harika bir genel bakış sunar](../../stream-analytics/stream-analytics-power-bi-dashboard.md). Tanılama günlükleri ile ayarlanan için bazı ipuçları şunlardır:

  * Portalda seçeneği işaretleyin veya ile başlayan ada sahip ad alanında olay hub'ı seçmek istediğiniz şekilde PowerShell aracılığıyla etkinleştirin. tanılama günlüklerini kategorisi için bir olay hub'ı otomatik olarak oluşturulan **ınsights -**.
  * Aşağıdaki SQL kodunu tüm günlüğü verileri bir Power BI tablosuna ayrıştırmak için kullanabileceğiniz örnek bir Stream Analytics sorgu aşağıdaki gibidir:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the Power BI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Özel telemetri ve günlüğe kaydetme platform derleme** – bir yayımlama-abonelik yapısı ürettikleri telemetri platform veya olan biri, yüksek oranda ölçeklenebilir oluşturma hakkında düşünmek yalnızca zaten varsa Event Hubs, esnek bir şekilde tanılama günlüklerini alma olanak tanır. [Küresel ölçekte telemetri platform burada Event Hubs kullanarak Dan Rosanova'nın Kılavuzu](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-diagnostic-logs"></a>Tanılama günlükleri akışı etkinleştirme

Tanılama günlüklerini programlı olarak portal, akış veya kullanarak etkinleştirebilirsiniz [Azure İzleyici REST API'leri](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings). Tanılama ayarını oluşturduğunuz her iki durumda da bir Event Hubs ad alanı ve günlük kategorileri ve ad alanına göndermek istediğiniz ölçümleri, belirttiğiniz içinde. Bir olay hub'ı etkinleştirdiğiniz her günlük kategorisi için bir ad alanı oluşturulur. Bir tanılama **günlüğü kategorisi** kaynak toplayabilir günlük türüdür.

> [!WARNING]
> Etkinleştirme ve işlem kaynakları (örneğin, VM'ler veya Service Fabric) tanılama günlüklerinin akışını [farklı bir dizi adım gerektirir](../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md).

Event Hubs ad alanı, hem abonelik hem de her iki aboneliğin uygun RBAC erişim ayarı yapılandıran kullanıcının sahip olduğu sürece, günlükleri yayan kaynak ile aynı abonelikte olmak zorunda değil, aynı AAD kiracısındaki bir parçasıdır.

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir olay Hub'ındaki 'Gelen iletiler' ölçümü temelinde araştırılıp bir kuyruk düzeyi. Ancak, tanılama ayarları aracılığıyla dışarı aktarılan ölçüm, Olay Hub’ındaki tüm kuyruklarda tüm gelen iletiler halinde ifade edilir.
>
>

## <a name="stream-diagnostic-logs-using-the-portal"></a>Portalı kullanarak Stream tanılama günlükleri

1. Portal, Azure İzleyicisi'ne gidin ve tıklayarak **tanılama ayarları**

    ![Azure İzleyicisi İzleme](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-blade.png)

2. İsteğe bağlı olarak kaynak grubu veya kaynak türe göre listeyi filtreleyin ve kaynağın tanılama ayarını ayarlamak istediğiniz'i tıklatın.

3. Hiçbir ayar kaynağı varsa, istenen bir ayar oluşturmak için seçtiğiniz. "Tanılamayı Aç" tıklayın

   ![Tanılama ayarını - mevcut hiçbir ayar Ekle](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-none.png)

   Kaynakta mevcut ayarları varsa, bu kaynakta zaten yapılandırılmış ayarların bir listesini görürsünüz. "Tanılama ayarı Ekle" ye tıklayın

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-multiple.png)

3. Ayarı, bir ad verin ve kutusunu işaretlemeniz **Stream olay hub'ına**, ardından bir Event Hubs ad alanı seçin.

   ![Tanılama ayarını - var olan ayarları Ekle](media/diagnostic-logs-stream-event-hubs/diagnostic-settings-configure.png)

   Seçili ad alanı, olay hub'ı (Bu, ilk saat akış tanılama günlükleri ise) oluşturulan veya diğer akışla burada olacaktır (olup olmadığını zaten bu ad alanı için günlük kategori akış kaynakları), ve izinleri ilkeyi tanımlar, Akış mekanizması vardır. Bugün, bir olay hub'ına akış Yönet, gönderme ve dinleme izinleri gerektirir. Ad alanınız için Event Hubs ad alanı paylaşılan erişim ilkeleri portalında Yapılandır sekmesi altındaki değiştirme ya da oluşturun. Bu tanılama ayarları birini güncelleştirmek için istemcinin Event hubs'ı yetkilendirme kuralı ListKey izni olmalıdır. Ayrıca isteğe bağlı olarak, bir olay hub'ı adı belirtebilirsiniz. Bir olay hub'ı adı belirtirseniz, günlükleri Olay hub'ına yerine günlüğü Kategori başına yeni oluşturulan olay hub'ına yönlendirilir.

4. **Kaydet**’e tıklayın.

Birkaç dakika sonra yeni ayar, bu kaynak için ayarların listesi görüntülenir ve oluşturulan yeni olay verilerini hemen sonra tanılama günlüklerini, olay hub'ına akış.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aracılığıyla akışını etkinleştirmek için [Azure PowerShell cmdlet'leri](../../azure-monitor/platform/powershell-quickstart-samples.md), kullanabileceğiniz `Set-AzDiagnosticSetting` cmdlet şu parametrelerle:

```powershell
Set-AzDiagnosticSetting -ResourceId [your resource ID] -EventHubAuthorizationRuleId [your Event Hub namespace auth rule ID] -Enabled $true
```

Olay hub'ı yetkilendirme kuralı kimliği şu biçime sahip bir dizedir: `{Event Hub namespace resource ID}/authorizationrules/{key name}`, örneğin, `/subscriptions/{subscription ID}/resourceGroups/{resource group}/providers/Microsoft.EventHub/namespaces/{Event Hub namespace}/authorizationrules/RootManageSharedAccessKey`. Şu anda PowerShell belirli bir olay hub'ı adıyla seçemezsiniz.

### <a name="via-azure-cli"></a>Via Azure CLI

Aracılığıyla akışını etkinleştirmek için [Azure CLI](https://docs.microsoft.com/cli/azure/monitor?view=azure-cli-latest), kullanabileceğiniz [az İzleyici diagnostic-settings oluşturma](https://docs.microsoft.com/cli/azure/monitor/diagnostic-settings?view=azure-cli-latest#az-monitor-diagnostic-settings-create) komutu.

```azurecli
az monitor diagnostic-settings create --name <diagnostic name> \
    --event-hub <event hub name> \
    --event-hub-rule <event hub rule ID> \
    --resource <target resource object ID> \
    --logs '[
    {
        "category": <category name>,
        "enabled": true
    }
    ]'
```

Tanılama günlüğüne olarak geçirilen JSON dizisi sözlükleri ekleyerek ek kategori ekleyebilirsiniz `--logs` parametresi.

`--event-hub-rule` Bağımsız değişkeni için PowerShell Cmdlet açıklandığı gibi bu aynı biçimi olay hub'ı yetkilendirme kuralı kimliği kullanır.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Event Hubs günlük verilerini nasıl kullanır?

Event Hubs örnek çıkış verilerini şu şekildedir:

```json
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Öğe adı | Açıklama |
| --- | --- |
| kayıt |Bu yük tüm günlük olaylar dizisi. |
| time |Olayın gerçekleştiği zaman. |
| category |Bu olay için günlüğü kategorisi. |
| resourceId |Bu olayı oluşturan kaynağının kaynak kimliği. |
| operationName |İşlemin adı. |
| düzey |İsteğe bağlı. Günlük olayı düzeyini gösterir. |
| properties |Olay Özellikleri. |

Event Hubs'a akış'ı destekleyen tüm kaynak sağlayıcılarının bir listesini görüntüleyebileceğiniz [burada](diagnostic-logs-overview.md).

## <a name="stream-data-from-compute-resources"></a>Stream verilerden bilgi işlem kaynakları

Ayrıca Windows Azure tanılama aracısını kullanarak işlem kaynaklarını tanılama günlüklerinin akışını yapabilirsiniz. [Bu makaleye bakın](../../azure-monitor/platform/diagnostics-extension-stream-event-hubs.md) ayarlanacağını öğrenmek için.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İzleyici ile Stream Azure Active Directory günlükleri](../../active-directory/reports-monitoring/tutorial-azure-monitor-stream-logs-to-event-hub.md)
* [Azure Tanılama Günlükleri](diagnostic-logs-overview.md)
* [Event Hubs kullanmaya başlayın](../../event-hubs/event-hubs-dotnet-standard-getstarted-send.md)

