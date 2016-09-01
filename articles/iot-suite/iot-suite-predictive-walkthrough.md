<properties
 pageTitle="Tahmine dayalı bakımda gezinme | Microsoft Azure"
 description="Azure IoT önceden yapılandırılmış tahmine dayalı bakım çözümü gezintisi."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="aguilaaj"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/17/2016"
 ms.author="araguila"/>

# Önceden yapılandırılmış tahmine dayalı bakım çözümünde gezinme

## Giriş

IoT Paketi önceden yapılandırılmış tahmine dayalı bakım çözümü, arıza oluştuğu sırada noktayı tahmin eden iş senaryosu için uçtan uca bir çözümüdür. Bu önceden yapılandırılmış çözümü, bakım iyileştirmesi gibi etkinlikler için proaktif olarak kullanabilirsiniz. Çözüm, bir [Azure Machine Learning][lnk_machine_learning] çalışma alanı dahil olmak üzere önemli Azure IoT Paketi hizmetlerini birleştirir. Bu çalışma alanı, bir uçak motorunun Kalan Kullanım Ömrü’nü (RUL) öngörmek için genel bir örnek veri kümesini temel alan denemeler içerir. Bu çözüm, kendinize özel iş gereksinimlerinizi karşılayacak bir çözümü planlamanız ve uygulamanız amacıyla sizin için bir başlangıç noktası olarak IoT iş senaryosunu tam olarak uygular.

## Mantıksal mimari

Aşağıdaki diyagram önceden yapılandırılmış çözümün mantıksal bileşenlerinin ana hatların vermektedir:

![][img-architecture]

Önceden yapılandırılmış çözümü hazırladığınızda seçtiğiniz konumda hazırlanan Azure hizmetleri mavi olan öğelerdir. Önceden yapılandırılmış çözümü Doğu ABD, Kuzey Avrupa veya Doğu Asya bölgesinde hazırlayabilirsiniz.

Bazı kaynaklar, önceden yapılandırılmış çözümü hazırladığınız bölgelerde olmayabilir. Diyagramdaki turuncu öğeler, söz konusu seçili bölgeye en yakın Azure hizmetlerinin hazırlandığı uygun bölgeyi (Güney Merkez ABD, Batı Avrupa veya Güneydoğu Asya) temsil eder.

Yeşil öğe uçak motorunu temsil eden sanal cihazdır. Aşağıdaki bölümde bu sanal cihazlarla ilgili daha fazla bilgiye ulaşabilirsiniz.

Gri öğeler, *cihaz yönetimi* becerilerini uygulayan bileşenleri temsil eder. Önceden yapılandırılmış tahmine dayalı bakım çözümü bu kaynakları hazırlamaz. Cihaz yönetimi hakkında daha fazla bilgi edinmek için [önceden yapılandırılmış uzaktan izleme çözümü][lnk-remote-monitoring] konusuna bakın.

## Sanal cihazlar

Önceden yapılandırılmış çözümde sanal cihaz uçak motorunu temsil eder. Çözüm, tek bir uçakla eşlenen 2 motorla sağlanır. Her motor dört tür telemetri yayar: Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15, Machine Learning modelinin bu motorun Kalan Kullanım Ömrü’nü (RUL) hesaplaması için gereken verileri sağlar. Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:

*Döngü sayısı*. Bir döngü, 2-10 saat arası değişken bir uzunluğa sahip olan ve uçuş sırasında her yarım saatte bir telemetri verilerinin yakalandığı tamamlanmış bir uçuşu temsil eder.

*Telemetri*. Motor özniteliklerini temsil eden dört algılayıcı vardır. Bu algılayıcılar genel olarak Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15 olarak etiketlenir. Bu 4 algılayıcı RUL için Machine Learning modelinden yararlı sonuçlar almak yeterli telemetriyi temsil eder. Bu model, gerçek motor algılayıcı verilerinin bulunduğu ortak bir veri kümesinden oluşturulur. Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

Sanal cihazlar IoT hub'ından gönderilen şu komutları işleyebilir:

| Komut | Açıklama |
|---------|-------------|
| StartTelemetry | Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini başlatır     |
| StopTelemetry  | Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini durdurur |

IoT hub'ı cihaz komut bildirim sağlar.

## Azure Stream Analytics işi

**İş: Telemetri**, gelen cihaz telemetrisi akışını iki durumu kullanarak çalıştırır. Önce, cihazlardan tüm telemetriyi seçer ve bu verileri web uygulamasında görselleştirildiği yerden blob depolamaya gönderir. İkinci durum, iki dakikalık kayan pencere üzerinde ortalama algılayıcı değerlerini ölçer ve bu verileri Olay hub'ı aracılığıyla **olay işlemcisi**’ne gönderir.

## Olay işlemcisi

**Olay işlemcisi**, tamamlanan bir döngü için ortalama algılayıcı değerlerini alır. Bu değerleri bir motorun RUL değerini hesaplaması için Machine Learning eğitilmiş modelinin kullanımına sunan bir API’ye geçirir.

## Azure Machine Learning

Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

## Haydi dolaşmaya başlayalım

Bu bölümde, çözüm bileşenlerinde gezineceksiniz; burada hedeflenen kullanım örneğini açıklanacak ve örnekler verilecektir.

### Tahmine Dayalı Bakım Panosu

Web uygulamasındaki bu sayfa PowerBI JavaScript denetimlerini (bkz. [PowerBI-visuals repository][lnk-powerbi]) kullanarak şunları görselleştirir:

- Blob depolamada Stream Analytics işlerine ait çıktı verileri.
- Uçak motoru başına RUL ve döngüsü sayısı.

### Bulut çözümünün davranışını gözlemleme

Azure portalda sağlanan kaynaklarınızı görüntülemek için seçtiğiniz çözüm adına sahip kaynak grubuna gidin.

![][img-resource-group]

Önceden yapılandırılmış çözümü hazırlarken, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. Sağladığınız çözüm **Hazır** durumunda olduğunda çözümün [azureiotsuite.com][lnk-azureiotsuite] sayfasından da Machine Learning çalışma alanına gidebilirsiniz.

![][img-machine-learning]

Çözüm portalında, uçak başına her biri dört algılayıcı içeren iki motorun düştüğü iki uçağı temsil etmek için örneğin dört sanal cihazla dağıtıldığını görebilirsiniz. Çözüm portalına ilk gittiğinizde benzetim durdurulur.

![][img-simulation-stopped]

Algılayıcı geçmişi, RUL, Döngüler ve RUL geçmişinin panoyu doldurduğunu görebileceğiniz benzetimi başlatmak için **Benzetimi başlat**’a tıklayın.

![][img-simulation-running]

RUL değeri 160’tan (gösterim amaçlı seçilen rastgele bir eşik) azsa, çözüm portalı RUL görüntüsünün yanında bir uyarı simgesi görüntüler ve uçak motorunu sarı renkle vurgular. RUL değerlerinde topluca genel bir düşüş eğilimi olsa da aşağı ve yukarı sıçramalar da olduğunu fark edebilirsiniz. Bu davranış, değişen döngü uzunlukları ve model doğruluğundan sonuçlanır.

![][img-simulation-warning]

Tam benzetim, 148 döngüyü tamamlamak için yaklaşık 35 dakika sürer. 160 RUL eşiği ilk seferinde yaklaşık 5 dakikayı karşılar, her iki motor da yaklaşık 8 dakikada eşiği yakalar.

Benzetim 148 döngü için tam veri kümesinde çalışır, son RUL ve döngü değerlerinde de kapanır.

Benzetimi istediğiniz an durdurabilirsiniz; ancak, **Benzetimi Başlat**’a tıkladığınızda benzetim veri kümesinin başından başlayarak yeniden oynatılır.

## Sonraki adımlar

Şimdi de değiştirmek istediğiniz önceden yapılandırılmış tahmine dayalı bakım çözümünü çalıştırıyorsunuz; bkz.[Önceden yapılandırılmış çözümleri özelleştirme kılavuzu][lnk-customize].

 [IoT Paketi - Başlık Altında - Tahmine Dayalı Bakım](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet blog gönderisi önceden yapılandırılmış tahmine dayalı bakım çözümü hakkında ek ayrıntılar sağlar.

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

- [IoT Paketi hakkında sık sorulan sorular][lnk-faq]
- [Her yönüyle IoT güvenliği][lnk-security-groundup]


[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-suite-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-suite-predictive-walkthrough/machine-learning.png
[img-simulation-stopped]: media/iot-suite-predictive-walkthrough/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-walkthrough/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-walkthrough/simulation-warning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk-remote-monitoring]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md



<!--HONumber=Aug16_HO4-->


