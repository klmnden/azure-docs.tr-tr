## <a name="view-device-telemetry"></a>Cihaz telemetrisini görüntüleme

Cihazınızı gönderilen telemetriyi görüntüleyebilir **aygıtları** çözümdeki sayfası.

1. Üzerinde aygıtlar listesinde sağladığınız cihazı seçin **aygıtları** sayfası. Bölmenin çizim cihaz telemetri dahil olmak üzere Cihazınızı hakkında bilgileri görüntüler:

    ![Cihaz ayrıntıları](media/iot-suite-visualize-connecting/devicesdetail.png)

1. Seçin **baskısı** telemetri görünümünü değiştirmek için:

    ![Görünüm baskısı telemetri](media/iot-suite-visualize-connecting/devicespressure.png)

1. Cihazınızla ilgili tanılama bilgilerini görüntülemek için ekranı aşağı kaydırarak **tanılama**:

    ![Görünüm aygıt tanılama](media/iot-suite-visualize-connecting/devicesdiagnostics.png)

## <a name="act-on-your-device"></a>Cihazınızda hareket

Cihazlarınızda yöntemleri çağırmak için **aygıtları** Uzaktan izleme çözümü sayfasında. Örneğin, Uzaktan izleme çözümü içinde **Soğutucu** aygıtları uygulayan bir **FirmwareUpdate** yöntemi.

1. Seçin **aygıtları** gitmek için **aygıtları** çözümdeki sayfası.

1. Üzerinde aygıtlar listesinde sağladığınız cihazı seçin **aygıtları** sayfa:

    ![Fiziksel Cihazınızı seçin](media/iot-suite-visualize-connecting/devicesselect.png)

1. Cihazınızda çağırabilir yöntemlerinin listesini görüntülemek için seçin **zamanlama**. Birden fazla cihazda çalıştırmak için bir yöntem zamanlamak için listede birden çok aygıt seçebilirsiniz. **Zamanlama** paneli yöntemi türlerini seçtiğiniz tüm cihazların ortak gösterir.

1. Seçin **FirmwareUpdate**, iş adı ayarlamak **UpdatePhysicalChiller**. Ayarlama **bellenim sürümü** için **2.0.0**ayarlayın **bellenim URI** için **http://contoso.com/updates/firmware.bin**ve ardından **Uygula**:

    ![Zamanlama bellenimi güncelleştirme](media/iot-suite-visualize-connecting/deviceschedule.png)

1. İleti sırası, sanal cihaz yöntemi işlerken aygıt kodunuzu çalıştırmaya görüntülemez.

1. Güncelleştirme tamamlandığında, yeni bellenim sürümü görüntüler **aygıtları** sayfa:

    ![Güncelleştirme tamamlandı](media/iot-suite-visualize-connecting/complete.png)

> [!NOTE]
> Çözümdeki işinin durumunu izlemek için tercih **Görünüm**.

## <a name="next-steps"></a>Sonraki adımlar

Makaleyi [önceden yapılandırılmış Uzaktan izleme çözümü özelleştirme](../articles/iot-suite/iot-suite-remote-monitoring-customize.md) önceden yapılandırılmış çözümü özelleştirme için bazı yollar açıklanmaktadır.