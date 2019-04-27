---
title: Azure Service Bus tanılama günlükleri | Microsoft Docs
description: Azure'da hizmet veri yolu için tanılama günlüklerini ayarlama konusunda bilgi edinin.
keywords: ''
documentationcenter: .net
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 7d4cb8e55c5d1561c09cf85122550a66e3671f17
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714156"
---
# <a name="service-bus-diagnostic-logs"></a>Service Bus tanılama günlükleri

Azure Service Bus için iki tür günlüğü görüntüleyebilirsiniz:
* **[Etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md)**. Bu günlükler bir proje üzerinde gerçekleştirilen işlemler hakkında bilgi içerir. Günlükler her zaman etkindir.
* **[Tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md)**. Bir işin içinde gerçekleşen daha zengin bilgi hakkında her şey için tanılama günlüklerini yapılandırabilirsiniz. Tanılama günlüklerine yönelik güncelleştirmeleri ve iş çalışırken gerçekleşen etkinlikler dahil olmak üzere iş silinene kadar iş oluşturulur zamanından kapak etkinlikler.

## <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini açın

Tanılama günlükleri, varsayılan olarak devre dışıdır. Tanılama günlüklerini etkinleştirmek için aşağıdaki adımları gerçekleştirin:

1.  İçinde [Azure portalında](https://portal.azure.com)altında **izleme + Yönetim**, tıklayın **tanılama günlükleri**.

    ![Tanılama günlükleri dikey penceresinde gezinme](./media/service-bus-diagnostic-logs/image1.png)

2. İzlemek istediğiniz kaynağa tıklayın.  

3.  **Tanılamayı aç**’a tıklayın.

    ![Tanılama günlüklerini açın](./media/service-bus-diagnostic-logs/image2.png)

4.  İçin **durumu**, tıklayın **üzerinde**.

    ![Tanılama günlükleri durumunu değiştirin](./media/service-bus-diagnostic-logs/image3.png)

5.  İstediğiniz arşiv hedef ayarlayın; Örneğin, bir depolama hesabı, bir olay hub'ı veya Azure İzleyici günlüğe kaydeder.

6.  Yeni tanılama ayarları kaydedin.

Yeni ayarları yaklaşık 10 dakika içinde etkinleşir. Bundan sonra günlüklerini arşivleme yapılandırılmış hedefte görünür **tanılama günlükleri** dikey penceresi.

Tanılama yapılandırma hakkında daha fazla bilgi için bkz. [Azure tanılama günlükleri'ne genel bakış](../azure-monitor/platform/diagnostic-logs-overview.md).

## <a name="diagnostic-logs-schema"></a>Tanılama günlükleri şeması

Tüm günlükler, JavaScript nesne gösterimi (JSON) biçiminde depolanır. Her girişin aşağıdaki bölümde açıklanan biçimde kullanan dize alanları içerir.

## <a name="operational-logs-schema"></a>İşlem günlüklerinde şeması

' De oturum açması **OperationalLogs** kategori yakalama Service Bus işlemleri sırasında ne olur. Özellikle, bu günlükleri sıra oluşturma, kullanılan kaynakları ve işlemin durumu gibi bir işlem türü yakalayın.

İşlem günlüğü JSON dizelerini aşağıdaki tabloda listelenen öğeleri içerir:

Ad | Açıklama
------- | -------
Etkinlik Kimliği | İzleme için kullanılan iç kimliği
EventName | İşlem adı           
resourceId | Azure Resource Manager kaynak kimliği
SubscriptionId | Abonelik Kimliği
EventTimeString | İşlem süresi
EventProperties | İşlem özellikleri
Durum | İşlem durumu
Çağıran | Çağıran işlemin (Azure portalı ya da yönetim istemcisi)
category | OperationalLogs

Aşağıda, bir işlem günlüğü JSON dizesi örneği verilmiştir:

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

Hizmet veri yolu hakkında daha fazla bilgi için aşağıdaki bağlantılara bakın:

* [Service Bus'a giriş](service-bus-messaging-overview.md)
* [Service Bus ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
