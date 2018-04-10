---
title: Azure IoT Hub Cihazı Sağlama Hizmeti için cihazı ayarlama
description: Cihaz üretim süreci sırasında IoT Hub Cihazı Sağlama Hizmeti aracılığıyla sağlanacak cihazı ayarlama
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c885e4d5d747d913eaf0b7137b240950e920e7ff
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Azure IoT Hub Cihazı Sağlama Hizmeti’ni kullanarak sağlanacak bir cihaz ayarlama

Önceki öğreticide, IoT hub’ınıza cihazlarınızı otomatik olarak sağlamak için Azure IoT Hub Cihazı Sağlama Hizmeti’nin nasıl ayarlanacağını öğrendiniz. Bu öğretici, cihazınızın IOT Hub ile otomatik olarak sağlanmasına imkan vermek için üretim işlemi boyunca cihazınızın ayarlanacağını gösterir. Cihazınız, [Kanıtlama mekanizmasına](concepts-device.md#attestation-mechanism) dayalı olarak, ilk önyüklemenin ve sağlama hizmetiyle bağlantının ardından sağlanır. Bu öğretici aşağıdakileri gerçekleştirmek için işlemleri açıklar:

> [!div class="checklist"]
> * Platforma özgü Cihaz Sağlama Hizmetleri İstemci SDK’sını derleme
> * Güvenlik yapılarını ayıklama
> * Cihaz kayıt yazılımı oluşturma

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce, önceki [1 - Bulut kaynaklarını ayarlama](./tutorial-set-up-cloud.md) öğreticisinde yer alan yönergeleri kullanarak Cihaz Sağlama Hizmeti örneğinizi ve bir IoT hub’ı oluşturun.

Bu öğretici, C için Cihaz Sağlama Hizmeti İstemci SDK’sını içeren [C deposu için Azure IoT SDK’ları ve kitaplıkları](https://github.com/Azure/azure-iot-sdk-c)’nı kullanır. SDK şu anda Windows veya Ubuntu uygulamalarında çalıştırılan cihazlar için TPM ve X.509 desteği sağlar. Bu öğretici, Visual Studio 2017 konusunda temel yetkinliğe sahip olunduğu varsayılarak bir Windows geliştirme istemcisi kullanımına dayanır. 

Otomatik sağlama işlemini bilmiyorsanız, devam etmeden önce [Otomatik sağlama kavramlarını](concepts-auto-provisioning.md) gözden geçirdiğinizden emin olun. 

## <a name="build-a-platform-specific-version-of-the-sdk"></a>SDK’nın platforma özgü sürümünü derleme

Cihaz Sağlama Hizmeti İstemci SDK’sı, cihaz kaydı yazılımınızı uygulamanıza yardımcı olur. Ancak bunu kullanabilmeniz için önce geliştirme istemci platformunuza ve kanıtlama mekanizmanıza özgü bir SDK sürümü derlemeniz gerekir. Bu öğreticide, desteklenen bir kanıtlama türü için, Windows geliştirme platformunda Visual Studio 2017’yi kullanan bir SDK derlersiniz:

1. Gerekli araçları yükleyin ve C için sağlama hizmeti İstemci SDK’sını içeren GitHub deposunu kopyalayın:

   a. Makinenizde Visual Studio 2015 veya [Visual Studio 2017](https://www.visualstudio.com/vs/) olduğundan emin olun. Visual Studio yüklemenizde ["C++ ile masaüstü geliştirme"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirilmiş olması gerekir.

   b. [CMake derleme sistemini](https://cmake.org/download/) indirin ve yükleyin. CMake yüklemesi **öncesinde** makinenize "C++ ile masaüstü geliştirme" iş yüküne sahip bir Visual Studio uygulamasının yüklenmiş olması önemlidir.

   c. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yerel Git deponuzla etkileşime geçmeye yönelik bir komut satırı Bash kabuğu olan **Git Bash** de dahil, en son `git` araçları için bkz. [Software Freedom Conservancy’nin Git istemci araçları](https://git-scm.com/download/). 

   d. Git Bash’i açın ve "C deposu için Azure IoT SDK’ları ve kitaplıklarını" kopyalayın. Kopyalama komutu, birçok bağımlı alt modülü de indirdiğinden, bu komutun tamamlanması birkaç dakika sürebilir:
    
   ```cmd/sh
   git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
   ```

   e. Yeni oluşturulan depo alt dizininin yeni bir `cmake` alt dizinini oluşturun:

   ```cmd/sh
   mkdir azure-iot-sdk-c/cmake
   ``` 

2. Git Bash komut isteminden azure-iot-sdk-c deposunun `cmake` alt dizini olarak değiştirin:

   ```cmd/sh
   cd azure-iot-sdk-c/cmake
   ```

3. Aşağıdaki komutlardan birini kullanarak (ayrıca sondaki iki nokta karakterine de dikkat ederek), desteklenen kanıtlama mekanizmalarından birini ve geliştirme platformunuz için SDK’yı derleyin. Tamamlandığında CMake, cihazınıza özgü içerikle `/cmake` alt dizinini derler:
    - Fiziksel bir TPM/HSM veya kanıtlama için sanal X.509 sertifikası kullanan cihazlar için:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Kanıtlama için TPM simülatörünü kullanan cihazlar için:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

Şimdi cihaz kayıt kodunuzu derlemek için SDK’yı kullanmaya hazırsınız. 
 
<a id="extractsecurity"></a> 

## <a name="extract-the-security-artifacts"></a>Güvenlik yapılarını ayıklama 

Bir sonraki adım, cihazınız tarafından kullanılan kanıtlama mekanizması için güvenlik yapılarını ayıklamaktır. 

### <a name="physical-device"></a>Fiziksel cihaz 

Fiziksel bir TPM/HSM’deki kanıtlamayı kullanmak için SDK’yı derlediyseniz:

- TPM cihazı için, TPM yongası üreticisinden bu TPM cihazıyla ilişkili **Onay Anahtarı**’nı belirlemeniz gerekir. Onay anahtarını karma hale getirerek TPM cihazınız için benzersiz bir **Kayıt Kimliği** türetebilirsiniz.  

- X.509 cihazı için, bir yandan cihazların grup kayıtları için kök sertifikalarını, diğer yandan cihazlarınıza düzenlenen sertifikaları (bireysel cihaz kayıtları için end-entity sertifikaları) almanız gerekir. 

### <a name="simulated-device"></a>Sanal cihaz

Sanal bir TPM veya X.509 sertifikasındaki kanıtlamayı kullanmak için SDK’yı derlerseniz:

- Sanal TPM cihazı için:
   1. Ayrı/yeni bir komut isteminde, `azure-iot-sdk-c` dizinine gidin ve TPM simülatörünü çalıştırın. 2321 ve 2322 bağlantı noktalarında bulunan bir yuva üzerinden dinler. Bu komut penceresini kapatmayın; aşağıdaki Hızlı Başlangıcın sonuna kadar bu simülatörü çalışır durumda tutmanız gerekir. 

      `azure-iot-sdk-c` alt dizininden aşağıdaki komutu çalıştırarak simülatörü başlatın:

      ```cmd/sh
      .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
      ```

   2. Visual Studio kullanarak, `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve "Derleme" menüsündeki "Çözümü derle" komutunu kullanarak bunu derleyin.

   3. Visual Studio'nın *Çözüm Gezgini* bölmesinde **Sağlama\_Araçlar** klasörüne gidin. **tpm_device_provision** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 

   4. "Hata Ayıklama" menüsündeki “Başlat” komutlarından birini kullanarak çözümü çalıştırın. Çıktı penceresi, cihaz kaydı için gereken TPM simülatörün **_Kayıt Kimliği_**’ni ve **_Kayıt Anahtarı_**’nı görüntüler. Daha sonra kullanmak için bu değerleri kopyalayın. Bu pencereyi (Kayıt Kimliği ve Onay Anahtarı ile) kapatabilirsiniz, ancak 1. adımda başlattığınız TPM simülatörü penceresini çalışır durumda bırakın.

- Sanal X.509 cihazı için:
  1. Visual Studio kullanarak, `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve "Derleme" menüsündeki "Çözümü derle" komutunu kullanarak bunu derleyin.

  2. Visual Studio'nın *Çözüm Gezgini* bölmesinde **Sağlama\_Araçlar** klasörüne gidin. **dice\_device\_enrollment** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. 
  
  3. "Hata Ayıklama" menüsündeki “Başlat” komutlarından birini kullanarak çözümü çalıştırın. Çıktı penceresinde, istendiğinde tek kayıt için **i** girin. Çıktı penceresi, sanal cihazınız için yerel olarak oluşturulmuş X.509 sertifikasını görüntüler. *-----BEGIN CERTIFICATE-----* ile başlayıp ilk *-----END PUBLIC KEY-----* ile biten çıktıyı panoya kopyalayın ve bu iki satırı da dahil ettiğinizden emin olun. Yalnızca çıktı penceresindeki ilk sertifikayı kopyalamanız gerektiğini unutmayın.
 
  4. **_X509testcert.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz metin düzenleyicide açın ve panonun içeriklerini bu dosyaya kopyalayın. Cihaz kaydı için daha sonra kullanacağınızdan, dosyayı kaydedin. Kayıt yazılımınız çalıştırıldığında, otomatik sağlamadaki aynı sertifikayı kullanır.    

Cihaz Sağlama Hizmeti’nde cihazınızın kaydı sırasında bu güvenlik yapıları gerekir. Sağlama hizmeti, cihazın önyüklenmesini ve daha sonra buna bağlanmasını bekler. Cihazınız ilk kez önyüklendiğinde, istemci SDK’sı mantığı, cihazdaki güvenlik yapılarını ayıklamak için yonganız (veya simülatör) ile etkileşime geçer ve Cihaz Sağlama hizmetinize kaydı doğrular. 

## <a name="create-the-device-registration-software"></a>Cihaz kayıt yazılımı oluşturma

Son adım, cihazı IoT Hub hizmetine kaydetmek için Cihaz Sağlama Hizmeti istemci SDK’sını kullanan bir kayıt uygulaması yazmaktır. 

> [!NOTE]
> Bu adım için, iş istasyonunuzdan SDK örnek kayıt uygulaması çalıştırılarak gerçekleştirilen bir sanal cihaz kullanıldığı varsayılacaktır. Ancak fiziksel cihaza dağıtım için bir kayıt uygulaması derliyorsanız aynı kavramlar geçerlidir. 

1. Azure portalında, Cihaz Sağlama hizmetiniz için **Genel Bakış** dikey penceresini seçin ve **_Kimlik Kapsamı_** değerini kopyalayın. *Kimlik Kapsamı* genellikle hizmet tarafından oluşturulur ve benzersizliği garanti eder. Bu sabittir ve kayıt kimliklerini benzersiz şekilde tanımlamak için kullanılır.

    ![Portal dikey penceresinden DPS uç nokta bilgilerini ayıklama](./media/tutorial-set-up-device/extract-dps-endpoints.png) 

2. Makinenizdeki Visual Studio *Çözüm Gezgini*'nde **Sağlama\_Örnekler** klasörüne gidin. **prov\_dev\_client\_sample** adlı örnek projeyi seçip **prov\_dev\_client\_sample.c** kaynak dosyasını açın.

3. 1. adımda elde edilen _Kimlik Kapsamı_ değerini `id_scope` değişkenine atayın (left/`[` ve right/`]` ayraçlarını kaldırarak): 

    ```c
    static const char* global_prov_uri = "global.azure-devices-provisioning.net";
    static const char* id_scope = "[ID Scope]";
    ```

    Başvuru için, `IoTHubClient_LL_CreateFromDeviceAuth` IoT Hub istemci kaydı API’sinin, belirlenen Cihaz Sağlama Hizmeti örneğine bağlanmasına olanak sağlayan `global_prov_uri` değişkeni.

4. Aynı dosyadaki **main()** işlevinde, cihazınızın kayıt yazılımı (TPM veya X.509) tarafından kullanılmakta olan kanıtlama mekanizmasıyla eşleşen `hsm_type` değişkenine açıklama ekleyin veya bu değişkenin açıklamasını kaldırın: 

    ```c
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. Değişikliklerinizi kaydedin ve "Derleme" menüsünden "Çözümü derle" seçeneğini belirleyerek **prov\_dev\_client\_sample** örneğini yeniden derleyin. 

6. **Sağlama\_Örnekler** klasörünün altında **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. Henüz örnek uygulamayı ÇALIŞTIRMAYIN.

> [!IMPORTANT]
> Henüz cihazı çalıştırmayın/başlatmayın! Cihazı başlatmadan önce ilk olarak Cihaz Sağlama Hizmeti’ne cihazı kaydederek işlemi tamamlamanız gerekir. Aşağıda yer alan Sonraki adımlar, sonraki makale için size yol gösterecektir.

### <a name="sdk-apis-used-during-registration-for-reference-only"></a>Kayıt sırasında kullanılan SDK API’leri (yalnızca başvuru)

Başvuru için SDK, kayıt sırasında kullanmak üzere uygulamanız için aşağıdaki API’leri sağlar. Bu API’ler, cihazınızın önyüklenirken Cihaz Sağlama Hizmetine bağlanmasına ve kaydolmasına yardımcı olur. Buna karşılık cihazınız, IoT Hub örneğinizle bağlantı kurmak için gerekli bilgileri alır:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service.  
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Ayrıca, ilk olarak bir sanal cihaz kullanarak Cihaz Sağlama Hizmeti istemci kaydı uygulamanızı iyileştirmeniz ve hizmet kurulumunu test etmeniz gerektiğini de fark edebilirsiniz. Uygulamanız test ortamında çalıştığında, cihazınız için uygulamayı derleyebilir ve yürütülebilir dosyayı cihazınızın görüntüsüne kopyalayabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada, Cihaz Sağlama ve IoT Hub hizmetleri portalda çalışıyor olabilir. Cihaz sağlama kurulumundan çıkmak ve/veya bu öğretici serisinin tamamlanmasını geciktirmek istiyorsanız, gereksiz yinelenen maliyetler oluşmasını önlemek için bunların kapatılmasını öneririz.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  
2. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Platforma özgü Cihaz Sağlama Hizmeti İstemci SDK’sını derleme
> * Güvenlik yapılarını ayıklama
> * Cihaz kayıt yazılımı oluşturma

Otomatik sağlama için cihazı Azure IoT Hub Cihazı Sağlama Hizmeti’ne kaydederek IoT hub’ına nasıl sağlanacağı hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [IoT hub’ınıza cihazı sağlama](tutorial-provision-device-to-hub.md)

