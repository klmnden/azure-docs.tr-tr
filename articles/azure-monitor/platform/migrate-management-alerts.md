---
title: Azure uyarıları yönetim olayları etkinlik günlüğü uyarılarına geçirme
description: 1 Ekim yönetim olayları ile ilgili uyarılar kaldırılacak. Geçirme mevcut uyarıları göre hazırlayın.
author: lingliw
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/12/19
ms.author: v-lingwu
ms.subservice: alerts
ms.openlocfilehash: fb54e11c9da6bec2a1e0354317df6343140cbf09
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60255921"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Azure uyarıları yönetim olayları etkinlik günlüğü uyarılarına geçirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

> [!WARNING]
> Tarihinde veya sonrasında 1 Ekim yönetim olayları ile ilgili uyarılar kapatılır. Bu uyarılar ve bu durumda geçiş anlamak için aşağıdaki yönergeleri kullanın.

## <a name="what-is-changing"></a>Ne değişiyor

Azure İzleyici (eski adıyla Azure öngörüleri) yönetimi olaylarını dışına tetiklenir ve bir Web kancası URL'si veya e-posta adreslerine bildirim oluşturulan bir uyarı oluşturmak için bir özellik sunuluyor. Oluşturmuş olabileceğiniz bunlardan birine uyarılarını şu adımlardan herhangi birini:
* Bazı kaynak türleri için Azure portalında izleme bölümünden -> Uyarılar -> Ekle "Uyarısı" ayarlandığı "Olaylar" için uyarı,
* Add-AzLogAlertRule PowerShell cmdlet'ini çalıştırarak
* Doğrudan kullanarak [uyarı REST API'si](https://docs.microsoft.com/rest/api/monitor/alertrules) odata.type ile "ManagementEventRuleCondition" ve dataSource.odata.type = "RuleManagementEventDataSource" =
 
Aşağıdaki PowerShell betiğini tüm uyarıların bir listesi, aboneliğinizin yanı sıra her bir uyarı koşullara sahip yönetim olayları döndürür.

```powershell
Connect-AzAccount -Environment AzureChinaCloud
$alerts = $null
foreach ($rg in Get-AzResourceGroup ) {
  $alerts += Get-AzAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

Uyarı Yönetimi olayları varsa, PowerShell cmdlet'i yukarıdaki uyarı iletilerini bunun gibi bir dizi çıkarır:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Bu uyarı iletilerini yoksayılabilir. Yönetim olaylarına uyarılar varsa, bu PowerShell cmdlet'in çıktısı şuna benzeyecektir:

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

Her bir uyarı tarafından kesikli çizgiye ayrılır ve ayrıntıları, uyarı ve izlenmekte olan belirli bir kuralın kaynak Kimliğini içerir.

Bu işlev için geçirileceğini [Azure etkinlik günlüğü uyarıları izleme](../../azure-monitor/platform/activity-log-alerts.md). Bu yeni uyarılar, etkinlik günlüğü olayları bir koşul ayarlamaya ve yeni bir olay koşul eşleştiğinde bir bildirim almak etkinleştirin. Bunlar ayrıca uyarılar yönetim olayları ile ilgili çeşitli geliştirmeler sağlar:
* Grubunuzun bildirim alıcıları ("Eylemler") kullanarak çok sayıda uyarı yeniden [Eylem grupları](../../azure-monitor/platform/action-groups.md), bir uyarı almanız gerekir değiştirme karmaşıklığı azaltır.
* Doğrudan SMS ile Eylem grupları kullanarak telefonunuza bir bildirim alabilir.
* Yapabilecekleriniz [Resource Manager şablonları ile etkinlik günlüğü uyarıları oluşturma](../../azure-monitor/platform/alerts-activity-log.md).
* Daha fazla esneklik ve belirli ihtiyaçlarınızı karşılamak için karmaşıklık koşulları oluşturabilirsiniz.
* Bildirimleri daha hızlı teslim edilir.
 
## <a name="how-to-migrate"></a>Geçiş yapma
 
Yeni Etkinlik günlüğü uyarısı oluşturmak için şunlardan birini yapabilirsiniz:
* İzleyin [kılavuzumuza Azure portalında uyarı oluşturma](../../azure-monitor/platform/activity-log-alerts.md)
* Bilgi edinmek için nasıl [Resource Manager şablonu kullanarak bir uyarı oluştur](../../azure-monitor/platform/alerts-activity-log.md)
 
Daha önce oluşturduğunuz yönetim olayları ile ilgili uyarılar, etkinlik günlüğü uyarılarına otomatik olarak geçirilmez. Şu anda yapılandırdıysanız ve bunları el ile etkinlik günlüğü uyarısı yeniden Yönetimi olayları ile ilgili uyarılar listelemek için yukarıdaki PowerShell Betiği kullanmanız gerekir. Bu yönetim olayları ile ilgili uyarılar artık Azure aboneliğinizde görünür olacak 1 Ekim önce yapılmalıdır. Azure uyarıları, Azure İzleyici ölçüm uyarıları, Application Insights uyarıları ve günlük araması uyarılar gibi diğer türleri, bu değişiklikten etkilenmez. Herhangi bir sorunuz varsa aşağıdaki yorumları gönderin.


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [etkinlik günlüğü](../../azure-monitor/platform/activity-logs-overview.md)
* Yapılandırma [Azure portal aracılığıyla etkinlik günlüğü uyarıları](../../azure-monitor/platform/activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları Resource Manager aracılığıyla](../../azure-monitor/platform/alerts-activity-log.md)
* Gözden geçirme [etkinlik günlüğü uyarısı Web kancası şeması](../../azure-monitor/platform/activity-log-alerts-webhook.md)
* Daha fazla bilgi edinin [hizmet bildirimleri](../../azure-monitor/platform/service-notifications.md)
* Daha fazla bilgi edinin [Eylem grupları](../../azure-monitor/platform/action-groups.md)
