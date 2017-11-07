## <a name="view-the-telemetry"></a>Telemetriyi görüntüleyebilir

Raspberry Pi'yi, artık uzaktan izleme çözümüne telemetri gönderiyor. Çözüm panosunda telemetri görüntüleyebilirsiniz. Ayrıca, Raspberry Pi'yi çözüm panodan iletisi gönderebilir.

- Çözüm panosuna gidin.
- Cihazınızı seçmek **cihaz görünümüne** açılır.
- Raspberry Pi'yi telemetrisinden Panoda görüntüler.

![Raspberry Pi'yi telemetrisini görüntüleme][img-telemetry-display]

## <a name="initiate-the-firmware-update"></a>Bellenim güncelleştirme başlatın

Bellenim güncelleştirme işlemini indirir ve aygıt istemci uygulamanın güncelleştirilmiş bir sürümü üzerinde Raspberry Pi'yi yükler. Bellenim güncelleştirme desende açıklaması bellenim güncelleştirme işlemi hakkında daha fazla bilgi için bkz [cihaz yönetimine genel bakış IOT Hub ile][lnk-update-pattern].

Aygıtta bir yöntemini çağırarak bellenim güncelleştirme işlemini başlatır. Bu yöntem zaman uyumsuz ve güncelleştirme işlemi başlar başlamaz döndürür. Cihaz çözüm güncelleştirmenin ilerleme durumunu hakkında bilgilendirmek için bildirilen özelliklerini kullanır.

Çözüm panosundan, Raspberry Pi'yi üzerinde yöntemleri çağırır. Raspberry Pi'yi Uzaktan izleme çözümüne ilk bağlandığında destekliyorsa yöntemleri hakkında bilgi gönderir. 

[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-advanced/telemetry.png
[lnk-update-pattern]: ../articles/iot-hub/iot-hub-device-management-overview.md
