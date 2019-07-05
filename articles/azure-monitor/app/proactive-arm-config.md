---
title: Azure Resource Manager şablonları ile Azure Application Insights, akıllı algılama kuralı ayarlarını yapılandırma | Microsoft Docs
description: Yönetimi ve Azure Resource Manager şablonları ile Azure Application Insights akıllı algılama kuralları yapılandırmasını otomatikleştirin
services: application-insights
documentationcenter: ''
author: harelbr
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/26/2019
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: 6bb89eec0b4905e101bed87d3d3fc617dec589e0
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477854"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Application Insights akıllı algılama kurallarını yönetme

Application Insights, akıllı algılama kuralları yönetilebilir ve kullanılarak yapılandırılan [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-authoring-templates.md).
Bu yöntem, yeni Application Insights kaynakları mevcut kaynakların ayarlarını değiştirmek için veya Azure Resource Manager otomasyon ile dağıtım yaparken kullanılabilir.

## <a name="smart-detection-rule-configuration"></a>Akıllı algılama kuralı yapılandırması

Bir akıllı algılama kuralına için aşağıdaki ayarları yapılandırabilirsiniz:
- Kural etkin olduğunda (varsayılan değer **true**.)
- Aboneliğin ilişkili kullanıcılara e-posta gönderilip gönderilemeyeceğini [izleme okuyucusu](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) ve [izleme katılımcı](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) bir algılama bulunduğunda rolleri (varsayılan değer **true**.)
- Bir algılama bulunduğu zaman bir bildirim almanız gerekir herhangi ek bir e-posta alıcıları.
    -  E-posta yapılandırmasını kullanılamaz olarak akıllı algılama kuralları işaretlenmiş için _Önizleme_.

Azure Resource Manager aracılığıyla kural ayarları yapılandırma izin vermek için akıllı algılama kuralı yapılandırması adlı Application Insights kaynağı içinde bir iç kaynak olarak sunuldu **ProactiveDetectionConfigs**.
Düzeyde esneklik için her bir akıllı algılama kuralına benzersiz bildirim ayarları ile yapılandırılabilir.

## 

## <a name="examples"></a>Örnekler

Aşağıda Azure Resource Manager şablonlarını kullanarak akıllı algılama kuralları ayarlarının nasıl yapılandırılacağını gösteren birkaç örnek verilmiştir.
Tüm örnekleri adlı bir Application Insights kaynağına başvurmak _"myApplication"_ , ve "uzun bağımlılık süresi akıllı algılama kuralına" olarak dahili olarak adlandırılmış _"longdependencyduration"_ .
Application Insights kaynak adı değiştirin ve ilgili akıllı algılama kuralı iç adını belirtmek için emin olun. Akıllı algılama, her kural için karşılık gelen iç Azure Resource Manager adlarının listesi için aşağıdaki tabloya bakın.

### <a name="disable-a-smart-detection-rule"></a>Bir akıllı algılama kuralı devre dışı bırak

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": [],
            "enabled": false
          }
        }
      ]
    }
```

### <a name="disable-sending-email-notifications-for-a-smart-detection-rule"></a>Gönderen e-posta bildirimleri için bir akıllı algılama kuralına devre dışı bırak

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": false,
            "customEmails": [],
            "enabled": true
          }
        }
      ]
    }
```

### <a name="add-additional-email-recipients-for-a-smart-detection-rule"></a>Bir akıllı algılama kuralına ek e-posta alıcıları ekleyin

```json
{
      "apiVersion": "2018-05-01-preview",
      "name": "myApplication",
      "type": "Microsoft.Insights/components",
      "location": "[resourceGroup().location]",
      "properties": {
        "ApplicationId": "myApplication"
      },
      "resources": [
        {
          "apiVersion": "2018-05-01-preview",
          "name": "longdependencyduration",
          "type": "ProactiveDetectionConfigs",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[resourceId('Microsoft.Insights/components', 'myApplication')]"
          ],
          "properties": {
            "name": "longdependencyduration",
            "sendEmailsToSubscriptionOwners": true,
            "customEmails": ['alice@contoso.com', 'bob@contoso.com'],
            "enabled": true
          }
        }
      ]
    }

```

### <a name="failure-anomalies-v2-non-classic-alert-rule"></a>Hata Anomalileri v2 (Klasik olmayan) uyarı kuralı

Bu Azure Resource Manager şablonu, bir hata Anomalileri v2 uyarı kuralı 2 önem derecesine sahip yapılandırma gösterilmektedir. Hata Anomalileri uyarı kuralı bu yeni sürümü yeni Azure platformu uyarı bir parçasıdır ve bir parçası olarak devre dışı bırakılıyor Klasik sürümü değiştirir [Klasik uyarıları devre dışı bırakma işlemi](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "microsoft.alertsmanagement/smartdetectoralertrules",
            "apiVersion": "2019-03-01",
            "name": "Failure Anomalies - my-app",
            "properties": {
                  "description": "Detects a spike in the failure rate of requests or dependencies",
                  "state": "Enabled",
                  "severity": "2",
                  "frequency": "PT1M",
                  "detector": {
                  "id": "FailureAnomaliesDetector"
                  },
                  "scope": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/MyResourceGroup/providers/microsoft.insights/components/my-app"],
                  "actionGroups": {
                        "groupIds": ["/subscriptions/00000000-1111-2222-3333-444444444444/resourcegroups/MyResourceGroup/providers/microsoft.insights/actiongroups/MyActionGroup"]
                  }
            }
        }
    ]
}
```

> [!NOTE]
> Bu Azure Resource Manager şablonu hata Anomalileri v2 uyarı kuralı için benzersiz olan ve diğer Klasik akıllı algılama kuralları farklı bu makalede açıklanmıştır.   

## <a name="smart-detection-rule-names"></a>Akıllı algılama kuralı adları

Aşağıda Azure Resource Manager şablonunda kullanılan iç adlarını birlikte portalında göründükleri gibi bir tablo akıllı algılama kuralı adları verilmiştir.

> [!NOTE]
> Akıllı algılama kuralları olarak işaretlenmiş _Önizleme_ e-posta bildirimleri desteklemez. Bu nedenle, yalnızca ayarlayabilirsiniz _etkin_ özelliği bu kurallar için. 

| Azure portal kural adı | İç adı
|:---|:---|
| Yavaş sayfa yükleme süresi | slowpageloadtime |
| Yavaş sunucu yanıtı süresi | slowserverresponsetime |
| Uzun bağımlılık süresi | longdependencyduration |
| Sunucu yanıt süresinde performans düşüşü | degradationinserverresponsetime |
| Bağımlılık süresinde düşüş | degradationindependencyduration |
| İzleme önem derecesi oranı (Önizleme) içinde performans düşüşü | extension_traceseveritydetector |
| Olağan dışı özel durum hacmindeki artışı (Önizleme) | extension_exceptionchangeextension |
| Olası bellek sızıntısı algılandı (Önizleme) | extension_memoryleakextension |
| Olası bir güvenlik sorunu algılandı (Önizleme) | extension_securityextensionspackage |
| Olağan dışı artış, günlük veri hacmi (Önizleme) | extension_billingdatavolumedailyspikeextension |

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anormallikleri](../../azure-monitor/app/proactive-performance-diagnostics.md)
