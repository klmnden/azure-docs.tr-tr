---
title: Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış - Azure | Microsoft Docs
description: Azure IOT Tahmine dayalı bakım Çözüm Hızlandırıcısı genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: 3387996dc0e1953eaafee9c4c61eb8faa865b654
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61447547"
---
# <a name="predictive-maintenance-solution-accelerator-overview"></a>Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış

Tahmine Dayalı Bakım çözümü, arızanın oluşma ihtimali olan anı tahmin eden iş senaryosu için uçtan uca bir çözümdür. Bu çözüm hızlandırıcısını, bakım iyileştirmesi gibi etkinlikler için proaktif olarak kullanabilirsiniz. Çözüm önemli Azure IOT Çözüm Hızlandırıcıları hizmetlerin, IOT Hub gibi bir araya getirir ve [Azure Machine Learning] [ lnk-machine-learning] çalışma. Bu çalışma alanı, bir uçak motorunun Kalan Kullanım Ömrü’nü (RUL) öngörmek için genel bir örnek veri kümesini temel alan bir model içerir. Bu çözüm, kendinize özel iş gereksinimlerinizi karşılayacak bir çözümü planlamanız ve uygulamanız amacıyla sizin için bir başlangıç noktası olarak IoT iş senaryosunu tam olarak uygular.

Tahmine dayalı bakım Çözüm Hızlandırıcısı [kodu Github'da kullanılabilir](https://github.com/Azure/azure-iot-predictive-maintenance).

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, çözüm hızlandırıcısının mantıksal bileşenlerinin ana hatlarını vermektedir:

![Mantıksal mimari][img-architecture]

Mavi öğeler, çözüm hızlandırıcısını dağıttığınız bölgede sağlanan Azure hizmetleridir. Çözüm hızlandırıcısını dağıtabileceğiniz bölgelerin listesi, [sağlama sayfasında][lnk-azureiotsolutions] görüntülenir.

Yeşil öğe uçak sanal altyapısıdır. [Sanal cihazlar](#simulated-devices) bölümde bu sanal cihazlarla ilgili daha fazla bilgiye ulaşabilirsiniz.

Gri öğeler uygulayan bileşenlerdir *cihaz Yönetimi* özellikleri. Tahmine Dayalı Bakım çözüm hızlandırıcısının geçerli sürümü, bu kaynakları sağlamaz. Cihaz yönetimi hakkında daha fazla bilgi için bkz [Uzaktan izleme çözüm hızlandırıcısının][lnk-remote-monitoring].

## <a name="azure-resources"></a>Azure kaynakları

Azure portalda sağlanan kaynaklarınızı görüntülemek için seçtiğiniz çözüm adına sahip kaynak grubuna gidin.

![Hızlandırıcı kaynakları][img-resource-group]

Çözüm hızlandırıcısını sağladığınızda, Machine Learning çalışma alanına bağlantısı da olan bir e-posta alırsınız. Machine Learning çalışma alanına gidebilirsiniz [Microsoft Azure IOT Çözüm Hızlandırıcıları] [ lnk-azureiotsolutions] sayfası. Çözüm **Hazır** durumda olduğunda bu sayfada bir kutucuk kullanılabilir.

![Machine learning modeli][img-machine-learning]

## <a name="simulated-devices"></a>Sanal cihazlar

Çözüm içinde bir sanal cihaz uçak motorunun hızlandırıcısıdır. Çözüm, tek bir uçakla eşlenen 2 motorla sağlanır. Her motor dört tür telemetri yayar: Algılayıcı 9, algılayıcı 11, algılayıcı 14 ve algılayıcı 15, Machine Learning modelinin bu motorun rul değerini hesaplaması için gereken verileri sağlar. Her sanal cihaz IoT Hub'ına şu telemetri iletilerini gönderir:

*Döngü sayısı*. Bir döngü, iki ile on saat arasındaki bir süreyle tamamlanmış uçuşu olur. Uçuş sırasında telemetri verileri yarım saatte bir yakalanır.

*Telemetri*. Motor özniteliklerini kayıt dört algılayıcı vardır. Bu algılayıcılar genel olarak Algılayıcı 9, Algılayıcı 11, Algılayıcı 14 ve Algılayıcı 15 olarak etiketlenir. Bu dört algılayıcı RUL modelinden yararlı sonuçlar almak yeterli telemetriyi gönderin. Çözüm hızlandırıcısında kullanılan model, gerçek altyapı algılayıcı verilerinin bulunduğu ortak bir veri kümesinden oluşturulur. Özgün veri kümesinden modelin oluşturulması hakkında daha fazla bilgi için bkz. [Cortana Intelligence Gallery Tahmine Dayalı Bakım Şablonu][lnk-cortana-analytics].

Sanal cihazlar, çözümde IoT hub'ı tarafından gönderilen aşağıdaki komutları işleyebilir:

| Komut | Açıklama |
| --- | --- |
| StartTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini başlatır |
| StopTelemetry |Benzetim durumunu denetler.<br/>Cihazın telemetri göndermesini durdurur |

IoT hub'ı cihaz komut bildirim sağlar.

## <a name="azure-stream-analytics-job"></a>Azure Stream Analytics işi

**İş: Telemetri** iki durumu kullanarak gelen cihaz telemetrisi akışını çalıştırır:

* İlki, cihazlardan tüm telemetriyi seçer ve bu verileri blob depolamaya gönderir. Buradan, bu web uygulamasında görselleştirilir.
* İkinciyse iki dakikalık kayan pencere üzerinde ortalama algılayıcı değerlerini ölçer ve bu verileri Olay hub'ı aracılığıyla **olay işlemcisi**’ne gönderir.

## <a name="event-processor"></a>Olay işlemcisi
**Olay işleyicisi konağı** bir Azure Web İşi’nde çalıştırır. **Olay işlemcisi**, tamamlanan bir döngü için ortalama algılayıcı değerlerini alır. Ardından motor için RUL hesaplar eğitilen bir modeli için bu değerleri geçirir. Bir API, çözümün bir parçası olan Machine Learning çalışma alanı modelde erişim sağlar.

## <a name="machine-learning"></a>Machine Learning
Machine Learning bileşeni gerçek uçak motorlarından toplanan verilerden türetilmiş bir model kullanır. Üzerinde çözümün kutucuğundan Machine Learning çalışma alanına gidebilirsiniz [azureiotsolutions.com] [ lnk-azureiotsolutions] sayfası. Çözüm **Hazır** durumda olduğunda kutucuk kullanılabilir.

Machine Learning modeli, IOT Çözüm Hızlandırıcısı hizmetleriyle toplanan telemetri ile nasıl çalışılacağını gösteren bir şablon olarak kullanılabilir. Microsoft oluşturduğu bir [regresyon modeli] [ lnk_regression_model] herkese verilerini temel alarak uçak motorunun<sup>\[1\]</sup>ve adım adım kılavuzu modelin nasıl kullanılacağını üzerinde.

Azure IoT Tahmine Dayalı Bakım çözümü, bu şablondan oluşturulan regresyon modelini kullanır. Model Azure aboneliğinize dağıtılır ve otomatik olarak oluşturulan bir API kullanılabilir. Çözüm için 4 (toplamda 100) test veri kümesini içeren altyapıları ve 4 (toplamda 21) algılayıcı veri akışı. Bu veriler, eğitilmiş modelden doğru bir sonuç elde etmek için yeterlidir.

*\[1\] A. Saxena ve K. Goebel (2008). "Turbofan Engine Degradation Simulation Data Set" (Turbofan Motor Bozulması Benzetimi Veri Kümesi), NASA Ames Prognostics Veri Deposu (https://c3.nasa.gov/dashlink/resources/139/), NASA Ames Research Center, Moffett Field, CA*)

## <a name="next-steps"></a>Sonraki adımlar
Tahmine Dayalı Bakım çözüm hızlandırıcısının temel bileşenlerini gördüğünüze göre bunları özelleştirmek isteyebilirsiniz.

IOT Çözüm Hızlandırıcıları diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [IoT çözüm hızlandırıcıları için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[img-architecture]: media/iot-accelerators-predictive-walkthrough/architecture.png
[img-resource-group]: media/iot-accelerators-predictive-walkthrough/resource-group.png
[img-machine-learning]: media/iot-accelerators-predictive-walkthrough/machine-learning.png

[lnk-remote-monitoring]: quickstart-predictive-maintenance-deploy.md
[lnk-cortana-analytics]: https://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsolutions]: https://www.azureiotsolutions.com/
[lnk-faq]: iot-accelerators-faq.md
[lnk-security-groundup]:/azure/iot-fundamentals/iot-security-ground-up
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/
[lnk_regression_model]: https://gallery.cortanaanalytics.com/Collection/Predictive-Maintenance-Template-3
