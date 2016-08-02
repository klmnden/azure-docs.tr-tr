<properties
 pageTitle="Azure IoT önceden yapılandırılmış çözümleri | Microsoft Azure"
 description="Azure IoT önceden yapılandırılmış çözümlerin ve ek kaynaklara bağlantısı olan mimarisinin bir açıklaması."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="05/25/2016"
 ms.author="dobett"/>

# Azure IoT Paketi önceden yapılandırılmış çözümleri nelerdir?

Azure IoT Paketi önceden yapılandırılmış çözümleri, aboneliği kullanarak Azure’e dağıtabildiğiniz yaygın IoT çözüm modellerinin uygulamalarıdır. Önceden yapılandırılmış çözümleri kullanabilirsiniz:

- Kendi IoT çözümleriniz için bir başlangıç noktası olarak.
- IoT çözüm tasarımı ve geliştirmesinde ortak yaklaşımlar hakkında bilgi edinmek için.

Önceden yapılandırılmış her çözüm tam, uçtan uca, telemetri oluşturmak için sanal cihazları kullanan bir uygulamadır.

Azure’de çözümleri dağıtmanın ve çalıştırmanın yanı sıra, tam kaynak kodunu indirebilir, sonra da size özel IoT gereksinimlerini karşılaması için bunu özelleştirebilir ve uzatabilirsiniz.

> [AZURE.NOTE] Önceden yapılandırılmış çözümlerden birini dağıtmak için [Microsoft Azure IOT Paketi][lnk-azureiotsuite] sayfasını ziyaret edin. [IoT önceden yapılandırılmış çözümlerine giriş][lnk-preconf-get-started] makalesi çözümlerden birinin dağıtılması ve çalıştırılması hakkında daha fazla bilgi sağlamaktadır.

Aşağıdaki tabloda, çözümlerin belirli IoT özelliklerini nasıl karşıladığı gösterilmektedir:

| Çözüm | Veri Alımı | Cihaz Kimliği | Komut ve Denetim | Kurallar ve Eylemler | Tahmine Dayalı Analiz |
|------------------------|-----|-----|-----|-----|-----|
| [Uzaktan izleme][lnk-preconf-get-started] | Evet | Evet | Evet | Evet | -   |
| [Tahmine dayalı bakım][lnk-predictive-maintenance] | Evet | Evet | Evet | Evet | Evet |

- *Veri alımı*: Bulut ölçeğinde veri girişi.
- *Cihaz kimliği*: Bağlı her cihazın benzersiz kimliklerini yönetme.
- *Komut ve denetim*: Cihazın bazı işlemler yapması için buluttan cihaza ileti gönderme.
- *Kurallar ve eylemler*: Belirli cihazdan buluta verilerde işlem yapmak için çözüm arka ucu kuralları kullanır.
- *Tahmine dayalı analiz*: Belirli işlemlerin ne zaman gerçekleştirileceğini tahmin etmek için çözüm arka ucu cihazdan buluta verilerine analiz uygular. Örneğin, motor bakımının ne zaman olacağını saptamak için uçak motoru telemetrisinin analiz edilmesi.

## Önceden yapılandırılmış Uzaktan İzleme çözümüne genel bakış

Diğer çözümlerin de paylaştığı çok sayıda ortak tasarım öğesi gösterdiğinden, bu makalede önceden yapılandırılmış uzaktan izleme çözümünü seçtik.

Aşağıdaki diyagram uzaktan izleme çözümünün önemli öğelerin göstermektedir. Aşağıdaki bölümlerde bu öğeler hakkında daha fazla bilgi verilmektedir.

![Önceden yapılandırılmış Uzaktan İzleme çözümü mimarisi][img-remote-monitoring-arch]

## Cihazlar

Önceden yapılandırılmış uzaktan izleme çözümünü dağıttığınızda, dört sanal cihaz bir soğutma cihazının benzetimini yapan çözümde önceden hazırlanır. Bu sanal cihazlarda telemetri yayan yerleşik bir sıcaklık ve nem modeline bulunur. Bu sanal cihazlar, çözüm aracılığıyla uçtan uca veri akışının gösterilmesine, müşteri uygulaması için başlangıç noktası olarak çözümü kullanan bir arka uç geliştiriciyseniz de uygun telemetri kaynağının ve komut hedefinin sağlanmasına katılırlar.

Cihaz önce, önceden yapılandırılmış uzaktan izleme çözümünde IoT Hub'ına bağlandığında, IoT hub'ına gönderilen cihaz bilgileri iletisi cihazın karşılık verebildiği komutların listesini numaralandırır. Önceden yapılandırılmış uzaktan izleme çözümünde komutlar şunlardır: 

- *Cihaza Ping Yap*: Cihaz bu komutu bir bildirimle yanıtlar. Cihazın halen etkin ve dinleniyor olduğunu denetlemek için yararlıdır.
- *Telemetriyi Başlat*: Cihaza telemetri göndermeye başlaması talimatı verir.
- *Telemetriyi Durdur*: Cihaza telemetri göndermeyi durdurması talimatı verir.
- *Sıcaklık Ayar Noktasını Değiştir*: Cihazın gönderdiği benzetimli sıcaklık telemetri değerlerini denetler. Arka uç mantığının test edilmesi için yararlıdır.
- *Telemetriyi Tanıla*: Cihazın dış sıcaklığı telemetri olarak gönderip göndermediğini denetler.
- *Cihaz Durumunu Değiştir*.: Cihaz durumu meta veri özelliğini cihaz raporlarına ait olarak ayarlar. Arka uç mantığının test edilmesi için yararlıdır.

Aynı telemetriyi yayan ve aynı komutu yanıtlayan çözüme daha fazla sanal cihaz ekleyebilirsiniz. 

## IoT Hub’ı

Önceden yapılandırılmış bu çözümde, IoT Hub’ı örneği tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] *Bulut Ağ Geçidi*’ne karşılık gelir.

IoT hub’ı telemetriyi tek uç noktada yer alan cihazlardan alır. IoT hub'ı, her cihazın kendisine gönderilen komutları alabildiği cihaza özel uç noktaları da korur.

IoT hub’ı, sunucu tarafı telemetri okuma uç noktasında alınan telemetriyi kullanılabilir hale getirir.

## Azure Stream Analytics

Önceden yapılandırılmış çözüm, cihazlara ait telemetri akışına filtre uygulamak için üç [Azure Stream Analytics][lnk-asa] (ASA) işini kullanır:


- *DeviceInfo işi* - cihaz kaydına özel iletileri yönlendiren Olay hub’ına verilerin çıktısını alır, cihaz çözüm cihazı kayıt defterine (DocumentDB veritabanı) ilk bağlandığında veya **Cihaz durumunu değiştir** komutuna yanıt olarak gönderilir. 
- *Telemetry işi* - ham telemetrenin tümünü soğuk depolama için Azure blob depolamaya gönderir ve çözüm panosunda görüntülenen telemetri toplamalarını hesaplar.
- *Rules işi* - kural eşiklerini aşan değerlerle ilgili telemetri akışına filtre uygular ve verilerin çıktısını bir Olay hub’ına alır. Bir kural başlatıldığında, çözüm portalı panosu görünümü bu olayı alarm geçmişi tablosunun yeni bir satırı olarak görüntüler ve çözüm portalında Kurallar ve Eylemler görünümlerinde tanımlanan ayarlar tabanında eylemi tetikler.

Önceden yapılandırılmış bu çözümde, ASA işleri tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucu**’nun bir parçasını oluşturur.

## Olay işlemcisi

Önceden yapılandırılmış bu çözümde, olay işlemcisi tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucu**’nun bir parçasını oluşturur.

**DeviceInfo** ve **Rules** ASA işleri, diğer arka iç hizmetlerine dağıtılması amacıyla kendi çıktılarını Olay hub’larına gönderir. Çözüm, bu Olay Hub’larından iletileri okumak için [WebJob][lnk-web-job]’da çalışan bir [EventPocessorHost][llnk-event-processor] örneğini kullanır. **EventProcessorHost**, DocumentDB veritabanındaki cihaz verilerini güncelleştirmek için **DeviceInfo** verilerini, Mantıksal uygulamayı çağırmak ve çözüm portalında uyarılar ekranını güncelleştirmek için de **Rules** verilerini kullanır.

## Cihaz kimliği kayıt defteri ve DocumentDB

Her IoT hub'ında cihaz anahtarlarını depolayan bir [cihaz kimliği kayıt defteri][lnk-identity-registry] vardır. IoT hub'ı bu bilgileri cihazların kimliğini doğrulamak için kullanır - hub’a bağlanmadan önce cihazların kayıtlı ve geçerli bir anahtara sahip olması gerekir.

Bu çözüm, durumları, destekledikleri komutlar ve diğer meta veriler gibi cihazlar hakkında ek bilgileri depolar. Çözüme özel bu cihaz verilerin depolamak için çözüm DocumentDB veritabanını kullanır; çözüm portalı da görüntülenmeleri ve düzenlenmeleri amacıyla verileri bu DocumentDB veritabanından alır.

Çözüm bu bilgileri, DocumentDB veritabanı içeriğiyle eşitlenmiş cihaz kimliği kayıt defterinde tutmalıdır. **EventProcessorHost**, eşitlemeyi yönetmek için **DeviceInfo** akış analizi işine ait verileri kullanır.

## Çözüm portalı

![Çözüm panosu][img-dashboard]

Çözüm portalı, önceden yapılandırılmış çözümün bir parçası olarak buluta dağıtılan web tabanlı bir UI’dir. Şunları yapmanızı sağlar:

- Bir panoda telemetri ve uyarı geçmişini görüntüleme.
- Yeni cihazları hazırlama.
- Cihazları yönetme ve izleme.
- Komutları belirli cihazlara gönderme.
- Kuralları ve eylemleri yönetme.

Önceden yapılandırılmış bu çözümde, çözüm portalı tipik bir [IoT çözüm mimarisinde][lnk-what-is-azure-iot] **Iot çözümü arka ucu**’nun bir parçasını ve **İşleme ve iş bağlantısı**’nın bir parçasını oluşturur.

## Sonraki adımlar

IoT çözümü mimarileri hakkında daha fazla bilgi için bkz. [Microsoft Azure IoT hizmetleri: Başvuru Mimarisi][lnk-refarch].

IoT önceden yapılandırılmış çözümleri hakkında daha fazla bilgi edinmek için şu kaynaklara gidin:

- [IoT önceden yapılandırılmış çözümlerine giriş][lnk-preconf-get-started].
- [Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış][lnk-predictive-maintenance]

[img-remote-monitoring-arch]: ./media/iot-suite-what-are-preconfigured-solutions/remote-monitoring-arch1.png
[img-dashboard]: ./media/iot-suite-what-are-preconfigured-solutions/dashboard.png
[lnk-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[llnk-event-processor]: ../event-hubs/event-hubs-programming-guide.md#event-processor-host
[lnk-web-job]: ../app-service-web/web-sites-create-web-jobs.md
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-identity-registry]: ../iot-hub/iot-hub-devguide.md#device-identity-registry
[lnk-suite-overview]: iot-suite-overview.md
[lnk-preconf-get-started]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-predictive-maintenance]: iot-suite-predictive-overview.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf


<!--HONumber=Jun16_HO2-->


