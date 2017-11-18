---
title: "Azure IOT Hub içinde X.509 güvenlik Öğreticisi | Microsoft Docs"
description: "Sanal bir ortamda Azure IOT hub'ınızdaki X.509 tabanlı güvenlik kullanmaya başlayın."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/10/2017
ms.author: dkshir
ms.openlocfilehash: 93f9099d7aef1161f7789e7b21a88a8691cb2a8e
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/18/2017
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Azure IOT hub'ınızdaki X.509 güvenliği ayarlama

Bu öğretici, Azure IOT hub'ı kullanarak güvenli hale getirmek için gereken adımları benzetim *X.509 sertifikası kimlik doğrulaması*. Çizim amacıyla, açık kaynak aracı OpenSSL sertifikaları Windows makinenizde yerel olarak oluşturmak için nasıl kullanılacağını göstereceğiz. Bu öğretici yalnızca test amacıyla kullanmanızı öneririz. Üretim ortamı için sertifikalarını satın alın bir *kök sertifika yetkilisi (CA)*. 

## <a name="prerequisites"></a>Ön koşullar
Bu öğretici, aşağıdaki kaynaklara hazır olmasını gerektirir:

- Azure aboneliğiniz ile bir IOT hub oluşturdunuz. Bkz: [portal üzerinden IOT hub oluşturma](iot-hub-create-through-portal.md) ayrıntılı adımlar için. 
- Sahip olduğunuz [Visual Studio 2015 veya Visual Studio 2017](https://www.visualstudio.com/vs/) makinenize yüklü. 

<a id="getcerts"></a>

## <a name="get-x509-ca-certificates"></a>X.509 CA sertifikaları alma
İle başlamak X.509 Sertifika tabanlı güvenlik IOT Hub'ında gerektirir bir [X.509 Sertifika zinciri](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), tüm ara sertifikaların Yaprak sertifikası kadar yanı sıra kök sertifikası içerir. 

Sertifikalarınızı almak için aşağıdaki yollardan birini seçebilirsiniz:
- X.509 sertifikaları satın bir *kök sertifika yetkilisi (CA)*. Bu, üretim ortamları için önerilir.
VEYA,
- Üçüncü taraf bir araç kullanarak kendi X.509 sertifikaları oluşturma [OpenSSL](https://www.openssl.org/). Bu, test ve geliştirme amaçlarıyla ince olacaktır. Başlıklı bölümlere *X.509 sertifikaları oluşturma* ve *oluşturma X.509 Sertifika zinciri* makalede [X.509 sertifikalarını oluşturmak için PowerShell kullanma](iot-hub-security-x509-create-certificates.md) , yol sertifikaları oluşturmak için bir örnek PowerShell komut dosyası OpenSSL ve PowerShell kullanarak. Kullanmayı tercih ederseniz, **Bash** Kabuk PowerShell yerine, lütfen ilgili bölümlerine bakın [yönetme CA sertifikaları örnek](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md). Bu öğreticinin geri kalanını bu ayarlanan OpenSSL ortamı kullanacağı *nasıl* Kılavuzu, Azure IOT Hub uçtan uca X.509 güvenliğinde size yol.


<a id="registercerts"></a>

## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>IOT hub'ınıza X.509 CA sertifikaları kaydetme

Bu adımlar, portal üzerinden IOT hub'ınıza yeni bir sertifika yetkilisi ekleme gösterir.

1. Azure portalında IOT hub'ına gidin ve açmak **ayarları** > **sertifikaları** menüsü. 
2. Tıklatın **Ekle** yeni bir sertifika eklemek için.
3. Sertifikanızı için kolay bir görünen ad girin. Adlı kök sertifika dosyasını seçin *RootCA.cer* makinenizden önceki bölümde oluşturulan. **Karşıya Yükle**'ye tıklayın.
4. Sertifikanızı başarıyla karşıya yüklendiğini bildirimi aldıktan sonra tıklatın **kaydetmek**.

    ![Sertifikayı karşıya yükleme](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Bu, sertifikanızı gösterir **sertifika Explorer** listesi. Not **durum** Bu sertifika *Unverified*.

5. Önceki adımda eklediğiniz sertifika'yı tıklatın.

6. İçinde **sertifika ayrıntıları** dikey penceresinde tıklatın **doğrulama kodu oluştur**.

7. Oluşturduğu bir **doğrulama kodu** sertifika sahipliği doğrulamak için. Kodunu panonuza kopyalayın. 

   ![Sertifika doğrulayın](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Şimdi, bu oturum açmanız gerekir *doğrulama kodu* X.509 CA sertifikanız ile özel anahtar ilişkilendirme ile hangi oluşturur imza. Örneğin, OpenSSL imzalama bu işlemi gerçekleştirmek için kullanılabilen araçlar vardır. Bu olarak bilinir [kanıtını](https://tools.ietf.org/html/rfc5280#section-3.1). Bizim örnek PowerShell komut dosyaları önceki bölümde kullandıysanız, başlıklı bölümde belirtilen komut dosyası çalıştırma [X.509 CA sertifikanızın kanıtını](iot-hub-security-x509-create-certificates.md#signverificationcode).
 
9. Yukarıdaki adım 8 portalında IOT hub'ınızı elde edilen imza karşıya yükleyin. İçinde **sertifika ayrıntıları** Azure portal dikey penceresinde gidin **doğrulama sertifikası .pem veya .cer dosyasını**, imza, örneğin seçin *VerifyCert4.cer*örnek PowerShell komutu kullanılarak oluşturulmuş _dosya Gezgini_ yanı sıra, simge.

10. Sertifika başarıyla yüklendikten sonra tıklatın **doğrula**. **Durum** sertifika değişikliklerinizi  **_doğrulandı_**  içinde **sertifikaları** dikey. Tıklatın **yenileme** varsa otomatik olarak güncelleştirmez.

   ![Sertifika doğrulama karşıya yükle](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  


<a id="createdevice"></a>

## <a name="create-an-x509-device-for-your-iot-hub"></a>IOT hub'ınız için bir X.509 cihaz oluşturma

1. Azure portalında, IOT hub'ın gidin **aygıt Explorer**.

2. Tıklatın **Ekle** yeni aygıt eklemek için. 

3. Kolay bir görünen ad için vermek **cihaz kimliği**seçip  **_X.509 CA imzalı_**  olarak **kimlik doğrulama türü**. **Kaydet** düğmesine tıklayın.

   ![X.509 cihaz Portalı'nda oluşturma](./media/iot-hub-security-x509-get-started/create-x509-device.png)



<a id="authenticatedevice"></a>

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>X.509 sertifikaları X.509 cihazınızla kimlik doğrulaması

X.509 Cihazınızı kimliğini doğrulamak için önce CA sertifikasını aygıtla imzalamanız gerekir. Yaprak aygıtların imzalama üretim araçları uygun şekilde etkinleştirilmiş olduğu üretim tesis sırasında normal olarak yapılır. Cihaz bir üreticiden gider gibi her bir üreticinin imzalama eylem bir ara sertifika zinciri içinde yakalanır. CA sertifikasını bir sertifika zinciri aygıtın Yaprak sertifikası son sonucudur. Bizim PowerShell komut dosyaları önceki bölümlerde kullanmakta olduğunuz sonra başlıklı bölümde belirtilen komut dosyası çalıştırabilirsiniz *oluşturma yaprak X.509 sertifikası cihazınız için* makalede [PowerShell komut dosyaları için CA tarafından imzalanmış X.509 sertifikalarını yönetmek](iot-hub-security-x509-create-certificates.md) bu işlem benzetimini yapmak için.

Ardından, IOT hub'ınız için kayıtlı X.509 aygıt benzetimini yapmak için bir C# uygulamasının nasıl oluşturulacağını göstereceğiz. Hub'ınıza sıcaklık ve nem değerlerini benzetimli aygıttan göndereceğiz. Bu öğreticide, yalnızca cihaz uygulaması oluşturacağız olduğunu unutmayın. Bu sanal aygıt tarafından gönderilen olaylarına yanıt olarak gönderir IOT hub'ı hizmet uygulaması oluşturmak için okuyucuların bir alıştırma olarak kalır. C# uygulaması makalede açıklanan PowerShell betikleri izlediğinizden varsayar [CA tarafından imzalanmış X.509 sertifikalarını yönetmek için PowerShell komut dosyaları](iot-hub-security-x509-create-certificates.md)

1. Visual Studio'da konsol uygulaması proje şablonunu kullanarak yeni bir Visual C# Windows Klasik Masaüstü projesi oluşturun. Proje adı **SimulateX509Device**.
   ![Visual Studio'da X.509 cihaz projesi oluşturma](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. Çözüm Gezgini'nde sağ **SimulateX509Device** proje ve ardından **NuGet paketlerini Yönet...** . NuGet Paket Yöneticisi penceresinde seçin **Gözat** arayın ve **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve Azure IOT cihaz SDK'sı NuGet paketi ve bağımlılıklarını bir başvuru eklenir.
   ![Visual Studio'da cihaz SDK'sı NuGet paketi ekleme](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. Aşağıdaki kod satırlarını en üstünde eklemek *Program.cs* dosyası:
    
    ```CSharp
        using Microsoft.Azure.Devices.Client;
        using Microsoft.Azure.Devices.Shared;
        using System.Security.Cryptography.X509Certificates;
    ```

4. Kod içinde aşağıdaki satırları ekleyin **Program** sınıfı:
    
    ```CSharp
        private static int MESSAGE_COUNT = 5;
        private const int TEMPERATURE_THRESHOLD = 30;
        private static String deviceId = "<your-device-id>";
        private static float temperature;
        private static float humidity;
        private static Random rnd = new Random();
    ```
   Önceki bölümde yerine kullandığınız kolay aygıt adı kullanmak _< your_device_id >_ yer tutucu.

5. Rastgele sayılar sıcaklık ve nem oluşturmak ve bu değerleri hub'ına göndermek için aşağıdaki işlevi ekleyin:
    ```CSharp
    static async Task SendEvent(DeviceClient deviceClient)
    {
        string dataBuffer;
        Console.WriteLine("Device sending {0} messages to IoTHub...\n", MESSAGE_COUNT);

        for (int count = 0; count < MESSAGE_COUNT; count++)
        {
            temperature = rnd.Next(20, 35);
            humidity = rnd.Next(60, 80);
            dataBuffer = string.Format("{{\"deviceId\":\"{0}\",\"messageId\":{1},\"temperature\":{2},\"humidity\":{3}}}", deviceId, count, temperature, humidity);
            Message eventMessage = new Message(Encoding.UTF8.GetBytes(dataBuffer));
            eventMessage.Properties.Add("temperatureAlert", (temperature > TEMPERATURE_THRESHOLD) ? "true" : "false");
            Console.WriteLine("\t{0}> Sending message: {1}, Data: [{2}]", DateTime.Now.ToLocalTime(), count, dataBuffer);

            await deviceClient.SendEventAsync(eventMessage);
        }
    }
    ```

6. Son olarak aşağıdaki kod satırlarını ekleyin **ana** işlevi, yer tutucular değiştirme _cihaz kimliği_, _-iot-hub-adınız_ ve  _Absolute-Path-to-Your-Device-PFX-File_ kurulumunuzu gerektirdiği.
    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "123");
        var auth = new DeviceAuthenticationWithX509Certificate("<device-id>", cert);
        var deviceClient = DeviceClient.Create("<your-iot-hub-name>.azure-devices.net", auth, TransportType.Amqp_Tcp_Only);

        if (deviceClient == null)
        {
            Console.WriteLine("Failed to create DeviceClient!");
        }
        else
        {
            Console.WriteLine("Successfully created DeviceClient!");
            SendEvent(deviceClient).Wait();
        }

        Console.WriteLine("Exiting...\n");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
   Bu kod, X.509 cihazınız için bağlantı dizesi oluşturarak IOT hub'ınıza bağlanır. Başarıyla kurulduktan sonra onu sonra sıcaklık ve nem olayları hub'ına gönderir ve yanıt için bekler. 
7. Bu uygulama erişen bu yana bir *.pfx* dosyası, bu yürütmek için ihtiyacınız *yönetici* modu. Visual Studio çözümü oluşturun. Yeni bir komut penceresi açın bir **yönetici**ve bu çözümü içeren klasöre gidin. Gidin *bin/Debug* çözüm klasördeki yolu. Uygulamayı çalıştırmak **SimulateX509Device.exe** gelen _yönetici_ komut penceresi. Cihazınızı başarıyla hub'a bağlanan ve olayların gönderilmesi görmeniz gerekir. 
   ![Cihaz uygulama çalıştırma](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="see-also"></a>Ayrıca bkz.
IOT çözümünüzün güvenliğini sağlama hakkında daha fazla bilgi için bkz:

* [IOT en iyi güvenlik uygulamaları][lnk-security-best-practices]
* [IOT güvenlik mimarisi][lnk-security-architecture]
* [IOT dağıtımınızın güvenliğini][lnk-security-deployment]

Daha fazla IOT hub'ı özelliklerini keşfetmek için bkz:

* [AI ile Azure IOT kenar sınır cihazları için dağıtma][lnk-iotedge]

[lnk-security-best-practices]: iot-hub-security-best-practices.md
[lnk-security-architecture]: iot-hub-security-architecture.md
[lnk-security-deployment]: iot-hub-security-deployment.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
