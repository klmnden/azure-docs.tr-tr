---
title: Azure güvenlik duvarı günlükleri'ne genel bakış
description: Bu makalede, Azure güvenlik duvarı tanılama günlükleri bir genel bakıştır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: 0698f1dbc491781089ef94eec32f2a427fd3cca4
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/27/2018
ms.locfileid: "52422397"
---
# <a name="azure-firewall-logs"></a>Azure güvenlik duvarı günlükleri

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz.

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükler [Log Analytics](../azure-monitor/insights/azure-networking-analytics.md), Depolama ve Event Hubs'a gönderilebilir, Log Analytics'te veya Excel ve Power BI gibi farklı araçlarda analiz edilebilir.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

 Azure Güvenlik Duvarı'nda aşağıdaki tanılama günlükleri mevcuttur:

* **Uygulama kuralı günlüğü**

   Uygulama kuralı günlüğünü depolama hesabına kaydetmek, Event Hubs'a aktarmak ve/veya Log Analytics'e göndermek için her bir Azure Güvenlik Duvarı'nda etkinleştirmiş olmanız gerekir. Yapılandırdığınız uygulama kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: application rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
   {
    "category": "AzureFirewallApplicationRule",
    "time": "2018-04-16T23:45:04.8295030Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallApplicationRuleLog",
    "properties": {
        "msg": "HTTPS request from 10.1.0.5:55640 to mydestination.com:443. Action: Allow. Rule Collection: collection1000. Rule: rule1002"
    }
   }
   ```

* **Ağ kuralı günlüğü**

   Ağ kuralı günlüğünü depolama hesabına kaydetmek, Event Hubs'a aktarmak ve/veya Log Analytics'e göndermek için her bir Azure Güvenlik Duvarı'nda etkinleştirmiş olmanız gerekir. Yapılandırdığınız ağ kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

   ```
   Category: network rule logs.
   Time: log timestamp.
   Properties: currently contains the full message. 
   note: this field will be parsed to specific fields in the future, while maintaining backward compatibility with the existing properties field.
   ```

   ```json
  {
    "category": "AzureFirewallNetworkRule",
    "time": "2018-06-14T23:44:11.0590400Z",
    "resourceId": "/SUBSCRIPTIONS/{subscriptionId}/RESOURCEGROUPS/{resourceGroupName}/PROVIDERS/MICROSOFT.NETWORK/AZUREFIREWALLS/{resourceName}",
    "operationName": "AzureFirewallNetworkRuleLog",
    "properties": {
        "msg": "TCP request from 111.35.136.173:12518 to 13.78.143.217:2323. Action: Deny"
    }
   }

   ```

Günlüklerinizi depolamak için kullanabileceğiniz üç seçenek vardır:

* **Depolama hesabı**: Depolama hesaplarının en iyi kullanım amacı, günlüklerin uzun süre depolanması ve ihtiyaç duyulduğunda gözden geçirilmesi durumlarıdır.
* **Event Hubs**: Event Hubs, kaynaklarınızla ilgili uyarılar almak için diğer güvenlik bilgisi ve olay yönetimi (SEIM) araçlarıyla tümleştirmek için idealdir.
* **Log Analytics**: Log Analytics'in en iyi kullanım amacı, uygulamanızın gerçek zamanlı olarak izlenmesi veya eğilimlerin incelenmesidir.

## <a name="activity-logs"></a>Etkinlik günlükleri

   Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.

   [Azure etkinlik günlüklerini](../azure-resource-manager/resource-group-audit.md) (eski adıyla işlem günlükleri ve denetim günlükleri) kullanarak Azure aboneliğinize gönderilmiş olan tüm işlemleri görüntüleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik duvarı günlükleri ve ölçümleri izleme öğrenmek için bkz. [öğretici: Azure Güvenlik Duvarı İzleme günlükleri](tutorial-diagnostics.md).