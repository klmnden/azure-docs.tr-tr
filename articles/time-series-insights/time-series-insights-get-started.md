---
title: "Azure Zaman Serisi Görüşleri ortamı oluşturma | Microsoft Docs"
description: "Bu öğreticide, Zaman Serisi Ortamı oluşturmayı, bunu bir olay kaynağına bağlamayı ve dakikalar içinde olay verilerinizin analizine hazır olmayı öğreneceksiniz."
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
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.lasthandoff: 04/25/2017

---

# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Azure Portal’da yeni Zaman Serisi Görüşleri ortamı oluşturma

Zaman Serisi Görüşleri ortamı, giriş ve depolama kapasitesi olan bir Azure kaynağıdır. Müşteriler, Azure Portal aracılığıyla ortamları gereken kapasiteyle hazırlar.

## <a name="steps-to-create-the-environment"></a>Ortam oluşturma adımları

Ortamınızı oluşturmak için şu adımları izleyin:

1.    [Azure Portal](https://portal.azure.com) oturum açın.
2.    Sol üst köşedeki artı işaretine (“+”) tıklayın.
3.    Arama kutusunda “Zaman Serisi Görüşleri” için arama yapın.

  ![Zaman Serisi Görüşleri oluşturma ortam](media/get-started/getstarted-create-environment1.png)

4.    “Zaman Serisi Görüşleri” öğesini seçin, “Oluştur” düğmesine tıklayın.

  ![Zaman Serisi Görüşleri oluşturma kaynak grubu](media/get-started/getstarted-create-environment2.png)

5.    Ortam adını belirtin. Bu ad, [zaman serisi gezgininde](https://insights.timeseries.azure.com) ortamı temsil edecektir.
6.    Bir abonelik seçin. Olay kaynağınızı içeren aboneliği seçin. Zaman Serisi Görüşleri aynı abonelikteki mevcut Azure IoT Hub ve Event Hub kaynaklarını otomatik olarak algılayabilir.
7.    Kaynak grubunu seçin veya oluşturun. Kaynak grubu, birlikte kullanılan Azure kaynakları koleksiyonudur.
8.    Barındırma konumunu seçin. Verileri veri merkezleri arasında taşımaktan kaçınmak için, olay kaynağınızı içeren konumu seçin.
9.    Fiyatlandırma katmanını seçin.
10.    Kapasite seçin. Oluşturduktan sonra ortam kapasitesini değiştirebilirsiniz.
11.    Ortamınızı oluşturun. Ayrıca, her oturum açtığınızda kolayca erişmek için ortamınızı panoya sabitleyebilirsiniz.

  ![Zaman Serisi Görüşleri oluşturma panoya sabitleme](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Zaman Serisi Görüşleri Portalı](time-series-insights-data-access.md)’nda ortamınıza erişmek için [veri erişim ilkelerini tanımlama](https://insights.timeseries.azure.com)
* [Olay kaynağı oluşturma](time-series-insights-add-event-source.md)
* Olay kaynağına [olayları gönderme](time-series-insights-send-events.md)

