---
title: E-posta ve Web kancası uyarı bildirimleri göndermek için otomatik ölçeklendirme kullanma
description: "Otomatik ölçeklendirme eylemleri web URL'leri arayın veya Azure İzleyicisi'nde e-posta bildirimleri göndermek için nasıl kullanılacağını bakın. "
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/03/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 65405a6d7f1d49911da1e2a5d26b02098a261c01
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35262231"
---
# <a name="use-autoscale-actions-to-send-email-and-webhook-alert-notifications-in-azure-monitor"></a>E-posta ve Web kancası Azure İzleyicisi'nde uyarı bildirimleri göndermek için otomatik ölçeklendirme eylemleri kullanın
Bu makalede, böylece belirli web URL'leri çağrısı veya Azure otomatik ölçeklendirme eylemleri göre e-postalar göndermek Tetikleyicileri nasıl ayarlama gösterilir.  

## <a name="webhooks"></a>Web Kancaları
Web kancası, diğer sistemlere işlem sonrası veya özel bildirimleri için Azure uyarı bildirimleri yönlendirmek izin verir. Örneğin, uyarıyı sohbet kullanarak veya Mesajlaşma bir team services SMS, günlük hataları, bildirim göndermek için gelen bir web isteği işleyebilecek services, vb. için yönlendirme. Web kancası URI geçerli bir HTTP veya HTTPS uç noktası olması gerekir.

## <a name="email"></a>Email
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
| işlem |evet |değerin "Ölçek" olması gerekir |
| sendToSubscriptionAdministrator |evet |değer "true" veya "false" olmalıdır |
| sendToSubscriptionCoAdministrators |evet |değer "true" veya "false" olmalıdır |
| customEmails |evet |değer null [] veya e-postaların dize dizisi olabilir |
| Web kancaları |evet |değer null veya geçerli URI olabilir |
| serviceUri |evet |Geçerli bir https URI |
| properties |evet |Değer boş olmalıdır {} veya anahtar-değer çiftleri içerebilir |

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
| durum |evet |Otomatik ölçeklendirme eylemi oluşturulan gösteren durum |
| işlem |evet |Artışı örnekleri için "Ölçeği Genişlet" olur ve bir düşüş için örnekleri, "Ölçek içinde" olacaktır |
| bağlam |evet |Otomatik ölçeklendirme eylem bağlamı |
| timestamp |evet |Otomatik ölçeklendirme eylemi tetiklendiğinde zaman damgası |
| id |Evet |Otomatik ölçeklendirme ayarında Resource Manager kimliği |
| ad |Evet |Otomatik ölçeklendirme ayarı adı |
| ayrıntılar |Evet |Otomatik ölçeklendirme hizmet sürdü eylem ve örnek sayısı değişikliği açıklaması |
| subscriptionId |Evet |Genişletilmiş hedef kaynak abonelik kimliği |
| resourceGroupName |Evet |Genişletilmiş hedef kaynak kaynak grubu adı |
| resourceName |Evet |Genişletilmiş hedef kaynağın adı |
| Kaynak türü |Evet |Üç desteklenen değerler: "microsoft.classiccompute/domainnames/slots/roles" - bulut hizmeti rolleri, "microsoft.compute/virtualmachinescalesets" - sanal makine ölçek kümeleri ve "Microsoft.Web/serverfarms" - Web uygulaması |
| resourceId |Evet |Genişletilmiş hedef kaynak Resource Manager kimliği |
| portalLink |Evet |Hedef kaynak Özet sayfasında Azure portalı bağlantı |
| oldCapacity |Evet |Bir ölçek eylemi otomatik ölçeklendirme geçen zaman geçerli (eski) örnek sayısı |
| newCapacity |Evet |Otomatik ölçeklendirme kaynağa ölçeklendirilmiş yeni örnek sayısı |
| Özellikler |Hayır |İsteğe bağlı. < Anahtar, değer > kümesi çiftleri (örneğin, sözlük < dize, dize >). Özellikler alanı isteğe bağlıdır. Özel kullanıcı arabirimi veya mantığı tabanlı uygulama iş akışı, anahtarlar ve yük kullanılarak geçirilebilir değerler girebilirsiniz. Web kancası URI kendisini (gibi sorgu parametrelerini) kullanmak için özel özellikler giden Web kancası çağrı geçirmek için alternatif bir yoludur. |
