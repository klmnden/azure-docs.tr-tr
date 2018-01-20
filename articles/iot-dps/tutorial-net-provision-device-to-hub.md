---
title: "Azure IOT Hub cihaz sağlama hizmeti (.NET) kullanarak bir cihazı sağlama | Microsoft Docs"
description: "Cihazınızı Azure IOT Hub cihaz sağlama hizmeti (.NET) kullanılarak tek bir IOT hub'ına sağlama"
services: iot-dps
keywords: 
author: msebolt
ms.author: v-masebo
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6919f962d853faa572ee7ad5d0cb9aeacd3bd2b6
ms.sourcegitcommit: 817c3db817348ad088711494e97fc84c9b32f19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2018
---
# <a name="enroll-the-device-to-an-iot-hub-using-the-azure-iot-hub-provisioning-service-client-net"></a>Azure IOT Hub sağlama hizmeti istemci (.NET) kullanılarak bir IOT hub'ına cihaz kaydetme

Önceki öğreticide, bir aygıtı Aygıt sağlama hizmete bağlanmak için ayarlama öğrendiniz. Bu öğreticide, tek bir IOT hub aygıtınıza sağlamak için bu hizmeti kullanmak her ikisini de kullanarak öğrenin  **_tek tek kayıt_**  ve  **_kayıt grupları_**. Bu öğretici şunların nasıl yapıldığını gösterir:

> [!div class="checklist"]
> * Cihaz kaydı
> * Cihaz Başlat
> * Cihaz kayıtlı doğrulayın

## <a name="prerequisites"></a>Önkoşullar

Devam etmeden önce Cihazınızı yapılandırdığınızdan emin olun ve kendi *donanım güvenlik modülü* öğreticide anlatıldığı gibi [Azure IOT Hub cihaz sağlama hizmeti ile sağlamak için bir aygıtı ayarlama](./tutorial-set-up-device.md).

* Visual Studio 2015 veya Visual Studio 2017

> [!NOTE]
> Visual Studio gerekli değildir. Yüklemesini [.NET](https://www.microsoft.com/net) yeterlidir ve Windows veya Linux'ta geliştiricileri kendi tercih edilen Düzenleyicisi'ni kullanabilir.  

Bu öğretici dönemi sırasında veya sonrasında sağ aygıt bilgileri sağlama hizmetine eklendiğinde işlemi, üretim donanım benzetimini yapar. Bu kod, genellikle bir PC veya .NET kod çalıştırabilir ve aygıtlar için eklenmemesi Fabrika cihazında çalıştırılır.


## <a name="enroll-the-device"></a>Cihaz kaydı

Bu adım, cihazın benzersiz güvenlik yapıları sağlama cihazı hizmetine ekleme içerir. Bu güvenlik yapıtların aşağıdaki gibidir:

- TPM tabanlı cihazlar için:
    - *Onay anahtarını* her TPM yongası veya benzetimi benzersiz. Okuma [anlamak TPM onay anahtarını](https://technet.microsoft.com/library/cc770443.aspx) daha fazla bilgi için.
    - *Kayıt kimliği* ad/kapsamında bir cihazı benzersiz olarak tanımlamak için kullanılır. Bu olabilir veya cihaz kimliği ile aynı olamaz Kimliği her aygıt için zorunludur. TPM tabanlı cihazlar için kayıt kimliği TPM kendisini, örneğin, bir SHA-256 karmasını TPM onay anahtarını türetilmiş olmalıdır.

- X.509 tabanlı cihazlar için:
    - [Cihaz için verilen X.509 sertifikası](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx), ya da biçiminde bir *.pem* veya *.cer* dosya. Tek tek kayıt için kullanmanız gerekir. *Yaprak sertifikası* X.509 sisteminiz için kayıt gruplarının karşın, kullanmanız gereken *kök sertifika* veya eşdeğer bir *imzalayan Sertifika*.
    - *Kayıt kimliği* ad/kapsamında bir cihazı benzersiz olarak tanımlamak için kullanılır. Bu olabilir veya cihaz kimliği ile aynı olamaz Kimliği her aygıt için zorunludur. X.509 tabanlı cihazlar için kayıt kimliği sertifikanın ortak adı (CN) türetilir. Bu gereksinimler hakkında daha fazla bilgi için bkz: [aygıt kavramları](https://docs.microsoft.com/en-us/azure/iot-dps/concepts-device).

Cihaz sağlama hizmetine cihazı kaydetmek için iki yolu vardır:

- **Tek tek kayıtları** bu cihaz sağlama hizmeti ile kaydedebilir tek bir cihaz için bir giriş temsil eder. Tek tek kayıtları kanıtlama mekanizmaları X.509 sertifikalarını veya SAS belirteçlerinde (gerçek veya sanal TPM) kullanabilir. Benzersiz ilk yapılandırmaları gerektirir, cihazlar için veya TPM aracılığıyla SAS belirteci kanıtlama mekanizması olarak yalnızca kullanabilirsiniz cihazlar için tek tek kayıtları kullanmanızı öneririz. Tek tek kayıtları, belirtilen istenen IOT hub cihaz kimliği olabilir.

- **Kayıt grupları** bu belirli kanıtlama mekanizması paylaşan aygıtları grubunu temsil eder. Çok sayıda istenen ilk yapılandırma paylaşmak, cihazlar için veya cihazlar için bir kayıt grubunu kullanarak aynı Kiracı tüm gitmeyi öneririz. Kayıt X.509 yalnızca gruplarıdır ve tüm kullanıcıların X.509 sertifikası zincirindeki bir imza sertifikası paylaşır.

### <a name="enroll-the-device-using-individual-enrollments"></a>Tek tek kayıtları kullanarak cihaz kaydetme

1. Visual Studio kullanarak bir Visual C# konsol uygulaması projesi oluşturma **konsol uygulaması** proje şablonu. Proje adı **DeviceProvisioning**.
    
1. Çözüm Gezgini'nde sağ **DeviceProvisioning** proje ve ardından **NuGet paketlerini Yönet...** .

1. İçinde **NuGet Paket Yöneticisi** penceresinde, seçin **Gözat** arayın ve **microsoft.azure.devices.provisioning.service**. Girişi seçin ve'ı tıklatın **yükleme** yüklemek için **Microsoft.Azure.Devices.Provisioning.Service** paketini ve kullanım koşullarını kabul edin. Bu yordam indirir, yükler ve bir başvuru ekler [Azure IOT cihaz SDK hizmeti sağlama](https://www.nuget.org/packages/Microsoft.Azure.Devices.Provisioning.Service/) NuGet paketi ve bağımlılıklarını.

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
    ```csharp
    using Microsoft.Azure.Devices.Provisioning.Service;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini önceki bölümünde belirtildiği DPS bağlantı dizesiyle değiştirin.
   
    ```csharp
    static readonly string ServiceConnectionString = "{DPS connection string}";

    private const string SampleRegistrationId = "sample-individual-csharp";
    private const string SampleTpmEndorsementKey =
            "AToAAQALAAMAsgAgg3GXZ0SEs/gakMyNRqXXJP1S124GUgtk8qHaGzMUaaoABgCAAEMAEAgAAAAAAAEAxsj2gUS" +
            "cTk1UjuioeTlfGYZrrimExB+bScH75adUMRIi2UOMxG1kw4y+9RW/IVoMl4e620VxZad0ARX2gUqVjYO7KPVt3d" +
            "yKhZS3dkcvfBisBhP1XH9B33VqHG9SHnbnQXdBUaCgKAfxome8UmBKfe+naTsE5fkvjb/do3/dD6l4sGBwFCnKR" +
            "dln4XpM03zLpoHFao8zOwt8l/uP3qUIxmCYv9A7m69Ms+5/pCkTu/rK4mRDsfhZ0QLfbzVI6zQFOKF/rwsfBtFe" +
            "WlWtcuJMKlXdD8TXWElTzgh7JS4qhFzreL0c1mI0GCj+Aws0usZh7dLIVPnlgZcBhgy1SSDQMQ==";
    private const string OptionalDeviceId = "myCSharpDevice";
    private const ProvisioningStatus OptionalProvisioningStatus = ProvisioningStatus.Enabled;
    ```

1. Cihaz kaydını uygulamak için aşağıdakileri ekleyin:

    ```csharp
    static async Task SetRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetRegistrationData");

        Attestation attestation = new TpmAttestation(SampleTpmEndorsementKey);

        IndividualEnrollment individualEnrollment = new IndividualEnrollment(SampleRegistrationId, attestation);

        individualEnrollment.DeviceId = OptionalDeviceId;
        individualEnrollment.ProvisioningStatus = OptionalProvisioningStatus;

        Console.WriteLine("\nAdding new individualEnrollment...");
        var serviceClient = ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString);

        IndividualEnrollment individualEnrollmentResult =
            await serviceClient.CreateOrUpdateIndividualEnrollmentAsync(individualEnrollment).ConfigureAwait(false);

        Console.WriteLine("\nIndividualEnrollment created with success.");
        Console.WriteLine(individualEnrollmentResult);
    }
    ```

1. Son olarak, aşağıdaki kodu ekleyin **ana** IOT hub'ınıza bağlantıyı açın ve kaydına başlamak için yöntem:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Provisioning example");

        SetRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```
        
1. Visual Studio Çözüm Gezgini'nde Çözümünüze sağ tıklayın ve ardından **başlangıç projelerini Ayarla...** . Seçin **tek başlangıç projesi**ve ardından **DeviceProvisioning** açılır menüsünde projesinde.  

1. .NET cihaz uygulama çalıştırma **DeviceProvisiong**. Aygıt için sağlama yukarı ayarlamanız gerekir: 

    ![Tek tek kayıt çalıştırın](./media/tutorial-net-provision-device-to-hub/individual.png)

Cihaz başarıyla kaydedildiğinde görüntülenen aşağıdaki olarak Portalı'nda görmeniz gerekir:

   ![Kayıt Portalı'nda](./media/tutorial-net-provision-device-to-hub/individual-portal.png)

### <a name="enroll-the-device-using-enrollment-groups"></a>Kayıt gruplarını kullanarak cihaz kaydetme

> [!NOTE]
> Kayıt grubu örneği bir X.509 sertifikası gerektirir.

1. Visual Studio Çözüm Gezgini'nde açık **DeviceProvisioning** yukarıda oluşturduğunuz proje. 

1. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
    
    ```csharp
    using System.Security.Cryptography.X509Certificates;
    ```

1. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini X509 ile değiştirin sertifika konumu.
   
    ```csharp
    private const string X509RootCertPathVar = "{X509 Certificate Location}";
    private const string SampleEnrollmentGroupId = "sample-group-csharp";
    ```

1. Aşağıdakileri ekleyin **Program.cs** Grup kaydını uygulayın:

    ```csharp
    public static async Task SetGroupRegistrationDataAsync()
    {
        Console.WriteLine("Starting SetGroupRegistrationData");

        using (ProvisioningServiceClient provisioningServiceClient =
                ProvisioningServiceClient.CreateFromConnectionString(ServiceConnectionString))
        {
            Console.WriteLine("\nCreating a new enrollmentGroup...");

            var certificate = new X509Certificate2(X509RootCertPathVar);

            Attestation attestation = X509Attestation.CreateFromRootCertificates(certificate);

            EnrollmentGroup enrollmentGroup = new EnrollmentGroup(SampleEnrollmentGroupId, attestation);

            Console.WriteLine(enrollmentGroup);
            Console.WriteLine("\nAdding new enrollmentGroup...");

            EnrollmentGroup enrollmentGroupResult =
                await provisioningServiceClient.CreateOrUpdateEnrollmentGroupAsync(enrollmentGroup).ConfigureAwait(false);

            Console.WriteLine("\nEnrollmentGroup created with success.");
            Console.WriteLine(enrollmentGroupResult);
        }
    }
    ```

1. Son olarak, aşağıdaki kodu yerine **ana** IOT hub'ınıza bağlantıyı açın ve Grup kaydına başlamak için yöntem:
   
    ```csharp
    try
    {
        Console.WriteLine("IoT Device Group Provisioning example");

        SetGroupRegistrationDataAsync().GetAwaiter().GetResult();
            
        Console.WriteLine("Done, hit enter to exit.");
        Console.ReadLine();
    }
    catch (Exception ex)
    {
        Console.WriteLine();
        Console.WriteLine("Error in sample: {0}", ex.Message);
    }
    ```

1. .NET cihaz uygulama çalıştırma **DeviceProvisiong**. Aygıt için Grup sağlama yukarı ayarlamanız gerekir: 

    ![Grup kaydı çalıştırın](./media/tutorial-net-provision-device-to-hub/group.png)

    Cihaz grubu başarıyla kaydedildiğinde görüntülenen aşağıdaki olarak Portalı'nda görmeniz gerekir:

   ![Başarılı Grup kayıt Portalı'nda](./media/tutorial-net-provision-device-to-hub/group-portal.png)


## <a name="start-the-device"></a>Cihaz Başlat

Bu noktada, aşağıdaki Kurulum cihaz kaydı için hazır:

1. Cihaz veya cihaz grubu cihaz sağlama hizmetinize, kaydedilen ve 
2. Cihazınızı güvenlikle yapılandırıldı ve cihaz sağlama hizmeti istemci SDK'sını kullanarak uygulama üzerinden erişilebilir hazırdır.

Cihazı kayıt cihazı sağlama hizmetiniz ile başlatmak istemci uygulamanızı başlatın.  


## <a name="verify-the-device-is-registered"></a>Cihaz kayıtlı doğrulayın

Bir kez, cihaz önyüklemesinde, aşağıdaki eylemleri gerçekleşmesi. TPM simulator örnek bir uygulama bkz [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) daha fazla ayrıntı için. 

1. Aygıtın aygıt sağlama hizmetinize bir kayıt isteği gönderir.
2. TPM aygıtları için aygıt hizmeti sağlama Cihazınızı yanıt vereceği bir kayıt sınama geri gönderir. 
3. Başarılı kaydı üzerinde aygıt hizmeti sağlama IOT hub'ı URI, cihaz kimliği ve şifrelenmiş anahtar cihaza geri gönderir. 
4. IOT Hub istemci uygulaması cihazda ardından hub'ınıza bağlanır. 
5. Hub'ına başarılı bağlantı üzerine IOT hub ' ın görünür aygıt görmelisiniz **aygıt Explorer**. 

    ![Hub'ına Portalı'nda başarılı bağlantı](./media/tutorial-net-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Cihaz kaydı
> * Cihaz Başlat
> * Cihaz kayıtlı doğrulayın

Yük dengeli hubs arasında birden çok aygıt sağlama konusunda bilgi almak için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Yük dengeli IOT hub'ları aygıtlarda sağlama](./tutorial-provision-multiple-hubs.md)
