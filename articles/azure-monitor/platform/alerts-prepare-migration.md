---
title: Logic apps ve runbook'ları güncelleştirerek Azure İzleyici Klasik uyarılar geçiş için hazırlama
description: Web kancaları, logic apps ve gönüllü geçişe hazırlamak için runbook'ları değiştirme hakkında bilgi edinin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 347c89991cbb4d28b46eafff0a783148793ad2f7
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64727491"
---
# <a name="prepare-your-logic-apps-and-runbooks-for-migration-of-classic-alert-rules"></a>Logic apps ve runbook'ları Klasik uyarı kuralları bir geçiş için hazırlama

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Temmuz 2019 ' kullanımdan. Geçiş Aracı, Azure portalında Klasik uyarı kuralları kullanan ve kendilerini geçişini isteyen müşteriler için kullanılabilir.

Yeni uyarı kuralları için uyarı kurallarınızı Klasik gönüllü olarak geçirmeyi seçerseniz, iki sistem arasındaki bazı farklar olduğunu unutmayın. Bu makalede bu farklılıklar ve değişikliğe hazırlanmak nasıl açıklanmaktadır.

## <a name="api-changes"></a>API değişiklikleri

Oluşturma ve klasik uyarı kurallarını yönet API'leri (`microsoft.insights/alertrules`), yeni ölçüm uyarıları oluşturma ve yönetme API'lerinden farklı (`microsoft.insights/metricalerts`). Program aracılığıyla oluşturma ve bugün Klasik uyarı kuralları yönetin, dağıtım betiklerinizi yeni API'leri ile çalışacak şekilde güncelleştirin.

Aşağıdaki tabloda, hem Klasik hem de yeni uyarılar için programlama arabirimleri başvurusu vardır:

|         |Klasik uyarılar  |Yeni ölçüm uyarıları |
|---------|---------|---------|
|REST API     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure CLI     | [az İzleyici Uyarısı](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [az İzleyici ölçümleri Uyarısı](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Başvuru](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |      |
| Azure Resource Manager şablonu | [Klasik uyarılar için](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Yeni ölçüm uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Bildirim yükü değişiklikleri

Bildirim yükü biçimi arasında biraz daha farklı olmasına [Klasik uyarı kuralları](alerts-webhooks.md) ve [yeni ölçüm uyarıları](alerts-metric-near-real-time.md#payload-schema). Herhangi bir Web kancası, mantıksal uygulama veya Klasik uyarı kuralları tarafından tetiklenen runbook'u eylemler varsa, yeni ölçüm uyarıları yükü biçimini kabul etmek için bu bildirim uç noktalarını güncelleştirmeniz gerekir.

Klasik biçimi Web kancası yükü alanları yeni biçime eşlemek için aşağıdaki tabloyu kullanın:

|  |Klasik uyarılar  |Yeni ölçüm uyarıları |
|---------|---------|---------|
|Uyarının etkin veya çözümlenen?    | **Durumu**       | **data.status** |
|Uyarı hakkında bağlamsal bilgiler     | **Bağlam**        | **Data.Context**        |
|Zaman damgası, uyarının etkin veya çözümlendi     | **Context.Timestamp**       | **Data.Context.Timestamp**        |
| Uyarı kuralı kimliği | **Context.id** | **Data.Context.id** |
| Uyarı kuralı adı | **Context.Name** | **Data.Context.Name** |
| Uyarı kuralı açıklaması | **Context.Description** | **Data.Context.Description** |
| Uyarı kuralı koşulu | **Context.condition** | **Data.Context.condition** |
| Ölçüm adı | **context.condition.metricName** | **data.context.condition.allOf[0].metricName** |
| Zaman toplama (nasıl ölçüm değerlendirme pencere üzerinde toplanır)| **data.context.condition.timeAggregation** | **data.context.condition.timeAggregation** |
| Değerlendirme süresi | **context.condition.windowSize** | **data.context.condition.windowSize** |
| (Nasıl toplanan bir ölçüm değeri eşik karşı karşılaştırılır) işleci | **Context.Condition.operator** | **Data.Context.Condition.operator** |
| Eşik | **Context.Condition.Threshold** | **data.context.condition.allOf[0].threshold** |
| Ölçüm değeri | **context.condition.metricValue** | **data.context.condition.allOf[0].metricValue** |
| Abonelik Kimliği | **context.subscriptionId** | **data.context.subscriptionId** |
| Etkilenen kaynak kaynak grubu | **context.resourceGroup** | **data.context.resourceGroup** |
| Etkilenen kaynak adı | **context.resourceName** | **data.context.resourceName** |
| Etkilenen kaynak türü | **context.resourceType** | **data.context.resourceType** |
| Etkilenen kaynak kaynak kimliği | **context.resourceId** | **data.context.resourceId** |
| Portal kaynak Özet sayfasında doğrudan bağlantı | **context.portalLink** | **data.context.portalLink** |
| Web kancası veya mantıksal uygulamaya geçirilecek özel yük alanları | **Özellikleri** | **Data.Properties** |

Gördüğünüz gibi yüklerini benzerdir. Aşağıdaki bölümde sunar:

- Mantıksal uygulamalar'yeni biçime ile çalışacak şekilde değiştirme hakkında ayrıntılar.
- Yeni uyarılar için bildirim yükü ayrıştırmak için bir runbook örneği.

## <a name="modify-a-logic-app-to-receive-a-metric-alert-notification"></a>Bir ölçüm uyarı bildirim almak için bir mantıksal uygulama değiştirme

Logic apps ile klasik uyarıları kullanıyorsanız, yeni ölçüm uyarıları yükü ayrıştırmak için mantıksal uygulama kodunuzu değiştirmeniz gerekir. Şu adımları uygulayın:

1. Yeni bir mantıksal uygulama oluşturun.

1. "Azure İzleyici – ölçümleri uyarı işleyicisi" şablonu kullanın. Bu şablonda bir **HTTP isteği** içindeki uygun şemayı tanımlanan tetikleyici.

    ![mantıksal uygulama şablonunu](media/alerts-migration/logic-app-template.png "ölçüm uyarı şablonu")

1. İşlem mantığınızı barındırmak için bir eylem ekleme.

## <a name="use-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Ölçüm uyarı bildirim aldığı bir Otomasyon runbook'unu kullanın

Aşağıdaki örnek, bir runbook'ta kullanmak için PowerShell kodu sağlar. Bu kod yüklerini hem Klasik ölçüm uyarı kuralları hem de yeni ölçüm uyarı kuralları için ayrıştırabilirsiniz.

```PowerShell
## Example PowerShell code to use in a runbook to handle parsing of both classic and new metric alerts.

[OutputType("PSAzureOperationResponse")]

param
(
    [Parameter (Mandatory=$false)]
    [object] $WebhookData
)

$ErrorActionPreference = "stop"

if ($WebhookData)
{
    # Get the data object from WebhookData.
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Determine whether the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new metric alert schema.
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic metric alert schema.
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition.
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is neither a classic metric alert nor a new metric alert.
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    # Parse fields related to resource affected.
    $ResourceName = $AlertContext.resourceName
    $ResourceType = $AlertContext.resourceType
    $ResourceGroupName = $AlertContext.resourceGroupName
    $ResourceId = $AlertContext.resourceId
    $SubId = $AlertContext.subscriptionId

    ## Your logic to handle the alert here.
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}

```

Bir sanal makine bir uyarı tetiklendiğinde durduran bir runbook'un tam bir örnek için bkz. [Azure Otomasyonu belgeleri](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Web kancaları aracılığıyla iş ortağı tümleştirmesi

Çoğu [Klasik uyarılar ile Birleşen İş Ortaklarımızın](https://docs.microsoft.com/azure/azure-monitor/platform/partners) kendi tümleştirmeler aracılığıyla yeni ölçüm uyarılarının zaten desteklemektedir. Zaten yeni ölçüm uyarılarla çalışma bilinen tümleştirmeleri şunlardır:

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Sıgnl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Burada listelenmeyen bir iş ortağı tümleştirmesi kullanıyorsanız, tümleştirme sağlayıcı ile Tümleştirmesi ile yeni ölçüm uyarıları çalışır durumda olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş Aracı nasıl çalıştığını anlamak](alerts-understand-migration.md)
