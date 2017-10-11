---
title: "Windows C kullanarak bir cihazı bağlanma | Microsoft Docs"
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
ms.date: 08/24/2017
ms.author: dobett
ms.openlocfilehash: d222bcbd64f288d4091acb0ecd2922b9ceee57e5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Cihazınızı bağlama Uzaktan izleme önceden yapılandırılmış çözümü için (Windows)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Windows C örnek bir çözüm oluşturun
Aşağıdaki adımlar iletişim kuran bir istemci uygulamasının Uzaktan izleme önceden yapılandırılmış çözümü nasıl oluşturulacağını gösterir. Bu uygulama C'de yazılmış ve yerleşik ve Windows üzerinde çalıştırın.

Visual Studio 2015 veya Visual Studio 2017 içinde bir başlangıç projesi oluşturun ve IOT Hub cihaz istemcisi NuGet paketleri ekleyin:

1. Visual Studio'da Visual C++ kullanarak C konsol uygulaması oluşturma **Win32 konsol uygulaması** şablonu. Proje adı **RMDevice**.
2. Üzerinde **uygulama ayarları** sayfasındaki **Win32 Uygulama Sihirbazı'nı**, emin **konsol uygulaması** seçilidir ve işaretini **önceden derlenmiş Üstbilgi** ve **güvenlik geliştirme yaşam döngüsü (SDL) denetler**.
3. İçinde **Çözüm Gezgini**, dosyaları stdafx.h, targetver.h ve stdafx.cpp silin.
4. İçinde **Çözüm Gezgini**, dosyayı RMDevice.cpp RMDevice.c için yeniden adlandırın.
5. İçinde **Çözüm Gezgini**, sağ tıklayın **RMDevice** proje ve ardından **Manage NuGet paketleri**. Tıklatın **Gözat**, ardından aramak ve aşağıdaki NuGet paketlerini yükleyin:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. İçinde **Çözüm Gezgini**, sağ tıklayın **RMDevice** proje ve ardından **özellikleri** projenin açmak için **özellik sayfaları** iletişim kutusu. Ayrıntılar için bkz [Visual C++ proje özelliklerini ayarlama][lnk-c-project-properties]. 
7. Tıklatın **bağlayıcı** klasörü, ardından **giriş** özellik sayfası.
8. Ekleme **crypt32.lib** için **ek bağımlılıklar** özelliği. Tıklatın **Tamam** ve ardından **Tamam** projeyi özellik değerlerini yeniden kaydetmek için.

Parson JSON kitaplığa eklemek **RMDevice** proje ve gerekli eklemek `#include` deyimleri:

1. Bilgisayarınızda uygun klasöründe aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Parson depoya yerel kopyasını parson.h ve parson.c dosyalarını kopyalayın, **RMDevice** proje klasörü.

1. Visual Studio'da sağ **RMDevice** proje, tıklatın **Ekle**ve ardından **varolan öğeyi**.

1. İçinde **varolan öğeyi Ekle** parson.h ve parson.c Dosyaları iletişim kutusunda **RMDevice** proje klasörü. Ardından **Ekle** bu iki dosyayı projenize eklemek için.

1. Visual Studio'da RMDevice.c dosyasını açın. Varolan `#include` deyimleri ile aşağıdaki kodu:
   
    ```c
    #include "iothubtransportmqtt.h"
    #include "schemalib.h"
    #include "iothub_client.h"
    #include "serializer_devicetwin.h"
    #include "schemaserializer.h"
    #include "azure_c_shared_utility/threadapi.h"
    #include "azure_c_shared_utility/platform.h"
    #include "parson.h"
    ```

    > [!NOTE]
    > Artık, projenizi derleme tarafından ayarlanan doğru bağımlılıkları olduğunu doğrulayabilirsiniz.

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Derleme ve örnek çalıştırma

Çağırmak için kodu ekleyin **uzak\_izleme\_çalıştırmak** işlev oluşturmak ve cihaz uygulamayı çalıştırın.

1. Değiştir **ana** işlevi çağırmak için aşağıdaki kod ile **uzak\_izleme\_çalıştırmak** işlevi:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Tıklatın **yapı** ve ardından **yapı çözümü** aygıt uygulama oluşturmak için.

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** projesi,'ı tıklatın **hata ayıklama**ve ardından **başlangıç yeni örnek** örneği çalıştırmak için. Konsol Uygulama önceden yapılandırılmış çözümü örnek telemetri gönderir, istenen özellik değerleri çözüm panosunda alır ve çözüm panodan çağrılan yöntemlerine yanıt iletileri görüntüler.

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
