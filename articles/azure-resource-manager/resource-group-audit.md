---
title: Kaynakları izlemek için Azure etkinlik günlüklerini görüntüleme | Microsoft Docs
description: Kullanıcı eylemleri gözden geçirin ve hataları için etkinlik günlüklerini kullanın. Azure portalında PowerShell, Azure CLI ve REST gösterir.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: tomfitz
ms.openlocfilehash: 7ff45be4eea5c6e8ab83093847164ede0e94579a
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65606591"
---
# <a name="view-activity-logs-to-monitor-actions-on-resources"></a>Kaynaklar üzerinde işlemleri izlemek için etkinlik günlüklerini görüntüleme

Etkinlik günlükleri ile aşağıdakileri belirleyebilirsiniz:

* hangi işlemleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen
* kimin işlemi başladı
* İşlem zaman oluştu
* İşlemin durumu
* Yardımcı olabilecek diğer özelliklerin değerlerine işlemi araştırın

Etkinlik günlüğü, kaynaklarınız için tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Bu, okuma işlemlerini (GET) içermez. Kaynak eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md). Sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki kullanıcının bir kaynağı nasıl değiştirdiğini izlemek için etkinlik günlüklerini kullanabilirsiniz.

Etkinlik günlükleri, 90 gün boyunca tutulur. Başlangıç tarihi geçmiş 90 günden daha uzun olmadığı sürece herhangi bir tarih aralığı için sorgulayabilirsiniz.

Portal, PowerShell, Azure CLI, Insights REST API aracılığıyla etkinlik günlüklerinden bilgi almak veya [Insights .NET kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="azure-portal"></a>Azure portal

1. Portal üzerinden etkinlik günlüklerini görüntülemek için seçin **İzleyici**.

    ![İzleme seçin](./media/resource-group-audit/select-monitor.png)

1. Seçin **etkinlik günlüğü**.

    ![Etkinlik günlüğü seçin](./media/resource-group-audit/select-activity-log.png)

1. Son işlem özetini görürsünüz. Varsayılan bir filtre kümesi işlemleri için uygulanır. Özet bilgilere kimin eylem başlatıldı ve ne zaman oluştuğunu içerir dikkat edin.

    ![Son işlemlerinin özetini görüntüle](./media/resource-group-audit/audit-summary.png)

1. Filtreler önceden tanımlı bir dizi hızlı bir şekilde çalıştırmak için seçin **hızlı Öngörüler**.

    ![Hızlı Öngörüler'i seçin](./media/resource-group-audit/select-quick-insights.png)

1. Seçeneklerden birini seçin. Örneğin **başarısız dağıtım** dağıtımlarından hatalar görebilirsiniz.

    ![Dağıtımları başarısız seçin](./media/resource-group-audit/select-failed-deployments.png)

1. Filtreler dağıtım hataları odaklanmak için son 24 saat içindeki değiştirilmiş dikkat edin. Filtrelerle eşleşen işlemler görüntülenir.

    ![Görünüm filtreleri](./media/resource-group-audit/view-filters.png)

1. Belirli işlemleri odaklanmak için filtreleri değiştirebilir veya yenilerini uygulayın. Örneğin, aşağıdaki görüntüde için yeni bir değer gösterir **Timespan** ve **kaynak türü** depolama hesapları için ayarlanır. 

    ![filtre seçeneklerini ayarlama](./media/resource-group-audit/set-filter.png)

1. Sorgu daha sonra tekrar çalıştırmanız gerekiyorsa, seçin **PIN geçerli filtreler**.

    ![PIN filtreleri](./media/resource-group-audit/pin-filters.png)

1. Filtre, bir ad verin.

    ![Ad filtreleri](./media/resource-group-audit/name-filters.png)

1. Filtre, panoda kullanılabilir.

    ![Panoda filtre göster](./media/resource-group-audit/show-dashboard.png)

1. Portaldan bir kaynağa değişiklikleri görüntüleyebilirsiniz. Gidin varsayılanına İzleyicisi'nde görüntülemek ve bir kaynağı değiştirme dahil bir işlem seçin.

    ![İşlem seçin](./media/resource-group-audit/select-operation.png)

1. Seçin **değişiklik geçmişini (Önizleme)** ve kullanılabilir işlemleri birini seçin.

    ![Değişiklik geçmişi seçin](./media/resource-group-audit/select-change-history.png)

1. Değişiklikler kaynak görüntülenir.

    ![Değişiklikleri Göster](./media/resource-group-audit/show-changes.png)

Değişiklik geçmişi hakkında daha fazla bilgi için bkz: [alma kaynak değişiklikleri](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Günlük girişlerini almak için çalıştırın **Get-AzLog** komutu. Giriş listesine filtre uygulamak için ek parametreler sunar. Bir başlangıç ve bitiş saati belirtmezsiniz ise son yedi gün girişlerinde döndürülür.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup
```

Aşağıdaki örnek, araştırma işlemleri belirtilen bir süre boyunca gerçekleştirilen etkinlik günlüğünün kullanma işlemini gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime 2019-05-05T06:00 -EndTime 2019-05-09T06:00
```

Ya da tarih işlevler gibi son 14 gün, tarih aralığını belirtmek için kullanabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
```

Belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
```

Başarısız işlemler için filtre uygulayabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup -Status Failed
```

Bu giriş için durum iletisi bakarak bir hatada odaklanabilirsiniz.

```azurepowershell-interactive
(Get-AzLog -ResourceGroup ExampleGroup -Status Failed).Properties.Content.statusMessage | ConvertFrom-Json
```

Döndürülen verileri sınırlamak için belirli değerler seçebilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroupName ExampleGroup | Format-table EventTimeStamp, Caller, @{n='Operation'; e={$_.OperationName.value}}, @{n='Status'; e={$_.Status.value}}, @{n='SubStatus'; e={$_.SubStatus.LocalizedValue}}
```

Belirttiğiniz başlangıç zamanı bağlı olarak, önceki komutların uzun işlemler için kaynak grubu listesini döndürebilir. İçin arama ölçütlerini sağlayarak aradıklarınızı sonuçları filtreleyebilirsiniz. Örneğin, işlem türüne göre filtre uygulayabilirsiniz.

```azurepowershell-interactive
Get-AzLog -ResourceGroup ExampleGroup | Where-Object {$_.OperationName.value -eq "Microsoft.Resources/deployments/write"}
```

Bir kaynak için değişiklik geçmişini görmek için Kaynak Grafiği'ni kullanabilirsiniz. Daha fazla bilgi için [alma kaynak değişiklikleri](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="azure-cli"></a>Azure CLI'si

Günlük girişlerini almak için çalıştırın [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutunu zaman aralığını belirtmek için bir uzaklık.

```azurecli-interactive
az monitor activity-log list --resource-group ExampleGroup --offset 7d
```

Aşağıdaki örnek, araştırma işlemleri belirtilen bir süre boyunca gerçekleştirilen etkinlik günlüğünün kullanma işlemini gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --start-time 2019-05-01 --end-time 2019-05-15
```

Da artık mevcut bir kaynak grubu için belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --caller someone@contoso.com --offset 5d
```

Başarısız işlemler için filtre uygulayabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d
```

Bu giriş için durum iletisi bakarak bir hatada odaklanabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --status Failed --offset 1d --query [].properties.statusMessage
```

Döndürülen verileri sınırlamak için belirli değerler seçebilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query '[].{Operation: operationName.value, Status: status.value, SubStatus: subStatus.localizedValue}'
```

Belirttiğiniz başlangıç zamanı bağlı olarak, önceki komutların uzun işlemler için kaynak grubu listesini döndürebilir. İçin arama ölçütlerini sağlayarak aradıklarınızı sonuçları filtreleyebilirsiniz. Örneğin, işlem türüne göre filtre uygulayabilirsiniz.

```azurecli-interactive
az monitor activity-log list -g ExampleGroup --offset 1d --query "[?operationName.value=='Microsoft.Storage/storageAccounts/write']"
```

Bir kaynak için değişiklik geçmişini görmek için Kaynak Grafiği'ni kullanabilirsiniz. Daha fazla bilgi için [alma kaynak değişiklikleri](../governance/resource-graph/how-to/get-resource-changes.md).

## <a name="rest-api"></a>REST API

Etkinlik günlüğü ile çalışmaya yönelik REST işlemlerini parçası olan [Insights REST API](/rest/api/monitor/). Etkinlik günlüğü olayları almak için bkz: [bir abonelik yönetimi olayları listesi](/rest/api/monitor/activitylogs).

## <a name="next-steps"></a>Sonraki adımlar

* Azure etkinlik günlükleri, aboneliğinizdeki eylemler hakkında daha fazla Öngörüler elde etmek için Power BI ile kullanılabilir. Bkz: [View ve Power BI ve diğer Azure etkinlik günlüklerini analiz](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Güvenlik ilkelerini ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
* Dağıtım işlemlerini görüntüleme için komutlar hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md).
* Tüm kullanıcılar için kaynak silme işlemlerini önlemek öğrenmek için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).
* Her bir Microsoft Azure Resource Manager sağlayıcısı için işlemlerin listesini görmek için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md)
