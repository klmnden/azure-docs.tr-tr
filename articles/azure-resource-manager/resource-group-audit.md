---
title: Kaynakları izlemek için Azure etkinlik günlükleri görüntüleme | Microsoft Docs
description: Etkinlik günlükleri gözden geçirme kullanıcı eylemleri ve hatalar için kullanın. Azure Portal PowerShell, Azure CLI ve REST gösterir.
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
ms.date: 04/04/2018
ms.author: tomfitz
ms.openlocfilehash: 2dcf93a635a8eb0a01ec266d2478b6e5a336ec00
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="view-activity-logs-to-audit-actions-on-resources"></a>Kaynakları eylemlerini denetlemek için etkinlik günlüklerini görüntüle

Etkinlik günlükleri, belirleyebilirsiniz:

* hangi işlemlerin aboneliğinizde kaynaklar üzerinde gerçekleştirilen
* (bir arka uç hizmeti tarafından başlatılan işlemleri bir kullanıcı olarak çağıran döndürmeyen rağmen) işlemi kimin başlattığını
* İşlem zaman oluştu
* İşlem durumu
* İşlemi yardımcı olabilecek diğer özelliklerin değerlerine araştırma

Etkinlik günlüğü kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, DELETE) içerir. Okuma işlemleri (GET) içermez. Kaynak eylemler listesi için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md). Denetim günlüklerini hata sorunlarını giderirken bulmak için veya bir kullanıcı, kuruluşunuzdaki bir kaynak nasıl değişiklik izlemek için kullanabilirsiniz.

Etkinlik günlükleri 90 gün boyunca saklanır. Başlangıç tarihi geçmiş 90 günden fazla değil sürece herhangi bir tarih aralığı için sorgulayabilirsiniz.



PowerShell, Azure CLI, Öngörüler REST API'si, portal üzerinden etkinlik günlüklerindeki bilgi almak veya [Öngörüler .NET kitaplığı](https://www.nuget.org/packages/Microsoft.Azure.Insights/).

## <a name="portal"></a>Portal

1. Portal üzerinden etkinlik günlükleri görüntülemek için seçin **İzleyici**.
   
    ![etkinlik günlükleri seçin](./media/resource-group-audit/select-monitor.png)

   Veya otomatik olarak etkinlik günlüğü belirli kaynak veya kaynak grubu için filtre uygulamak için seçim **etkinlik günlüğü**. Etkinlik günlüğü seçilen kaynak tarafından otomatik olarak filtrelenir dikkat edin.
   
    ![Kaynağa göre filtrele](./media/resource-group-audit/filtered-by-resource.png)
2. İçinde **etkinlik günlüğü**, son işlemleri özetini bakın.
   
    ![Eylemler Göster](./media/resource-group-audit/audit-summary.png)
3. Görüntülenen işlemlerinin sayısını sınırlamak için farklı koşullar seçin. Örneğin, aşağıdaki gösterir görüntü **Timespan** ve **olayı başlatan tarafından** alanları değiştirilen belirli kullanıcı veya geçen ay için uygulama tarafından gerçekleştirilen eylemleri görüntülemek için. Seçin **Uygula** Sorgunuzun sonuçlarını görüntülemek için.
   
    ![filtre seçeneklerini ayarlama](./media/resource-group-audit/set-filter.png)

4. Sorgu daha sonra yeniden çalıştırmanız gerekiyorsa, seçin **kaydetmek** ve sorguyu bir ad verin.
   
    ![Sorgu kaydetme](./media/resource-group-audit/save-query.png)
5. Hızla bir sorguyu çalıştırmak için dağıtımları başarısız gibi yerleşik sorguları birini seçebilirsiniz.

    ![seçme sorgusu](./media/resource-group-audit/select-quick-query.png)

   Seçili sorguyu gerekli filtre değerleri otomatik olarak ayarlar.

    ![Dağıtım hataları görüntüleme](./media/resource-group-audit/view-failed-deployment.png)   

6. Olay özetini görmek için işlemlerden birini seçin.

    ![Görünüm işlemi](./media/resource-group-audit/view-operation.png)  

## <a name="powershell"></a>PowerShell

1. Günlük girişlerini almak üzere çalışmaya **Get-AzureRmLog** komutu. Girişlerin listesini filtrelemek için ek parametreleri sağlayın. Bir başlangıç ve bitiş zamanı belirtmezseniz, son bir saat girişleri döndürülür. Örneğin, almak için bir kaynak grubu için işlemleri son bir saat sırasında çalıştırın:

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup
  ```
   
    Aşağıdaki örnek belirtilen bir süre boyunca gerçekleştirilecek araştırma işlemleri için etkinlik günlüğü kullanmayı gösterir. Başlangıç ve bitiş tarihleri, tarih biçiminde belirtilir.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime 2015-08-28T06:00 -EndTime 2015-09-10T06:00
  ```

    Ya da tarih işlevleri son 14 gün gibi tarih aralığı belirtmek için kullanabilirsiniz.
   
  ```powershell 
  Get-AzureRmLog -ResourceGroup ExampleGroup -StartTime (Get-Date).AddDays(-14)
  ```

2. Belirttiğiniz başlangıç saati bağlı olarak, önceki komutların işlemleri kaynak grubu için uzun bir listesi döndürebilir. Ne arama ölçütlerini sağlayarak aradığınız için sonuçları filtreleyebilirsiniz. Örneğin, bir web uygulaması nasıl durduruldu araştırma çalışıyorsanız, aşağıdaki komutu çalıştırabilirsiniz:

  ```powershell
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

3. Hatta artık mevcut bir kaynak grubu için belirli bir kullanıcı tarafından gerçekleştirilen eylemler arayabilir.

  ```powershell 
  Get-AzureRmLog -ResourceGroup deletedgroup -StartTime (Get-Date).AddDays(-14) -Caller someone@contoso.com
  ```

4. Başarısız işlemler için filtre uygulayabilirsiniz.

  ```powershell
  Get-AzureRmLog -ResourceGroup ExampleGroup -Status Failed
  ```

5. Bu girişi için durum iletisi bakarak bir hatada odaklanabilirsiniz.
   
        ((Get-AzureRmLog -Status Failed -ResourceGroup ExampleGroup -DetailedOutput).Properties[1].Content["statusMessage"] | ConvertFrom-Json).error
   
    Hangi döndürür:
   
        code           message                                                                        
        ----           -------                                                                        
        DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. 


## <a name="azure-cli"></a>Azure CLI

Günlük girişlerini almak üzere çalışmaya [az İzleyici etkinlik günlüğü listesi](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) komutu.

  ```azurecli
  az monitor activity-log list --resource-group <group name>
  ```


## <a name="rest-api"></a>REST API

Etkinlik günlüğü ile çalışmaya yönelik REST işlemlerini parçası olan [Insights REST API](https://msdn.microsoft.com/library/azure/dn931943.aspx). Etkinlik günlüğü olaylarını almak için bkz: [bir abonelik yönetimi olayları listesinde](https://msdn.microsoft.com/library/azure/dn931934.aspx).

## <a name="next-steps"></a>Sonraki adımlar

* Power BI ile Azure etkinlik günlükleri aboneliğinizde eylemler hakkında daha fazla öngörü elde etmek için kullanılır. Bkz: [Görünüm ve Power BI ve daha fazla Azure etkinlik günlüklerini analiz edin](https://azure.microsoft.com/blog/analyze-azure-audit-logs-in-powerbi-more/).
* Güvenlik ilkelerini ayarlama bilgi edinmek için [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md).
* Dağıtım işlemlerini görüntülemek için komutları hakkında bilgi edinmek için [görüntülemek dağıtım işlemlerini](resource-manager-deployment-operations.md).
* Tüm kullanıcılar için bir kaynakta Silmeleri Engelle öğrenmek için bkz: [Azure Resource Manager ile kaynakları kilitleme](resource-group-lock-resources.md).
* Her bir Microsoft Azure Resource Manager sağlayıcısı için kullanılabilir işlemleri listesini görmek için bkz: [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md)

