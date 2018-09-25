---
title: include dosyası
description: IOT edge
author: kgremban
manager: timlt
ms.reviewer: veyalla
ms.service: iot-edge
services: iot-edge
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: kgremban
ms.openlocfilehash: a8160e677fa99d8cb691db39d7f29ba6eddbd261
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47004688"
---
## <a name="enabling-extended-offline-operation-preview"></a>Genişletilmiş çevrimdışı işlemi (Önizleme) etkinleştirme
İle başlayarak [v1.0.2 yayın](https://aka.ms/edge102) Edge çalışma zamanını, kendisine bağlanmak için aşağı akış cihazlarının ve Edge cihazı için genişletilmiş çevrimdışı işlemi yapılandırılabilir. 

Bu özellik sayesinde yerel modülleri veya aşağı akış cihazları Edge cihazı ile gerektiği gibi kimliğinizi yeniden doğrulayın ve iletileri ve IOT Hub'ından değilken bile yöntemleri kullanarak birbirlerinin ile iletişim. Bkz. Bu [blog gönderisi](https://aka.ms/iot-edge-offline) ve [kavramı makale](../articles/iot-edge/offline-capabilities.md) daha fazla ayrıntı ve bu özellik kapsamı için.

Çevrimdışı bir ağ geçidi senaryosunda genişletilmiş etkinleştirmek için edge cihazı ve buna bağlanmak aşağı akış cihazlar arasında bir üst-alt ilişkisi oluşturun.

1. IOT hub'ı portalda Edge cihaz ayrıntıları dikey penceresinden tıklayın **alt cihazları yönetme (Önizleme)** üst komut çubuğunda düğme.

1. Tıklayın **+ Ekle** düğmesi.

1. Cihazlar listesinde, alt cihazları seçin ve alt öğeleri olarak eklemek için olanları seçmek için sağ ok tuşunu kullanın.

1. Tıklayın **Tamam** onaylamak için.

1. İçinde **modülleri ayarlama** Edge cihaz ayrıntıları ekranında **Gelişmiş Edge çalışma zamanı ayarları Yapılandır**, altında **Edge hub'ı** ortam değişkenlerini bir girdi ekleyin **UpstreamProtocol** değerle **MQTT**. Aynı ortam değişkeni ekleyin ve değerini **Edge Aracısı** de. 

1. Tıklayın **Kaydet** ve mutlaka **Gönder** tıkladıktan sonra değişiklikleri **sonraki** iki kez.

Sınır cihazı ve onun alt cihazlar artık genişletilmiş çevrimdışı işlemi için etkinleştirilir.  