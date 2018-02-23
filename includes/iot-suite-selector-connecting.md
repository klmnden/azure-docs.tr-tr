> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (genel)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [Raspberry Pi üzerinde C](../articles/iot-suite/iot-suite-connecting-pi-c.md)

Bu öğreticide, uygulamanız bir **Soğutucu** Uzaktan izleme için aşağıdaki telemetri gönderen aygıt [önceden yapılandırılmış çözüm](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Sıcaklık
* baskısı
* Nem oranı

Kolaylık olması için örnek telemetri değerleri için kod oluşturur **Soğutucu**. Örnek gerçek algılayıcılar aygıtınıza bağlanma ve gerçek telemetri göndermesini genişletebilirsiniz.

Örnek cihaz ayrıca:

* Meta veri özelliklerini tanımlamak için çözüme gönderir.
* Gelen tetiklenen eylemler yanıtlar **aygıtları** çözümdeki sayfası.
* Yapılandırma değişiklikleri yanıtlar göndermek **aygıtları** çözümdeki sayfası.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için herhangi bir kod yazmadan önce Uzaktan izleme önceden yapılandırılmış çözümünüzü dağıtmak ve yeni bir fiziksel aygıt ekleyin.

### <a name="deploy-your-remote-monitoring-preconfigured-solution"></a>Uzaktan izleme önceden yapılandırılmış çözümünüzü dağıtma

**Soğutucu** Bu öğreticide oluşturduğunuz cihaz örneğine verileri gönderir [Uzaktan izleme](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) çözüm önceden yapılandırılmış. Azure hesabınızda önceden yapılandırılmış Uzaktan izleme çözümü sağlanan yüklemediyseniz, bkz: [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Dağıtım işlemi Uzaktan izleme çözümü sona için tıklattığınızda **başlatma** tarayıcınızda çözüm panosunu açın.

![Çözüm Panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="add-your-device-to-the-remote-monitoring-solution"></a>Cihazınızı Uzaktan izleme çözümüne ekleme

> [!NOTE]
> Çözümünüze bir aygıt zaten eklediyseniz, bu adımı atlayabilirsiniz. Ancak, sonraki adım, aygıt bağlantı dizesi gerektirir. Bir aygıtın bağlantı dizesinden alabilir [Azure portal](https://portal.azure.com) veya kullanarak [az IOT](https://docs.microsoft.com/cli/azure/iot?view=azure-cli-latest) CLI aracı.

Bir cihazın önceden yapılandırılmış çözüme bağlanabilmesi için geçerli kimlik bilgileriyle kendini IoT Hub üzerinde tanıtması gerekir. Çözüm cihazı eklediğinizde, bu kimlik bilgileri içeren cihaz bağlantı dizesini kaydedin bildirme fırsatı bulabilirsiniz. Bu öğreticide daha sonra istemci uygulamanızda cihaz bağlantı dizesini içerir.

Uzaktan izleme çözümünüz için bir aygıt eklemek için aşağıdaki adımları tamamlayın **aygıtları** çözümü sayfasında:

1. Seçin **+ yeni cihaz**ve ardından **fiziksel** olarak **aygıt türü**:

    ![Bir fiziksel aygıt ekleme](media/iot-suite-selector-connecting/devicesprovision.png)

1. Girin **fiziksel Soğutucu** aygıt kimliği olarak Seçin **simetrik anahtar** ve **otomatik oluştur anahtarları** seçenekleri:

    ![Aygıt seçenekleri seçin](media/iot-suite-selector-connecting/devicesoptions.png)

1. Seçin **uygulamak**. Not edin **cihaz kimliği**, **birincil anahtar**, ve **bağlantı dize birincil anahtarı** değerler:

    ![Kimlik bilgilerini alma](media/iot-suite-selector-connecting/credentials.png)

Artık fiziksel bir aygıtı için Uzaktan izleme önceden yapılandırılmış çözümü eklendi ve kendi cihaz bağlantı dizesini not ettiğiniz. Aşağıdaki bölümlerde, çözümünüz için bağlanmak için cihaz bağlantı dizesini kullanır istemci uygulaması yürütürsünüz.

İstemci uygulaması yerleşik uygulayan **Soğutucu** cihaz modeli. Önceden yapılandırılmış çözüm cihaz modeli bir cihaz hakkında aşağıdakileri belirtir:

* Çözüme raporları cihazı özellikleri. Örneğin, bir **Soğutucu** aygıt, bellenim ve konumu hakkında bilgi raporlar.
* Telemetri türlerini cihaz çözüme gönderir. Örneğin, bir **Soğutucu** cihaz sıcaklık ve nem baskısı değerleri gönderir.
* Yöntemleri cihazda çalıştırmak için çözümden zamanlayabilirsiniz. Örneğin, bir **Soğutucu** aygıt uygulanmalı **yeniden**, **FirmwareUpdate**, **EmergencyValveRelease**, ve  **IncreasePressure** yöntemleri.