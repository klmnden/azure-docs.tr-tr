---
title: "Azure IOT cihaz yönetimini IOT uzantısı ile Azure CLI 2.0 | Microsoft Docs"
description: "Azure IOT Hub cihaz yönetimi, doğrudan yöntemleri ve Twin'ın istenen özellikleri yönetim seçenekleri için Azure CLI 2.0 aracı IOT uzantısını kullanır."
services: iot-hub
documentationcenter: 
author: chrissie926
manager: timlt
tags: 
keywords: "Azure IOT cihaz yönetimi, azure IOT hub cihaz yönetimi, cihaz yönetim IOT, IOT hub cihaz Yönetimi"
ms.assetid: b34f799a-fc14-41b9-bf45-54751163fffe
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: menchi
ms.openlocfilehash: 760a6a30513308aa59c5e253e3b91e28cf9e3241
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="use-the-iot-extension-for-azure-cli-20-for-azure-iot-hub-device-management"></a>IOT uzantısı Azure CLI 2.0 için Azure IOT Hub cihaz yönetimi için kullanın.

![Uçtan uca diyagramı](media/iot-hub-get-started-e2e-diagram/2.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[Azure CLI 2.0 IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) yeni bir açık kaynak eklediğinden IOT uzantısı [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/overview?view=azure-cli-latest) Azure Kaynak Yöneticisi'ni ve yönetim uç ile etkileşim için komutlar içerir. Azure CLI 2.0 Azure Kaynak Yöneticisi'ni ve yönetim uç ile etkileşim için komutlar içerir. Örneğin, bir Azure VM veya bir IOT hub'ı oluşturmak için Azure CLI 2.0 kullanabilirsiniz. CLI uzantısını ek hizmete özgü özellikleri erişim Azure CLI vermiş büyütmek bir Azure hizmetini etkinleştirir. IOT uzantısı IOT geliştiriciler, tüm IOT Hub, IOT kenarı ve IOT Hub cihaz hizmeti sağlama olanağı komut satırı erişim sağlar.

| Yönetim seçeneği          | Görev                                                                                                                            |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------------|
| Doğrudan yöntemler             | Başlatma veya ileti gönderme veya cihaz yeniden başlatıldığı durdurma gibi davranacak bir aygıtı olun.                                        |
| İstenen twin özellikleri    | Bir aygıt için yeşil bir Işığı ayarlama veya telemetri gönderme aralığı 30 dakika gibi bazı durumların yerleştirin.         |
| Özellikler Twin bildirdi   | Bir aygıt bildirilen durumunu alın. Örneğin, aygıt LED şimdi yanıp sönen bildirir.                                    |
| Twin etiketleri                  | Aygıta özgü meta verileri bulutta depolayın. Örneğin, bir satış makinenin dağıtım konumu.                         |
| Cihaz çifti sorguları        | Kullanılabilir aygıtları tanımlama gibi rasgele koşullarla almak için tüm cihaz çiftlerini sorgulamak. |

Daha ayrıntılı açıklama farklara ve bu seçenekleri kullanma konusunda yönergeler için bkz: [cihaz bulut iletişimi Kılavuzu](iot-hub-devguide-d2c-guidance.md) ve [bulut-cihaz iletişimi Kılavuzu](iot-hub-devguide-c2d-guidance.md).

> [!NOTE]
> Cihaz çiftleri, cihaz durumu bilgilerini (meta veriler, yapılandırmalar ve koşullar) depolayan JSON belgelerdir. IOT Hub cihaz çifti ona bağlanan her aygıt için devam ettirir. Cihaz çiftlerini hakkında daha fazla bilgi için bkz: [cihaz çiftlerini ile çalışmaya başlama](iot-hub-node-node-twin-getstarted.md).

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

- Azure CLI 2.0 yükleyin. Windows yüklemek için bir basit yoludur indirmek ve yüklemek için [MSI](https://aka.ms/InstallAzureCliWindows). De yükleme yönergelerini izleyebilirsiniz [Microsoft Docs](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) Azure CLI 2.0 ortamınızda kurulumu. En azından, Azure CLI 2.0 sürümünüzün 2.0.24 olması gerekir veya üstü. Kullanım `az –version` doğrulamak için. 

- IOT uzantısını yükleyin. En basit yolu çalıştırmaktır `az extension add --name azure-cli-iot-ext`. [IOT uzantısı Benioku](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) uzantıyı yüklemek için çeşitli yollar açıklanmaktadır.


## <a name="login-to-your-azure-account"></a>Azure hesabınızda oturum açın

Aşağıdaki komutu çalıştırarak Azure hesabınızda oturum açın:

```bash
az login
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-direct-methods"></a>IOT uzantısı Azure CLI 2.0 için doğrudan yöntemleriyle kullanın.

```bash
az iot hub invoke-device-method --device-id <your device id> --hub-name <your hub name> --method-name <the method name> --method-payload <the method payload>
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-desired-properties"></a>IOT uzantısı için Azure CLI 2.0 twin'ın istenen özelliklere sahip kullanın.

İstenen özellik aralığını ayarlayın aşağıdaki komutu çalıştırarak 3000 =:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.desired.interval = 3000
```

Bu özellik aygıtınızdan okuyabilir.

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-reported-properties"></a>IOT uzantısı için Azure CLI 2.0 twin'ın bildirilen özelliklerle kullanın.

Aşağıdaki komutu çalıştırarak bildirilen cihaz özelliklerini alın:

```bash
az iot hub device-twin update -n <your hub name> -d <your device id> --set properties.reported.interval = 3000
```

$Metadata özellikleri biridir. Bu aygıtın son zamanı gösteren $lastUpdated gönderir veya bir ileti alır.

## <a name="use-the-iot-extension-for-azure-cli-20-with-twins-tags"></a>IOT uzantısı için Azure CLI 2.0 twin'ın etiketleriyle kullanın.

Aşağıdaki komutu çalıştırarak etiketleri ve cihaz özelliklerini görüntüle:

```bash
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Bir alan rolünü ekleyin aşağıdaki komutu çalıştırarak sıcaklık ve nem aygıta =:

```bash
az iot hub device-twin update --hub-name <your hub name> --device-id <your device id> --set tags = '{"role":"temperature&humidity"}}'
```

## <a name="use-the-iot-extension-for-azure-cli-20-with-device-twins-queries"></a>IOT uzantısı Azure CLI 2.0 için cihaz çiftlerini sorgularıyla kullanın.

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