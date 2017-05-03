---
title: "Azure Zaman Serisi Görüşleri ortamınıza olay kaynağı ekleme | Microsoft Docs"
description: "Bu öğreticide, Zaman Serisi Görüşleri ortamınıza bir olay kaynağı bağlayacaksınız"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: 9dc394c0da34a8ee2796c6114e7f2f647c5345a1
ms.lasthandoff: 04/25/2017

---

# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamınız için olay kaynağı oluşturma

Zaman Serisi Görüşleri Olay Kaynağı, Azure Event Hubs gibi bir olay aracısından türetilir. Zaman Serisi Görüşleri doğrudan Olay Kaynaklarına bağlanarak, kullanıcının tek bir kod satırı bile yazmasına gerek kalmadan veri akışını alır. Şu anda, Zaman Serisi Görüşleri Azure Event Hubs ve Azure IoT Hubs desteği sağlamaktadır ve gelecekte daha fazla Olay Kaynağı eklenecektir.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Ortamınıza olay kaynağı ekleme adımları

1.    [Azure Portal](https://portal.azure.com) oturum açın.
2.    Azure Portal'ın sol tarafındaki menüde "Tüm kaynaklar"a tıklayın.
3.    Zaman Serisi Görüşleri ortamınızı seçin.

  ![Zaman Serisi Görüşleri oluşturma olay kaynağı](media/add-event-source/getstarted-create-eventsource1.png)

4.    “Olay Kaynakları” öğesini seçin ve “+ Ekle” düğmesine tıklayın.

  ![Zaman Serisi Görüşleri oluşturma olay kaynağı - ayrıntılar](media/add-event-source/getstarted-create-eventsource2.png)

5.    Olay kaynağının adını belirtin. Bu ad, bu olay kaynağından gelen tüm olaylarla ilişkilendirilmiştir ve sorgu zamanında kullanılabilir.
6.    Geçerli aboneliğin Olay Hub’ı kaynakları listesinden bir olay hub’ı seçin veya başka bir abonelikte yer alan bir olay kaynağını belirtmek için "Olay Hub’ı ayarlarını elle gir” İçeri Aktarma seçeneğini belirtin. Olayların JSON biçiminde yayımlanması gerekir.
7.    Olay hub’ında okuma izni olan ilkeyi seçin.
8.    Olay hub’ı tüketici grubunu seçin.

  > [!IMPORTANT]
  > Bu tüketici grubunun başka hiçbir hizmet (örneğin, Stream Analytics işi veya başka bir Zaman Serisi Görüşleri ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka hizmetler tarafından kullanılıyorsa, bu ortam ve diğer hizmetler için okuma işlemi olumsuz etkilenir. Tüketici grubu olarak “$Default” kullanıyorsanız, bu diğer okuyucular tarafından olası yeniden kullanıma yol açabilir.

9.    “Oluştur” düğmesine tıklayın.

Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* Olay kaynağına [olayları gönderme](time-series-insights-send-events.md)
* [Zaman Serisi Görüşleri Portalı](https://insights.timeseries.azure.com)’nda ortamınızı görüntüleme

