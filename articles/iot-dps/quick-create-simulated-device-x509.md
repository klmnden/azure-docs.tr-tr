---
title: C kullanarak Azure IOT Hub'a sanal bir X.509 cihazı sağlama | Microsoft Docs
description: Azure Hızlı Başlangıcı - Azure IoT Hub Cihazı Sağlama Hizmeti için C cihaz SDK'sını kullanarak sanal bir X.509 cihazı oluşturma ve sağlama
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 12/20/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 484b82b79d796536a2c9a527b42e90f4e37c7bda
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="create-and-provision-an-x509-simulated-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>IoT Hub Cihazı Sağlama Hizmeti için C cihaz SDK'sını kullanarak bir X.509 sanal cihazı oluşturma ve sağlama
[!INCLUDE [iot-dps-selector-quick-create-simulated-device-x509](../../includes/iot-dps-selector-quick-create-simulated-device-x509.md)]

Bu adımlar, Windows işletim sistemi çalıştıran geliştirme makinenizde X.509 cihazının simülasyonunu yapmayı ve örnek kodlar kullanarak bu sanal cihazı Cihaz Sağlama Hizmeti ve IoT hub’ınızla bağlamayı gösterir. 

Devam etmeden önce [IoT Hub Cihazı Sağlama Hizmetini Azure portalıyla ayarlama](./quick-setup-auto-provision.md) bölümünde bulunan adımları tamamladığınızdan emin olun.

[!INCLUDE [IoT DPS basic](../../includes/iot-dps-basic.md)]

<a id="setupdevbox"></a>

## <a name="prepare-the-development-environment"></a>Geliştirme ortamını hazırlama 

1. Makinenizde Visual Studio 2015 veya [Visual Studio 2017](https://www.visualstudio.com/vs/) olduğundan emin olun. Visual Studio yüklemenizde ["C++ ile masaüstü geliştirme"](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) iş yükünün etkinleştirilmiş olması gerekir.

2. [CMake derleme sistemini](https://cmake.org/download/) indirin ve yükleyin. Yükleme **öncesinde**`cmake` makinenize "C++ ile masaüstü geliştirme" iş yüküne sahip bir Visual Studio uygulamasının yüklenmiş olması önemlidir. 

3. `git` uygulamasının makinenizde yüklü olduğundan ve komut penceresinden erişilebilir ortam değişkenlerine eklendiğinden emin olun. Yüklenecek `git` araçlarının son sürümleri için [Software Freedom Conservancy’nin Git istemci araçlarına](https://git-scm.com/download/) bakın. Bunlara yerel Git deponuzla etkileşim kurmak için kullanabileceğiniz bir komut satırı uygulaması olan **Git Bash** dahildir. 

4. Bir komut istemi veya Git Bash’i açın. Cihaz benzetim kod örneği için GitHub deposunu kopyalayın:
    
    ```cmd
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

5. CMake oluşturma işlemi için bu GitHub deposunun yerel kopyasında bir klasör oluşturun. 

    ```cmd
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

6. Aşağıdaki komutu çalıştırarak sağlama istemcisi için Visual Studio çözümü oluşturun.

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```
    
    `cmake`, C++ derleyicinizi bulamazsa yukarıdaki komutu çalıştırırken derleme hatalarıyla karşılaşabilirsiniz. Bu durumda bu komutu [Visual Studio komut isteminde](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs) çalıştırmayı deneyin. 


<a id="portalenroll"></a>

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Cihaz sağlama hizmetinde bir cihaz kayıt girişi oluşturma

1. `azure_iot_sdks.sln` adlı *cmake* klasöründe oluşturulan çözümü açın ve Visual Studio'da derleyin.

2. **Sağlama\_Araçlar** klasörünün altında **dice\_device\_enrollment** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**'yı seçin. Çözümü çalıştırın. Çıktı penceresinde, istendiğinde tek kayıt için **i** girin. Çıktı penceresi, sanal cihazınız için yerel olarak oluşturulmuş X.509 sertifikasını görüntüler. *-----BEGIN CERTIFICATE-----* ile başlayıp ilk *-----END PUBLIC KEY-----* ile biten çıktıyı panoya kopyalayın ve bu iki satırı da dahil ettiğinizden emin olun. Yalnızca çıktı penceresindeki ilk sertifikayı kopyalamanız gerektiğini unutmayın.
 
3. Windows makinenizde **_X509testcert.pem_** adlı bir dosya oluşturun, dosyayı dilediğiniz düzenleyicide ve panonun içeriğini bu dosyaya kopyalayın. Dosyayı kaydedin. 

4. Azure portalında oturum açın, sol taraftaki menüden **Tüm kaynaklar**’a tıklayın ve sağlama hizmetinizi açın.

4. Hizmetinizin **Kayıtları yönetme** dikey penceresini açın. **Bireysel Kayıtlar** sekmesini seçin ve üstteki **Ekle** düğmesine tıklayın. 

5. **Kayıt listesi girişi ekle** bölümüne aşağıdaki bilgileri girin:
    - Kimlik onay *Mekanizması* olarak **X.509**'u seçin.
    - *Sertifika .pem veya .cer dosyası*'nın altında, önceki adımlarda *Dosya Gezgini* pencere öğesi kullanılarak oluşturulmuş **_X509testcert.pem_** sertifika dosyasını seçin.
    - İsteğe bağlı olarak, aşağıdaki bilgileri sağlayabilirsiniz:
        - Sağlama hizmetinizle bağlanacak IoT hub'ını seçin.
        - Benzersiz bir cihaz kimliği girin. Cihazınızı adlandırırken gizli veriler kullanmaktan kaçının. 
        - **Başlangıç cihaz ikizi durumu** alanını cihaz için istenen başlangıç yapılandırmasına göre güncelleştirin.
    - Tamamlandığında **Kaydet** düğmesine tıklayın. 

    ![Portal dikey penceresinde X.509 cihazı kayıt bilgilerini girme](./media/quick-create-simulated-device-x509/enter-device-enrollment.png)  

   Kayıt başarıyla tamamlandığında, X.509 cihazınız **Bireysel Kayıtlar** sekmesindeki *Kayıt Kimliği* sütununun altında *riot-device-cert* olarak gösterilir. 



<a id="firstbootsequence"></a>

## <a name="simulate-first-boot-sequence-for-the-device"></a>Cihazın ilk önyükleme sırasını benzetim olarak çalıştırma

1. Azure Portal'da Cihaz Sağlama hizmetiniz için **Genel Bakış** dikey penceresini seçin ve **_Kimlik Kapsamı_** değerini not alın.

    ![Portal dikey penceresinden DPS uç nokta bilgilerini ayıklama](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Makinenizdeki Visual Studio'da, **Sağlama\_Örnekler** klasörünüzün altında **prov\_dev\_client\_sample** adlı örnek projeye gidin ve **prov\_dev\_client\_sample.c** dosyasını açın.

3. `id_scope` değişkenine _Kimlik kapsamı_ değerini atayın. 

    ```c
    static const char* id_scope = "[ID Scope]";
    ```

4. Aynı dosyadaki **main()** işlevinin **SECURE_DEVICE_TYPE** parametresinin X.509 olarak ayarlandığından emin olun.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

   Varsa `hsm_type = SECURE_DEVICE_TYPE_TPM;` deyimini açıklama satırı yapın veya silin. 

5. **prov\_dev\_client\_sample** projesine sağ tıklayın ve **Başlangıç Projesi Olarak Ayarla**’yı seçin. Örnek uygulamayı çalıştırın. IoT hub bilgilerinizi almak için cihaz önyüklemesi ve Cihaz Sağlama Hizmetine bağlanma benzetimi gerçekleştiren iletilere dikkat edin. Hub'ınızla kaydın başarılı olduğunu bildiren iletiyi bulun: *Hizmetten alınan Kayıt Bilgileri: huburl'niz!*. İstendiğinde pencereyi kapatın.

6. Portalda, sağlama hizmetinize bağlı olan IoT hub'ına gidin ve **IoT Cihazları** dikey penceresini açın. X.509 sanal cihazının hub'a başarıyla sağlanması durumunda, cihaz kimliği **IoT Cihazları** dikey penceresinde *DURUM* değeri **etkinleştirildi** olarak gösterilir. Unutmayın;dikey pencereyi, örnek cihaz uygulamasını çalıştırmadan önce açtıysanız en üstteki **Yenile** düğmesine tıklamanız gerekebilir. 

    ![Cihaz IOT hub'da kayıtlı](./media/quick-create-simulated-device/hub-registration.png) 

    Cihazınız için *başlangıç cihaz ikizi durumu* ayarının kayıt girişindeki varsayılan değerini değiştirdiyseniz istenen ikili durumu hub'dan çekerek ona göre hareket edebilir. Daha fazla bilgi için bkz. [IoT Hub'ındaki cihaz ikizlerini kavrama ve kullanma](../iot-hub/iot-hub-devguide-device-twins.md).


> [!IMPORTANT]
> Bu Hızlı Başlangıçtaki adımlarda aşağıdaki değişiklikleri gerçekleştirerek X.509 cihazları için *Grup Kaydı* da gerçekleştirebilirsiniz:
>    1. [Windows geliştirme ortamı ayarlama](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md#windows) kılavuzundaki **WebSockets** bölümünde bulunan adımları uygulayarak Windows makinenizi varsayılan **SChannel** yerine **OpenSSL** kitaplığını kullanacak şekilde yapılandırın. Linux makinelerin varsayılan olarak OpenSSL kitaplığını kullandığını unutmayın. 
>    2. Yukarıdaki [Cihaz sağlama hizmetinde bir cihaz kayıt girişi oluşturma](#portalenroll) bölümünün 2. adımında grup kaydı için **g** değerini girin.
>    3. [Aynı bölümün](#portalenroll) 4. ve 5. adımlarında **Kayıt Grupları**'nı seçip grup girişi için gerekli bilgileri girin.  
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
