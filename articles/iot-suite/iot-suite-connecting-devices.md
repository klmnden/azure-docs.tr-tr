---
title: "Uzaktan izleme C - Azure için Windows cihazlara sağlamak | Microsoft Docs"
description: "Windows üzerinde çalışan C yazılmış bir uygulama kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümü bir aygıt bağlanmaya açıklar."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 83d0427a3ba8c634699608c38ab22efb1f275e52
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Cihazınızı bağlama Uzaktan izleme önceden yapılandırılmış çözümü için (Windows)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, fiziksel bir aygıtı için Uzaktan izleme önceden yapılandırılmış çözümü bağlanmak nasıl gösterir.

## <a name="create-a-c-client-solution-on-windows"></a>Windows üzerinde bir C istemci çözümü oluşturma

Kısıtlanmış cihazlarda çalıştırılan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu, c dilinde yazılır Bu öğreticide, uygulamayı Windows çalıştıran bir makinede oluşturun.

### <a name="create-the-starter-project"></a>Başlangıç projesi oluşturma

Visual Studio 2017 içinde bir başlangıç projesi oluşturun ve IOT Hub cihaz istemcisi NuGet paketleri ekleyin:

1. Visual Studio'da Visual C++ kullanarak C konsol uygulaması oluşturma **Windows konsol uygulaması** şablonu. Proje adı **RMDevice**.

    ![Visual C++ Windows konsol uygulaması oluşturun](media/iot-suite-connecting-devices/visualstudio01.png)

1. İçinde **Çözüm Gezgini**, dosyaları silmek `stdafx.h`, `targetver.h`, ve `stdafx.cpp`.

1. İçinde **Çözüm Gezgini**, dosyayı yeniden adlandırın `RMDevice.cpp` için `RMDevice.c`.

    ![Çözüm Gezgini gösteren RMDevice.c dosyası yeniden adlandırıldı](media/iot-suite-connecting-devices/visualstudio02.png)

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje ve ardından **Manage NuGet paketleri**. Seçin **Gözat**, ardından aramak ve aşağıdaki NuGet paketlerini yükleyin:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![NuGet Paket Yöneticisi yüklü Microsoft.Azure.IoTHub paketleri gösterir](media/iot-suite-connecting-devices/visualstudio03.png)

1. İçinde **Çözüm Gezgini**, sağ tıklayın **RMDevice** proje ve ardından **özellikleri** projenin açmak için **özellik sayfaları** iletişim kutusu. Ayrıntılar için bkz [Visual C++ proje özelliklerini ayarlama](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Seçin **C/C++** klasörü, ardından **önceden derlenmiş üstbilgiler** özellik sayfası.

1. Ayarlama **önceden derlenmiş üstbilgi** için **önceden derlenmiş üstbilgiler kullanmıyorsa**. Ardından **Uygula**.

    ![Önceden derlenmiş başlıkları kullanma değil proje proje özelliklerini göster](media/iot-suite-connecting-devices/visualstudio04.png)

1. Seçin **bağlayıcı** klasörü, ardından **giriş** özellik sayfası.

1. Ekleme `crypt32.lib` için **ek bağımlılıklar** özelliği. Proje özellik değerlerini kaydetmek üzere seçim yapın **Tamam** ve ardından **Tamam** yeniden.

    ![Proje Özellikleri crypt32.lib dahil olmak üzere bağlayıcı Göster](media/iot-suite-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Parson JSON kitaplığı Ekle

Parson JSON kitaplığa eklemek **RMDevice** proje ve gerekli eklemek `#include` deyimleri:

1. Bilgisayarınızda uygun klasöründe aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopya `parson.h` ve `parson.c` Parson depoya yerel kopyasını dosyalarından, **RMDevice** proje klasörü.

1. Visual Studio'da sağ **RMDevice** seçin, proje **Ekle**ve ardından **varolan öğeyi**.

1. İçinde **varolan öğeyi Ekle** iletişim kutusunda `parson.h` ve `parson.c` dosyalar **RMDevice** proje klasörü. Bu iki dosyayı projenize eklemek için **Ekle**.

    ![Çözüm Gezgini parson.h ve parson.c dosyaları gösterir](media/iot-suite-connecting-devices/visualstudio06.png)

1. Visual Studio'da açın `RMDevice.c` dosya. Varolan `#include` deyimleri ile aşağıdaki kodu:

    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include <string.h>
    ```

    > [!NOTE]
    > Şimdi projenizi çözümü oluşturma tarafından ayarlanan doğru bağımlılıkları olduğunu doğrulayabilirsiniz.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Çağırmak için kodu ekleyin **uzak\_izleme\_çalıştırmak** işlev, ardından yapı ve cihaz uygulamayı çalıştırın:

1. Çağrılacak **uzak\_izleme\_çalıştırmak** işlev, yerini **ana** işlevi aşağıdaki kod ile:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Seçin **yapı** ve ardından **yapı çözümü** aygıt uygulama oluşturmak için.

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** projesi, seçin **hata ayıklama**ve ardından **başlangıç yeni örnek** örneği çalıştırmak için . Konsol iletileri olarak görüntüler:

    * Uygulama, önceden yapılandırılmış çözümü örnek telemetri gönderir.
    * İstenen özellik değerleri çözüm panosunda alır.
    * Çözüm panodan çağrılan yöntemlerine yanıt verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
