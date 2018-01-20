---
title: "Bir Otomasyon runbook'u ile Azure uyarılara yanıt | Microsoft Docs"
description: "Azure uyarılar ortaya çıktığında çalıştırmak için runbook tetiklenemedi öğrenin."
services: automation
keywords: 
author: georgewallace
ms.author: gwallace
ms.date: 01/11/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 486a3c80c9d6c19ca0a7ba7d4e3cdcde927053b9
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="respond-to-azure-alerts-with-an-automation-runbook"></a>Bir Otomasyon runbook'u ile Azure uyarılara yanıt

[Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md?toc=%2fazure%2fautomation%2ftoc.json) Microsoft Azure içindeki çoğu hizmetler için temel düzeyi ölçümlerini ve günlükleri sağlar. Azure Otomasyonu runbook'ları kullanarak çağrılabilir [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md?toc=%2fazure%2fautomation%2ftoc.json) veya uyarılar temelinde görevleri otomatikleştirmek için Klasik uyarılardan. Bu makalede yapılandırma ve bu uyarıları dışına dayalı bir runbook çalıştırma gösterilmektedir.

## <a name="alert-types"></a>Uyarı türleri

Runbook'ları, tüm üç uyarı türleri üzerinde desteklenen eylemlerdir. Uyarı runbook aradığında, gerçek Web kancası için bir HTTP POST isteği çağrıdır. POST isteğini gövdesini uyarıyla ilgili yararlı özellikleri içeren bir JSON biçiminin nesnesi içerir. Aşağıdaki tabloda her bir uyarı türü için yükü şema bağlantıları içerir:

|Uyarı  |Açıklama|Yükü şeması  |
|---------|---------|---------|
|[Klasik ölçüm Uyarısı](../monitoring-and-diagnostics/insights-alerts-portal.md?toc=%2fazure%2fautomation%2ftoc.json)    |Herhangi bir platform düzeyi ölçümü belirli bir koşulun karşıladığında bir bildirim alın (örneğin, bir VM CPU % son 5 dakika boyunca 90'dan büyük olduğu).| [Yükü şeması](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)         |
|[Etkinlik günlüğü Uyarısı](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Tüm yeni olay Azure etkinlik günlüğü içinde belirli koşullar (örneğin, "VM silme" işlemi içinde myProductionResourceGroup meydana geldiğinde veya "Etkin" olarak durumu ile yeni bir hizmet durumu olay görüntülendiğinde) eşleştiğinde bir bildirim alırsınız.| [Yükü şeması](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)        |
|[Gerçek zamanlı ölçüm Uyarısı](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)    |Bir veya daha fazla platform düzeyi ölçümlerini belirtilen koşulları karşıladığında ölçüm uyarıları daha hızlı bildirimi (örneğin, bir VM CPU % 90'dan büyük ve ağ içinde 500 MB'tan fazla son 5 dakika olarak).| [Yükü şeması](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md?toc=%2fazure%2fautomation%2ftoc.json#payload-schema)          |

Her uyarı tarafından sağlanan verileri farklı olduğundan, her uyarı farklı şekilde ele alınması gerekir. Sonraki bölümde, bu farklı türde bir uyarı işlemek için bir runbook'un nasıl oluşturulacağını öğrenin.

## <a name="create-a-runbook-to-handle-alerts"></a>Uyarıları yönetmek için bir runbook oluşturma

Uyarıları ile Otomasyon kullanmak için uyarı runbook'a geçirilir JSON yükü yönetmek için mantığı içeren bir runbook gerekir. Aşağıdaki örnek runbook bir Azure uyarıdan çağrılmalıdır. Önceki bölümde açıklandığı gibi her uyarı türü farklı bir şema türü. Komut dosyası, Web kancası verilerini alır `WebhookData` runbook giriş parametresi bir uyarıdan ve hangi uyarı türünün kullanıldığını belirlemek için JSON yükü değerlendirir. Aşağıdaki örnekte bir uyarıdan bir VM üzerinde kullanılabilir. VM veri yükü alır ve VM durdurmak için bu bilgileri kullanır. Otomasyon runbook olduğu hesap çalıştırdığınız bağlantı ayarlanması gerekir.

Runbook kullanan **AzureRunAsConnection** [farklı çalıştır hesabı](automation-create-runas-account.md) bir VM'ye karşı yönetim eylemi gerçekleştirmek için Azure kimlik doğrulaması için.

Birçok farklı kaynaklar ile kullanmak için aşağıdaki PowerShell betiğini değiştirilebilir.

Aşağıdaki örnek alın ve adlı bir runbook oluşturmak **Stop-AzureVmInResponsetoVMAlert** örnek PowerShell ile.

1. Otomasyon hesabınızı açın.
1. **SÜREÇ OTOMASYONU**'nın altındaki **Runbook'lar** öğesine tıklayın. Runbook'ların listesi görüntülenir.
1. Listenin en üstünde yer alan **Runbook ekle** düğmesine tıklayın. Üzerinde **Runbook Ekle sayfası**seçin **hızlı Oluştur**.
1. Runbook için "Stop-AzureVmInResponsetoVMAlert" girin **adı**seçip **PowerShell** için **Runbook türü**. **Oluştur**’a tıklayın.
1. Aşağıdaki PowerShell örneğinde düzenleme bölmesine kopyalayın. Tıklatın **Yayımla** kaydetmek ve runbook'u yayımlamak için.

```powershell-interactive
<#
.SYNOPSIS
  This runbook will stop a resource management VM in response to an Azure alert trigger.

.DESCRIPTION
  This runbook will stop an resource management VM in response to an Azure alert trigger.
  Input is alert data with information needed to identify which VM to stop.
  
  DEPENDENCIES
  - The runbook must be called from an Azure alert via a webhook.
  
  REQUIRED AUTOMATION ASSETS
  - An Automation connection asset called "AzureRunAsConnection" that is of type AzureRunAsConnection.
  - An Automation certificate asset called "AzureRunAsCertificate".

.PARAMETER WebhookData
   Optional (user does not need to enter anything, but the service will always pass an object)
   This is the data that is sent in the webhook that is triggered from the alert.

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
    # Get the data object from WebhookData
    $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Get the info needed to identify the VM (depends on the payload schema)
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
        # Schema not supported
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

        # Do work only if this is a resource management VM
        if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
        {
            # This is the VM
            Write-Verbose "This is a resource management VM." -Verbose

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

            # Stop the VM
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
        # The alert status was not 'Activated' so no action taken
        Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
    }
}
else {
    # Error
    Write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="create-an-action-group"></a>Bir eylem grubu oluştur

Bir eylem grubu, bir uyarı dışına tabanlı gerçekleştirilen eylemler koleksiyonudur. Runbook'ları Eylem grupları ile kullanılabilen birçok eylemlerini biridir.

1. Portalı'nda seçin **İzleyici**.

1. Seçin **Eylem grupları** altında **ayarları**.

1. Seçin **eylem Grup Ekle**ve alanları doldurun.

1. Eylem grup adı kutusuna bir ad girin ve kısa ad kutusuna bir ad girin. Bu grubun kullanarak bildirimler gönderildiğinde kısa adı yerine bir tam eylem grup adı kullanılır.

1. Abonelik kutusunu autofills geçerli aboneliğiniz ile. Bu abonelik eylem grubunu kaydedildiği adrestir.
Eylem grubunu kaydedildiği kaynak grubu seçin.

Bu örnekte, iki eylem ve runbook eylemini bir bildirim eylemi oluşturun.

### <a name="runbook-action"></a>Runbook eylemi

Aşağıdaki adımlar bir runbook eylemi eylem grubu içinde oluşturur.

1. Altında **Eylemler**, eylem bir ad verin.

1. Seçin **Otomasyon Runbook'u** için **eylem türü**.

1. Altında **ayrıntıları** seçin, **Ayrıntıları Düzenle**

1. Üzerinde **yapılandırma Runbook** sayfasında, **kullanıcı** altında **Runbook kaynağı**.

1. Seçin, **abonelik** ve **Otomasyon hesabı**ve son olarak "Stop-AzureVmInResponsetoVMAlert" olarak adlandırılan önceki adımda oluşturduğunuz runbook'u seçin.

1. İşiniz bittiğinde, tıklatın **Tamam**.

### <a name="notification-action"></a>Bildirim eylemi

Aşağıdaki adımlar bir bildirim eylemi eylem grubu içinde oluşturur.

1. Altında **Eylemler**, eylem bir ad verin.

1. Seçin **e-posta** için **eylem türü**.

1. Altında **ayrıntıları** seçin, **Ayrıntıları Düzenle**

1. Üzerinde **e-posta** sayfasında, bildirim ve e-posta adresi girin **Tamam**. Runbook başlatılırken bildirilir gibi bir eylem olarak bir e-posta adresi gibi runbook ekleme yardımcı olur. Eylem grubunuzun aşağıdaki görüntü gibi görünmelidir:

   ![Eylem Grup Sayfası Ekle](./media/automation-create-alert-triggered-runbook/add-action-group.png)

1. Tıklatın **Tamam** eylem grubu oluşturmak için.

Oluşturulan eylem grubuyla oluşturduğunuz [etkinlik günlüğü uyarıları](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json) veya [yakın gerçek zamanlı uyarılar](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal) ve oluşturduğunuz eylem grubu kullanın.

## <a name="classic-alert"></a>Klasik Uyarısı

Klasik uyarıları ölçümleri temel alır ve kullanım Eylem grupları ve bunun runbook eylemlerini sahip üzerlerinde tabanlı. Klasik bir uyarı oluşturmak için aşağıdaki adımları kullanın:

1. Seçin **+ ölçüm uyarı Ekle**.

1. Ölçüm Uyarınız 'myVMCPUAlert' adlandırın ve uyarı için kısa bir açıklama sağlayın.

1. Koşul için ölçüm uyarı 'Değerinden' olarak ayarlayın, Eşiği '10' ayarlayın ve 'üzerinde son 5 dakika olarak' dönemi ayarlayın.

1. Altında **ele eylem**seçin **bu uyarıdan bir runbook çalıştırma**

1. Üzerinde **yapılandırma Runbook** sayfasında, **kullanıcı** için **Runbook kaynağı**. Otomasyon hesabınızı seçin ve Seç **Stop-AzureVmInResponsetoVMAlert** runbook. Tıklatın **Tamam**ve ardından **Tamam** kaydetmeyi yeniden uyarı kuralı.

## <a name="next-steps"></a>Sonraki adımlar

* Otomasyon runbook'ları ile Web kancalarını başlatma hakkında daha fazla bilgi için bkz: [bir Web kancası bir runbook başlatın](automation-webhooks.md)
* Bir runbook'u başlatmak için çeşitli yollar hakkında daha fazla bilgi için bkz [Runbook başlatma](automation-starting-a-runbook.md).
* Bir etkinlik günlüğü uyarı oluşturmayı öğrenmek için bkz: [etkinlik günlüğü uyarı oluşturma](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md?toc=%2fazure%2fautomation%2ftoc.json)
* Yakın gerçek zamanlı uyarı oluşturma hakkında bilgi için ziyaret [Azure portalıyla bir uyarı kuralı oluşturma](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md?toc=%2fazure%2fautomation%2ftoc.json#create-an-alert-rule-with-the-azure-portal).