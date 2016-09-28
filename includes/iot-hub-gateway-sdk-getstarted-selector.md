> [AZURE.SELECTOR]
- [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
- [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)

Bu makalede [Azure IoT Ağ Geçidi SDK’sı][lnk-gateway-sdk] mimarisinin temel bileşenlerini göstermek üzere [Hello World örnek kodunun][lnk-helloworld-sample] ayrıntılı bilgileri verilmektedir. Örnek, "hello world" iletisini her beş saniyede bir dosyaya kaydeden basit bir ağ geçidi oluşturmak üzere Ağ Geçidi SDK’sını kullanır.

Bu kılavuzda aşağıdaki konular ele alınmaktadır:

- **Kavramlar**: Ağ Geçidi SDK’sı ile oluşturduğunuz herhangi bir ağ geçidini oluşturan bileşenlere kavramsal genel bakış.  
- **Hello World örnek mimarisi**: Kavramların Hello World örneğine nasıl uygulandığını ve bileşenlerin birbiriyle ne kadar uyumlu olduğunu açıklar.
- **Örneği oluşturma**: Örneği oluşturmak için gerekli adımlar.
- **Örneği çalıştırma**: Örneği çalıştırmak için gerekli adımlar. 
- **Tipik çıkış**: Örneği çalıştırdığınızda beklenecek çıktı örneği.
- **Kod parçacıkları**: Hello World örneğinin temel ağ geçidi bileşenlerini nasıl uyguladığını gösteren kod parçacıkları koleksiyonu.

## Ağ Geçidi SDK’sı kavramları

Örnek kodu incelemeden veya Ağ Geçidi SDK’sını kullanarak kendi alan ağ geçidinizi oluşturmadan önce SDK mimarisini destekleyen temel kavramları anlamanız gerekir.

### Modules

Azure IoT Ağ Geçidi SDK’sı ile *modüller* oluşturup derleyerek bir ağ geçidi oluşturursunuz. Modüller birbirileriyle veri alışverişinde bulunmak için *iletileri* kullanır. Modül bir ileti alır, üzerinde bir işlem yapar, isteğe bağlı olarak onu yeni bir iletiye dönüştürür ve ardından diğer modüllerin işlemesi için yayımlar. Bazı modüller yalnızca yeni iletiler oluşturabilir ve hiçbir zaman gelen iletileri işlemez. Bir modül zinciri, her bir modülün bir noktadaki veriler üzerinde dönüştürme gerçekleştirdiği bir veri işleme işlem hattı oluşturur.

![][1]
 
SDK aşağıdakileri içerir:

- Ortak ağ geçidi işlevlerini yerine getiren önceden yazılmış modüller.
- Bir geliştiricinin özel modüller yazmak için kullanabileceği arabirimler.
- Bir modül kümesini dağıtmak ve çalıştırmak için gereken altyapı.

SDK çeşitli işletim sistemleri ve platformlar üzerinde çalışacak ağ geçitleri derlemenizi sağlayan bir soyut katman sağlar.

![][2]

### İletiler

Birbirlerine ileti gönderen modüllerin düşünülmesi bir ağ geçidinin nasıl çalıştığını kavramsallaştırmanın kolay bir yolu olsa da neler olduğunu doğru şekilde yansıtmaz. Modüller birbirleri ile iletişim kurmak için bir ileti veri yolu kullanır, veri yoluna iletileri yayımlar ve veri yolu bu iletileri veri yoluna bağlı tüm modüllere yayınlar.

Modül, ileti veri yoluna bir mesajı yayımlamak için **MessageBus_Publish** işlevini kullanır. İleti veri yolu bir geri çağırma işlevini çağırarak iletileri bir modüle teslim eder. İleti bir dizi anahtar/değer özelliklerinden ve bir bellek bloğu olarak geçirilen içeriklerden oluşur.

![][3]

İleti veri yolu her bir iletiyi kendisine bağlı her bir modüle iletmek için bir yayın mekanizması kullandığı için her modül iletilerin filtrelenmesinden sorumludur. Bir modül yalnızca kendisine yönelik iletiler üzerinde işlem yapmalıdır. İleti filtreleme, ileti işlem hattını etkili bir şekilde oluşturur. Modül genellikle işlemesi gereken iletileri tanımlamak üzere ileti özelliklerini kullanarak aldığı iletileri filtreler.

## Hello World örnek mimarisi

Hello World örneği önceki bölümde açıklanan kavramları göstermektedir. Hello World örneği iki modülden oluşan bir işlem hattına sahip ağ geçidini kullanır:

-   *Hello world* modülü beş saniyede bir ileti oluşturur ve günlükçü modülüne geçirir.
-   *Günlükçü* modülü aldığı iletileri bir dosyaya yazar.

![][4]

Önceki bölümde açıklandığı gibi Hello World modülü iletileri beş saniyede bir günlükçü modülüne doğrudan iletmez. Bunun yerine, beş saniyede bir ileti veri yoluna bir ileti yayımlar.

Günlükçü modülü ileti veri yolundan iletiyi alır ve bir filtre içinde özelliklerini inceler. Günlükçü modülü iletiyi işlemesi gerektiğini belirlerse iletinin içindekileri bir dosyaya yazar.

Günlükçü modülü yalnızca ileti veri yolundan gelen iletileri kullanır, veri yoluna hiçbir zaman yeni ileti yayımlamaz.

![][5]

Yukarıdaki şekilde Hello World örneğinin mimarisi ve örneğin farklı kısımlarını [depoda][lnk-gateway-sdk] kullanan kaynak dosyalarının ilgili yolları gösterilmektedir. Kodu kendiniz keşfedin veya kılavuz olarak aşağıdaki kod parçacıklarından birini kullanın.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

<!--HONumber=Sep16_HO3-->


