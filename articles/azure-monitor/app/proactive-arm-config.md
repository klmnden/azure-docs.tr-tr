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
ms.date: 02/07/2019
ms.reviewer: mbullwin
ms.author: harelbr
ms.openlocfilehash: 3ab50c92543615488d9ced599df433bf7e1e4061
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61461570"
---
# <a name="manage-application-insights-smart-detection-rules-using-azure-resource-manager-templates"></a>Azure Resource Manager şablonlarını kullanarak Application Insights akıllı algılama kurallarını yönetme

Application Insights, akıllı algılama kuralları yönetilebilir ve kullanılarak yapılandırılan [Azure Resource Manager şablonları](../../azure-resource-manager/resource-group-authoring-templates.md).
Bu yöntem, yeni Application Insights kaynakları mevcut kaynakların ayarlarını değiştirmek için veya Azure Resource Manager otomasyon ile dağıtım yaparken kullanılabilir.

## <a name="smart-detection-rule-configuration"></a>Akıllı algılama kuralı yapılandırması

Bir akıllı algılama kuralına için aşağıdaki ayarları yapılandırabilirsiniz:
- Kural etkin olduğunda (varsayılan değer **true**.)
- Abonelik sahiplerine e-posta gönderilmesi gerekiyorsa, Katkıda Bulunanlar ve okuyucular bir algılama zaman bulundu (varsayılan değer **true**.)
- Bir algılama bulunduğu zaman bir bildirim almanız gerekir herhangi ek bir e-posta alıcıları.
- * E-posta yapılandırmasını kullanılamaz olarak akıllı algılama kuralları işaretlenmiş için _Önizleme_.

Azure Resource Manager aracılığıyla kural ayarları yapılandırma izin vermek için akıllı algılama kuralı yapılandırması adlı Application Insights kaynağı içinde bir iç kaynak olarak sunuldu **ProactiveDetectionConfigs**.
Düzeyde esneklik için her bir akıllı algılama kuralına benzersiz bildirim ayarları ile yapılandırılabilir.

## <a name="examples"></a>Örnekler

Aşağıda Azure Resource Manager şablonlarını kullanarak akıllı algılama kuralları ayarlarının nasıl yapılandırılacağını gösteren birkaç örnek verilmiştir.
Tüm örnekleri adlı bir Application Insights kaynağına başvurmak _"myApplication"_, ve "uzun bağımlılık süresi akıllı algılama kuralına" olarak dahili olarak adlandırılmış _"longdependencyduration"_.
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

## <a name="smart-detection-rule-names"></a>Akıllı algılama kuralı adları

Aşağıda Azure Resource Manager şablonunda kullanılan iç adlarını birlikte portalında göründükleri gibi bir tablo akıllı algılama kuralı adları verilmiştir.

> [!NOTE]
> Akıllı algılama kuralları e-posta bildirimleri Önizleme desteklemez olarak işaretlenmiş. Bu nedenle, yalnızca bu kuralları için enabled özelliğini ayarlayabilirsiniz. 

| Azure portal kural adı | İç adı
|:---|:---|
| Yavaş sayfa yükleme süresi | slowpageloadtime |
| Yavaş sunucu yanıtı süresi | slowserverresponsetime |
| Uzun bağımlılık süresi | longdependencyduration |
| Sunucu yanıt süresinde performans düşüşü | degradationinserverresponsetime |
| Bağımlılık süresi düşüşü | degradationindependencyduration |
| İzleme önem derecesi oranı (Önizleme) içinde performans düşüşü | extension_traceseveritydetector |
| Olağan dışı özel durum hacmindeki artışı (Önizleme) | extension_exceptionchangeextension |
| Olası bellek sızıntısı algılandı (Önizleme) | extension_memoryleakextension |
| Olası bir güvenlik sorunu algılandı (Önizleme) | extension_securityextensionspackage |
| Kaynak kullanımı sorunu algılandı (Önizleme) | extension_resourceutilizationextensionspackage |

## <a name="who-receives-the-classic-alert-notifications"></a>Kimin (Klasik) Uyarı bildirimlerini alır?

Bu bölüm yalnızca, akıllı algılama Klasik uyarılar için geçerlidir ve yalnızca istenen alıcılarınız bildirimlerini aldığından emin olmak için Uyarı bildirimlerini iyileştirmenize yardımcı olur. Arasındaki fark hakkında daha fazla anlamak için [Klasik uyarılar](../platform/alerts-classic.overview.md) ve yeni uyarılar deneyimini başvurduğu [uyarılar genel bakış makalesi](../platform/alerts-overview.md). Şu anda yalnızca klasik uyarılar deneyimini desteği akıllı algılama uyarıları. Bir özel durum [akıllı algılama uyarıları azure'da bulut Hizmetleri](./proactive-cloud-services.md). Uyarı denetlemek için Azure bulutunda akıllı algılama uyarılar için Bildirim Hizmetleri kullanım [Eylem grupları](../platform/action-groups.md).

* Akıllı algılama/Klasik Uyarı bildirimlerini belirli alıcılara kullanılmasını öneririz.

* Akıllı algılama uyarıları için **toplu/grup** abonelik sahibi, katkıda bulunan veya okuyucu rollerine sahip kullanıcılar için onay kutusu seçeneği etkinleştirilirse, gönderir. Aslında, _tüm_ abonelik Application Insights kaynağına erişimi olan kullanıcılar kapsamındaki ve ilgili bildirimler alacaksınız. 

> [!NOTE]
> Şu anda kullanıyorsanız **toplu/grup** onay kutusu seçeneğini ve devre dışı bırakmak, bu değişikliği geri almak mümkün olmayacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

Otomatik olarak algılama hakkında daha fazla bilgi edinin:

- [Hata anormallikleri](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Bellek sızıntıları](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Performans anormallikleri](../../azure-monitor/app/proactive-performance-diagnostics.md)