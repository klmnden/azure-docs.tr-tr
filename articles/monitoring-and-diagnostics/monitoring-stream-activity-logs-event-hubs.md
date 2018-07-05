---
title: Azure etkinlik günlüğünün Event Hubs'a Stream
description: Azure etkinlik günlüğünün Event Hubs'a akışını yapma hakkında bilgi edinin.
author: johnkemnetz
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/02/2018
ms.author: johnkem
ms.component: activitylog
ms.openlocfilehash: 45352c1cf4aca9043c23bbe12e94ba770a38c01b
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436714"
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Azure etkinlik günlüğünün Event Hubs'a Stream
Akış [Azure etkinlik günlüğü](monitoring-overview-activity-logs.md) neredeyse gerçek zamanlı olarak ya da herhangi bir uygulama için:

* Yerleşik kullanarak **dışarı** portalında seçeneği
* Azure PowerShell cmdlet'leri aracılığıyla bir günlük profili Azure Service Bus kural kimliği veya Azure CLI'yı etkinleştirme

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Etkinlik günlüğü ve olay hub'ları ile yapabilecekleriniz
Etkinlik günlüğü için akış özelliği kullanabileceğinize iki yolu vardır:

* **Üçüncü taraf günlüğe kaydetme ve telemetri sistemleriyle Stream**: zaman içinde Azure Event Hubs akış etkinlik günlüğünüzü üçüncü taraf Sıem'lerden kanal oluşturarak ve analiz çözümleri oturum için bir mekanizma olur.
* **Özel telemetri ve günlüğe kaydetme platform derleme**: zaten ürettikleri telemetri platformu varsa veya biri oluşturma hakkında düşünmeye olan yüksek düzeyde ölçeklenebilir Yayımla-doğasını abone ol olay hub'ları esnek etkinlik günlüğü alma olanak sağlar. Daha fazla bilgi için [küresel ölçekli bir telemetri platform Event Hubs'ı kullanma hakkında video Dan Rosanova'nın](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## <a name="enable-streaming-of-the-activity-log"></a>Etkinlik günlüğü akışını etkinleştir
Programlama yoluyla veya portal aracılığıyla etkinlik günlüğü akışını etkinleştirebilirsiniz. Her iki durumda da bir Event Hubs ad alanı ve bu ad alanı için bir paylaşılan erişim ilkesi seçin. İlk yeni etkinlik günlüğü olay gerçekleştiğinde bir olay hub'ı ınsights-günlükleri-operationallogs adı ile o ad alanında oluşturulur. 

Bir Event Hubs ad alanı yoksa, öncelikle bir oluşturmanız gerekir. Daha önce bu Event Hubs ad alanı için etkinlik günlüğü olayları akış, daha önce oluşturduğunuz olay hub'ı yeniden kullanılabilir. 

Paylaşılan Erişim İlkesi akış mekanizması olan izni tanımlar. Bugün, Event Hubs'a akış gerektirir **Yönet**, **Gönder**, ve **dinleme** izinleri. Oluşturma veya değiştirme altında Azure portalında Event Hubs ad alanı için paylaşılan erişim ilkeleri **yapılandırma** Event Hubs ad alanınız için sekmesinde. 

Etkinlik günlüğü günlük profilini akış içerecek şekilde güncelleştirmek için değişikliği yapan kullanıcının bu Event hubs'ı yetkilendirme kuralı ListKey izni olmalıdır. Event Hubs ad alanı ayarı yapılandıran kullanıcının her iki abonelik için uygun RBAC erişimine sahip olduğu sürece, günlükleri yayan aboneliği ile aynı abonelikte olması gerekmez.

### <a name="via-the-azure-portal"></a>Azure portalı üzerinden
1. Gözat **etkinlik günlüğü** kullanarak bölümü **tüm hizmetleri** portalın sol tarafındaki arama.
   
   ![Etkinlik günlüğü Portalı'ndaki Hizmetler listesinden seçme](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Seçin **dışarı** günlüğü üstünde düğme.
   
   ![Portalda Dışarı Aktar](./media/monitoring-stream-activity-logs-event-hubs/export.png)

   Etkinlik günlüğü önceki görünümünde görüntülerken, uyguladığınız filtresi ayarları dışarı aktarma ayarlarınıza göre herhangi bir etkisi gerektiğini unutmayın. Etkinlik günlüğünüzü portalında atarken bkz yalnızca filtrelemesinde olanlardır.
3. Görüntülenen bölümde, seçin **tüm bölgeler**. Belirli bölgelerde seçmeyin.
   
   ![Dışarı aktarma bölümü](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)

   > [!WARNING]  
   > Dışında herhangi bir şey seçeneğini belirlerseniz **tüm bölgeler**, almayı beklediğiniz anahtar olayları kaçırmayın. Etkinlik günlüğüne genel bir (Bölgesel olmayanlar) günlük olduğundan, çoğu olayları kendileriyle ilişkilendirilmiş bir bölgeye sahip değildir. 
   >

4. Seçin **Kaydet** bu ayarları kaydedin. Ayarlar aboneliğinize hemen uygulanır.
5. Birden fazla aboneliğiniz varsa, bu eylemi yineleyin ve tüm verileri aynı olay hub'ına gönderir.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri
Günlük profilini zaten varsa önce mevcut günlük profilini kaldırın ve ardından yeni bir günlük profili oluşturmak gerekir.

1. Kullanım `Get-AzureRmLogProfile` günlük profili var olmadığını belirleyin.  Günlük profilini mevcut değilse bulun *adı* özelliği.
2. Kullanım `Remove-AzureRmLogProfile` değerini kullanarak günlük profilini kaldırmak için *adı* özelliği.

    ```powershell
    # For example, if the log profile name is 'default'
    Remove-AzureRmLogProfile -Name "default"
    ```
3. Kullanım `Add-AzureRmLogProfile` yeni bir günlük profili oluşturmak için:

   ```powershell
   # Settings needed for the new log profile
   $logProfileName = "default"
   $locations = (Get-AzureRmLocation).Location
   $locations += "global"
   $subscriptionId = "<your Azure subscription Id>"
   $resourceGroupName = "<resource group name your event hub belongs to>"
   $eventHubNamespace = "<event hub namespace>"

   # Build the service bus rule Id from the settings above
   $serviceBusRuleId = "/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.EventHub/namespaces/$eventHubNamespace/authorizationrules/RootManageSharedAccessKey"

   Add-AzureRmLogProfile -Name $logProfileName -Location $locations -ServiceBusRuleId $serviceBusRuleId
   ```

### <a name="via-azure-cli"></a>Azure CLI
Günlük profilini zaten varsa önce mevcut günlük profilini kaldırın ve ardından yeni bir günlük profili oluşturmak gerekir.

1. Kullanım `az monitor log-profiles list` günlük profili var olmadığını belirleyin.
2. Kullanım `az monitor log-profiles delete --name "<log profile name>` değerini kullanarak günlük profilini kaldırmak için *adı* özelliği.
3. Kullanım `az monitor log-profiles create` yeni bir günlük profili oluşturmak için:

   ```azurecli-interactive
   az monitor log-profiles create --name "default" --location null --locations "global" "eastus" "westus" --categories "Delete" "Write" "Action"  --enabled false --days 0 --service-bus-rule-id "/subscriptions/<YOUR SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP NAME>/providers/Microsoft.EventHub/namespaces/<EVENT HUB NAME SPACE>/authorizationrules/RootManageSharedAccessKey"
   ```

## <a name="consume-the-log-data-from-event-hubs"></a>Event Hubs günlük verilerini kullanma
Etkinlik günlüğü için şema kullanılabilir [Azure etkinlik günlüğü ile abonelik etkinliğini İzle](monitoring-overview-activity-logs.md). JSON bloblarını adlı bir dizide her olaydır *kayıtları*.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir depolama hesabı için Etkinlik günlüğünü arşivleme](monitoring-archive-activity-log.md)
* [Azure etkinlik günlüğüne genel bakış okuyun](monitoring-overview-activity-logs.md)
* [Bir etkinlik günlüğü olayı temel alan bir uyarı ayarlama](insights-auditlog-to-webhook-email.md)

