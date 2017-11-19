---
title: "Azure IOT Hub ile ölçeklendirme | Microsoft Docs"
description: "Beklenen ileti işleme desteklemek için IOT hub'ını ölçeklendirmek nasıl. Her katman için desteklenen işleme ve parçalama seçeneklerini özetini içerir."
services: iot-hub
documentationcenter: 
author: fsautomata
manager: timlt
editor: 
ms.assetid: e7bd4968-db46-46cf-865d-9c944f683832
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00043293eb57768f0117e912bb67f02d088934f3
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="scale-your-iot-hub-solution"></a>IOT hub çözümünüzü ölçeklendirme
Azure IOT Hub, en çok bir milyon eş zamanlı cihazı destekleyebilir. Daha fazla bilgi için bkz: [IOT Hub'ın fiyatlandırma][lnk-pricing]. Her IOT hub'ı birimi belirli sayıda günlük iletileri izin verir.

Çözümünüzü düzgün ölçeklendirmek için belirli IOT hub'ı kullanımınızı göz önünde bulundurun. Özellikle, gerekli en yüksek verimlilik işlemlerinin Aşağıdaki kategorilerde göz önünde bulundurun:

* Cihazdan buluta iletiler
* Bulut-cihaz iletilerini
* Kimlik kayıt defteri işlemleri

Bu işleme ek bilgi [IOT hub'ı kotaları ve kısıtlamaları] [ IoT Hub quotas and throttles] ve çözümünüzün uygun şekilde tasarlayın.

## <a name="device-to-cloud-and-cloud-to-device-message-throughput"></a>Cihaz Bulut ve bulut-cihaz ileti işleme
IOT hub'ı çözümünü boyutu için en iyi birim başına temelinde trafiği değerlendirmek için yoludur.

Cihaz bulut iletilerini bu aralıksız üretilen yönergeleri izleyin.

| Katman | Aralıksız üretilen | Sürdürülen gönderme oranı |
| --- | --- | --- |
| S1 |Birim başına 1111 KB/dakika kadar<br/>(1,5 GB/gün/birim) |278 iletileri dakikada birim başına ortalama<br/>(400.000 iletileri/gün birim başına) |
| S2 |Birim başına 16 MB/dakika kadar<br/>(22.8 GB/gün/birim) |4,167 iletileri dakikada birim başına ortalama<br/>(6 milyon iletileri/gün birim başına) |
| S3 |Birim başına 814 MB/dakika kadar<br/>(1144.4 GB/gün/birim) |208,333 iletileri dakikada birim başına ortalama<br/>(300 milyon iletileri/gün birim başına) |

## <a name="identity-registry-operation-throughput"></a>Kimlik kayıt defteri işlemi işleme
Cihaz sağlamak için çoğunlukla ilişkili oldukları gibi IOT Hub kimlik kayıt defteri işlemlerini çalıştırma işlemleri olması gereken değil.

Belirli veri bloğu performans numaraları için bkz: [IOT hub'ı kotaları ve kısıtlamaları][IoT Hub quotas and throttles].

## <a name="sharding"></a>Parçalama
Bazen tek bir IOT hub, milyonlarca cihaza için ölçeklendirebilirsiniz olmakla birlikte, çözümünüzü tek bir IOT hub garanti edemez özel performans özellikleri gerektirir. Bu durumda, birden çok IOT hub'ları aygıtlarınızı bölüm önerilir. Birden çok IOT hub'ları trafiği WINS'e kesintisiz ve gerekli işlem hızları ve gerekli verimlilik elde edin.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [IOT Hub Geliştirici Kılavuzu][lnk-devguide]
* [AI ile Azure IOT kenar sınır cihazları için dağıtma][lnk-iotedge]

[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub
[IoT Hub quotas and throttles]: iot-hub-devguide-quotas-throttling.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
