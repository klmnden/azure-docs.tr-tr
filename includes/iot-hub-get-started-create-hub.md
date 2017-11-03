## <a name="create-an-iot-hub"></a>IoT hub oluşturma
Bağlanılacak sanal cihaz uygulamanız için bir IoT Hub oluşturun. Aşağıdaki adımlar, Azure portalını kullanarak bu görevi nasıl tamamlayacağınızı gösterir.

[!INCLUDE [iot-hub-create-hub](iot-hub-create-hub.md)]

IOT hub'ı oluşturduğunuza göre cihazlar ve uygulamalar IOT hub'ınıza bağlanmak için kullanacağınız önemli bilgiler bulun. 

1. IOT hub'ı sorunsuz oluşturulduğunda, yeni IOT hub'ının özellikler penceresini açmak için Azure portalındaki IOT hub'ına yönelik yeni kutucuğa tıklayın. **Ana bilgisayar adını** not edin ve **Paylaşılan erişim ilkeleri**'ne tıklayın.
   
    ![Yeni IoT hub penceresi][4]
1. **Paylaşılan erişim ilkeleri** penceresinde **iothubowner** ilkesine tıklayın, ardından **iothubowner** penceresindeki IoT Hub bağlantı dizesini kopyalayın ve not edin. Daha fazla bilgi için "IoT Hub geliştirici kılavuzunun" [Erişim denetimi][lnk-access-control] bölümüne bakın.
   
    ![Paylaşılan erişim ilkeleri][5]

<!-- Images. -->
[4]: ./media/iot-hub-get-started-create-hub/create-iot-hub4.png
[5]: ./media/iot-hub-get-started-create-hub/create-iot-hub5.png

<!-- Links -->
[lnk-access-control]: ../articles/iot-hub/iot-hub-devguide-security.md
