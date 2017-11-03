---
title: "Azure IOT Hub cihaz sağlama hizmeti için aygıtı ayarlama | Microsoft Docs"
description: "Cihaz IOT Hub cihaz sağlama hizmeti aracılığıyla sağlama işlemini üretim aygıt sırasında ayarlama"
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c99279413b50e7bf1e6058a4151890e3a8f83892
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Azure IOT Hub cihaz sağlama hizmeti ile sağlamak için bir aygıtı ayarlama

Önceki öğreticide cihazlarınızı IOT hub'ınıza otomatik olarak sağlamak için Azure IOT Hub cihaz sağlama hizmeti oluşturan ayarlamak öğrendiniz. Temel cihazınız için aygıt hizmeti sağlama yapılandırabilmeniz Bu öğretici Cihazınızı ayarlama üretim süreci sırasında yönelik yönergeler sağlanmaktadır kendi [donanım güvenlik modülü (HSM)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security), ve aygıt ilk kez önyükleme yaptığında, cihaz sağlamayı hizmetine bağlanın. Bu öğretici için işlemleri açıklanır:

> [!div class="checklist"]
> * Bir donanım güvenlik modülü seçin
> * Cihaz sağlama istemci SDK'sı için seçilen HSM derleme
> * Güvenlik yapıları Ayıkla
> * Aygıttaki aygıt sağlama hizmet yapılandırmasını ayarlama

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce aygıtı sağlama hizmeti örneğinizi ve öğreticide belirtilen yönergeleri kullanarak bir IOT hub oluşturma [cihaz sağlama bulut ayarlama](./tutorial-set-up-cloud.md).


## <a name="select-a-hardware-security-module"></a>Bir donanım güvenlik modülü seçin

[Aygıtı sağlama hizmeti istemci SDK](https://github.com/Azure/azure-iot-sdk-c/tree/master/dps_client) veya donanım güvenlik modülleri (HSM'ler) iki türleri için destek sağlar: 

- [Güvenilir Platform Modülü (TPM)](https://en.wikipedia.org/wiki/Trusted_Platform_Module).
    - TPM, birkaç Linux/Ubuntu tabanlı aygıtların yanı sıra, çoğu Windows tabanlı cihaz platformları için kurulmuş bir standarttır. Bir aygıt üreticisi ya da bu işletim sistemleri çalıştıran, cihazlarda varsa ve HSM'ler için belirli bir standardı için arıyorsanız bu HSM tercih edebilirsiniz. TPM yongaları ile yalnızca aygıt hizmeti sağlama ayrı ayrı her cihazı kaydedebilir. Geliştirme amaçlarıyla Windows veya Linux geliştirme makinenizde TPM simulator kullanabilirsiniz.

- [X.509](https://cryptography.io/en/latest/x509/) donanım güvenlik modülleri tabanlı. 
    - X.509 tabanlı HSM'ler içinde Microsoft X.509 sertifikalarını uygulayan RIoT veya BÖLMEK yongaları üzerinde şu anda İleri aşamalara iş görece daha yeni yongaları şunlardır. X.509 yongaları ile yapabileceğiniz toplu kayıt Portalı'nda. Ayrıca, belirli olmayan - Windows işletim sistemleri embedOS gibi destekler. Geliştirme amaçla aygıtı sağlama hizmeti istemci SDK, bir X.509 aygıt benzeticisi destekler. 

Bir aygıt üreticisi donanım güvenlik modülleri/yukarıdaki türlerine birini temel alan yongaları seçmeniz gerekir. HSM'ler diğer türleri aygıtı sağlama hizmeti istemci SDK'sı şu anda desteklenmemektedir.   


## <a name="build-device-provisioning-client-sdk-for-the-selected-hsm"></a>Cihaz sağlama istemci SDK'sı için seçilen HSM derleme

Cihaz sağlama hizmeti istemci SDK'sı seçilen güvenlik mekanizması yazılımda uygulaması yardımcı olur. Aşağıdaki adımlar için seçilen HSM yonga SDK kullanmayı gösterir:

1. İzlediyseniz [sanal cihaz oluşturmak için Hızlı Başlangıç](./quick-create-simulated-device.md), Kurulum SDK'yı oluşturmak hazır olması. Aksi durumda, ilk dört başlıklı bölümündeki adımları [geliştirme ortamını hazırlayın](./quick-create-simulated-device.md#setupdevbox). Bu adımları cihaz sağlama hizmeti istemci SDK'sı için GitHub depoyu kopyalama yanı sıra yükleme `cmake` aracı oluşturun. 

1. Cihazınız için komut isteminde aşağıdaki komutlardan birini kullanarak seçtiğiniz HSM türü için SDK oluşturun:
    - TPM aygıtları için:
        ```cmd/sh
        cmake -Ddps_auth_type=tpm ..
        ```

    - TPM simülatörü:
        ```cmd/sh
        cmake -Ddps_auth_type=tpm_simulator ..
        ```

    - X.509 cihazlar ve simulator için:
        ```cmd/sh
        cmake -Ddps_auth_type=x509 ..
        ```

1. SDK, TPM ve X.509 HSM'ler Windows veya Ubuntu uygulamaları çalıştıran cihazlar için varsayılan destek sağlar. Bu HSM'ler desteklenen için başlıklı bölüme geçin [güvenlik yapıları ayıklamak](#extractsecurity) aşağıda. 
 
## <a name="support-custom-tpm-and-x509-devices"></a>Özel TPM ve X.509 cihazları destekler

Cihaz sağlama sistem istemci SDK'sı, Windows ya da Ubuntu çalıştırmayan tüm TPM ve X.509 cihazlar için varsayılan destek sağlamaz. Bu tür cihazlar için belirli HSM yonga için özel kod yazmanıza izin aşağıdaki adımlarda gösterildiği gibi gerekir:

### <a name="develop-your-custom-repository"></a>Özel deponuz geliştirin

1. HSM erişmek için GitHub deposunu geliştirin. Cihaz sağlama kullanmak SDK'sı statik kitaplık üretmek bu proje gerekir.
1. Kitaplığınızı aşağıdaki üstbilgi dosyasında tanımlanan işlevler uygulamanız gerekir: bir. Özel TPM için tanımlanan işlevlerini gerçekleştirmek `\azure-iot-sdk-c\dps_client\adapters\custom_hsm_tpm_impl.h`.
    b. Özel X.509 için tanımlanan işlevlerini gerçekleştirmek `\azure-iot-sdk-c\dps_client\adapters\custom_hsm_x509_impl.h`. 
1. HSM deponuz da içermelidir bir `CMakeLists.txt` oluşturulmalıdır depo kökündeki dosya.

### <a name="integrate-with-the-device-provisioning-service-client"></a>Cihaz hizmeti istemcisi sağlama ile tümleştirme

Kitaplığınızı başarıyla kendi üzerine kurulur sonra Iothub C-SDK taşıyın ve deponuz içinde çekme:

1. Özel HSM GitHub deposunu, kitaplık yolu ve adı aşağıdaki cmake komutta sağlayın:
    ```cmd/sh
    cmake -Ddps_auth_type=<custom_hsm> -Ddps_hsm_custom_repo=<github_repo_name> -Ddps_hsm_custom_lib=<path_and_name_of library> <PATH_TO_AZURE_IOT_SDK>
    ```
   Değiştir `<custom_hsm>` ya da bu komutla içinde `tpm` veya `x509`. Bu komut, içinde özel HSM deponuz için bir işaretçi oluşturur `cmake` dizin. Özel HSM TPM veya X.509 güvenlik mekanizmalarına hala dayanması gerektiğini unutmayın.

1. Visual studio SDK'yı açmak ve onu oluşturun. 

    - Derleme işlemi özel depo klonlar ve kitaplığı oluşturur.
    - SDK cmake komut içinde tanımlanmış özel HSM karşı bağlantı dener.

1. Çalıştırma `\azure-iot-sdk-c\dps_client\samples\dps_client_sample\dps_client_sample.c` , HSM doğru uygulanmışsa doğrulamak için örnek.

<a id="extractsecurity"></a>
## <a name="extract-the-security-artifacts"></a>Güvenlik yapıları Ayıkla

HSM aygıtınızda için güvenlik yapıları ayıklamak için sonraki adımdır bakın.

1. TPM aygıt için öğrenmek gerekecek **onay anahtarını** TPM yongası üreticisinden ilişkili. Benzersiz bir türetilemeyeceğini **kayıt kimliği** onay anahtarını karma hale getirerek TPM cihazınız için. 
2. Bir X.509 aygıt için aygıtlarınızın - aygıtların grup kayıtları için kök sertifikalarını sırasında tek tek cihaz kayıtları için son varlık sertifikaları için verilen sertifikaların edinmeniz gerekir.

Bu güvenlik yapıtların cihazlarınıza cihaz hizmeti sağlama kaydetmek için gereklidir. Sağlama hizmeti sonra herhangi bir önyükleme ve daha sonra herhangi bir noktada ile zamanında bağlamak için bu cihazların bekler. Bkz: [cihaz kayıtlarını yönetme](how-to-manage-enrollments.md) bu güvenlik yapıtların kayıtları oluşturmak için nasıl kullanılacağı hakkında bilgi için. 

Cihazınızı ilk kez önyükleme yaptığında, istemci SDK güvenlik yapıları aygıttan ayıklamak için yonga etkileşimde ve cihaz sağlamayı hizmetinizle kayıt doğrular. 


## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Aygıttaki aygıt sağlama hizmet yapılandırmasını ayarlama

İşlem üretim aygıt son adımda, cihaz hizmete kaydolmak için cihaz sağlama hizmeti istemci SDK kullanan bir uygulamayı yazmaktır. Bu SDK kullanmak, uygulamalarınız için aşağıdaki API'ler sağlar:

```C
typedef void(*DPS_REGISTER_DEVICE_CALLBACK)(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* user_context); // Callback to notify user of device registration results.
DPS_CLIENT_LL_HANDLE DPS_Client_LL_Create (const char* dps_uri, const char* scope_id, DPS_TRANSPORT_PROVIDER_FUNCTION protocol, DPS_CLIENT_ON_ERROR_CALLBACK on_error_callback, void* user_ctx); // Creates the IOTHUB_DPS_LL_HANDLE to be used in subsequent calls.
void DPS_Client_LL_Destroy(DPS_CLIENT_LL_HANDLE handle); // Frees any resources created by the IoTHub Device Provisioning Service module.
DPS_RESULT DPS_LL_Register_Device(DPS_LL_HANDLE handle, DPS_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, DPS_CLIENT_REGISTER_STATUS_CALLBACK status_cb, void* status_ctx); // Registers a device that has been previously registered with Device Provisioning Service
void DPS_Client_LL_DoWork(DPS_LL_HANDLE handle); // Processes the communications with the Device Provisioning Service and calls any user callbacks that are required.
```

Değişkenleri başlatmayı unutmayın `dps_uri` ve `dps_scope_id` bölümünde belirtildiği gibi [benzetim Bu hızlı başlangıç aygıt bölümünün ilk önyükleme sırası](./quick-create-simulated-device.md#firstbootsequence), kullanmadan önce. Bir cihaz sağlamayı istemci kaydı için API `DPS_Client_LL_Create` genel cihaz sağlama Hizmeti'ne bağlanır. *Kimliği kapsam* hizmeti tarafından oluşturulan ve benzersizliği garanti eder. Bu sabit ve kayıt kimlikleri benzersiz şekilde tanımlamak için kullanılan olur. `iothub_uri` IOT Hub istemci kaydı API sağlayan `IoTHubClient_LL_CreateFromDeviceAuth` sağ IOT hub ile bağlanmak için. 


Bu API'leri bağlanmak ve bu önyüklendiğinde cihaz sağlama hizmeti ile kaydetme, IOT hub'ınız hakkında bilgi alın ve buna bağlanmak için Cihazınızı yardımcı olur. Dosya `dps_client/samples/dps_client_sample/dps_client_sample.c` bu API'leri kullanmayı gösterir. Genel olarak, istemci kaydı için aşağıdaki çerçevesi oluşturmanız gerekir:

```C
static const char* dps_uri = "global.azure-devices-provisioning.net";
static const char* dps_scope_id = "[ID scope for your provisioning service]";
...
static void register_callback(DPS_RESULT register_result, const char* iothub_uri, const char* device_id, void* context)
{
    USER_DEFINED_INFO* user_info = (USER_DEFINED_INFO *)user_context;
    ...
    user_info. reg_complete = 1;
}
static void registation_status(DPS_REGISTRATION_STATUS reg_status, void* user_context)
{
}
int main()
{
    ...    
    security_device_init(); // initialize your HSM 

    DPS_CLIENT_LL_HANDLE handle = DPS_Client_LL_Create(dps_uri, dps_scope_id, dps_transport, on_dps_error_callback, &user_info); // Create your DPS client

    if (DPS_Client_LL_Register_Device(handle, register_callback, &user_info, register_status, &user_info) == IOTHUB_DPS_OK) {
        do {
            // The dps_register_callback is called when registration is complete or fails
            DPS_Client_LL_DoWork(handle);
        } while (user_info.reg_complete == 0);
    }
    DPS_Client_LL_Destroy(handle); // Clean up the DPS client
    ...
    iothub_client = IoTHubClient_LL_CreateFromDeviceAuth(user_info.iothub_uri, user_info.device_id, transport); // Create your IoT hub client and connect to your hub
    ...
}
```

Bir test hizmeti Kurulumu kullanarak ilk başta, bir sanal cihaz kullanarak cihaz sağlama hizmeti istemci kayıt uygulamanız İyileştir. Uygulamanızı test ortamında çalıştığından sonra belirli cihazınız için yapı ve cihaz görüntünüzü yürütülebilir kopyalayın. Cihaz başlatmayın henüz yapmanız [cihaz sağlama hizmeti ile cihaz kaydetme](./tutorial-provision-device-to-hub.md#enrolldevice) cihaz başlatmadan önce. Bu işlem öğrenmek için aşağıdaki sonraki adımlara bakın. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu noktada, cihaz sağlama ve IOT hub'ı Hizmetleri portalında ayarlamış olabilir. Cihaz Kurulumu sağlama bırakın ve/veya bu hizmetlerin herhangi birini kullanarak gecikme isterseniz, gereksiz maliyetlerin oluşmasını önlemek için kapatılıyor öneririz.

1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  
1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. **Tüm kaynaklar** dikey pencerenin en üstündeki **Sil** seçeneğine tıklayın.  


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir donanım güvenlik modülü seçin
> * Cihaz sağlama istemci SDK'sı için seçilen HSM derleme
> * Güvenlik yapıları Ayıkla
> * Aygıttaki aygıt sağlama hizmet yapılandırmasını ayarlama

Azure IOT Hub cihaz sağlama hizmeti için otomatik sağlama kaydederek cihaz IOT hub'ınıza sağlama konusunda bilgi almak için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Cihaz IOT hub'ınıza sağlama](tutorial-provision-device-to-hub.md)

