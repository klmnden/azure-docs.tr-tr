---
title: Raspberry Pi'yi sanal telemetri ile Node.js kullanarak Azure IOT paketi bağlanın | Microsoft Docs
description: Raspberry Pi 3 için Microsoft Azure IOT başlangıç seti kullanın ve Azure IOT paketi. Raspberry Pi'yi Uzaktan izleme çözümüne bağlanmak için node.js'yi kullanma buluta sanal telemetri gönderme ve çözüm panosundan çağrılan yöntemlere yanıt.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: node.js
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 53297049fd36eae3839c6a8146afc336b8f5cd02
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38676496"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-simulated-telemetry-using-nodejs"></a>Raspberry Pi 3 ', Uzaktan izleme çözümüne bağlama ve Node.js kullanarak sanal telemetri gönderme

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğreticide Raspberry Pi 3 buluta gönderilecek sıcaklık ve nem verileri benzetimini yapmak için nasıl kullanılacağını gösterir. Öğretici kullanır:

- Raspbian işletim sistemi, Node.js programlama diline ve Node.js için Microsoft Azure IOT SDK'sı bir örnek cihaz uygulamak için.
- IOT paketi Uzaktan izleme çözümü, bulut tabanlı arka uç önceden yapılandırılmış.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-overview-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-overview-simulator.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü, Azure aboneliğinizdeki Azure Hizmetleri kümesi sağlar. Dağıtımın gerçek bir kurumsal mimari yansıtır. Gereksiz Azure tüketim ücreti ödememek için örneğiniz azureiotsuite.com önceden yapılandırılmış çözüm ile işiniz bittiğinde silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz. [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri Tanıtım amaçlı][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi-simulator.md)]

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Şimdi indirin ve Raspberry Pi'yi Uzaktan izleme istemci uygulaması yapılandırın.

### <a name="install-nodejs"></a>Node.js yükleme

Bunu henüz yapmadıysanız, Raspberry Pi üzerinde Node.js yükleyin. Node.js için IOT SDK'sı 0.11.5 Node.js veya sonraki sürümü gerektirir. Aşağıdaki adımlar, Raspberry Pi üzerinde Node.js v6.10.2 yükleme işlemini gösterir:

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Raspberry Pi'yi Node.js ikili dosyaları indirmek için aşağıdaki komutu kullanın:

    ```sh
    wget https://nodejs.org/dist/v6.10.2/node-v6.10.2-linux-armv7l.tar.gz
    ```

1. İkili dosyaları yüklemek için aşağıdaki komutu kullanın:

    ```sh
    sudo tar -C /usr/local --strip-components 1 -xzf node-v6.10.2-linux-armv7l.tar.gz
    ```

1. Node.js v6.10.2 başarıyla yüklediniz doğrulamak için aşağıdaki komutu kullanın:

    ```sh
    node --version
    ```

### <a name="clone-the-repositories"></a>Depoları kopyalayın

Zaten yapmadıysanız, Pi'yi üzerinde bir terminalde aşağıdaki komutları çalıştırarak gerekli depoları kopyalayın:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Cihaz bağlantı dizesini güncelleştirme

Örnek kaynak dosyasını açın **nano** aşağıdaki komutu kullanarak Düzenleyicisi:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Satırı bulun:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Cihaz ve IOT hub'ı bilgi oluşturduğunuz ve kaydettiğiniz Bu öğreticinin başında yer tutucu değerlerini değiştirin. Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnek için önkoşul paketleri yüklemek için aşağıdaki komutları çalıştırın:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator
npm install
```

Bu gibi durumlarda, örnek program artık Raspberry Pi üzerinde çalıştırabilirsiniz. Aşağıdaki komutu girin:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/simulator/remote_monitoring.js
```

Aşağıdaki örnek çıktıda, Raspberry Pi üzerinde komut satırında gördüğünüz çıkış örneğidir:

![Raspberry Pi uygulama çıktısı][img-raspberry-output]

Tuşuna **Ctrl-C** istediğiniz zaman programdan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-simulator/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
