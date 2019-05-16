---
title: Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın
description: Azure bir uyarı ortaya çıktığında çalıştırmak için bir runbook tetikleme hakkında bilgi edinin.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/29/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8a1ae906a72d781f638fb171a409b860ffa6d501
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2019
ms.locfileid: "65517698"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın

Kullanabileceğiniz [Azure İzleyici](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) temel düzeyde ölçümlerini ve günlüklerini çoğu Azure Hizmetleri için izleme. Azure Otomasyonu runbook'ları kullanarak çağırabilirsiniz [Eylem grupları](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) veya uyarılara göre görevleri otomatikleştirmek için Klasik uyarıları kullanarak. Bu makalede uyarıları kullanarak bir runbook'u çalıştırma ve yapılandırma gösterilmektedir.

## <a name="alert-types"></a>Uyarı türleri

Otomasyon runbook'ları ile üç uyarı türlerini kullanabilirsiniz:

* Yaygın uyarılar
* Etkinlik günlüğü uyarıları
* Gerçek zamanlı ölçüm uyarıları

> [!NOTE]
> Ortak uyarı şema için uyarı bildirimleri Azure tüketim deneyimi bugün standart hale getirir. Tarihsel olarak, üç uyarı türlerini azure'da bugün (ölçümü, günlük ve etkinlik günlüğü) kendi e-posta şablonları, Web kancası şemaları vb. kalmışlardır. Daha fazla bilgi için bkz: [ortak uyarı şeması](../azure-monitor/platform/alerts-common-schema.md)

Uyarı runbook çağırdığında, gerçek bir Web kancası HTTP POST isteği çağrısıdır. POST isteğinin gövdesi biçiminin JSON nesnesini uyarıyla ilgili yararlı özellikleri içerir. Aşağıdaki tabloda, her uyarı türü için yükü şema bağlantıları listeler:

|Uyarı  |Açıklama|Yükü şeması  |
|---------|---------|---------|
|[Ortak uyarı](../azure-monitor/platform/alerts-common-schema.md?toc=%2fazure%2fautomation%2ftoc.json)|Bugün azure'da uyarı bildirimleri tüketim deneyimi standartlaştırır ortak uyarı şeması.|[Ortak uyarı yükü şeması](../azure-monitor/platform/alerts-common-schema-definitions.md?toc=%2fazure%2fautomation%2ftoc.json#sample-alert-payload)|
|[Etkinlik günlüğü Uyarısı](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Azure etkinlik günlüğü her yeni olay belirli koşullar eşleştiğinde bir bildirim gönderir. Örneğin, bir `Delete VM` işlemi oluşuyor **myProductionResourceGroup** veya yeni bir Azure hizmet durumu olay ile bir **etkin** durumu görüntülenir.| [Etkinlik günlüğü uyarısı yükü şeması](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Gerçek zamanlı ölçüm Uyarısı](../azure-monitor/platform/alerts-metric-near-real-time.md?toc=%2fazure%2fautomation%2ftoc.json)    |Bir veya daha fazla platform düzeyi ölçümleri belirtilen koşulları karşıladığında bir bildirim daha hızlı ölçüm uyarıları gönderir. Örneğin, değeri **CPU %** bir VM'de büyüktür **90**ve değeri **ağ içinde** büyüktür **500 MB** için geçmiş 5 dakika.| [Gerçek zamanlı ölçüm uyarı yükü şeması](../azure-monitor/platform/alerts-webhooks.md#payload-schema)          |

Her uyarı türü tarafından sağlanan veri farklı olduğundan, her uyarı türünün farklı şekilde ele alınır. Sonraki bölümde, farklı uyarı türleri işlemek için bir runbook'un nasıl oluşturulacağını öğrenin.

## <a name="create-a-runbook-to-handle-alerts"></a>Uyarıları yönetmek için bir runbook oluşturma

Uyarıları ile Otomasyon kullanmak için runbook'a geçirilen uyarı bir JSON yükü yöneten mantığı içeren bir runbook'u gerekir. Aşağıdaki örnek runbook, Azure uyarıdan çağrılmalıdır.

Önceki bölümde açıklandığı gibi her uyarı türü farklı bir şeması vardır. Web kancası verilerini betikte `WebhookData` bir uyarıdan runbook giriş parametresi. Ardından, betiği hangi uyarı türünün kullanıldığını belirlemek için JSON yükü olarak değerlendirilir.

Bu örnek, bir VM'den bir uyarı kullanır. Yükten VM verileri alır ve ardından sanal Makineyi durdurmak için bu bilgileri kullanır. Bağlantı Otomasyon hesabında runbook çalıştırıldığı ayarlanması gerekir. Uyarılar, runbook'ları tetikleme kullanırken, tetiklenen runbook'u uyarıda durumunu denetlemek önemlidir. Runbook, uyarıyı durumu her değiştiğinde tetikler. Uyarılar, birden fazla durum olması, iki en yaygın durumlar `Activated` ve `Resolved`. Bu durumda, runbook mantığının runbook'unuzu birden çok kez çalıştırmaz emin olmak için kontrol edin. Bu makaledeki örnek aranacak gösterilmektedir `Activated` yalnızca uyarır.

Runbook kullanan **AzureRunAsConnection** [Run As hesabı](automation-create-runas-account.md) bir VM'ye karşı yönetim eylemi gerçekleştirmek için Azure ile kimlik doğrulaması.

Adlı bir runbook oluşturmak için bu örneği kullanmak **Stop-AzureVmInResponsetoVMAlert**. PowerShell betiğini değiştirebilir ve birçok farklı kaynaklar ile kullanın.

1. Azure Otomasyon hesabınıza gidin.
2. Altında **süreç otomasyonu**seçin **runbook'ları**.
3. Runbook'ların listenin en üstünde seçin **+ bir runbook Oluştur**.
4. Üzerinde **Runbook Ekle** want **Stop-AzureVmInResponsetoVMAlert** runbook adı. Runbook türü için **PowerShell**. Ardından **Oluştur**’u seçin.  
5. Aşağıdaki PowerShell örneği içine kopyalayın **Düzenle** sayfası.

    ```powershell-interactive
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )
    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema)
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "azureMonitorCommonAlertSchema") {
            # This is the common Metric Alert schema (released March 2019)
            $Essentials = [object] ($WebhookBody.data).essentials
            # Get the first target only as this script doesn't handle multiple
            $alertTargetIdArray = (($Essentials.alertTargetIds)[0]).Split("/")
            $SubId = ($alertTargetIdArray)[2]
            $ResourceGroupName = ($alertTargetIdArray)[4]
            $ResourceType = ($alertTargetIdArray)[6] + "/" + ($alertTargetIdArray)[7]
            $ResourceName = ($alertTargetIdArray)[-1]
            $status = $Essentials.monitorCondition
        }
        elseif ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # Schema not supported
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if (($status -eq "Activated") -or ($status -eq "Fired"))
        {
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Determine code path depending on the resourceType
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is an Resource Manager VM
                Write-Verbose "This is an Resource Manager VM." -Verbose

                # Authenticate to Azure with service principal and certificate and set subscription
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the Resource Manager VM
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType not supported
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated' or 'Fired' so no action taken
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```

6. Seçin **Yayımla** kaydedin ve runbook'u yayımlayamadı.

## <a name="create-the-alert"></a>Uyarı Oluştur

Uyarıları, uyarı tarafından tetiklenen eylemler koleksiyonlarıdır eylem gruplarını kullanın. Runbook'ları ile Eylem grupları kullanabileceğiniz birçok eylemi biridir.

1. Otomasyon hesabınızı seçin **uyarılar** altında **izleme**.
1. Seçin **+ yeni uyarı kuralı**.
1. Tıklayın **seçin** altında **kaynak**. Üzerinde **bir kaynak seçin** sayfasında, VM'nize kapatıp uyarı seçin ve tıklayın **Bitti**.
1. Tıklayın **koşul Ekle** altında **koşul**. Örneğin, kullanmak istediğiniz sinyal seçin **CPU yüzdesi** tıklatıp **Bitti**.
1. Üzerinde **sinyal mantığını yapılandırma** girin, **eşik değeri** altında **uyarı mantığı**, tıklatıp **Bitti**.
1. Altında **Eylem grupları**seçin **Yeni Oluştur**.
1. Üzerinde **eylem grubu Ekle** sayfasında, eylem grubunuz bir ad ve kısa bir ad verin.
1. Eylemi bir ad verin. Eylem türü için **Otomasyon Runbook'u**.
1. Seçin **Ayrıntıları Düzenle**. Üzerinde **Runbook yapılandırma** sayfasındaki **Runbook kaynağı**seçin **kullanıcı**.  
1. Seçin, **abonelik** ve **Otomasyon hesabı**ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. Seçin **Evet** için **ortak uyarı şema etkinleştirme**.
1. Eylem grubunu oluşturmak için Seç **Tamam**.

    ![Eylem grubu Sayfası Ekle](./media/automation-create-alert-triggered-runbook/add-action-group.png)

    Bu eylem grubuna kullanabileceğiniz [etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) ve [neredeyse gerçek zamanlı uyarılar](../azure-monitor/platform/alerts-overview.md?toc=%2fazure%2fautomation%2ftoc.json) oluşturduğunuz.

1. Altında **uyarı ayrıntıları**, bir uyarı kuralı adı ve açıklama ekleyin ve tıklayın **uyarı kuralı oluştur**.

## <a name="next-steps"></a>Sonraki adımlar

* Web kancası kullanarak bir Otomasyon runbook'unu başlatma hakkında daha fazla bilgi için bkz. [Web kancasından runbook başlatma](automation-webhooks.md).
* Bir runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz: [runbook başlatma](automation-starting-a-runbook.md).
* Bir etkinlik günlüğü uyarısı oluşturmayı öğrenmek için bkz: [etkinlik günlüğü uyarıları oluşturma](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Neredeyse gerçek zamanlı bir uyarı oluşturma hakkında bilgi edinmek için [Azure portalında bir uyarı kuralı oluşturma](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json).
