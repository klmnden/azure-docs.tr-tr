---
title: Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış - Azure | Microsoft Docs
description: Azure IOT Tahmine dayalı bakım Çözüm Hızlandırıcısı genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 822e02d42be8ce516901190af4be579d13ac841d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48888331"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış

Tahmine Dayalı Bakım çözümü, arızanın oluşma ihtimali olan anı tahmin eden iş senaryosu için uçtan uca bir çözümdür. Bu çözüm hızlandırıcısını, bakım iyileştirmesi gibi etkinlikler için proaktif olarak kullanabilirsiniz. Çözüm; IoT Hub, Akış analizi ve [Azure Machine Learning][lnk-machine-learning] çalışma alanı gibi önemli Azure IoT çözüm hızlandırıcılarını birleştirir. Bu çalışma alanı, bir uçak motorunun Kalan Kullanım Ömrü’nü (RUL) öngörmek için genel bir örnek veri kümesini temel alan bir model içerir. Bu çözüm, kendinize özel iş gereksinimlerinizi karşılayacak bir çözümü planlamanız ve uygulamanız amacıyla sizin için bir başlangıç noktası olarak IoT iş senaryosunu tam olarak uygular.

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, çözüm hızlandırıcısının mantıksal bileşenlerinin ana hatlarını vermektedir:

![][img-architecture]

Mavi öğeler, çözüm hızlandırıcısını dağıttığınız bölgede sağlanan Azure hizmetleridir. Çözüm hızlandırıcısını dağıtabileceğiniz bölgelerin listesi, [sağlama sayfasında][lnk-azureiotsuite] görüntülenir.

Yeşil öğe uçak motorunu temsil eden sanal cihazdır. [Sanal cihazlar](#simulated-devices) bölümde bu sanal cihazlarla ilgili daha fazla bilgiye ulaşabilirsiniz.

Gri öğeler, *cihaz yönetimi* becerilerini uygulayan bileşenleri temsil eder. Tahmine Dayalı Bakım çözüm hızlandırıcısının geçerli sürümü, bu kaynakları sağlamaz. Cihaz yönetimi hakkında daha fazla bilgi için bkz [Uzaktan izleme çözüm hızlandırıcısının][lnk-remote-monitoring].

## <a name="azure-resources"></a>Azure kaynakları

Azure portalda sağlanan kaynaklarınızı görüntülemek için seçtiğiniz çözüm adına sahip kaynak grubuna gidin.

![Hızlandırıcı kaynakları][img-resource-group]

Çözüm hızlandırıcısını sağladığınızda, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. Machine Learning çalışma alanına gidebilirsiniz [Microsoft Azure IOT Çözüm Hızlandırıcıları] [ lnk-azureiotsuite] sağlanan çözümünüz için sayfa. Çözüm **Hazır** durumda olduğunda bu sayfada bir kutucuk kullanılabilir.

![Machine learning modeli][img-machine-learning]

## <a name="simulated-devices"></a>Sanal cihazlar

Çözüm hızlandırıcısında simülasyon cihazı bir uçak motorunu temsil eder. Çözüm, tek bir uçakla eşlenen 2 motorla sağlanır. Her motor dört tür telemetri yayar: Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15, Machine Learning modelinin bu motorun RUL değerini hesaplaması için gereken verileri sağlar. Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:

*Döngü sayısı*. Bir döngü, iki ile on saat arasındaki bir süreyle tamamlanmış uçuşu temsil eder. Uçuş sırasında telemetri verileri yarım saatte bir yakalanır.

*Telemetri*. Motor özniteliklerini temsil eden dört algılayıcı vardır. Bu algılayıcılar genel olarak Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15 olarak etiketlenir. Bu dört algılayıcı, RUL modelinden yararlı sonuçlar almak için yeterli olan telemetriyi temsil eder. Çözüm hızlandırıcısında kullanılan model, gerçek altyapı algılayıcı verilerinin bulunduğu ortak bir veri kümesinden oluşturulur. Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

Sanal cihazlar, çözümde IoT hub'ı tarafından gönderilen aşağıdaki komutları işleyebilir:

| Komut | Açıklama |
| --- | --- |
| StartTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini başlatır |
| StopTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini durdurur |

IoT hub'ı cihaz komut bildirim sağlar.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics işi

**İş: Telemetri**, gelen cihaz telemetrisi akışını iki durumu kullanarak çalıştırır:

* İlki, cihazlardan tüm telemetriyi seçer ve bu verileri blob depolamaya gönderir. Ardından veriler web uygulamasında görselleştirilir.
* İkinciyse iki dakikalık kayan pencere üzerinde ortalama algılayıcı değerlerini ölçer ve bu verileri Olay hub'ı aracılığıyla **olay işlemcisi**’ne gönderir.

## <a name="event-processor"></a>Olay işlemcisi
**Olay işleyicisi konağı** bir Azure Web İşi’nde çalıştırır. **Olay işlemcisi**, tamamlanan bir döngü için ortalama algılayıcı değerlerini alır. Daha sonra bu değerleri bir motorun RUL değerini hesaplaması için eğitilmiş modelin kullanımına sunan bir API’ye geçirir. API, çözümün bir parçası olarak sağlanan Machine Learning çalışma alanı tarafından kullanıma sunulur.

## <a name="machine-learning"></a>Machine Learning
Machine Learning bileşeni gerçek uçak motorlarından toplanan verilerden türetilmiş bir model kullanır. [Azureiotsuite.com][lnk-azureiotsuite] sayfasındaki çözümün kutucuğundan Machine Learning çalışma alanına gidebilirsiniz. Çözüm **Hazır** durumda olduğunda kutucuk kullanılabilir.

Azure Machine Learning modeli, IOT Çözüm Hızlandırıcıları hizmetleriyle toplanan cihaz telemetrisinden bu becerileri göstermek için bir şablon olarak kullanılabilir. Microsoft oluşturduğu bir [regresyon modeli] [ lnk_regression_model] herkese verilerini temel alarak uçak motorunun<sup>\[1\]</sup>ve adım adım kılavuzu modelin nasıl kullanılacağını üzerinde.

Azure IoT Tahmine Dayalı Bakım çözümü, bu şablondan oluşturulan regresyon modelini kullanır. Bu model, Azure aboneliğinize dağıtılır ve otomatik olarak oluşturulan bir API ile kullanıma sunulur. Çözümde, 4 (toplamda 100) motoru temsil eden test verilerinin bir alt kümesi ve 4 (toplamda 21) algılayıcı veri akışı bulunur. Bu veriler, eğitilmiş modelden doğru bir sonuç elde etmek için yeterlidir.

*\[1\] A. Saxena ve K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set" (Turbofan Motor Bozulması Benzetimi Veri Kümesi), NASA Ames Prognostics Veri Deposu (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*)

## <a name="next-steps"></a>Sonraki adımlar
Tahmine Dayalı Bakım çözüm hızlandırıcısının temel bileşenlerini gördüğünüze göre bunları özelleştirmek isteyebilirsiniz.

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [IoT çözüm hızlandırıcıları için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: http://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
