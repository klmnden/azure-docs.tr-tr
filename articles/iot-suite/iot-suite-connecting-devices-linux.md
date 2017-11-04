---
title: "Uzaktan izleme c - Azure Linux aygıtlara sağlamak | Microsoft Docs"
description: "Linux üzerinde çalışan C yazılmış bir uygulama kullanarak Azure IOT paketi önceden yapılandırılmış Uzaktan izleme çözümü bir aygıt bağlanmaya açıklar."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0c7c8039-0bbf-4bb5-9e79-ed8cff433629
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/16/2017
ms.author: dobett
ms.openlocfilehash: 542d1e0c4c4d6cfa5d2fe9df90a7a34c72f19fc0
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-linux"></a>Cihazınızı bağlama Uzaktan izleme önceden yapılandırılmış çözümü (Linux)

[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

Bu öğretici, fiziksel bir aygıtı için Uzaktan izleme önceden yapılandırılmış çözümü bağlanmak nasıl gösterir.

## <a name="create-a-c-client-project-on-linux"></a>Linux üzerinde bir C istemci projesi oluşturma

Kısıtlanmış cihazlarda çalıştırılan en katıştırılmış uygulamalarında olduğu gibi cihaz uygulaması için istemci kodu, c dilinde yazılır Bu öğreticide, Ubuntu (Linux) çalıştıran bir makinede uygulama oluşturun.

Bu adımları tamamlamak için Ubuntu sürüm 15.04 veya sonraki sürümünü çalıştıran bir cihazda gerekir. Devam etmeden önce aşağıdaki komutu kullanarak, Ubuntu aygıtınızda önkoşul yükleyin:

```sh
sudo apt-get install cmake gcc g++
```

### <a name="install-the-client-libraries-on-your-device"></a>İstemci kitaplıkları cihazınıza yükleyin

Azure IOT Hub istemci kitaplıkları, Ubuntu aygıt kullanarak yükleyebilirsiniz bir paketi olarak kullanılabilir **get apt** komutu. Ubuntu bilgisayarınızdaki IOT Hub istemci kitaplığı ve başlık dosyaları içeren paket yüklemek için aşağıdaki adımları tamamlayın:

1. Bir Kabuğu'nda, bilgisayarınıza AzureIoT deposunu ekleyin:

    ```sh
    sudo add-apt-repository ppa:aziotsdklinux/ppa-azureiot
    sudo apt-get update
    ```

1. Azure-IOT-sdk-c-dev paketini yükle

    ```sh
    sudo apt-get install -y azure-iot-sdk-c-dev
    ```

### <a name="install-the-parson-json-parser"></a>Parson JSON ayrıştırıcı yükleyin

IOT Hub istemci kitaplıkları Parson JSON ayrıştırıcı ileti yükü ayrıştırmak için kullanın. Bilgisayarınızda uygun klasöründe aşağıdaki komutu kullanarak Parson GitHub deposunu kopyalayın:

```sh
git clone https://github.com/kgabis/parson.git
```

### <a name="prepare-your-project"></a>Projenizi hazırlama

Ubuntu makinenizde adlı bir klasör oluşturun `remote_monitoring`. İçinde `remote_monitoring` klasörü:

- Dört dosyaları oluşturma `main.c`, `remote_monitoring.c`, `remote_monitoring.h`, ve `CMakeLists.txt`.
- Adlı bir klasör oluşturun `parson`.

Dosyaları kopyalamak `parson.c` ve `parson.h` Parson depoya yerel kopyasından `remote_monitoring/parson` klasör.

Bir metin düzenleyicisinde açın `remote_monitoring.c` dosya. Aşağıdaki `#include` deyimlerini ekleyin:

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
