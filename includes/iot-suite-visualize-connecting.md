## <a name="view-device-telemetry-in-the-dashboard"></a>Cihaz telemetrisini panoda görüntüleme
Uzaktan izleme çözümündeki pano, cihazlarınızın IoT Hub'a gönderdiği telemetriyi görüntülemenizi sağlar.

1. Tarayıcınızda uzaktan izleme çözümü panosuna dönün, sol taraftaki **Cihazlar** paneline tıklayın ve **Cihaz listesi** sayfasına gidin.
2. **Cihaz listesinde** cihazınızın durumu **Çalışıyor** olmalıdır. Değilse **Cihaz Ayrıntıları** panelinden **Cihazı Etkinleştir**'e tıklayın.
   
    ![Cihaz durumunu görüntüle][18]
3. **Pano**'ya tıklayarak panoya dönün ve telemetrisini görüntülemek için **Görüntülenecek Cihaz** açılır menüsünden cihazınızı seçin. Örnek uygulamadan gelen telemetri iç ortam sıcaklığı için 50 birim, dış ortam sıcaklığı için 55 birim ve nem için 50 birim şeklindedir.
   
    ![Cihaz telemetrisini görüntüleme][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>Cihazınızda bir yöntem çağırma
Uzaktan izleme çözümündeki pano, IoT Hub aracılığıyla cihazlarınızda yöntem çağırmanızı sağlar. Örneğin uzaktan izleme çözümünde cihazın yeniden başlatılması benzetimini yapmak için bir yöntem çağırabilirsiniz.

1. Uzaktan izleme çözümü panosunda, sol taraftaki **Cihazlar** paneline tıklayın ve **Cihaz listesi** sayfasına gidin.
2. **Cihaz listesi** bölümünde cihazınızın **Cihaz kimliğine** tıklayın.
3. **Cihaz ayrıntıları** panelinde **Yöntemler**'e tıklayın.
   
    ![Cihaz yöntemleri][13]
4. **Yöntem** açılır menüsünde **InitiateFirmwareUpdate** seçin ve **FWPACKAGEURI** alanına işlevsiz bir URL girin. Yöntemi cihazda çağırmak için **Yöntemi Çağır**'a tıklayın.
   
    ![Cihaz yöntemi çağırma][14]
   

5. Cihaz yöntemi işlediğinde cihazınızın kodunu çalıştıran konsolda bir ileti göreceksiniz. Yöntemin sonuçları, çözüm portalındaki geçmişe eklenir:

    ![Yöntem geçmişini görüntüleme][img-method-history]

## <a name="next-steps"></a>Sonraki adımlar
[Önceden yapılandırılmış çözümleri özelleştirme][lnk-customize] makalesinde bu örneği bir adım ileri taşımak için kullanabileceğiniz adımlar anlatılmaktadır. Gerçek sensörler kullanma ve ek komutlar uygulama gibi eklemeler yapabilirsiniz.

İzinler hakkında daha fazla bilgi için [azureiotsuite.com sitesini][lnk-permissions] inceleyebilirsiniz.

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md
