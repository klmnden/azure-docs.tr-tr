---
title: "Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı Donanım Güvenlik Modülü kullanma | Microsoft Docs"
description: "Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı Donanım Güvenlik Modülü kullanma"
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 08/28/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: 
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 184bbdc0a6bef74d0e5ac79afe3858354c6b1695
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk"></a>Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı Donanım Güvenlik Modülleri kullanma
Bu adımlar C içinde Cihaz Sağlama Hizmeti İstemci SDK'sı ile fiziksel cihaz ve simülatör kullanan farklı [Donanım Güvenlik Modülü (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) kullanımını göstermektedir.  Sağlama hizmeti iki kimlik doğrulaması modunu destekler: X**.**509 ve Güvenilir Platform Modülü (TPM).

## <a name="prerequisites"></a>Ön koşullar

Geliştirme ortamınızı [Sanal cihaz oluşturma ve sağlama] (./quick-create-simulated-device.md) kılavuzundaki "Geliştirme ortamınızı hazırlama" bölümüne göre hazırlayın.

## <a name="enable-authentication-with-different-hsms"></a>Kimlik doğrulamasını farklı HSM'lerle etkinleştirme

Fiziksel cihazın veya simülatörün Azure Portal'a kaydedilmesi için kimlik doğrulaması modu (X**.**509 veya TPM) etkinleştirilmelidir.  azure-iot-sdk-c kök klasörüne gidin.  Seçtiğiniz kimlik doğrulaması moduna bağlı olarak belirtilen komutu çalıştırın.

### <a name="use-x509-with-simulator"></a>Simülatör ile X**.**509 kullanma

Sağlama hizmeti, cihaz kimliğini doğrulamak için bir X**.**509 sertifikası oluşturan Cihaz Kimliği Bileşim Motoru öykünücüsü ile birlikte gelir.  X**.**509 kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın:

```
cmake -Ddps_auth_type=x509 ..
```

DICE özellikli donanım hakkında bilgiler [burada](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) bulunabilir.

### <a name="use-x509-with-hardware"></a>Donanım ile X**.**509 kullanma

Sağlama hizmeti X**.**509 ile başka donanımlarda kullanılabilir.  Bağlantı kurmak için donanım ile SDK arasında bir arabirim bulunması gerekir.  Arabirim hakkında bilgi almak için HSM üreticinizle iletişime geçin.

### <a name="use-tpm"></a>TPM kullanma

Sağlama hizmeti SAS Belirteci ile Windows ve Linux donanımı TPM yongalarına bağlanabilir.  TPM kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Simülatör ile TPM kullanma

TPM yongasına sahip bir cihazınız yoksa Windows işletim sistemi üzerinde geliştirme yapma amacıyla bir simülatör kullanabilirsiniz.  TPM kimlik doğrulamasını etkinleştirmek ve TPM simülatörünü çalıştırmak için aşağıdaki komutu çalıştırın:

```
cmake -Ddps_auth_type=tpm_simulator ..
```

## <a name="build-the-sdk"></a>SDK'yı derleme
Cihaz kaydını oluşturmadan önce SDK'yı derleyin.

### <a name="linux"></a>Linux
- SDK'yı Linux'ta derlemek için:
    ```
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    cmake ..
    cmake --build .  # append '-- -j <n>' to run <n> jobs in parallel
    ```
- Hata ayıklama ikililerini derlemek için proje oluşturma komutuna ilgili CMake seçeneğini ekleyin, örneğin:
    ```
    cmake -DCMAKE_BUILD_TYPE=Debug ..
    ```

- SDK'yı derlemek için kullanabileceğiniz birçok [CMake yapılandırma seçeneği](https://cmake.org/cmake/help/v3.6/manual/cmake.1.html) mevcuttur. Örneğin CMake proje oluşturma komutuna bir bağımsız değişken ekleyerek kullanılabilir protokol yığınlarının birini devre dışı bırakabilirsiniz:
    ```
    cmake -Duse_amqp=OFF ..
    ```
- Ayrıca birim testi derleyebilir ve çalıştırabilirsiniz:
    ```
    cmake -Drun_unittests=ON ..
    cmake --build .
    ctest -C "debug" -V
    ```

### <a name="windows"></a>Windows
- SDK'yı Windows'da derlemek için aşağıdaki adımları uygulayarak proje dosyalarını oluşturun:
    - "VS2015 için Geliştirici Komut İstemi" açın
    - Depo kök dizininde aşağıdaki CMake komutlarını çalıştırın:
      ```
      cd azure-iot-sdk-c
      mkdir cmake
      cd cmake
      cmake -G "Visual Studio 14 2015" ..
      ```
    Bu komut x86 kitaplıklarını derler. x64 için derlemek istiyorsanız cmake oluşturucu bağımsız değişkenini değiştirin: 
    ```
    cmake .. -G "Visual Studio 14 2015 Win64"
    ```

- Proje oluşturma işlemi başarıyla sonuçlanırsa `cmake` klasöründe bir Visual Studio çözüm dosyası (.sln) oluşturulur. SDK'yı derlemek için:
    - Visual Studio'da **cmake\azure_iot_sdks.sln** dosyasını açın ve derleyin **VEYA**
    - Proje dosyalarını oluşturmak için kullandığınız komut isteminde aşağıdaki komutu çalıştırın:
      ```
      cmake --build . -- /m /p:Configuration=Release
      ```
- Hata Ayıklama ikililerini derlemek için ilgili MSBuild bağımsız değişkenini kullanın: 
    ```
    cmake --build . -- /m /p:Configuration=Debug`
    ```
- SDK'yı derlemek için kullanabileceğiniz birçok CMake yapılandırma seçeneği mevcuttur. Örneğin CMake proje oluşturma komutuna bir bağımsız değişken ekleyerek kullanılabilir protokol yığınlarının birini devre dışı bırakabilirsiniz:
    ```
    cmake -G "Visual Studio 14 2015" -Duse_amqp=OFF ..
    ```
- Ayrıca birim testleri derleyebilir ve çalıştırabilirsiniz:
    ```
    cmake -G "Visual Studio 14 2015" -Drun_unittests=ON ..
    cmake --build . -- /m /p:Configuration=Debug
    ctest -C "debug" -V
    ```
  
### <a name="libraries-to-include"></a>Eklenecek kitaplıklar
- Bu kitaplıklar SDK'nıza eklenmelidir:
    - Sağlama hizmeti: dps_http_transport, dps_client, dps_security_client
    - IoTHub Güvenliği: iothub_security_client

## <a name="create-a-device-enrollment-entry-in-dps"></a>DPS'de bir cihaz kaydı girişi oluşturma

### <a name="tpm"></a>TPM
TPM kullanıyorsanız ["IoT Hub Cihazı Sağlama Hizmetini kullanarak sanal cihaz oluşturma ve sağlama"](./quick-create-simulated-device.md) talimatlarını uygulayarak DPS'de bir cihaz kaydı girişi oluşturun ve ilk önyüklemeyi simüle edin.

### <a name="x509"></a>X**.**509
1. Bir cihazı sağlama hizmetine kaydetmek için her cihazın İstemci SDK'sı ile verilen Sağlama Aracı'nda görüntülenen Onay Anahtarı ve Kayıt Kimliği bilgilerini not etmeniz gerekir. Kök CA sertifikasını (kayıt grupları için) ve imzalayan sertifikasını (tek kayıt için) yazdırmak için aşağıdaki komutu çalıştırın:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Azure Portal'da oturum açın, sol taraftaki menüden **Tüm kaynaklar**'a tıklayın ve DPS hizmetinizi açın.
   - X**.**509 Tek Kayıt: Sağlama hizmeti özeti dikey penceresinde **Kayıtları yönet**'i seçin. **Tek Tek Kayıtlar** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik onayı *Mekanizması* olarak **X**.**509** seçin ve dikey pencerede istenen imzalayan sertifikasını yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 
   - X**.**509 Grup Kaydı: Sağlama hizmeti özeti dikey penceresinde **Kayıtları yönet**'i seçin. **Grup Kayıtları** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik onayı *Mekanizması* olarak **X**.**509** seçin, grup adını ve sertifika adını girin ve dikey pencerede istenen kök CA sertifikasını yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 

## <a name="connecting-to-iot-hub-after-provisioning"></a>Sağlama sonrasında IoT Hub'a bağlanma

Cihaz sağlama hizmetiyle sağlandıktan sonra bu API, HSM kimlik doğrulaması modunu kullanarak IoT Hub ile bağlantı kurar: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```
