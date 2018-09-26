---
title: Windows üzerinde C kullanarak bir cihazı bağlayın | Microsoft Docs
description: Bir cihaz üzerinde Windows çalıştıran C dilinde yazılmış bir Web uygulaması kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümüne bağlanmak açıklar.
services: ''
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 34e39a58-2434-482c-b3fa-29438a0c05e8
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/02/2017
ms.author: dobett
ms.openlocfilehash: 56c58a31c6b20bd8da7d1442ae75425cb3e20f3d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093104"
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-windows"></a>Cihazınızı Uzaktan izleme önceden yapılandırılmış çözüme (Windows) bağlama
[!INCLUDE [iot-suite-v1-selector-connecting](../../includes/iot-suite-v1-selector-connecting.md)]

## <a name="create-a-c-sample-solution-on-windows"></a>Windows üzerinde C örnek bir çözüm oluşturma
Aşağıdaki adımlarda, Uzaktan izleme önceden yapılandırılmış çözümü ile iletişim kuran istemci uygulaması oluşturma gösterilmektedir. Bu uygulama C dilinde yazılan ve oluşturulan ve Windows üzerinde çalıştırın.

Visual Studio 2015 veya Visual Studio 2017 ' bir başlangıç projesi oluşturun ve IOT Hub cihazı istemci NuGet paketlerini ekleyin:

1. Visual Studio'da C, Visual C++ kullanarak bir konsol uygulaması oluşturma **Win32 konsol uygulaması** şablonu. Projeyi adlandırın **RMDevice**.
2. Üzerinde **uygulama ayarları** sayfasını **Win32 Uygulama Sihirbazı**, emin **konsol uygulaması** seçilidir ve işaretini kaldırın **önceden derlenmiş üst bilgi** ve **Security Development Lifecycle (SDL) denetimleri**.
3. İçinde **Çözüm Gezgini**, dosyaları stdafx.h, targetver.h ve stdafx.cpp silin.
4. İçinde **Çözüm Gezgini**, ' % s'dosyasını RMDevice.cpp RMDevice.c için yeniden adlandırın.
5. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje ve ardından **NuGet paketlerini Yönet**. Tıklayın **Gözat**, ardından için aşağıdaki NuGet paketlerini arayıp yükleyin:
   
   * Microsoft.Azure.IoTHub.Serializer
   * Microsoft.Azure.IoTHub.IoTHubClient
   * Microsoft.Azure.IoTHub.MqttTransport
6. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje ve ardından **özellikleri** projenin açmak için **özellik sayfaları** iletişim kutusu. Ayrıntılar için bkz [Visual C++ proje özelliklerini ayarlama][lnk-c-project-properties]. 
7. Tıklayın **bağlayıcı** klasörü, ardından **giriş** özellik sayfası.
8. Ekleme **crypt32.lib** için **ek bağımlılıklar** özelliği. Tıklayın **Tamam** ardından **Tamam** proje özellik değerlerini yeniden kaydetmek için.

Parson JSON kitaplığa eklemek **RMDevice** proje ve gerekli olanları Ekle `#include` ifadeleri:

1. Bilgisayarınızda uygun bir klasörde aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

    ```
    git clone https://github.com/kgabis/parson.git
    ```

1. Parson deposunun yerel kopyasındaki parson.h ve parson.c dosyaları kopyalayın, **RMDevice** proje klasörü.

1. Visual Studio'da sağ tıklayıp **RMDevice** proje, tıklayın **Ekle**ve ardından **varolan öğe**.

1. İçinde **varolan öğeyi Ekle** parson.h ve parson.c Dosyaları iletişim kutusunda **RMDevice** proje klasörü. Ardından **Ekle** bu iki dosyayı projenize eklemek için.

1. Visual Studio'da RMDevice.c dosyasını açın. Varolan `#include` deyimlerini aşağıdaki kod ile:
   
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
    > Artık, projenizi oluşturmaya tarafından ayarlanan doğru bağımlılıkları olduğunu doğrulayabilirsiniz.

[!INCLUDE [iot-suite-v1-connecting-code](../../includes/iot-suite-v1-connecting-code.md)]

## <a name="build-and-run-the-sample"></a>Örneği derleme ve çalıştırma

Çağırmak için kod ekleme **uzak\_izleme\_çalıştırma** işlev oluşturun ve cihaz uygulamayı çalıştırın.

1. Değiştirin **ana** işlevini çağırmak için aşağıdaki kodu kullanarak **uzak\_izleme\_çalıştırma** işlevi:
   
    ```c
    int main()
    {
      remote_monitoring_run();
      return 0;
    }
    ```

1. Tıklayın **derleme** ardından **Çözümü Derle** cihaz uygulamayı oluşturmak için.

1. İçinde **Çözüm Gezgini**, sağ **RMDevice** proje, tıklayın **hata ayıklama**ve ardından **yeni örnek Başlat** örneği çalıştırmak için. Uygulama, önceden yapılandırılmış çözüme örnek telemetri gönderir, çözüm panosunda ayarlanan istenen özellik değerlerini alır ve çözüm panosundan çağrılan yöntemlere yanıt veren konsol iletileri gösterir.

[!INCLUDE [iot-suite-v1-visualize-connecting](../../includes/iot-suite-v1-visualize-connecting.md)]

[lnk-c-project-properties]: https://msdn.microsoft.com/library/669zx6zc.aspx
