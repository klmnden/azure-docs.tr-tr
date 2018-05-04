---
title: Azure RBAC değişiklikleri etkinlik günlükleri görüntüleme | Microsoft Docs
description: Son 90 gün için rol tabanlı erişim denetimi değişikliklerin etkinlik günlüklerini görüntüle.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/23/2017
ms.author: rolyon
ms.reviewer: rqureshi
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5e09ccdc4942a39e54b760cb5ad78c035dbc05f8
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="view-activity-logs-for-role-based-access-control-changes"></a>Rol tabanlı erişim denetimi değişiklikleri etkinlik günlüklerini görüntüle

Birisi yapar değişiklikleri rol tanımları veya rol atamalarını aboneliklerinizi içinde dilediğiniz zaman değişiklikleri günlüğe [Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) yönetim kategorisi içinde. Son 90 gün için tüm rol tabanlı erişim denetimi (RBAC) değişiklikleri görmek için etkinlik günlükleri görüntüleyebilirsiniz.

## <a name="operations-that-are-logged"></a>Günlüğe kaydedilen işlemleri

Etkinlik günlüğüne RBAC güvenlikle ilgili işlemler şunlardır:

- Özel rol tanımı oluştur veya güncelleştir
- Özel rol tanımını sil
- Rol ataması oluştur
- Rol atamasını sil

## <a name="azure-portal"></a>Azure portalına

Başlamak için en kolay yolu, Azure portal ile etkinlik günlükleri görüntülemektir. Görüntüleyecek şekilde filtrelenmiş bir etkinlik günlüğü örneği aşağıdaki ekran görüntüsü gösterilmektedir **Yönetim** rol tanımı ve rol ataması işlemlerinin yanı sıra kategorisi. Ayrıca, bir CSV dosyası olarak günlükleri indirmek için bir bağlantı içerir.

![Etkinlik günlükleri using the portal - ekran görüntüsü](./media/change-history-report/activity-log-portal.png)

Daha fazla bilgi için bkz: [etkinlik günlüğünde olayları görüntülemek](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

Azure PowerShell ile etkinlik günlükleri görüntülemek için kullanın [Get-AzureRmLog](/powershell/module/azurerm.insights/get-azurermlog) komutu.

Bu komut, son yedi gün için bir Abonelikteki tüm rol atama değişiklikleri listeler:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Bu komut, son yedi gün için bir kaynak grubundaki tüm rol tanımı değişiklikleri listeler:

```azurepowershell
Get-AzureRmLog -ResourceGroupName pharma-sales-projectforecast -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Bu komut tüm rol ataması ve son yedi gün için bir abonelik rol tanımı değişiklikleri listeler ve sonuçları bir liste görüntüler:

```azurepowershell
Get-AzureRmLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales-projectforecast"}}

```

## <a name="azure-cli"></a>Azure CLI

Azure CLI ile etkinlik günlükleri görüntülemek için kullanın [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutu.

Bu komut, başlangıç zamanından bu yana bir kaynak grubunda etkinlik günlükleri listeler:

```azurecli
az monitor activity-log list --resource-group pharma-sales-projectforecast --start-time 2018-04-20T00:00:00Z
```

Bu komut, etkinlik günlükleri için yetkilendirme kaynak sağlayıcısı başlangıç zamanından bu yana listeler:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-log-analytics"></a>Azure Log Analytics

[Azure günlük analizi](../log-analytics/log-analytics-overview.md) toplamak ve tüm Azure kaynakları için rol tabanlı erişim denetimi değişiklikleri analiz etmek için kullanabileceğiniz başka bir araçtır. Günlük analizi aşağıdaki faydaları vardır:

- Karmaşık sorgular ve mantığı yazma
- Uyarılar, Power BI ve diğer araçlarla tümleştirme
- Daha uzun bekletme dönemleri verilerini kaydetme
- Güvenlik, sanal makine ve özel gibi diğer günlükleriyle çapraz başvuru

Başlamak için temel adımlar şunlardır:

1. [Günlük analizi çalışma alanı oluşturma](../log-analytics/log-analytics-quick-create-workspace.md).

1. [Etkinlik günlüğü analiz çözümü yapılandırmak](../log-analytics/log-analytics-activity.md#configuration) çalışma alanınız için.

1. [Etkinlik günlükleri görüntülemek](../log-analytics/log-analytics-activity.md#using-the-solution). Etkinlik günlüğü analizi Genel Bakış sayfasına gitmek için hızlı bir yolu **günlük analizi** seçeneği.

   ![Günlük analizi seçeneği portalında](./media/change-history-report/azure-log-analytics-option.png)

1. İsteğe bağlı olarak kullanmak [günlük arama](../log-analytics/log-analytics-log-search.md) sayfa veya [Advanced Analytics portalı](https://docs.loganalytics.io/docs/Learn) sorgulamak ve günlükleri görüntülemek için. Bu iki seçenek hakkında daha fazla bilgi için bkz: [günlük arama sayfası veya Advanced Analytics portalı](../log-analytics/log-analytics-log-search-portals.md).

Aşağıda, hedef kaynak sağlayıcısı tarafından düzenlenen yeni rol atamalarını döndüren bir sorgu verilmiştir:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Aşağıda, grafikte görüntülenen rol atama değişiklikleri döndüren bir sorgu verilmiştir:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Etkinlik günlükleri kullanarak Advanced Analytics portalı - ekran görüntüsü](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğünde olayları görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure etkinlik günlüğü ile abonelik etkinliğini izleme](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
