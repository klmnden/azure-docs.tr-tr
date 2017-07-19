## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma
Bu bölümde, IoT hub'ınızdaki kimlik kayıt defterinde cihaz kimliği oluşturan bir .NET konsol uygulaması oluşturursunuz. Kimlik kayıt defterinde girişi olmayan bir cihaz IoT hub'ına bağlanamaz. Daha fazla bilgi için [IoT Hub geliştirici kılavuzunun][lnk-devguide-identity] "Kimlik kayıt defteri" bölümüne bakın. Bu konsol uygulamasını çalıştırdığınızda, cihazınızın IoT Hub'a cihaz-bulut iletileri gönderdiğinde kendisini tanımlamak için kullanabileceği benzersiz bir cihaz kimliği ve anahtarı oluşturulur. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

1. Visual Studio’da **Konsol Uygulaması (.NET Framework)** proje şablonunu kullanarak yeni bir çözüme bir Visual C# Windows Klasik Masaüstü projesi ekleyin. .NET Framework sürümünün 4.5.1 veya sonraki bir sürüm olduğundan emin olun. Projeyi **CreateDeviceIdentity** ve çözümü **IoTHubGetStarted** olarak adlandırın.
   
    ![Yeni Visual C# Windows Klasik Masaüstü projesi][10]
2. Çözüm Gezgini'nde **CreateDeviceIdentity** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın.
3. **NuGet Paket Yöneticisi** penceresinde **Gözat**'ı seçin, **microsoft.azure.devices**'ı aratın, **Microsoft.Azure.Devices** paketini yüklemek için **Yükle**'yi seçin ve kullanım koşullarını kabul edin. Bu yordam ile [Azure IoT hizmet SDK'sı][lnk-nuget-service-sdk] NuGet paketi ve bağımlılıkları indirilir, yüklenir ve bu pakete bir başvuru eklenir.
   
    ![NuGet Paket Yöneticisi penceresi][11]
4. Aşağıdaki `using` deyimlerini **Program.cs** dosyasının üst kısmına ekleyin:
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. **Program** sınıfına aşağıdaki alanları ekleyin. Yer tutucu değerini, önceki bölümde hub için oluşturduğunuz IoT Hub bağlantı dizesiyle değiştirin.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. **Program** sınıfına aşağıdaki yöntemi ekleyin:
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    Bu yöntem, **myFirstDevice** kimliği ile bir cihaz kimliği oluşturur. (Bu cihaz kimliği, kimlik kayıt defterinde zaten varsa, kod yalnızca mevcut cihaz bilgilerini alır.) Bu durumda uygulama, bu kimliğin birincil anahtarını görüntüler. IoT hub'ınıza bağlanmak için sanal cihaz uygulamasında bu anahtarı kullanırsınız.
[!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

7. Son olarak, **Main** yöntemine aşağıdaki satırları ekleyin:
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Bu uygulamayı çalıştırın ve cihaz anahtarını not edin.
   
    ![Uygulama tarafından üretilen cihaz anahtarı][12]

> [!NOTE]
> IoT Hub kimlik kayıt defteri, yalnızca IoT hub'ına güvenli erişim sağlamak amacıyla cihaz kimliklerini depolar. Güvenlik kimlik bilgileri olarak kullanılmak üzere cihaz kimliklerini ve anahtarlarını ve tek bir cihaza erişimi devre dışı bırakmak için kullanabileceğiniz etkin/devre dışı bayrağını depolar. Uygulamanızın cihaza özgü diğer meta verileri depolaması gerekiyorsa uygulamaya özgü bir depo kullanması gerekir. Daha fazla bilgi için bkz. [IoT Hub geliştirici kılavuzu][lnk-devguide-identity].
> 
> 

<!-- Images. -->
[10]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp1.png
[11]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp2.png
[12]: ./media/iot-hub-get-started-create-device-identity-csharp/create-identity-csharp3.png


<!-- Links -->
[lnk-devguide-identity]: ../articles/iot-hub/iot-hub-devguide-identity-registry.md
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
