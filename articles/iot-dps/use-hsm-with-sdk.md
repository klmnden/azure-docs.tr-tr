---
title: Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı Donanım Güvenlik Modülü kullanma
description: Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı Donanım Güvenlik Modülü kullanma
services: iot-dps
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 03/28/2018
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: ''
manager: ''
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 0d392f4a8d935cb37b6f4cfcd69826de58b33880
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="how-to-use-different-hardware-security-modules-with-device-provisioning-service-client-sdk-for-c"></a>C için Cihaz Sağlama Hizmeti İstemci SDK’sı ile farklı Donanım Güvenlik Modülleri kullanma

Bu makalede, C için Cihaz Sağlama Hizmeti İstemci SDK’sı ile farklı [Donanım Güvenlik Modülleri’nin (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) nasıl kullanılacağı gösterilmektedir. Fiziksel bir cihaz veya simülatör kullanabilirsiniz. Sağlama hizmeti, iki tür kanıtlama mekanizması için kimlik doğrulamasını destekler: X**.**509 ve Güvenilir Platform Modülü (TPM).

## <a name="prerequisites"></a>Ön koşullar

Geliştirme ortamınızı [Simülasyon cihazı oluşturma ve sağlama](./quick-create-simulated-device.md) kılavuzundaki "Geliştirme ortamınızı hazırlama" başlıklı bölüme göre hazırlayın.

### <a name="choose-a-hardware-security-module"></a>Donanım Güvenlik Modülü Seçme

Cihaz üreticisi olarak ilk önce desteklenen türlerden birini temel alan Donanım Güvenlik Modüllerini (HSM) seçmeniz gerekir. Şu anda [C için Cihaz Sağlama Hizmeti istemci SDK’sı](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client), aşağıdaki HSM’ler için destek sağlar: 

- [Güvenilir Platform Modülü (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module): TPM, çoğu Windows tabanlı cihaz platformu ve birkaç Linux/Ubuntu tabanlı cihaz için belirlenmiş bir standarttır. Cihaz üreticisi olarak, cihazlarınızda bu işletim sistemlerinden herhangi biri yüklüyse ve HSM’ler için belirlenmiş bir standart arıyorsanız bu HSM’yi seçebilirsiniz. TPM yongaları ile her bir cihazı ayrı olarak Cihaz Sağlama Hizmetine kaydedebilirsiniz. Geliştirme amacıyla, Windows veya Linux geliştirme makinenizde TPM simülatörünü kullanabilirsiniz.

- [X.509](https://cryptography.io/en/latest/x509/): X.509 tabanlı HSM’ler nispeten daha yeni yongalardır. İş aynı zamanda Microsoft bünyesinde, X.509 sertifikalarını uygulayan RIoT veya DICE yongalarında da devam eder. X.509 yongalarıyla, portalda toplu cihaz kaydı gerçekleştirebilirsiniz. Bu, embedOS gibi Windows olmayan belirli işletim sistemlerini de destekler. Geliştirme amacıyla Cihaz Sağlama Hizmeti istemci SDK’sı, X.509 cihaz simülatörünü destekler. 

Daha fazla bilgi için bkz. [IoT Hub Cihazı Sağlama Hizmeti güvenlik kavramları](concepts-security.md). 

## <a name="enable-authentication-for-supported-hsms"></a>Desteklenen HSM’ler için kimlik doğrulamasını etkinleştirme

Fiziksel cihazın veya simülatörün Azure portalına kaydedilebilmesi için kimlik doğrulaması modu (X**.**509 veya TPM) etkinleştirilmelidir. İlk olarak azure-iot-sdk-c kök klasörüne gidin. Ardından, seçtiğiniz kimlik doğrulaması moduna bağlı olarak belirtilen komutu çalıştırın:

### <a name="use-x509-with-simulator"></a>Simülatör ile X**.**509 kullanma

Sağlama hizmeti, cihaz kimliğini doğrulamak için bir X**.**509 sertifikası oluşturan Cihaz Kimliği Bileşim Motoru (DICE) öykünücüsü ile birlikte gelir. X**.**509 kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın: 

```
cmake -Ddps_auth_type=x509 ..
```

DICE özellikli donanım hakkında bilgiler [burada](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) bulunabilir.

### <a name="use-x509-with-hardware"></a>Donanım ile X**.**509 kullanma

Sağlama hizmeti X**.**509 ile başka donanımlarda kullanılabilir. Bağlantı kurmak için donanım ile SDK arasında bir arabirim bulunması gerekir. Arabirim hakkında bilgi almak için HSM üreticinizle iletişime geçin.

### <a name="use-tpm"></a>TPM kullanma

Sağlama hizmeti SAS Belirteci ile Windows ve Linux donanımı TPM yongalarına bağlanabilir. TPM kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın:

```
cmake -Ddps_auth_type=tpm ..
```

### <a name="use-tpm-with-simulator"></a>Simülatör ile TPM kullanma

TPM yongasına sahip bir cihazınız yoksa Windows işletim sistemi üzerinde geliştirme yapma amacıyla bir simülatör kullanabilirsiniz. TPM kimlik doğrulamasını etkinleştirmek ve TPM simülatörünü çalıştırmak için aşağıdaki komutu çalıştırın:

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

## <a name="create-a-device-enrollment-entry-in-device-provisioning-services"></a>Cihaz Sağlama Hizmetleri’nde cihaz kayıt girişi oluşturma

### <a name="tpm"></a>TPM
TPM kullanıyorsanız [“IoT Hub Cihazı Sağlama Hizmetini kullanarak simülasyon cihazı oluşturma ve sağlama”](./quick-create-simulated-device.md) talimatlarını uygulayarak Cihaz Sağlama Hizmetinizde bir cihaz kaydı girişi oluşturun ve ilk önyüklemeyi simüle edin.

### <a name="x509"></a>X**.**509
1. Bir cihazı sağlama hizmetine kaydetmek için her cihazın İstemci SDK'sı ile verilen Sağlama Aracı'nda görüntülenen Onay Anahtarı ve Kayıt Kimliği bilgilerini not etmeniz gerekir. Kök CA sertifikasını (kayıt grupları için) ve imzalayan sertifikasını (tek kayıt için) yazdırmak için aşağıdaki komutu çalıştırın:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Azure portalında, sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve DPS hizmetinizi açın.
   - X**.**509 Tek Kayıt: Sağlama hizmeti özeti dikey penceresinde **Kayıtları yönet**'i seçin. **Tek Tek Kayıtlar** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik onayı *Mekanizması* olarak **X**.**509** seçin ve dikey pencerede istenen imzalayan sertifikasını yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 
   - X**.**509 Grup Kaydı: Sağlama hizmeti özeti dikey penceresinde **Kayıtları yönet**'i seçin. **Grup Kayıtları** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik onayı *Mekanizması* olarak **X**.**509** seçin, grup adını ve sertifika adını girin ve dikey pencerede istenen kök CA sertifikasını yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 

## <a name="enable-authentication-for-custom-tpm-and-x509-devices-optional"></a>Özel TPM ve X.509 cihazları için kimlik doğrulamasını etkinleştirme (isteğe bağlı)

> [!NOTE]
> Bu bölüm yalnızca özel platform veya HSM için destek gerektiren, şu anda C için Cihaz Sağlama Hizmeti İstemci SDK’sı tarafından desteklenmeyen cihazlar için geçerlidir.

İlk olarak, özel HSM deponuzu ve kitaplığınızı geliştirmeniz gerekir:

1. HSM’nize erişmek için bir kitaplık geliştirin. Bu projenin, kullanılacak Cihaz Sağlama SDK’sı için statik kitaplık üretmesi gerekir.

2. Kitaplığınızda, aşağıdaki üst bilgi dosyasında tanımlanan işlevleri uygulayın: 

    - Özel TPM için: [HSM TPM API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api) bölümünde tanımlanan Özel HSM işlevlerini uygulayın.  
    - Özel X.509 için: [HSM X509 API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api) bölümünde tanımlanan Özel HSM işlevlerini uygulayın. 

Kitaplığınız başarıyla derlendikten sonra, kitaplığınıza karşı bağlantı oluşturarak Cihaz Sağlama Hizmeti İstemci SDK’sı ile bunu tümleştirmeniz gerekir. :

1. Aşağıdaki `cmake` komutunda, özel HSM GitHub deposunu, kitaplık yolunu ve adını sağlayın:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
2. CMake (`\azure-iot-sdk-c\cmake\azure_iot_sdks.sln`) tarafından derlenen Visual Studio çözüm dosyasını açıp derleyin. 

    - Derleme işlemi, SDK kitaplığını derler.
    - SDK, `cmake` komutunda tanımlanan özel HSM’ye karşı bağlantı oluşturmaya çalışır.

3. HSM’nizin düzgün şekilde uygulandığını doğrulamak için, "Provision_Samples" bölümünde (`\azure-iot-sdk-c\cmake\provisioning_client\samples\prov_dev_client_ll_sample` bölümünde) "prov_dev_client_ll_sample" örnek uygulamasını çalıştırın.

## <a name="connecting-to-iot-hub-after-provisioning"></a>Sağlama sonrasında IoT Hub'a bağlanma

Cihaz sağlama hizmetiyle sağlandıktan sonra bu API, HSM kimlik doğrulaması modunu kullanarak IoT Hub ile bağlantı kurar: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```

