---
title: "Nesnelerin İnterneti (IoT Paketi) için Azure çözümleri | Microsoft Docs"
description: "Azure&quot;da IoT&quot;ye, örnek bir çözüm mimarisini ve bunun Azure IoT Paketi ve önceden yapılandırılmış çözümlerle nasıl ilişkili olduğunu içeren genel bir bakış."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 437d2655-896f-4a9e-a4a8-b864790d3ef8
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: b0c27ca561567ff002bbb864846b7a3ea95d7fa3
ms.openlocfilehash: 1c8703d64ff45e589a7ce93f1f2176681e1bf331
ms.contentlocale: tr-tr
ms.lasthandoff: 04/25/2017


---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="azure-iot-suite"></a>Azure IoT Paketi
Microsoft Azure IoT Paketi, kapsamlı önceden yapılandırılmış çözümler sayesinde hızlı başlangıç yapmanızı sağlayan kurumsal düzeyde bir çözümdür. Bu çözümler [uzaktan izleme][lnk-preconfigured-solutions], [tahmine dayalı bakım][lnk-predictive-maintenance] ve [bağlı fabrika][lnk-connected-factory] gibi yaygın IoT senaryolarını hedeflemektedir. Bu çözümler, bu makalede ana hatlarıyla açıklanan IOT çözüm mimarisinin uygulamalarıdır.

Bu önceden yapılandırılmış çözümler, şunları içeren eksiksiz, çalışan ve uçtan uca çözümlerdir:

- Başlamanıza yardımcı olmak için sanal cihazlar.
- [Azure IoT Hub][Azure IoT Hub], [Azure Event Hubs][Azure Event Hubs], [Azure Stream Analytics][Azure Stream Analytics], [Azure Machine Learning][Azure Machine Learning] ve [Azure depolama][Azure storage] gibi önceden yapılandırılmış Azure hizmetleri.
- Çözüme özel yönetim konsolları.

Önceden yapılandırılmış çözümlerde, özelleştirebildiğiniz ve kendi özel IoT senaryolarınıza uygulamak amacıyla uzanabildiğiniz kanıtlanmış, üretime hazır kod bulunur.

Çok sayıda önceden yapılandırılmış çözümün kullandığı [Azure IoT Hub][Azure IoT Hub] hizmeti de ilginizi çekebilir. [Azure IoT Hub][Azure IoT Hub], önceden yapılandırılmış çözüm mimarisinde kullanılan, cihazlar ve bulut arasındaki güvenli ve çift yönlü iletişimi sağlar.

## <a name="next-steps"></a>Sonraki adımlar
IoT Paketi ve önceden yapılandırılmış çözümler hakkında bilgi almaya devam etmek için aşağıdaki kaynakları keşfedin:

* [Azure IoT Paketi nedir?][lnk-whatissuite]
* [Azure IoT Paketi önceden yapılandırılmış çözümleri nelerdir?][lnk-whatarepreconfigured]

[lnk-whatissuite]: iot-suite-overview.md
[lnk-whatarepreconfigured]: iot-suite-what-are-preconfigured-solutions.md

[lnk-preconfigured-solutions]: iot-suite-getstarted-preconfigured-solutions.md
[Azure IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[Azure Event Hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Azure Stream Analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[Azure Machine Learning]: https://azure.microsoft.com/documentation/services/machine-learning/
[Azure storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-connected-factory]: iot-suite-connected-factory-overview.md
