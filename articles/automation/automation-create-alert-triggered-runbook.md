---
title: Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın
description: Azure bir uyarı ortaya çıktığında çalıştırmak için bir runbook tetikleme hakkında bilgi edinin.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 09/18/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 88fe7740170638e9e0d7398a02dcf83ab81f6ffc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61073869"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Azure Otomasyonu runbook'u tetiklemek için bir uyarı kullanın

Kullanabileceğiniz [Azure İzleyici](../azure-monitor/overview.md?toc=%2fazure%2fautomation%2ftoc.json) temel düzeyde ölçümlerini ve günlüklerini çoğu Azure Hizmetleri için izleme. Azure Otomasyonu runbook'ları kullanarak çağırabilirsiniz [Eylem grupları](../azure-monitor/platform/action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) veya uyarılara göre görevleri otomatikleştirmek için Klasik uyarıları kullanarak. Bu makalede uyarıları kullanarak bir runbook'u çalıştırma ve yapılandırma gösterilmektedir.

## <a name="alert-types"></a>Uyarı türleri

Otomasyon runbook'ları ile üç uyarı türlerini kullanabilirsiniz:
* Klasik ölçüm uyarıları
* Etkinlik günlüğü uyarıları
* Gerçek zamanlı ölçüm uyarıları

Uyarı runbook çağırdığında, gerçek bir Web kancası HTTP POST isteği çağrısıdır. POST isteğinin gövdesi biçiminin JSON nesnesini uyarıyla ilgili yararlı özellikleri içerir. Aşağıdaki tabloda, her uyarı türü için yükü şema bağlantıları listeler:

|Uyarı  |Açıklama|Yükü şeması  |
|---------|---------|---------|
|[Klasik ölçüm Uyarısı](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Herhangi bir platform düzeyi ölçümü belirli bir koşulu karşıladığında bir bildirim gönderir. Örneğin, değeri **CPU %** bir VM'de büyüktür **90** son 5 dakika.| [Sınıf ölçüm uyarı yükü şeması](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Etkinlik günlüğü Uyarısı](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Azure etkinlik günlüğü her yeni olay belirli koşullar eşleştiğinde bir bildirim gönderir. Örneğin, bir `Delete VM` işlemi oluşuyor **myProductionResourceGroup** veya yeni bir Azure hizmet durumu olay ile bir **etkin** durumu görüntülenir.| [Etkinlik günlüğü uyarısı yükü şeması](../azure-monitor/platform/activity-log-alerts-webhook.md)        |
|[Gerçek zamanlı ölçüm Uyarısı](../azure-monitor/platform/alerts-metric-near-real-time.md?toc=%2fazure%2fautomation%2ftoc.json)    |Bir veya daha fazla platform düzeyi ölçümleri belirtilen koşulları karşıladığında bir bildirim daha hızlı ölçüm uyarıları gönderir. Örneğin, değeri **CPU %** bir VM'de büyüktür **90**ve değeri **ağ içinde** büyüktür **500 MB** için geçmiş 5 dakika.| [Gerçek zamanlı ölçüm uyarı yükü şeması](../azure-monitor/platform/alerts-webhooks.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Her uyarı türü tarafından sağlanan veri farklı olduğundan, her uyarı türünün farklı şekilde ele alınır. Sonraki bölümde, farklı uyarı türleri işlemek için bir runbook'un nasıl oluşturulacağını öğrenin.

## <a name="create-a-runbook-to-handle-alerts"></a>Uyarıları yönetmek için bir runbook oluşturma

Uyarıları ile Otomasyon kullanmak için runbook'a geçirilen uyarı bir JSON yükü yöneten mantığı içeren bir runbook'u gerekir. Aşağıdaki örnek runbook, Azure uyarıdan çağrılmalıdır.

Önceki bölümde açıklandığı gibi her uyarı türü farklı bir şeması vardır. Web kancası verilerini betikte `WebhookData` bir uyarıdan runbook giriş parametresi. Ardından, betiği hangi uyarı türünün kullanıldığını belirlemek için JSON yükü olarak değerlendirilir.

Bu örnek, bir VM'den bir uyarı kullanır. Yükten VM verileri alır ve ardından sanal Makineyi durdurmak için bu bilgileri kullanır. Bağlantı Otomasyon hesabında runbook çalıştırıldığı ayarlanması gerekir. Uyarılar, runbook'ları tetikleme kullanırken, tetiklenen runbook'u uyarıda durumunu denetlemek önemlidir. Runbook, uyarıyı durumu her değiştiğinde tetikler. Uyarılar, birden fazla durum olması, iki en yaygın durumlar `Activated` ve `Resolved`. Bu durumda, runbook mantığının runbook'unuzu birden çok kez çalıştırmaz emin olmak için kontrol edin. Bu makaledeki örnek aranacak gösterilmektedir `Activated` yalnızca uyarır.

Runbook kullanan **AzureRunAsConnection** [Run As hesabı](automation-create-runas-account.md) bir VM'ye karşı yönetim eylemi gerçekleştirmek için Azure ile kimlik doğrulaması.

Adlı bir runbook oluşturmak için bu örneği kullanmak **Stop-AzureVmInResponsetoVMAlert**. PowerShell betiğini değiştirebilir ve birçok farklı kaynaklar ile kullanın.

1. Azure Otomasyon hesabınıza gidin.
1. Altında **süreç OTOMASYONU**seçin **runbook'ları**.
1. Runbook'ların listenin en üstünde seçin **runbook Ekle**. 
1. **Runbook ekle** sayfasında **Hızlı Oluştur**'u seçin.
1. Runbook adı olarak **Stop-AzureVmInResponsetoVMAlert**. Runbook türü için **PowerShell**. Ardından **Oluştur**’u seçin.  
1. Aşağıdaki PowerShell örneği içine kopyalayın **Düzenle** bölmesi. 

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
1. Seçin **Yayımla** kaydedin ve runbook'u yayımlayamadı.

## <a name="create-an-action-group"></a>Bir eylem grubu oluştur

Bir eylem grubu, bir uyarı tarafından tetiklenen eylemler koleksiyonudur. Runbook'ları ile Eylem grupları kullanabileceğiniz birçok eylemi biridir.

1. Azure portalında **İzleyici** > **ayarları** > **Eylem grupları**.
1. Seçin **eylem grubu Ekle**ve ardından gerekli bilgileri girin:  
    1. İçinde **eylem grubu adı** kutusunda, bir ad girin.
    1. İçinde **kısa ad** kutusunda, bir ad girin. Bu eylem grubu kullanarak bildirimleri gönderilirken kısa adı bir tam eylem grubu adı yerine kullanılır.
    1. **Abonelik** kutusu, geçerli aboneliğiniz ile otomatik olarak doldurulur. Bu eylem grubu kaydedildiği aboneliktir.
    1. Eylem grubu kaydedildiği kaynak grubunu seçin.

Bu örnekte, iki eylem oluştur: bir runbook eylemi ve bildirim eylemi.

### <a name="runbook-action"></a>Runbook eylemini

Bir runbook eylemi içinde eylem grubunu oluşturmak için:

1. Altında **eylemleri**, için **eylem adı**, eylem için bir ad girin. İçin **eylem türü**seçin **Otomasyon Runbook'u**.
1. Altında **ayrıntıları**seçin **Ayrıntıları Düzenle**.  
1. Üzerinde **Runbook yapılandırma** sayfasındaki **Runbook kaynağı**seçin **kullanıcı**.  
1. Seçin, **abonelik** ve **Otomasyon hesabı**ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook.  
1. İşlemi tamamladığınızda, seçin **Tamam**.

### <a name="notification-action"></a>Bildirim eylemi

Bildirim eylemi içinde eylem grubunu oluşturmak için:

1. Altında **eylemleri**, için **eylem adı**, eylem için bir ad girin. İçin **eylem türü**seçin **e-posta**.  
1. Altında **ayrıntıları** seçin, **Ayrıntıları Düzenle**.  
1. Üzerinde **e-posta** sayfasında, bildirim için kullanmak ve ardından e-posta adresi girin **Tamam**. Bir eylemi olarak runbook'u ek olarak bir e-posta adresi ekleme yararlıdır. Runbook'u başlattığımızda size bildirilir.  

    Eylem grubu, şu resimdeki gibi görünmelidir:

   ![Eylem grubu Sayfası Ekle](./media/automation-create-alert-triggered-runbook/add-action-group.png)
1. Eylem grubunu oluşturmak için Seç **Tamam**.

Bu eylem grubuna kullanabileceğiniz [etkinlik günlüğü uyarıları](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) ve [neredeyse gerçek zamanlı uyarılar](../azure-monitor/platform/alerts-overview.md?toc=%2fazure%2fautomation%2ftoc.json) oluşturduğunuz.

## <a name="classic-alert"></a>Klasik bir uyarı

Klasik uyarılar ölçümlere göre ve Eylem grupları kullanmayın. Ancak, klasik bir uyarıya dayanan bir runbook eylemi ayarlayabilirsiniz. 

Klasik bir uyarı oluşturmak için:

1. **Ölçüm uyarısı ekle**’yi seçin.
1. Ölçüm uyarınızı ad **myVMCPUAlert**. Uyarının kısa bir açıklama girin.
1. Ölçüm Uyarı koşulu için seçin **büyüktür**. İçin **eşiği** değeri, select **10**. İçin **süresi** değeri, select **son beş dakikadan**.
1. Altında **harekete**seçin **bu uyarıdan runbook Çalıştır**.
1. Üzerinde **Runbook yapılandırma** sayfası için **Runbook kaynağı**seçin **kullanıcı**. Otomasyon hesabınızı seçin ve ardından **Stop-AzureVmInResponsetoVMAlert** runbook. **Tamam**’ı seçin.
1. Uyarı kuralını kaydetmek için seçmeniz **Tamam**.

## <a name="next-steps"></a>Sonraki adımlar

* Web kancası kullanarak bir Otomasyon runbook'unu başlatma hakkında daha fazla bilgi için bkz. [Web kancasından runbook başlatma](automation-webhooks.md).
* Bir runbook başlatmak için çeşitli yollar hakkında daha fazla ayrıntı için bkz: [runbook başlatma](automation-starting-a-runbook.md).
* Bir etkinlik günlüğü uyarısı oluşturmayı öğrenmek için bkz: [etkinlik günlüğü uyarıları oluşturma](../azure-monitor/platform/activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json).
* Neredeyse gerçek zamanlı bir uyarı oluşturma hakkında bilgi edinmek için [Azure portalında bir uyarı kuralı oluşturma](../azure-monitor/platform/alerts-metric.md?toc=/azure/azure-monitor/toc.json).

