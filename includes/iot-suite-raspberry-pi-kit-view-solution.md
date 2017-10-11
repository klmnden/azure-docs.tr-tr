## <a name="view-the-solution-dashboard"></a>Çözüm panosunu görüntüleme

Çözüm panosu, dağıtılan çözümü yönetmenizi sağlar. Örneğin, telemetriyi görüntüleyebilir, cihazları eklemek ve yöntemleri çağırma.

1. Sağlama tamamlandığında ve önceden yapılandırılmış çözümünüzün kutucuğu **Hazır**’ı gösterdiğinde, uzaktan izleme çözümü portalınızı yeni bir sekmede açmak için **Başlat**’ı seçin.

    ![Önceden yapılandırılmış çözümü başlatma][img-launch-solution]

1. Varsayılan olarak, çözüm portalı *panoyu* gösterir. Sayfanın sol tarafındaki menüyü kullanarak çözüm portalının diğer alanlarına gidebilirsiniz.

    ![Önceden yapılandırılmış uzaktan izleme panosu][img-menu]

## <a name="add-a-device"></a>Cihaz ekleme

Bir cihazın önceden yapılandırılmış çözüme bağlanabilmesi için geçerli kimlik bilgileriyle kendini IoT Hub üzerinde tanıtması gerekir. Cihaz kimlik bilgilerini çözüm panosundan alabilirsiniz. Cihaz kimlik bilgilerini bu öğreticinin sonraki adımlarında istemci uygulamanıza ekleyebilirsiniz.

Zaten yapmadıysanız, özel bir aygıt, Uzaktan izleme çözümüne ekleyin. Çözüm Panosu, aşağıdaki adımları tamamlayın:

1. Panonun sol alt köşesinde **Cihaz ekle**'ye tıklayın.

   ![Cihaz ekleme][1]

1. **Özel Cihaz** panelinde **Yeni ekle**'ye tıklayın.

   ![Özel cihaz ekleme][2]

1. **Kendi Cihaz Kimliğimi tanımlamama izin ver**'i seçin. Bir cihaz kimliği girin **rasppi**, tıklatın **denetleyin kimliği** adı çözümünüzde zaten kullanmadıysanız ve ardından doğrulamak için **oluşturma** cihaz sağlamak için.

   ![Cihaz kimliği ekleme][3]

1. Cihaz kimlik bilgilerini not edin (**cihaz kimliği**, **IOT Hub ana bilgisayar adına**, ve **aygıt anahtarı**). İstemci uygulamanızı Raspberry Pi'yi üzerinde Uzaktan izleme çözümüne bağlanmak için bu değerleri gerekir. Sonra da **Bitti**’ye tıklayın.

    ![Cihaz kimlik bilgilerini görüntüleme][4]

1. Çözüm panosundaki cihaz listesinden cihazınızı seçin. Ardından **Cihaz Ayrıntıları** panelinde **Cihazı Etkinleştir**'e tıklayın. Cihazınızın durumu **Çalışıyor** olarak değişir. Uzaktan izleme çözümü artık cihazınızdan telemetri verileri alabilir ve cihazınızda yöntemler çağırabilir.

[img-launch-solution]: media/iot-suite-raspberry-pi-kit-view-solution/launch.png
[img-menu]: media/iot-suite-raspberry-pi-kit-view-solution/menu.png
[1]: media/iot-suite-raspberry-pi-kit-view-solution/suite0.png
[2]: media/iot-suite-raspberry-pi-kit-view-solution/suite1.png
[3]: media/iot-suite-raspberry-pi-kit-view-solution/suite2.png
[4]: media/iot-suite-raspberry-pi-kit-view-solution/suite3.png