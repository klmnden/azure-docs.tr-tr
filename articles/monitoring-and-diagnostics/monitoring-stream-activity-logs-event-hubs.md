---
title: "Olay hub'ları için Azure etkinlik günlüğü akış | Microsoft Docs"
description: "Olay hub'ları için Azure etkinlik günlüğü akış öğrenin."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ec4c2d2c-8907-484f-a910-712403a06829
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2018
ms.author: johnkem
ms.openlocfilehash: 4b2d9866839f943f65beb271d44bc691441b0fb3
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Akış Event hubs'a Azure etkinlik günlüğü
Akış [Azure etkinlik günlüğü](monitoring-overview-activity-logs.md) yakın gerçek zamanlı olarak ya da herhangi bir uygulama için:

* Yerleşik kullanarak **verme** portalında seçeneği
* Azure PowerShell cmdlet'leri aracılığıyla bir günlük profilinde Azure Service Bus kural kimliği veya Azure CLI etkinleştirme

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Etkinlik günlüğü ve olay hub'ları ile yapabilecekleriniz
Burada, etkinlik günlüğü için akış özelliği kullanabilir iki yolu bulunmaktadır:

* **Üçüncü taraf günlüğe kaydetme ve telemetri sistemlerde akışına**: zaman içinde Azure Event Hubs akış üçüncü taraf Sıem'lerden etkinlik günlüğü kanal ve analiz çözümleri oturum mekanizması olur.
* **Bir özel telemetri ve günlüğe kaydetme platformu yapı**: zaten bir özel olarak geliştirilmiş telemetri platform veya bir oluşturma konusunda planlayın, yüksek düzeyde ölçeklenebilir yayımlama yapısını abonelik Event Hubs esnek etkinlik günlüğü alma olanak sağlar. Daha fazla bilgi için bkz: [bir küresel ölçekli telemetri platform olay hub'ları kullanma hakkında Dan Rosanova'nın video](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Etkinlik günlüğü akışı etkinleştir
Etkinlik günlüğü program aracılığıyla veya portal aracılığıyla akış etkinleştirebilirsiniz. Her iki durumda da, bir olay hub'ları ad alanı ve bu ad alanı için bir paylaşılan erişim ilkesi seçin. İlk yeni etkinlik günlüğü olay gerçekleştiğinde bir olay hub'Öngörüler-günlükleri-operationallogs adı ile bu ad alanında oluşturulur. 

Bir olay hub'ları ad alanı yoksa, öncelikle bir oluşturmanız gerekir. Daha önce bu olay hub'ları ad etkinlik günlüğü olaylarını akışı, daha önce oluşturduğunuz olay hub'ı yeniden kullanılabilir. 

Paylaşılan Erişim İlkesi akış mekanizmaya sahip izinleri tanımlar. Günümüzde, Event Hubs'a akış gerektirir **Yönet**, **Gönder**, ve **dinleme** izinleri. Oluşturma veya olay hub'ları ad alanı altında Azure portalında için paylaşılan erişim ilkeleri değiştirmek için **yapılandırma** sekmesinde olay hub'ları ad alanınız için. 

Değişikliği yapan kullanıcının akış dahil etmek için etkinlik günlüğü günlük profilini güncelleştirmek için bu olay hub'ları yetkilendirme kuralı ListKey izni olmalıdır. Her iki aboneliğin uygun RBAC erişimi ayarı yapılandıran kullanıcının sahip olduğu sürece günlükleri, yayma abonelik ile aynı abonelikte olması olay hub'ları ad alanı yok.

### <a name="via-the-azure-portal"></a>Azure portalı üzerinden
1. Gözat **etkinlik günlüğü** kullanarak bölüm **tüm hizmetleri** portalın sol tarafındaki arama.
   
   ![Etkinlik günlüğü Hizmetleri portalında listesinden seçme](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Seçin **verme** günlük üstündeki düğmesi.
   
   ![Portalda Dışarı Aktar](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Önceki görünümünde etkinlik günlüğü görüntülerken uygulanan filtre ayarlarını dışa aktarma ayarlarınızı etkilemez gerektiğini unutmayın. Yalnızca, etkinlik günlüğü Portalı aracılığıyla göz atarken bkz filtreleme için izinlerdir.
3. Görüntülenen bölümünde **tüm bölgelere**. Belirli bölgeler seçmeyin.
   
   ![Bölüm dışarı aktarma](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Herhangi bir şey dışında seçerseniz **tüm bölgelere**, almayı beklediğiniz anahtar olayları kaçırılması. Etkinlik günlüğü genel (Bölgesel olmayan) günlük olduğundan, çoğu olayları ilişkili bir bölgesi yoktur. 
   > 

4. Seçin **kaydetmek** bu ayarları kaydetmek için. Ayarları, aboneliğinizi hemen uygulanır.
5. Birden fazla aboneliğiniz varsa, bu eylemi yineleyin ve aynı olay hub'ına tüm verileri gönder.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri
Bir günlük profili zaten varsa, önce bu profili kaldırmanız gerekir.

1. Kullanım `Get-AzureRmLogProfile` günlük profili olup olmadığını belirlemek için.
2. Aksi takdirde kullanmak `Remove-AzureRmLogProfile` kaldırmak için.
3. Kullanım `Set-AzureRmLogProfile` bir profil oluşturmak için:

   ```powershell

   Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action

   ```

Hizmet veri yolu kural kimliği bu biçiminde bir dizedir: `{service bus resource ID}/authorizationrules/{key name}`. 

### <a name="via-azure-cli"></a>Via Azure CLI
Bir günlük profili zaten varsa, önce bu profili kaldırmanız gerekir.

1. Kullanım `azure insights logprofile list` günlük profili olup olmadığını belirlemek için.
2. Aksi takdirde kullanmak `azure insights logprofile delete` kaldırmak için.
3. Kullanım `azure insights logprofile add` bir profil oluşturmak için:

   ```azurecli-interactive
   azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
   ```

Hizmet veri yolu kural kimliği bu biçiminde bir dizedir: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="consume-the-log-data-from-event-hubs"></a>Olay hub'ları günlük verilerini kullanma
Etkinlik günlüğü için şemayı kullanılabilir [Azure etkinlik günlüğü ile abonelik etkinliğini İzle](monitoring-overview-activity-logs.md). Her bir olaydır adlı JSON BLOB'ları bir dizi *kayıtları*.

## <a name="next-steps"></a>Sonraki adımlar
* [Arşiv depolama hesabı etkinlik günlüğü](monitoring-archive-activity-log.md)
* [Azure etkinlik günlüğü bakış okuma](monitoring-overview-activity-logs.md)
* [Etkinlik günlüğü olaya dayanarak bir uyarı ayarlama](insights-auditlog-to-webhook-email.md)

