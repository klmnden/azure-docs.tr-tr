---
title: Azure IOT paketi ile gerçek algılayıcılar C kullanarak Raspberry Pi'yi bağlanın | Microsoft Docs
description: Raspberry Pi 3 için Microsoft Azure IOT başlangıç seti kullanın ve Azure IOT paketi. Raspberry Pi'yi Uzaktan izleme çözümüne bağlanmak için kullanmak C buluta sensörden telemetri gönderme ve çözüm panosundan çağrılan yöntemlere yanıt.
services: ''
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 635eb9d4e85eaf43dc83f2bd5a0d0f7c32620d98
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611316"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-send-telemetry-from-a-real-sensor-using-c"></a>C kullanarak gerçek algılayıcıdan telemetri gönderme ve Raspberry Pi 3 ', Uzaktan izleme çözümüne bağlama

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğreticide Raspberry Pi 3 için Microsoft Azure IOT başlangıç Seti bulutla iletişim kurabilen bir sıcaklık ve nem okuyucu geliştirmek için nasıl kullanılacağını gösterir. Öğretici kullanır:

- Raspbian işletim sistemi, C programlama dilini ve C için Microsoft Azure IOT SDK'sı bir örnek cihaz uygulamak için.
- IOT paketi Uzaktan izleme çözümü, bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

- Önceden yapılandırılmış Uzaktan izleme çözümünün bir örneği, Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetini yapılandırır.
- Bilgisayarınızı uzaktan izleme çözümü ile iletişim kurmak için cihaz ve sensörlerden ayarlayın.
- Uzaktan izleme çözümüne bağlanmak için örnek cihaz kodu güncelleştirme ve çözüm panosunda görüntüleyebilirsiniz telemetri gönderin.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü, Azure aboneliğinizdeki Azure Hizmetleri kümesi sağlar. Dağıtımın gerçek bir kurumsal mimari yansıtır. Gereksiz Azure tüketim ücreti ödememek için örneğiniz azureiotsuite.com önceden yapılandırılmış çözüm ile işiniz bittiğinde silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz. [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri Tanıtım amaçlı][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Şimdi indirin ve Raspberry Pi'yi Uzaktan izleme istemci uygulaması yapılandırın.

### <a name="clone-the-repositories"></a>Depoları kopyalayın

Zaten yapmadıysanız, Pi'yi üzerinde bir terminalde aşağıdaki komutları çalıştırarak gerekli depoları kopyalayın:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
git clone --recursive https://github.com/WiringPi/WiringPi.git
```

### <a name="update-the-device-connection-string"></a>Cihaz bağlantı dizesini güncelleştirme

Örnek kaynak dosyasını açın **nano** aşağıdaki komutu kullanarak Düzenleyicisi:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/remote_monitoring/remote_monitoring.c
```

Aşağıdaki satırları bulun:

```c
static const char* deviceId = "[Device Id]";
static const char* connectionString = "HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]";
```

Cihaz ve IOT hub'ı bilgi oluşturduğunuz ve kaydettiğiniz Bu öğreticinin başında yer tutucu değerlerini değiştirin. Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="build-the-sample"></a>Örneği oluşturmak

Önkoşul paketleri, Microsoft Azure IOT cihaz SDK'sı için C Raspberry Pi üzerinde bir terminalde aşağıdaki komutları çalıştırarak yükleyin:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Bu gibi durumlarda, güncelleştirilmiş örnek çözümde şimdi Raspberry Pi üzerinde oluşturabilirsiniz:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/basic/build.sh
```

Bu gibi durumlarda, örnek program artık Raspberry Pi üzerinde çalıştırabilirsiniz. Aşağıdaki komutu girin:

```sh
sudo ~/cmake/remote_monitoring/remote_monitoring
```

Aşağıdaki örnek çıktıda, Raspberry Pi üzerinde komut satırında gördüğünüz çıkış örneğidir:

![Raspberry Pi uygulama çıktısı][img-raspberry-output]

Tuşuna **Ctrl-C** istediğiniz zaman programdan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry.md)]

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-basic/appoutput.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
