> [!div class="op_single_selector"]
> * [Windows üzerinde C](../articles/iot-suite/iot-suite-connecting-devices.md)
> * [Linux üzerinde C](../articles/iot-suite/iot-suite-connecting-devices-linux.md)
> * [mbed üzerinde C](../articles/iot-suite/iot-suite-connecting-devices-mbed.md)
> * [Node.js](../articles/iot-suite/iot-suite-connecting-devices-node.md)
> 
> 

## <a name="scenario-overview"></a>Senaryoya genel bakış
Bu senaryoda aşağıdaki telemetriyi önceden yapılandırılmış uzaktan izleme [çözümüne][lnk-what-are-preconfig-solutions] gönderen bir cihaz oluşturacaksınız:

* Dış ortam sıcaklığı
* İç ortam sıcaklığı
* Nem oranı

Örneği basitleştirmek amacıyla cihaz üzerindeki kod örnek değerler oluşturmaktadır. Ancak cihazınıza gerçek sensörler bağlayıp gerçek telemetri verileri göndererek örneği ileri taşımanızı öneririz.

Cihaz ayrıca çözüm panosundan çağrılan yöntemlere ve çözüm panosunda ayarlanan istenen özellik değerlerine yanıt verebilir.

Bu öğreticiyi tamamlamak için etkin bir Azure hesabınızın olması gerekir. Bir hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk-free-trial].

## <a name="before-you-start"></a>Başlamadan önce
Cihazınız için kod yazmadan önce, önceden yapılandırılmış uzaktan izleme çözümünüzü ve bu çözümde yeni bir özel cihaz hazırlamanız gerekir.

### <a name="provision-your-remote-monitoring-preconfigured-solution"></a>Önceden yapılandırılmış uzaktan izleme çözümünüzü sağlama
Bu öğreticide oluşturduğunuz cihaz, önceden yapılandırılmış bir [uzaktan izleme][lnk-remote-monitoring] çözümü örneğine veri göndermektedir. Önceden yapılandırılmış uzaktan izleme çözümünüzü henüz Azure hesabınızda hazırlamadıysanız aşağıdaki adımları uygulayın:

1. <https://www.azureiotsuite.com/> sayfasında çözüm oluşturmak için **+** öğesine tıklayın.
2. Çözümünüzü oluşturmak için **Uzaktan izleme** panelindeki **Seç**'e tıklayın.
3. **Uzaktan izleme çözümü oluştur** sayfasında bir **Çözüm adı** belirleyin, dağıtmak istediğiniz **Bölgeyi** seçin ve kullanmak istediğiniz Azure aboneliğini belirtin. Ardından **Çözüm oluştur**'a tıklayın.
4. Sağlama işleminin tamamlanmasını bekleyin.

> [!WARNING]
> Önceden yapılandırılmış çözümler, faturalanabilir Azure hizmetlerini kullanır. Gereksiz ücretlerden kaçınmak için işinizi tamamladıktan sonra önceden yapılandırılmış çözümü aboneliğinizden kaldırmayı unutmayın. Önceden yapılandırılmış çözümü aboneliğinizden tamamen kaldırmak için <https://www.azureiotsuite.com/> sayfasını ziyaret edin.
> 
> 

Uzaktan izleme çözümü için sağlama işlemi tamamlandıktan sonra çözüm panosunu tarayıcınızda açmak için **Başlat**'a tıklayın.

![Çözüm panosu][img-dashboard]

### <a name="provision-your-device-in-the-remote-monitoring-solution"></a>Cihazınızı uzaktan izleme çözümünde sağlama
> [!NOTE]
> Çözümünüzde önceden bir cihaz hazırladıysanız bu adımı atlayabilirsiniz. İstemci uygulamasını oluştururken cihaz kimlik bilgilerini bilmeniz gerekir.
> 
> 

Bir cihazın önceden yapılandırılmış çözüme bağlanabilmesi için geçerli kimlik bilgileriyle kendini IoT Hub üzerinde tanıtması gerekir. Cihaz kimlik bilgilerini çözüm panosundan alabilirsiniz. Cihaz kimlik bilgilerini bu öğreticinin sonraki adımlarında istemci uygulamanıza ekleyebilirsiniz.

Uzaktan izleme çözümünüze cihaz eklemek için çözüm panosunda aşağıdaki adımları tamamlayın:

1. Panonun sol alt köşesinde **Cihaz ekle**'ye tıklayın.
   
   ![Cihaz ekleme][1]
2. **Özel Cihaz** panelinde **Yeni ekle**'ye tıklayın.
   
   ![Özel cihaz ekleme][2]
3. **Kendi Cihaz Kimliğimi tanımlamama izin ver**'i seçin. **cihazım** gibi bir Cihaz Kimliği girin, adın kullanımda olmadığını doğrulamak için **Kimliği denetle**'ye tıklayın ve ardından cihazı hazırlamak için **Oluştur**'a tıklayın.
   
   ![Cihaz kimliği ekleme][3]
4. Cihaz kimlik bilgilerini (Cihaz Kimliği, IoT Hub Ana Bilgisayar Adı ve Cihaz Anahtarı) not edin. İstemci uygulamanız uzaktan izleme çözümüne bağlanmak için bu değerleri kullanır. Sonra da **Bitti**’ye tıklayın.
   
    ![Cihaz kimlik bilgilerini görüntüleme][4]
5. Çözüm panosundaki cihaz listesinden cihazınızı seçin. Ardından **Cihaz Ayrıntıları** panelinde **Cihazı Etkinleştir**'e tıklayın. Cihazınızın durumu **Çalışıyor** olarak değişir. Uzaktan izleme çözümü artık cihazınızdan telemetri verileri alabilir ve cihazınızda yöntemler çağırabilir.

[img-dashboard]: ./media/iot-suite-selector-connecting/dashboard.png
[1]: ./media/iot-suite-selector-connecting/suite0.png
[2]: ./media/iot-suite-selector-connecting/suite1.png
[3]: ./media/iot-suite-selector-connecting/suite2.png
[4]: ./media/iot-suite-selector-connecting/suite3.png

[lnk-what-are-preconfig-solutions]: ../articles/iot-suite/iot-suite-what-are-preconfigured-solutions.md
[lnk-remote-monitoring]: ../articles/iot-suite/iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/