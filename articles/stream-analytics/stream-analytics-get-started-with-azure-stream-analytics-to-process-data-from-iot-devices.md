<properties
    pageTitle="IoT cihazlarından veri işlemek için Azure Stream Analytics'i kullanmaya başlayın. | Stream Analytics"
    description="Akış analizi ve gerçek zamanlı veri işleme ile birlikte IoT algılayıcı etiketleri ve veri akışları"
    keywords="iot çözümü, iot ile çalışmaya başlama"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"
/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="08/11/2016"
    ms.author="jeffstok"
/>

# IoT cihazlarından veri işlemek için Azure Stream Analytics'i kullanmaya başlama

Bu öğreticide, Nesnelerin İnterneti (IoT) cihazlarından veri toplamak üzere akış işleme mantığı oluşturmayı öğreneceksiniz. Çözümünüzü hızlı ve ekonomik bir şekilde nasıl oluşturacağınızı göstermek için gerçek hayattaki bir Nesnelerin İnterneti (IoT) kullanım örneğinden yararlanacağız.

## Önkoşullar

-   [Azure Aboneliği](https://azure.microsoft.com/pricing/free-trial/)
-   Örnek sorgu ve veri dosyaları [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)'dan indirilebilir

## Senaryo

Contoso, endüstriyel otomasyon alanında faaliyet gösteren bir şirkettir ve üretim süreçlerini tamamen otomatik hale getirmiştir. Bu fabrikada bulunan makineler, gerçek zamanlı olarak veri akışları yayabilen algılayıcılara sahiptir. Bu senaryoda bir üretim katı yöneticisi, algılayıcı verilerinden gerçek zamanlı bilgiler alarak belirli kalıpları aramak ve bunlara yönelik işlemler yapmak istiyor. Gelen veri akışındaki ilginç kalıpları bulmak için algılayıcı verileri üzerinde Stream Analytics Sorgu Dili'ni (SAQL) kullanacağız.

Burada, veriler bir Texas Instrument Algılayıcı Etiketi cihazında oluşturulmaktadır.

![Texas Instruments Algılayıcı Etiketi](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Verilerin Yükü JSON biçimindedir ve aşağıdaki gibi görünür:

    
    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  
    
Gerçek hayattaki bir senaryoda, olayları bir akış şeklinde oluşturan bunun gibi yüzlerce algılayıcınız olabilir. İdeal olarak bu olayları [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a veya [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/)'larına gönderecek bazı kodları çalıştıran bir ağ geçidi cihazının bulunması gerekir. Akış Analizi işiniz, bu olayları Event Hubs'dan alır ve akışlara yönelik gerçek zamanlı analiz sorguları çalıştırır. Daha sonra, [desteklenen çıkışlardan](stream-analytics-define-outputs.md) birine sonuçları gönderebilirsiniz.

Kullanım kolaylığı açısından, bu Başlarken kılavuzunda SensorTag cihazlarından yakalanan bir örnek veri dosyası sağlanmıştır; bu dosya üzerinde farklı sorgular çalıştırabilir ve sonuçlarını görebilirsiniz. Daha sonraki öğreticilerde, işinizi girişlere ve çıkışlara nasıl bağlayacağınızı ve bunları Azure hizmetine nasıl dağıtacağınızı öğreneceksiniz.

## Stream Analytics işi oluşturma

Yeni bir analiz işi oluşturmak için [Azure portalında](http://manage.windowsazure.com) Akış Analizi'ni seçin ve sayfanın sol alt köşesindeki **"Yeni"** seçeneğine tıklayın.

![Yeni bir Akış Analizi işi oluşturma](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

"**Hızlı Oluştur**"seçeneğine tıklayın.

**"Bölgesel İzleme Depolama Hesabı"** ayarı için **"Yeni depolama hesabı oluştur"** seçeneğini belirleyin ve buna herhangi bir benzersiz ad verin. Azure Stream Analytics bu hesabı gelecekteki tüm işleriniz için izleme bilgilerini depolamak üzere kullanır.

> [AZURE.NOTE] Bu depolama hesabını her bölge için yalnızca bir kere oluşturmanız gerekir, bu depolama bu bölgede oluşturulan tüm Stream Analytics işleri arasında paylaştırılır.

Sayfanın altındaki "**Stream Analytics İşi Oluştur**" seçeneğine tıklayın.

![Depolama hesabı yapılandırması](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Azure Stream Analytics sorgusu

Sorgu Düzenleyicisi'ne gitmek için Sorgu sekmesine tıklayın. Sorgu sekmesinde, gelen olay verilerinde dönüştürme işlemini gerçekleştiren bir T-SQL sorgusu bulunur.

## Ham verilerinizi arşivleme

En basit sorgu biçimi, tüm giriş verilerini belirlenmiş çıkışlarına arşivleyen bir doğrudan sorgudur.

![İş sorgusunu arşivleme](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Şimdi örnek veri dosyasını [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)'dan bilgisayarınızdaki bir konuma indirin. **PassThrough.txt** dosyasından sorguyu kopyalayıp yapıştırın. Aşağıda bulunan Test Et düğmesine tıklayın ve **HelloWorldASA-InputStream.json** adlı veri dosyasını indirdiğiniz konumdan seçin.

![Akış Analizi'nde Test Et düğmesi](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Giriş akışını test etme](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Aşağıda gösterildiği şekilde sorgunun sonuçlarını tarayıcıda görebilirsiniz.

![Test sonuçları](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Verileri bir koşula göre filtreleme

Sonuçları bir koşula göre filtrelemeyi deneyelim. Yalnızca "SensorA"dan gelen olaylar için sonuçları göstermek istiyoruz. Sorgu **Filtering.txt** dosyasında yer alır.

![bir veri akışını filtreleme](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Burada bir dize değerini karşılaştırdığımızı ve bunun büyük/küçük harfe duyarlı olduğunu unutmayın. Sorguyu yürütmek için **Yeniden Çalıştır** düğmesine tıklayın. Sorgu 1860 olay içinden yalnızca 389 satır döndürmelidir.

![Sorgu testine ilişkin ikinci çıktı sonuçları](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## İş akışı tetiklemesi için uyarı

Şimdi sorgumuzu daha ayrıntılı hale getireceğiz. Her algılayıcı türü için ortalama sıcaklığı 30 saniyelik aralıklarda izlemek ve yalnızca ortalama sıcaklık 100 derecenin üzerinde olduğu zaman sonuçları görüntülemek istiyorsak aşağıdaki sorguyu yazıp ardından sonuçları görmek için **Yeniden çalıştır** düğmesine tıklamamız gerekir. Sorgu **ThresholdAlerting.txt** dosyasındadır.

![30 saniyelik filtre sorgusu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Şimdi sonuçların yalnızca 245 satır içerdiğini ve ortalama sıcaklığın 100 dereceden fazla olduğu algılayıcıları listelediğini göreceksiniz. Bu sorguda olay akışını **dspl**'ye göre grupladık. Burada dspl Algılayıcı Adı ve 30 saniyelik bir **Atlayan Pencere** içeriyor. Bunun gibi zamana bağlı sorgular gerçekleştirirken, zamanın nasıl ilerlemesini istediğimizi belirtmemiz önemlidir. **TIMESTAMP BY** yan tümcesini kullanarak, zamana bağlı tüm hesaplamalar için zamanın ilerlemesinin bir yolu olarak "zaman" sütununu belirttik. Ayrıntılı bilgi için lütfen [Zaman Yönetimi](https://msdn.microsoft.com/library/azure/mt582045.aspx) ve [Pencereleme işlevleri](https://msdn.microsoft.com/library/azure/dn835019.aspx) hakkındaki MSDN konu başlıklarını okuyun.

![100 derece üzerinde sıcaklık](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Var olmayan olayları algılama

Eksik giriş olaylarını bulmak için nasıl sorgu yazabiliriz? Bunu yapmak oldukça kolaydır. Bir Algılayıcının veri gönderip sonraki bir dakika boyunca hiç olay göndermediği son zamanı bulalım. Sorgu **AbsenseOfEvent.txt** dosyasındadır.

![Var olmayan olayları algılama](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

Burada, **LEFT OUTER JOIN** deyimini aynı veri akışı üzerinde kullanıyoruz (kendi kendine birleşme). Bir iç birleşimde, yalnızca bir eşleşme bulunduğu zaman bir sonuç döndürülür.  Ancak bir **LEFT OUTER** birleştirmede, birleştirmenin sol tarafındaki bir olay eşleşmemişse sağ satırdaki tüm sütunlar için NULL değerine sahip bir satır döndürülür. Bu teknik, var olmayan olayların bulunması için oldukça kullanışlıdır. [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) hakkında daha fazla bilgi edinmek için lütfen MSDN belgelerimize bakın.

![sonuçları birleştirme](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Sonuç

Bu öğreticide, farklı Akış Analizi sorgu dili sorgularının nasıl yazılacağı ve sonuçların tarayıcıda nasıl görüntüleneceği gösterilecek. Ancak bu sadece bir başlangıçtır. Stream Analytics ile yapabileceğiniz daha birçok şey bulunmaktadır. Akış Analizi birçok giriş ve çıkışı desteklemenin yanı sıra Azure Machine Learning'deki işlevlerden de faydalanır. Bu da onu veri akışlarının analizi için sağlam bir araç haline getirir. [Öğrenme Haritamızı](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) kullanarak Akış Analizi hakkında daha fazlasını keşfetmeye başlayabilirsiniz. Sorgu yazmaya ilişkin daha fazla bilgi için [Ortak Sorgu Kalıpları](./stream-analytics-stream-analytics-query-patterns.md) ile ilgili makaleyi okuyun.



<!--HONumber=Aug16_HO4-->


