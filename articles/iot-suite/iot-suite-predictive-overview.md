---
title: "Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış | Microsoft Belgeleri"
description: "Azure IoT Paketi önceden yapılandırılmış tahmine dayalı bakım çözüm açıklaması."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: b370b3d7-2ce5-4906-9818-3aeedd471ee3
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: caecd0d6e546d4ddef71ebeee6fad3a5c7ba6451
ms.openlocfilehash: 5447dff1ff9aea1b90f7e18499f795e7c173e2b9


---
# <a name="predictive-maintenance-preconfigured-solution-overview"></a>Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış

Önceden yapılandırılmış *tahmine dayalı bakım* [önceden yapılandırılmış çözümü][lnk_preconfigured_solutions], önceden yapılandırılmış [Microsoft Azure IoT Paketi][lnk_iot_suite] çözümlerinden biridir. Bu çözüm, gerçek zamanlı cihaz telemetri koleksiyonunu [Azure Machine Learning][lnk-machine-learning] kullanılarak oluşturulan tahmine dayalı modelle tümleştirir.

Azure IoT Paketi’yle bir kurum varlıklara hızlı ve kolay bağlanıp bunları izleyebilir ve verileri gerçek zamanlı analiz edebilir. Önceden yapılandırılmış tahmine dayalı bakım çözümü bu verileri alır, verimlilikleri yönetebilen ve gelir akışlarını geliştiren yeni bilgiler sağlamak için panolar ve görselleştirmeleri kullanır.

## <a name="the-scenario"></a>Senaryo
Fabrikam, rekabetçi fiyatlarıyla büyük müşteri deneyimine odaklanan bölgesel bir havayoludur. Uçuş rötarlarının bir nedeni bakım sorunlarıdır ve uçak motorunun bakımı özellikle zordur. Maliyeti ne olursa olsun, uçuş sırasında motor arızasından kaçınılmalıdır; bu nedenle, Fabrikam düzenli olarak motorları muayene eder ve zamanlanmış bakım programına uygun davranır. Ancak, uçak motorları her zaman aynı şekilde yıpranmaz. Motorlardı bazı gereksiz bakımlar gerçekleştirilir. Daha da önemlisi, bakım yapılana kadar çıkan sorunlar nedeniyle uçağın yerde kalmasıdır. Bu sorunlar, özellikle de uçak doğru teknisyenlerin ve yedek parçaların olmadığı bir yerdeyse maliyetli gecikmelere neden olur.

Fabrikam uçağının motorları, uçuş sırasında motor koşullarını izleyen algılayıcılarla donatılmıştır. Fabrikam, uçuş sırasında toplanan algılayıcı verilerini toplamak için tahmine dayalı bakım için önceden yapılandırılmış çözümü kullanır. Motor çalışması ve arızaları verilerini yıllarca biriktirdikten sonra, Fabrikam’ın veri bilim insanları uçak motorunun Kalan Kullanım Ömrü’nü (RUL) tahmin etmek için bir yol modeli oluşturmuştur. Bunların tanımladıkları, er geç ortaya çıkacak arızalara neden olan motor yıpranmasıyla ilişkili motor algılayıcılarından dördüne ait veriler arasındaki bağıntıdır. Fabrikam güvenliği sağlamak için normal muayeneleri yapmaya devam ederken, her uçuştan sonra her motor için RUL hesaplayacak modelleri kullanmaktadır. Bu model, uçuş sırasında motorlardan toplanan telemetriyi kullanır. Fabrikam artık arızanın ve bakım planının ileri tarihli noktalarını tahmin edebiliyor ve önceden onarıyor.

> [!NOTE]
> Çözüm modeli asıl motor yıpranması verilerini kullanır.

Fabrikam, maliyet düşürmek amacıyla bakımın gerektiği noktayı tahmin ederek çalışmasını en iyi hale getirebilir. Bakım düzenleyicileri aşağıdaki amaçlarla zamanlayıcılar ile çalışır:

- Bakımı uçak belirli bir konumda durduğunda başlayacak şekilde planlamak için.
- Uçağın zamanlamayı bozmadan yeterli süreyle hizmet dışı kaldığından emin olmak için.
- Teknisyenleri, uçağı bekleme süresi olmadan verimli bir şekilde servise sokmayı sağlayacak şekilde zamanlamak için.

Stok denetimi yöneticileri bakım planlarını alır; bu nedenle, sipariş sürecini ve yedek parça stoğunu en iyi hale getirebilirler. Bu faktörlerin tümü, bir yandan da yolcuların ve personelin güvenliğini sağlayarak Fabrikam’ın uçağın yerdeki süresini en aza indirmesini ve çalıştırma maliyetini düşürmesini sağlar.

[Azure IoT Paketi][lnk_iot_suite]'nin, müşterilerin gerçekleştirmeleri gereken tahmine dayalı bakım potansiyeli becerilerini nasıl sağladığını anlamak için bu [bilgi görselini][lnk_infographic] gözden geçirin.

## <a name="how-the-predictive-maintenance-solution-is-built"></a>Tahmine dayalı bakım çözümünü yapılandırma

Çözüm, IoT Paketi hizmetleriyle toplanan cihaz telemetrisinden bu becerileri göstermek için şablon olarak kullanılabilen mevcut Azure Machine Learning modelini geliştirir. Microsoft, genel kullanıma sunulan verileri<sup>\[1\]</sup> temel alarak uçak motorunun bir [gerileme modelini][lnk_regression_model] oluşturmuştur ve modelin nasıl kullanılacağını gösteren adım adım yönergeleri yayımlamıştır.

Azure IoT tahmine dayalı bakım için önceden yapılandırılmış çözümü, bu şablondan oluşturulan gerileme modelini kullanır. Bu model, Azure aboneliğinize dağıtılır ve otomatik olarak oluşturulan bir API ile kullanıma sunulur. Çözümde, 4 (toplamda 100) motoru temsil eden test verilerinin bir alt kümesi ve 4 (toplamda 21) algılayıcı veri akışı bulunur. Bu veriler, eğitilmiş modelden doğru bir sonuç elde etmek için yeterlidir.

*\[1\] A. Saxena ve K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set" (Turbofan Motor Bozulması Benzetimi Veri Kümesi), NASA Ames Prognostics Veri Deposu (http://ti.arc.nasa.gov/tech/dash/pcoe/prognostic-data-repository/), NASA Ames Research Center, Moffett Field, CA*

## <a name="get-started-with-predictive-maintenance"></a>Tahmine dayalı bakım ile çalışmaya başlama

Bu öğreticide tahmine dayalı bakım çözümünün nasıl sağlanacağı gösterilmektedir. Ayrıca, tahmine dayalı bakım çözümünün temel özelliklerinde rehberlik sağlar. Bu özelliklerin birçoğuna önceden yapılandırılmış çözüm ile birlikte dağıtılan çözüm panosundan erişebilirsiniz.

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

> [!NOTE]
> Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].

1. Azure hesabı kimlik bilgilerinizi kullanarak [azureiotsuite.com][lnk-azureiotsuite] adresinde oturum açın ve çözüm oluşturmak için **+** seçeneğine tıklayın.
1. **Tahmine dayalı bakım** kutucuğunu **seçin**.
1. Tahmine dayalı bakım için önceden yapılandırılmış çözümünüze ait bir **Çözüm adı** girin.
1. Çözümü sağlamak için kullanmak istediğiniz **Bölge** ve **Abonelik** seçimini yapın.
1. Hazırlama işlemini başlatmak için **Çözümü Oluştur**'a tıklayın. Bu işlemin çalışması genellikle birkaç dakika sürer.

### <a name="wait-for-the-provisioning-process-to-complete"></a>Hazırlama işleminin tamamlanmasını bekleme

1. Çözümünüzün **Hazırlama** durumuna sahip olan kutucuğuna tıklayın.
1. Azure hizmetleri Azure aboneliğinize dağıtılırken **Hazırlama durumlarına** dikkat edin.
1. Hazırlama tamamlandığında durum **Hazır** olarak değişir.
1. Kutucuğa tıkladığınızda sağ bölmede çözümünüzün ayrıntılarını görürsünüz. Bu bölmeden çözüm panosunu başlatabilir ve Machine Learning çalışma alanına erişebilirsiniz.

> [!NOTE]
> Önceden yapılandırılmış çözümün dağıtımında sorunlarla karşılaşırsanız bkz. [Azureiotsuite.com sitesindeki izinler][lnk-permissions] ve [SSS][lnk-faq]. Sorunlar devam ederse [portalda][lnk-portal] bir hizmet bileti oluşturun.

Görmeyi beklediğiniz ancak çözümünüz için listelenmemiş ayrıntılar mı var? [User Voice](https://feedback.azure.com/forums/321918-azure-iot)'da bize özellik önerileri verin.

## <a name="view-the-solution"></a>Çözümü görüntüleme

Bu bölüm çözüm kullanıcı arabiriminde size yol gösterir.

### <a name="predictive-maintenance-dashboard"></a>Tahmine Dayalı Bakım Panosu
Web uygulamasındaki bu sayfa PowerBI JavaScript denetimlerini (bkz. [PowerBI-visuals repository][lnk-powerbi]) kullanarak şunları görselleştirir:

* Blob depolamada Stream Analytics işlerine ait çıktı verileri.
* Uçak motoru başına RUL ve döngüsü sayısı.

### <a name="observing-the-behavior-of-the-cloud-solution"></a>Bulut çözümünün davranışını gözlemleme
Azure portalda sağlanan kaynaklarınızı görüntülemek için seçtiğiniz çözüm adına sahip kaynak grubuna gidin.

![][img-resource-group]

Önceden yapılandırılmış çözümü hazırlarken, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. Sağladığınız çözüm **Hazır** durumunda olduğunda çözümün [azureiotsuite.com][lnk-azureiotsuite] sayfasındaki kutucuktan da Machine Learning çalışma alanına gidebilirsiniz.

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

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT’nin tahmine dayalı bakım senaryolarını etkinleştirmesi hakkında daha fazla bilgi için [Nesnelerin İnterneti’nden değer yakalama][lnk_capture_value] makalesini okuyun.

Tahmine dayalı önceden yapılandırılmış çözümün [adım adım kılavuzunu][lnk-predictive-walkthrough] inceleyin.

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [IoT Paketi için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[img-resource-group]: media/iot-suite-predictive-overview/resource-group.png
[img-simulation-stopped]: media/iot-suite-predictive-overview/simulation-stopped.png
[img-simulation-running]: media/iot-suite-predictive-overview/simulation-running.png
[img-simulation-warning]: media/iot-suite-predictive-overview/simulation-warning.png
[img-machine-learning]: media/iot-suite-predictive-overview/machine-learning.png

[lnk-powerbi]: https://www.github.com/Microsoft/PowerBI-visuals
[lnk-predictive-walkthrough]: iot-suite-predictive-walkthrough.md
[lnk_preconfigured_solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk_iot_suite]: iot-suite-overview.md
[lnk_infographic]: https://www.microsoft.com/server-cloud/predictivemaintenance/Index.html
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3

[lnk_capture_value]: http://download.microsoft.com/download/0/7/D/07D394CE-185D-4B96-AC3C-9B61179F7080/Capture_value_from_the_Internet%20of%20Things_with_Predictive_Maintenance.PDF
[lnk-faq]: iot-suite-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-permissions]: iot-suite-permissions.md
[lnk-portal]: http://portal.azure.com/
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/


<!--HONumber=Feb17_HO1-->


