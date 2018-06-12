---
title: Etkinlik günlüğü Uyarıları Yönetimi olayları Azure uyarılar geçirme
description: Uyarılar yönetim olaylarına 1 Ekim kaldırılır. Geçirme kullanılamamasıdır uyarıları ile hazırlayın.
author: johnkemnetz
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 08/14/2017
ms.author: johnkem
ms.component: alerts
ms.openlocfilehash: 9e4302b780d0c08afbc791a0aec6bfd806aba161
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263713"
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a>Etkinlik günlüğü Uyarıları Yönetimi olayları Azure uyarılar geçirme


> [!WARNING]
> Yönetim olaylarını uyarılar veya ondan sonra Ekim 1 kapatılır. Bu uyarılar ve öyleyse geçirmek anlamak için aşağıdaki yönergeleri kullanın.
>
> 

## <a name="what-is-changing"></a>Ne değiştirme

Azure İzleyicisi'ni (önceki adıyla Azure Öngörüler) Yönetimi olayları dışına tetiklenir ve bir Web kancası URL'si veya e-posta adreslerine bildirim oluşturulan bir uyarı oluşturmak için bir özellik sunmuştur. Oluşturmuş olabileceğiniz bunlardan birini uyarıları şu yollardan biriyle:
* Belirli kaynak türlerine yönelik Azure portalında bölümünde İzleme -> Uyarılar -> Ekle "Uyarı" olarak ayarlandığı "Olayları" Uyarısı,
* Add-AzureRmLogAlertRule PowerShell cmdlet'ini çalıştırarak
* Doğrudan kullanarak [uyarı REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) ile odata.type = "ManagementEventRuleCondition" ve dataSource.odata.type "RuleManagementEventDataSource" =
 
Aşağıdaki PowerShell betiğini her uyarıdaki ayarlanan koşulları yanı sıra, aboneliğiniz olan Yönetim olayları tüm uyarıların bir listesi döndürür.

```powershell
Connect-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
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

Yönetim olaylarına herhangi bir uyarı varsa PowerShell cmdlet'i yukarıdaki uyarı iletilerini bunun gibi bir dizi çıktı:

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

Bu uyarı iletilerini göz ardı edilebilir. Yönetim olaylarına uyarıları varsa, bu PowerShell cmdlet'ini çıktısı şuna benzeyecektir:

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

Her uyarı kesikli çizgi ile ayrılır ve ayrıntıları uyarı ve izlenmekte olan belirli kuralın kaynak Kimliğini içerir.

Bu işlev için geçişi [Azure etkinlik günlüğü uyarıları izleme](monitoring-activity-log-alerts.md). Bu yeni uyarılar, etkinlik günlüğü olaylarını bir koşul ayarlamaya ve yeni bir olay koşul eşleştiğinde bir bildirim almak etkinleştirin. Bunlar ayrıca Yönetim olaylarına uyarılardan çeşitli iyileştirmeler sağlar:
* Grubunuzun bildirim alıcıların ("eylem") kullanarak çok sayıda uyarı yeniden [Eylem grupları](monitoring-action-groups.md), bir uyarı alması gereken değiştirme karmaşıklığını azaltır.
* Doğrudan eylem gruplarıyla SMS kullanarak telefonunuza bir bildirim alırsınız.
* Yapabilecekleriniz [Resource Manager şablonları ile etkinlik günlüğü uyarıları oluşturma](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* Daha fazla esneklik ve belirli ihtiyaçlarınızı karşılaması için karmaşıklık koşullar oluşturabilirsiniz.
* Bildirimleri daha hızlı bir şekilde teslim edilir.
 
## <a name="how-to-migrate"></a>Geçirme
 
Yeni Etkinlik günlüğü uyarısı oluşturmak için şunlardan birini yapabilirsiniz:
* İzleyin [kılavuzumuzu Azure portalında bir uyarı oluşturma hakkında](monitoring-activity-log-alerts.md)
* Bilgi edinmek için nasıl [Resource Manager şablonu kullanarak bir uyarı oluşturabilir.](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Daha önce oluşturduğunuz yönetim olaylarını uyarılar, etkinlik günlüğü uyarıları otomatik olarak geçirilmez. Şu anda yapılandırdıysanız ve el ile etkinlik günlüğü uyarıları olarak yeniden oluşturmanız Yönetimi olayları uyarılar listelemek için yukarıdaki PowerShell komut dosyasını kullanmanız gerekir. Bu uyarılar yönetim olayları artık Azure aboneliğinizde görünür olacak Ekim 1 önce yapılmalıdır. Azure uyarıları, Azure İzleyici ölçüm uyarıları, Application Insights uyarıları ve günlük analizi uyarılar dahil olmak üzere diğer türleri, bu değişiklikten etkilenmez. Herhangi bir sorunuz varsa, aşağıdaki yorumları gönderin.


## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [etkinlik günlüğü](monitoring-overview-activity-logs.md)
* Yapılandırma [Azure Portalı aracılığıyla etkinlik günlüğü uyarıları](monitoring-activity-log-alerts.md)
* Yapılandırma [etkinlik günlüğü uyarıları aracılığıyla kaynak yöneticisi](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Gözden geçirme [etkinlik günlüğü uyarı Web kancası şeması](monitoring-activity-log-alerts-webhook.md)
* Daha fazla bilgi edinmek [hizmet bildirimleri](monitoring-service-notifications.md)
* Daha fazla bilgi edinmek [Eylem grupları](monitoring-action-groups.md)
