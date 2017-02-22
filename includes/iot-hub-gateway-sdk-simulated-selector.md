> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-simulated-device.md)
> 
> 

[Sanal Cihaz Buluta Yükleme örneği] ile ilgili bu yönergeler, [Azure IoT Gateway SDK’sını][lnk-sdk] kullanarak sanal cihazlardan IoT Hub’ına cihazdan buluta telemetri gönderme işlemini göstermektedir.

Bu kılavuzda aşağıdaki konular ele alınmaktadır:

1. **Mimari**: Sanal Cihaz Buluta Yükleme örneği hakkında önemli mimari bilgiler.
2. **Derleme ve çalıştırma**: örneği derlemek ve çalıştırmak için gerekli adımlar.

## <a name="architecture"></a>Mimari
Sanal Cihaz Buluta Yükleme örneği, sanal cihazlardan bir IoT hub’ına telemetri gönderen ağ geçidi oluşturmak için SDK’nın nasıl kullanılacağını gösterir. Sanal cihazlar aşağıdaki nedenlerden dolayı IoT Hub'ına doğrudan bağlanamaz:

* Cihazlar IoT Hub tarafından anlaşılan bir iletişim protokolü kullanmaz.
* Cihazlar kendilerine IoT Hub tarafından atanmış kimliği anımsayacak kadar akıllı değildir.

Ağ geçidi, sanal cihazlar için bu sorunları aşağıdaki yollarla çözer:

* Ağ geçidi, sanal cihazlar tarafından kullanılan protokolü anlar, cihazdan buluta telemetri alır ve bu iletileri IoT Hub tarafından anlaşılan bir protokol kullanarak IoT hub’ına iletir.
* Ağ geçidi, IoT Hub kimliklerini sanal cihazlar adına depolar ve sanal cihazlar IoT Hub’a ileti gönderdiğinde ara sunucu olarak görev yapar.

Aşağıdaki diyagramda ağ geçidi modülleriyle birlikte örneğin ana bileşenleri gösterilmektedir:

![][1]

> [!NOTE]
> Modüller iletileri doğrudan birbirine geçirmez. Modüller aşağıdaki diyagramda gösterilen bir abonelik mekanizmasını kullanarak iletileri diğer modüllere ileten bir iç aracıya yayımlar. Daha fazla bilgi için bkz. [IoT Ağ Geçidi SDK’sını kullanmaya başlama][lnk-gw-getstarted].
> 
> 

### <a name="protocol-ingestion-module"></a>Protokol alım modülü
Bu modül, ağ geçidi aracılığıyla ve buluta cihazlardan veri almanın başlangıç noktasıdır. Örnekte, modül dört görev gerçekleştirir:

1. Sanal sıcaklık verileri oluşturur. Gerçek cihazlar kullanırsanız modül, verileri bu fiziksel cihazlardan okur.
2. Sanal sıcaklık verilerini iletinin içeriğine yerleştirir.
3. Sanal sıcaklık verilerini içeren iletiye sahte bir MAC adresine sahip özellik ekler.
4. İletiyi zincirdeki sonraki modülün kullanımına sunar.

> [!NOTE]
> Yukarıdaki diyagramda **Protokol X alımı** adlı modül, kaynak kodunda **Sanal cihaz** olarak adlandırılır.
> 
> 

### <a name="mac-lt-gt-iot-hub-id-module"></a>MAC &lt;-&gt; IoT Hub kimlik modülü
Bu modül, protokol alım modülü tarafından eklenen sanal cihaz uygulaması MAC adresini içeren bir özelliğin bulunduğu iletileri tarar. Modül böyle bir özellik bulursa, iletiye bir IoT Hub cihaz anahtarıyla birlikte başka bir özellik ekler ve sonra iletiyi zincirdeki sonraki modülün kullanımına sunar. Örnek, IoT Hub cihaz kimliklerini sanal cihazlarla bu şekilde ilişkilendirir. Geliştirici, MAC adresleri ile IoT Hub kimlikleri arasındaki eşlemeyi modül yapılandırması sırasında el ile gerçekleştirir. 

> [!NOTE]
> Bu örnekte benzersiz cihaz tanımlayıcısı olarak bir MAC adresi kullanılır ve bir IoT Hub cihaz kimliğiyle ilişkilendirilir. Bununla birlikte, farklı bir benzersiz tanımlayıcı kullanan kendi modülünüzü yazabilirsiniz. Örneğin, benzersiz seri numaralarına veya cihazın benzersiz adını içeren telemetri verilerine sahip cihazlarınız olabilir ve bunları IoT Hub cihaz kimliğini belirlemek için kullanabilirsiniz.
> 
> 

### <a name="iot-hub-communication-module"></a>IoT Hub iletişim modülü
Bu modül, önceki modül tarafından atanan bir IoT Hub cihazıyla iletileri alır ve HTTP kullanarak ileti içeriğini IoT Hub’a gönderir. HTTP, IoT Hub tarafından anlaşılan üç protokolden biridir.

Bu modül her sanal cihaz uygulaması için IoT Hub’da bir bağlantı açmak yerine, ağ geçidi ile IoT hub’ı arasında tek bir HTTP bağlantısı açar ve bu bağlantı üzerinden tüm sanal cihazlardan bağlantıları çoğaltır. Bunun yapılması tek bir ağ geçidinin, her cihaz için benzersiz bir bağlantının açıldığı duruma kıyasla çok daha fazla sayıda sanal ya da fiziksel cihaza bağlanmasını sağlar.

![][2]

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-simulated-selector/image1.png
[2]: media/iot-hub-gateway-sdk-simulated-selector/image2.png

<!-- Links -->
[Sanal Cihaz Buluta Yükleme örneği]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md

<!--HONumber=Feb17_HO1-->


