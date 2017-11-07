---
title: "Önceden yapılandırılmış Azure IoT Paketi çözümlerine genel bakış | Microsoft Docs"
description: "Önceden yapılandırılmış Azure IoT Paketi çözümlerinin ve mimarisinin ek kaynak bağlantılarıyla birlikte açıklaması."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.openlocfilehash: 502b7678e0c47f594291409a9ede976dea3895e5
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2017
---
# <a name="what-is-azure-iot-suite"></a>Azure IoT Paketi nedir?

Azure IoT Paketi, aşağıdaki özelliklere sahip olan *önceden yapılandırılmış çözümler* kümesidir:

* Dakikalar içinde dağıtım
* Hızlıca kullanmaya başlama
* Gereksinimlerinizi karşılayacak şekilde özelleştirebilirsiniz

Önceden yapılandırılmış *IoT Paketi* çözümlerinin tümü aynı ilke ve hedeflere göre tasarlanmıştır.

## <a name="preconfigured-solutions-overview"></a>Önceden yapılandırılmış çözümlere genel bakış

Önceden yapılandırılmış çözüm, aboneliği kullanarak Azure’e dağıtabildiğiniz yaygın IoT çözüm modellerinin açık kaynak uygulamasıdır. Önceden yapılandırılmış her çözüm, belirli bir IoT senaryosu veya senaryolarını uygulamak için özel kod ile Azure hizmetlerini birleştirir. Senaryoların herhangi birini gereksinimlerinizi karşılayacak şekilde özelleştirebilirsiniz. Bu senaryolar şunlardır:

* Ayrıntılı öngörüler ve çözüm durumu için zengin bir pano üzerinde verileri görselleştirme.
* Dinamik IoT cihaz telemetrisi üzerinde kurallar ve alarmlar yapılandırma.
* Yazılım güncelleştirmeleri ve yapılandırma gibi cihaz yönetimi işlerini zamanlama.
* Kendi özel fiziksel veya sanal cihazlarınızı sağlama.
* IoT cihaz gruplarınızın içindeki sorunları giderme ve düzeltme.

Önceden yapılandırılmış her çözüm, telemetri oluşturmak için tam, uçtan uca sanal veya fiziksel cihazları kullanabilen bir uygulamadır. Aşağıdaki işlemleri yapmak için çözüm hızlandırıcıları olarak önceden yapılandırılmış çözümleri kullanabilirsiniz:

* Kendi IoT çözümleriniz için bir başlangıç noktası sağlama.
* IoT çözüm tasarımı ve geliştirmesinde ortak yaklaşımlar hakkında bilgi edinme.

Günümüzde üç adet önceden yapılandırılmış çözüm mevcuttur:

* [Uzaktan izleme](iot-suite-remote-monitoring-explore.md)
* [Tahmine dayalı bakım](iot-suite-predictive-overview.md)
* [Bağlı fabrika](iot-suite-connected-factory-overview.md)

Aşağıdaki tabloda, çözümlerin belirli IoT özelliklerini nasıl karşıladığı gösterilmektedir:

| Çözüm | Veri alımı | Cihaz kimliği | Cihaz yönetimi | Uç işleme | Komut ve denetim | Kurallar ve eylemler | Tahmine dayalı analiz |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Uzaktan izleme](iot-suite-remote-monitoring-explore.md)  |Evet |Evet |Evet |-   |Evet |Evet |-   |
| [Tahmine dayalı bakım](iot-suite-predictive-overview.md)   |Evet |Evet |-   |-   |Evet |Evet |Evet |
| [Bağlı fabrika](iot-suite-connected-factory-overview.md) |Evet |Evet |Evet |Evet |Evet |Evet |-   |

* *Veri alımı*: Bulut ölçeğinde veri girişi.
* *Cihaz kimliği*: Benzersiz cihaz kimliklerini yönetin ve çözüme cihaz erişimini denetleyin.
* *Cihaz yönetimi*: Cihaz meta verilerini yönetin ve cihazı yeniden başlatma ile üretici yazılımını yükseltme gibi işlemleri gerçekleştirin.
* *Komuta ve denetim*: Cihazın bir işlem yapmasını sağlamak için buluttan cihaza ileti gönderme.
* *Kurallar ve eylemler*: Çözüm, belirli bir cihazdan buluta verilerde işlem yapmak için arka uç kuralları kullanır.
* *Tahmine dayalı analiz*: Çözüm arka ucu, belirli işlemlerin ne zaman gerçekleştirileceğini tahmin etmek için cihazdan buluta verileri analiz eder. Örneğin, motor bakımının ne zaman olacağını saptamak için uçak motoru telemetrisinin analiz edilmesi.

> [!NOTE]
> Önceden yapılandırılmış bir çözümü dağıtmak ve nasıl özelleştirildiği hakkında daha fazla bilgi edinmek için [Microsoft Azure IoT Paketi](https://www.azureiotsuite.com/) sayfasını ziyaret edin.

## <a name="azure-services"></a>Azure hizmetleri

Önceden yapılandırılmış bir çözümü dağıttığınızda, sağlama işlemi birkaç Azure hizmetini yapılandırır. Aşağıdaki tabloda önceden yapılandırılmış çözümlerde kullanılan hizmetler gösterilmektedir:

|                      | Uzaktan izleme  | Tahmine dayalı bakım | Bağlı fabrika |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub’ı              | Evet                |                        | Evet               |
| Event Hubs           |                    | Evet                    |                   |
| Zaman Serisi Öngörüleri |                    |                        | Evet               |
| Kapsayıcı Hizmetleri   | Evet                |                        | Evet               |
| Akış Analizi     |                    | Evet                    |                   |
| Web Apps             | Evet                | Evet                    | Evet               |
| Cosmos DB            | Evet                | Evet                    | Evet               |
| Azure Tabloları         |                    | Evet                    | Evet               |

* [Azure IoT Hub](../iot-hub/index.md). Bu hizmet cihaz-bulut arası ve bulut-cihaz arası ileti gönderme özellikleri sağlar ve buluta ve diğer temel IoT Paketi hizmetlerine ağ geçidi görevi görür. Hizmet cihazınızdan ölçekte mesajlar almanızı ve cihazlarınıza komutlar göndermenizi sağlar. Hizmet ayrıca [cihazlarınızı yönetmenizi](../iot-hub/iot-hub-device-management-overview.md) sağlar. Örneğin hub'a bağlı bir veya daha fazla cihazı yapılandırabilir, yeniden başlatabilir veya fabrika ayarlarına döndürebilirsiniz.
* [Azure Event Hubs](../event-hubs/index.md). Bu hizmet, bulutta yüksek hacimli olay alımı sağlar. Şu konuyu inceleyin: [Azure IoT Hub ile Azure Event Hubs Karşılaştırması](../iot-hub/iot-hub-compare-event-hubs.md).
* [Azure Time Series Insights](../time-series-insights/index.md). Önceden yapılandırılmış çözümler, cihazlarınızın telemetri verilerini görüntülemek için bu hizmeti kullanır.
* [Azure Container Service](../container-service/index.yml). Bu hizmet, önceden yapılandırılmış çözümlerde mikro hizmetleri barındırır ve yönetir.
* Veri depolama için [Azure Cosmos DB](../cosmos-db/index.yml) ve [Azure Depolama](../storage/index.yml).
* [Azure Stream Analytics](../stream-analytics/index.md). Tahmin dayalı bakım önceden yapılandırılmış çözümü, gelen telemetri işlemek, toplama gerçekleştirmek ve olayları algılamak için bu hizmeti kullanır. Önceden yapılandırılmış bu çözüm, meta veriler ya da cihazlardan alınan komut yanıtları gibi verileri içeren bilgi iletilerini işlemek için de akış analizini kullanır.
* [Azure Web Apps](../app-service/index.yml) önceden yapılandırılmış çözümlerde özel uygulama kodunu barındırır.

Tipik bir IoT çözüm mimarisine genel bakış için [Microsoft Azure ve Nesnelerin İnterneti (IoT)](iot-suite-what-is-azure-iot.md) konusunu inceleyin.

## <a name="whats-new-in-preconfigured-solutions"></a>Önceden yapılandırılmış çözümlerdeki yenilikler nelerdir?

Microsoft önceden yapılandırılmış çözümleri yeni bir mikro hizmet tabanlı mimariye güncelleştirmektedir. Aşağıdaki tabloda önceden yapılandırılmış çözümlerin geçerli durumu gösterilmektedir:

| Önceden yapılandırılmış çözüm | Mimari  | Diller     |
| ---------------------- | ------------- | ------------- |
| Uzaktan izleme      | Mikro hizmetler | Java ve .NET |
| Tahmine dayalı bakım | MVC           | .NET          |
| Bağlı fabrika      | MVC           | .NET          |

Aşağıdaki bölümlerde, mikro hizmet tabanlı önceden yapılandırılmış çözümlerdeki yenilikler açıklanmaktadır:

### <a name="microservices"></a>Mikro hizmetler

Uzaktan izleme önceden yapılandırılmış çözümünün yeni sürümü bir mikro hizmet mimarisi kullanır. Önceden yapılandırılmış bu çözüm, *IoT Hub yöneticisi* ve *Depolama yöneticisi* gibi birden fazla mikro hizmetten oluşur. İlgili geliştirici belgeleriyle birlikte her bir mikro hizmetin Java ve .NET sürümleri indirilebilir. Mikro hizmetler hakkında daha fazla bilgi için [Uzaktan izleme mimarisi](iot-suite-remote-monitoring-sample-walkthrough.md) konusunu inceleyin.

Bu mikro hizmet mimarisi, bulut çözümleri için aşağıdaki özelliklere sahip olduğu kanıtlanmış bir düzendir:

* Ölçeklenebilir.
* Genişletilebilirlik sağlar.
* Anlaşılması kolaydır.
* Alternatifleri için hizmetlerin tek tek takas edilmesini sağlar.

> [!TIP]
> Mikro hizmet mimarisi hakkında daha fazla bilgi için [.NET Uygulama Mimarisi](https://www.microsoft.com/net/learn/architecture) ve [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) konusunu inceleyin.

Uzaktan izlemenin yeni sürümünü dağıttığınızda, aşağıdaki dağıtım seçeneklerinden birini belirlemeniz gerekir:

* **Temel:** Tanıtım için veya bir dağıtımı test etmek için daha düşük maliyetli sürüm. Tüm mikro hizmetler tek bir Azure sanal makinesine dağıtılır.
* **Kurumsal:** Bir üretim dağıtımı geliştirmek için genişletilmiş altyapı dağıtımı. Azure Container Service, mikro hizmetleri birden fazla Azure sanal makinesine dağıtır. Kubernetes mikro hizmetleri tek tek barındıran Docker kapsayıcılarını düzenler.

### <a name="language-choices-java-and-net"></a>Dil seçimleri: Java ve .NET

Mikro hizmetlerin her birinin uygulamaları hem Java hem de .NET ile kullanılabilir. .NET kodu gibi Java kaynak kodu da açık bir kaynaktır ve belirli gereksinimlerinize göre özelleştirmek için kullanılabilir:

* [Uzaktan izleme .NET GitHub deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Uzaktan izleme Java GitHub deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Diğer dil uygulamalarını görmek isterseniz, [Azure IoT user voice](https://feedback.azure.com/forums/321918-azure-iot)’a bir istek ekleyin.

### <a name="react-user-interface-framework"></a>React kullanıcı arabirimi çerçevesi

Kullanıcı arabirimi, [React](https://facebook.github.io/react/) javascript kitaplığı kullanılarak derlenir. Kaynak kodu açık kaynaktır ve indirilip özelleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış IoT Paketi çözümlerine genel bir bakış gördüğünüze göre, önceden yapılandırılmış her bir çözüm için önerilen sonraki adımlara bakabilirsiniz:

* [Azure IoT Paketi uzaktan izleme çözümü Kaynak Yöneticisi dağıtım modelini keşfedin](iot-suite-remote-monitoring-explore.md).
* [Önceden yapılandırılmış tahmine dayalı bakım çözümüne genel bakış](iot-suite-predictive-overview.md).
* [Önceden yapılandırılmış bağlı fabrika çözümlerini kullanmaya başlama](iot-suite-connected-factory-overview.md).

IoT çözüm mimarileri hakkında daha fazla bilgi için [Microsoft Azure IoT hizmetleri: Başvuru Mimarisi](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) konusunu inceleyin.
