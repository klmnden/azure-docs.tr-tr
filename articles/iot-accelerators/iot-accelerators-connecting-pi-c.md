---
title: Raspberry Pi C - Azure'ı kullanarak Uzaktan izleme sağlama | Microsoft Docs
description: C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını bir Raspberry Pi cihazın bağlanması açıklar
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 3331db51f4d141cf142d1bd0578043ca6681f3cd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61454515"
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-solution-accelerator-c"></a>Raspberry Pi'yi Cihazınızı Uzaktan izleme çözüm hızlandırıcısına (C) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, gerçek bir cihaz Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir. Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi Raspberry Pi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Raspbian işletim sistemi çalıştıran Raspberry Pi üzerinde uygulama oluşturun.

Bir cihazın benzetimini gerçekleştirme isterseniz, bkz. [oluşturma ve test yeni bir simülasyon cihazı](iot-accelerators-remote-monitoring-create-simulated-device.md).

### <a name="required-hardware"></a>Gerekli donanım

Raspberry Pi komut satırında bilgisayarlarına uzaktan bağlanabilmelerini sağlamak için bir masaüstü bilgisayar.

[Microsoft IOT Starter Kit, Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) veya eşdeğer bileşenleri. Bu öğreticide setindeki aşağıdaki öğeleri kullanır:

- Raspberry Pi 3
- (İle NOOBS) MicroSD kartı
- Bir Mini USB kablosu
- Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Komut satırı Raspberry Pi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](https://www.putty.org/).
- Çoğu Linux dağıtımları ve MAC'te SSH komut satırı yardımcı programı içerir. Daha fazla bilgi için [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Raspberry Pi'yi yazılım gerekli

Bu makalede, en son sürümünü yüklediğinizden varsayılır [Raspberry Pi'yi Raspbian OS'de](https://www.raspberrypi.org/learning/software-guide/quickstart/).

Aşağıdaki adımları Raspberry Pi'yi çözüm hızlandırıcısına bağlanan bir C uygulaması oluşturmak için hazırlama işlemini gösterir:

1. Kullanarak, Raspberry Pi bağlanmak **ssh**. Daha fazla bilgi için [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) üzerinde [Raspberry Pi Web sitesi](https://www.raspberrypi.org/).

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Nasıl yapılır bu kılavuzdaki adımları tamamlamak için adımları izleyin [Linux geliştirme ortamınızı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#linux) gerekli geliştirme araçları ve kitaplıkları Raspberry Pi'yi eklemek için.

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
