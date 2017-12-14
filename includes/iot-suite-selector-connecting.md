> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [Node.js (genel)](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> * [Raspberry Pi üzerinde Node.js](../articles/iot-suite/iot-suite-connecting-pi-node.md)
> * [Raspberry Pi üzerinde C](../articles/iot-suite/iot-suite-connecting-pi-c.md)

Bu öğreticide, uygulamanız bir **Soğutucu** Uzaktan izleme için aşağıdaki telemetri gönderen aygıt [önceden yapılandırılmış çözüm](../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md):

* Sıcaklık
* Basınç
* Nem oranı

Kolaylık olması için örnek telemetri değerleri için kod oluşturur **Soğutucu**. Örnek gerçek algılayıcılar aygıtınıza bağlanma ve gerçek telemetri göndermesini genişletebilirsiniz.

Örnek cihaz ayrıca:

* Meta veri özelliklerini tanımlamak için çözüme gönderir.
* Gelen tetiklenen eylemler yanıtlar **aygıtları** çözümdeki sayfası.
* Yapılandırma değişiklikleri yanıtlar göndermek **aygıtları** çözümdeki sayfası.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü](http://azure.microsoft.com/pricing/free-trial/).

## <a name="before-you-start"></a>Başlamadan önce

Cihazınız için kod yazmadan önce, önceden yapılandırılmış uzaktan izleme çözümünüzü ve bu çözümde yeni bir özel cihaz hazırlamanız gerekir.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış uzaktan izleme çözümünüzü sağlama

**Soğutucu** Bu öğreticide oluşturduğunuz cihaz örneğine verileri gönderir [Uzaktan izleme](../articles/iot-suite/iot-suite-remote-monitoring-explore.md) çözüm önceden yapılandırılmış. Azure hesabınızda önceden yapılandırılmış Uzaktan izleme çözümü sağlanan yüklemediyseniz, bkz: [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](../articles/iot-suite/iot-suite-remote-monitoring-deploy.md)

Uzaktan izleme çözümü için sağlama işlemi tamamlandıktan sonra çözüm panosunu tarayıcınızda açmak için **Başlat**'a tıklayın.

![Çözüm Panosu](media/iot-suite-selector-connecting/dashboard.png)

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Cihazınızı uzaktan izleme çözümünde sağlama

> [!NOTE]
> Çözümünüzde önceden bir cihaz hazırladıysanız bu adımı atlayabilirsiniz. İstemci uygulaması oluşturduğunuzda cihaz kimlik bilgileri gerekir.

Bir cihazın önceden yapılandırılmış çözüme bağlanabilmesi için geçerli kimlik bilgileriyle kendini IoT Hub üzerinde tanıtması gerekir. Aygıt kimlik bilgisi çözümden alabilir **aygıtları** sayfası. Cihaz kimlik bilgilerini bu öğreticinin sonraki adımlarında istemci uygulamanıza ekleyebilirsiniz.

Uzaktan izleme çözümünüz için bir aygıt eklemek için aşağıdaki adımları tamamlayın **aygıtları** çözümü sayfasında:

1. Seçin **sağlama**ve ardından **fiziksel** olarak **aygıt türü**:

    ![Bir fiziksel cihaz sağlama](media/iot-suite-selector-connecting/devicesprovision.png)

1. Girin **fiziksel Soğutucu** aygıt kimliği olarak Seçin **simetrik anahtar** ve **otomatik oluştur anahtarları** seçenekleri:

    ![Aygıt seçenekleri seçin](media/iot-suite-selector-connecting/devicesoptions.png)

Cihazınızı önceden yapılandırılmış çözümü bağlanmak için kullanmaları gereken kimlik bilgilerini bulmak için tarayıcınızı Azure Portalı'na gidin. Aboneliğiniz için oturum açın.

1. Uzaktan izleme çözümünüz kullandığı Azure hizmetleri içeren kaynak grubunu bulun. Kaynak grubu sağladığınız Uzaktan izleme çözümü ile aynı ada sahiptir.

1. Bu kaynak grubundaki IOT hub'ına gidin. Ardından **IOT cihazları**:

    ![Cihaz Gezgini](media/iot-suite-selector-connecting/deviceexplorer.png)

1. Seçin **cihaz kimliği** üzerinde oluşturduğunuz **aygıtları** Uzaktan izleme çözümü sayfasında.

1. Not **cihaz kimliği** ve **birincil anahtar** değerleri. Çözüme Cihazınızı bağlamak için kod eklediğinizde, bu değerleri kullanın.

Çözüm Uzaktan izleme fiziksel bir aygıtı sağlanan artık önceden yapılandırılmış. Aşağıdaki bölümlerde, çözümünüz için bağlanmak için cihaz kimlik bilgilerini kullanan istemci uygulaması yürütürsünüz.

İstemci uygulaması yerleşik uygulayan **Soğutucu** cihaz modeli. Önceden yapılandırılmış çözüm cihaz modeli bir cihaz hakkında aşağıdakileri belirtir:

* Çözüme raporları cihazı özellikleri. Örneğin, bir **Soğutucu** aygıt, bellenim ve konumu hakkında bilgi raporlar.
* Telemetri türlerini cihaz çözüme gönderir. Örneğin, bir **Soğutucu** cihaz sıcaklık ve nem baskısı değerleri gönderir.
* Yöntemleri cihazda çalıştırmak için çözümden zamanlayabilirsiniz. Örneğin, bir **Soğutucu** aygıt uygulanmalı **yeniden**, **FirmwareUpdate**, **EmergencyValveRelease**, ve  **IncreasePressuree** yöntemleri.