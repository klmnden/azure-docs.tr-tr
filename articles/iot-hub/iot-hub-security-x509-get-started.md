---
title: Azure IOT hub'ında X.509 güvenlik için öğretici | Microsoft Docs
description: Sanal bir ortamda Azure IOT hub'ınızdaki X.509 tabanlı güvenlik kullanmaya başlayın.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/10/2017
ms.openlocfilehash: 0bfb66f54ec09e86b46a41499211e93a0083e8d1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65779923"
---
# <a name="set-up-x509-security-in-your-azure-iot-hub"></a>Azure IOT hub'ınızdaki X.509 güvenliği

Bu öğreticide, Azure IOT hub'ı kullanarak güvenli hale getirmek için gereken adımlar benzetim *X.509 sertifika kimlik doğrulaması*. Gösterim amacıyla, OpenSSL açık kaynak aracı sertifikalar, Windows makinenizde yerel olarak oluşturmak için nasıl kullanılacağını göstereceğiz. Bu öğreticide yalnızca test amacıyla kullanmanızı öneririz. Üretim ortamı için sertifikaları satın almalıyım bir *kök sertifika yetkilisi (CA)* .

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticide, aşağıdaki kaynakları hazır olmasını gerektirir:

* Azure aboneliğinizde bir IOT hub oluşturdunuz. Bkz: [Portalı aracılığıyla IOT hub oluşturma](iot-hub-create-through-portal.md) ayrıntılı adımlar için.

* Sahip olduğunuz [Visual Studio 2017 veya Visual Studio 2019](https://www.visualstudio.com/vs/) makinenizde yüklü.

## <a name="get-x509-ca-certificates"></a>X.509 CA sertifikaları Al

IOT hub'ında X.509 sertifikası tabanlı güvenlik ile başlaması gerekir bir [X.509 Sertifika zinciri](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), tüm ara sertifikaları yaprak sertifikayı gönderinizi yanı sıra, kök sertifikasını içerir.

Sertifikalarınızı almak için aşağıdaki yollardan birini seçebilirsiniz:

* Satın X.509 sertifikasından bir *kök sertifika yetkilisi (CA)* . Bu, üretim ortamları için önerilir.

* Gibi bir üçüncü taraf araç kullanarak kendi X.509 sertifikaları oluşturma [OpenSSL](https://www.openssl.org/). Bu, test ve geliştirme amacıyla yeterli olur. Bkz: [yönetme test CA sertifikaları için örnekler ve öğreticiler](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) oluşturma hakkında bilgi için CA sertifikalarını PowerShell veya Bash kullanarak test edin. Bu öğreticinin geri kalanını yönergelerini takip ederek oluşturulan test CA sertifikaları kullanan [yönetme test CA sertifikaları için örnekler ve öğreticiler](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

* Oluşturmak bir [X.509 ara CA sertifikası](iot-hub-x509ca-overview.md#sign-devices-into-the-certificate-chain-of-trust) var olan bir kök CA sertifikası tarafından imzalanmış ve IOT Hub'ına yükleyin. Ara Sertifika yüklenmiş ve doğrulanmış, aşağıda belirtildiği gibi sonra aşağıda belirtilen bir kök CA sertifikası yerine kullanılabilir. OpenSSL gibi araçlar ([openssl isteği](https://www.openssl.org/docs/manmaster/man1/openssl-req.html) ve [openssl ca](https://www.openssl.org/docs/manmaster/man1/openssl-ca.html)) oluşturmak ve bir ara CA sertifikasını imzalamak için kullanılabilir.


## <a name="register-x509-ca-certificates-to-your-iot-hub"></a>X.509 CA sertifikalarını IOT hub'ınıza kaydedin

Bu adımlar Portalı aracılığıyla IOT hub'ınıza yeni bir sertifika yetkilisi ekleme gösterir.

1. Azure portalında, IOT hub'ınıza gidin ve açmak **ayarları** > **sertifikaları** menüsü.

2. Tıklayın **Ekle** yeni bir sertifika eklemek için.

3. Sertifikanız için kolay bir görünen ad girin. Adlı kök sertifika dosyasını seçmek *RootCA.cer* , makinenizde önceki bölümde oluşturulan. **Karşıya Yükle**'ye tıklayın.

4. Sertifikanızı başarıyla karşıya yüklendiğini bir bildirim alırsınız bitince **Kaydet**.

    ![Sertifikayı karşıya yükleme](./media/iot-hub-security-x509-get-started/add-new-cert.png)  

   Bu, sertifikanızın gösterir **sertifika Gezgini** listesi. Not **durumu** Bu sertifika *doğrulanmamış*.

5. Önceki adımda eklediğiniz sertifika tıklayın.

6. İçinde **sertifika ayrıntıları** dikey penceresinde tıklayın **doğrulama kodu oluştur**.

7. Oluşturur bir **doğrulama kodu** sertifika sahipliğini doğrulamak için. Kodunu panonuza kopyalayın.

   ![Sertifika doğrulama](./media/iot-hub-security-x509-get-started/verify-cert.png)  

8. Şimdi, bu oturum için ihtiyacınız *doğrulama kodu* , X.509 CA sertifika özel anahtar ilişkilendirme ile bir imza oluşturduğu. Örneğin, OpenSSL bu imzalama işlemi gerçekleştirmek kullanılabilen araçlar vardır. Bu olarak bilinir [kanıtını](https://tools.ietf.org/html/rfc5280#section-3.1). Adım 3 [yönetme test CA sertifikaları için örnekler ve öğreticiler](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) bir doğrulama kodu oluşturur.

9. Yukarıdaki adım 8 elde edilen imzasının Portalı'nda IOT hub'ınıza karşıya yükleyin. İçinde **sertifika ayrıntıları** Azure portalındaki dikey penceresine gidin **doğrulama sertifikası .pem veya .cer dosyası**, örneğin, imza seçin *VerifyCert4.cer*örnek PowerShell komutu kullanılarak oluşturulmuş _dosya Gezgini_ yanı sıra, simge.

10. Sertifika başarıyla karşıya yüklendikten sonra tıklayın **doğrulama**. **Durumu** sertifika değişikliklerinizin **_doğrulandı_** içinde **sertifikaları** dikey penceresi. Tıklayın **Yenile** varsa otomatik olarak güncelleştirilmez.

    ![Karşıya sertifika doğrulama](./media/iot-hub-security-x509-get-started/upload-cert-verification.png)  

## <a name="create-an-x509-device-for-your-iot-hub"></a>IOT hub'ınız için bir X.509 cihazı oluşturma

1. Azure portalında, IOT hub'ın gidin **gezginler > IOT cihazları** sayfası.

2. Tıklayın **+ Ekle** yeni bir cihaz eklemek için.

3. Kolay görünen adı verin **cihaz kimliği**seçip **_X.509 CA imzalı_** olarak **kimlik doğrulama türü**. **Kaydet**’e tıklayın.

   ![Portalda X.509 cihazı oluşturma](./media/iot-hub-security-x509-get-started/create-x509-device.png)

## <a name="authenticate-your-x509-device-with-the-x509-certificates"></a>X.509 sertifikalarıyla X.509 cihazınızın kimlik doğrulaması

X.509 cihazınızın kimlik doğrulaması için CA sertifikası ile bir cihaz ilk kez oturum gerekir. Yaprak cihazlar imzalama üretim araçları uygun şekilde etkinleştirilmiş olduğu, üretim tesisindeki normal olarak gerçekleştirilir. Cihaz bir üreticiden gider gibi her bir üreticinin imzalama eylem olarak bir ara sertifika zinciri içinde yakalanır. CA sertifikasını bir sertifika zinciri cihazın yaprak sertifikayı son sonucudur. Adım 4 [yönetme test CA sertifikaları için örnekler ve öğreticiler](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) bir cihaz sertifika oluşturur.

Ardından, IOT hub'ınız için kayıtlı X.509 cihazının simülasyonunu bir C# uygulaması oluşturulacağını göstereceğiz. Hub'ınıza sıcaklık ve nem değerlerini sanal CİHAZDAN göndereceğiz. Bu öğreticide unutmayın, yalnızca cihaz uygulama oluşturacağız. Okuyucular, bu sanal cihazı tarafından gönderilen olaylara yanıt olarak gönderecek olan IOT Hub hizmet uygulaması oluşturmak için bunu bir alıştırma olarak kalır. C# uygulaması altında verilen adımları izlediyseniz varsayar [yönetme test CA sertifikaları için örnekler ve öğreticiler](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md).

1. Visual Studio'da konsol uygulaması proje şablonunu kullanarak yeni bir Visual C# Windows Klasik Masaüstü projesi oluşturun. Projeyi adlandırın **SimulateX509Device**.

   ![Visual Studio'da X.509 cihaz projesi oluşturma](./media/iot-hub-security-x509-get-started/create-device-project.png)

2. Çözüm Gezgini'nde sağ **SimulateX509Device** proje ve ardından **NuGet paketlerini Yönet...** . NuGet Paket Yöneticisi penceresini seçin **Gözat** araması **microsoft.azure.devices.client**. Seçin **yükleme** yüklemek için **Microsoft.Azure.Devices.Client** paketini ve kullanım koşullarını kabul edin. Bu yordam, indirir, yükler ve Azure IOT cihaz SDK'sı NuGet paketi ve bağımlılıkları için bir başvuru ekler.

   ![Visual Studio'da cihaz SDK'sı NuGet paketi ekleme](./media/iot-hub-security-x509-get-started/device-sdk-nuget.png)

3. Aşağıdaki kod satırlarını üstüne ekleyin *Program.cs* dosyası:

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

     Önceki bölümde yerine kullandığınız cihaz kolay adı kullanmak _< your_device_id >_ yer tutucu.

5. Sıcaklık ve nem için rastgele sayılar oluşturma ve bu değerleri hub'a göndermek için aşağıdaki işlevi ekleyin:

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

6. Son olarak, aşağıdaki kod satırlarını ekleme **ana** işlevi, yer tutucuları değiştirmek _cihaz kimliği_, _-IOT-hub-adınız_ ve  _Absolute-Path-to-Your-Device-PFX-File_ kurulumunuzu gerektirdiği.

    ```CSharp
    try
    {
        var cert = new X509Certificate2(@"<absolute-path-to-your-device-pfx-file>", "1234");
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

   Bu kod, X.509 cihazınız için bağlantı dizesini oluşturarak IOT hub'ınıza bağlanır. Başarıyla bağlandıktan sonra ardından sıcaklık ve nem olay hub'ına gönderir ve kendi yanıt bekler. 
7. Bu uygulama erişen bu yana bir *.pfx* dosyası bu yürütme gerekebilir *yönetici* modu. Visual Studio çözümü oluşturun. Yeni bir komut penceresi açık bir **yönetici**ve bu çözümü içeren klasöre gidin. Gidin *bin/Debug* Çözüm klasörü içindeki yol. Uygulamayı çalıştırmak **SimulateX509Device.exe** gelen _yönetici_ komut penceresi. Cihazınızı başarıyla hub'ına bağlanmayı ve olay göndermeye görmeniz gerekir. 

   ![Cihaz uygulamasını çalıştırın](./media/iot-hub-security-x509-get-started/device-app-success.png)

## <a name="next-steps"></a>Sonraki adımlar

IOT çözümünüzün güvenliğini sağlama hakkında daha fazla bilgi için bkz:

* [IOT güvenlik en iyi uygulamalar](../iot-fundamentals/iot-security-best-practices.md)

* [IOT güvenlik mimarisi](../iot-fundamentals/iot-security-architecture.md)

* [IoT dağıtımınızın güvenliğini sağlama](../iot-fundamentals/iot-security-deployment.md)

Daha fazla IOT Hub'ın özelliklerini keşfetmek için bkz:

* [Yapay ZEKA, Azure IOT Edge ile uç cihazlarına dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
