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
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 123ecca28f0d970851487827d0d18e244ce6d98e
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-azure-portal"></a>Azure Portal’ı kullanarak Zaman Serisi Görüşleri ortamınız için olay kaynağı oluşturma

Zaman Serisi Görüşleri Olay Kaynağı, Azure Event Hubs gibi bir olay aracısından türetilir. Zaman Serisi Görüşleri doğrudan Olay Kaynaklarına bağlanarak, kullanıcının tek bir kod satırı bile yazmasına gerek kalmadan veri akışını alır. Şu anda Zaman Serisi Görüşleri, Azure Event Hubs'ı ve Azure IoT Hub'larını destekler. İleride daha fazla Olay Kaynağı eklenecektir.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Ortamınıza olay kaynağı ekleme adımları

1.  [Azure Portal](https://portal.azure.com) oturum açın.
2.  Azure Portal'ın sol tarafındaki menüde "Tüm kaynaklar"a tıklayın.
3.  Zaman Serisi Görüşleri ortamınızı seçin.

  ![Zaman Serisi Görüşleri oluşturma olay kaynağı](media/add-event-source/getstarted-create-event-source-1.png)

4.  “Olay Kaynakları” öğesini seçin ve “+ Ekle” düğmesine tıklayın.

  ![Zaman Serisi Görüşleri oluşturma olay kaynağı - ayrıntılar](media/add-event-source/getstarted-create-event-source-2.png)

5.  Olay kaynağının adını belirtin. Bu ad, bu olay kaynağından gelen tüm olaylarla ilişkilendirilmiştir ve sorgu zamanında kullanılabilir.
6.  Geçerli abonelikte yer alan Olay Hub'ı kaynakları listesinden bir olay hub'ı seçin. Alternatif olarak, başka bir abonelikte yer alan bir olay hub'ı belirtmek için "Olay hub'ı ayarlarını elle gir" içeri aktarma seçeneğini de belirleyebilirsiniz. Olayların JSON biçiminde yayımlanması gerekir.
7.  Olay hub’ında okuma izni olan ilkeyi seçin.
8.  Olay hub’ı tüketici grubunu seçin.

  > [!IMPORTANT]
  > Bu tüketici grubunun başka hiçbir hizmet (örneğin, Stream Analytics işi veya başka bir Zaman Serisi Görüşleri ortamı) tarafından kullanılmadığından emin olun. Tüketici grubu başka hizmetler tarafından kullanılıyorsa, bu ortam ve diğer hizmetler için okuma işlemi olumsuz etkilenir. Tüketici grubu olarak “$Default” kullanıyorsanız, bu diğer okuyucular tarafından olası yeniden kullanıma yol açabilir.

9.  “Oluştur” düğmesine tıklayın.

Olay kaynağının oluşturulmasından sonra, Zaman Serisi Görüşleri ortamınıza veri akışını otomatik olarak başlatır.

## <a name="next-steps"></a>Sonraki adımlar

* Olay kaynağına [olayları gönderme](time-series-insights-send-events.md)
* [Zaman Serisi Görüşleri Portalı](https://insights.timeseries.azure.com)’nda ortamınızı görüntüleme
