---
title: "Azure Service Bus tanılama günlüklerini | Microsoft Docs"
description: "Azure'da hizmet veri yolu için tanılama günlüklerini ayarlamak öğrenin."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/16/2017
ms.author: sethm
ms.openlocfilehash: 2bf65b7c5b0518da59e767db18fe6f4193e0ab6e
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="service-bus-diagnostic-logs"></a>Hizmet veri yolu tanılama günlükleri

Azure hizmet veri yolu için iki tür günlükleri görüntüleyebilirsiniz:
* **[Etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Bu günlükler bir iş üzerinde gerçekleştirilen işlemler hakkında bilgi içerir. Günlükleri her zaman etkindir.
* **[Tanılama günlüklerini](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. İçinde bir işi olur daha zengin hakkında bilgi için tanılama günlüklerini yapılandırabilirsiniz. Tanılama güncelleştirmeleri ve iş çalışırken oluşan etkinlikler dahil olmak üzere iş silinene kadar işin oluşturulduğu zamandan itibaren kapak etkinlikleri günlüğe kaydeder.

## <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini Aç

Tanılama günlükleri, varsayılan olarak devre dışıdır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları gerçekleştirin:

1.  İçinde [Azure portal](https://portal.azure.com)altında **izleme + Yönetim**, tıklatın **tanılama günlükleri**.

    ![Tanılama günlüklerini dikey gezinme](./media/service-bus-diagnostic-logs/image1.png)

2. İzlemek istediğiniz kaynak'ı tıklatın.  

3.  Tıklatın **tanılamayı açın**.

    ![Tanılama günlüklerini Aç](./media/service-bus-diagnostic-logs/image2.png)

4.  İçin **durum**, tıklatın **üzerinde**.

    ![Tanılama günlüklerini durumunu değiştirme](./media/service-bus-diagnostic-logs/image3.png)

5.  İstediğiniz arşiv hedef ayarlanmış; Örneğin, bir depolama hesabı, bir olay hub'ı veya Azure günlük analizi.

6.  Yeni tanılama ayarları kaydedin.

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra günlükler yapılandırılmış arşivleme hedef görünür **tanılama günlükleri** dikey.

Tanılama yapılandırma hakkında daha fazla bilgi için bkz: [Azure tanılama günlükleri'ne genel bakış](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Tanılama günlüklerini şeması

Tüm günlükler JavaScript nesne gösterimi (JSON) biçiminde depolanır. Her giriş aşağıdaki bölümde açıklanan biçimini kullanan alanlarına sahiptir.

## <a name="operational-logs-schema"></a>İşlem günlükleri şeması

Her oturum açtığında **OperationalLogs** kategori yakalama ne Service Bus işlemleri sırasında olur. Özellikle, bu günlükler sıra oluşturma, kullanılan kaynakları ve işlem durumu da dahil olmak üzere işlem türü yakalayın.

İşlem günlüğü JSON dizeler aşağıdaki tabloda listelenen öğeleri şunları içerir:

Ad | Açıklama
------- | -------
Etkinlik Kimliği | İzleme için kullanılan iç kimliği
EventName | İşlem adı           
resourceId | Azure Resource Manager kaynak kimliği
SubscriptionId | Abonelik Kimliği
EventTimeString | İşlem süresi
EventProperties | İşlem özellikleri
Durum | İşlem durumu
Çağıran | Arayan işlemi (Azure portalı veya yönetim istemcisi)
category | OperationalLogs

Bir işlem günlüğü JSON dize örneği şöyledir:

```json
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Sonraki adımlar

Service Bus hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu giriş](service-bus-messaging-overview.md)
* [Service Bus ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
