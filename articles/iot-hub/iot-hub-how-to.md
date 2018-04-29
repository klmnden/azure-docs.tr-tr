---
title: Azure IOT hub'ı nasıl | Microsoft Docs
description: Bir geliştirici olarak, çeşitli IOT Hub özellikleri nasıl kullanırım?
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: dobett
ms.openlocfilehash: 9b112d2d7fc1756b74e98335831175f5d4c13320
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="how-to-use-azure-iot-hub"></a>Azure IOT hub'ı kullanma

IOT Hub hizmeti için geliştirmeyi öğrenin için çeşitli seçenekleriniz vardır:

* IOT hub'ı özellikleri ayrıntılı açıklamaktadır kavramsal makaleleri okuyun.
* IOT Hub'ın çeşitli özellikleri kapsayan öğreticileri birini izleyin.

## <a name="developer-guide"></a>Geliştirici kılavuzu

Bir geliştirici olarak, IOT Hub hakkında ayrıntılı kavramsal kılavuz okuyabilirsiniz [Geliştirici Kılavuzu][lnk-devguide]. Bu kılavuz içerir:

* Bunları nasıl kullanacağınızı öğrenin yardımcı olmak üzere tüm IOT hub'ı özellikleri ayrıntılı açıklamaları.
* Birden çok seçenek kullanılabilir olduğunda seçme hakkında yönergeler.

## <a name="tutorials"></a>Öğreticiler

Uygulama alıştırmalarını aracılığıyla çalışarak belirli IOT hub'ı özellikleri hakkında bilgi edinmek tercih ederseniz, içinden seçim yapabileceğiniz çeşitli eğitim vardır. Bu öğreticileri birçoğu, birden fazla programlama dillerinde kullanılabilir. Bu öğreticiler şunları içerir:

- [IOT Hub cihaz-bulut iletileri yolları kullanma][lnk-routes-tutorial]. Bu öğretici bir kolay, yapılandırma tabanlı şekilde cihaz bulut iletilerini göndermek için IOT hub'ı yönlendirme kuralları kullanmayı gösterir.

- [IOT Hub ile bulut-cihaz iletilerini göndermek][lnk-c2d-tutorial]. Bu öğretici, IOT hub'ı aracılığıyla bulut-cihaz iletilerini göndermek ve bulut-cihaz iletilerini bir cihazda gösterilmektedir.

- [IOT Hub ile bulut aygıtlardan dosyaları karşıya yükleme][lnk-upload-tutorial]. Bu öğretici, IOT hub'ı dosya karşıya yükleme özelliklerini kullanmayı gösterir.

- [Cihaz çiftlerini ile çalışmaya başlama][lnk-twin-tutorial]. Bu öğretici cihaz çiftlerini, bildirilen özellikleri, istenen özellikleri ve etiketler için tanıtır. Cihaz çiftlerini cihazlarınızı değerleri eşitlemek için kullanın.

- [Doğrudan yöntemleri kullanın][lnk-methods-tutorial]. Bu öğretici, doğrudan yöntemlerinin nasıl kullanılacağını gösterir. Sanal cihazınız doğrudan bir yöntem için bir işleyici ekleyin ve IOT hub'ı doğrudan yöntemini çağırma.

- [Aygıt Management'i kullanmaya başlama][lnk-dm-tutorial]. Bu öğretici çiftlerini ve doğrudan yöntemleri gibi anahtar cihaz yönetimi özelliklerinin nasıl kullanılacağını gösterir. Sanal cihazınız Uzaktan yeniden başlatmak için bu özellikleri kullanın.

- [Cihazları yapılandırmak için istenen özellikleri kullanmak][lnk-properties-tutorial]. Bu öğretici nasıl cihazı kullanmak için twin'ın istenen ve özellikleri için uzaktan bildirilen Cihazınızı yapılandırmak gösterir.

- [Cihaz üretici yazılımını güncelleştirmek için cihaz Yönetimi'ni kullanın][lnk-jobs-tutorial]. Bu öğretici çiftlerini ve doğrudan yöntemleri gibi anahtar cihaz yönetimi özelliklerinin nasıl kullanılacağını gösterir. Uzaktan aygıtınızın üretici yazılımını güncelleştirmek için bu özellikleri kullanmayı öğrenin.

- [Zamanlama ve yayın işleri][lnk-schedule-tutorial]. Bu öğretici, istenen özellikleri ve doğrudan yöntemleri zamanlanmış bir saatte birden çok aygıt ile etkileşim için nasıl kullanılacağını gösterir.

## <a name="next-steps"></a>Sonraki adımlar

IOT Hub hizmeti hakkında daha fazla bilgi için bkz: [Geliştirici Kılavuzu][lnk-devguide].

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-routes-tutorial]: ./iot-hub-csharp-csharp-process-d2c.md
[lnk-c2d-tutorial]: ./iot-hub-csharp-csharp-c2d.md
[lnk-upload-tutorial]: ./iot-hub-csharp-csharp-file-upload.md
[lnk-twin-tutorial]: ./iot-hub-node-node-twin-getstarted.md
[lnk-methods-tutorial]: ./iot-hub-node-node-direct-methods.md
[lnk-dm-tutorial]: ./iot-hub-node-node-device-management-get-started.md
[lnk-properties-tutorial]: ./iot-hub-node-node-twin-how-to-configure.md
[lnk-jobs-tutorial]: ./iot-hub-node-node-firmware-update.md
[lnk-schedule-tutorial]: ./iot-hub-node-node-schedule-jobs.md