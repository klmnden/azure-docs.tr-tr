---
title: Azure Event Grid olay etki alanları
description: Olay etki alanlarını Azure Event Grid konuları yönetmek için nasıl kullanıldığını açıklar.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 10/30/2018
ms.openlocfilehash: fe66ca8b8f5b4474290e302f73b35868dce68caa
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50634320"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Olay ızgarası konu başlıkları yönetmek için olay etki alanlarını anlama

Bu makalede, çeşitli iş düzenlemeler, müşteriler veya uygulamalar için özel olaylar akışını yönetmek için olay etki alanı kullanmayı açıklar. Olay etki alanları için kullanın:

* Çok kiracılı olay mimarileri uygun ölçekte yönetin.
* Yetkilendirme ve kimlik doğrulama yönetin.
* Her ayrı ayrı yönetmenize gerek kalmadan, konuları bölümleme.
* Tek tek her konuda uç noktalarınızı yayımlamaktan kaçının.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]


## <a name="event-domains-overview"></a>Olay etki alanlarına genel bakış

Bir olay etki alanı için olay ızgarası konu başlıkları aynı uygulamayla ilgili çok sayıda yönetim aracıdır. Bunu bir meta-tek tek ilgili konulara binlerce içeren konu düşünebilirsiniz.

Olay etki alanı kendi sisteminizde kullanmak için olayları yayımlamak için kullandığınız depolama ve IOT Hub gibi Azure Hizmetleri mimarisi olun. Konuları binlerce için olayları yayımlama olanak sağlar. Böylece kiracılarınız bölümleyebilirsiniz etki alanları, ayrıca her konu yetkilendirme ve kimlik doğrulama denetiminin kendilerinde.

### <a name="example-use-case"></a>Örnek Kullanım örneği

Olay etki alanları, en kolay bir örnek kullanarak açıklanmıştır. Burada ekipmanları ve ağır diğer makineler bakıldığında tractors, üretim Contoso Construction makineler çalıştırdığınız varsayalım. İş çalıştıran bir parçası olarak, müşterilerin gerçek zamanlı bilgi gönderme sistemi durumunun ekipmanı bakımına güncelleştirmeleri, vb. sözleşme. Uygulama, müşteri uç noktaları da dahil olmak üzere çeşitli Uç noktalara giden tüm bu bilgileri ve diğer altyapı müşterilerin sahip.

Olay etki alanı sayesinde Contoso Construction makineler modeline tek olay varlık olarak. Müşterilerinizin her biri etki alanı içindeki bir konuya olarak temsil edilen / kimlik doğrulama ve yetkilendirme işlenmesini Azure Active Directory'yi kullanarak. Müşterilerinizin her biri kendi konuya abone olur ve teslim olayları kendilerine atanan. Bunlar, yalnızca kendi konuya erişebilirsiniz olay etki alanı aracılığıyla yönetilen erişimden sağlar.

Ayrıca, tüm müşteri olaylarınızı yayımlayabilirsiniz tek uç tanır. Olay Kılavuzu her konuda yalnızca olaylar için kendi Kiracı kapsamlı farkında olduğundan emin olma ilgileniriz.

![Contoso Construction örneği](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Erişim Yönetimi

 Bir etki alanı ile hedeflemediğinizden yetkilendirme ve kimlik doğrulama denetimini aracılığıyla Azure'nın rol tabanlı erişim denetleyin (RBAC) her bir konuda ele alın. Bu roller, uygulamanızda yalnızca erişim vermek istediğiniz konulara her Kiracı kısıtlamak için kullanabilirsiniz.

Rol tabanlı erişim denetleyin (RBAC) olay etki alanlarında aynı şekilde çalışır [yönetilen erişim denetimi](https://docs.microsoft.com/azure/event-grid/security-authentication#management-access-control) Event Grid ve Azure rest içinde çalışır. RBAC, oluşturmak ve özel rol tanımları olay alanlarındaki zorlamak için kullanın.

### <a name="built-in-roles"></a>Yerleşik roller

Event Grid, RBAC kolaylaştırmak için iki yerleşik rol tanımlarına sahiptir:

#### <a name="eventgrid-eventsubscription-contributor-preview"></a>EventGrid EventSubscription katkıda bulunan (Önizleme)

```json
[
  {
    "Description": "Lets you manage EventGrid event subscription operations.",
    "IsBuiltIn": true,
    "Id": "428e0ff05e574d9ca2212c70d0e0a443",
    "Name": "EventGrid EventSubscription Contributor (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/*",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Resources/deployments/*",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Support/*"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "Condition": null
      }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

#### <a name="eventgrid-eventsubscription-reader-preview"></a>EventGrid EventSubscription Okuyucu (Önizleme)

```json
[
  {
    "Description": "Lets you read EventGrid event subscriptions.",
    "IsBuiltIn": true,
    "Id": "2414bbcf64974faf8c65045460748405",
    "Name": "EventGrid EventSubscription Reader (Preview)",
    "IsServiceRole": false,
    "Permissions": [
      {
        "Actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.EventGrid/eventSubscriptions/read",
          "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/eventSubscriptions/read",
          "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": []
       }
    ],
    "Scopes": [
      "/"
    ]
  }
]
```

## <a name="subscribing-to-topics"></a>Başlıklarına abone

Olayları bir olay etki alanı içindeki bir konuya abone olma aynıdır [bir olay aboneliği bir özel konu oluşturma](./custom-event-quickstart.md) veya olay yayımcıları Azure yerleşik birine abone sunar.

### <a name="domain-scope-subscriptions"></a>Etki alanı kapsamı abonelikler

Olay etki alanı için etki alanı kapsamı abonelikler de sağlar. Bir olay aboneliği olay etki alanı üzerinde etki alanına olayları gönderilen konu bağımsız olarak gönderilen tüm olaylarını alacaksınız. Etki alanı kapsamı abonelikler için yönetim ve denetim amacıyla yararlı olabilir.

## <a name="publishing-to-an-event-domain"></a>Bir olay etki alanı yayımlama

Bir olay etki alanı oluşturduğunuzda, Event grid'de bir konu oluşturduysanız, benzer bir yayımlama uç verilir. 

Bir olay etki alanındaki herhangi bir konunun olayları yayımlamak için etki alanının uç noktasına olayları gönderme [bir özel konu için yaptığınız şekilde](./post-to-custom-topic.md). Tek fark, teslim edilmek üzere Olay istediğiniz konu belirtmeniz gerekir.

Örneğin, aşağıdaki olaylar dizisi yayımlama olayı gönderir gibi `"id": "1111"` konuya `foo` while olayla `"id": "2222"` konu başlığına gönderilen `bar`:

```json
[{
  "topic": "foo",
  "id": "1111",
  "eventType": "maintenanceRequested",
  "subject": "myapp/vehicles/diggers",
  "eventTime": "2018-10-30T21:03:07+00:00",
  "data": {
    "make": "Contoso",
    "model": "Small Digger"
  },
  "dataVersion": "1.0"
},
{
  "topic": "bar",
  "id": "2222",
  "eventType": "maintenanceCompleted",
  "subject": "myapp/vehicles/tractors",
  "eventTime": "2018-10-30T21:04:12+00:00",
  "data": {
    "make": "Contoso",
    "model": "Big Tractor"
  },
  "dataVersion": "1.0"
}]
```

Olay etki alanı konuları için bir yayımlama sizin için işler. Olayları tek tek yönetmek her konu başlığında yayımlamaya, yerine tüm olaylarınızı etki alanının uç noktasına yayımlayabilir ve Event Grid üstlenir doğru konuya gönderilen her olay sağlama.

## <a name="limits-and-quotas"></a>Limitler ve kotalar

### <a name="control-plane"></a>Denetim düzlemi

Önizleme sırasında olay etki alanları içindeki bir etki alanı 1.000 konuları ve konu içerisinde bir etki alanı başına 50 olay abonelikleri sınırlı olacaktır. Olay etki alanı kapsamı Abonelikleri, ayrıca 50 sınırlı olacaktır.

### <a name="data-plane"></a>Veri düzlemi

Önizleme sırasında bir olay etki alanı için olay işleme hızı aynı 5.000 olaylara başına özel konu için sınırlı ikinci alımı oranı sınırlı olacaktır.

## <a name="pricing"></a>Fiyatlandırma

Önizleme sırasında aynı olay etki alanları kullanır [fiyatlandırma operations](https://azure.microsoft.com/pricing/details/event-grid/) , Event Grid diğer tüm özelliklerini kullanın.

Özel konulardaki yaptıkları gibi işlemleri aynı olay alanlarındaki çalışır. Her bir olay etki alanı için bir olay girişi bir işlemdir ve her bir olay için teslim denemesi bir işlemdir.

## <a name="next-steps"></a>Sonraki adımlar

* Oluşturma konuları, olay abonelikleri oluşturma ve olayları, olay etki alanları hakkında bilgi edinmek için bkz [olay etki alanlarını yönet](./how-to-event-domains.md).