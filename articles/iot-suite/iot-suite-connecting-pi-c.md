---
title: "Sağlama C - Azure kullanarak Uzaktan izleme için Raspberry Pi'yi | Microsoft Docs"
description: "C dilinde yazılmış bir uygulaması kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümü Raspberry Pi'yi aygıt bağlanmaya açıklar"
services: iot-suite
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2017
ms.author: dobett
ms.openlocfilehash: 2f5915093a0d7984f0350af21aa438cdd588bbf2
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="connect-your-raspberry-pi-device-to-the-remote-monitoring-preconfigured-solution-c"></a>Uzaktan izleme önceden yapılandırılmış çözümü (C) Raspberry Pi'yi Cihazınızı bağlama

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, fiziksel bir aygıtı için Uzaktan izleme önceden yapılandırılmış çözümü bağlanmak nasıl gösterir. Kısıtlanmış cihazlarda çalıştırılan en katıştırılmış uygulamalarında olduğu gibi Raspberry Pi'yi cihaz uygulaması için istemci kodu, c dilinde yazılır Bu öğreticide, Raspbian işletim sistemi çalıştıran Raspberry Pi'yi uygulamayı derleyin.

### <a name="required-hardware"></a>Gerekli donanım

Komut satırı Raspberry Pi'yi üzerinde uzaktan bağlanmak etkinleştirmek için bir masaüstü bilgisayar.

[Microsoft IOT Starter Kit Raspberry Pi 3](https://azure.microsoft.com/develop/iot/starter-kits/) veya eşdeğer bileşenleri. Bu öğretici Seti'nden aşağıdaki öğeleri kullanır:

- Böğürtlenli Pi 3
- MicroSD kartı (ile NOOBS)
- Bir USB Mini kablosu
- Ethernet kablosu

### <a name="required-desktop-software"></a>Gerekli masaüstü yazılımı

Komut satırı Raspberry Pi'yi üzerinde uzaktan erişim sağlamak için Masaüstü makinenizde SSH istemcisi gerekir.

- Windows, bir SSH istemcisi içermez. Kullanmanızı öneririz [PuTTY](http://www.putty.org/).
- Çoğu Linux dağıtımları ve Mac OS komut satırı SSH yardımcı programı içerir. Daha fazla bilgi için bkz: [SSH kullanarak Linux veya Mac OS](https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md).

### <a name="required-raspberry-pi-software"></a>Gerekli Raspberry Pi'yi yazılım

Aşağıdaki adımlar, Raspberry Pi'yi önceden yapılandırılmış çözümü bağlayan bir C uygulaması oluşturmak için hazırlamak nasıl gösterir:

1. Kullanarak Raspberry Pi'yi bağlanmak `ssh`. Daha fazla bilgi için bkz: [SSH (Secure Shell)](https://www.raspberrypi.org/documentation/remote-access/ssh/README.md) üzerinde [Raspberry Pi'yi Web sitesi](https://www.raspberrypi.org/).

1. Raspberry Pi'yi güncelleştirmek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get update
    ```

1. Gerekli geliştirme araçları ve kitaplıkları, Raspberry Pi'yi eklemek için aşağıdaki komutu kullanın:

    ```sh
    sudo apt-get install g++ make cmake gcc git
    ```

1. IOT Hub istemci kitaplıkları yüklemek için aşağıdaki komutları kullanın:

    ```sh
    grep -q -F 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    grep -q -F 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' /etc/apt/sources.list || sudo sh -c "echo 'deb-src http://ppa.launchpad.net/aziotsdklinux/ppa-azureiot/ubuntu vivid main' >> /etc/apt/sources.list"
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys FDA6A393E4C2257F
    sudo apt-get update
    sudo apt-get install -y azure-iot-sdk-c-dev cmake libcurl4-openssl-dev git-core
    ```

1. Aşağıdaki komutları kullanarak, Raspberry Pi'yi Parson JSON ayrıştırıcıya kopyalama:

    ```sh
    cd ~
    git clone https://github.com/kgabis/parson.git
    ```

## <a name="create-a-project"></a>Proje oluşturma

Kullanarak aşağıdaki adımları tamamlayın `ssh` Raspberry Pi'yi bağlantı:

1. Adlı bir klasör oluşturun `remote_monitoring` Raspberry Pi'yi, ev klasöründedir. Komut satırında bu klasöre gidin:

    ```sh
    cd ~
    mkdir remote_monitoring
    cd remote_monitoring
    ```

1. Dört dosyaları oluşturma `main.c`, `remote_monitoring.c`, `remote_monitoring.h`, ve `CMakeLists.txt` içinde `remote_monitoring` klasör.

1. Adlı bir klasör oluşturun `parson` içinde `remote_monitoring` klasör.

1. Dosyaları kopyalamak `parson.c` ve `parson.h` Parson depoya yerel kopyasından `remote_monitoring/parson` klasör.

1. Bir metin düzenleyicisinde açın `remote_monitoring.c` dosya. Ya da kullanmak Raspberry Pi'yi üzerinde `nano` veya `vi` metin düzenleyici. Aşağıdaki `#include` deyimlerini ekleyin:

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

[!INCLUDE [iot-suite-connecting-code](../../includes/iot-suite-connecting-code.md)]

## <a name="add-code-to-run-the-app"></a>Uygulamayı çalıştırmak için kod ekleme

Bir metin düzenleyicisinde açın `remote_monitoring.h` dosya. Aşağıdaki kodu ekleyin:

```c
void remote_monitoring_run(void);
```

Bir metin düzenleyicisinde açın `main.c` dosya. Aşağıdaki kodu ekleyin:

```c
#include "remote_monitoring.h"

int main(void)
{
  remote_monitoring_run();

  return 0;
}
```

## <a name="build-and-run-the-application"></a>Uygulamayı derleme ve çalıştırma

Aşağıdaki adımlar nasıl kullanılacağını açıklamaktadır *CMake* istemci Uygulamanızı yapılandırmak için.

1. Bir metin düzenleyicisinde açın **CMakeLists.txt** dosyasını `remote_monitoring` klasör.

1. İstemci uygulamanızı oluşturmak nasıl tanımlamak için aşağıdaki yönergeleri ekleyin:

    ```cmake
    macro(compileAsC99)
      if (CMAKE_VERSION VERSION_LESS "3.1")
        if (CMAKE_C_COMPILER_ID STREQUAL "GNU")
          set (CMAKE_C_FLAGS "--std=c99 ${CMAKE_C_FLAGS}")
          set (CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_CXX_FLAGS}")
        endif()
      else()
        set (CMAKE_C_STANDARD 99)
        set (CMAKE_CXX_STANDARD 11)
      endif()
    endmacro(compileAsC99)

    cmake_minimum_required(VERSION 2.8.11)
    compileAsC99()

    set(AZUREIOT_INC_FOLDER "${CMAKE_SOURCE_DIR}" "${CMAKE_SOURCE_DIR}/parson" "/usr/include/azureiot" "/usr/include/azureiot/inc")

    include_directories(${AZUREIOT_INC_FOLDER})

    set(sample_application_c_files
        ./parson/parson.c
        ./remote_monitoring.c
        ./main.c
    )

    set(sample_application_h_files
        ./parson/parson.h
        ./remote_monitoring.h
    )

    add_executable(sample_app ${sample_application_c_files} ${sample_application_h_files})

    target_link_libraries(sample_app
        serializer
        iothub_client
        iothub_client_mqtt_transport
        aziotsharedutil
        umqtt
        pthread
        curl
        ssl
        crypto
        m
    )
    ```

1. İçinde `remote_monitoring` klasörünü depolamak için bir klasör oluşturun *olun* CMake oluşturur dosyaları. Ardından çalıştırın **cmake** ve **olun** gibi komutlar:

    ```sh
    mkdir cmake
    cd cmake
    cmake ../
    make
    ```

1. İstemci uygulaması çalıştırın ve IOT Hub'ına telemetri gönder:

    ```sh
    ./sample_app
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]
