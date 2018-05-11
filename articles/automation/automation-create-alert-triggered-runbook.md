---
title: Bir Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın
description: Azure bir uyarı oluştuğunda çalıştırmak için runbook tetiklenemedi öğrenin.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 03/15/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 2a226a348df4f289dd68924e24b8d4b374a87766
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Bir Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın

Kullanabileceğiniz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) taban düzeyi ölçümlerini ve günlükleri çoğu Azure hizmetlerini izlemek için. Azure Otomasyonu runbook'ları kullanarak çağırabilirsiniz [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) veya uyarılar temelinde görevleri otomatikleştirmek için Klasik uyarıları kullanarak. Bu makalede nasıl yapılandırılacağı ve Uyarıları kullanarak bir runbook'u çalıştırmak gösterilmektedir.

## <a name="alert-types"></a>Uyarı türleri

Otomasyon runbook'ları ile üç uyarı türlerini kullanabilirsiniz:
* Klasik ölçüm uyarıları
* Etkinlik günlüğü uyarıları
* Gerçek zamanlı ölçüm uyarıları

Bir runbook bir uyarı çağırdığında, gerçek Web kancası için bir HTTP POST isteği çağrıdır. POST isteğini gövdesini uyarıyla ilgili yararlı özellikleri olan JSON biçiminin bir nesneyi içerir. Aşağıdaki tabloda her bir uyarı türü için yükü şema bağlantıları listeler:

|Uyarı  |Açıklama|Yükü şeması  |
|---------|---------|---------|
|[Klasik ölçüm Uyarısı](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Herhangi bir platform düzeyi ölçümü belirli bir koşulun karşıladığında bir bildirim gönderir. Örneğin, değeri **CPU %** bir VM üzerinde değerinden daha büyük **90** son 5 dakika.| [Sınıf ölçüm uyarı yükü şeması](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Etkinlik günlüğü Uyarısı](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Herhangi bir yeni olay Azure etkinlik günlüğünde belirli koşullar eşleştiğinde bir bildirim gönderir. Örneğin, bir `Delete VM` işlemi oluşuyor **myProductionResourceGroup** ile yeni bir Azure hizmet durumu olay gerçekleştiğinde bir **etkin** durumu görüntülenir.| [Etkinlik günlüğü uyarı yükü şeması](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Gerçek zamanlı ölçüm Uyarısı](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Bir veya daha fazla platform düzeyi ölçümlerini belirtilen koşulları karşıladığında ölçüm uyarıları daha hızlı bir bildirim gönderir. Örneğin, değeri **CPU %** bir VM üzerinde değerinden daha büyük **90**ve değeri **ağ içinde** değerinden daha büyük **500 MB** son 5 dakika.| [Gerçek zamanlı ölçüm uyarı yükü şeması](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Her uyarı türünü tarafından sağlanan verileri farklı olduğundan, her uyarı türünü farklı şekilde ele alınır. Sonraki bölümde, farklı türde bir uyarı işlemek için bir runbook'un nasıl oluşturulacağını öğrenin.

## <a name="create-a-runbook-to-handle-alerts"></a>Uyarıları yönetmek için bir runbook oluşturma

Uyarıları ile Otomasyon kullanmak için runbook'a geçirilir uyarı JSON yükü yönetir mantığı varsa bir runbook gerekir. Aşağıdaki örnek runbook bir Azure uyarıdan çağrılmalıdır. 

Önceki bölümde açıklandığı gibi her uyarı türünü farklı bir şema vardır. Komut dosyası, Web kancası verilerini alır `WebhookData` bir uyarıdan runbook giriş parametresi. Ardından, kodun hangi uyarı türünün kullanıldığını belirlemek için JSON yükü değerlendirir. 

Bu örnek, bir VM'den gelen bir uyarı kullanır. VM veri yükü alır ve VM durdurmak için bu bilgileri kullanır. Bağlantı Otomasyon hesabında runbook çalıştırdığı ayarlanması gerekir.

Runbook kullanan **AzureRunAsConnection** [farklı çalıştır hesabı](automation-create-runas-account.md) bir VM'ye karşı yönetim eylemi gerçekleştirmek için Azure kimlik doğrulaması yapmak için.

Adlı bir runbook oluşturmak için bu örneği kullanmak **Stop-AzureVmInResponsetoVMAlert**. PowerShell komut dosyasını değiştirin ve birçok farklı kaynaklarla kullanın.

1. Azure Otomasyonu hesabınızı gidin.
1. Altında **işlem OTOMASYONU**seçin **Runbook'lar**.
1. Runbook'ların listesinin başında seçin **runbook Ekle**. 
1. **Runbook ekle** sayfasında **Hızlı Oluştur**'u seçin.
1. Runbook, adı **Stop-AzureVmInResponsetoVMAlert**. Runbook türü için **PowerShell**. Ardından **Oluştur**’u seçin.  
1. Aşağıdaki PowerShell örnek içine kopyalamak **Düzenle** bölmesi. 

    ```powershell-interactive
    <#
    .SYNOPSIS
    This runbook stops a resource management VM in response to an Azure alert trigger.

    .DESCRIPTION
    This runbook stops a resource management VM in response to an Azure alert trigger.
    The input is alert data that has the information required to identify which VM to stop.
    
    DEPENDENCIES
    - The runbook must be called from an Azure alert via a webhook.
    
    REQUIRED AUTOMATION ASSETS
    - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
    - An Automation certificate asset called "AzureRunAsCertificate".

    .PARAMETER WebhookData
    Optional. (The user doesn't need to enter anything, but the service always passes an object.)
    This is the data that's sent in the webhook that's triggered from the alert.

    .NOTES
    AUTHOR: Azure Automation Team
    LASTEDIT: 2017-11-22
    #>

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

        # Get the info needed to identify the VM (depends on the payload schema).
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # The schema isn't supported.
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if ($status -eq "Activated")
        {
            $ResourceType = $AlertContext.resourceType
            $ResourceGroupName = $AlertContext.resourceGroupName
            $SubId = $AlertContext.subscriptionId
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Use this only if this is a resource management VM.
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is the VM.
                Write-Verbose "This is a resource management VM." -Verbose

                # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzureRmContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the VM.
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzureRmVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType isn't supported.
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated', so no action taken.
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```
1. Seçin **Yayımla** kaydetmek ve runbook'u yayımlamak için.

## <a name="create-an-action-group"></a>Bir eylem grubu oluştur

Bir eylem grubu, bir uyarı tarafından tetiklenen eylemler koleksiyonudur. Runbook'ları Eylem grupları ile kullanabileceğiniz birçok Eylemler biridir.

1. Azure portalında seçin **İzleyici** > **ayarları** > **Eylem grupları**.
1. Seçin **eylem Grup Ekle**ve ardından gerekli bilgileri girin:  
    1. İçinde **eylem grup adı** kutusunda, bir ad girin.
    1. İçinde **kısa ad** kutusunda, bir ad girin. Bu eylem Grup kullanarak bildirimler gönderildiğinde kısa adı yerine bir tam eylem grup adı kullanılır.
    1. **Abonelik** kutusunu geçerli aboneliğiniz ile otomatik olarak doldurulur. Eylem grubunu kaydedildiği abonelik budur.
    1. Eylem grubunu kaydedildiği kaynak grubu seçin.

Bu örnekte, iki eylem oluşturmak: bir runbook eylemi ve bir bildirim eylemi.

### <a name="runbook-action"></a>Runbook eylemi

Bir runbook eylemi eylem grubu oluşturmak için:

1. Altında **Eylemler**, için **eylem adı**, eylem için bir ad girin. İçin **eylem türü**seçin **Otomasyon Runbook'u**.
1. Altında **ayrıntıları**seçin **Ayrıntıları Düzenle**.  
1. Üzerinde **yapılandırma Runbook** sayfasında **Runbook kaynağı**seçin **kullanıcı**.  
1. Seçin, **abonelik** ve **Otomasyon hesabı**ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. İşiniz bittiğinde, seçin **Tamam**.

### <a name="notification-action"></a>Bildirim eylemi

Bir bildirim eylemi eylem grubu oluşturmak için:

1. Altında **Eylemler**, için **eylem adı**, eylem için bir ad girin. İçin **eylem türü**seçin **e-posta**.  
1. Altında **ayrıntıları** seçin, **Ayrıntıları Düzenle**.  
1. Üzerinde **e-posta** sayfasında, bildirim için kullanmak ve daha sonra seçmek için e-posta adresi girin **Tamam**. Bir eylem olarak bir e-posta adresi runbook yanı sıra ekleme yardımcı olur. Runbook başlatıldığında size bildirilir.  

    Eylem grubunuzun aşağıdaki görüntü gibi görünmelidir:

   ![Eylem Grup Sayfası Ekle](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Eylem grubu oluşturmak için seçin **Tamam**.

Bu eylem gruptaki kullanabilirsiniz [etkinlik günlüğü uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) ve [yakın gerçek zamanlı uyarılar](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) oluşturduğunuz.

## <a name="classic-alert"></a>Klasik Uyarısı

Klasik uyarılar ölçümleri temel alır ve Eylem grupları kullanmayın. Ancak, klasik bir uyarıya dayanan bir runbook eylemi ayarlayabilirsiniz. 

Klasik bir uyarı oluşturmak için:

1. **Ölçüm uyarısı ekle**’yi seçin.
1. Ölçüm Uyarınız ad **myVMCPUAlert**. Uyarı için kısa bir açıklama girin.
1. Ölçüm Uyarı koşulu için seçin **büyük**. İçin **eşik** değeri, select **10**. İçin **süresi** değeri, select **son beş dakika içinde**.
1. Altında **ele eylem**seçin **bu uyarıdan Runbook'a**.
1. Üzerinde **yapılandırma Runbook** sayfası için **Runbook kaynağı**seçin **kullanıcı**. Otomasyon hesabınızı seçin ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook. **Tamam**’ı seçin.
1. Uyarı kuralı kaydetmeyi seçin **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

* Bir Web kancası kullanarak bir Otomasyon runbook'u başlatma hakkında daha fazla bilgi için bkz: [bir Web kancası bir runbook başlatın](automation-webhooks.md).
* Bir runbook'u başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz: [runbook başlatma](automation-starting-a-runbook.md).
* Bir etkinlik günlüğü uyarı oluşturmayı öğrenmek için bkz: [etkinlik günlüğü uyarı oluşturma](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Yakın gerçek zamanlı uyarı oluşturmayı öğrenmek için bkz: [Azure portalında bir uyarı kuralı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).
