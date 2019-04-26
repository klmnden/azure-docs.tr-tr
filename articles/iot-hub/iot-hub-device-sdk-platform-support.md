---
title: Azure IOT cihaz SDK'ları platformu desteği | Microsoft Docs
description: Kavramlar - Azure IOT cihaz SDK'ları tarafından desteklenen platformlar listesi
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: yizhon
ms.openlocfilehash: 7bcc1bf6b734abe202c5fec5d515604f4bf8e4a7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60398714"
---
# <a name="azure-iot-sdks-platform-support"></a>Azure IOT SDK'ları Platform desteği

[Azure IOT SDK'ları](iot-hub-devguide-sdks.md) kitaplıkları ile IOT Hub ve cihaz sağlama hizmeti geniş bir dil ve platform desteği ile etkileşim kurmak için bir dizi. En yaygın platformlar üzerinde SDK'ları çalıştırmak ve geliştiriciler bağlantı noktası için belirli platform C SDK'sı izleyerek [Taşıma Kılavuzu](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md). 

Microsoft, çeşitli işletim sistemlerini/platformları/çerçevesini destekler ve Azure IOT C SDK'sını kullanarak genişletilebilir. Bazı kullanıcılar bekleyebileceğiniz destek düzeyini temsil eden katmanlarına gruplandırılmış ekibi tarafından resmi olarak desteklenir. *Tam olarak desteklenen platformlar* anlamına gelir, Microsoft:

- Sürekli derlemeler ve ana ve desteklenen LTS sürüm(ler) karşı uçtan uca testleri çalıştırır.  Test kapsamı, farklı sürümler arasında sağlamak için genellikle en son LTS sürümünü ve en popüler sürüm karşı test ederiz.  Platform sürümü uyumluluğu diğer sürümleri aynı platform desteklenmiyor olabilir.
- Yükleme yönergeleri veya paketleri varsa sağlar.
- Tam olarak Github'da platformları destekler.

Ayrıca, iş ortaklarının listesi bağlantı noktalı C SDK'mız daha fazla platformda açın ve platform Soyutlama Katmanı (PAL) bakımını yapma. [Azure IOT için sertifikalı cihaz Kataloğu](https://catalog.azureiotsolutions.com/) ayrıca özellikleri çeşitli SDK'lar OS platformların bir listesini sınanmıştır karşı. SDK'ları da düzenli olarak sınırlı test ile bu platformlardaki oluşturun ve destekler:

* MBED2
* Arduino
* Windows CE 2013 (Ekim 2018'de kullanımdan)
* .NET Core 2.1 ve .NET Framework 4.7 ile .NET standart 1.3
* Xamarin iOS, Android, UWP

## <a name="supported-platforms"></a>Desteklenen platformlar

Desteklenen çeşitli platformlar vardır.

### <a name="c-sdk"></a>C SDK'SI

| İşletim Sistemi                  | Arch | Derleyici             | TLS kitaplığı       |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | gcc 5.4.0            | openssl - 1.0.2g |
| Ubuntu 18.04 LTS    | X64  | gcc 7.3              | WolfSSL – 1.13    |
| Ubuntu 18.04 LTS    | X64  | Clang 6.0.X          | Openssl – 1.1.0g  |
| OSX 10.13.4         | x64  | XCode 9.4.1          | Yerel OSX        |
| Windows Server 2016 | x64  | Visual Studio 14.0.X | SChannel          |
| Windows Server 2016 | x86  | Visual Studio 14.0.X | SChannel          |
| Debian 9 Uzat    | x64  | gcc 7.3              | Openssl – 1.1.0f  |

### <a name="python-sdk"></a>Python SDK'sı

| İşletim Sistemi                  | Arch | Derleyici   | TLS kitaplığı |
|---------------------|------|------------|-------------|
| Windows Server 2016 | x86  | Python 2.7 | openssl     |
| Windows Server 2016 | x64  | Python 2.7 | openssl     |
| Windows Server 2016 | x86  | Python 3.5 | openssl     |
| Windows Server 2016 | x64  | Python 3.5 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 2.7 | openssl     |
| Ubuntu 18.04 LTS    | x86  | Python 3.4 | openssl     |
| Yüksek MacOS Sierra   | x64  | Python 2.7 | openssl     |

### <a name="net-sdk"></a>.NET SDK

| İşletim Sistemi                  | Arch | Framework            | Standart          |
|---------------------|------|----------------------|-------------------|
| Ubuntu 16.04 LTS    | X64  | .NET Core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET Core 2.1        | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET framework 4.7   | .NET standard 2.0 |
| Windows Server 2016 | X64  | .NET Framework 4.5.1 | Yok               |

### <a name="nodejs-sdk"></a>Node.js SDK'sı

| İşletim Sistemi                                           | Arch | Düğüm sürümü |
|----------------------------------------------|------|--------------|
| Ubuntu 16.04 LTS (düğüm 6 docker görüntüsü kullanma) | X64  | Düğüm 6       |
| Windows Server 2016                          | X64  | Düğüm 6       |

### <a name="java-sdk"></a>Java SDK

| İşletim Sistemi                  | Arch | Java sürümü |
|---------------------|------|--------------|
| Ubuntu 16.04 LTS    | X64  | Java 8       |
| Windows Server 2016 | X64  | Java 8       |
| Android API 28 | X64  | Java 8       |
| Android şeyler | X64  | Java 8      |

## <a name="partner-supported-platforms"></a>İş ortağı desteklenen platformlar

Müşteriler, platform desteğimiz genişletebilir SDK'ın (PAL) platform Soyutlama Katmanı oluşturma Azure IOT C SDK'yı özellikle taşıma. Microsoft genişletilmiş destek sağlamak üzere iş ortakları ile çalışmaktadır. İş ortaklarının listesini açın diğer platformlar ve PAL koruma C SDK'sı unity'nin.

| İş ortağı             | Cihazlar                            | Bağlantı                     | Destek |
|---------------------|------------------------------------|--------------------------|---------|
| Espressif           | ESP32 <br/> ESP8266                              | [Esp-azure](https://github.com/espressif/esp-azure)                | [GitHub](https://github.com/espressif/esp-azure)  
| Qualcomm            | Qualcomm MDM9206 LTE IOT Modem     | [Qualcomm LTE IOT SDK'sı](https://developer.qualcomm.com/software/lte-iot-sdk) | [Forum](https://developer.qualcomm.com/forums/software/lte-iot-sdk)   |
| ST Microelectronics | STM32L4 serisi <br/> STM32F4 serisi <br/>  STM32F7 serisi <br/>  IOT düğümü STM32L4 bulma Seti    | [X KÜP BULUT](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-cloud.html) <br/> [X-KÜP AZURE'A](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32cube-expansion-packages/x-cube-azure.html) <br/> [P-NUCLEO AZURE'A](https://www.st.com/content/st_com/en/products/evaluation-tools/solution-evaluation-tools/communication-and-connectivity-solution-eval-boards/p-nucleo-azure1.html) <br/> [FP CLD AZURE](https://www.st.com/content/st_com/en/products/embedded-software/mcus-embedded-software/stm32-embedded-software/stm32-ode-function-pack-sw/fp-cld-azure1.html)            | [Destek](https://www.st.com/content/st_com/en/support/support-home.html)
| Texas Instruments   | CC3220SF Başlatma çubuğu <br/> CC3220S Başlatma çubuğu <br/> MSP432E4 Başlatma çubuğu      | [Azure IOT eklentisi SimpleLink için](https://github.com/TexasInstruments/azure-iot-pal-simplelink) | [Za E2E Forumu](https://e2e.ti.com) <br/> [CC3220 Za E2E Forumu](https://e2e.ti.com/support/wireless_connectivity/simplelink_wifi_cc31xx_cc32xx/) <br/> [MSP432E4 Za E2E Forumu](https://e2e.ti.com/support/microcontrollers/msp430/) |

## <a name="next-steps"></a>Sonraki adımlar

* [Cihaz ve hizmet SDK’ları](iot-hub-devguide-sdks.md)
* [Taşıma Kılavuzu](https://github.com/Azure/azure-c-shared-utility/blob/master/devdoc/porting_guide.md)
