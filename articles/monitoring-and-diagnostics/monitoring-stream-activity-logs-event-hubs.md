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
ms.openlocfilehash: 2cd3e2e471135242b52459abc231a0f3545e05e1
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="stream-the-azure-activity-log-to-event-hubs"></a>Akış Event hubs'a Azure etkinlik günlüğü
[ **Azure etkinlik günlüğü** ](monitoring-overview-activity-logs.md) portalında veya Azure PowerShell aracılığıyla bir günlük profilinde Service Bus kural kimliği etkinleştirerek yerleşik "Verme" seçeneğini kullanarak herhangi bir uygulama için yakın gerçek zamanlı akış Cmdlet'lerini veya Azure CLI.

## <a name="what-you-can-do-with-the-activity-log-and-event-hubs"></a>Etkinlik günlüğü ve olay hub'ları ile yapabilecekleriniz
Etkinlik günlüğü için akış özelliği kullanabilir birkaç yollar şunlardır:

* **Üçüncü taraf günlüğe kaydetme ve telemetri sistemlerde akışına** – zaman içinde olay hub'ları akış üçüncü taraf Sıem'lerden etkinlik günlüğü kanal ve analiz çözümleri oturum mekanizması olur.
* **Özel telemetri ve günlüğe kaydetme platform derleme** – yayımlama-abone yapısı bir özel olarak geliştirilmiş telemetri platform veya olan biri, yüksek düzeyde ölçeklenebilir oluşturma hakkında düşünüyorum yalnızca zaten varsa Event Hubs, esnek etkinlik günlüğü alma olanak sağlar. [Olay hub'ları bir küresel ölçekteki telemetri platform burada kullanmanın Dan Rosanova'nın Kılavuzu'na bakın.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-the-activity-log"></a>Etkinlik günlüğü akışı etkinleştir
Etkinlik günlüğü program aracılığıyla veya portal aracılığıyla akış etkinleştirebilirsiniz. Her iki durumda da, bir olay hub'ları ad alanı ve bu ad alanı için bir paylaşılan erişim ilkesi seçin ve ilk yeni etkinlik günlüğü olay gerçekleştiğinde bir olay hub ' name 'Öngörüler-günlükleri-operationallogs' ile bu ad alanında oluşturulur. Bir olay hub'ları ad alanı yoksa, öncelikle bir oluşturmanız gerekir. Daha önce bu olay hub'ları ad etkinlik günlüğü olaylarını akışı değilse, daha önce oluşturduğunuz olay hub'ı yeniden kullanılabilir. Paylaşılan Erişim İlkesi akış mekanizmaya sahip izinleri tanımlar. Günümüzde, olay hub'ına akış gerektirir **Yönet**, **Gönder**, ve **dinleme** izinleri. Oluşturun veya olay hub'ları ad alanı paylaşılan erişim ilkeleri Azure portalında "Yapılandırma" sekmesinin altında ad alanı için değiştirin. Değişiklik yapmadan kullanıcı Akış dahil etmek için etkinlik günlüğü günlük profilini güncelleştirmek için bu olay hub'ı yetkilendirme kuralı ListKey izni olmalıdır.

Her iki aboneliğin uygun RBAC erişimi ayarı yapılandıran kullanıcının sahip olduğu sürece günlükleri yayma abonelik ile aynı abonelikte olması olay hub'ları ad alanı yok.

### <a name="via-azure-portal"></a>Azure portalı üzerinden
1. Gidin **etkinlik günlüğü** tüm kullanma bölümüne portalın sol tarafındaki arama hizmetleri.
   
    ![Etkinlik günlüğü portalında gidin](./media/monitoring-stream-activity-logs-event-hubs/activity.png)
2. Tıklatın **verme** etkinlik günlüğü üstündeki düğmesi. Önceki görünümünde etkinlik günlüğü görüntülerken uygulanan filtre ayarlarını dışa aktarma ayarlarınızı etkilemez gerektiğini unutmayın--yalnızca, etkinlik günlüğü portalında göz atarken bkz filtreleme için olanlardır.
   
    ![Portalında Dışarı Aktar](./media/monitoring-stream-activity-logs-event-hubs/export.png)
3. Görüntülenen bölümünde **tüm bölgelere**. Belirli bölgeler seçmeyin.
   
    ![Etkinlik Günlüğünü Dışarı Aktar](./media/monitoring-stream-activity-logs-event-hubs/export-audit.png)
    
    > [!WARNING]
    > Yalnızca 'Tüm bölgelere.' seçin Aksi takdirde, aksi takdirde almak için beklenen sahip anahtar olayları eksik. Etkinlik günlüğü çoğu olayları ilişkili bir bölgesi yoktur genel (Bölgesel olmayan) günlük kitabıdır due için bu olabilir.
    >
    >
    
4. Tıklatın **kaydetmek** bu ayarları kaydetmek için. Ayarları, aboneliğinizi hemen uygulanır.
5. Birden fazla aboneliğiniz varsa, bu eylemi yineleyin ve aynı olay hub'ına tüm verileri gönder.

### <a name="via-powershell-cmdlets"></a>PowerShell cmdlet'leri
Bir günlük profili zaten varsa, önce bu profili kaldırmanız gerekir.

1. Kullanım `Get-AzureRmLogProfile` günlük profili olup olmadığını belirlemek için
2. Aksi takdirde kullanmak `Remove-AzureRmLogProfile` kaldırmak için.
3. Kullanım `Set-AzureRmLogProfile` bir profil oluşturmak için:

```powershell

Add-AzureRmLogProfile -Name my_log_profile -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus -RetentionInDays 90 -Categories Write,Delete,Action

```

Hizmet veri yolu kural kimliği bu biçiminde bir dizedir: {hizmet veri yolu kaynak kimliği} /authorizationrules/ {anahtar adı}, örneğin 

### <a name="via-azure-cli"></a>Via Azure CLI
Bir günlük profili zaten varsa, önce bu profili kaldırmanız gerekir.

1. Kullanım `azure insights logprofile list` günlük profili olup olmadığını belirlemek için
2. Aksi takdirde kullanmak `azure insights logprofile delete` kaldırmak için.
3. Kullanım `azure insights logprofile add` bir profil oluşturmak için:

```azurecli-interactive
azure insights logprofile add --name my_log_profile --storageId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Storage/storageAccounts/my_storage --serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey --locations global,westus,eastus,northeurope --retentionInDays 90 –categories Write,Delete,Action
```

Hizmet veri yolu kural kimliği bu biçiminde bir dizedir: `{service bus resource ID}/authorizationrules/{key name}`.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Olay hub'ları günlük verilerini nasıl kullanabilir?
[Etkinlik günlüğü için şemayı buraya kullanılabilir](monitoring-overview-activity-logs.md). Her bir olaydır JSON BLOB'ları "kayıtlar." olarak adlandırılan bir dizi

## <a name="next-steps"></a>Sonraki Adımlar
* [Arşiv depolama hesabı etkinlik günlüğü](monitoring-archive-activity-log.md)
* [Azure etkinlik günlüğü bakış okuma](monitoring-overview-activity-logs.md)
* [Etkinlik günlüğü olaya dayanarak bir uyarı ayarlama](insights-auditlog-to-webhook-email.md)

