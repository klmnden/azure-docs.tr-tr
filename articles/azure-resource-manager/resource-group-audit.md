---
title: Kaynakları izlemek için Azure etkinlik günlüklerini görüntüleme | Microsoft Docs
description: Kullanıcı eylemleri gözden geçirin ve hataları için etkinlik günlüklerini kullanın. Azure Portal PowerShell, Azure CLI ve REST gösterir.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: tomfitz
ms.openlocfilehash: 8348099d778a9ec65e907bb3d21ae995041b9fb6
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786108"
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme

Etkinlik günlükleri ile aşağıdakileri belirleyebilirsiniz:

* hangi işlemleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen
* kimin işlemi başladı
* İşlem zaman oluştu
* İşlemin durumu
* Yardımcı olabilecek diğer özelliklerin değerlerine işlemi araştırın

Etkinlik günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Bu, okuma işlemlerini (GET) içermez. Kaynak eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md). Denetim günlüklerinde sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki bir kullanıcı bir kaynağı nasıl değiştirdiğini izlemek için kullanabilirsiniz.

Etkinlik günlükleri, 90 gün boyunca tutulur. Başlangıç tarihi geçmiş 90 günden daha uzun olmadığı sürece herhangi bir tarih aralığı için sorgulayabilirsiniz.

Portal, PowerShell, Azure CLI, Insights REST API aracılığıyla etkinlik günlüklerinden bilgi almak veya [Insights .NET kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="the-azure-portal"></a>Azure portal

1. Portal üzerinden etkinlik günlüklerini görüntülemek için seçin **İzleyici**.

    ![İzleme seçin](./media/resource-group-audit/select-monitor.png)

1. Seçin **etkinlik günlüğü**.

    ![Etkinlik günlüğü seçin](./media/resource-group-audit/select-activity-log.png)

1. Son işlem özetini görürsünüz. Varsayılan bir filtre kümesi işlemleri için uygulanır.

    ![Son işlemlerinin özetini görüntüle](./media/resource-group-audit/audit-summary.png)

1. Filtreler önceden tanımlı bir dizi hızlı bir şekilde çalıştırmak için seçin **hızlı Öngörüler** ve seçeneklerden birini belirleyin.

    ![sorgu seçin](./media/resource-group-audit/quick-insights.png)

1. Belirli işlemleri odaklanmak için filtreleri değiştirebilir veya yenilerini uygulayın. Örneğin, aşağıdaki görüntüde için yeni bir değer gösterir **Timespan** ve **kaynak türü** depolama hesapları için ayarlanır. 

    ![filtre seçeneklerini ayarlama](./media/resource-group-audit/set-filter.png)

1. Sorgu daha sonra tekrar çalıştırmanız gerekiyorsa, seçin **PIN geçerli filtreler**.

    ![PIN filtreleri](./media/resource-group-audit/pin-filters.png)

1. Filtre, bir ad verin.

    ![Ad filtreleri](./media/resource-group-audit/name-filters.png)

1. Filtre, panoda kullanılabilir.

    ![Panoda filtre göster](./media/resource-group-audit/show-dashboard.png)

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Günlük girişlerini almak için çalıştırın **Get-AzLog** komutu. Giriş listesine filtre uygulamak için ek parametreler sunar. Bir başlangıç ve bitiş saati belirtmezsiniz ise son yedi gün girişlerinde döndürülür.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup
  ```

    Aşağıdaki örnek, araştırma işlemleri belirtilen bir süre boyunca gerçekleştirilen etkinlik günlüğünün kullanma işlemini gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-01-09T06:00 -EndTime 2019-01-15T06:00
  ```

    Ya da tarih işlevler gibi son 14 gün, tarih aralığını belirtmek için kullanabilirsiniz.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

* Da artık mevcut bir kaynak grubu için belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilirsiniz.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

* Başarısız işlemler için filtre uygulayabilirsiniz.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup -Status Failed
  ```

* Bu giriş için durum iletisi bakarak bir hatada odaklanabilirsiniz.

  ```azurepowershell-interactive
  ((Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties[0].Content.statusMessage | ConvertFrom-Json).error
  ```

* Döndürülen verileri sınırlamak için belirli değerler seçebilirsiniz.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
  ```

* Belirttiğiniz başlangıç zamanı bağlı olarak, önceki komutların uzun işlemler için kaynak grubu listesini döndürebilir. İçin arama ölçütlerini sağlayarak aradıklarınızı sonuçları filtreleyebilirsiniz. Örneğin, işlem türüne göre filtre uygulayabilirsiniz.

  ```azurepowershell-interactive
  Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
  ```

## <a name="azure-cli"></a>Azure CLI

* Günlük girişlerini almak için çalıştırın [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutunu zaman aralığını belirtmek için bir uzaklık.

  ```azurecli-interactive
  az monitor activity-log list --resource-group ExampleGroup --offset 7d
  ```

  Aşağıdaki örnek, araştırma işlemleri belirtilen bir süre boyunca gerçekleştirilen etkinlik günlüğünün kullanma işlemini gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --start-time 2019-01-01 --end-time 2019-01-15
  ```

* Da artık mevcut bir kaynak grubu için belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilirsiniz.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
  ```

* Başarısız işlemler için filtre uygulayabilirsiniz.

  ```azurecli-interactive
  az monitor activity-log list -g demoRG --status Failed --offset 1d
  ```

* Bu giriş için durum iletisi bakarak bir hatada odaklanabilirsiniz.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
  ```

* Döndürülen verileri sınırlamak için belirli değerler seçebilirsiniz.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
  ```

* Belirttiğiniz başlangıç zamanı bağlı olarak, önceki komutların uzun işlemler için kaynak grubu listesini döndürebilir. İçin arama ölçütlerini sağlayarak aradıklarınızı sonuçları filtreleyebilirsiniz. Örneğin, işlem türüne göre filtre uygulayabilirsiniz.

  ```azurecli-interactive
  az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
  ```

## <a name="rest-api"></a>REST API

Etkinlik günlüğü ile çalışmaya yönelik REST işlemlerini parçası olan [Insights REST API](/rest/api/monitor/). Etkinlik günlüğü olayları almak için bkz: [bir abonelik yönetimi olayları listesi](/rest/api/monitor/activitylogs).

## <a name="next-steps"></a>Sonraki adımlar

* Azure etkinlik günlükleri, aboneliğinizdeki eylemler hakkında daha fazla Öngörüler elde etmek için Power BI ile kullanılabilir. Bkz: [View ve Power BI ve diğer Azure etkinlik günlüklerini analiz](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Güvenlik ilkelerini ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
* Dağıtım işlemlerini görüntüleme için komutlar hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md).
* Tüm kullanıcılar için kaynak silme işlemlerini önlemek öğrenmek için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).
* Her bir Microsoft Azure Resource Manager sağlayıcısı için işlemlerin listesini görmek için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md)
