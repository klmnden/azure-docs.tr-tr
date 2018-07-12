---
title: Raspberry Pi'yi üretici yazılımı güncelleştirmeleri destekleyecek şekilde Node.js kullanarak Azure IOT Paketi'ne bağlanın | Microsoft Docs
description: Raspberry Pi 3 için Microsoft Azure IOT başlangıç seti kullanın ve Azure IOT paketi. Raspberry Pi'yi Uzaktan izleme çözümüne bağlanmak için node.js'yi kullanma buluta sensörden telemetri gönderin ve bir uzak üretici yazılımı güncelleştirmesi gerçekleştirmek.
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
ms.openlocfilehash: 31bbeff8049c6005671b991f965fae7316e3adf6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309600"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-nodejs"></a>Raspberry Pi 3 ', Uzaktan izleme çözümüne bağlama ve Node.js kullanarak uzak üretici yazılımı güncelleştirmelerini etkinleştir

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğreticide, Raspberry Pi 3 için Microsoft Azure IOT başlangıç Seti kullanma işlemini gösterir:

* Bulut ile iletişim kurabilen bir sıcaklık ve nem okuyucu geliştirin.
* Etkinleştirme ve uzak bir üretici yazılımı güncelleştirme için güncelleştirme istemci uygulaması Raspberry Pi üzerinde gerçekleştirin.

Öğretici kullanır:

- Raspbian işletim sistemi, Node.js programlama diline ve Node.js için Microsoft Azure IOT SDK'sı bir örnek cihaz uygulamak için.
- IOT paketi Uzaktan izleme çözümü, bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayacaksınız:

- Önceden yapılandırılmış Uzaktan izleme çözümünün bir örneği, Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetini yapılandırır.
- Bilgisayarınızı uzaktan izleme çözümü ile iletişim kurmak için cihaz ve sensörlerden ayarlayın.
- Uzaktan izleme çözümüne bağlanmak için örnek cihaz kodu güncelleştirme ve çözüm panosunda görüntüleyebilirsiniz telemetri gönderin.
- İstemci uygulamayı güncelleştirmek için cihaz örnek kodu kullanın.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü, Azure aboneliğinizdeki Azure Hizmetleri kümesi sağlar. Dağıtımın gerçek bir kurumsal mimari yansıtır. Gereksiz Azure tüketim ücreti ödememek için örneğiniz azureiotsuite.com önceden yapılandırılmış çözüm ile işiniz bittiğinde silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz. [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri Tanıtım amaçlı][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

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

Henüz yapmadıysanız, Pi'yi üzerinde aşağıdaki komutları çalıştırarak gerekli depoları kopyalayın:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Cihaz bağlantı dizesini güncelleştirme

Örnek yapılandırma dosyasını açın **nano** aşağıdaki komutu kullanarak Düzenleyicisi:

```sh
nano ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Cihaz kimliği ve IOT hub'ı bilgileriyle oluşturduğunuz ve kaydettiğiniz Bu öğreticinin başında yer tutucu değerlerini değiştirin.

İşiniz bittiğinde deviceınfo dosyasının içeriğini aşağıdaki örnekteki gibi görünmelidir:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="run-the-sample"></a>Örneği çalıştırma

Örnek için önkoşul paketleri yüklemek için aşağıdaki komutları çalıştırın:

```sh
cd ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advance/1.0
npm install
```

Bu gibi durumlarda, örnek program artık Raspberry Pi üzerinde çalıştırabilirsiniz. Aşağıdaki komutu girin:

```sh
sudo node ~/iot-remote-monitoring-node-raspberrypi-getstartedkit/advanced/1.0/remote_monitoring.js
```

Aşağıdaki örnek çıktıda, Raspberry Pi üzerinde komut satırında gördüğünüz çıkış örneğidir:

![Raspberry Pi uygulama çıktısı][img-raspberry-output]

Tuşuna **Ctrl-C** istediğiniz zaman programdan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Çözüm panosunda tıklayın **cihazları** ziyaret etmek için **cihazları** sayfası. Raspberry Pi'yi seçin **cihaz listesi**. Ardından **yöntemleri**:

    ![Pano listesi cihazları][img-list-devices]

1. Üzerinde **yöntemi Çağır** sayfasında **Initiatefirmwareupdate** içinde **yöntemi** açılır.

1. İçinde **Fwpackageurı** alanına **https://raw.githubusercontent.com/Azure-Samples/iot-remote-monitoring-node-raspberrypi-getstartedkit/master/advanced/2.0/raspberry.js**. Bu dosya, üretici yazılımı sürümü 2.0 uygulamasını içerir.

1. Seçin **InvokeMethod**. Raspberry Pi üzerinde uygulama, çözüm panosuna geri bildirim gönderir. Ardından, üretici yazılımı güncelleştirme işlemi'nin yeni bellenim sürümünü indirerek başlatır:

    ![Yöntem Geçmişi Göster][img-method-history]

## <a name="observe-the-firmware-update-process"></a>Üretici yazılımı güncelleştirme işlemi gözlemleyin

Cihazda ve çözüm panosunda bildirilen özellikleri görüntüleyerek çalışan işlemi güncelleştirme bellenim görebilirsiniz:

1. Raspberry Pi üzerinde güncelleştirme işleminin ilerleme durumunu görüntüleyebilirsiniz:

    ![Güncelleştirme ilerleme durumunu göster][img-update-progress]

    > [!NOTE]
    > Uzaktan izleme uygulama sessizce güncelleştirme tamamlandığında yeniden başlatır. Komutunu `ps -ef` çalıştırdığı doğrulayın. İşlemi sonlandırmak istiyorsanız kullanın `kill` işlem kimliğine komutu.

1. Çözüm portalında cihaz tarafından bildirilen üretici yazılımı güncelleştirme durumunu görüntüleyebilirsiniz. Aşağıdaki ekran görüntüsünde, durum ve güncelleştirme işlemi ve yeni bellenim sürümünü her aşaması süresince gösterir:

    ![İş durumunu göster][img-job-status]

    Panosuna geri gidin, cihaz yine de şu üretici yazılımı güncelleştirme telemetri gönderdiği doğrulayabilirsiniz.

> [!WARNING]
> Azure hesabınızda çalışan Uzaktan izleme çözümü değiştirmeden bırakırsanız, çalıştığı süre için faturalandırılırsınız. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz. [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri Tanıtım amaçlı][lnk-demo-config]. Kullanmayı bitirdikten sonra önceden yapılandırılmış çözümü Azure hesabınızdan silin.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirici Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.


[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-v1-raspberry-pi-kit-node-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
