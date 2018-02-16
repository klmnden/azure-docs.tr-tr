1. [Azure portalında][lnk-portal] oturum açın.
1. **Yeni** > **Nesnelerin İnterneti** > **IoT Hub**’ı seçin.
   
    ![Azure portalı Atlama Çubuğu][1]

1. **IoT hub** bölmesine IoT hub’ınızla ilgili aşağıdaki bilgileri girin:

   * **Ad**: IoT hub'ınız için bir ad oluşturun. Girdiğiniz ad geçerli ise yeşil bir onay işareti görünür.

   [!INCLUDE [iot-hub-pii-note-naming-hub](iot-hub-pii-note-naming-hub.md)]

   * **Fiyatlandırma ve ölçek katmanı**: Bu öğretici için **F1 - Ücretsiz** katmanını seçin. Daha fazla bilgi için [Fiyatlandırma ve ölçek katmanı][lnk-pricing] konusunu inceleyin.

   * **Kaynak grubu**: IoT hub’ını barındıracak bir kaynak grubu oluşturun veya mevcut bir kaynak grubunu kullanın. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma][lnk-resource-groups] konusunu inceleyin

   * **Konum**: Size en yakın konumu seçin.

   * **Panoya sabitle**: Panodan IoT hub'ınıza kolay erişim için bu seçeneği işaretleyin.

    ![IoT hub penceresi][2]

1. **Oluştur**’a tıklayın. IoT hub’ınızın oluşturulması birkaç dakika sürebilir. İlerleme durumunu **Bildirimler** bölmesinden izleyebilirsiniz.
<!-- Images -->
[1]: ./media/iot-hub-get-started-create-hub/create-iot-hub1.png
[2]: ./media/iot-hub-get-started-create-hub/create-iot-hub2.png
<!-- Links -->
[lnk-portal]: https://portal.azure.com/
[lnk-pricing]: https://azure.microsoft.com/pricing/details/iot-hub/
[lnk-resource-groups]: ../articles/azure-resource-manager/resource-group-portal.md