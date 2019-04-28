---
title: Uzaktan izleme c - Azure Linux cihaz sağlamayı | Microsoft Docs
description: Linux üzerinde çalışan C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını için bir cihaz bağlamak açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: dobett
ms.openlocfilehash: 91d4eda566c8b534daa10c62637db28ccb01bbb6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61454511"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-linux"></a>Cihazınızı Uzaktan izleme çözüm Hızlandırıcısını için (Linux) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihaz Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir.

Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Ubuntu (Linux) çalıştıran bir makinede uygulama oluşturun.

Bir cihazın benzetimini gerçekleştirme isterseniz, bkz. [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için Ubuntu sürümü 15.04 veya üzerini çalıştıran bir cihaz gerekir. Devam etmeden önce [Linux geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux).

## <a name="view-the-code"></a>Kod Görünümü

[Örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) kullanılan Kılavuzu Azure IOT C SDK'ları GitHub deposunda kullanılabilir.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirebilir ve proje hazırlama

Proje hazırlamak için Kopyala veya indir [Azure IOT C SDK'ları depo](https://github.com/Azure/azure-iot-sdk-c) github'dan.

Örnek bulunan **çözümleri/samples/remote_monitoring_client** klasör.

Açık **remote_monitoring.c** dosyası **çözümleri/samples/remote_monitoring_client** bir metin düzenleyicisinde klasör.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımları nasıl kullanılacağını açıklayan *CMake* istemci uygulaması oluşturmak için. Uzaktan izleme istemci uygulaması oluşturma işleminin bir parçası için SDK'sı yerleşik olarak bulunur.

1. Düzen **remote_monitoring.c** değiştirmek için dosya `<connectionstring>` bu nasıl yapılır kılavuzunda başlangıcında not ettiğiniz çözüm hızlandırıcısına bir cihazı eklendiğinde cihaz bağlantı dizesiyle.

1. Kopyaladığınız kök dizinine gidin [Azure IOT C SDK'ları depo](https://github.com/Azure/azure-iot-sdk-c) deposu ve istemci uygulaması oluşturmak için aşağıdaki komutları çalıştırın:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. İstemci uygulamasını çalıştırın ve IOT Hub'ına telemetri gönderme:

    ```sh
    ./samples/solutions/remote_monitoring_client/remote_monitoring_client
    ```

    Konsol iletileri olarak gösterir:

    - Uygulama, çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
