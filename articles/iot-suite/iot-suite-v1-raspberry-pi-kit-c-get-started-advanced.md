---
title: Bellenim güncelleştirmeleri desteklemek için C kullanarak Azure IOT paketi için Raspberry Pi'yi bağlanma | Microsoft Docs
description: Microsoft Azure IOT Starter Kit Böğürtlenli pi 3 kullanın ve Azure IOT paketi. Uzaktan izleme çözümüne Raspberry Pi'yi bağlanmak için kullanım C telemetri algılayıcı buluta göndermek ve bir uzak bellenim güncelleştirme gerçekleştirin.
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
ms.openlocfilehash: 8160752b0116c3ef3e6b6ab7920bb35e471f180b
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
ms.locfileid: "24012067"
---
# <a name="connect-your-raspberry-pi-3-to-the-remote-monitoring-solution-and-enable-remote-firmware-updates-using-c"></a>Raspberry Pi 3 Uzaktan izleme çözümüne bağlama ve C kullanarak uzak Bellenim güncelleştirmeleri etkinleştir

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-selector](../../includes/iot-suite-v1-raspberry-pi-kit-selector.md)]

Bu öğretici Raspberry Pi 3 için Microsoft Azure IOT Starter Kit kullanmayı gösterir:

* Bulut ile iletişim kurabilen bir sıcaklık ve nem okuyucu geliştirin.
* Etkinleştirmek ve bir uzak bellenim güncelleştirme için güncelleştirme istemci uygulaması Raspberry Pi'yi üzerinde gerçekleştirin.

Öğretici kullanır:

* Raspbian işletim sistemi, C programlama dili ve C için Microsoft Azure IOT SDK'sı bir örnek aygıt uygulamak için.
* IOT paketi Uzaktan izleme çözümü bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayın:

* Önceden yapılandırılmış Uzaktan izleme çözümü örneği Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetlerini yapılandırır.
* Bilgisayarınız ve Uzaktan izleme çözümü ile iletişim kurmak için aygıt ve algılayıcılar ayarlayın.
* Uzaktan izleme çözümüne bağlama için örnek cihaz kod güncelleştirin ve çözüm panosunda görüntüleyebilirsiniz telemetri gönderebilir.
* İstemci uygulaması güncelleştirmek için örnek aygıt kodu kullanın.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prerequisites](../../includes/iot-suite-v1-raspberry-pi-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü Azure aboneliğinizde Azure Hizmetleri kümesi sağlar. Dağıtım gerçek Kurumsal Mimarisi yansıtır. Gereksiz Azure tüketim ücretleri önlemek için kendisiyle tamamladığınızda azureiotsuite.com önceden yapılandırılmış çözüm örneğiniz silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config].

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-solution](../../includes/iot-suite-v1-raspberry-pi-kit-view-solution.md)]

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-prepare-pi](../../includes/iot-suite-v1-raspberry-pi-kit-prepare-pi.md)]

## <a name="download-and-configure-the-sample"></a>İndirme ve örnek yapılandırma

Şimdi, indirin ve Uzaktan izleme istemci uygulaması, Raspberry Pi'yi yapılandırın.

### <a name="clone-the-repositories"></a>Depoları kopyalama

Henüz yapmadıysanız, gerekli depoları, Pi üzerinde aşağıdaki komutları çalıştırarak kopyalama:

```sh
cd ~
git clone --recursive https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit.git
```

### <a name="update-the-device-connection-string"></a>Güncelleştirme cihaz bağlantı dizesi

Örnek yapılandırma dosyasını açın **nano** Düzenleyicisi aşağıdaki komutu kullanarak:

```sh
nano ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/config/deviceinfo
```

Oluşturulan ve bu öğreticinin başlangıcında kaydedilen cihaz kimliği ve IOT Hub bilgilerini yer tutucu değerlerini değiştirin.

İşiniz bittiğinde, deviceınfo dosyasının içeriğini aşağıdaki gibi görünmelidir:

```conf
yourdeviceid
HostName=youriothubname.azure-devices.net;DeviceId=yourdeviceid;SharedAccessKey=yourdevicekey
```

Değişikliklerinizi kaydetmek (**Ctrl-O**, **Enter**) ve düzenleyiciden çıkın (**Ctrl-X**).

## <a name="build-the-sample"></a>Örnek oluşturma

Zaten yapmadıysanız, önkoşul bir terminal Raspberry Pi'yi üzerinde aşağıdaki komutları çalıştırarak C için Microsoft Azure IOT cihaz SDK yükleyin:

```sh
sudo apt-get update
sudo apt-get install g++ make cmake git libcurl4-openssl-dev libssl-dev uuid-dev
```

Örnek çözümü Raspberry Pi'yi şimdi oluşturabilirsiniz:

```sh
chmod +x ~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
~/iot-remote-monitoring-c-raspberrypi-getstartedkit/advanced/1.0/build.sh
```

Örnek program Raspberry Pi'yi şimdi çalıştırabilirsiniz. Aşağıdaki komutu girin:

  ```sh
  sudo ~/cmake/remote_monitoring/remote_monitoring
  ```

Aşağıdaki örnek çıkış Raspberry Pi'yi komut satırına bakın çıkış örneğidir:

![Böğürtlenli Pi uygulamadan çıktı][img-raspberry-output]

Tuşuna **Ctrl-C** herhangi bir zamanda programı'ndan çıkmak için.

[!INCLUDE [iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced](../../includes/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced.md)]

1. Çözüm panosunda tıklatın **aygıtları** ziyaret etmek için **aygıtları** sayfası. Böğürtlenli Pi seçin **cihaz listesi**. Ardından **yöntemleri**:

    ![Panoda aygıtları listele][img-list-devices]

1. Üzerinde **yöntemi çağırma** sayfasında, **InitiateFirmwareUpdate** içinde **yöntemi** açılır.

1. İçinde **FWPackageURI** alanına, **https://github.com/Azure-Samples/iot-remote-monitoring-c-raspberrypi-getstartedkit/raw/master/advanced/2.0/package/remote_monitoring.zip**. Bu Arşiv 2.0 sürümünde bellenimin uygulamasını içerir.

1. Seçin **InvokeMethod**. Uygulamasını Raspberry Pi'yi çözüm panosuna geri bildirim gönderir. Ardından, bellenim güncelleştirme işlemi'nin yeni bellenim sürümünü indirerek başlar:

    ![Yöntem geçmişini göster][img-method-history]

## <a name="observe-the-firmware-update-process"></a>İşlem güncelleştirme bellenim inceleyin

Cihazda ve çözüm panosunda bildirilen özelliklerini görüntüleyerek çalışan işlem güncelleştirme bellenim görebilirsiniz:

1. Üzerinde Raspberry Pi'yi güncelleştirme işleminin devam görüntüleyebilirsiniz:

    ![Güncelleştirme ilerlemesini Göster][img-update-progress]

    > [!NOTE]
    > Güncelleştirme tamamlandığında Uzaktan izleme uygulama sessizce yeniden başlatır. Komutunu `ps -ef` çalıştığından doğrulanamadı. İşlemi sonlandırmak istiyorsanız kullanın `kill` işlem kimlikli komutu.

1. Çözüm Portalı'nda bir aygıt tarafından belirlendiği şekilde, bellenim güncelleştirme durumunu görüntüleyebilirsiniz. Aşağıdaki ekran görüntüsü durum ve süresini, güncelleştirme işlemini yeni üretici yazılımı sürümüne ve her bir aşamaya gösterir:

    ![İş durumunu göster][img-job-status]

    Panosuna geri gidin, cihaz yine bellenim güncelleştirme aşağıdaki telemetri gönderiyor doğrulayabilirsiniz.

> [!WARNING]
> Azure hesabınızda çalıştıran uzaktan izleme çözümü bırakırsanız çalıştırıldığında için faturalandırılır. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config]. Bunu kullanmayı bitirdikten sonra önceden yapılandırılmış çözümü Azure hesabınızdan silin.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirme Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.


[img-raspberry-output]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/app-output.png
[img-update-progress]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/updateprogress.png
[img-job-status]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/jobstatus.png
[img-list-devices]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/listdevices.png
[img-method-history]: ./media/iot-suite-v1-raspberry-pi-kit-c-get-started-advanced/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md