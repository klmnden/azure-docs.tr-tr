## <a name="create-an-iot-hub"></a>IoT hub oluşturma

1. [Azure portalında](https://portal.azure.com/) **Yeni** > **Nesnelerin İnterneti** > **IoT Hub** öğesine tıklayın.

   ![Azure portalında IoT hub' oluşturma](../articles/iot-hub/media/iot-hub-create-hub-and-device/1_create-azure-iot-hub-portal.png)
2. **IoT hub** bölmesine IoT hub’ınızla ilgili aşağıdaki bilgileri girin:

     **Ad**: IoT hub'ınızın adını girin. Girdiğiniz ad geçerli ise yeşil bir onay işareti görünür.

     **Fiyatlandırma ve ölçek katmanı**: **F1 - Ücretsiz** katmanını seçin. Bu seçenek bu tanıtım için yeterlidir. Daha fazla bilgi için bkz. [Fiyatlandırma ve ölçek katmanı](https://azure.microsoft.com/pricing/details/iot-hub/).

     **Kaynak grubu**: IoT hub’ını barındıracak bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](../articles/azure-resource-manager/resource-group-portal.md).

     **Konum**: IoT hub'ının oluşturulduğu, size en yakın konumu seçin.

     **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği belirleyin.

   ![IoT hub'ınızı oluşturmak için bilgileri girin](../articles/iot-hub/media/iot-hub-create-hub-and-device/2_fill-in-fields-for-azure-iot-hub-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

3. **Oluştur**’a tıklayın. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden görebilirsiniz.

   ![IoT hub’ınıza ilişkin ilerleme durumu bildirimlerine bakın](../articles/iot-hub/media/iot-hub-create-hub-and-device/3_notification-azure-iot-hub-creation-progress-portal.png)

4. IoT hub'ınız oluşturulduktan sonra panoda IoT hub’ınıza tıklayın. **Ana bilgisayar adını** not edin ve **Paylaşılan erişim ilkeleri**'ne tıklayın.

   ![IoT hub’ınızın ana bilgisayar adını alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/4_get-azure-iot-hub-hostname-portal.png)

5. **Paylaşılan erişim ilkeleri** bölmesinde **iothubowner** ilkesine tıklayın, ardından IoT hub’ınızın **Bağlantı dizesi** değerini kopyalayıp not edin. Daha fazla bilgi için bkz. [IoT Hub'a erişimi denetleme](../articles/iot-hub/iot-hub-devguide-security.md).

> [!NOTE] 
Bu kurulum öğreticisinde bu iothubowner bağlantı dizesi gerekli değildir. Ancak, bu kurulum tamamlandıktan sonra farklı IoT senaryolarında bazı öğreticiler için gerekli olabilir.

   ![IoT hub bağlantı dizenizi alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/5_get-azure-iot-hub-connection-string-portal.png)

## <a name="register-a-device-in-the-iot-hub-for-your-device"></a>Cihazınız için IoT hub’a cihaz kaydetme

1. [Azure portalında](https://portal.azure.com/) IoT hub'ınızı açın.

2. **Device Explorer**’a tıklayın.
3. Device Explorer bölmesinde **Ekle**’ye tıklayarak IoT hub’ınıza bir cihaz ekleyin. Ardından şunları yapın:

   **Cihaz Kimliği**: Yeni cihazın kimliğini girin. Cihaz Kimlikleri büyük/küçük harfe duyarlıdır.

   **Kimlik Doğrulama Türü**: **Simetrik Anahtar**’ı seçin.

   **Anahtarları Otomatik Onayla**: Bu onay kutusunu işaretleyin.

   **Cihazı IoT Hub'a bağla**: **Etkinleştir**’e tıklayın.

   ![IoT hub’ınızın Device Explorer menüsünde cihaz ekleme](../articles/iot-hub/media/iot-hub-create-hub-and-device/6_add-device-in-azure-iot-hub-device-explorer-portal.png)

   [!INCLUDE [iot-hub-pii-note-naming-device](iot-hub-pii-note-naming-device.md)]

4. **Kaydet** düğmesine tıklayın.
5. Cihaz oluşturulduktan sonra cihazı **Device Explorer** bölmesinde açın.
6. Bağlantı dizesinin birincil anahtarını not edin.

   ![Cihaz bağlantı dizesini alma](../articles/iot-hub/media/iot-hub-create-hub-and-device/7_get-device-connection-string-in-device-explorer-portal.png)
