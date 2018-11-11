---
title: Kaynakları izlemek için Azure etkinlik günlüklerini görüntüleme | Microsoft Docs
description: Kullanıcı eylemleri gözden geçirin ve hataları için etkinlik günlüklerini kullanın. Azure Portal PowerShell, Azure CLI ve REST gösterir.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fcdb3125-13ce-4c3b-9087-f514c5e41e73
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/08/2018
ms.author: tomfitz
ms.openlocfilehash: 09f7fba2b8ae3b3ccc8710ffe9302d02d311c74c
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514341"
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Kaynaklara uygulanan eylemleri denetlemek için etkinlik günlüklerini görüntüleme

Etkinlik günlükleri ile aşağıdakileri belirleyebilirsiniz:

* hangi işlemleri, aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen
* (bir arka uç hizmeti tarafından başlatılan işlemleri bir kullanıcı olarak çağıran döndürmeyen rağmen) işlemi başlatandır
* İşlem zaman oluştu
* İşlemin durumu
* Yardımcı olabilecek diğer özelliklerin değerlerine işlemi araştırın

Etkinlik günlüğü, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Okuma işlemlerini (GET) içermez. Kaynak eylemleri listesi için bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md). Denetim günlüklerinde sorun giderme sırasında bir hata bulmak veya kuruluşunuzdaki bir kullanıcı bir kaynağı nasıl değiştirdiğini izlemek için kullanabilirsiniz.

Etkinlik günlükleri 90 gün boyunca saklanır. Başlangıç tarihi 90 günden eski olmamak şartıyla istediğiniz tarih aralığını sorgulayabilirsiniz.

Portal, PowerShell, Azure CLI, Insights REST API aracılığıyla etkinlik günlüklerinden bilgi almak veya [Insights .NET kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portal

1. Portal üzerinden etkinlik günlüklerini görüntülemek için seçin **İzleyici**.

    ![Etkinlik günlüklerini seçin](./media/resource-group-audit/select-monitor.png)

   Veya belirli bir kaynak veya kaynak grubu için etkinlik günlüğü otomatik olarak filtrelemek için seçin **etkinlik günlüğü**. Etkinlik günlüğü, seçili kaynak tarafından otomatik olarak filtrelenmiştir dikkat edin.

    ![Kaynağa göre filtrele](./media/resource-group-audit/filtered-by-resource.png)
2. İçinde **etkinlik günlüğü**, son işlemleri özetini görürsünüz.

    ![eylemleri göster](./media/resource-group-audit/audit-summary.png)
3. Görüntülenen işlemlerin sayısını sınırlamak için farklı koşullar'ı seçin. Örneğin, aşağıdaki gösterir görüntüde **Timespan** ve **olayı başlatan tarafından** değiştirilen alanları belirli kullanıcı veya geçen ay için uygulama tarafından gerçekleştirilen eylemler görüntülemek için. Seçin **Uygula** Sorgunuzun sonuçlarını görüntülemek için.

    ![filtre seçeneklerini ayarlama](./media/resource-group-audit/set-filter.png)

4. Sorgu daha sonra tekrar çalıştırmanız gerekiyorsa, seçin **Kaydet** ve sorgunun bir ad verin.

    ![Sorguyu Kaydet](./media/resource-group-audit/save-query.png)
5. Hızla bir sorguyu çalıştırmak için dağıtımları başarısız gibi yerleşik sorgulardan birini seçebilirsiniz.

    ![sorgu seçin](./media/resource-group-audit/select-quick-query.png)

   Seçili sorguyu gerekli filtre değerlerini otomatik olarak ayarlar.

    ![Dağıtım hatalarını görüntüle](./media/resource-group-audit/view-failed-deployment.png)

6. Bir olay özetini görmek için işlemleri seçin.

    ![işlem görünümü](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell

1. Günlük girişlerini almak için çalıştırın **Get-AzureRmLog** komutu. Giriş listesine filtre uygulamak için ek parametreler sunar. Başlangıç ve bitiş zamanı belirtmezseniz, son bir saat girişleri döndürülür. Örneğin, almak için bir kaynak grubu için operations son bir saat boyunca çalıştırın:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```

    Aşağıdaki örnek, araştırma işlemleri belirtilen bir süre boyunca gerçekleştirilen etkinlik günlüğünün kullanma işlemini gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Ya da tarih işlevler gibi son 14 gün, tarih aralığını belirtmek için kullanabilirsiniz.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Belirttiğiniz başlangıç zamanı bağlı olarak, önceki komutların uzun işlemler için kaynak grubu listesini döndürebilir. İçin arama ölçütlerini sağlayarak aradıklarınızı sonuçları filtreleyebilirsiniz. Örneğin, nasıl bir web uygulaması durduruldu araştırma çalışıyorsanız, aşağıdaki komutu çalıştırabilirsiniz:

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14) | Where-Object OperationName -eq Microsoft.Web/sites/stop/action
  ```

    Bu örneğin gösterir durdurma eylemi tarafından gerçekleştirilen someone@contoso.com.

  ```powershell
  Authorization     :
  Scope     : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Action    : Microsoft.Web/sites/stop/action
  Role      : Subscription Admin
  Condition :
  Caller            : someone@contoso.com
  CorrelationId     : 84beae59-92aa-4662-a6fc-b6fecc0ff8da
  EventSource       : Administrative
  EventTimestamp    : 8/28/2015 4:08:18 PM
  OperationName     : Microsoft.Web/sites/stop/action
  ResourceGroupName : ExampleGroup
  ResourceId        : /subscriptions/xxxxx/resourcegroups/ExampleGroup/providers/Microsoft.Web/sites/ExampleSite
  Status            : Succeeded
  SubscriptionId    : xxxxx
  SubStatus         : OK
  ```

3. Da artık mevcut bir kaynak grubu için belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilirsiniz.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Başarısız işlemler için filtre uygulayabilirsiniz.

  ```azurepowershell-interactive
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Bu giriş için durum iletisi bakarak bir hatada odaklanabilirsiniz.

  ```azurepowershell-interactive
  ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
  ```

    Döndürür:

        code           message
        ----           -------
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP.

## <a name="azure-cli"></a>Azure CLI

Günlük girişlerini almak için çalıştırın [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutu.

  ```azurecli
  az monitor activity-log list --resource-group <group name>
  ```

## <a name="rest-api"></a>REST API

Etkinlik günlüğü ile çalışmaya yönelik REST işlemlerini parçası olan [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Etkinlik günlüğü olayları almak için bkz: [bir abonelik yönetimi olayları listesi](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* Azure etkinlik günlükleri, aboneliğinizdeki eylemler hakkında daha fazla Öngörüler elde etmek için Power BI ile kullanılabilir. Bkz: [View ve Power BI ve diğer Azure etkinlik günlüklerini analiz](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Güvenlik ilkelerini ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
* Dağıtım işlemlerini görüntüleme için komutlar hakkında bilgi edinmek için bkz. [dağıtım işlemlerini görüntüleme](resource-manager-deployment-operations.md).
* Tüm kullanıcılar için kaynak silme işlemlerini önlemek öğrenmek için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).
* Her bir Microsoft Azure Resource Manager sağlayıcısı için işlemlerin listesini görmek için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md)
