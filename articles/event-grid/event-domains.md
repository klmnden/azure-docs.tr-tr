---
title: Azure Event Grid olay etki alanları
description: Olay etki alanlarını Azure Event Grid konuları yönetmek için nasıl kullanıldığını açıklar.
services: event-grid
author: banisadr
ms.service: event-grid
ms.author: babanisa
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: 61821caa2450096bdbdde3461316ad21a82f6f18
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66304292"
---
# <a name="understand-event-domains-for-managing-event-grid-topics"></a>Event Grid konuları yönetmek için olay etki alanlarını anlama

Bu makalede, çeşitli iş kuruluşlar, müşteriler veya uygulamalar için özel olaylar akışını yönetmek için olay etki alanı kullanmayı açıklar. Olay etki alanları için kullanın:

* Çok kiracılı olay mimarileri uygun ölçekte yönetin.
* Yetkilendirme ve kimlik doğrulama yönetin.
* Her ayrı ayrı yönetmenize gerek kalmadan, konuları bölümleme.
* Tek tek her konuda uç noktalarınızı yayımlamaktan kaçının.

## <a name="event-domain-overview"></a>Olay etki alanı genel bakış

Bir olay etki alanı bir Event Grid konuları aynı uygulamayla ilgili çok sayıda yönetim aracıdır. Bunu bir meta-tek tek ilgili konulara binlerce olan konu düşünebilirsiniz.

Olay etki alanı olayları yayımlamak için Azure Hizmetleri (gibi depolama ve IOT hub'ı) tarafından kullanılan aynı mimariye kullanılabilir hale. Konuları binlerce için olayları yayımlama olanak sağlar. Böylece kiracılarınız bölümleyebilirsiniz etki alanları, ayrıca her konu yetkilendirme ve kimlik doğrulama denetiminin kendilerinde.

### <a name="example-use-case"></a>Örnek Kullanım örneği

Olay etki alanları, en kolay bir örnek kullanarak açıklanmıştır. Burada ekipmanları ve ağır diğer makineler bakıldığında tractors, üretim Contoso Construction makineler çalıştırdığınız varsayalım. İş çalıştıran bir parçası olarak, müşteriler ekipmanı bakımına, sistemi durumunun ve sözleşme güncelleştirmeleri hakkında gerçek zamanlı bilgi gönderin. Tüm bu bilgileri gider uygulamanıza dahil olmak üzere çeşitli uç noktaları, müşteri uç noktaları ve müşterilerin ayarlamış olduğunuz diğer altyapı.

Olay etki alanı modeline Contoso Construction makineler tek olay varlık olarak sağlar. Müşterilerinizin her biri, etki alanı içindeki bir konuya olarak temsil edilir. Azure Active Directory'yi kullanarak kimlik doğrulaması ve yetkilendirme işlenir. Müşterilerinizin her biri kendi konuya abone olur ve teslim olayları kendilerine atanan. Bunlar, yalnızca kendi konuya erişebilirsiniz olay etki alanı aracılığıyla yönetilen erişimden sağlar.

Ayrıca, tüm müşteri olaylarınızı yayımlayabilirsiniz tek uç tanır. Olay Kılavuzu her konuda yalnızca olaylar için kendi Kiracı kapsamlı farkında olduğundan emin olma ilgileniriz.

![Contoso Construction örneği](./media/event-domains/contoso-construction-example.png)

## <a name="access-management"></a>Erişim Yönetimi

Bir etki alanı ile hedeflemediğinizden yetkilendirme ve kimlik doğrulama denetimini aracılığıyla Azure'nın rol tabanlı erişim denetimi (RBAC) her bir konuda ele alın. Bu roller, uygulamanızda yalnızca erişim vermek istediğiniz konulara her Kiracı kısıtlamak için kullanabilirsiniz.

RBAC olay etki alanlarında aynı şekilde çalışır [yönetilen erişim denetimi](security-authentication.md#management-access-control) Event Grid ve Azure rest içinde çalışır. RBAC, oluşturmak ve özel rol tanımları olay alanlarındaki zorlamak için kullanın.

### <a name="built-in-roles"></a>Yerleşik roller

Event Grid olay etki alanı ile çalışmak için RBAC kolaylaştırmak için iki yerleşik rol tanımları vardır. Bu roller **EventGrid EventSubscription katkıda bulunan (Önizleme)** ve **EventGrid EventSubscription Okuyucu (Önizleme)** . Bu roller, konular, olay etki alanı, abone olmak için ihtiyaç duyan kullanıcılar atayın. Kullanıcıların abone olmak için gereken konuya rol ataması kapsam.

Bu roller hakkında daha fazla bilgi için bkz. [Event Grid için yerleşik roller](security-authentication.md#built-in-roles).

## <a name="subscribing-to-topics"></a>Başlıklarına abone

Olayları bir olay etki alanı içindeki bir konuya abone olma aynıdır [bir olay aboneliği bir özel konu oluşturma](./custom-event-quickstart.md) veya bir Azure hizmetinden bir olaya abone olma.

### <a name="domain-scope-subscriptions"></a>Etki alanı kapsamı abonelikler

Olay etki alanı için etki alanı kapsamına abonelikler de sağlar. Bir olay aboneliği olay etki alanı üzerinde etki alanına olayları gönderilen konu bağımsız olarak gönderilen tüm olaylarını alacaksınız. Etki alanı kapsamı abonelikler için yönetim ve denetim amacıyla yararlı olabilir.

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

Olay etki alanı konuları için bir yayımlama sizin için işler. Tek tek yönettiğiniz her bir konuda yayımlama olaylarına yerine tüm olaylarınızı etki alanının uç noktasına yayımlayabilirsiniz. Olay Kılavuzu her olay doğru konuya gönderilen emin olur.

## <a name="limits-and-quotas"></a>Limitler ve kotalar
Limitler ve kotalar olay etki alanlarıyla ilgili şunlardır:

- Olay etki alanı başına 100.000 konuları 
- Azure aboneliği başına 100 olay etki alanı 
- bir olay etki alanı, konu başına 500 olay abonelikleri
- 50 etki alanı kapsamı abonelikler 
- İkinci alımı oranı (içine, bir etki alanı) başına 5.000 olayları

Bu sınırlar, uymuyorsa ürün ekibi bir destek bileti açarak veya bir e-posta göndererek ulaşmak [ askgrid@microsoft.com ](mailto:askgrid.microsoft.com). 

## <a name="pricing"></a>Fiyatlandırma
Olay etki alanları kullanır aynı [fiyatlandırma operations](https://azure.microsoft.com/pricing/details/event-grid/) , Event Grid diğer tüm özelliklerini kullanın.

Özel konulardaki yaptıkları gibi işlemleri aynı olay alanlarındaki çalışır. Her bir olay etki alanı için bir olay girişi bir işlemdir ve her bir olay için teslim denemesi bir işlemdir.

## <a name="next-steps"></a>Sonraki adımlar

* Oluşturma konuları, olay abonelikleri oluşturma ve olayları, olay etki alanları hakkında bilgi edinmek için bkz [olay etki alanlarını yönet](./how-to-event-domains.md).