---
title: Azure IOT cihaz yönetimi ile IOT uzantısı için Azure CLI | Microsoft Docs
description: IOT uzantısı için doğrudan yöntemleri ve İkizinin istenen özellikleri yönetim seçenekleri, Azure IOT Hub cihaz yönetimi için Azure CLI aracını kullanın.
author: chrissie926
manager: ''
keywords: Azure IOT cihaz yönetimi, azure IOT hub cihaz yönetimi, cihaz Yönetimi IOT, IOT hub cihaz Yönetimi
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.tgt_pltfrm: arduino
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 6b1029c5532e106c269b47e6e184b9c93faf8d09
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59681429"
---
# <a name="use-the-iot-extension-for-azure-cli-for-azure-iot-hub-device-management"></a>IOT uzantısı, Azure CLI için Azure IOT Hub cihaz yönetimi için kullanın.

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) yeni açık kaynak özelliklerini ekler IOT uzantısı [Azure CLI](https://docs.microsoft.com/cli/azure/overview?view=azure-cli-latest). Azure CLI, Azure resource manager ve yönetim uç noktaları ile etkileşime yönelik komutlar içerir. Örneğin, bir Azure VM veya IOT hub'ı oluşturmak için Azure CLI'yı kullanabilirsiniz. CLI uzantısı, Azure CLI, ek hizmete özgü özellikler erişmenize olanak tanır genişletmek bir Azure hizmeti sağlar. IOT uzantısı IOT geliştiriciler, tüm IOT Hub, IOT Edge ve IOT Hub cihazı sağlama hizmeti özellikleri için komut satırı erişmenizi sağlar.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

| Yönetim seçeneği          | Görev  |
|----------------------------|-----------|
| Doğrudan yöntemler             | Başlatma veya ileti göndermek ya da cihazın yeniden başlatılması durdurma gibi davranacak bir cihaz olun.                                        |
| Çiftinin istenen özelliklerini    | Bir cihaz için yeşil bir LED ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| İkiz bildirilen özellikler   | Bir cihaz bildirilen durumunu alın. Örneğin, cihaz LED artık yanıp sönen bildirir.                                    |
| İkiz etiketleri                  | Cihaza özgü meta veriler bulutta Store. Örneğin, bir satış makinenin dağıtım konumu.                         |
| Cihaz çifti sorguları        | Tüm cihaz çiftlerini olanlar gibi kullanıma hazır olan cihazları tanımlayan rasgele koşullarla almak için sorgulayın. |

Daha ayrıntılı açıklama farklılıklarla ilgili ve bu seçenekleri kullanma yönergeleri için bkz. [CİHAZDAN buluta iletişim Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut buluttan cihaza iletişim Kılavuzu](iot-hub-devguide-c2d-guidance.md).

Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT hub'ı ona bağlanan her cihaz için cihaz ikizi'ni kalıcıdır. Cihaz ikizleri hakkında daha fazla bilgi için bkz: [cihaz ikizlerini kullanmaya başlama](iot-hub-node-node-twin-getstarted.md).

## <a name="what-you-learn"></a>Öğrenecekleriniz

Geliştirme makinenizde çeşitli yönetim seçenekleri ile Azure CLI için IOT uzantısı kullanarak bilgi edinin.

## <a name="what-you-do"></a>Neler

Azure CLI ve IOT uzantısını Azure CLI için çeşitli yönetim seçenekler ile Çalıştır.

## <a name="what-you-need"></a>Ne gerekiyor

* Tamamlamak [Raspberry Pi çevrimiçi simülatör](iot-hub-raspberry-pi-web-simulator-get-started.md) öğretici veya bir cihaz öğreticileri; Örneğin, [node.js ile Raspberry Pi](iot-hub-raspberry-pi-kit-node-get-started.md). Bunlar aşağıdaki gereksinimleri ele::

  - Etkin bir Azure aboneliği.
  - Azure IOT hub, aboneliğiniz altında.
  - Azure IOT hub'ınıza ileti gönderen bir istemci uygulaması.

* Cihazınız, Bu öğretici sırasında istemci uygulaması ile çalıştığından emin olun.

* [Python 2.7x veya Python 3.x](https://www.python.org/downloads/)

* Azure CLI. Yüklemeniz gerekiyorsa bkz [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). En az 2.0.24 Azure CLI sürümünüzü olmalıdır veya üzeri. Doğrulamak için `az –version` kullanın. 

* IOT uzantısını yükleyin. En basit yol `az extension add --name azure-cli-iot-ext` komutunu çalıştırmaktır. [IoT uzantısı benioku](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) dosyası, uzantıyı yüklemenin birkaç yolunu açıklar.

## <a name="log-in-to-your-azure-account"></a>Azure hesabınızda oturum açma

Aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

```bash
az login
```

## <a name="direct-methods"></a>Doğrudan yöntemler

```bash
az iot hub invoke-device-method --device-id <your device id> \
  --hub-name <your hub name> \
  --method-name <the method name> \
  --method-payload <the method payload>
```

## <a name="device-twin-desired-properties"></a>Cihaz çiftinin istenen özelliklerini

İstenen özellik aralığı ayarlamak aşağıdaki komutu çalıştırarak 3000 =:

```bash
az iot hub device-twin update -n <your hub name> \
  -d <your device id> --set properties.desired.interval = 3000
```

Bu özellik, cihazınızın okuyabilirsiniz.

## <a name="device-twin-reported-properties"></a>Özellikler cihaz çiftinin bildirilen

Cihaz, bildirilen özellikleri, aşağıdaki komutu çalıştırarak alın:

```bash
az iot hub device-twin show -n <your hub name> -d <your device id>
```

İkiz birini bildirilen özellikler $metadata. son cihaz uygulamasını gösteren $lastUpdated bildirilen özellik kendine güncelleştirildi.

## <a name="device-twin-tags"></a>Cihaz ikizi etiketleri

Cihaz özelliklerini ve etiketlerini aşağıdaki komutu çalıştırarak görüntüle:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Bir alan rolünü ekleyin aşağıdaki komutu çalıştırarak sıcaklık ve nem cihaza =:

```bash
az iot hub device-twin update \
  --hub-name <your hub name> \
  --device-id <your device id> \
  --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="device-twin-queries"></a>Cihaz çifti sorguları

Sorgu rolü bir etikete sahip cihazları aşağıdaki komutu çalıştırarak 'sıcaklık ve nem' =:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Bir etiketi rolünün olanlar dışında tüm cihazlar sorgu aşağıdaki komutu çalıştırarak 'sıcaklık ve nem' =:

```bash
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## <a name="next-steps"></a>Sonraki adımlar

CİHAZDAN buluta iletileri izlemeye ve Azure IOT Hub ve IOT cihaz arasında bulut-cihaz iletilerini göndermek öğrendiniz.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
