---
title: Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı kanıtlama mekanizmaları kullanma
description: Azure ile Nasıl Yapılır? - Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı kanıtlama mekanizmaları kullanma
author: yzhong94
ms.author: yizhon
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.custom: mvc
ms.openlocfilehash: af59ccc6d14dce49d06e178aac3ecafc29bd982c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61248139"
---
# <a name="how-to-use-different-attestation-mechanisms-with-device-provisioning-service-client-sdk-for-c"></a>Azure’da Cihaz Sağlama Hizmeti İstemci SDK'sı ile farklı kanıtlama mekanizmaları kullanma

Bu makalede, C için Cihaz Sağlama Hizmeti İstemci SDK’sı ile farklı [kanıtlama mekanizmalarının](concepts-security.md#attestation-mechanism) nasıl kullanılacağı gösterilmektedir. Fiziksel bir cihaz veya simülatör kullanabilirsiniz. Sağlama hizmeti, iki tür kanıtlama mekanizması için kimlik doğrulamasını destekler: X.509 ve Güvenilir Platform Modülü (TPM).

## <a name="prerequisites"></a>Önkoşullar

Geliştirme ortamınızı [Simülasyon cihazı oluşturma ve sağlama](./quick-create-simulated-device.md) kılavuzundaki "Geliştirme ortamınızı hazırlama" başlıklı bölüme göre hazırlayın.

### <a name="choose-an-attestation-mechanism"></a>Kanıtlama mekanizması seçme

Cihaz üreticisi olarak öncelikle desteklenen türlerden birini temel alan bir kanıtlama mekanizması seçmeniz gerekir. Şu anda [C için Cihaz Sağlama Hizmeti istemci SDK’sı](https://github.com/Azure/azure-iot-sdk-c/tree/master/provisioning_client), aşağıdaki kanıtlama mekanizmaları için destek sağlar: 

- [Güvenilir Platform Modülü (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module): TPM birkaç Linux/Ubuntu tabanlı cihazların yanı sıra, çoğu Windows tabanlı cihaz platformu için yerleşik bir standarttır. Cihaz üreticisi olarak, cihazlarınızda bu işletim sistemlerinden herhangi biri yüklüyse ve belirlenmiş bir standart arıyorsanız bu kanıtlama mekanizmasını seçebilirsiniz. TPM yongaları ile her bir cihazı ayrı olarak Cihaz Sağlama Hizmetine kaydedebilirsiniz. Geliştirme amacıyla, Windows veya Linux geliştirme makinenizde TPM simülatörünü kullanabilirsiniz.

- [X.509](https://cryptography.io/en/latest/x509/): X.509 sertifikaları adlı nispeten daha yeni yongalarda depolanabilir [donanım güvenlik modülleri (HSM)](concepts-security.md#hardware-security-module). İş aynı zamanda Microsoft bünyesinde, X.509 sertifikalarını uygulayan RIoT veya DICE yongalarında da devam eder. X.509 yongalarıyla, portalda toplu cihaz kaydı gerçekleştirebilirsiniz. Bu, embedOS gibi Windows olmayan belirli işletim sistemlerini de destekler. Geliştirme amacıyla Cihaz Sağlama Hizmeti istemci SDK’sı, X.509 cihaz simülatörünü destekler. 

Daha fazla bilgi için IoT Hub Cihazı Sağlama Hizmeti [güvenlik kavramları](concepts-security.md) ve [otomatik sağlama kavramları](/azure/iot-dps/concepts-auto-provisioning) bölümlerine bakın.

## <a name="enable-authentication-for-supported-attestation-mechanisms"></a>Desteklenen kanıtlama mekanizmaları için kimlik doğrulamasını etkinleştirme

Fiziksel cihazın veya simülatörün Azure portalına kaydedilebilmesi için SDK kimlik doğrulaması modu (X **.** 509 veya TPM) etkinleştirilmelidir. İlk olarak azure-iot-sdk-c kök klasörüne gidin. Ardından, seçtiğiniz kimlik doğrulaması moduna bağlı olarak belirtilen komutu çalıştırın:

### <a name="use-x509-with-simulator"></a>Simülatör ile X **.** 509 kullanma

Sağlama hizmeti, cihaz kimliğini doğrulamak için bir X **.** 509 sertifikası oluşturan Cihaz Kimliği Bileşim Motoru (DICE) öykünücüsü ile birlikte gelir. X **.** 509 kimlik doğrulamasını etkinleştirmek için aşağıdaki komutu çalıştırın: 

```
cmake -Ddps_auth_type=x509 ..
```

DICE özellikli donanım hakkında bilgiler [burada](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) bulunabilir.

### <a name="use-x509-with-hardware"></a>Donanım ile X **.** 509 kullanma

Sağlama hizmeti X **.** 509 ile başka donanımlarda kullanılabilir. Bağlantı kurmak için donanım ile SDK arasında bir arabirim bulunması gerekir. Arabirim hakkında bilgi almak için HSM üreticinizle iletişime geçin.

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

### <a name="x509"></a>X **.** 509

1. Bir cihazı sağlama hizmetine kaydetmek için her cihazın İstemci SDK'sı ile verilen Sağlama Aracı'nda görüntülenen Onay Anahtarı ve Kayıt Kimliği bilgilerini not etmeniz gerekir. Kök CA sertifikasını (kayıt grupları için) ve yaprak sertifikayı (tek kayıt için) yazdırmak için aşağıdaki komutu çalıştırın:
      ```
      ./azure-iot-sdk-c/dps_client/tools/x509_device_provision/x509_device_provision.exe
      ```
2. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar** düğmesine tıklayın ve Cihaz Sağlama hizmetinizi açın.
   - X **.** 509 tek kayıt: Sağlama hizmeti Özet dikey penceresinde, seçin **kayıtları Yönet**. **Tek Tek Kayıtlar** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik kanıtlama *Mekanizması* olarak **X**.**509** seçeneğini belirleyin ve dikey pencerede istenen yaprak sertifikayı karşıya yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 
   - X **.** 509 Grup kaydı: Sağlama hizmeti Özet dikey penceresinde, seçin **kayıtları Yönet**. **Grup Kayıtları** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. Kimlik kanıtlama *Mekanizması* olarak **X**.**509** seçeneğini belirleyin, bir grup adı ve sertifika adı girip dikey pencerede istenen CA sertifikasını/ara sertifikayı karşıya yükleyin. Tamamlandığında **Kaydet** düğmesine tıklayın. 

## <a name="enable-authentication-for-devices-using-a-custom-attestation-mechanism-optional"></a>Özel bir kanıtlama mekanizması kullanarak cihazlar için kimlik doğrulamasını etkinleştirme (isteğe bağlı)

> [!NOTE]
> Bu bölüm yalnızca şu anda Cihaz Sağlama Hizmeti İstemci SDK'sı tarafından desteklenmeyen özel bir platforma veya kanıtlama mekanizmalarına yönelik destek gerektiren cihazlar için geçerlidir. Ayrıca SDK’nın “kanıtlama mekanizması” yerine genel bir ifade olarak sıklıkla "HSM" terimini kullandığını da unutmayın.

Özel kanıtlama mekanizmanız için önce bir depo ve kitaplık geliştirmeniz gerekir:

1. Kanıtlama mekanizmanıza erişmek için bir kitaplık geliştirin. Bu projenin, kullanılacak Cihaz Sağlama SDK’sı için statik kitaplık üretmesi gerekir.

2. Kitaplığınızda, aşağıdaki üst bilgi dosyasında tanımlanan işlevleri uygulayın: 

    - Özel TPM için: [HSM TPM API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-tpm-api) bölümünde tanımlanan işlevleri uygulayın.  
    - Özel X.509 için: [HSM X509 API](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_custom_hsm.md#hsm-x509-api) bölümünde tanımlanan işlevleri uygulayın. 

Kitaplığınız başarıyla oluşturulduktan sonra, kitaplığınıza yönelik bir bağlantı oluşturarak Cihaz Sağlama Hizmeti İstemci SDK’sı ile bunu tümleştirmeniz gerekir. :

1. Aşağıdaki `cmake` komutunda, özel GitHub deposunu ve kitaplığı sağlayın:
    ```cmd/sh
    cmake -Duse_prov_client:BOOL=ON -Dhsm_custom_lib=<path_and_name_of_library> <PATH_TO_AZURE_IOT_SDK>
    ```
   
2. CMake (`\azure-iot-sdk-c\cmake\azure_iot_sdks.sln`) tarafından derlenen Visual Studio çözüm dosyasını açıp derleyin. 

    - Derleme işlemi, SDK kitaplığını derler.
    - SDK, `cmake` komutunda tanımlanan özel kitaplığa yönelik bir bağlantı oluşturmaya çalışır.

3. Özel kanıtlama mekanizmanızın doğru şekilde uygulandığını doğrulamak için "Provision_Samples" bölümünde (`\azure-iot-sdk-c\cmake\provisioning_client\samples\prov_dev_client_ll_sample` bölümünde) "prov_dev_client_ll_sample" örnek uygulamasını çalıştırın.

## <a name="connecting-to-iot-hub-after-provisioning"></a>Sağlama sonrasında IoT Hub'a bağlanma

Cihaz, sağlama hizmetiyle sağlandıktan sonra bu API, IoT Hub ile bağlantı kurmak için belirtilen kimlik doğrulaması modunu (X **.** 509 veya TPM) kullanır: 
  ```
  IOTHUB_CLIENT_LL_HANDLE handle = IoTHubClient_LL_CreateFromDeviceAuth(iothub_uri, device_id, iothub_transport);
  ```
