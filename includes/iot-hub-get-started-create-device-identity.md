## <a name="create-a-device-identity"></a>Cihaz kimliği oluşturma

Bu bölümde, kullandığınız adı verilen bir Node.js aracı [iothub-explorer] [ iot-hub-explorer] Bu öğretici için bir cihaz kimliği oluşturma. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

1. Aşağıdaki komut satırı ortamınızda çalıştırın:

    `npm install -g iothub-explorer@latest`

1. Ardından hub'ınıza oturum açmak için aşağıdaki komutu çalıştırın. Yedek `{iot hub connection string}` daha önce kopyaladığınız IOT Hub bağlantı dizesiyle:

    `iothub-explorer login "{iot hub connection string}"`

1. Son olarak, adlı yeni bir cihaz kimliği oluşturma `myDeviceId` komutu:

    `iothub-explorer create myDeviceId --connection-string`

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

Sonuç cihaz bağlantı dizesini not edin. Bu cihaz bağlantı dizesi, bir cihaz olarak IOT hub'ınıza bağlanmak için cihaz uygulama tarafından kullanılır.

![][img-identity]

Başvurmak [IOT Hub ile çalışmaya başlama] [ lnk-getstarted] program aracılığıyla cihaz kimlikleri oluşturmak için.

<!-- images and links -->
[img-identity]: media/iot-hub-get-started-create-device-identity/devidentity.png

[iot-hub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md

[lnk-getstarted]: ../articles/iot-hub/quickstart-send-telemetry-dotnet.md
