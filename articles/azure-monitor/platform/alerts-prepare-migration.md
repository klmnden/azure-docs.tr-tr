---
title: Logic apps ve runbook'ları güncelleştirerek Azure İzleyici Klasik uyarılar geçiş için hazırlama
description: Web kancası, mantıksal uygulamanızı ve gönüllü geçişe hazırlamak için runbook'ları değiştirme hakkında bilgi edinin.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 3c47404826d5055d4a82d4842523f790fb11f000
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632513"
---
# <a name="prepare-your-logic-apps-and-run-books-for-classic-alert-rules-migration"></a>Mantıksal uygulamalarınızı hazırlamak ve klasik uyarı kuralları geçiş için books çalıştırın

Olarak [daha önce duyurulduğu gibi](monitoring-classic-retirement.md), Azure İzleyici'de klasik uyarılar, Temmuz 2019 ' kullanımdan. Geçiş Aracı, geçiş gönüllü olarak tetiklemek için Azure portalında kullanılabilir ve klasik uyarı kuralları kullanan müşteriler için kullanıma sunuluyor.

Yeni uyarı kuralları için uyarı kurallarınızı Klasik gönüllü olarak geçirmeyi seçerseniz, farkında olmanız gereken iki sistem arasındaki bazı farklar vardır. Bu makalede, iki sistem arasındaki değişikliğe hazırlanmak nasıl farkları aracılığıyla size yol gösterir.

## <a name="api-changes"></a>API değişiklikleri

API'ler Klasik uyarı kuralları oluşturma/yönetmek için kullanılan (`microsoft.insights/alertrules`) yeni ölçüm uyarıları oluşturma/yönetmek için kullanılan API'lerinden farklı (`microsoft.insights/metricalerts`). Program aracılığıyla oluşturma/Klasik uyarı kuralları bugün yönetiyorsanız, dağıtım betiklerinizi yeni API'leri ile çalışacak şekilde güncelleştirin.

Aşağıdaki tabloda, hem Klasik hem de yeni uyarılar için programlama arabirimleri başvuru sağlar.

|         |Klasik uyarılar  |Yeni ölçüm uyarıları |
|---------|---------|---------|
|REST API     | [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules)         | [microsoft.insights/metricalerts](https://docs.microsoft.com/rest/api/monitor/metricalerts)       |
|Azure CLI'si     | [az İzleyici Uyarısı](https://docs.microsoft.com/cli/azure/monitor/alert?view=azure-cli-latest)        | [az İzleyici ölçümleri Uyarısı](https://docs.microsoft.com/cli/azure/monitor/metrics/alert?view=azure-cli-latest)        |
|PowerShell      | [Başvuru](https://docs.microsoft.com/powershell/module/az.monitor/add-azmetricalertrule)       |      |
| Azure Resource Manager şablonu | [Klasik uyarılar için](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-enable-template)|[Yeni ölçüm uyarıları](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-create-templates)|

## <a name="notification-payload-changes"></a>Bildirim yükü değişiklikleri

Bildirim yükü biçimi arasında biraz daha farklı olmasına [Klasik uyarı kuralları](alerts-webhooks.md) ve [yeni ölçüm uyarıları](alerts-metric-near-real-time.md#payload-schema). Herhangi bir Web kancası, mantıksal uygulama'iniz varsa veya Klasik uyarı kuralları tarafından tetiklenen runbook eylemleri, yeni ölçüm uyarıları yükü biçimini kabul etmek için bu bildirim uç noktalarını güncelleştirmek gerekir.

Aşağıdaki tabloda, klasik bir uyarı kuralı Web kancası yükü ve yeni ölçüm uyarı Web kancası yükü arasındaki alanları eşleyin için kullanabilirsiniz.

|  |Klasik uyarılar  |Yeni ölçüm uyarıları |
|---------|---------|---------|
|Uyarının etkin veya çözümlenen     | durum       | Data.Status |
|Uyarı hakkında bağlamsal bilgiler     | Bağlam        | Data.Context        |
|Hangi uyarının etkin veya çözümlenmiş bir zaman damgası      | Context.Timestamp       | Data.Context.Timestamp        |
| Uyarı kuralı kimliği | Context.id | Data.Context.id |
| Uyarı kuralı adı | Context.Name | Data.Context.Name |
| Uyarı kuralı açıklaması | Context.Description | Data.Context.Description |
| Uyarı kuralı koşulu | Context.condition | Data.Context.condition|
| Ölçüm adı | context.condition.metricName| data.context.condition.allOf[0].metricName|
| Zaman toplama (nasıl ölçüm değerlendirme pencere üzerinde toplanır)|data.context.condition.timeAggregation|data.context.condition.timeAggregation|
| Değerlendirme süresi | context.condition.windowSize | data.context.condition.windowSize|
| (Nasıl toplanan bir ölçüm değeri eşik karşı karşılaştırılır) işleci | Context.Condition.operator | Data.Context.Condition.operator|
| Eşik | Context.Condition.Threshold| data.context.condition.allOf[0].threshold|
| Ölçüm değeri | context.condition.metricValue | data.context.condition.allOf[0].metricValue|
| Abonelik kimliği | context.subscriptionId | data.context.subscriptionId|
| Etkilenen kaynak kaynak grubu | context.resourceGroup | data.context.resourceGroup|
| Etkilenen kaynak adı | context.resourceName | data.context.resourceName |
| Etkilenen kaynak türü | context.resourceType | data.context.resourceType |
|  Etkilenen kaynak kaynak kimliği | context.resourceId | data.context.resourceId |
| Portal kaynak Özet sayfasında doğrudan bağlantı | context.portalLink | data.context.portalLink|
| Web kancası veya mantıksal uygulama için geçirilecek özel yük alanları | özellikler |Data.Properties |

Gördüğünüz gibi her iki yüklerini benzerdir. Aşağıdaki bölümde, örnek logic apps hakkında ayrıntılı bilgi ve yeni uyarılar için bildirim yükü ayrıştırmak için örnek bir runbook sahiptir.

## <a name="using-a-logic-app-that-receives-a-metric-alert-notification"></a>Ölçüm uyarı bildirim aldığı bir mantıksal uygulama kullanma

Logic apps ile klasik uyarıları kullanıyorsanız, mantıksal uygulamanız yeni ölçüm uyarıları yükü ayrıştırılamıyor değiştirmek gerekir.

1. Yeni bir mantıksal uygulama oluşturun.

2. "Azure İzleyici – ölçümleri uyarı işleyicisi" şablonu kullanın. Bu şablonda bir **HTTP isteği** içindeki uygun şemayı tanımlanan tetikleyici

    ![mantıksal uygulama şablonunu](media/alerts-migration/logic-app-template.png "ölçüm uyarı şablonu")

3. İşlem mantığınızı barındırmak için bir eylem ekleme.

## <a name="using-an-automation-runbook-that-receives-a-metric-alert-notification"></a>Ölçüm uyarı bildirim aldığı bir Otomasyon runbook'u kullanma

Aşağıdaki örnek, hem Klasik ölçüm uyarı kuralları hem de yeni ölçüm uyarı kuralları için yüklerini ayrıştırabilen runbook'unuzda kullanılabilecek PowerShell kodu sağlar.

```PS
## Sample PowerShell code to be used in a runbook to handle parsing of both classic and new metric alerts

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

    # Identify if the alert triggering the runbook is a classic metric alert or a new metric alert (depends on the payload schema).
    $schemaId = $WebhookBody.schemaId
    Write-Verbose "schemaId: $schemaId" -Verbose
    if ($schemaId -eq "AzureMonitorMetricAlert") {

        # This is the new Metric Alert schema
        $AlertContext = [object] ($WebhookBody.data).context
        $status = ($WebhookBody.data).status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.allOf[0].metricName
        $metricValue = $AlertContext.condition.allOf[0].metricValue
        $threshold = $AlertContext.condition.allOf[0].threshold
        $timeAggregation = $AlertContext.condition.allOf[0].timeAggregation
    }
    elseif ($schemaId -eq $null) {
        # This is the classic Metric Alert schema
        $AlertContext = [object] $WebhookBody.context
        $status = $WebhookBody.status

        # Parse fields related to alert rule condition
        $metricName = $AlertContext.condition.metricName
        $metricValue = $AlertContext.condition.metricValue
        $threshold = $AlertContext.condition.threshold
        $timeAggregation = $AlertContext.condition.timeAggregation
    }
    else {
        # The schema is not either a classic metric alert or a new metric alert
        Write-Error "The alert data schema - $schemaId - is not supported."
    }

    #parse fields related to resource affected
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

Bir VM içinde bir uyarı tetiklendiğinde durduran bir runbook'un tam bir örnek görmek [Azure Otomasyonu belgeleri](https://docs.microsoft.com/azure/automation/automation-create-alert-triggered-runbook).

## <a name="partner-integration-via-webhooks"></a>Web kancaları aracılığıyla iş ortağı tümleştirmesi

Çoğu [Klasik uyarılar ile Birleşen İş Ortaklarımızın](https://docs.microsoft.com/azure/azure-monitor/platform/partners) kendi tümleştirmeler aracılığıyla yeni ölçüm uyarılarının zaten desteklemektedir. Zaten yeni ölçüm uyarılarla çalışma bilinen tümleştirmeler aşağıda listelenmiştir.

- [PagerDuty](https://www.pagerduty.com/docs/guides/azure-integration-guide/)
- [OpsGenie](https://docs.opsgenie.com/docs/microsoft-azure-integration)
- [Sıgnl4](https://www.signl4.com/blog/mobile-alert-notifications-azure-monitor/)

Burada listelenmeyen bir iş ortağı tümleştirmesi kullanıyorsanız, tümleştirme sağlayıcı ile Tümleştirmesi ile yeni ölçüm uyarıları çalışır durumda olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş aracını kullanma](alerts-using-migration-tool.md)
- [Geçiş Aracı nasıl çalıştığını anlamak](alerts-understand-migration.md)
