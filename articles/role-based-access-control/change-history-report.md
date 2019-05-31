---
title: Azure kaynakları için RBAC değişiklikleri için etkinlik günlüklerini görüntüleme | Microsoft Docs
description: Rol tabanlı erişim denetimi (RBAC) değişikliklerin Son 90 güne ait Azure kaynakları için etkinlik günlüklerini görüntüleme.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 842d3146bf1927871e29eb750cde09e9029b7c12
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242096"
---
# <a name="view-activity-logs-for-rbac-changes-to-azure-resources"></a>RBAC değişiklikler Azure kaynakları için etkinlik günlüklerini görüntüleme

Bazen Azure kaynakları için rol tabanlı erişim denetimi (RBAC) değişiklikler hakkında bilgiler gibi denetim ve sorun giderme amacıyla gerekir. Birisi yapar değişiklikleri rol atamaları veya rol tanımları, Abonelikleriniz dahilindeki dilediğiniz zaman değişiklikleri günlüğe [Azure etkinlik günlüğü](../azure-monitor/platform/activity-logs-overview.md). Son 90 gün için tüm RBAC değişiklikleri görmek için etkinlik günlüklerini görüntüleyebilirsiniz.

## <a name="operations-that-are-logged"></a>Günlüğe kaydedilen işlemleri

Etkinlik günlüğüne RBAC ile ilgili işlemler şunlardır:

- Rol ataması oluştur
- Rol atamasını Sil
- Özel rol tanımını güncelle
- Özel rol tanımını sil

## <a name="azure-portal"></a>Azure portal

Başlamanın en kolay yolu, Azure portalı ile etkinlik günlükleri görüntülemektir. Aşağıdaki ekran görüntüsünde, rol ataması ve rol tanımı işlemleri görüntülemek için filtre uygulanmış bir etkinlik günlüğü örneği gösterilmektedir. CSV dosyası olarak günlükleri indirmek için bağlantıyı da içerir.

![Etkinlik günlüklerini kullanarak portalı - ekran görüntüsü](./media/change-history-report/activity-log-portal.png)

Portalda etkinlik günlüğü birkaç filtreleri sahiptir. RBAC ile ilgili filtreleri şunlardır:

|Filtre  |Değer  |
|---------|---------|
|Olay kategorisi     | <ul><li>Yönetim</li></ul>         |
|İşlem     | <ul><li>Rol ataması oluştur</li> <li>Rol atamasını Sil</li> <li>Özel rol tanımını güncelle</li> <li>Özel rol tanımını sil</li></ul>      |


Etkinlik günlükleri hakkında daha fazla bilgi için bkz. [etkinlik günlüğünde olayları görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json).

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Azure PowerShell ile etkinlik günlüklerini görüntülemek için kullanın [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) komutu.

Bu komut, bir abonelikte son yedi güne ait tüm rol atama değişiklikleri listeler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

Bu komut, son yedi gün için bir kaynak grubundaki tüm rol tanımı değişiklikleri listeler:

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

Bu komut tüm rol ataması ve son yedi güne ait bir abonelik rol tanımı değişiklikleri listeler ve sonuçları bir listede görüntüler:

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
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
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"}}

```

## <a name="azure-cli"></a>Azure CLI

Azure CLI ile etkinlik günlüklerini görüntülemek için kullanın [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutu.

Bu komut, bir kaynak grubundaki etkinlik günlükleri başlangıç zamanından bu yana listeler:

```azurecli
az monitor activity-log list --resource-group pharma-sales --start-time 2018-04-20T00:00:00Z
```

Bu komut, başlangıç zamanından bu yana yetkilendirme kaynak sağlayıcısı için etkinlik günlüklerini listeler:

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure İzleyici günlüklerine](../log-analytics/log-analytics-overview.md) toplamak ve tüm Azure kaynaklarınız için RBAC değişiklikleri analiz etmek için kullanabileceğiniz başka bir araçtır. Azure İzleyici günlüklerine, aşağıdaki faydaları vardır:

- Karmaşık sorgular ve mantıksal yazma
- Uyarılar, Power BI ve diğer araçlar ile tümleştirme
- Uzun saklama süreleri için verileri kaydetme
- Güvenlik, sanal makine ve özel gibi diğer günlükleri ile çapraz başvuru

Başlamak için temel adımlar şunlardır:

1. [Log Analytics çalışma alanı oluşturma](../azure-monitor/learn/quick-create-workspace.md).

1. [Etkinlik günlüğü analizi çözümü yapılandırma](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution) çalışma alanınız için.

1. [Etkinlik günlüklerini görüntüleme](../azure-monitor/platform/activity-log-collect.md#activity-logs-analytics-monitoring-solution). Etkinlik günlüğü analizi çözümü genel bakış sayfasına gitmek için hızlı bir yolu **Log Analytics** seçeneği.

   ![Azure İzleyici seçeneği portalda günlükleri](./media/change-history-report/azure-log-analytics-option.png)

1. İsteğe bağlı olarak [günlük araması](../log-analytics/log-analytics-log-search.md) sayfası veya [Gelişmiş analiz portalını](../azure-monitor/log-query/get-started-portal.md) sorgulamak ve günlükleri görüntülemek için. Bu iki seçenek hakkında daha fazla bilgi için bkz: [günlük araması sayfasını veya Gelişmiş analiz portalını](../azure-monitor/log-query/portals.md).

Hedef kaynak sağlayıcısı tarafından düzenlenmiş olan yeni rol atamaları döndüren bir sorgu aşağıda verilmiştir:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

Rol atama değişikliklerinin bir grafikte görüntülenen döndüren bir sorgu aşağıda verilmiştir:

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![Etkinlik günlükleri kullanarak Advanced Analytics portalı - ekran görüntüsü](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Etkinlik günlüğünde olayları görüntüleme](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure Etkinlik Günlüğü ile Abonelik Etkinliğini İzleme](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
