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
 ms.date="05/16/2016"
 ms.author="araguila"/>

# Önceden yapılandırılmış tahmine dayalı bakım çözümünde gezinme

## Giriş

IoT Paketi önceden yapılandırılmış tahmine dayalı bakım çözümü, arıza oluştuğu sırada noktayı tahmin eden iş senaryosu için uçtan uca bir çözümüdür. Önceden yapılandırılmış bu çözümü bakımın iyileştirilmesi gibi etkinlikler için öngörülebilir olarak geliştirebilirsiniz. Bu çözüm, ortak örnek veri kümesine dayandırılan uçak motorunun Kalan Kullanım Ömrü’nü (RUL) tahmin etmeye yönelik deneylerin bulunduğu [Azure Machine Learning][lnk_machine_learning] çalışma alanını da kapsayan önemli Azure IoT Paketi hizmetlerini birleştirir. Size özel iş gereksinimlerinizi karşılayacak IoT çözümünün bu türünü planlamak ve uygulamak amacıyla bu çözüm, sizin için bir başlangıç noktası olarak iş senaryosunun tam uygulamasını sağlar.

## Mantıksal mimari

Aşağıdaki diyagram önceden yapılandırılmış çözümün mantıksal bileşenlerinin ana hatların vermektedir:

![][img-architecture]

Önceden yapılandırılmış çözümü hazırladığınızda seçtiğiniz konumda hazırlanan Azure hizmetleri mavi olan öğelerdir. Önceden yapılandırılmış çözümü Doğu ABD, Kuzey Avrupa veya Doğu Asya bölgesinde hazırlayabilirsiniz.

Bazı kaynaklar, önceden yapılandırılmış çözümü hazırladığınız bölgelerde olmayabilir. Diyagramdaki turuncu öğeler, söz konusu seçili bölgeye en yakın Azure hizmetlerinin hazırlandığı uygun bölgeyi (Güney Merkez ABD, Batı Avrupa veya Güneydoğu Asya) temsil eder.

Yeşil öğe uçak motorunu temsil eden sanal cihazdır. Bu sanal makinelere yönelik daha fazla bilgiye aşağıdan ulaşabilirsiniz.

Gri öğeler, *cihaz yönetimi* becerilerini uygulayan bileşenleri temsil eder. Önceden yapılandırılmış tahmine dayalı bakım çözümü bu kaynakları hazırlamaz. Cihaz yönetimi hakkında daha fazla bilgi edinmek için [önceden yapılandırılmış uzaktan izleme çözümü][lnk-remote-monitoring] konusuna bakın.

## Sanal cihazlar

Önceden yapılandırılmış çözümde sanal cihaz uçak motorunu temsil eder. Çözüm, tek uçakla eşlenen 2 motorla hazırlanır. Her motor 4 tür telemetri yayar: Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15; bunlar, bu motor için Kalan Kullanım Ömrü’nü (RUL) hesaplayacak Machine Learning modeline gereken verileri sağlar. Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:

*Döngü sayısı*. Döngü, uçuş süresince her yarım saatte bir telemetri verilerinin alındığı 2-10 saat arası uzunlukta değişkenin tamamlanan uçuşunu temsil eder.

*Telemetri*. Motor özniteliklerini temsil eden 4 algılayıcı vardır. Bu algılayıcılar genel olarak Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15 olarak etiketlenir. Bu 4 algılayıcı RUL için Machine Learning modelinden yararlı sonuçlar almak yeterli telemetriyi temsil eder. Bu model, gerçek motor algılayıcı verilerinin bulunduğu ortak bir veri kümesinden oluşturulur. Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

Sanal cihazlar IoT hub'ından gönderilen şu komutları işleyebilir:

| Komut | Açıklama |
|---------|-------------|
| StartTelemetry | Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini başlatır     |
| StopTelemetry  | Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini durdurur |

IoT hub'ı cihaz komut bildirim sağlar.

## Azure Stream Analytics işi

**İş: Telemetri**, gelen cihaz telemetrisi akışını iki durumu kullanarak çalıştırır. Önce, cihazlardan tüm telemetriyi seçer ve bu verileri web uygulamasında görselleştirildiği yerden blob depolamaya gönderir. İkinci durum, iki dakikalık kayan pencere üzerinde ortalama algılayıcı değerlerini ölçer ve bu verileri Olay hub'ı aracılığıyla **olay işlemcisi**’ne gönderir.

## Olay işlemcisi

**Olay işlemcisi** tamamlanmış döngünün ortalama algılayıcı değerlerini alır ve bu değerleri bir API’ye geçirir; bu API, motor için RUL hesaplamak amacıyla Machine Learning eğitilen modelini gösterir.

## Azure Machine Learning

Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

## Haydi dolaşmaya başlayalım

Bu bölümde, çözüm bileşenlerinde gezineceksiniz; burada hedeflenen kullanım örneğini açıklanacak ve örnekler verilecektir.

### Tahmine Dayalı Bakım Panosu

Web uygulamasındaki bu sayfa PowerBI JavaScript denetimlerini (bkz. [PowerBI-visuals repository][lnk-powerbi]) kullanarak şunları görselleştirir:

- Blob depolamada Stream Analytics işlerine ait çıktı verileri.
- Uçak motoru başına RUL ve döngüsü sayısı.

### Bulut çözümünün davranışını gözlemleme

Hazırlanan kaynaklarınızı Azure portalına göz atarak ve seçtiğiniz çözüm adına sahip kaynak grubunda gezinerek görüntüleyebilirsiniz.

![][img-resource-group]

Önceden yapılandırılmış çözümü hazırlarken, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. **Hazır** durumunda olduğunda hazırlanan çözüm için bu Machine Learning çalışma alanına [azureiotsuite.com][lnk-azureiotsuite] sayfasından da gidebilirsiniz.

![][img-machine-learning]

Çözüm portalında, örneğin dört sanal cihazla hazırlandığını görebilirsiniz; uçak başına 2 motorlu 2 uçak ve motor başına 4 algılayıcı temsil etmektedir. Çözüm portalına ilk gittiğinizde benzetim durdurulur.

![][img-simulation-stopped]

Panoyu dolduran algılayıcı geçmişi, RUL, Döngüler ve RUL geçmişini görebildiğiniz benzetimi başlatmak için **Benzetimi başlat**’a tıklayın.

![][img-simulation-running]

RUL değeri 160 altındaysa (gösterim amaçlı seçilen rastgele eşik), çözüm portalı RUL görüntüsünün yanında bir uyarı simgesi görüntüler ve uçak motorunu sarı renkli bir resimde boyar. RUL değerlerinde topluca genel bir düşüş eğilimi olsa da aşağı ve yukarı sıçramalar da olduğunu fark edeceksiniz. Değişen döngü uzunlukları ve model doğruluğundan sonuçlanır.

![][img-simulation-warning]

Tam benzetim, 148 döngüyü tamamlamak için yaklaşık 35 dakika sürer. 160 RUL eşiği ilk seferinde yaklaşık 5 dakikayı karşılar, her iki motor da yaklaşık 8 dakikada eşiği yakalar.

Benzetim 148 döngü için tam veri kümesinde çalışır, son RUL ve döngü değerlerinde de kapanır.

Benzetimi istediğiniz an durdurabilirsiniz; ancak, **Benzetimi Başlat**’a tıkladığınızda benzetim veri kümesinin başından başlayarak yeniden oynatılır.

## Sonraki adımlar

Şimdi de değiştirmek istediğiniz önceden yapılandırılmış tahmine dayalı bakım çözümünü çalıştırıyorsunuz; bkz.[Önceden yapılandırılmış çözümleri özelleştirme kılavuzu][lnk-customize].

 [IoT Paketi - Başlık Altında - Tahmine Dayalı Bakım](http://social.technet.microsoft.com/wiki/contents/articles/33527.iot-suite-under-the-hood-predictive-maintenance.aspx) TechNet blog gönderisi önceden yapılandırılmış tahmine dayalı bakım çözümü hakkında ek ayrıntılar sağlar.

  
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



<!---HONumber=Jun16_HO2-->


