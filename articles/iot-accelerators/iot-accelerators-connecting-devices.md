---
title: Windows cihazları uzaktan C - Azure izleme sağlama | Microsoft Docs
description: Windows üzerinde çalışan C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını için bir cihaz bağlamak açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 2a8a0bf1e63f06bbe6b6a073af6b3da8904dcaeb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61450230"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Cihazınızı Uzaktan izleme çözüm Hızlandırıcısını için (Windows) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihaz Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir.

Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Windows çalıştıran bir makinede cihazı istemci uygulaması oluşturun.

Bir cihazın benzetimini gerçekleştirme isterseniz, bkz. [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

## <a name="prerequisites"></a>Önkoşullar

Nasıl yapılır bu kılavuzdaki adımları tamamlamak için adımları izleyin [Windows geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) gerekli geliştirme araçları ve kitaplıkları Windows makinenizi eklemek için.

## <a name="view-the-code"></a>Kod Görünümü

[Örnek kod](https://github.com/Azure/azure-iot-sdk-c/tree/master/samples/solutions/remote_monitoring_client) kullanılan Kılavuzu Azure IOT C SDK'ları GitHub deposunda kullanılabilir.

### <a name="download-the-source-code-and-prepare-the-project"></a>Kaynak kodunu indirebilir ve proje hazırlama

Proje hazırlamak için [Azure IOT C SDK'ları depoyu kopyalama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#set-up-a-windows-development-environment) github'dan.

Örnek bulunan **çözümleri/samples/remote_monitoring_client** klasör.

Açık **remote_monitoring.c** dosyası **çözümleri/samples/remote_monitoring_client** bir metin düzenleyicisinde klasör.

[!INCLUDE [iot-accelerators-connecting-code](../../includes/iot-accelerators-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

1. Düzen **remote_monitoring.c** değiştirmek için dosya `<connectionstring>` bu nasıl yapılır kılavuzunda başlangıcında not ettiğiniz çözüm hızlandırıcısına bir cihazı eklendiğinde cihaz bağlantı dizesiyle.

1. Bağlantısındaki [Windows C SDK'yı derlemek](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#build-the-c-sdk-in-windows) SDK'sı ve Uzaktan izleme istemci uygulaması oluşturun.

1. Çözümü derleyin kullandığınız komut isteminde aşağıdakini çalıştırın:

    ```cmd
    samples\solutions\remote_monitoring_client\Release\remote_monitoring_client.exe
    ```

    Konsol iletileri olarak gösterir:

    - Uygulama, çözüm hızlandırıcısına örnek telemetri gönderir.
    - Çözüm panosundan çağrılan yöntemlere verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
