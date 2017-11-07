---
title: "Azure IOT paketi bir Intel NUC kullanarak bir ağ geçidine bağlanmak | Microsoft Docs"
description: "Microsoft IOT ticari ağ geçidi Seti ve önceden yapılandırılmış Uzaktan izleme çözümü kullanın. Uzaktan izleme çözümüne bağlama, buluta sanal telemetriyi göndermek ve çözüm panodan çağrılan yöntemler yanıtlamak için Azure IOT sınır ağ geçidi kullanın."
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
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: f3b4a80b37b0b869847c90834731d4c23883a961
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2017
---
# <a name="connect-your-azure-iot-edge-gateway-to-the-remote-monitoring-preconfigured-solution-and-send-simulated-telemetry"></a>Önceden yapılandırılmış Uzaktan izleme çözümü, Azure IOT kenar ağ geçidine bağlanmak ve sanal telemetriyi Gönder

[!INCLUDE [iot-suite-v1-gateway-kit-selector](../../includes/iot-suite-v1-gateway-kit-selector.md)]

Bu öğretici Azure IOT kenar Uzaktan izleme önceden yapılandırılmış çözümü göndermek için sıcaklık ve nem veri benzetimini yapmak için nasıl kullanılacağını gösterir. Öğretici kullanır:

- Bir örnek ağ geçidi uygulamak için Azure IOT kenar.
- IOT paketi Uzaktan izleme çözümü bulut tabanlı arka uç önceden yapılandırılmış.

## <a name="overview"></a>Genel Bakış

Bu öğreticide, aşağıdaki adımları tamamlayın:

- Önceden yapılandırılmış Uzaktan izleme çözümü örneği Azure aboneliğinize dağıtın. Bu adım, otomatik olarak dağıtır ve birden çok Azure hizmetlerini yapılandırır.
- Bilgisayarınız ve Uzaktan izleme çözümü ile iletişim kurmak için Intel NUC ağ geçidi aygıtınızı ayarlayın.
- Çözüm Panosu üzerinde görüntüleyebilirsiniz sanal telemetriyi göndermek için IOT sınır ağ geçidi yapılandırın.

[!INCLUDE [iot-suite-v1-gateway-kit-prerequisites](../../includes/iot-suite-v1-gateway-kit-prerequisites.md)]

[!INCLUDE [iot-suite-v1-provision-remote-monitoring](../../includes/iot-suite-v1-provision-remote-monitoring.md)]

> [!WARNING]
> Uzaktan izleme çözümü Azure aboneliğinizde Azure Hizmetleri kümesi sağlar. Dağıtım gerçek Kurumsal Mimarisi yansıtır. Gereksiz Azure tüketim ücretleri önlemek için kendisiyle tamamladığınızda azureiotsuite.com önceden yapılandırılmış çözüm örneğiniz silin. Önceden yapılandırılmış çözümü yeniden gerekiyorsa, kolayca yeniden oluşturabilirsiniz. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config].

[!INCLUDE [iot-suite-v1-gateway-kit-view-solution](../../includes/iot-suite-v1-gateway-kit-view-solution.md)]

Bir cihaz kimliği gibi kullanarak ikinci bir cihaz eklemek için önceki adımları yineleyin **device02**. Örnek verileri Uzaktan izleme çözümü için ağ geçidinde iki sanal aygıtlardan gönderir.

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-connectivity](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-connectivity.md)]

[!INCLUDE [iot-suite-v1-gateway-kit-prepare-nuc-software](../../includes/iot-suite-v1-gateway-kit-prepare-nuc-software.md)]

## <a name="build-the-custom-iot-edge-module"></a>Özel IOT kenar modülü oluşturmak

Şimdi, Uzaktan izleme çözümüne iletileri göndermek ağ geçidi sağlayan özel IOT kenar modülü de oluşturabilirsiniz. Bir ağ geçidi ve IOT kenar modülleri yapılandırma hakkında daha fazla bilgi için bkz: [Azure IOT kenar kavramları][lnk-gateway-concepts].

Kaynak kodu özel IOT kenar modülleri için aşağıdaki komutları kullanarak Github'dan yükleyin:

```bash
cd ~
git clone https://github.com/Azure-Samples/iot-remote-monitoring-c-intel-nuc-gateway-getting-started.git
```

Aşağıdaki komutları kullanarak özel IOT kenar modülü oluşturun:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
chmod u+x build.sh
sed -i -e 's/\r$//' build.sh
./build.sh
```

Derleme betiğinin libsimulator.so özel IOT kenar modülü oluşturma klasöründe yerleştirir.

## <a name="configure-and-run-the-iot-edge-gateway"></a>Yapılandırma ve IOT sınır ağ geçidi çalıştırma

Uzaktan izleme panonuza sanal telemetriyi göndermek için IOT sınır ağ geçidi artık yapılandırabilirsiniz. Bir ağ geçidi ve IOT kenar modülleri yapılandırma hakkında daha fazla bilgi için bkz: [Azure IOT kenar kavramları][lnk-gateway-concepts].

> [!TIP]
> Bu öğreticide kullandığınız standart `vi` Intel NUC üzerindeki metin düzenleyici. Değil kullandıysanız `vi` önce bir giriş öğretici gibi tamamlanacaksa [UNIX - VI Düzenleyicisi Öğreticisi] [ lnk-vi-tutorial] bu düzenleyicisiyle tanımak için. Alternatif olarak, daha kullanıcı dostu yükleyebilirsiniz [nano](https://www.nano-editor.org/) komutunu kullanarak Düzenleyicisi `smart install nano -y`.

Örnek yapılandırma dosyasını açın **VI** Düzenleyicisi aşağıdaki komutu kullanarak:

```bash
vi ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator/remote_monitoring.json
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
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  },
  {
    "macAddress": "AA:BB:CC:DD:EE:FF",
    "deviceId": "<<Azure IoT Hub Device ID>>",
    "deviceKey": "<<Azure IoT Hub Device Key>>"
  }
]
```

Değiştir **DeviceID** ve **deviceKey** kimliklerini ve anahtarlarını Uzaktan izleme çözümünde daha önce oluşturduğunuz iki cihazlar için yer tutucularını.

Yaptığınız değişiklikleri kaydedin.

Aşağıdaki komutları kullanarak IOT sınır ağ geçidi artık çalıştırabilirsiniz:

```bash
cd ~/iot-remote-monitoring-c-intel-nuc-gateway-getting-started/simulator
/usr/share/azureiotgatewaysdk/samples/simulated_device_cloud_upload/simulated_device_cloud_upload remote_monitoring.json
```

Ağ geçidi üzerinde Intel NUC başlatır ve sanal telemetriyi Uzaktan izleme çözümüne gönderir:

![IOT sınır ağ geçidi sanal telemetriyi oluşturur][img-simulated telemetry]

Tuşuna **Ctrl-C** herhangi bir zamanda programı'ndan çıkmak için.

## <a name="view-the-telemetry"></a>Telemetriyi görüntüleyebilir

IOT sınır ağ geçidi artık uzaktan izleme çözümüne benzetimli telemetri gönderiyor. Çözüm panosunda telemetri görüntüleyebilirsiniz.

- Çözüm panosuna gidin.
- Bir ağ geçidi olarak yapılandırılmış iki cihaz seçin **cihaz görünümüne** açılır.
- Ağ geçidi aygıtlardan telemetri Panoda görüntüler.

![Sanal ağ geçidi aygıtlardan telemetri görüntüleme][img-telemetry-display]

> [!WARNING]
> Azure hesabınızda çalıştıran uzaktan izleme çözümü bırakırsanız çalıştırıldığında için faturalandırılır. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config]. Bunu kullanmayı bitirdikten sonra önceden yapılandırılmış çözümü Azure hesabınızdan silin.

## <a name="next-steps"></a>Sonraki adımlar

Ziyaret [Azure IOT Geliştirme Merkezi](https://azure.microsoft.com/develop/iot/) daha fazla örnekleri ve Azure IOT belgeler.

[img-simulated telemetry]: ./media/iot-suite-v1-gateway-kit-get-started-simulator/appoutput.png

[img-telemetry-display]: ./media/iot-suite-v1-gateway-kit-get-started-simulator/telemetry.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md

[lnk-vi-tutorial]: http://www.tutorialspoint.com/unix/unix-vi-editor.htm
[lnk-gateway-concepts]: https://docs.microsoft.com/azure/iot-hub/iot-hub-linux-iot-edge-get-started