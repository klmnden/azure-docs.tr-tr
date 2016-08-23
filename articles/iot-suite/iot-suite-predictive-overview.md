<properties
 pageTitle="Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış | Microsoft Azure"
 description="Azure IoT önceden yapılandırılmış tahmine dayalı bakım çözüm açıklaması."
 services=""
 suite="iot-suite"
 documentationCenter=""
 authors="stevehob"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-suite"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="05/24/2016"
 ms.author="stevehob"/>

# Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış

Önceden yapılandırılmış *tahmine dayalı bakım* çözümü [önceden yapılandırılmış çözümlerden][lnk_preconfigured_solutions] biridir; [Microsoft Azure IOT Paketi][lnk_iot_suite]’nin bir parçası olarak yayımlanmıştır. Bu çözüm, gerçek zamanlı cihaz telemetri koleksiyonunu [Azure Machine Learning][lnk_machine_learning] kullanılarak oluşturulan tahmine dayalı modelle tümleştirir.


Azure IoT Paketi’yle, kurumlar varlıklara hızlı ve kolay bağlanıp bunları izleyebilir ve verileri gerçek zamanlı analiz edebilirler. Önceden yapılandırılmış tahmine dayalı bakım çözümü bu verileri alır, verimlilikleri yönetebilen ve gelir akışlarını geliştiren yeni zekaya sahip işler sağlamak için panolar ve görselleştirmeleri kullanır.

## Senaryo

Fabrikam, rekabetçi fiyatlarıyla büyük müşteri deneyimine odaklanan bölgesel bir havayoludur. Uçuş rötarlarının bir nedeni bakım sorunlarıdır ve uçak motorunun bakımı özellikle zordur. Maliyeti ne olursa olsun, uçuş sırasında motor arızasından kaçınılmalıdır; bu nedenle, Fabrikam düzenli olarak motorları muayene eder ve zamanlanmış bakım programına uygun davranır. Ancak, uçak motorları her zaman aynı şekilde yıpranmaz. Motorlardı bazı gereksiz bakımlar gerçekleştirilir. Daha da önemlisi, bakım yapılana kadar çıkan sorunlar nedeniyle uçağın yerde kalmasıdır. Bu, maliyetli bir gecikmedir, özellikle de uçak doğru teknisyenlerin ve yedek parçaların olmadığı bir yerdeyse.

Fabrikam uçağının motorları, uçuş sırasında motor koşullarını izleyen algılayıcılarla donatılmıştır. Fabrikam, uçuş sırasında toplanan algılayıcı verilerini toplamak için Azure IoT Paketi kullanır. Motor çalışması ve arızaları verilerini yıllarca biriktirdikten sonra, Fabrikam’ın veri bilim insanları uçak motorunun Kalan Kullanım Ömrü’nü (RUL) tahmin etmek için bir yol modeli oluşturmuştur. Bunların tanımladıkları, er geç ortaya çıkacak arızalara neden olan motor yıpranmasıyla ilişkili motor algılayıcılarından dördüne ait veriler arasındaki bağıntıdır. Fabrikam güvenliği sağlamak için normal muayeneleri yapmaya devam ederken, bir yandan da uçuş sırasında motorlardan toplanan telemetriyi kullanarak her uçuştan sonra her motor için RUL hesaplayacak modelleri de kullanmaktadır. Fabrikam artık arızanın ve bakım planının ileri tarihli noktalarını tahmin edebiliyor ve önceden onarıyor.

> [AZURE.NOTE] Çözüm modeli asıl motor yıpranması verilerini kullanır.

Fabrikam, maliyet düşürmek amacıyla bakımın gerektiği noktayı tahmin ederek çalışmasını en iyi hale getirebilir. Bakım düzenleyicileri, belirli bir konumda uçağın durdurulmasıyla örtüşen ve planı bozmadan ve uçağın hizmet dışı kalacağı yeterli süreyi sağlayan bakımı planlamak için zamanlayıcılarla çalışırlar. Fabrikam teknisyenleri uygun şekilde zamanlayabilir; uçağın bekleme süresi olmadan verimli bir şekilde servise girmesini sağlar. Stok denetimi yöneticileri bakım planlarını alır; bu nedenle, sipariş sürecini ve yedek parça stoğunu en iyi hale getirebilirler. Bunların tümü, bir yandan da yolcuların ve personelin güvenliğini sağlayarak Fabrikam’ın uçağın yerdeki süresini en aza indirmesini ve çalıştırma maliyetini düşürmesini sağlar.

[Azure IoT Paketi][lnk_iot_suite]’nin, müşterilerin gerçekleştirmeleri gereken tahmine dayalı bakım potansiyeli becerilerini nasıl sağladığını anlamak için lütfen bu [bilgi görselini][lnk_infographic] gözden geçirin.

## Tahmine dayalı bakım çözümünü yapılandırma

Çözüm, IoT Paketi hizmetleriyle toplanan cihaz telemetrisinden bu becerileri göstermek için şablon olarak kullanılabilen mevcut Azure Machine Learning modelini geliştirir. Microsoft’ta uçak motorunun bir [gerileme modeli][lnk_regression_model], veriler<sup>\[1\]</sup> ve tam veri şablonunun yanı sıra modelin nasıl kullanılacağını hakkında adım adım yönergeler de bulunur.

Azure IOT önceden yapılandırılmış tahmine dayalı bakım çözümü bu şablondan oluşturulan gerileme modelini kullanır; Azure aboneliğinize dağıtılır ve otomatik olarak API üretir. Çözümde, 4 (toplamda 100) motoru temsil eden test verilerinin bir alt kümesi ve eğitilen modele ait doğru sonuç sağlayan 4 (toplamda 21) algılayıcı veri akışı bulunur.

*\[1\] A. Saxena ve K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set" (Turbofan Motor Bozulması Benzetimi Veri Kümesi), NASA Ames Prognostics Veri Deposu (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## Sonraki adımlar

Azure IoT’nin tahmine dayalı bakım senaryolarını etkinleştirmesi hakkında daha fazla bilgi için [Nesnelerin İnterneti’nden değer yakalama][lnk_capture_value] makalesini okuyun.

Tahmine dayalı önceden yapılandırılmış çözümün bir [adım adım kılavuzunu][lnk-predictive-walkthrough] edinin.

[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_machine_learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

- [IoT Paketi hakkında sık sorulan sorular][lnk-faq]
- [Her yönüyle IoT güvenliği][lnk-security-groundup]

[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md


<!--HONumber=Aug16_HO1-->


