
# <a name="azure-and-internet-of-things"></a>Azure ve Nesnelerin İnterneti

Microsoft Azure’a ve Nesnelerin İnterneti’ne (IOT) Hoş Geldiniz. Bu makalede, Azure hizmetlerini kullanarak dağıtabileceğiniz bir IoT çözümünün genel özellikleri anlatılır. IoT çözümleri, çok sayıda cihaz ve çözüm arka ucu arasında güvenli, çift yönlü iletişim gerektirir. Çözüm arka ucu, cihazdan buluta olay akışınızdan bilgileri açığa çıkarmak için otomatik, tahmine dayalı analizleri kullanabilir.

[Azure IoT Hub][lnk-iot-hub], Azure hizmetlerini kullanan tüm IoT çözümlerinin temel bir yapı taşıdır. IoT Hub, milyonlarca IoT cihazı ile bir çözüm arka ucu arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir hizmettir. 

[Azure IoT Paketi][lnk-iot-suite], belirli IoT senaryoları için bu mimarinin eksiksiz, uçtan uca uygulamalarını sağlar. Örneğin:

* *Uzaktan izleme* çözümü, satış makineleri gibi cihazların durumunu takip etmeniz sağlar.
* *Tahmine dayalı bakım* çözümü, uzak pompa istasyonlarındaki pompalar gibi cihazların bakım gereksinimlerini tahmin etmenize ve plansız zaman kayıplarından kaçınmanıza yardımcı olur.
* *Bağlı fabrika* çözümü sektörel cihazlarınızı bağlamanıza ve izlemenize yardımcı olur.

## <a name="iot-solution-architecture"></a>IOT çözüm mimarisi

Aşağıdaki diyagram tipik bir IoT çözüm mimarisini göstermektedir. Belirli Azure hizmetlerinin adları görülemese de, genel IoT çözüm mimarisinin önemli öğelerini açıklamaktadır. Bu mimaride, IoT cihazları bulut ağ geçidine gönderdikleri verileri toplar. Bulut ağ geçidi, verileri diğer arka uç hizmetleri tarafından işlenebilir hale getirir. Çözüm arka ucu, bir pano veya rapor aracılığıyla verileri iş kolu uygulamalarına veya insan kullanıcılara sunar.

![IOT çözüm mimarisi][img-solution-architecture]

> [!NOTE]
> IoT mimarisinin ayrıntılı incelemesi için bkz. [Microsoft Azure IoT Başvuru Mimarisi][lnk-refarch].

### <a name="device-connectivity"></a>Cihaz bağlantısı

Bu IoT çözümünde, cihazlar pompa istasyonuna ait sensör okumaları gibi telemetriyi depolanması ve işlenmesi amacıyla bulut uç noktasına gönderir. Tahmine dayalı bakım senaryosunda çözüm arka ucu, belirli bir pompanın ne zaman bakıma gerek duyacağını saptamak için sensör verilerinin akışını kullanabilir. Cihazlar, bulut uç noktasına ait iletileri okuyarak buluttan cihaza iletileri de alıp yanıtlayabilir. Örneğin, tahmine dayalı bakım senaryosunda çözüm arka ucu, bakımın başlamasından hemen önce akışların yeniden yönlendirilmesini başlatmak amacıyla pompa istasyonundaki diğer pompalara da ileti gönderebilir. Bu yordam, bakım mühendisinin sorunu düzeltmeye hemen başlayabilmesini sağlar.

IoT projelerinin karşılaştığı en büyük zorluklardan biri de, cihazların güvenle ve güvenilir olarak çözüm arka uçlarına nasıl bağlanacaklarıdır. IoT cihazlarında, tarayıcılar ve mobil uygulamalar gibi diğer istemcilerle karşılaştırıldığında farklı özellikler bulunur. IoT cihazları:

* İnsan olan bir operatörü bulunmayan ve genellikle katıştırılmış sistemlerdir.
* Fiziksel erişimin pahalı olduğu uzak konumlarda dağıtılabilir.
* Yalnızca çözüm arka ucu aracılığıyla erişilebilir. Cihazla etkileşime geçmek için başka bir yol yoktur.
* Sınırlı güç ve işleme kaynaklarına sahip olabilir.
* Aralıklı, yavaş veya pahalı bir ağ bağlantısına sahip olabilir.
* Mülkiyete ait, özel veya sektöre özgü uygulama protokolleri kullanması gerekebilir.
* Büyük bir popüler donanım ve yazılım platformu kümesi kullanılarak oluşturulabilir.

Yukarıdaki gereksinimlere ek olarak, tüm IoT çözümlerinin ölçek, güvenlik ve güvenilirlik de sunması gerekir. Elde edilen bağlantı gereksinimleri kümesi, web kapsayıcıları ve mesajlaşma aracıları gibi geleneksel teknolojiler kullanıldığında uygulaması zor ve zaman alıcı olabilir. Azure IoT Hub ve IoT cihaz SDK'ları bu gereksinimleri karşılayan çözümlerin uygulanmasını kolaylaştırır.

Cihaz doğrudan bir bulut ağ geçidi uç noktasıyla iletişim kurabilir. Cihaz bulut ağ geçidinin desteklediği iletişim protokollerinin hiçbirini kullanamıyorsa, bir ara ağ geçidi üzerinden bağlanabilir. Örneğin, cihazlar IoT Hub'ın desteklediği protokollerden herhangi birini kullanamıyorsa [Azure IoT protokol ağ geçidi][lnk-protocol-gateway] tarafından protokol çevirisi gerçekleştirilebilir.

### <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Bulutta, verilerin büyük bir bölümü bir IoT çözümü arka ucunda işlenir. IoT çözüm arka ucu:

* Telemetriyi cihazlarınızdan ölçekli alır ve bu verilerin nasıl işleneceğini ve depolanacağını saptar. 
* Komutları buluttan belirli cihazlara göndermenizi sağlayabilir.
* Cihaz sağlamanıza ve hangi cihazların altyapıya bağlanabileceğini denetlemenize olanak tanıyan cihaz kaydı becerileri sağlar.
* Cihazlarınızın durumunu izlemenizi ve etkinliklerini takip etmenizi sağlar.

Tahmine dayalı bakım senaryosunda, çözüm arka ucu geçmiş telemetri verilerini depolar. Çözüm arka ucu, bu verileri belirli bir pompanın bakım zamanının geldiğini belirten desenleri görmek amacıyla kullanabilir.

IOT çözümlerinde otomatik geri bildirim döngüleri bulunabilir. Örneğin, çözüm arka ucundaki analitik bir modül, belirli bir cihazdaki sıcaklığın normal çalışma seviyesinin üzerinde olduğunu telemetriden tanımlayabilir. Ardından çözüm cihaza, doğru işlemi yapması için talimat veren bir komut gönderebilir.

### <a name="presentation-and-business-connectivity"></a>Sunu ve iş bağlantısı

Sunu ve iş bağlantı katmanı son kullanıcıların IoT çözümü ve cihazlarla etkileşime geçmesini sağlar. Kullanıcıların kendi cihazlarından toplanan verileri görüntülemelerini ve çözümlemelerini sağlar. Bu görünümler, hem geçmiş verileri hem de yakın gerçek zamanlı verileri görüntüleyebilen panolar veya raporlar biçiminde olabilir. Örneğin, bir kullanıcı belirli bir pompa istasyonunun durumunu denetleyebilir ve sistem tarafından gerçekleştirilen tüm uyarıları görebilir. Bu katman, kurumsal iş süreçlerine veya iş akışlarına bağlanmak üzere var olan iş kolu uygulamalarına sahip IoT çözüm arka ucunun tümleştirilmesini de sağlar. Örneğin, tahmine dayalı bakım çözümü bir zamanlama sistemiyle tümleştirilebilir. Çözüm bakım yapılması gereken bir pompa olduğunu belirlediğinde, zamanlama sistemi pompa istasyonunu ziyaret etmesi için bir mühendis ayırır.

![IoT çözüm panosu][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
