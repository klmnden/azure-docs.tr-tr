---
title: "E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemlerini kullanın. | Microsoft Belgeleri"
description: "Otomatik ölçeklendirme eylemleri web URL'leri arayın veya Azure İzleyicisi'nde e-posta bildirimleri göndermek için nasıl kullanılacağını bakın. "
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: eb9a4c98-0894-488c-8ee8-5df0065d094f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2017
ms.author: ancav
ms.openlocfilehash: 16caf14028494800e9259f0296c292b606d0210a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>E-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın
Bu makalede, böylece belirli web URL'leri çağrısı veya Azure otomatik ölçeklendirme eylemleri göre e-postalar göndermek Tetikleyicileri nasıl ayarlama gösterilir.  

## <a name="webhooks"></a>Web kancaları
Web kancası, diğer sistemlere işlem sonrası veya özel bildirimleri için Azure uyarı bildirimleri yönlendirmek izin verir. Örneğin, uyarıyı sohbet kullanarak veya Mesajlaşma bir team services SMS, günlük hataları, bildirim göndermek için gelen bir web isteği işleyebilecek services, vb. için yönlendirme. Web kancası URI geçerli bir HTTP veya HTTPS uç noktası olması gerekir.

## <a name="email"></a>E-posta
Tüm geçerli e-posta adresine e-posta gönderilebilir. Yöneticiler ve kural çalıştığı abonelik ortak yöneticileri aynı zamanda bildirilecek.

## <a name="cloud-services-and-web-apps"></a>Bulut Hizmetleri ve Web uygulamaları
Azure portalından bulut Hizmetleri ve sunucu grupları (Web uygulamaları) için katılımı.

* Seçin **göre ölçeklendirmeniz** ölçüm.

![tarafından ölçeklendirme](./media/insights-autoscale-to-webhook-email/insights-autoscale-notify.png)

## <a name="virtual-machine-scale-sets"></a>Sanal makine ölçekleme kümeleri
Yeni Kaynak Yöneticisi (sanal makine ölçek kümeleri) ile oluşturulan sanal makineler için bunu REST API, Resource Manager şablonları, PowerShell ve CLI kullanarak yapılandırabilirsiniz. Bir portal arabirimi henüz kullanılabilir değil.
REST API veya Resource Manager şablonu kullanırken, aşağıdaki seçeneklere sahip bildirimleri öğesi ekleyin.

```
"notifications": [
      {
        "operation": "Scale",
        "email": {
          "sendToSubscriptionAdministrator": false,
          "sendToSubscriptionCoAdministrators": false,
          "customEmails": [
              "user1@mycompany.com",
              "user2@mycompany.com"
              ]
        },
        "webhooks": [
          {
            "serviceUri": "https://foo.webhook.example.com?token=abcd1234",
            "properties": {
              "optional_key1": "optional_value1",
              "optional_key2": "optional_value2"
            }
          }
        ]
      }
    ]
```
| Alan | Zorunlu? | Açıklama |
| --- | --- | --- |
| işlemi |Evet |değerin "Ölçek" olması gerekir |
| sendToSubscriptionAdministrator |Evet |değer "true" veya "false" olmalıdır |
| sendToSubscriptionCoAdministrators |Evet |değer "true" veya "false" olmalıdır |
| customEmails |Evet |değer null [] veya e-postaların dize dizisi olabilir |
| Web kancaları |Evet |değer null veya geçerli URI olabilir |
| serviceUri |Evet |Geçerli bir https URI |
| properties |Evet |Değer boş {} olması gerekir veya anahtar-değer çiftleri içerebilir |

## <a name="authentication-in-webhooks"></a>Web kancası kimlik doğrulaması
Web kancası Web kancası URI sorgu parametresi olarak bir belirteç kimliği ile kaydettiğiniz belirteç tabanlı kimlik doğrulamasını kullanarak kimlik doğrulaması yapabilir. Örneğin, https://mysamplealert/webcallback?tokenid=sometokenid&someparameter=somevalue

## <a name="autoscale-notification-webhook-payload-schema"></a>Otomatik ölçeklendirme bildirim Web kancası yükü şeması
Otomatik ölçeklendirme bildirim oluşturulduğunda, aşağıdaki meta verileri Web kancası yükünde eklenmiştir:

```
{
        "version": "1.0",
        "status": "Activated",
        "operation": "Scale In",
        "context": {
                "timestamp": "2016-03-11T07:31:04.5834118Z",
                "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/myautoscaleSetting",
                "name": "myautoscaleSetting",
                "details": "Autoscale successfully started scale operation for resource 'MyCSRole' from capacity '3' to capacity '2'",
                "subscriptionId": "s1",
                "resourceGroupName": "rg1",
                "resourceName": "MyCSRole",
                "resourceType": "microsoft.classiccompute/domainnames/slots/roles",
                "resourceId": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService/slots/Production/roles/MyCSRole",
                "portalLink": "https://portal.azure.com/#resource/subscriptions/s1/resourceGroups/rg1/providers/microsoft.classicCompute/domainNames/myCloudService",
                "oldCapacity": "3",
                "newCapacity": "2"
        },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
}
```


| Alan | Zorunlu? | Açıklama |
| --- | --- | --- |
| durum |Evet |Otomatik ölçeklendirme eylemi oluşturulan gösteren durum |
| işlemi |Evet |Artışı örnekleri için "Ölçeği Genişlet" olur ve bir düşüş için örnekleri, "Ölçek içinde" olacaktır |
| bağlam |Evet |Otomatik ölçeklendirme eylem bağlamı |
| timestamp |Evet |Otomatik ölçeklendirme eylemi tetiklendiğinde zaman damgası |
| id |Evet |Otomatik ölçeklendirme ayarında Resource Manager kimliği |
| ad |Evet |Otomatik ölçeklendirme ayarı adı |
| Ayrıntıları |Evet |Otomatik ölçeklendirme hizmet sürdü eylem ve örnek sayısı değişikliği açıklaması |
| subscriptionId |Evet |Genişletilmiş hedef kaynak abonelik kimliği |
| resourceGroupName |Evet |Genişletilmiş hedef kaynak kaynak grubu adı |
| resourceName |Evet |Genişletilmiş hedef kaynağın adı |
| Kaynak türü |Evet |Üç desteklenen değerler: "microsoft.classiccompute/domainnames/slots/roles" - bulut hizmeti rolleri, "microsoft.compute/virtualmachinescalesets" - sanal makine ölçek kümeleri ve "Microsoft.Web/serverfarms" - Web uygulaması |
| resourceId |Evet |Genişletilmiş hedef kaynak Resource Manager kimliği |
| portalLink |Evet |Hedef kaynak Özet sayfasında Azure portalı bağlantı |
| oldCapacity |Evet |Bir ölçek eylemi otomatik ölçeklendirme geçen zaman geçerli (eski) örnek sayısı |
| newCapacity |Evet |Otomatik ölçeklendirme kaynağa ölçeklendirilmiş yeni örnek sayısı |
| Özellikler |Hayır |İsteğe bağlı. < Anahtar, değer > kümesi çiftleri (örneğin, sözlük < dize, dize >). Özellikler alanı isteğe bağlıdır. Özel kullanıcı arabirimi veya mantığı tabanlı uygulama iş akışı, anahtarlar ve yük kullanılarak geçirilebilir değerler girebilirsiniz. Web kancası URI kendisini (gibi sorgu parametrelerini) kullanmak için özel özellikler giden Web kancası çağrı geçirmek için alternatif bir yoludur. |
