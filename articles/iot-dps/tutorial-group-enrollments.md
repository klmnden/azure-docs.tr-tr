---
title: "Azure IOT hub'ına Java ve kayıt gruplarını kullanarak sanal bir X.509 cihaz sağlama | Microsoft Docs"
description: "Azure Öğreticisi - oluşturun ve IOT Hub cihaz sağlama hizmeti için Java cihazını ve hizmetini SDK ve kayıt grupları kullanarak sanal bir X.509 cihaz sağlama"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 01/04/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 14e5e7613fd5df650625cf8997d569b754ceb689
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-and-service-sdk-and-group-enrollments-for-iot-hub-device-provisioning-service"></a>Oluşturun ve IOT Hub cihaz sağlama hizmeti için Java cihaz ve hizmeti SDK'sını ve grup kayıtları kullanarak sanal bir X.509 cihaz sağlama

Bu adımları geliştirme makinenizde Windows işletim sistemi çalıştıran bir X.509 cihazında benzetimini yapma gösterir ve bu sanal cihaz aygıt hizmeti sağlama ve kayıt gruplarını kullanarak IOT hub'ınıza bağlanmak için bir kod örneği kullanın. 

Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun.


## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) uygulamasının yüklü olduğundan emin olun.

1. [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Aşağıdaki [sertifikası genel bakış](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) test sertifikalarınızı oluşturmak için. Sertifikaları oluşturma bir daha derinlemesine bakış için lütfen bkz. [CA tarafından imzalanmış X.509 sertifikalarını yönetmek için PowerShell komut dosyalarını](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-security-x509-create-certificates).

    > [!NOTE]
    > Bu adım gerektirir [OpenSSL](https://www.openssl.org/), hangi ya da yerleşik ve kaynağından yüklü veya indirilebilen ve yüklenen bir [3. taraf](https://wiki.openssl.org/index.php/Binaries) gibi [bu](https://sourceforge.net/projects/openssl/). Zaten oluşturduysanız, _kök_, _Ara_ ve _aygıt_ sertifikaları bu adımı atlayabilir.
    >

1. Kayıt grup bilgileri oluşturun:

    1. Çalıştırma **1. adım** ve **2. adım** oluşturmak için _kök_ ve _Ara_ sertifikalar.

    1. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sağlama hizmetinizi açın.

        1. Cihaz Sağlama Hizmeti özet dikey penceresinde **Sertifikalar**'ı seçip üstteki **Ekle** düğmesine tıklayın.

        1. **Sertifika Ekle** bölümüne aşağıdaki bilgileri girin:
            - Benzersiz bir sertifika adı girin.
            - Seçin  **_RootCA.pem_**  oluşturduğunuz dosya.
            - Tamamlandığında **Kaydet** düğmesine tıklayın.

        ![Sertifika ekleme](./media/tutorial-group-enrollments/add-certificate.png)

        1. Yeni oluşturulan sertifikayı seçin:
            - **Doğrulama Kodu Oluştur**'a tıklayın. Oluşturulan kodu kopyalayın.
            - Çalıştırma **3. adım**. Girin _doğrulama kodu_ veya, çalışan bir PowerShell penceresinde yapıştırmak için sağ tıklatın.  **Enter**'a basın.
            - Yeni oluşturulan seçin  **_verifyCert4.pem_**  Azure portalında dosya. Tıklatın **doğrulayın**.

            ![Sertifika doğrulama](./media/tutorial-group-enrollments/validate-certificate.png)

1. Son çalıştırarak **4. adım** ve **adım 5** cihaz sertifikaları ve temizleme kaynakları oluşturmak için.

> [!NOTE]
> Cihaz sertifikaları oluştururken yalnızca küçük harf alfasayısal ve kısa çizgi, aygıt adı kullandığınızdan emin olun.
>


## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

1. Bir komut istemi açın. Java SDK kod örneklerini GitHub depoyu kopyalama:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. İndirilen kaynak kodunda örnek klasörüne gidin: **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_**. **_/src/main/java/samples/com/microsoft/azure/sdk/iot/ServiceEnrollmentGroupSample.java_** adlı dosyayı istediğiniz düzenleyicide açıp aşağıdaki ayrıntıları ekleyin:

    1. Portaldan sağlama hizmetinize ait `[Provisioning Connection String]` bilgisini aşağıdaki şekilde ekleyin:

        1. [Azure portalında](https://portal.azure.com) sağlama hizmetinize gidin. 

        1. **Paylaşılan erişim ilkeleri**'ni açıp *EnrollmentWrite* iznine sahip bir ilke seçin.
    
        1. **Birincil anahtar bağlantı dizesini** kopyalayın. 

            ![Sağlama bağlantısı dizesini portaldan alma](./media/tutorial-group-enrollments/provisioning-string.png)  

        1. **_ServiceEnrollmentGroupSample.java_** adlı örnek kod dosyasında `[Provisioning Connection String]` yerine **Birincil anahtar bağlantı dizesini** yazın.

            ```java
            private static final String PROVISIONING_CONNECTION_STRING = "[Provisioning Connection String]";
            ```

    1. Açık  **_RootCA.pem_**  dosyasını bir metin düzenleyicisinde. **Root Cert** değerini aşağıda gösterilen şekilde **PUBLIC_KEY_CERTIFICATE_STRING** parametresine atayın:

        ```java
        private static final String PUBLIC_KEY_CERTIFICATE_STRING =
                "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
        ```
 
    1. [Azure portalında](https://portal.azure.com) sağlama hizmetinizle bağlantılı IoT hub sayfasına gidin. Hub'ın **Özet** sayfasını açıp **Ana bilgisayar adı**'nı kopyalayın. Bu **Ana bilgisayar adı** değerini *IOTHUB_HOST_NAME* parametresine atayın.

        ```java
        private static final String IOTHUB_HOST_NAME = "[Host name].azure-devices.net";
        ```

    1. Örnek kodu inceleyin. Bu kod, X.509 cihazları için bir grup kaydı oluşturup bu kaydı güncelleştirir, sorgular ve siler. Portal kaydının başarılı olduğundan emin olmak için _ServiceEnrollmentGroupSample.java_ dosyasının en sonunda bulunan aşağıdaki kod satırlarını geçici olarak açıklama satırı yapın:

        ```java
        // ************************************** Delete info of enrollmentGroup ***************************************
        System.out.println("\nDelete the enrollmentGroup...");
        provisioningServiceClient.deleteEnrollmentGroup(enrollmentGroupId);
        ```

    1. _ServiceEnrollmentGroupSample.java_ dosyasını kaydedin. 

1. Bir komut penceresi açıp **_azure-iot-sdk-java/provisioning/provisioning-samples/service-enrollment-group-sample_** klasörüne gidin.

1. Şu komutu kullanarak örnek kodu derleyin:

    ```cmd\sh
    mvn install -DskipTests
    ```

1. Komut penceresinde aşağıdaki komutları çalıştırarak örneği çalıştırın:

    ```cmd\sh
    cd target
    java -jar ./service-enrollment-group-sample-{version}-with-deps.jar
    ```

1. Kaydın başarılı olup olmadığını görmek için çıktı penceresini izleyin.

    ![başarılı kaydı](./media/tutorial-group-enrollments/enrollment.png) 

1. Azure portalında sağlama hizmetinize gidin. **Kayıtları yönetme**'ye tıklayın. X.509 cihaz grubunun **Kayıt Grupları** bölümünde, otomatik olarak oluşturulmuş bir *GRUP ADI* altında göründüğüne dikkat edin. 


## <a name="simulate-the-device"></a>Cihazı benzetme

1. Cihaz Sağlama Hizmeti özet dikey penceresinde **Genel Bakış**'ı seçip _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini not edin.

    ![Hizmet bilgileri](./media/tutorial-group-enrollments/extract-dps-endpoints.png)

1. Bir komut istemi açın. Örnek proje klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Kayıt grubu bilgilerini aşağıdaki biçimde girin:

    - `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` girişini düzenleyerek önceden not ettiğiniz _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini girin. Açık,  **_{deviceName}-public.pem_**  dosya ve bu değeri olarak dahil, _istemci sertifikası_. Açık,  **_{deviceName}-all.pem_**  dosya ve metinden kopyalayın _---başlangıç özel anahtarı---_ için _---bitiş özel anahtarı---_.  Bu olarak kullanmak, _istemci sertifika özel anahtar_.

        ```java
        private static final String idScope = "[Your ID scope here]";
        private static final String globalEndpoint = "[Your Provisioning Service Global Endpoint here]";
        private static final ProvisioningDeviceClientTransportProtocol PROVISIONING_DEVICE_CLIENT_TRANSPORT_PROTOCOL = ProvisioningDeviceClientTransportProtocol.HTTPS;
        private static final String leafPublicPem = "<Your Public PEM Certificate here>";
        private static final String leafPrivateKey = "<Your Private PEM Key here>";
        ```

        - Sertifikanızı ve anahtarınızı dahil etmek için aşağıdaki biçimi kullanın:
            
            ```java
            private static final String leafPublicPem = "-----BEGIN CERTIFICATE-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "-----END CERTIFICATE-----\n";
            private static final String leafPrivateKey = "-----BEGIN PRIVATE KEY-----\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                "XXXXXXXXXX\n" +
                "-----END PRIVATE KEY-----\n";
            ```

1. Örneği derleyin. Hedef klasöre gidin ve oluşturulan jar dosyasını ayıklayın.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

    ![Başarılı kayıt](./media/tutorial-group-enrollments/registration.png)

1. Portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. X.509 sanal cihazının hub'a başarıyla sağlanması durumunda, cihaz kimliği **Device Explorer** dikey penceresinde *DURUM* değeri **etkinleştirildi** olarak gösterilir. Unutmayın;dikey pencereyi, örnek cihaz uygulamasını çalıştırmadan önce açtıysanız en üstteki **Yenile** düğmesine tıklamanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/tutorial-group-enrollments/hub-registration.png) 


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneği üzerinde çalışmaya ve inceleme yapmaya devam etmeyi planlıyorsanız bu Hızlı Başlangıç’ta oluşturulan kaynakları silmeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. Hizmetinizin **Kayıtları Yönetme** dikey penceresini açıp **Bireysel Kayıtlar** sekmesine tıklayın. Bu Hızlı Başlangıç adımlarını kullanarak kaydettiğiniz cihazın *KAYIT KİMLİĞİ* değerini seçip en üstte bulunan **Sil** düğmesine tıklayın. 
1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. Hub'ınızın **IoT Cihazları** dikey penceresini açıp bu Hızlı Başlangıç adımlarını kullanarak kaydettiğiniz cihazın *CİHAZ KİMLİĞİ* değerini seçip en üstte bulunan **Sil** düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, sanal bir X.509 cihaz Windows makinenizde oluşturulur ve Azure IOT Hub cihaz hizmet sağlama ve kayıt gruplarını kullanarak IOT hub'ınıza sağlanan. X.509 Cihazınızı hakkında daha fazla bilgi için aygıt kavramlara devam edin. 

> [!div class="nextstepaction"]
> [IOT Hub cihaz sağlama hizmeti aygıt kavramları](concepts-device.md)
