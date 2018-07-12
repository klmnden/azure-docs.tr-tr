---
title: Windows cihazları uzaktan C - Azure izleme sağlama | Microsoft Docs
description: Windows üzerinde çalışan C dilinde yazılmış bir Web uygulaması kullanarak Uzaktan izleme çözüm Hızlandırıcısını için bir cihaz bağlamak açıklar.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: dobett
ms.openlocfilehash: 139daea3e885636b352d4c9a1ba2651a24195b21
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38309879"
---
# <a name="connect-your-device-to-the-remote-monitoring-solution-accelerator-windows"></a>Cihazınızı Uzaktan izleme çözüm Hızlandırıcısını için (Windows) bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğreticide, fiziksel bir cihazı Uzaktan izleme çözüm hızlandırıcısına bağlamayı gösterilmektedir.

## <a name="create-a-c-client-solution-on-windows"></a>Windows üzerinde C istemci çözümü oluşturmak

Kısıtlanmış cihazlarında çalışan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu c dilinde yazılan Bu öğreticide, Windows çalıştıran bir makinede uygulama oluşturun.

### <a name="create-the-starter-project"></a>Başlangıç projesi oluşturma

Visual Studio 2017'de bir başlangıç projesi oluşturun ve IOT Hub cihazı istemci NuGet paketlerini ekleyin:

1. Visual Studio'da C, Visual C++ kullanarak bir konsol uygulaması oluşturma **Windows konsol uygulaması** şablonu. Projeyi adlandırın **RMDevice**.

    ![Visual C++ Windows konsol uygulaması oluşturun](./media/iot-accelerators-connecting-devices/visualstudio01.png)

1. İçinde **Çözüm Gezgini**, dosyaları silmek `stdafx.h`, `targetver.h`, ve `stdafx.cpp`.

1. İçinde **Çözüm Gezgini**, dosyayı yeniden adlandırın `RMDevice.cpp` için `RMDevice.c`.

    ![Çözüm Gezgini gösteren RMDevice.c dosyası yeniden adlandırıldı](./media/iot-accelerators-connecting-devices/visualstudio02.png)

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje ve ardından **NuGet paketlerini Yönet**. Seçin **Gözat**, ardından için aşağıdaki NuGet paketlerini arayıp yükleyin:

    * Microsoft.Azure.IoTHub.Serializer
    * Microsoft.Azure.IoTHub.IoTHubClient
    * Microsoft.Azure.IoTHub.MqttTransport

    ![NuGet Paket Yöneticisi yüklü Microsoft.Azure.IoTHub paketler gösterilmektedir.](./media/iot-accelerators-connecting-devices/visualstudio03.png)

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje ve ardından **özellikleri** projenin açmak için **özellik sayfaları** iletişim kutusu. Ayrıntılar için bkz [Visual C++ proje özelliklerini ayarlama](https://docs.microsoft.com/cpp/ide/working-with-project-properties).

1. Seçin **C/C++** klasörü seçin **önceden derlenmiş üst bilgiler** özellik sayfası.

1. Ayarlama **Ön derlenmiş üstbilgi** için **önceden derlenmiş üst bilgiler kullanılmıyor**. Ardından **Uygula**.

    ![Önceden derlenmiş üst bilgiler kullanmayan proje proje özelliklerini göster](./media/iot-accelerators-connecting-devices/visualstudio04.png)

1. Seçin **bağlayıcı** klasörü seçin **giriş** özellik sayfası.

1. Ekleme `crypt32.lib` için **ek bağımlılıklar** özelliği. Proje özellik değerlerini kaydetmek için seçin **Tamam** ardından **Tamam** yeniden.

    ![Bağlayıcı crypt32.lib dahil olmak üzere proje özelliklerini göster](./media/iot-accelerators-connecting-devices/visualstudio05.png)

### <a name="add-the-parson-json-library"></a>Parson JSON kitaplığı Ekle

Parson JSON kitaplığa eklemek **RMDevice** proje ve gerekli olanları Ekle `#include` ifadeleri:

1. Bilgisayarınızda uygun bir klasörde aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

    ```cmd
    git clone https://github.com/kgabis/parson.git
    ```

1. Kopyalama `parson.h` ve `parson.c` Parson deposunun yerel kopyasındaki dosyaları, **RMDevice** proje klasörü.

1. Visual Studio'da sağ **RMDevice** projesinin **Ekle**ve ardından **var olan öğe**.

1. İçinde **varolan öğeyi Ekle** iletişim kutusunda `parson.h` ve `parson.c` dosyalar **RMDevice** proje klasörü. Bu iki dosyayı projenize eklemek için **Ekle**.

    ![Çözüm Gezgini parson.h ve parson.c dosyaları gösterir.](./media/iot-accelerators-connecting-devices/visualstudio06.png)

1. Visual Studio'da açın `RMDevice.c` dosya. Varolan `#include` deyimlerini aşağıdaki kod ile:

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
    > Artık projenize çözümü oluşturarak ayarlanan doğru bağımlılıkları olduğunu doğrulayabilirsiniz.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Çağırmak için kod ekleme **uzak\_izleme\_çalıştırma** işlevini sonra oluşturun ve cihaz uygulamayı çalıştırın:

1. Çağrılacak **uzak\_izleme\_çalıştırma** işlev, değiştirin **ana** işlevi aşağıdaki kod ile:

    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Seçin **derleme** ardından **Çözümü Derle** cihaz uygulamayı oluşturmak için.

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** projesinin **hata ayıklama**ve ardından **yeni örnek Başlat** örneği çalıştırmak için . Konsol iletileri olarak gösterir:

    * Uygulama, çözüm hızlandırıcısına örnek telemetri gönderir.
    * Çözüm panosunda ayarlanan istenen özellik değerlerini alır.
    * Çözüm panosundan çağrılan yöntemlere verir.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
