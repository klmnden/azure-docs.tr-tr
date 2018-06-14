---
title: Azure IOT paketi ile gerçek algılayıcılar Node.js kullanarak Raspberry Pi'yi bağlanma | Microsoft Docs
description: Microsoft Azure IOT Starter Kit Böğürtlenli pi 3 kullanın ve Azure IOT paketi. Uzaktan izleme çözümüne Raspberry Pi'yi bağlanmak için kullanım Node.js telemetri algılayıcı buluta göndermek ve çözüm panodan çağrılan yöntemler yanıt verir.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: nodejs
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 9bb4f62b1a3de68ce8796b60fe76cd97b9ab9ade
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
ms.locfileid: "24011969"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-nodejs"></a>Raspberry Pi 3 Uzaktan izleme çözümüne bağlama ve Node.js kullanarak gerçek algılayıcı telemetri Gönder

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğretici, Microsoft Azure IOT Starter Kit Raspberry Pi 3 Bulutu ile iletişim kurabilen bir sıcaklık ve nem okuyucu geliştirmek için nasıl kullanılacağını gösterir. Öğretici kullanır:

- Raspbian işletim sistemi, Node.js programlama dili ve Node.js için Microsoft Azure IOT SDK'sı bir örnek aygıt uygulamak için.
- IOT paketi Uzaktan izleme çözümü bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayın:

- Önceden yapılandırılmış Uzaktan izleme çözümü örneği Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetlerini yapılandırır.
- Bilgisayarınız ve Uzaktan izleme çözümü ile iletişim kurmak için aygıt ve algılayıcılar ayarlayın.
- Uzaktan izleme çözümüne bağlama için örnek cihaz kod güncelleştirin ve çözüm panosunda görüntüleyebilirsiniz telemetri gönderebilir.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü Azure aboneliğinizde Azure Hizmetleri kümesi sağlar. Dağıtım gerçek Kurumsal Mimarisi yansıtır. Gereksiz Azure tüketim ücretleri önlemek için kendisiyle tamamladığınızda azureiotsuite.com önceden yapılandırılmış çözüm örneğiniz silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Şimdi, indirin ve Uzaktan izleme istemci uygulaması, Raspberry Pi'yi yapılandırın.

### <a name="install-nodejs"></a>Node.js yükleme

Node.js, Böğürtlenli Pi üzerinde yükleyin. Node.js için IOT SDK'sı 0.11.5 Node.js veya sonraki sürümünü gerektirir. Aşağıdaki adımlar, Raspberry Pi'yi Node.js v6.10.2 yükleme gösterir:

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Raspberry Pi'yi Node.js ikilileri indirmek için aşağıdaki komutu kullanın:

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

### <a name="clone-the-repositories"></a>Depoları kopyalama

Zaten yapmadıysanız, gerekli depoları, Pi üzerinde aşağıdaki komutları çalıştırarak kopyalama:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git`
```

### <a name="update-the-device-connection-string"></a>Güncelleştirme cihaz bağlantı dizesi

Örnek kaynak dosyasında açma **nano** Düzenleyicisi aşağıdaki komutu kullanarak:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

Satırı bulun:

```javascript
var connectionString = 'HostName=[Your IoT hub name].azure-devices.net;DeviceId=[Your device id];SharedAccessKey=[Your device key]';
```

Aygıt ve oluşturulan ve bu öğreticinin başlangıcında kaydedilen IOT hub'ı bilgi yer tutucu değerlerini değiştirin. Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="run-the-sample"></a>Örnek çalıştırın

Örnek önkoşul paketleri yüklemek için aşağıdaki komutları çalıştırın:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic
npm install
```

Örnek program Raspberry Pi'yi şimdi çalıştırabilirsiniz. Aşağıdaki komutu girin:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/basic/remote_monitoring.js
```

Aşağıdaki örnek çıkış Raspberry Pi'yi komut satırına bakın çıkış örneğidir:

![Böğürtlenli Pi uygulamadan çıktı][img-raspberry-output]

Tuşuna **Ctrl-C** herhangi bir zamanda programı'ndan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirme Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-basic/app-output.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
