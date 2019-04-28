---
title: Azure güvenlik duvarı günlükleri'ne genel bakış
description: Bu makalede, Azure güvenlik duvarı tanılama günlükleri bir genel bakıştır.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 9/24/2018
ms.author: victorh
ms.openlocfilehash: c129c394f3d694b832722287027c1f9e58028a33
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61065861"
---
# <a name="azure-firewall-logs"></a>Azure güvenlik duvarı günlükleri

Güvenlik duvarı günlüklerini kullanarak Azure Güvenlik Duvarı'nı izleyebilirsiniz. Ayrıca etkinlik günlüklerini kullanarak Azure Güvenlik Duvarı kaynaklarıyla ilgili işlemleri denetleyebilirsiniz.

Bu günlüklerden bazılarına portaldan erişebilirsiniz. Günlükleri gönderilebilir [Azure İzleyici günlükleri](../azure-monitor/insights/azure-networking-analytics.md), depolama ve Event Hubs ve Azure İzleyici günlüklerine veya Excel ve Power BI gibi farklı araçları tarafından analiz edilir.

## <a name="diagnostic-logs"></a>Tanılama günlükleri

 Azure Güvenlik Duvarı'nda aşağıdaki tanılama günlükleri mevcuttur:

* **Uygulama kuralı günlüğü**

   Uygulama kuralı günlüğü, Event hubs'a akış ve/veya Azure İzleyici günlüklerine yalnızca her Azure Güvenlik Duvarı için etkinleştirdiyseniz, gönderilen bir depolama hesabına kaydedilir. Yapılandırdığınız uygulama kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

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

   Ağ kuralı günlüğü, Event hubs'a akış ve/veya Azure İzleyici günlüklerine yalnızca her Azure Güvenlik Duvarı için etkinleştirdiyseniz, gönderilen bir depolama hesabına kaydedilir. Yapılandırdığınız ağ kurallarınızla eşleşen yeni bağlantılar kabul edilen/reddedilen bağlantı için bir günlük oluşturur. Veriler aşağıdaki örnekte gösterildiği gibi JSON biçiminde günlüğe kaydedilir:

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

* **Depolama hesabı**: Günlükleri uzun bir süre için depolanır ve gerektiğinde gözden depolama hesapları en iyi günlükler için kullanılır.
* **Olay hub'ları**: Olay hub'ları, kaynaklarınız üzerinde uyarıları almak için diğer güvenlik bilgileri ve Olay yönetimi (SEIM) araçları ile tümleştirmeye yönelik mükemmel bir seçenektir ' dir.
* **Azure İzleyici günlüklerine**: Azure İzleyici günlüklerine en iyi şekilde kullanılır uygulamanızın genel gerçek zamanlı izleme veya eğilimlere bakmaya.

## <a name="activity-logs"></a>Etkinlik günlükleri

   Etkinlik günlüğü girişleri varsayılan olarak toplanır ve bunları Azure portalda görüntüleyebilirsiniz.

   [Azure etkinlik günlüklerini](../azure-resource-manager/resource-group-audit.md) (eski adıyla işlem günlükleri ve denetim günlükleri) kullanarak Azure aboneliğinize gönderilmiş olan tüm işlemleri görüntüleyebilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar

Azure güvenlik duvarı günlükleri ve ölçümleri izleme öğrenmek için bkz: [Öğreticisi: Azure güvenlik duvarı günlükleri izleme](tutorial-diagnostics.md).