> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Bu makalede [Azure IoT Edge][lnk-iot-edge] mimarisinin temel bileşenlerini göstermek üzere [Hello World örnek kodunun][lnk-helloworld-sample] ayrıntılı bilgileri verilmektedir. Örnek, "hello world" iletisini her beş saniyede bir dosyaya kaydeden basit bir ağ geçidi oluşturmak üzere Azure IoT Edge'i kullanır.

Bu kılavuzda aşağıdaki konular ele alınmaktadır:

* **Hello World örnek mimarisi**: açıklar nasıl [Azure IOT kenar mimari kavramlarını] [ lnk-edge-concepts] Hello World örneği ve bileşenlerin nasıl bir araya getireceğinizi için geçerlidir.
* **Örneği oluşturma**: Örneği oluşturmak için gerekli adımlar.
* **Örneği çalıştırma**: Örneği çalıştırmak için gerekli adımlar. 
* **Tipik çıkış**: Örneği çalıştırdığınızda beklenecek çıktı örneği.
* **Kod parçacıkları**: Hello World örnek anahtar IOT sınır ağ geçidi bileşenlerini nasıl uyguladığını gösteren kod parçacıkları koleksiyonu.


## <a name="hello-world-sample-architecture"></a>Hello World örnek mimarisi
Hello World örneği önceki bölümde açıklanan kavramları göstermektedir. Hello World örneği iki IOT kenar modülden oluşan bir işlem hattına sahip bir IOT sınır ağ geçidi uygular:

* *Hello world* modülü beş saniyede bir ileti oluşturur ve günlükçü modülüne geçirir.
* *Günlükçü* modülü aldığı iletileri bir dosyaya yazar.

![Azure IoT Edge ile oluşturulan Hello World örneğinin mimarisi][4]

Önceki bölümde açıklandığı gibi Hello World modülü iletileri beş saniyede bir günlükçü modülüne doğrudan iletmez. Bunun yerine, beş saniyede bir aracıya bir ileti yayımlar.

Günlükçü modülü aracıdan iletiyi alır ve buna göre davranırken iletinin içeriğini bir dosyaya yazar.

Günlükçü modülü yalnızca aracıdan gelen iletileri kullanır, aracıya hiçbir zaman yeni ileti yayımlamaz.

![Aracının Azure IoT Edge’deki modüller arasında iletileri yönlendirme yöntemi][5]

Yukarıdaki şekilde Hello World örneği ve içinde farklı kısımlarını uygulamak kaynak dosyaları için göreli yolların mimarisi gösterilmektedir [deposu][lnk-iot-edge]. Kodu kendiniz keşfedin veya kılavuz olarak bu makaledeki kod parçacıkları kullanın.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md