<properties
    pageTitle="IoT cihazlarından veri işlemek için Azure Stream Analytics'i kullanmaya başlayın. | Stream Analytics"
    description="Akış analizi ve gerçek zamanlı veri işleme ile birlikte IoT algılayıcı etiketleri ve veri akışları"
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
    ms.date="05/03/2016"
    ms.author="jeffstok"
/>

# IoT cihazlarından veri işlemek için Azure Stream Analytics'i kullanmaya başlama

Bu öğreticide, Nesnelerin İnterneti (IoT) cihazlarından veri toplamak üzere akış işleme mantığı oluşturmayı öğreneceksiniz. Çözümünüzü hızlı ve ekonomik bir şekilde nasıl oluşturacağınızı göstermek için gerçek hayattaki bir Nesnelerin İnterneti (IoT) kullanım örneğinden yararlanacağız.

## Önkoşullar

-   [Azure Aboneliği](https://azure.microsoft.com/pricing/free-trial/)
-   Örnek sorgu ve veri dosyaları [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted)'dan indirilebilir

## Senaryo

Contoso, endüstriyel otomasyon alanında faaliyet gösteren bir üretim şirketidir ve üretim süreçlerini tamamen otomatik hale getirmiştir. Bu fabrikada bulunan makineler, gerçek zamanlı olarak veri akışları yayan algılayıcılara sahiptir. Bu senaryoda bir üretim katı yöneticisi, algılayıcı verilerinden gerçek zamanlı bilgiler alarak belirli kalıpları aramak ve bunlara yönelik işlemler yapmak istiyor. Gelen veri akışındaki ilginç kalıpları bulmak için algılayıcı verileri üzerinde Stream Analytics Sorgu Dili'ni (SAQL) kullanacağız.

Burada, veriler bir Texas Instrument Algılayıcı Etiketi cihazında oluşturulmaktadır.

![Texas Instruments Algılayıcı Etiketi](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Verilerin Yükü JSON biçimindedir ve aşağıdaki gibi görünür:

    
    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  
    
Gerçek hayattaki bir senaryoda, olayları bir akış şeklinde oluşturan bu gibi yüzlerce algılayıcınız olacaktır. İdeal bir durumda, bu olayları [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)'a gönderecek bazı kodları çalıştıran bir ağ geçidi cihazı bulunmalıdır. Stream Analytics işiniz bu olayları Event Hubs'dan kullanır, sorgular olarak ifade edilen gerçek zamanlı analizler çalıştırır ve sonuçları istenen çıkışlara gönderir.

Bu Başlarken kılavuzunda SensorTag cihazlarından yakalanan bir örnek veri dosyası sağlanmıştır, bu dosya üzerinde farklı sorgular çalıştırabilir ve sonuçlarını görebilirsiniz. Daha sonraki öğreticilerde, işinizi girişlere ve çıkışlara nasıl bağlayacağınızı ve bunları Azure hizmetine nasıl dağıtacağınızı öğreneceksiniz.

## Stream Analytics işi oluşturma

Yeni bir analiz işi oluşturmak için [Azure portalında](http://manage.windowsazure.com) Stream Analytics'i açın ve sayfanın sol alt köşesindeki **"Yeni"** seçeneğine tıklayın.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

"**Hızlı Oluştur**"seçeneğine tıklayın.

**"Bölgesel İzleme Depolama Hesabı"** ayarı için **"Yeni depolama hesabı oluştur"** seçeneğini belirleyin ve buna herhangi bir benzersiz ad verin. Azure Stream Analytics bu hesabı gelecekteki tüm işleriniz için izleme bilgilerini depolamak üzere kullanır.

> [AZURE.NOTE] Bu depolama hesabını her bölge için yalnızca bir kere oluşturmanız gerekir, bu depolama bu bölgede oluşturulan tüm Stream Analytics işleri arasında paylaştırılır.

Sayfanın altındaki "**Stream Analytics İşi Oluştur**" seçeneğine tıklayın.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Azure Stream Analytics sorgusu

Sorgu Düzenleyicisi'ne gitmek için Sorgu sekmesine tıklayın. Sorgu sekmesi, gelen verilerde dönüştürme işlemini gerçekleştiren bir SQL sorgusu içerir.

## Ham verilerinizi arşivleme

En basit sorgu biçimi, tüm giriş verilerini belirlenmiş çıkışlarına arşivleyen bir doğrudan sorgudur.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Şimdi örnek veri dosyasını [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted)'dan bilgisayarınızdaki bir konuma indirin. **PassThrough.txt** dosyasından sorguyu kopyalayıp yapıştırın. Aşağıda bulunan Test et düğmesine tıklayın ve **HelloWorldASA-InputStream.json** adlı veri dosyasını indirdiğiniz konumda seçin.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Aşağıdaki tarayıcıda sorgu sonuçlarını görebilirsiniz.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Verileri bir koşula göre temizleme

Sonuçları bir koşula göre filtrelemeyi deneyelim. Yalnızca "SensorA"dan gelen olaylar için sonuçları göstermek istiyoruz. Bu sorgu, **Filtering.txt** dosyasında bulunur.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Burada bir dize değerini karşılaştırdığımızı ve bunun büyük/küçük harfe duyarlı olduğunu unutmayın. Sorguyu yürütmek için **Yeniden Çalıştır** düğmesine tıklayın. Sorgu 1860 olay içinden yalnızca 389 satır döndürmelidir.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## İş akışı tetiklemesi için uyarı

Şimdi sorgumuzu biraz daha ilginç hale getirelim. Her algılayıcı türü için ortalama sıcaklığı 30 saniyelik aralıklarda izlemek ve yalnızca ortalama sıcaklık 100 derecenin üzerinde olduğu zaman sonuçları görüntülemek istiyorsak aşağıdaki Sorguyu yazıp ardından sonuçları görmek için Yeniden Çalıştır düğmesine tıklamamız gerekir. Bu sorgu, **ThresholdAlerting.txt** dosyasında bulunur.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Gördüğünüz gibi şimdi sonuçlar yalnızca ortalama sıcaklığın 100 dereceden fazla olduğu algılayıcılara ait 245 satırı içeriyor. Bu sorguda olay akışını dspl'e göre grupladık, bu da Algılayıcı Adını ve 30 saniyelik bir **Atlayan Pencereyi** içerir. Bu gibi zamana bağlı sorguları gerçekleştirirken, zamanın nasıl ilerlemesini istediğimizi belirtmemiz önemlidir. **TIMESTAMP BY** yan tümcesini kullanarak, zamana bağlı tüm hesaplamalar için zamanın ilerlemesinin bir yolu olarak "zaman" sütununu belirttik. Ayrıntılı bilgi için lütfen [Zaman Yönetimi](https://msdn.microsoft.com/library/azure/mt582045.aspx) ve [Pencereleme](https://msdn.microsoft.com/library/azure/dn835019.aspx) hakkındaki MSDN konu başlıklarımızı okuyun.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Kalıp eksikliklerini bulma

Kalıp eksikliklerini bulmayı sağlayacak bir sorguyu nasıl yazabilirim? Örneğin, bir Algılayıcının veri gönderip sonraki bir dakika boyunca hiçbir olay göndermediği son zamanı bulalım. Bu sorgu, **AbsenseOfEvent.txt** dosyasında bulunur.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

Burada, **LEFT OUTER JOIN** deyimini aynı veri akışı üzerinde kullanıyoruz (kendi kendine birleşme). Bir iç birleşimde, yalnızca bir eşleşme bulunduğu zaman bir sonuç döndürülür.  Ancak bir **LEFT OUTER** birleştirmede, birleştirmenin sol tarafındaki bir olay eşleşmemişse sağ satırdaki tüm sütunlar için NULL değerine sahip bir satır döndürülür. Bu teknik, olay eksikliklerini bulmak için çok kullanışlıdır. [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx) hakkında daha ayrıntılı bilgi için lütfen MSDN belgelerimize bakın.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Sonuç

Bu öğretici temel olarak farklı SAQL sorguları yazmanız ve verilerle ilgili farklı kalıplara ait sonuçları bir tarayıcıda görmeniz için giriş niteliğindedir. Ancak bu sadece bir başlangıçtır. Stream Analytics ile yapabileceğiniz daha birçok şey bulunmaktadır. Sonraki adımda, Stream Analytics işini girişlere ve çıkışlara bağlamayı ve Azure'a dağıtmayı öğreneceksiniz. [Öğrenme Haritası](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) Kılavuzumuzu kullanarak Stream Analytics hakkında daha fazlasını keşfetmeye başlayabilirsiniz. Sorgu yazmakla ilgili daha fazla bilgi için [Ortak Sorgu Kalıpları](./stream-analytics-stream-analytics-query-patterns.md#query-example-detect-the-absence-of-events) hakkındaki makaleyi okuyun.



<!----HONumber=Jun16_HO2-->


