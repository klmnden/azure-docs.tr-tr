---
title: Azure İzleyici uyarılar için ortak uyarı şeması
description: Neden ve nasıl etkinleştirileceğini kullanmanız gerektiğini ortak bir uyarı şemasını anlama
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: anantr
ms.subservice: alerts
ms.openlocfilehash: 91ec5aa42367f6caaa93aaf808fde504e92fbc04
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67594333"
---
# <a name="common-alert-schema"></a>Ortak uyarı şeması

Ortak uyarı şema nedir, bu makalede ve bunun nasıl etkinleştirileceğini kullanmanın avantajları.

## <a name="what-is-the-common-alert-schema"></a>Ortak uyarı şema nedir?

Ortak uyarı şema için uyarı bildirimleri Azure tüketim deneyimi bugün standart hale getirir. Tarihsel olarak, üç uyarı türlerini azure'da bugün (ölçümü, günlük ve etkinlik günlüğü) kendi e-posta şablonları, Web kancası şemaları vb. kalmışlardır. Ortak uyarı şeması ile tutarlı bir şema ile uyarı bildirimleri alabilir.

Herhangi bir uyarı örneği açıklar **etkilenen kaynak** ve **uyarının nedenini**, ve bu örnekler, ortak şema aşağıdaki bölümlerde açıklanmıştır:
* **Essentials**: Bir dizi **standart alanlar**açıklayan tüm uyarı türleri arasında ortak **hangi kaynak** uyarı ek ortak uyarı meta verileri birlikte (örneğin, önem derecesi veya açıklama) belirtir. 
* **Uyarı bağlamı**: Bir dizi tanımlayan alanları **uyarının neden**, farklı alanlara sahip **uyarı türüne göre**. Örneğin, bir etkinlik günlüğü uyarısı uyarının olayla ilgili bilgileri yoktur ancak bir ölçüm uyarısı uyarı bağlamındaki ölçüm değeri ve ölçüm adı gibi alanlarını gerekir. 

Müşterilerden aldığımız tipik tümleştirme senaryolarına sonra sorumlu takım çalışma üzerinde başladığı bazı pivot (örneğin, kaynak grubu), temel endişe ekibine bir uyarı örneğinin yönlendirme gerektirir. Ortak uyarı şeması ile yönlendirme mantığı uyarı türleri arasında önemli alanlar yararlanarak daha fazla araştırmak ilgili takımlar için bağlam alan bırakarak standartlaştırılmış.

Bu, büyük olasılıkla daha az tümleştirmeleri, yönetme ve bunların bakımını yapma işlemi yapmadan olamayacağı anlamına gelir bir _çok_ daha basit bir görev. Ayrıca, gelecekte uyarı yükü zenginleştirmelerinin (örneğin, özelleştirme, tanılama zenginleştirme, vb.) yalnızca yukarı ortak şemada belirir.

## <a name="what-enhancements-does-the-common-alert-schema-bring"></a>Ortak uyarı şema hangi geliştirmeleri getirir?

Ortak uyarı şema öncelikle kendisi, Uyarı bildirimlerini bildirilmez. Göreceğiniz iyileştirmeleri aşağıda listelenmiştir:

| Action | Geliştirmeler|
|:---|:---|
| SMS | Tüm uyarı türleri için tutarlı bir SMS şablonu. |
| Email | Bir bakışta sorunlarını kolayca tanılamanıza olanak tanıyan tutarlı ve ayrıntılı bir e-posta şablonu. Portal ve etkilenen kaynak uyarı örneği için katıştırılmış derin bağlantılar hızlı düzeltme işlemine geçebilirsiniz emin olun. |
| Web kancası/Logic App/Azure işlevi/Otomasyon Runbook'u | Tümleştirmeler farklı uyarı türleri arasında kolayca oluşturmanıza olanak tanıyan tutarlı bir JSON yapı tüm uyarı türleri için. |

Yeni Şema ayrıca daha zengin bir uyarı tüketim deneyimi Azure portalı ve Azure mobil uygulaması yakın gelecekte etkinleştirecektir. 

[Web kancaları/Logic Apps/Azure işlevleri/Automation runbook'ları için şema tanımları hakkında daha fazla bilgi edinin.](https://aka.ms/commonAlertSchemaDefinitions)

> [!NOTE]
> Ortak uyarı Şeması aşağıdaki eylemleri desteklemez: ITSM Bağlayıcısı.

## <a name="how-do-i-enable-the-common-alert-schema"></a>Ortak uyarı şema nasıl etkinleştirebilirim?

REST API ve her iki portal üzerinde eylem grupları aracılığıyla ortak uyarı şema çevirmek ya da kabul et. Yeni şemaya geçiş yapmak için iki durumlu bir eylem düzeyindedir. Örneğin, ayrı olarak e-posta eylem ve bir Web kancası eylemi için katılım gerekir.

> [!NOTE]
> 1. Aşağıdaki uyarı türleri varsayılan olarak ortak şema desteği (hiçbir iyileştirilmiş gerekli):
>     * Akıllı algılama uyarıları
> 1. Aşağıdaki uyarı türleri, ortak şema şu anda desteklemez:
>     * Tarafından oluşturulan uyarıların [VM'ler için Azure İzleyici](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)
>     * Tarafından oluşturulan uyarıların [Azure maliyet Yönetimi](https://docs.microsoft.com/azure/billing/billing-cost-management-budget-scenario)

### <a name="through-the-azure-portal"></a>Azure portalı üzerinden

![Ortak uyarı şeması kabul etme](media/alerts-common-schema/portal-opt-in.png)

1. Tüm mevcut veya yeni bir eylem içinde bir eylem grubu açın. 
1. 'Evet' gösterildiği gibi ortak uyarı şeması sağlamak için iki durumlu düğmeyi seçin.

### <a name="through-the-action-groups-rest-api"></a>Eylem grupları REST API

Ayrıca [Eylem grupları API](https://docs.microsoft.com/rest/api/monitor/actiongroups) içinde ortak uyarı şemaya katılmak için. Yapma sırasında [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) REST API çağrısı, "'(kabul et) doğru olarak ' bayrağı useCommonAlertSchema" veya 'false' (geri çevirmek için) - Web kancası/e-posta/logic app/Azure işlevi/Otomasyon runbook'u aşağıdaki eylemlerden herhangi birini ayarlayabilirsiniz.

Örneğin, aşağıdaki istek gövdesi yapılan [oluşturma veya güncelleştirme](https://docs.microsoft.com/rest/api/monitor/actiongroups/createorupdate) REST API aşağıdakileri yapın:

* "John Doe'nun e-posta" e-posta eylemi için ortak bir uyarı şema etkinleştir
* Ortak uyarı şema "Jane Smith'in e-posta" e-posta eylemi için devre dışı bırak
* Web kancası eylemi "Örnek Web kancası" için ortak bir uyarı şema etkinleştir

```json
{
  "properties": {
    "groupShortName": "sample",
    "enabled": true,
    "emailReceivers": [
      {
        "name": "John Doe's email",
        "emailAddress": "johndoe@email.com",
        "useCommonAlertSchema": true
      },
      {
        "name": "Jane Smith's email",
        "emailAddress": "janesmith@email.com",
        "useCommonAlertSchema": false
      }
    ],
    "smsReceivers": [
      {
        "name": "John Doe's mobile",
        "countryCode": "1",
        "phoneNumber": "1234567890"
      },
      {
        "name": "Jane Smith's mobile",
        "countryCode": "1",
        "phoneNumber": "0987654321"
      }
    ],
    "webhookReceivers": [
      {
        "name": "Sample webhook",
        "serviceUri": "http://www.example.com/webhook",
        "useCommonAlertSchema": true
      }
    ]
  },
  "location": "Global",
  "tags": {}
}
```





## <a name="next-steps"></a>Sonraki adımlar

- [Web kancaları/Logic Apps/Azure işlevleri/Automation runbook'ları için ortak uyarı şema tanımları.](https://aka.ms/commonAlertSchemaDefinitions)
- [Tüm uyarılarınız işlemek için ortak uyarı şema yararlanan bir mantıksal uygulama oluşturmayı öğrenin.](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-integrations) 



