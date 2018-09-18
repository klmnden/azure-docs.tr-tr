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
ms.openlocfilehash: d2ed07be829e48cc4fc0538c08fd498dea99e71e
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985184"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Azure IOT Central, cihaz bağlantısı

Bu makalede, Microsoft Azure IOT Central, cihaz bağlantısı ile ilgili temel kavramlar tanıtılmaktadır.

Azure IOT Central'ı kullanan [Azure IOT Hub cihazı sağlama hizmeti (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps), IOT Central'ı ekleme destekleyecek şekilde etkinleştirilmesi ve uygun ölçekte cihazları bağlama.

-   Müşteriler, artık cihaz kimlik bilgileri oluştur ve ilk IOT Central cihazları kaydetmek zorunda kalmadan çevrimdışı cihazları yapılandırma
-   IOT Central ile sektör X509 önerilen cihaz bağlantısını destekler sertifika tabanlı bağlantı, desteği ve paylaşılan erişim imzaları (SAS) bağlantı geliştirmeye devam ederken
-   IOT Central müşterileri artık IOT Central, cihazlarını kaydetmek için kendi cihaz kimlikleri mevcut arka ofis sistemleriyle basit tümleştirmesi etkinleştiriliyor getirebilirsiniz
-   Cihazları IOT Central bağlamak için tutarlı bir yöntem yoktur. 

>[!NOTE]
>IOT Central kullanan Azure IOT cihaz sağlama hizmeti (DPS) altındaki tüm cihaz kaydı ve bağlantı [DPS hakkında daha fazla bilgi](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Cihazları IOT Central bağlamak için yönergeler, kullanım örneği takip edin tabanlı
1. [Tek bir cihazı hızlı bir şekilde bağlanın (paylaşılan erişim imzaları kullanma)](#connect-a-single-device)
1. [Cihazları uygun ölçekte paylaşılan erişim imzaları (SAS) kullanarak bağlanma](#connect-devices-at-scale-using-shared-access-signatures)
1. [Cihazları uygun ölçekte X509 kullanarak bağlanma sertifikaları](#connect-devices-using-x509-certificates) **üretim iş yükleri için önerilir**
1. [İlk kayıt cihazları bağlayın](#connect-without-first-registering-devices) 


>[!NOTE]
>Bağlanmak ve sağlamak cihazlar için genel uç noktası işte **global.azure cihazları provisioning.net**.

## <a name="connect-a-single-device"></a>Tek bir cihazı bağlayın
Tek bir cihaz IOT Central için SAS kullanarak kolayca bağlayıp yalnızca birkaç adımda 
1. Ekleme bir **gerçek cihaz** Device Explorer tıklayarak **+ yeni > gerçek** gerçek bir cihaz eklemek için.
    * Cihaz kimliğini girebilir **<span style="color:Red">(küçük harf olması gerekir)</span>** veya önerilen cihaz kimliğini kullanması
    * Cihaz adını girin veya önerilen adı kullanın   
    ![Cihaz Ekleme](media\concepts-connectivity\add-device.png)
1. Bağlantı ayrıntıları gibi almak **kapsam kimliği, cihaz kimliği ve birincil anahtarınızı** tıklayarak eklenen bir cihazı için **Connect** cihaz sayfasında.
    * **[Kapsam kimliği](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**  IOT Central uygulaması ve dağıtım noktaları, bir uygulama içinde benzersiz cihaz kimliği sağlamak için kullanılan tarafından oluşturulur.
    * **Cihaz kimliği** benzersiz cihaz kimliği uygulama, cihaz başına gereken cihaz kimliği kayıt çağrısı bir parçası olarak göndermek için.   
    * **Birincil anahtar** bir SAS belirteci ile IOT Central belirli cihaz için oluşturulan. 
    ![Bağlantı ayrıntıları](media\concepts-connectivity\device-connect.PNG)
1. Bu bağlantı ayrıntıları **cihaz kimliği, cihaz adı ve cihaz birincil anahtarı** cihaz kodunuzdaki sağlama ve Cihazınızı bağlama aracılığıyla anında akış verileri görmeye başlayacaksınız. MxChip cihaz izleme kullanıyorsanız [adım adım yönergeleri burada](howto-connect-devkit.md#add-a-real-device), bölümünden Başlat **DevKit cihazı hazırlama**.   

    Kullanmak istediğiniz diğer diller için başvuruları aşağıdadır.

    *   **C dili:** C kullanıyorsanız izleyin [bu C örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) örnek cihaz bağlayamama. Örnekte aşağıdaki ayarları kullanın.   

         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
         
         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";

         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```

    *   **Node.js:** Node.js kullanmak istiyorsanız [adım adım yönergeleri burada kullanmak](tutorial-add-device.md#prepare-the-client-code), bölümünden Başlat **istemci kodu hazırlama**.



## <a name="connect-devices-at-scale-using-shared-access-signatures"></a>Uygun ölçekte paylaşılan erişim imzalarını kullanarak cihazları bağlayın

SAS kullanarak IOT Central ile cihazları uygun ölçekte bağlanmak için iki adımı kullanılan vardır 
1. **Cihazları kaydetme** CSV aracılığıyla IOT Central uygulamasına aktararak dosya ve cihazlarınıza bağlanmak için kullanılacak cihaz bağlantı ayrıntıları ile cihazları dışarı aktarma
1. **Cihaz Kurulumu** cihazı ile bağlantı ayrıntıları programlanmıştır ( **kapsam kimliği, cihaz kimliği ve birincil anahtarınızı**), kendi bağlantı bilgisi/IOT Central uygulama ataması olduğunda almak için sağlama hizmeti çağıracak şekilde etkinleştirme açık.

>[!NOTE]
>Gelişmiş bir seçenek de ilk IOT Central cihazlarını kaydetmek zorunda kalmadan aygıtları bağlayabileceğiniz kullanılabilir [buradan daha fazla bilgi edinin](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

**Cihaz kaydetme**

Uygulamanızı, Azure IOT Central sayıda cihaza bağlanmak için bir CSV dosyası içeri aktarılırken aygıtına teklifler toplu. 

CSV dosyası gereksinimleri: CSV dosyası aşağıdaki sütunları (ve üst bilgiler) sahip olmalıdır
1.  IOTC_DeviceID  **<span style="color:Red">(küçük harf olması gerekir)</span>**
1.  IOTC_DeviceName (isteğe bağlı)



Uygulamada kaydedilecek cihazları İçeri Aktar
1.  Seçin **Gezgini** sol gezinti menüsünde.
1.  Sol panelde, toplu olarak istediğiniz cihaz şablonu oluşturma cihazları seçin. 
1.  Tıklayın **alma**, içeri aktarılacak cihaz kimlikleri listesini içeren bir CSV dosyasını seçin.
CSV dosyası aşağıdaki sütunları (ve üst bilgiler) sahip olmalıdır
    *   IOTC_DeviceID  **<span style="color:Red">(küçük harf olması gerekir)</span>**
    *   IOTC_DeviceName (isteğe bağlı)
1.  İçeri aktarma işlemi tamamlandıktan sonra cihaz Kılavuzu'nun bir başarı iletisi gösterilir.

Bağlantı ayrıntıları almak için cihazları dışarı aktarma dışarı aktarma, cihaz kimliği, cihaz adı ve cihaz anahtarı ile bir CSV dosyası oluşturur. IOT Central için Cihazınızı bağlamak için bu Ayrıntılar'ı kullanın.
Uygulamanızdan dışarı aktarma cihazları toplu olarak:
1.  Seçin **Gezgini** sol gezinti menüsünde.
1.  Dışarı aktarma ve ardından istediğiniz cihazları seçin **dışarı** eylem.
1.  Dışarı aktarma işlemi tamamlandıktan sonra bir başarı iletisi oluşturulan dosyasını indirmek için bir bağlantı ile birlikte gösterilir.
1.  Başarılı iletisi, disk üzerindeki yerel bir klasöre dosyasını indirmek için tıklayın.
1.  Dışarı aktarılan CSV dosyasını aşağıdaki sütunları bilgilere sahip olacağı: **cihaz kimliği, cihaz adı, cihaz Priamary/tali anahtarları ve birincil/ikincil sertifika thumbrpints**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY


**Cihaz Kurulumu**

Bu bağlantı ayrıntıları **cihaz kimliği (IOTC_DEVICEID), cihaz birincil anahtarı (IOTC_SASKEY_PRIMARY) ve kapsam kimliği** cihaz kodunuzda sağlamak ve Cihazınızı bağlamak için. Zaten varsa, alma **kapsam kimliği** IOT Central uygulamanızdan **Yönetim > cihazı bağlantı > kapsam kimliği**.
Kullanıyorsanız **MxChip** izleyin bağlanmak için cihaz [adım adım yönergeleri burada](howto-connect-devkit.md#add-a-real-device), bölümünden Başlat **DevKit cihazı hazırlama**.   

Kullanmak istediğiniz diğer diller için başvuruları aşağıdadır.

   *   **C dili:** C izleyin kullanıyorsanız [bu C örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) örnek cihaz bağlayamama. Örnekte aşağıdaki ayarları kullanın.   
         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";
         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```
    * **Node.js:** Node.js kullanmak istiyorsanız [adım adım yönergeleri burada kullanmak](tutorial-add-device.md#prepare-the-client-code), bölümünden Başlat **istemci kodu hazırlama**.


## <a name="connect-devices-using-x509-certificates"></a>X509 kullanarak cihazları bağlayın sertifikaları

Kullanarak X.509 sertifikaları bir kanıtlama mekanizması olarak ölçeklendirmek için mükemmel bir yoldur **üretim** ve cihaz sağlamayı kolaylaştırır. X.509 sertifikaları, genellikle güven içinde zincirindeki her sertifika özel anahtarını sonraki daha yüksek sertifika ve benzeri, otomatik olarak imzalanan kök sertifikada sonlandırma tarafından imzalanmış bir sertifika zinciri halinde düzenlenir. Bu, bir temsilci kök sertifikadan bir güvenilen kök sertifika yetkilisi (CA) üzerinden her bir ara CA'ya bir cihaza yüklenen son varlık "yaprak" sertifika tarafından oluşturulan güven zinciri oluşturur. Daha fazla bilgi için bkz. [cihaz X.509 CA sertifikalarını kullanarak kimlik doğrulaması](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Cihazları IOT için bağlamak için X509 kullanarak merkezi sertifikaları, burada üç önemli adım karmaşıktır 
1. **Bağlantı ayarlarını yapılandırdıktan** ekleme/doğrulayarak kök/Ara sertifikayı cihaz sertifikalarını oluşturmak için kullanılan X509 IOT Central uygulamasında.  İki adımdan X509 bağlantı ayarlarını yapılandırmak için sertifikalar.  

    *   **X509 ekleme kök veya Ara sertifikayı** yaprak cihaz sertifikalarını oluşturmak için kullandığınız. Yönetim gidin > cihaz bağlantısı > sertifikalar. 
    
        ![Bağlantı ayarları](media\concepts-connectivity\connection-settings.PNG)
    *   **Sertifika doğrulama:** sertifika sahipliğini doğrulayarak sertifikanın uploader sertifikanın özel anahtarı elinde olmasını sağlar. Sertifikayı doğrulamak için
        *  Doğrulama kodu oluştur, doğrulama kodunu oluşturmak için bir doğrulama kodu alanın yanındaki düğmeye tıklayın. 
        *  Sertifikayı bir .cer dosyası olarak kaydetme doğrulama kodunu içeren bir X.509 doğrulama sertifikası oluşturun. 
        *  İmzalı doğrulama sertifikasını karşıya yükleyin ve tıklatın doğrulayın.

        ![Bağlantı ayarları](media\concepts-connectivity\verify-cert.png)
    *   **İkincil sertifika:** IOT çözümünüzü yaşam döngüsü boyunca, sertifikaları alma gerekecektir. İki sertifikaları çalışırken ana nedeni, güvenlik ihlali ve sertifika süre sonu olacaktır. Birincil sertifika güncelleştirildiği sırada sağlama girişiminde cihazlar için kapalı kalma süresini azaltmak için ikincil sertifikalar kullanılır.

    **YALNIZCA TEST ETME AMAÇLARI İÇİN** 
    
    CA sertifikaları ve cihaz sertifikaları oluşturmak için kullanabileceğiniz bazı yardımcı program komut satırı araçları aşağıdadır.

    * Burada MxChip kullanıyorsanız bir [komut satırı aracı](http://aka.ms/iotcentral-docs-dicetool) sertifikası CA oluşturmak için IOT Central uygulamanıza ekleyin ve sertifikaları doğrulayın. 

    *   Bunu kullanın [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) için
        * Sertifika zinciri (izleyin 2. adım GitHub docs) oluşturun. 
         Sertifikaları .cer dosyası kaydedin ve IOT Central için (birincil) karşıya yükleyin.   
        * IOT Central uygulama doğrulama kodu alma, sertifikayı (izleyin 3. adım GitHub docs) oluşturmak ve doğrulamak için yükleyin. 
        * Yaprak sertifikalar, cihaz Kimliği ' % s'aracı (izleyin 4. adım) bir parametre olarak oluşturun. Sertifika Kaydet ve Cihazınızda kullanın.     

1. **Cihazları kaydetme** bunları bir CSV dosyası IOT Central alarak.

1. **Cihaz Kurulumu** : karşıya yüklenen bir kök sertifikayı kullanarak yaprak sertifikalar oluşturur. Kullandığınızdan emin olun **cihaz kimliği** yaprak CNAME olarak sertifikaları ve içinde **küçük**. İşte bir [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) yönelik yaprak/cihaz sertifikaları oluşturulacak **yalnızca test AMAÇLI**.

    Cihaz sağlama hizmeti bilgilerini açık olduğunda uygulama ataması IOT Central ve bağlantı ayrıntılarını almak için etkinleştirme ile program.    

    **Daha fazla referene** 
    *   Örnek uygulama için [RaspberryPi.](http://aka.ms/iotcentral-docs-Raspi-releases)  

    *   [C'de, örnek cihaz istemcisi](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

>[!NOTE]
>Kullanım **cihaz kimliği** cihazlar için yaprak sertifikalar oluştururken bir cname olarak.

>[!NOTE]
>**Cihaz kimliği** küçük olmalıdır 
 
## <a name="connect-without-first-registering-devices"></a>İlk kayıt cihazları bağlayın
IOT Central sağlayan önemli senaryolar yığın üretim cihazlara OEM'ler için kimlik bilgileri oluştur ve bunları IOT Central önce kaydetmeniz gerek kalmadan fabrikada yapılandırırken biridir. Cihaz açık ve IOT Central bağlanmak sonra işleci IOT Central uygulamasına bağlanmak için cihaz onaylar.

Bu özelliği kullanarak cihazları bağlamak için akış aşağıda verilmiştir

![Bağlantı ayarları](media\concepts-connectivity\device-connection-flow.PNG)


Cihaz kimlik doğrulaması düzeni (X509/SA) ettiğiniz alarak adımlarını izleyin

1. **bağlantı ayarları** 
    * **X509 sertifikaları:** [Ekle ve kök/Ara sertifikayı doğrulamak](#connect-devices-using-x509-certificates) ve bir sonraki adımda cihaz sertifikaları oluşturmak için kullanın.
    * **SAS:** (Bu IOT Central uygulamasına grubu SAS anahtarı ise bu anahtar) kullanarak PRIMARY Key'i kopyalayın ve bir sonraki adımda cihaz SAS anahtarları oluşturmak için kullanın. 
![SAS bağlantı ayarları](media\concepts-connectivity\connection-settings-sas.png)

1. **Cihaz kimlik bilgileri oluştur** 
    *   **Sertifikaları X509:** için bu uygulamaya eklediğiniz kök/Ara sertifikayı kullanarak cihazlarınızı yaprak sertifikalar oluşturur. Kullandığınızdan emin olun **cihaz kimliği** bir cname yaprak sertifikalar olarak ve  **<span style="color:Red">(küçük harf olması gerekir)</span>**. İşte bir [komut satırı aracı](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) Test yaprak/cihaz sertifikaları oluşturulacak.
    *   **SAS** cihaz SAS anahtarları, bunu kullanarak oluşturulabilir [komut satırı aracını](https://www.npmjs.com/package/dps-keygen). Önceki adımdan birincil SAS anahtarı (Grup SAS anahtarı) kullanın. Cihaz kimliği emin  **<span style="color:Red">alt durumda</span>**.

        Kullanımı cihaz SAS anahtarı oluşturma yönergeleri aşağıda           

        ```
        npm i -g dps-keygen
        ```
    
        **Kullanım**
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Cihaz Kurulumu** 
    
     Cihazla flash **kapsam kimliği, cihaz kimliği, cihaz sertifika/SAS anahtarı** ve cihazın IOT Central uygulamasına bağlanmak için etkinleştirin.

1. **Cihazı IOT Central bağlayın:** kez açık cihazları kayıt için DPS/IOT Central uygulamasına bağlayın.

1. **Bir şablon için cihaz ilişkilendirme:** bağlı cihaza görünür **ilişkilendirilmemiş cihazları** içinde **Device Explorer**. Sağlama durumu cihaz **kayıtlı**. **İlişkilendirme** uygun cihaz şablonu cihaza ve cihazı IOT Central uygulamasına bağlanmak için onaylayın. Cihaz IOT Central bağlantısı ayrıntılarını alır, bağlanır ve veri göndermeye başlar. Cihaz provioning tamamlandığında artık ve *sağlama durumu* kapatır **sağlanan**.

## <a name="device-provisioning-status"></a>Cihaz sağlama durumu
Bir dizi adım kullanılan vardır gerçek bir cihaz için Azure IOT Central bağlı olduğunda 
1. **Kayıtlı**: cihaz ilk **kayıtlı**, yani cihazın IOT Central içinde oluşturulur ve cihaz için cihaz Kimliğine sahip.
Cihazdır Registeretd olduğunda  
    *   Yeni bir gerçek cihaz üzerinde eklenir **Gezgini**
    *   Bir cihaz kümesini kullanarak eklendiğinden **alma** üzerinde **Gezgini**
    *   Kaydedilmemiş ancak geçerli kimlik bilgileriyle bağlanır ve altında görünür bir cihaz **beklemediğiniz ilişkili** cihazlar. 

    Yukarıdaki durumların tümünde *sağlama durumu* olduğu **kayıtlı**

1. **Sağlanan**: cihaz, geçerli kimlik bilgileriyle bağlandığında, sonraki adım ise IOT Central (cihaz IOT Hub'ında oluşturarak) sağlama adımı tamamlar. Daha sonra cihaza bağlanmak ve veri göndermeye başlamak için bağlantı dizesini döndürür. Cihaz *sağlama durumu* gelen artık kapatır **kayıtlı** için **sağlanan**.

1.  **Engellenen**: cihaz engellenir sonra işleci bir cihaz engelleyebilirsiniz veri IOT Central gönderilemiyor ve sıfırlanması gerekir. Engellenmiş cihazların sahip *sağlama durumu* , **bloke**. İşleci ayrıca cihazın engelini kaldırma. Bir kez cihazın engellemesini *sağlama durumu* dönün, önceki *sağlama durumu* (kayıtlı veya sağlanan). 

## <a name="getting-device-connection-string"></a>Cihaz bağlantı dizesini alma
Aşağıdaki adımları kullanarak IOT hub cihaz bağlantı dizesini Azure IOT hub'ına alabilirsiniz. 
1. Bağlantı ayrıntıları gibi almak **kapsam kimliği, cihaz kimliği, cihaz birincil anahtar** cihaz sayfasından (cihaz sayfasına var > Bağlan'a tıklayın) 

    ![Bağlantı ayrıntıları](media\concepts-connectivity\device-connect.PNG)

1. Aşağıdaki komutu satırı aracını kullanarak cihaz bağlantı dizesini alın.
    Kullanım aşağıdaki cihaz bağlantı dizesini almak için yönergeleri  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Kullanım**

    Bir bağlantı dizesi oluşturmak için ikili depo altında Bul / klasör
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Daha fazla bilgi edinin [dps-keygen aracı.](https://www.npmjs.com/package/dps-keygen)

## <a name="sdk-support"></a>SDK desteği

Sizin için en kolay yolu Azure cihaz SDK'ları teklifi, Azure IOT Central uygulamasına bağlanır, cihazlarınızın kod uygulayın. Aşağıdaki cihaz SDK'ları kullanılabilir:

- [C için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-c)
- [Python için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-python)
- [Azure IOT SDK'sı için Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Java için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-java)
- [.NET için Azure IOT SDK](https://github.com/azure/azure-iot-sdk-csharp)

Her cihaz, cihaz tanımlayan benzersiz bir bağlantı dizesi kullanarak bağlanır. Bir cihaz, yalnızca, kayıtlı olduğu IOT hub'ına bağlanabilir. Azure IOT Central uygulamanızda gerçek bir cihaz oluşturduğunuzda, uygulama, kullanabilmeniz bir bağlantı dizesi oluşturur.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK özelliklerinin ve IOT Hub bağlantı

Tüm cihaz iletişimi IOT Hub ile IOT Hub bağlantı şunlardan kullanır:

- [CİHAZDAN buluta ileti gönderme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Cihaz ikizlerini](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

Azure IOT Central cihaz özellikleri açın IOT hub'ı özelliklerinden nasıl eşleştiği aşağıdaki tabloda özetlenmiştir:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Ölçüm: Telemetri | CİHAZDAN buluta ileti gönderme |
| Cihaz özellikleri | Özellikler cihaz çiftinin bildirilen |
| Ayarlar | Cihaz ikizi istenen ve bildirilen özellikler |

Cihaz SDK'ları kullanma hakkında daha fazla bilgi edinmek için aşağıdaki makalelerden birine örnek kod için bkz:

- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
- [Azure IOT Central uygulamanıza bir Raspberry Pi cihazı bağlayın](howto-connect-raspberry-pi-python.md)
- [Azure IOT Central uygulamanıza Devdiv'e Seti cihaz bağlayamama](howto-connect-devkit.md).


## <a name="protocols"></a>Protokoller

Cihaz SDK'ları, bir IOT hub'ına bağlamak için aşağıdaki ağ protokollerini destekler:

- MQTT
- AMQP
- HTTPS

Bir seçme bu fark protokolleri ve yönergeleri hakkında bilgi için [iletişim protokolü seçme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Cihazınızı desteklenen protokollerinin hiçbirini kullanamıyorsanız, Azure IOT Edge, dönüştürme protokolü için de kullanabilirsiniz. IOT Edge, işleme, Azure IOT Central uygulamadan uca boşaltmak için diğer zeka-üzerinde--edge senaryolarını destekler.

## <a name="security"></a>Güvenlik

Cihazlar ile Azure IOT Central arasında alınıp verilen tüm veriler şifrelenir. IOT Hub cihaz bakan IOT Hub uç noktaları birine bağlayan bir CİHAZDAN her isteğin kimliğini doğrular. Kimlik bilgileri kablo üzerinden değişimi önlemek için bir cihaz kimlik doğrulaması için imzalanmış belirteçleri kullanır. Daha fazla bilgi için bkz: [IOT hub'a erişimi denetleme](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Şu anda, Azure IOT Central'ı bağlayan cihazların, SAS belirteçleri kullanmanız gerekir. X.509 sertifikaları, Azure IOT Central bağlanan cihazlar için desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Azure IOT Central, cihaz bağlantısı hakkında öğrendiniz, önerilen sonraki adımlar şunlardır:

- [Hazırlama ve DevKit cihazı bağlayın](howto-connect-devkit.md)
- [Raspberry Pi'yi hazırlama ve bağlama](howto-connect-raspberry-pi-python.md)
- [Genel bir Node.js istemcisini Azure IoT Central uygulamanıza bağlama](howto-connect-nodejs.md)
