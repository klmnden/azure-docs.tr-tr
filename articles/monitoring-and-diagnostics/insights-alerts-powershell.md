---
title: "Azure Hizmetleri - PowerShell için uyarı oluşturma | Microsoft Docs"
description: "Belirttiğiniz koşullar karşılandığında tetikleyici e-postalar, bildirimler, Web siteleri URL'leri (Web kancaları) ya da Otomasyon çağırın."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: d3fca8675c1f15b8fd0f952cfbf520f5c68478b3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a>Ölçüm uyarılar Azure İzleyicisi'nde Azure Hizmetleri - PowerShell oluşturabilir.
> [!div class="op_single_selector"]
> * [Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [CLI](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Genel Bakış
Bu makalede PowerShell kullanarak Azure ölçüm uyarılarını ayarlama gösterilmiştir.  

İzleme ölçümlerini ya da olayları, Azure hizmetlerinizi göre bir uyarı alabilirsiniz.

* **Ölçüm değerleri** -herhangi bir yönde atadığınız bir eşik değeri, belirtilen bir ölçüm kestiği olduğunda uyarı tetikler. Diğer bir deyişle, her ikisi de tetikler koşul ilk ve ardından daha sonra ne zaman, koşul artık karşılanıp zaman.    
* **Etkinlik günlüğü olaylarını** -bir uyarıyı tetiklemek *her* olay veya yalnızca belirli bir olaylar oluşur. Etkinlik günlüğü Uyarıları hakkında daha fazla bilgi edinmek için [burayı tıklatın](monitoring-activity-log-alerts.md)

Tetikler, aşağıdakileri yapmak için bir ölçüm uyarısı yapılandırabilirsiniz:

* Hizmet yöneticisini ve ortak Yöneticiler e-posta bildirimleri gönder
* Belirttiğiniz ek e-postalar için e-posta gönderin.
* bir Web kancası çağırın
* (yalnızca Azure portalından) Azure bir runbook'un yürütülmesi Başlat

Yapılandırma ve uyarı kuralları kullanma hakkında bilgi edinin

* [Azure portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [komut satırı arabirimi (CLI)](insights-alerts-command-line-interface.md)
* [Azure monitör REST API'si](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Ek bilgi için her zaman yazabilirsiniz ```Get-Help``` ve ardından hakkında Yardım istediğiniz PowerShell komutu.

## <a name="create-alert-rules-in-powershell"></a>PowerShell'de uyarı kuralları oluşturma
1. Azure'da oturum açın.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Bir listesini almak abonelikleri sahip olduğunuz kullanılabilir. Doğru aboneliği çalıştığını doğrulayın. Aksi takdirde, çıkışı kullanarak doğru olanı ayarlayın `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. Bir kaynak grubu üzerinde mevcut kurallar listelemek için aşağıdaki komutu kullanın:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. Bir kural oluşturmak için birkaç önemli bilgi parçasından önce olması gerekir.

  * **Kaynak kimliği** için uyarı ayarlamak istediğiniz kaynak için
  * **Ölçüm tanımlarını** kaynağı için kullanılabilir

     Kaynak Kimliği almanın bir yolu, Azure Portalı'nı kullanmaktır. Kaynak zaten oluşturulmuş olduğu varsayılarak, portalda seçin. Sonraki dikey penceresinde, ardından *özellikleri* altında *ayarları* bölümü. **Kaynak Kimliği** sonraki dikey bir alandır. Başka bir yolu kullanmaktır [Azure kaynak Gezgini](https://resources.azure.com/).

     Bir web uygulaması için bir örnek kaynak kimliği

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Kullanabileceğiniz `Get-AzureRmMetricDefinition` belirli bir kaynak için tüm ölçüm açıklamalarının listesini görüntülemek için.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Aşağıdaki örnek, ölçüm adı içeren bir tablo ve bu ölçümü için birim oluşturur.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Get-AzureRmMetricDefinition için kullanılabilir seçenekleri tam listesi kullanılabilir çalıştırarak `Get-Help Get-AzureRmMetricDefinition -Detailed`.
5. Aşağıdaki örnek bir web sitesi kaynakta bir uyarı ayarlar. 5 dakikada bir ve yeniden zaman 5 dakika boyunca hiçbir trafik aldığı için herhangi bir trafik tutarlı bir şekilde aldığında uyarı tetikler.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. Web kancası oluşturma veya uyarıyı tetikleyen e-posta göndermek için önce e-posta ve/veya Web kancası oluşturun. Ardından hemen kural daha sonra Eylemler etikete sahip ve aşağıdaki örnekte gösterildiği gibi oluşturun. Web kancası veya e-posta zaten kuralları PowerShell aracılığıyla oluşturulan ilişkilendiremezsiniz.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. Uyarılarınızı düzgün tek tek kurallarında bakarak oluşturulduğunu doğrulamak için.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Uyarılarınızı silin. Bu komutlar, bu makalede daha önce oluşturduğunuz kurallar silin.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Sonraki adımlar
* [Azure izleme genel bir bakış elde](monitoring-overview.md) toplamak ve izlemek bilgi türlerini de dahil olmak üzere.
* Daha fazla bilgi edinmek [Web kancalarını uyarıları yapılandırma](insights-webhooks-alerts.md).
* Daha fazla bilgi edinmek [etkinlik günlüğü olayları uyarıları yapılandırma](monitoring-activity-log-alerts.md).
* Daha fazla bilgi edinmek [Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Alma bir [tanılama günlüklerinin toplanması genel bakış](monitoring-overview-of-diagnostic-logs.md) hizmetinizde ayrıntılı yüksek sıklıkta ölçümleri toplamak için.
* Alma bir [ölçümleri toplama genel bakış](insights-how-to-customize-monitoring.md) hizmetinizi kullanılabilir ve yanıt verebilir durumda olduğundan emin olmak için.
