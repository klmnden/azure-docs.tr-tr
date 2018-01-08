---
title: "Java kullanarak Azure IOT Hub'a sanal bir X.509 cihazı sağlama | Microsoft Belgeleri"
description: "Azure Hızlı Başlangıcı - IoT Hub Cihazı Sağlama Hizmeti için Java cihaz SDK'sını kullanarak sanal bir X.509 cihazı oluşturma ve sağlama"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 12/21/2017
ms.topic: quickstart
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: java
ms.custom: mvc
ms.openlocfilehash: 508df6e7c677d87c6b893519c59b02880e16d7ec
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="create-and-provision-a-simulated-x509-device-using-java-device-sdk-for-iot-hub-device-provisioning-service"></a>IoT Hub Cihazı Sağlama Hizmeti için Java cihaz SDK'sını kullanarak sanal bir X.509 cihazı oluşturma ve sağlama
> [!div class="op_single_selector"]
> * [C](quick-create-simulated-device-x509.md)
> * [Java](quick-create-simulated-device-x509-java.md)
> * [C#](quick-create-simulated-device-x509-csharp.md)
> * [Python](quick-create-simulated-device-x509-python.md)

Bu adımlar, Windows işletim sistemi çalıştıran geliştirme makinenizde X.509 cihazının simülasyonunu yapmayı ve örnek kodlar kullanarak bu sanal cihazı Cihaz Sağlama Hizmeti ve IoT hub’ınızla bağlamayı gösterir. 

Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun.


## <a name="prepare-the-environment"></a>Ortamı hazırlama 

1. Makinenizde [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) uygulamasının yüklü olduğundan emin olun.

1. [Maven](https://maven.apache.org/install.html)'ı indirip yükleyin.

1. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

1. Bir komut istemi açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-java.git --recursive
    ```

1. Sertifika oluşturucu projesine gidin ve projeyi derleyin. 

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-tools/provisioning-x509-cert-generator
    mvn clean install
    ```

1. Hedef klasöre gidin ve oluşturulan jar dosyasını ayıklayın.

    ```cmd/sh
    cd target
    java -jar ./provisioning-x509-cert-generator-{version}-with-deps.jar
    ```

1. Kurulumunuza göre aşağıdaki yöntemlerin birini kullanarak kayıt bilgilerini oluşturun:

    - **Bireysel kayıt**:

        1. _Do you want to input common name_ (Ortak ad girmek istiyor musunuz) istemi için **N** girin. `Client Cert` çıktısının *-----BEGIN CERTIFICATE-----* satırından *-----END CERTIFICATE-----* satırına kadar olan bölümü panoya kopyalayın.

            ![Bireysel sertifika oluşturucu](./media/java-quick-create-simulated-device-x509/individual.png)

        1. Windows makinenizde **_X509individual.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin.

        1. _Do you want to input Verification Code_ (Doğrulama Kodu girmek istiyor musunuz?) isteminde **N** girin ve Hızlı Başlangıcın ilerleyen bölümlerinde kullanmak üzere program çıktısını açık tutun. _Client Cert_ (İstemci Sertifikası) ve _Client Cert Private Key_ (İstemci Sertifikası Özel Anahtarı) değerlerini not edin.
    
    - **Kayıt grupları**:

        1. _Do you want to input common name_ (Ortak ad girmek istiyor musunuz) istemi için **N** girin. `Root Cert` çıktısının *-----BEGIN CERTIFICATE-----* satırından *-----END CERTIFICATE-----* satırına kadar olan bölümü panoya kopyalayın.

            ![Grup sertifikası oluşturucu](./media/java-quick-create-simulated-device-x509/group.png)

        1. Windows makinenizde **_X509group.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin.

        1. _Do you want to input Verification Code_ (Doğrulama Kodu girmek istiyor musunuz?) isteminde **Y** girin ve Hızlı Başlangıcın ilerleyen bölümlerinde kullanmak üzere programı açık tutun. _Client Cert_ (İstemci Sertifikası), _Client Cert Private Key_ (İstemci Sertifikası Özel Anahtarı), _Signer Cert_ (İmzalayan Sertifikası) ve _Root Cert_ (Kök Sertifika) değerlerini not edin.


## <a name="create-a-device-enrollment-entry"></a>Cihaz kaydı girişi oluşturma

1. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sağlama hizmetinizi açın.

1. Kurulumunuza göre aşağıdaki yöntemlerin birini kullanarak kayıt bilgilerini girin:

    - **Bireysel kayıt**: 

        1. Cihaz Sağlama Hizmeti özet dikey penceresinde, **Kayıtları yönet**’i seçin. **Tek Tek Kayıtlar** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. 

        1. **Kayıt listesi girişi ekle** bölümüne aşağıdaki bilgileri girin:
            - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
            - *Sertifika .pem veya .cer dosyası*'nın altında, önceki adımlarda *Dosya Gezgini* pencere öğesi kullanılarak oluşturulmuş **_X509individual.pem_** sertifika dosyasını seçin.
            - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
                - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
                - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
                - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
            - Tamamlandığında **Kaydet** düğmesine tıklayın. 

        ![Portal dikey penceresinde X.509 cihazı kayıt bilgilerini girme](./media/java-quick-create-simulated-device-x509/enter-device-enrollment.png)  

       Kayıt başarıyla tamamlandığında, X.509 cihazınız *Bireysel Kayıtlar* sekmesindeki *Kayıt Kimliği* sütununun altında **microsoftriotcore** olarak gösterilir. 

    - **Kayıt grupları**: 

        1. Cihaz Sağlama Hizmeti özet dikey penceresinde **Sertifikalar**'ı seçip üstteki **Ekle** düğmesine tıklayın.

        1. **Sertifika Ekle** bölümüne aşağıdaki bilgileri girin:
            - Benzersiz bir sertifika adı girin.
            - Önceki adımlarda oluşturduğunuz **_X509group.pem_** dosyasını seçin.
            - Tamamlandığında **Kaydet** düğmesine tıklayın.

        ![Sertifika ekleme](./media/java-quick-create-simulated-device-x509/add-certificate.png)

        1. Yeni oluşturulan sertifikayı seçin:
            - **Doğrulama Kodu Oluştur**'a tıklayın. Oluşturulan kodu kopyalayın.
            - Çalışan _provisioning-x509-cert-generator_ pencerenize _doğrulama kodunu_ girin veya sağ tıklayarak yapıştırın.  **Enter**'a basın.
            - `Verification Cert` çıktısının *-----BEGIN CERTIFICATE-----* satırından *-----END CERTIFICATE-----* satırına kadar olan bölümü panoya kopyalayın.
            
                ![Doğrulama oluşturucu](./media/java-quick-create-simulated-device-x509/validation-generator.png)

            - Windows makinenizde **_X509validation.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin.
            - Azure portalında **_X509validation.pem_** dosyasını seçin. **Doğrula**'ya tıklayın.

            ![Sertifika doğrulama](./media/java-quick-create-simulated-device-x509/validate-certificate.png)

        1. **Kayıtları yönetme**'yi seçin. **Kayıt Grupları** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın.
            - Benzersiz bir grup adı girin.
            - Önceki adımlarda oluşturulan benzersiz sertifika adını seçin
            - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
                - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
                - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.

        ![Portal dikey penceresinde X.509 grup kaydı bilgilerini girme](./media/java-quick-create-simulated-device-x509/enter-group-enrollment.png)

        Kayıt başarılı olduğunda X.509 cihaz grubunuz *Kayıt Grupları* sekmesinin *Grup Adı* sütununda görünür.


## <a name="simulate-the-device"></a>Cihazı benzetme

1. Cihaz Sağlama Hizmeti özet dikey penceresinde **Genel Bakış**'ı seçip _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini not edin.

    ![Hizmet bilgileri](./media/java-quick-create-simulated-device-x509/extract-dps-endpoints.png)

1. Bir komut istemi açın. Örnek proje klasörüne gidin.

    ```cmd/sh
    cd azure-iot-sdk-java/provisioning/provisioning-samples/provisioning-X509-sample
    ```

1. Kurulumunuza göre aşağıdaki yöntemlerin birini kullanarak kayıt bilgilerini girin:

    - **Bireysel kayıt**: 

        1. `/src/main/java/samples/com/microsoft/azure/sdk/iot/ProvisioningX509Sample.java` girişini düzenleyerek önceden not ettiğiniz _Kimlik Kapsamı_ ve _Sağlama Hizmeti Genel Uç Noktası_ değerlerini girin. Ayrıca önceden not ettiğiniz _Client Cert_ ve _Client Cert Private Key_ değerlerini de dahil edin.

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

    - **Kayıt grupları**: 

        1. Yukarıdaki **Bireysel kayıt** talimatlarını uygulayın.

        1. Aşağıdaki kod satırlarını `main` işlevinin başına ekleyin.
        
            ```java
            String intermediatePem = "<Your Signer Certificate here>";          
            String rootPem = "<Your Root Certificate here>";
                
            signerCertificates.add(intermediatePem);
            signerCertificates.add(root);
            ```
    
            - Sertifikalarınızı dahil etmek için aşağıdaki biçimi kullanın:
        
                ```java
                String intermediatePem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                String rootPem = "-----BEGIN CERTIFICATE-----\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "+XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX\n" +
                    "-----END CERTIFICATE-----\n";
                ```

1. Örneği derleyin. Hedef klasöre gidin ve oluşturulan jar dosyasını ayıklayın.

    ```cmd/sh
    mvn clean install
    cd target
    java -jar ./provisioning-x509-sample-{version}-with-deps.jar
    ```

1. Portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **Device Explorer** dikey penceresini açın. X.509 sanal cihazının hub'a başarıyla sağlanması durumunda, cihaz kimliği **Device Explorer** dikey penceresinde *DURUM* değeri **etkinleştirildi** olarak gösterilir. Unutmayın;dikey pencereyi, örnek cihaz uygulamasını çalıştırmadan önce açtıysanız en üstteki **Yenile** düğmesine tıklamanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/java-quick-create-simulated-device-x509/hub-registration.png) 

> [!NOTE]
> Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'ındaki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).
>


## <a name="clean-up-resources"></a>Kaynakları temizleme

Cihaz istemci örneği üzerinde çalışmaya ve inceleme yapmaya devam etmeyi planlıyorsanız bu Hızlı Başlangıç’ta oluşturulan kaynakları silmeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın:

1. Makinenizde cihaz istemci örnek çıktı penceresini kapatın.
1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından Cihaz Sağlama hizmetini seçin. Hizmetinizin **Kayıtları Yönetme** dikey penceresini açıp **Bireysel Kayıtlar** sekmesine tıklayın. Bu Hızlı Başlangıç adımlarını kullanarak kaydettiğiniz cihazın *KAYIT KİMLİĞİ* değerini seçip en üstte bulunan **Sil** düğmesine tıklayın. 
1. Azure portalında sol taraftaki menüden **Tüm kaynaklar**’ı ve ardından IoT hub’ınızı seçin. Hub'ınızın **IoT Cihazları** dikey penceresini açıp bu Hızlı Başlangıç adımlarını kullanarak kaydettiğiniz cihazın *CİHAZ KİMLİĞİ* değerini seçip en üstte bulunan **Sil** düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu Hızlı Başlangıçta, Windows makinenizde bir X.509 sanal cihazı oluşturdunuz ve portaldaki Azure IOT Hub Cihaz Sağlama Hizmeti ile IoT hub'ınıza sağladınız. X.509 cihazınızı programlı bir şekilde kaydetmeyi öğrenmek için X.509 cihazlarının programlı kaydının yer aldığı Hızlı Başlangıç adımlarına gidin. 

> [!div class="nextstepaction"]
> [Azure Hızlı Başlangıcı - X.509 cihazlarını Azure IoT Hub Cihaz Sağlama Hizmeti'ne kaydetme](quick-enroll-device-x509-java.md)
