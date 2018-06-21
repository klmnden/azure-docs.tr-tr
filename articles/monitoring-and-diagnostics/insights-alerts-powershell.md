---
title: Azure Hizmetleri için Klasik uyarıları oluşturmak için PowerShell kullanın | Microsoft Docs
description: E-postalar veya bildirimleri tetiklemek veya belirttiğiniz koşullar karşılandığında Web sitesine URL'lerini (Web kancaları) ya da Otomasyon çağırın.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/28/2018
ms.author: robb
ms.component: alerts
ms.openlocfilehash: b08a8f66add45d64085ac05605fe3c7d7f91b705
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36286208"
---
# <a name="use-powershell-to-create-alerts-for-azure-services"></a>Azure Hizmetleri için uyarı oluşturma için PowerShell kullanma
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

> [!NOTE]
> Bu makalede, eski classic ölçüm uyarıları oluşturmayı açıklar. Azure İzleyici destekler [yeni, daha iyi ölçüm uyarıları](monitoring-near-real-time-metric-alerts.md). Bu uyarılar, birden çok ölçümleri izleyin ve boyutlu ölçümleri uyarmak için izin verebilirsiniz. Yeni ölçüm uyarılar için PowerShell desteği yakında geliyor.
>
>

Bu makalede PowerShell kullanarak Azure Klasik ölçüm uyarıları ayarlamak gösterilmiştir.  

Uyarılar için Azure hizmetlerinizi ölçümleri temel alabilir veya Azure'da meydana gelen olayları için uyarı alabilirsiniz.

* **Ölçüm değerleri**: herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler zaman ilk koşul ve ardından zaman bu koşulu artık karşılanmıyor.    
* **Etkinlik günlüğü olaylarını**: bir uyarıyı tetiklemek *her* olay veya belirli olaylar meydana geldiğinde. Etkinlik günlüğü Uyarıları hakkında daha fazla bilgi için bkz: [etkinlik günlüğü uyarıları (Klasik) oluşturmak](monitoring-activity-log-alerts.md).

Tetikler olduğunda aşağıdaki görevleri gerçekleştirmek için Klasik bir ölçüm uyarı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönderin.
* E-posta, belirttiğiniz ek e-posta adreslerine gönder.
* Bir Web kancası çağırın.
* (Yalnızca Azure portalından) Azure bir runbook'un yürütülmesi başlatın.

Yapılandırın ve aşağıdaki konumlardan uyarı kuralları hakkında bilgi alın: 

* [Azure portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Azure komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Ek bilgi için her zaman yazabilirsiniz ```Get-Help``` ilgili yardıma ihtiyacınız PowerShell komutunu ve ardından.

## <a name="create-alert-rules-in-powershell"></a>PowerShell'de uyarı kuralları oluşturma
1. Azure'da oturum açın.   

    ```PowerShell
    Connect-AzureRmAccount

    ```
2. Kullanılabilen Aboneliklerin listesini alın. Doğru aboneliği çalıştığınızı doğrulayın. Değilse, doğru abonelik çıktısını kullanarak alma `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Aşağıdaki komutu kullanarak bir kaynak grubu üzerinde mevcut kurallar listesi:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Bir kural oluşturmak için birkaç önemli bilgi parçasından önce olması gerekir.

    - **Kaynak kimliği** için uyarı ayarlamak istediğiniz kaynak için.
    - **Ölçüm tanımlarını** bu kaynak için kullanılabilir.

     Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğunu varsayarsak Portalı'nda seçin. Ardından sonraki dikey penceresinde de **ayarları** bölümünde, select **özellikleri**. **Kaynak Kimliği** sonraki dikey bir alandır. 
     
     Kaynak kimliği kullanarak da elde edebilirsiniz [Azure kaynak Gezgini](https://resources.azure.com/).

     Bir web uygulaması için bir örnek kaynak kimliği aşağıdadır:

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Kullanabileceğiniz `Get-AzureRmMetricDefinition` belirli bir kaynak için tüm ölçüm açıklamalarının listesini görüntülemek için:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Aşağıdaki örnek ölçüm adı ve bu ölçümü için birim içeren bir tablo oluşturur:

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Get-AzureRmMetricDefinition için kullanılabilir seçenekleri tam bir listesini almak için Çalıştır `Get-Help Get-AzureRmMetricDefinition -Detailed`.
5. Aşağıdaki örnek bir Web sitesi kaynakta bir uyarı ayarlar. 5 dakikada bir ve yeniden zaman 5 dakika boyunca hiçbir trafik aldığı için herhangi bir trafik tutarlı bir şekilde aldığında uyarı tetikler.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Bir Web kancası oluşturma veya uyarıyı tetikleyen e-posta göndermek için önce e-posta veya Web kancası oluşturun. Hemen kural oluşturma aşağıdaki örnekte gösterildiği gibi daha sonra Eylemler etiketi. Web kancası veya e-postaları önceden oluşturduğunuz kurallar ile ilişkilendiremezsiniz.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Uyarılarınızı düzgün şekilde oluşturulduğunu doğrulamak için ayrı ayrı kuralları arayın.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Uyarılarınızı silin. Bu komutlar, bu makalede daha önce oluşturulan kuralları silin.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md), toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Öğrenme [Web kancalarını uyarıları yapılandırmanıza](insights-webhooks-alerts.md).
* Öğrenme [etkinlik günlüğü olayları uyarıları yapılandırmak](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Otomasyon çalışma kitabı](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlüklerinin toplanması genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
