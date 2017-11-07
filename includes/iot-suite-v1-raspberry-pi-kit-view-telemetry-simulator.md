## <a name="view-the-telemetry"></a>Telemetriyi görüntüleyebilir

Raspberry Pi'yi, artık uzaktan izleme çözümüne telemetri gönderiyor. Çözüm panosunda telemetri görüntüleyebilirsiniz. Ayrıca, Raspberry Pi'yi çözüm panodan iletisi gönderebilir.

- Çözüm panosuna gidin.
- Cihazınızı seçmek **cihaz görünümüne** açılır.
- Raspberry Pi'yi telemetrisinden Panoda görüntüler.

![Raspberry Pi'yi telemetrisini görüntüleme][img-telemetry-display]

## <a name="act-on-the-device"></a>Cihazda hareket

Çözüm panodan, Raspberry Pi'yi yöntemleri çağırabilirsiniz. Uzaktan izleme çözümüne Raspberry Pi'yi bağlandığında, desteklediği yöntemleri hakkında bilgi gönderir.

- Çözüm panosunda tıklatın **aygıtları** ziyaret etmek için **aygıtları** sayfası. Böğürtlenli Pi seçin **cihaz listesi**. Ardından **yöntemleri**:

    ![Panoda aygıtları listele][img-list-devices]

- Üzerinde **yöntemi çağırma** sayfasında, **LightBlink** içinde **yöntemi** açılır.

- Seçin **InvokeMethod**. Simulator konsolunda bir ileti üzerinde Raspberry Pi'yi yazdırır. Uygulamasını Raspberry Pi'yi çözüm panosuna geri bildirim gönderir:

    ![Yöntem geçmişini göster][img-method-history]

- Kullanarak açma ve kapatma LED geçebilirsiniz **ChangeLightStatus** yöntemi ile bir **LightStatusValue** kümesine **1** için üzerinde veya **0** için devre dışı.

> [!WARNING]
> Azure hesabınızda çalıştıran uzaktan izleme çözümü bırakırsanız çalıştırıldığında için faturalandırılır. Uzaktan izleme çözümü çalışırken tüketiminin azaltılması hakkında daha fazla bilgi için bkz: [yapılandırma Azure IOT paketi önceden yapılandırılmış çözümleri tanıtım amacıyla][lnk-demo-config]. Bunu kullanmayı bitirdikten sonra önceden yapılandırılmış çözümü Azure hesabınızdan silin.


[img-telemetry-display]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/telemetry.png
[img-list-devices]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/listdevices.png
[img-method-history]: media/iot-suite-v1-raspberry-pi-kit-view-telemetry-simulator/methodhistory.png

[lnk-demo-config]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/Docs/configure-preconfigured-demo.md