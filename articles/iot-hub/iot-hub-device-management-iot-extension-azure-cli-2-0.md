---
title: Azure IOT cihaz yönetimini IOT uzantısı ile Azure CLI 2.0 | Microsoft Docs
description: Azure IOT Hub cihaz yönetimi, doğrudan yöntemleri ve Twin'ın istenen özellikleri yönetim seçenekleri için Azure CLI 2.0 aracı IOT uzantısını kullanır.
author: chrissie926
manager: ''
keywords: Azure IOT cihaz yönetimi, azure IOT hub cihaz yönetimi, cihaz yönetim IOT, IOT hub cihaz Yönetimi
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: dc96e70a031d6080217e71b829ec5de3c64e4cf7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632824"
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>IOT uzantısı Azure CLI 2.0 için Azure IOT Hub cihaz yönetimi için kullanın.

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure CLI 2.0 IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) yeni bir açık kaynak eklediğinden IOT uzantısı [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Azure CLI 2.0 Azure Kaynak Yöneticisi'ni ve yönetim uç ile etkileşim için komutlar içerir. Örneğin, bir Azure VM veya bir IOT hub'ı oluşturmak için Azure CLI 2.0 kullanabilirsiniz. CLI uzantısını ek hizmete özgü özellikleri erişim Azure CLI vermiş büyütmek bir Azure hizmetini etkinleştirir. IOT uzantısı IOT geliştiriciler, tüm IOT Hub, IOT kenarı ve IOT Hub cihaz hizmeti sağlama olanağı komut satırı erişim sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Doğrudan yöntemler             | Başlatma veya ileti gönderme veya cihaz yeniden başlatıldığı durdurma gibi davranacak bir aygıtı olun.                                        |
| İstenen twin özellikleri    | Bir aygıt için yeşil bir Işığı ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| Özellikler Twin bildirdi   | Bir aygıt bildirilen durumunu alın. Örneğin, aygıt LED şimdi yanıp sönen bildirir.                                    |
| Twin etiketleri                  | Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir satış makinenin dağıtım konumu.                         |
| Cihaz çifti sorguları        | Kullanılabilir aygıtları tanımlama gibi rasgele koşullarla almak için tüm cihaz çiftlerini sorgulamak. |

Daha ayrıntılı açıklama farklara ve bu seçenekleri kullanma konusunda yönergeler için bkz: [cihaz bulut iletişimi Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut-cihaz iletişimi Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT Hub cihaz çifti ona bağlanan her aygıt için devam ettirir. Cihaz çiftlerini hakkında daha fazla bilgi için bkz: [cihaz çiftlerini ile çalışmaya başlama](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleri ile Azure CLI 2.0 için IOT uzantısını kullanarak öğrenin.

## <a name="what-you-do"></a>Neler

Azure CLI 2.0 ve IOT uzantısı Azure CLI 2.0 için çeşitli yönetim seçenekleri ile çalıştırın.

## <a name="what-you-need"></a>Ne gerekiyor

- Öğretici [Cihazınızı](iot-hub-raspberry-pi-kit-node-get-started.md) , aşağıdaki gereksinimleri ele alınmaktadır tamamlandı:
  - Etkin bir Azure aboneliği.
  - Azure IOT hub'ı aboneliğinizdeki.
  - Azure IOT hub'ına iletileri gönderen bir istemci uygulaması.

- Cihazınız Bu öğretici sırasında istemci uygulaması ile çalıştığından emin olun.

- [Python 2.7x veya Python 3.x](https://www.python.org/downloads/)

- Azure CLI 2.0 yükleyin. Windows’a yüklemenin kolay bir yolu, [MSI](https://aka.ms/InstallAzureCliWindows) indirip yüklemektir. De yükleme yönergelerini izleyebilirsiniz [Microsoft Docs](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI 2.0 ortamınızda kurulumu. Azure CLI 2.0 sürümünüz en az 2.0.24 veya üzerinde olmalıdır. Doğrulamak için `az –version` kullanın. 

- IOT uzantısını yükleyin. En basit yol `az extension add --name azure-cli-iot-ext` komutunu çalıştırmaktır. [IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar.


## <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

```bash
az login
```

## <a name="direct-methods"></a>Doğrudan yöntemler

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Cihaz çifti istenen özellikleri

İstenen özellik aralığını ayarlayın aşağıdaki komutu çalıştırarak 3000 =:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Bu özellik aygıtınızdan okuyabilir.

## <a name="device-twin-reported-properties"></a>Cihaz çifti özellikleri bildirdi

Aşağıdaki komutu çalıştırarak bildirilen cihaz özelliklerini alın:

```bash
az iot hub device-twin show -n <your hub name> -d <your device id>
```

Twin birini bildirilen özellikleri $metadata. cihaz uygulamasının en son ne zaman gösteren $lastUpdated kendi bildirilen bir özellik kümesi güncelleştirildi.

## <a name="device-twin-tags"></a>Cihaz çifti etiketleri

Aşağıdaki komutu çalıştırarak etiketleri ve cihaz özelliklerini görüntüle:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Bir alan rolünü ekleyin aşağıdaki komutu çalıştırarak sıcaklık ve nem aygıta =:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Cihaz çifti sorguları

Sorgu bir etiket rolünün aygıtlarla 'sıcaklık ve nem' aşağıdaki komutu çalıştırarak =:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Bir etiketi rolünün olanlar dışında tüm cihazlar sorgu aşağıdaki komutu çalıştırarak 'sıcaklık ve nem' =:

```bash
az iot hub query --hub-name <your hub name> --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Sonraki adımlar

Cihaz bulut iletilerini izlemek ve bulut-cihaz iletilerini IOT cihaz ile Azure IOT hub'ı arasında göndermek nasıl öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
