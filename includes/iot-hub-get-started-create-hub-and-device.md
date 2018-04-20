## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

IOT hub'ı oluşturduğunuza göre cihazlar ve uygulamalar IOT hub'ınıza bağlanmak için kullanacağınız önemli bilgiler bulun. 

1. IoT hub'ınız oluşturulduktan sonra panoda IoT hub’ınıza tıklayın. **Ana bilgisayar adını** not edin ve **Paylaşılan erişim ilkeleri**'ne tıklayın.

   ![IoT hub’ınızın ana bilgisayar adını alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

1. **Paylaşılan erişim ilkeleri** bölmesinde **iothubowner** ilkesine tıklayın, ardından IoT hub’ınızın **Bağlantı dizesi** değerini kopyalayıp not edin. Daha fazla bilgi için bkz. [IoT Hub'a erişimi denetleme](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Bu kurulum öğreticisinde bu iothubowner bağlantı dizesi gerekli değildir. Ancak, bu kurulum tamamlandıktan sonra farklı IoT senaryolarında bazı öğreticiler için gerekli olabilir.

   ![IoT hub bağlantı dizenizi alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Cihazınız için IoT hub’a cihaz kaydetme

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınızı açın.

2. Tıklatın **IOT cihazları**.
3. IOT cihazları bölmesinde **Ekle** bir cihaz IOT hub'ınıza eklemek için. Ardından şunları yapın:

   **Cihaz Kimliği**: Yeni cihazın kimliğini girin. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

   **Kimlik Doğrulama Türü**: **Simetrik Anahtar**’ı seçin.

   **Anahtarları Otomatik Onayla**: Bu onay kutusunu işaretleyin.

   **Cihazı IoT Hub'a bağla**: **Etkinleştir**’e tıklayın.

   ![Bir cihaz IOT hub'ınızın IOT cihazları ekleme](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-iot-devices-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. **Kaydet**’e tıklayın.
5. Cihaz oluşturulduktan sonra aygıtı açmak **IOT cihazları** bölmesi.

   ![IOT Hub'ındaki IOT cihaz listesi](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_device-list-in-iot-devices-portal.png)

6. Bağlantı dizesinin birincil anahtarını not edin.

   ![Cihaz bağlantı dizesini alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/8_get-device-connection-string-in-iot-devices-portal.png)
