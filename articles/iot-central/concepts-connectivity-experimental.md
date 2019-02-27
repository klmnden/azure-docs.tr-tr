---
title: Cihaz bağlantısı, Azure IOT Central | Microsoft Docs
description: Bu makalede Azure IOT Central, cihaz bağlantısı ile ilgili temel kavramlar tanıtılmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: edffc6677609537d8a07aeae45a57c5e88449099
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882674"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Azure IOT Central, cihaz bağlantısı

Bu makalede, Microsoft Azure IOT Central, cihaz bağlantısı ile ilgili temel kavramlar tanıtılmaktadır.

Azure IOT Central'ı kullanan [Azure IOT Hub cihazı sağlama hizmeti](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) kaydetmesini ve uygun ölçekte cihazları bağlayın.

Cihaz sağlama hizmeti ile:

- Müşteriler, artık cihaz kimlik bilgileri oluştur ve İlk Azure IOT Central cihazları kaydetmek zorunda kalmadan çevrimdışı cihazları yapılandırın.
- Azure IOT Central, destek ve paylaşılan erişim imzaları ile bağlantı geliştirmek devam edebilirsiniz, cihaz bağlantısı X.509 sertifikalarıyla destekler.
- Azure IOT Central müşterileri artık Azure IOT mevcut arka ofis sistemleriyle basit tümleştirme sağlayan Merkezi, cihazları kaydetmek için kendi cihaz kimliklerini kullanabilirsiniz.
- Cihazların Azure IOT Central uygulamasına bağlanmak için tutarlı bir yöntem yoktur.

>[!NOTE]
>Azure IOT Central, IOT Hub cihazı sağlama hizmeti tüm cihaz kaydı ve bağlantı için kullanır. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Kullanım Örneğinize bağlı olarak, cihazları Azure IOT Central uygulamasına bağlanmak için uygun yönergeleri izleyin:

- [Hızlı (paylaşılan erişim imzalarını kullanarak) tek bir cihazı bağlayın](#connect-a-single-device)
- [Paylaşılan erişim imzalarını kullanarak cihazları uygun ölçekte bağlayın](#connect-devices-at-scale-using-shared-access-signatures)
- [X.509 sertifikaları kullanarak uygun ölçekte cihazları bağlayın](#connect-devices-using-x509-certificates) (üretim iş yükleri için önerilir)
- [İlk kayıt cihazları bağlayın](#connect-without-first-registering-devices) 

>[!NOTE]
>Bağlanmak ve sağlamak cihazlar için genel uç noktası **global.azure cihazları provisioning.net**.

## <a name="connect-a-single-device"></a>Tek bir cihazı bağlayın

Paylaşılan erişim imzalarını kullanarak tek bir cihazı Azure IOT Central uygulamasına bağlanmak için:

1. Ekleme bir **gerçek cihaz** gelen **Device Explorer**. Seçin **+ yeni** > **gerçek** gerçek bir cihaz eklemek için.
    - Girin **cihaz kimliği** (küçük olmalıdır) veya önerilen cihaz kimliğini kullanması
    - Girin **cihaz adı** veya önerilen adı kullanın.

    ![Cihaz Ekleme](media/concepts-connectivity/add-device.png)

1. Bağlantı ayrıntıları gibi alma **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**, seçerek eklenen bir cihazı için **Bağlan** üzerinde cihaz sayfası.

    - **[Kapsam kimliği](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**  Azure IOT Central uygulaması ve cihaz sağlama hizmeti tarafından oluşturulur. Bu kimlik, bir uygulama içinde benzersiz cihaz kimliği emin olmak için kullanılır.
    - **Cihaz kimliği** uygulama başına benzersiz cihaz kimliği. Cihaz, cihaz kimliği kayıt çağrısı bir parçası olarak göndermek gereksinim duyar.
    - **Birincil anahtar** bir paylaşılan erişim imzası belirtecini olan Azure IOT Central ile özel bu cihaz için üretilen.
 
    ![Bağlantı ayrıntıları](media/concepts-connectivity/device-connect.png)

1. Bağlantı ayrıntıları **cihaz kimliği**, **cihaz adı**ve cihazın **birincil anahtar** cihaz kodunuzdaki sağlama ve görmeye başlayacaksınız Cihazınızı bağlama Veri anında aracılığıyla akışı. MXChip IOT DevKit (DevKit) cihaz kullanıyorsanız izleyin [adım adım yönergeleri](howto-connect-devkit-experimental.md#add-a-real-device)"Hazırlama DevKit cihaz" bölümünden başlayan

    Kullanmak istediğiniz diğer diller için başvuruları aşağıda verilmiştir.

    - **C dili:** İzleyin [bu C örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md) örnek cihaz bağlayamama. Örnekte aşağıdaki ayarları kullanın:

         ```c
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         ## Enter the Device Id and Symmetric keys 
         prov_dev_set_symmetric_key_info("<Device Id>", "<Enter Primary Symmetric key here>");
        ```

    - **Node.js:**  İzleyin [adım adım yönergeleri](tutorial-add-device-experimental.md#prepare-the-client-code)"Hazırlama istemci kodu" bölümünden başlayan

## <a name="connect-devices-at-scale-by-using-shared-access-signatures"></a>Paylaşılan erişim imzalarını kullanarak cihazları uygun ölçekte bağlayın

Paylaşılan erişim imzalarını kullanarak cihazları uygun ölçekte Azure IOT Central ile bağlanmak için iki genel adımlar vardır:

1. **Cihaz kaydı:** Bunları Azure IOT Central bir CSV dosyasını içeri aktararak, cihazları kaydetmek. Ardından **dışarı** cihazlarınızı dışarı aktarma ve cihazı bağlantı ayrıntılarını almak için eylem.
1. **Cihaz Kurulumu:** Program bağlantı ayrıntıları ile cihazlar (**kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar**). Her cihaz açıldığında, cihaz sağlama hizmeti, bağlantı bilgilerini veya Azure IOT Central uygulama ataması almak için çağırır.

>[!NOTE]
>Gelişmiş bir seçenek de, ilk Azure IOT Central cihazları kaydetmek zorunda kalmadan cihazlar nereye bağlanabilirsiniz kullanılabilir. [Daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

### <a name="device-registration"></a>Cihaz kaydı

Çok sayıda cihaz uygulamanızı bağlamak için Azure IOT Central bir CSV dosyası aracılığıyla cihazların Toplu içe aktarma destekler. 

1. Uygulamada kaydedilecek cihazları içeri aktarın:

   1. Seçin **Device Explorer** sol menüsünde.
   1. Sol panelde, cihaz şablonu toplu istediğiniz için cihazlar oluşturun. 
   1. Seçin **alma**ve ardından içeri aktarılacak cihaz kimlikleri listesi olan CSV dosyasını seçin. CSV dosyası aşağıdaki sütunları (ve üst bilgiler) sahip olmalıdır:

       - IOTC_DeviceID (küçük olmalıdır)
       - IOTC_DeviceName (isteğe bağlı)

   1. İçeri aktarma işlemi bittikten sonra cihaz Kılavuzu'nun bir başarı iletisi gösterilir.

1. Bağlantı ayrıntılarını almak için cihazları dışarı aktarın:

   **Dışarı** eylemi, cihaz Kimliğini, cihaz adını ve cihaz anahtarları ile bir CSV dosyası oluşturur. Cihazınızı Azure IOT Central bağlamak için bu Ayrıntılar'ı kullanın. Toplu dışarı aktarma, uygulamanızdan cihazlar için:

   1. Seçin **Device Explorer** sol menüsünde.
   1. Dışarı aktarma ve ardından istediğiniz cihazları seçin **dışarı** eylem.
   1. Dışarı aktarma işlemi tamamlandıktan sonra bir başarı iletisi oluşturulan dosyasını indirmek için bir bağlantı ile birlikte gösterilir.
   1. Başarılı iletisi, disk üzerindeki yerel bir klasöre dosyasını indirmek için tıklayın.
   1. Dışarı aktarılan CSV dosyasını ilişkin bağlantı bilgilerini içeren aşağıdaki sütunları içerir **cihaz kimliği**, **cihaz adı**, **cihaz birincil veya ikincil anahtarlar**ve  **Birincil veya ikincil sertifika parmak izleri**:

       - IOTC_DEVICEID
       - IOTC_DEVICENAME
       - IOTC_SASKEY_PRIMARY
       - IOTC_SASKEY_SECONDARY
       - IOTC_X509THUMBPRINT_PRIMARY
       - IOTC_X509THUMBPRINT_SECONDARY

### <a name="device-setup"></a>Cihaz Kurulumu

 Bağlantı ayrıntılarını sağlayın ve Cihazınızı bağlamak için kullanın **cihaz kimliği (IOTC_DEVICEID)**, **cihaz birincil anahtarı (IOTC_SASKEY_PRIMARY)**, ve **kapsam kimliği** içinde cihazınızın kodunu. Zaten yapmadıysanız, alma **kapsam kimliği** seçerek Azure IOT Central uygulamanızdan **Yönetim** > **cihaz bağlantı**  >  **Kapsam kimliği**.

DevKit cihaz bağlıyorsanız izleyin [adım adım yönergeleri](howto-connect-devkit-experimental.md#add-a-real-device)"Hazırlama DevKit cihaz" bölümünden başlayan

Kullanmak istediğiniz diğer diller için başvuruları aşağıda verilmiştir.

- **C dili:** İzleyin [bu C örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md) örnek cihaz bağlayamama. Örnekte aşağıdaki ayarları kullanın:

     ```c
     hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

     ## Enter the Device Id and Symmetric keys 
     prov_dev_set_symmetric_key_info("<Device Id>", "<Enter Primary Symmetric key here>");
    ```

- **Node.js:** İzleyin [adım adım yönergeleri](tutorial-add-device-experimental.md#prepare-the-client-code)"Hazırlama istemci kodu" bölümünden başlayan

## <a name="connect-devices-by-using-x509-certificates"></a>X.509 sertifikaları kullanarak cihazları bağlayın

Bir kanıtlama mekanizması olarak X.509 sertifikaları kullanarak, üretim ölçeği ve cihaz sağlamayı kolaylaştırmak için mükemmel bir yoldur. X.509 sertifikaları, genellikle güven içinde zincirindeki her sertifika özel anahtarını sonraki daha yüksek sertifika ve benzeri, otomatik olarak imzalanan kök sertifikada bitiş tarafından imzalanmış bir sertifika zinciri halinde düzenlenir. Bu yapı, bir temsilci aracılığıyla her bir cihaza yüklenen son varlık "yaprak" Sertifika bir ara CA'ya aşağı güvenilen kök sertifika yetkilisi (CA) tarafından oluşturulan kök sertifika güven zinciri oluşturur. Daha fazla bilgi için bkz. [X.509 CA sertifikalarını kullanarak cihaz kimlik doğrulaması](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

X.509 sertifikaları kullanarak cihazların Azure IOT Central uygulamasına bağlanmak için üç önemli adım kullanılan vardır:
 
1. **Bağlantı ayarlarını yapılandırdıktan** Azure IOT Central uygulamasında ekleyerek veya X.509 kök veya cihaz sertifikalarını oluşturmak için kullanılan ara sertifika doğrulanıyor. X.509 sertifikaları için bağlantı ayarlarını yapılandırmak için üç adım vardır:  

    - **X.509 kök veya ara sertifika ekleme** yaprak cihaz sertifikalarını oluşturmak için kullanmakta olduğunuz. Git **Yönetim** > **cihaz bağlantı** > **sertifikaları**. 
    
       ![Bağlantı ayarları](media/concepts-connectivity/connection-settings.png)

    - **Sertifika doğrulayın.** Sertifika sahipliğini doğrulayarak sertifikanın uploader sertifikanın özel anahtarı sahip olmasını sağlar. Sertifikayı doğrulamak için:

        - Doğrulama kodu oluştur. Düğmeyi seçin **doğrulama kodu** bu kodu oluşturmak için alan. 
        - Doğrulama kodu ile bir X.509 doğrulama sertifikası oluşturun. Sertifikayı bir .cer dosyası olarak kaydedin. 
        - İmzalı doğrulama sertifikasını karşıya yükleyin ve seçin **doğrulama**.

        ![Bağlantı ayarları](media/concepts-connectivity/verify-cert.png)

    - **İkincil sertifika ekleyin.** IOT çözümünüzü yaşam döngüsü boyunca, sertifikaları alma gerekecektir. Sertifikalar alınıyor ana nedeni güvenlik ihlallerini ve sertifika süre sonu ikisidir. İkincil sertifika birincil sertifikası güncelleştiriyorsanız, sağlamayı çalıştığınız cihazlar için kapalı kalma süresini azaltın.

      **Yalnızca test amaçlı:**
    
      CA sertifikaları ve cihaz sertifikaları oluşturmak için kullanabileceğiniz bazı yardımcı programı komut satırı araçları şunlardır.

      - DevKit cihaz kullanıyorsanız, işte bir [komut satırı aracı](https://aka.ms/iotcentral-docs-dicetool) CA sertifikalarını oluşturmak için. Azure IOT Central uygulamanıza ekleyin ve sertifikaları doğrulayın. 

      - Kullanım [bu komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) için:

          - Sertifika zinciri (izleme adım 2'de GitHub docs) oluşturun. Sertifika .cer dosyası kaydedin ve (birincil sertifikaları) için Azure IOT Central karşıya yükleyin.
          - Azure IOT Central uygulama doğrulama kodu alma, sertifikayı (izleme adımı 3 GitHub docs) oluşturmak ve doğrulamak için sertifikayı karşıya yüklemek. 
          - Yaprak sertifikalar, cihaz Kimliğine sahip ' % s'aracı (izleyin 4. adım) bir parametre olarak oluşturun. Sertifikayı kaydetmek ve Cihazınızda kullanın.

1. **Cihazları kaydetme** bir CSV dosyası bunları Azure IOT Central alarak.

1. **Cihaz Kurulumu**: Karşıya yüklenen bir kök sertifikayı yaprak sertifikalar oluşturmak. Cihaz kimliği yaprak sertifikalar CNAME kullanın ve küçük harflerle olduğundan emin olun emin olun. İşte bir [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) yaprak/cihaz sertifika oluşturmaya yönelik **yalnızca test amacıyla**.

1. **Cihaz sağlama hizmeti bilgileri ile program**, açık, zaman bağlantı ayrıntılarını ve Azure IOT Central, uygulama ataması almak için etkinleştirme.

    Daha fazla bilgi için şu makalelere bakın:

    - [Raspberry Pi için örnek uygulama](https://aka.ms/iotcentral-docs-Raspi-releases)  

    - [Örnek c cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

## <a name="connect-without-first-registering-devices"></a>İlk kayıt cihazları bağlayın

Azure IOT Central'ı destekleyen senaryoları biri, OEM'ler cihazları toplu olarak üretim, kimlik bilgileri oluştur ve İlk Azure IOT Central cihazları kaydetmek zorunda kalmadan cihazları Fabrika yapılandırmak içindir. Cihaz açık ve bunların Azure IOT Central uygulamasına bağlanma girişiminde sonra cihazlar Azure IOT Central uygulamasına bağlanmak için işleci onaylar.

Bu özelliği kullanarak cihazları bağlamak için akışı şu şekildedir:

![Bağlantı ayarları](media/concepts-connectivity/device-connection-flow.png)

Cihaz kimlik doğrulaması düzeni (X.509 sertifikalarıyla ya da paylaşılan erişim imzaları) ettiğiniz temel adımları izleyin:

1. **Yapılandırma veya bağlantı ayarlarını alın:**

    - **X.509 sertifikaları:** [Ekleme ve kök veya Ara Sertifika doğrulama](#connect-devices-using-x509-certificates) ve bir sonraki adımda cihaz sertifikaları oluşturmak için kullanın.
    - **Paylaşılan erişim imzaları:** (Bu grubu paylaşılan erişim imza anahtarı için bu Azure IOT Central uygulamasına anahtarıdır) kullanarak PRIMARY Key'i kopyalayın ve bir sonraki adımda cihaz paylaşılan erişim imzası anahtarları oluşturmak için kullanın.

       ![Paylaşılan erişim imzaları için bağlantı ayarları](media/concepts-connectivity/connection-settings-sas.png)

1. **Cihaz kimlik bilgilerini oluşturun:**

    - **X.509 sertifikaları:** Yaprak sertifikalar cihazlarınız için kök veya Ara sertifikayı bu uygulamaya eklediğiniz kullanarak oluşturun. Cihaz kimliği bir CNAME yaprak sertifikalar kullanın ve küçük harf olduğundan emin olun emin olun. İşte bir [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) Test yaprak/cihaz sertifikalarını oluşturmak için.
    - **Paylaşılan erişim imzaları:** Cihaz paylaşılan erişim imzası anahtarları oluşturmak için bunu kullanın [komut satırı aracı](https://www.npmjs.com/package/dps-keygen). Önceki adımdan birincil paylaşılan erişim imza anahtarı (Grup paylaşılan erişim imza anahtarı) kullanın. Küçük harflerle kimliğidir cihaz emin olun.

        Cihazın bağlantı dizesini almak için aşağıdaki komutu kullanın: 

        ```
        npm i -g dps-keygen
        ```
    
        Cihaz paylaşılan erişim imza anahtarı oluşturmak için aşağıdaki komutu kullanın:
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Cihazları ayarlayın:** Her bir cihaza flash **kapsam kimliği**, **cihaz kimliği**, ve **cihaz sertifika/SAS anahtarı**, Azure IOT Central uygulamasına bağlanmak için cihazda açın.

1. **Cihazların Azure IOT Central bağlanın:** Cihazları geçtikten sonra bunlar cihaz sağlama hizmeti/Azure IOT Central için kayıt için bağlanın.

1. **Bir şablon cihazlara ilişkilendirin:** Bağlı cihazlar görünmesini altında **beklemediğiniz ilişkili cihazları** içinde **Device Explorer**. Sağlama durumu, cihaz **kayıtlı**. **İlişkilendirme** uygun cihaz şablonu cihazlara ve cihazların Azure IOT Central uygulamasına bağlanmak için onaylayın. Cihazların Azure IOT Central uygulamasına yönelik bağlantı ayrıntılarını alın ve ardından bağlanmak ve veri göndermeye başlayın. Cihaz sağlama artık tamamlandı ve **sağlama durumu** cihazlar için kapatır **sağlanan**.

## <a name="device-provisioning-status"></a>Cihaz sağlama durumu

Aşağıdaki adımlar, Azure IOT Central için gerçek bir cihaz bağlandığında gerçekleşir:

1. **Kayıtlı**: Cihazın ilk kayıtlı cihaza Azure IOT Central içinde oluşturulur ve cihaz için cihaz kimliğinin anlamına gelir. Bir cihazın kayıtlı olduğunda:

    * Yeni bir gerçek cihaz eklenir **Device Explorer**
    * Bir cihaz kümesini kullanarak eklenen **alma** içinde **Device Explorer**
    * Henüz kayıtlı bir cihaz, geçerli kimlik bilgileriyle bağlanır ve altında görülebilir **beklemediğiniz ilişkilendirilmiş cihazlar**

    Tüm yukarıdaki durumlarda **sağlama durumu** olduğu **kayıtlı**.

1. **Sağlanan**: Cihaz, geçerli kimlik bilgileriyle bağlandığında, Azure IOT Central sağlama adım (cihaz IOT Hub'ında oluşturarak) tamamlar. Bağlanmak ve veri göndermeye başlamak için azure IOT Central cihaza sonra bağlantı dizesini döndürür. Cihazın **sağlama durumu** gelen kapatır **kayıtlı** için **sağlanan**.

1. **Engellenen**: İşleci, bir cihaz engelleyebilirsiniz. Bir cihaz engellenir sonra verileri Azure IOT Central gönderilemiyor ve sıfırlanması gerekir. Engellenmiş cihazların sahip **sağlama durumu** , **bloke**. İşleci ayrıca cihazın engelini kaldırma. Cihazın engellemesini sonra **sağlama durumu** önceki durumuna geri döndürür (**kayıtlı** veya **sağlanan**). 

## <a name="get-the-device-connection-string"></a>Cihaz bağlantı dizesini alma

Azure IOT Hub ile IOT hub'ı cihaz bağlantı dizesi, aşağıdaki adımları kullanarak alabilirsiniz:

1. Gibi cihazın bağlantı ayrıntıları alma **kapsam kimliği**, **cihaz kimliği**, ve **birincil anahtar** gelen **cihaz bağlantı** sayfası. Bu bilgi edinmek için Git **cihaz** sayfasından seçim yapıp **Connect**. 

    ![Bağlantı ayrıntıları](media/concepts-connectivity/device-connect.png)

1. Dps-keygen komut satırı aracını kullanarak cihazın bağlantı dizesini alın. Cihazın bağlantı dizesini almak için aşağıdaki komutu kullanın:  

    ```cmd/sh
    npm i -g dps-keygen
    ```

    Bir bağlantı dizesi oluşturmak için ikili altında Bul *bin /* klasörü:

    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```

    [Dps-keygen aracı hakkında daha fazla bilgi](https://www.npmjs.com/package/dps-keygen).

## <a name="sdk-support"></a>SDK desteği

Azure IOT SDK'ları, Azure IOT Central uygulamasına bağlanmak, cihazlarınızın kod kullanabilmeniz için en kolay yolu sunar. Aşağıdaki Sdk'lardan kullanılabilir:

- [C için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-python)
- [Azure IOT SDK'sı için Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-csharp)

Her cihaz, cihaz tanımlayan benzersiz bir bağlantı dizesi kullanarak bağlanır. Cihazlar, kayıtlı olduğu yalnızca IOT hub'ına bağlanabilir. Azure IOT Central uygulamanızda gerçek bir cihaz oluşturduğunuzda, uygulama, kullanabilmeniz bir bağlantı dizesi oluşturur.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özelliklerinin ve IOT Hub bağlantı

Tüm cihaz iletişimi IOT Hub ile IOT Hub bağlantı şunlardan kullanır:

- [CİHAZDAN buluta ileti gönderme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Cihaz ikizlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

Azure IOT Central cihaz özellikleri için IOT hub'ı özelliklerinden nasıl eşleştiği aşağıdaki tabloda özetlenmiştir:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Ölçüm: Telemetri | CİHAZDAN buluta ileti gönderme |
| Cihaz özellikleri | Özellikler cihaz çiftinin bildirilen |
| Ayarlar | Cihaz ikizi istenen ve bildirilen özellikler |

Azure IOT SDK'ları kullanma hakkında daha fazla bilgi edinmek için örnek kod için aşağıdaki makalelere bakın:

- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs-experimental.md)
- [Azure IOT Central uygulamanıza bir Raspberry Pi cihazı bağlayın](howto-connect-raspberry-pi-python.md)
- [MXChip IOT DevKit cihazı Azure IOT Central uygulamanızı bağlayın](howto-connect-devkit-experimental.md)


## <a name="protocols"></a>Protokoller

Azure IOT SDK'ları, bir IOT hub'ına bağlamak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bir seçme bu protokolleri ve yönergeleri hakkında bilgi için [iletişim protokolü seçme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Cihazınızı desteklenen protokollerinin hiçbirini kullanamıyorsanız, Azure IOT Edge, dönüştürme protokolü için de kullanabilirsiniz. IOT Edge, işleme, Azure IOT Central uygulamadan uca boşaltmak için diğer zeka-üzerinde--edge senaryolarını destekler.

## <a name="security"></a>Güvenlik

Azure IOT Central uygulamanızı ve aygıtlar arasında alınıp verilen tüm veriler şifrelenir. IOT Hub cihaz bakan IOT Hub uç noktaları birine bağlayan bir CİHAZDAN her isteğin kimliğini doğrular. Kimlik bilgileri kablo üzerinden değişimi önlemek için bir cihaz kimlik doğrulaması için imzalanmış belirteçleri kullanır. Daha fazla bilgi için bkz. [IoT Hub'a erişimi denetleme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

## <a name="next-steps"></a>Sonraki adımlar

- [Hazırlama ve MXChip IOT DevKit cihazı bağlayın](howto-connect-devkit-experimental.md)
- [Hazırlama ve Raspberry Pi cihazı bağlayın](howto-connect-raspberry-pi-python.md)
- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs-experimental.md)
