<properties
    pageTitle="Microsoft Azure IoT Paketi’ne genel bakış | Microsoft Azure"
    description="Azure IoT Paketi, verileri toplamak, çözümlemek ve depolamak, görselleştirmeler sağlamak ve diğer sistemlerle tümleştirmek için önceden yapılandırılmış nesnelerin internetini nasıl sağladığına genel bakış."
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
     ms.date="05/23/2016"
     ms.author="dobett"/>

# Azure IoT Paketi nedir?

Azure nesnelerin interneti (IoT) hizmetleri çok çeşitli özellikler sunar. Bu kurumsal sınıf hizmetler şunları yapmanızı sağlar:

- Cihazlardan veri toplama
- Hareket halinde veri akışı çözümleme
- Büyük veri gruplarını depolama ve sorgulama
- Gerçek zamanlı ve geçmiş verileri görselleştirme
- Arka ofis sistemleriyle tümleştirme

Bu özellikleri sunmak için, Azure IoT Paketi birçok Azure hizmetini özel uzantılarla birlikte *önceden yapılandırılmış çözümler* olarak paketledi. Bu önceden yapılandırılmış çözümler, IoT çözümlerinizi sunmak için geçirmeniz gereken süreyi azaltmak üzere genel IoT çözümü düzenlerinin temel uygulamalarıdır. [IoT yazılım geliştirme setlerini][lnk-sdks] kullanarak, kendi gereksinimlerinizi karşılamak için bu çözümleri özelleştirebilir ve genişletebilirsiniz. Ayrıca bu çözümleri, yeni IoT çözümleri geliştirirken örnekler ya da şablonlar olarak da kullanabilirsiniz.

Aşağıdaki video Azure IoT paketine giriş sağlar:

> [AZURE.VIDEO azurecon-2015-introducing-the-microsoft-azure-iot-suite]

## Azure IoT Paketindeki Azure IoT Hizmetleri

Önceden yapılandırılmış çözümler genellikle aşağıdaki hizmetleri kullanır:

- Azure IoT paketinin çekirdeği [Azure IoT Hub][lnk-iot-hub] hizmetidir. Bu hizmet cihaz-bulut arası ve bulut-cihaz arası ileti gönderme özellikleri sağlar ve buluta ve diğer temel IoT Paketi hizmetlerine ağ geçidi görevi görür. Hizmet cihazınızdan ölçekte mesajlar almanızı ve cihazlarınıza komutlar göndermenizi sağlar.

- [Azure Stream Analytics][lnk-asa] hareket halinde veri çözümleme sağlar. IoT Paketi, gelen telemetri işlemek, toplama gerçekleştirmek ve olayları algılamak için bu hizmeti kullanır. Önceden yapılandırılmış çözümler, meta veriler ya da cihazlardan alınan komut yanıtları gibi verileri içeren bilgi iletilerini işlemek için de akış analizini kullanır. Çözümler cihazlarınızdan gelen iletileri işlemek ve bu iletileri diğer cihazlara göndermek için Stream Analytics kullanır.

- [Azure Storage][lnk-azure-storage] ve [Azure DocumentDB][lnk-document-db] veri depolama özellikleri sağlar. Önceden yapılandırılmış çözümler, telemetri depolamak ve çözümleme için kullanılabilir hale getirmek üzere blob depolamayı kullanır. Çözümler cihaz meta verilerini depolamak ve çözümlerin cihaz yönetimi özelliklerini etkinleştirmek için DocumentDB kullanır.

- [Azure Web Apps][lnk-web-apps] ve [Microsoft Power BI][lnk-power-bi] veri görselleştirme özellikleri sağlar. Power BI esnekliği, IoT Paketi verilerini kullanan kendi etkileşimli panolarınızı hızlı bir şekilde oluşturmanızı sağlar.

Tipik bir IoT çözüm mimarisine genel bakış için bkz. [Microsoft Azure ve Nesnelerin İnterneti (IoT)][iot-suite-what-is-azure-iot].

## Önceden yapılandırılmış çözümler

IoT paketi, Azure IoT Paketi’nin mümkün kıldığı *Uzaktan izleme* ve *Tahmine dayalı bakım* gibi yaygın IoT senaryolarını hızlı şekilde kullanmaya başlamanızı ve keşfetmenizi sağlayan önceden yapılandırılmış çözümler sunar. Bu çözümleri Azure aboneliğinize dağıtabilir ve ardından tam, uçtan uca bir IoT senaryosu çalıştırabilirsiniz.

## Sonraki adımlar

Artık IoT Paketi’nin yapabileceklerine ve temel bileşenlerine ilişkin genel bir bakış sahibi olduğunuza göre şunları yapabilirsiniz:

- IoT Paketi’ndeki önceden yapılandırılmış çözümler hakkında daha fazla bilgi almak için bkz. [Azure IoT önceden yapılandırılmış çözümleri nelerdir?][lnk-what-are-preconfig]

- Önceden yapılandırılmış çözümlerden birini kullanmaya başlamak için bkz. [IoT önceden yapılandırılmış çözümlerini kullanmaya başlama][lnk-preconfig-start].

- Azure IoT Hub hizmeti hakkında daha fazla bilgi almak için bkz. [IoT Hub belgeleri][lnk-iot-hub].


[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-preconfig-start]: iot-suite-getstarted-preconfigured-solutions.md
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/



<!--HONumber=Jun16_HO2-->


