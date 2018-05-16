---
title: Tahmine Dayalı Bakım çözüm hızlandırıcısı adım adım kılavuzu - Azure | Microsoft Docs
description: Azure IoT Tahmine Dayalı Bakım çözüm hızlandırıcısı adım adım kılavuzu.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 3c48a716-b805-4c99-8177-414cc4bec3de
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2017
ms.author: dobett
ms.openlocfilehash: 397ac3c8b9caa5c392aff4683df2db3b2144899b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="predictive-maintenance-solution-accelerator-walkthrough"></a>Tahmine Dayalı Bakım çözüm hızlandırıcısı adım adım kılavuzu

Tahmine Dayalı Bakım çözümü, arızanın oluşma ihtimali olan anı tahmin eden iş senaryosu için uçtan uca bir çözümdür. Bu çözüm hızlandırıcısını, bakım iyileştirmesi gibi etkinlikler için proaktif olarak kullanabilirsiniz. Çözüm; IoT Hub, Akış analizi ve [Azure Machine Learning][lnk-machine-learning] çalışma alanı gibi önemli Azure IoT çözüm hızlandırıcılarını birleştirir. Bu çalışma alanı, bir uçak motorunun Kalan Kullanım Ömrü’nü (RUL) öngörmek için genel bir örnek veri kümesini temel alan bir model içerir. Bu çözüm, kendinize özel iş gereksinimlerinizi karşılayacak bir çözümü planlamanız ve uygulamanız amacıyla sizin için bir başlangıç noktası olarak IoT iş senaryosunu tam olarak uygular.

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram, çözüm hızlandırıcısının mantıksal bileşenlerinin ana hatlarını vermektedir:

![][img-architecture]

Mavi öğeler, çözüm hızlandırıcısını dağıttığınız bölgede sağlanan Azure hizmetleridir. Çözüm hızlandırıcısını dağıtabileceğiniz bölgelerin listesi, [sağlama sayfasında][lnk-azureiotsuite] görüntülenir.

Yeşil öğe uçak motorunu temsil eden sanal cihazdır. [Sanal cihazlar](#simulated-devices) bölümde bu sanal cihazlarla ilgili daha fazla bilgiye ulaşabilirsiniz.

Gri öğeler, *cihaz yönetimi* becerilerini uygulayan bileşenleri temsil eder. Tahmine Dayalı Bakım çözüm hızlandırıcısının geçerli sürümü, bu kaynakları sağlamaz. Cihaz yönetimi hakkında daha fazla bilgi edinmek için [önceden yapılandırılmış uzaktan izleme çözümü][lnk-remote-monitoring] konusuna bakın.

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


## <a name="next-steps"></a>Sonraki adımlar
Tahmine Dayalı Bakım çözüm hızlandırıcısının temel bileşenlerini gördüğünüze göre bunları özelleştirmek isteyebilirsiniz. Bkz. [Çözüm hızlandırıcılarını özelleştirme kılavuzu][lnk-customize].

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [IoT çözüm hızlandırıcıları için sık sorulan sorular][lnk-faq]
* [Baştan sona IoT güvenliği][lnk-security-groundup]

[img-architecture]: media/iot-suite-predictive-walkthrough/architecture.png

[lnk-remote-monitoring]: iot-suite-v1-remote-monitoring-sample-walkthrough.md
[lnk-cortana-analytics]: http://gallery.cortanaintelligence.com/Collection/Predictive-Maintenance-Template-3
[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-faq]: iot-suite-v1-faq.md
[lnk-security-groundup]: securing-iot-ground-up.md
[lnk-machine-learning]: https://azure.microsoft.com/services/machine-learning/