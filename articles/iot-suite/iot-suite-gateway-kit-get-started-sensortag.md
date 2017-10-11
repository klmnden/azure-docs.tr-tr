---
title: "Azure IOT paketi bir Intel NUC kullanarak bir ağ geçidine bağlanmak | Microsoft Docs"
description: "Microsoft IOT ticari ağ geçidi Seti ve önceden yapılandırılmış Uzaktan izleme çözümü kullanın. Azure IOT sınır ağ geçidi SensorTag aygıtını Uzaktan izleme çözümüne bağlama, buluta telemetri göndermesine ve çözüm panodan çağrılan yöntemler yanıtlamak üzere etkinleştirmek için kullanın."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-suite
ms.devlang: c
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.openlocfilehash: bda16be1094276fcecef1e708f9d7db307d94a89
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-telemetry-from-a-sensortag"></a>Önceden yapılandırılmış Uzaktan izleme çözümü, Azure IOT kenar ağ geçidine bağlanmak ve telemetri SensorTag gönderin

[!INCLUDE [iot-suite-gateway-kit-selector](../../includes/iot-suite-gateway-kit-selector.md)]

Bu öğretici, Azure IOT kenar Uzaktan izleme önceden yapılandırılmış çözümü SensorTag aygıttan sıcaklık ve nem veri göndermek için nasıl kullanılacağını gösterir. SensorTag Intel NUC ağ geçidine Bluetooth kullanarak bağlanır. Öğretici kullanır:

- Bir örnek ağ geçidi uygulamak için Azure IOT kenar.
- IOT paketi Uzaktan izleme çözümü bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayın:

- Önceden yapılandırılmış Uzaktan izleme çözümü örneği Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetlerini yapılandırır.
- Bilgisayarınız ve Uzaktan izleme çözümü ile iletişim kurmak için Intel NUC ağ geçidi aygıtınızı ayarlayın.
- Intel NUC ağ geçidiniz SensorTag aygıttan telemetri almasına ve Uzaktan izleme Panosu göndermek için ayarlayın.

[!INCLUDE [iot-suite-gateway-kit-prerequisites](../../includes/iot-suite-gateway-kit-prerequisites.md)]

[Texas Instruments bırak SensorTag][lnk-sensortag]. Bu öğretici, SensorTag Aygıttan Bluetooth üzerinden telemetri verilerini alır.

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü Azure aboneliğinizde Azure Hizmetleri kümesi sağlar. Dağıtım gerçek Kurumsal Mimarisi yansıtır. Gereksiz Azure tüketim ücretleri önlemek için kendisiyle tamamladığınızda azureiotsuite.com önceden yapılandırılmış çözüm örneğiniz silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config].

[!INCLUDE [iot-suite-gateway-kit-view-solution](../../includes/iot-suite-gateway-kit-view-solution.md)]

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-gateway-kit-prepare-nuc-connectivity.md)]

## <a name="configure-bluetooth-connectivity"></a>Bluetooth bağlantısını yapılandır

Bluetooth SensorTag aygıtını bağlanmak ve telemetri göndermek üzere etkinleştirmek için Intel NUC üzerinde yapılandırın.

### <a name="find-the-mac-address-of-the-sensortag"></a>SensorTag MAC adresini Bul

1. Intel NUC üzerinde kabuğunda Bluetooth hizmeti engellemesini kaldırmak için aşağıdaki komutu çalıştırın:

    ```bash
    sudo rfkill unblock bluetooth
    ```

1. Intel NUC Bluetooth hizmetini başlatın ve Bluetooth Kabuk girmek için aşağıdaki komutları çalıştırın:

    ```bash
    sudo systemctl start bluetooth
    bluetoothctl
    ```

1. Aşağıdaki komutu güç Bluetooth denetleyicisinde çalıştırın:

    ```bash
    power on
    ```

    Denetleyici açık olduğunda, bir ileti görür **güç değiştirme başarılı**.

1. Yakındaki Bluetooth aygıtlarını taramak için aşağıdaki komutu çalıştırın:

    ```bash
    scan on
    ```

1. Bulunabilir duruma getirmek için SensorTag güç düğmesine basın. Yeşil ışığı yanıp sönen.

1. Denetleyici SensorTag buldu Kabuğu'nda bir ileti gördüğünüzde, aygıtın MAC adresini not edin. MAC adresi benzer **A0:E6:F8:B5:F6:00**. Ağ geçidi yapılandırdığınızda öğreticide daha sonra MAC adresi gerekir.

1. Bluetooth tarama devre dışı bırakmak için aşağıdaki komutu çalıştırın:

    ```bash
    scan off
    ```

1. SensorTag cihaza bağlanabildiğini doğrulamak için aşağıdaki komutu çalıştırın:

    ```bash
    connect <SensorTag MAC address>
    ```

    Başarıyla bağlanıyorsanız, kabuk iletisi gösterir **bağlantı başarılı** ve SensorTag aygıt hakkındaki bilgileri yazdırır. Bağlanamıyorsanız, onay SensorTag hala açık.

1. Şimdi, SensorTag bağlantısını kesmek ve aşağıdaki komutları çalıştırarak Bluetooth kabuktan çıkış:

    ```bash
    disconnect
    exit
    ```

[!INCLUDE [iot-suite-gateway-kit-prepare-nuc-software](../../includes/iot-suite-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Özel IOT kenar modülü oluşturmak

Şimdi, Uzaktan izleme çözümüne iletileri göndermek ağ geçidi sağlayan özel IOT kenar modülü de oluşturabilirsiniz. Bir ağ geçidi ve IOT kenar modülleri yapılandırma hakkında daha fazla bilgi için bkz: [Azure IOT kenar kavramları][lnk-gateway-concepts].

Kaynak kodu özel IOT kenar modülleri için aşağıdaki komutları kullanarak Github'dan yükleyin:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Aşağıdaki komutları kullanarak özel IOT kenar modülü oluşturun:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Derleme betiğinin libsensor2remotemonitoring.so özel IOT kenar modülü oluşturma klasöründe yerleştirir.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Yapılandırma ve IOT sınır ağ geçidi çalıştırma

Telemetri, Uzaktan izleme Panosu SensorTag cihazınızdan göndermek için IOT sınır ağ geçidi artık yapılandırabilirsiniz. Bir ağ geçidi ve IOT kenar modülleri yapılandırma hakkında daha fazla bilgi için bkz: [Azure IOT kenar kavramları][lnk-gateway-concepts].

> [!TIP]
> Bu öğreticide kullandığınız standart `vi` Intel NUC üzerindeki metin düzenleyici. Değil kullandıysanız `vi` önce bir giriş öğretici gibi tamamlanacaksa [UNIX - VI Düzenleyicisi Öğreticisi] [ lnk-vi-tutorial] bu düzenleyicisiyle tanımak için. Alternatif olarak, daha kullanıcı dostu yükleyebilirsiniz [nano](https://www.nano-editor.org/) komutunu kullanarak Düzenleyicisi `smart install nano -y`.

Örnek yapılandırma dosyasını açın **VI** Düzenleyicisi aşağıdaki komutu kullanarak:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic/remote_monitoring.json
```

Iothub modülü yapılandırmasında aşağıdaki satırları bulun:

```json
"args": {
  "IoTHubName": "<<Azure IoT Hub Name>>",
  "IoTHubSuffix": "<<Azure IoT Hub Suffix>>",
  "Transport": "http"
}
```

Oluşturulan ve bu öğreticinin başlangıcında kaydedilen IOT hub'ı bilgilerle yer tutucu değerlerini değiştirin. IoTHubName değeri benzer **yourrmsolution37e08**, ve IoTSuffix değeri genellikle **azure devices.net**.

Eşleşme modülü yapılandırmasında aşağıdaki satırları bulun:

```json
args": [
  {
    "macAddress": "<<AA:BB:CC:DD:EE:FF>>",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Değiştir **macAddress** yer tutucu daha önce not ettiğiniz, SensorTag MAC adresine sahip. Değiştir **DeviceID** ve **deviceKey** kimliklerini ve anahtarlarını Uzaktan izleme çözümünde daha önce oluşturduğunuz iki cihazlar için yer tutucularını.

SensorTag modülü yapılandırmasında aşağıdaki satırları bulun:

```json
"args": {
  "controller_index": 0,
  "device_mac_address": "<<AA:BB:CC:DD:EE:FF>>",
  ...
}
```

Değiştir **aygıt\_mac\_adresi** yer tutucu daha önce not ettiğiniz, SensorTag MAC adresine sahip.

Yaptığınız değişiklikleri kaydedin.

Aşağıdaki komutları kullanarak ağ geçidi artık çalıştırabilirsiniz:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/basic
/usr/share/azureiotgatewaysdk/samples/ble_gateway/ble_gateway remote_monitoring.json
```

IOT sınır ağ geçidi üzerinde Intel NUC başlar ve telemetri SensorTag Uzaktan izleme çözümüne gönderir:

![IOT sınır ağ geçidi SensorTag telemetri gönderir][img-telemetry]

Tuşuna **Ctrl-C** herhangi bir zamanda programı'ndan çıkmak için.

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleyebilir

Ağ geçidi artık telemetri SensorTag aygıttan Uzaktan izleme çözümüne gönderiyor. Çözüm panosunda telemetri görüntüleyebilirsiniz. Ayrıca komutları ağ geçidi üzerinden SensorTag aygıtınıza çözüm panosunda da gönderebilirsiniz.

- Çözüm panosuna gidin.
- Yapılandırdığınız SensorTag temsil eden ağ geçidi cihazı seçin **cihaz görünümüne** açılır.
- SensorTag cihaz telemetrisinden Panoda görüntüler.

![Telemetri SensorTag cihazlarından görüntüleme][img-telemetry-display]

> [!WARNING]
> Azure hesabınızda çalıştıran uzaktan izleme çözümü bırakırsanız çalıştırıldığında için faturalandırılır. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config]. Bunu kullanmayı bitirdikten sonra önceden yapılandırılmış çözümü Azure hesabınızdan silin.


## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirme Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-telemetry]: ./media/iot-suite-gateway-kit-get-started-sensortag/appoutput.png


[img-telemetry-display]: ./media/iot-suite-gateway-kit-get-started-sensortag/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md
[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-sensortag]: http://www.ti.com/ww/en/wireless_connectivity/sensortag/
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started