---
title: "Azure IOT kenar genel bakış | Microsoft Docs"
description: "Azure IOT kenar ağ geçitleri, modüller ve aracıları gibi anahtar mimari kavramlarını açıklar."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/04/2017
ms.author: dobett
ms.openlocfilehash: 11f6375c319c8945b0278003ee08215715f7ac42
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-iot-edge-architectural-concepts"></a>Azure IOT kenar mimari kavramları

Tüm örnek kodu incelemeden veya IOT kenar kullanarak kendi alan ağ geçidi oluşturmadan önce IOT kenar mimarisini destekleyen temel kavramları anlamanız gerekir.

## <a name="iot-edge-modules"></a>IOT kenar modülleri

Bir ağ geçidi ile Azure IOT sınır oluşturma ve Birleştirme yapı *IOT kenar modülleri*. Modüller birbirileriyle veri alışverişinde bulunmak için *iletileri* kullanır. Modül bir ileti alır, üzerinde bir işlem yapar, isteğe bağlı olarak onu yeni bir iletiye dönüştürür ve ardından diğer modüllerin işlemesi için yayımlar. Bazı modüller yalnızca yeni iletiler oluşturabilir ve hiçbir zaman gelen iletileri işlemez. Bir modül zinciri, her bir modülün bir noktadaki veriler üzerinde dönüştürme gerçekleştirdiği bir veri işleme işlem hattı oluşturur.

![Ağ geçidinde Azure IoT Edge ile oluşturulan modül zinciri][1]

IOT kenar aşağıdaki bileşenleri içerir:

* Ortak ağ geçidi işlevlerini yerine önceden yazılmış modüller.
* Bir geliştiricinin özel modüller yazmak için kullanabileceği arabirimler.
* Bir modül kümesini dağıtmak ve çalıştırmak için gereken altyapı.

SDK çeşitli işletim sistemleri ve platformlar üzerinde çalışacak ağ geçitleri oluşturmanıza olanak sağlayan bir soyutlama katmanı sağlar.

![Azure IoT Edge özet düzeyi][2]

## <a name="messages"></a>İletiler

Birbirlerine ileti gönderen modüllerin düşünülmesi bir ağ geçidinin nasıl çalıştığını kavramsallaştırmanın kolay bir yolu olsa da neler olduğunu doğru şekilde yansıtmaz. IOT kenar modüller birbirleri ile iletişim kurmak için bir aracı kullanın. Modüller (veri yolu veya Yayınla/Abone ol gibi Mesajlaşma modelleri kullanarak) aracısı iletileri yayımlamak ve iletiyi kendisine bağlı modülleri yönlendirmek Aracısı olanak tanır.

Bir modül, aracıya ileti yayımlamak için **Broker_Publish** işlevini kullanır. Aracı bir geri çağırma işlevini çağırarak iletileri bir modüle teslim eder. İleti bir dizi anahtar/değer özelliklerinden ve bir bellek bloğu olarak geçirilen içeriklerden oluşur.

![Azure IoT Edge’de Aracı’nın rolü][3]

## <a name="message-routing-and-filtering"></a>İleti yönlendirme ve filtreleme

Doğru IOT kenar modülleri iletilerini yönlendirmek için iki yolu vardır:

* Kaynak ve havuz her modül için Aracısı bilmesi için bağlantılar kümesi Aracısı geçirebilirsiniz.
* Bir modül, ileti özelliklerini filtreleyebilirsiniz.

İleti bu amaca yönelikse modül yalnızca iletiye göre davranmalıdır. Bağlantılar ve etkili bir şekilde filtreleme ileti ileti işlem hattını oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Çalıştırabileceğiniz bir örnek uygulanan Bu kavramlar görmek için bkz: [keşfedin Azure IOT kenar mimarisi][lnk-hello-world].

<!-- Images -->
[1]: media/iot-hub-iot-edge-overview/modules.png
[2]: media/iot-hub-iot-edge-overview/modules_2.png
[3]: media/iot-hub-iot-edge-overview/messages_1.png

<!-- Links -->
[lnk-hello-world]: ./iot-hub-linux-iot-edge-get-started.md
