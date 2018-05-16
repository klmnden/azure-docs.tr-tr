---
title: Azure IoT çözüm hızlandırıcılarına genel bakış | Microsoft Docs
description: Azure IoT çözüm hızlandırıcılarının ve ek kaynaklara bağlantısı olan mimarisinin açıklaması.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 59009f37-9ba0-4e17-a189-7ea354a858a2
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 17/01/2018
ms.author: dobett
ms.openlocfilehash: 2dc286dd228b1990e0307476d3623ffc91489f98
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="what-are-the-iot-solution-accelerators"></a>IoT çözüm hızlandırıcıları nedir?

Azure IoT _çözüm hızlandırıcıları_, aşağıdakileri gerçekleştiren bir çözüm kümesidir:

* Dakikalar içinde dağıtım
* Hızlıca kullanmaya başlama
* Gereksinimlerinizi karşılayacak şekilde özelleştirebilirsiniz

Çözüm hızlandırıcılarının tümü aynı ilke ve hedeflere göre tasarlanmıştır.

Aşağıdaki videoda, uzaktan izleme çözümüne genel bakışa yer verilmiştir:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Meet-the-new-Remote-Monitoring-accelerator-for-Azure-IoT/Player]

## <a name="solution-accelerators-overview"></a>Çözüm hızlandırıcılarına genel bakış

Çözüm hızlandırıcısı, aboneliğinizi kullanarak Azure’a dağıtabildiğiniz yaygın IoT çözüm düzenlerinin açık kaynak uygulamasıdır. Her çözüm hızlandırıcısı, belirli bir IoT senaryosu veya senaryolarını uygulamak için özel kod ile Azure hizmetlerini birleştirir. Senaryoların herhangi birini gereksinimlerinizi karşılayacak şekilde özelleştirebilirsiniz. Bu senaryolar şunlardır:

* Ayrıntılı öngörüler ve çözüm durumu için zengin bir pano üzerinde verileri görselleştirme.
* Dinamik IoT cihaz telemetrisi üzerinde kurallar ve alarmlar yapılandırma.
* Yazılım güncelleştirmeleri ve yapılandırma gibi cihaz yönetimi işlerini zamanlama.
* Kendi özel fiziksel veya sanal cihazlarınızı sağlama.
* IoT cihaz gruplarınızın içindeki sorunları giderme ve düzeltme.

Her çözüm hızlandırıcısı, telemetri oluşturmak için tam, uçtan uca sanal veya fiziksel cihazları kullanabilen bir uygulamadır. Aşağıdaki işlemleri yapmak için çözüm hızlandırıcıları olarak çözüm hızlandırıcılarını kullanabilirsiniz:

* Kendi IoT çözümleriniz için bir başlangıç noktası sağlama.
* IoT çözüm tasarımı ve geliştirmesinde ortak yaklaşımlar hakkında bilgi edinme.

Şu anda üç çözüm hızlandırıcısı mevcuttur:

* [Uzaktan İzleme](iot-suite-remote-monitoring-explore.md)
* [Tahmine Dayalı Bakım](iot-suite-predictive-overview.md)
* [Bağlı Fabrika](iot-suite-connected-factory-overview.md)

Aşağıdaki tabloda, çözümlerin belirli IoT özelliklerini nasıl karşıladığı gösterilmektedir:

| Çözüm | Veri alımı | Cihaz kimliği | Cihaz yönetimi | Uç işleme | Komut ve denetim | Kurallar ve eylemler | Tahmine dayalı analiz |
| ------------------------------------------------------------ | -- | -- | -- | -- | -- | -- | -- |
| [Uzaktan İzleme](iot-suite-remote-monitoring-explore.md)  |Yes |Yes |Yes |-   |Yes |Yes |-   |
| [Tahmine Dayalı Bakım](iot-suite-predictive-overview.md)   |Yes |Yes |-   |-   |Yes |Yes |Yes |
| [Bağlı Fabrika](iot-suite-connected-factory-overview.md) |Yes |- |- |Yes |Yes |Yes |-   |

* *Veri alımı*: Bulut ölçeğinde veri girişi.
* *Cihaz kimliği*: Benzersiz cihaz kimliklerini yönetin ve çözüme cihaz erişimini denetleyin.
* *Cihaz yönetimi*: Cihaz meta verilerini yönetin ve cihazı yeniden başlatma ile üretici yazılımını yükseltme gibi işlemleri gerçekleştirin.
* *Komuta ve denetim*: Cihazın bir işlem yapmasını sağlamak için buluttan cihaza ileti gönderme.
* *Kurallar ve eylemler*: Çözüm, belirli bir cihazdan buluta verilerde işlem yapmak için arka uç kuralları kullanır.
* *Tahmine dayalı analiz*: Çözüm arka ucu, belirli işlemlerin ne zaman gerçekleştirileceğini tahmin etmek için cihazdan buluta verileri analiz eder. Örneğin, motor bakımının ne zaman olacağını saptamak için uçak motoru telemetrisinin analiz edilmesi.

> [!NOTE]
> Bir çözüm hızlandırıcısını dağıtmak ve nasıl özelleştirildiği hakkında daha fazla bilgi edinmek için [Microsoft Azure IoT çözüm hızlandırıcıları](https://www.azureiotsuite.com/) sayfasını ziyaret edin.

## <a name="azure-services"></a>Azure hizmetleri

Bir çözüm hızlandırıcısını dağıttığınızda, sağlama işlemi birkaç Azure hizmetini yapılandırır. Aşağıdaki tabloda, çözüm hızlandırıcılarında kullanılan hizmetler gösterilmektedir:

|                      | Uzaktan İzleme  | Tahmine Dayalı Bakım | Bağlı Fabrika |
| -------------------- | ------------------ | ---------------------- | ----------------- |
| IoT Hub              | Yes                | Yes                    | Yes               |
| Event Hubs           |                    | Yes                    |                   |
| Time Series Insights |                    |                        | Yes               |
| Kapsayıcı Hizmetleri   | Yes                |                        |                   |
| Stream Analytics     |                    | Yes                    |                   |
| Web Apps             | Yes                | Yes                    | Yes               |
| Cosmos DB            | Yes                | Yes                    |                    |
| Azure Storage         |                    | Yes                    | Yes               |

> [!NOTE]
> Uzaktan izleme çözüm hızlandırıcısında dağıtılan kaynaklar hakkında daha fazla bilgi için GitHub’daki bu [makaleye](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/blob/master/README.md#basic-vs-standard-deployments) bakın.

* [Azure IoT Hub](../iot-hub/index.yml). Bu hizmet, cihaz-bulut arası ve bulut-cihaz arası ileti gönderme özellikleri sağlar ve buluta ve diğer temel çözüm hızlandırıcısı hizmetlerine ağ geçidi görevi görür. Hizmet cihazınızdan ölçekte mesajlar almanızı ve cihazlarınıza komutlar göndermenizi sağlar. Hizmet ayrıca [cihazlarınızı yönetmenizi](../iot-hub/iot-hub-device-management-overview.md) sağlar. Örneğin hub'a bağlı bir veya daha fazla cihazı yapılandırabilir, yeniden başlatabilir veya fabrika ayarlarına döndürebilirsiniz.
* [Azure Event Hubs](../event-hubs/index.md). Bu hizmet, bulutta yüksek hacimli olay alımı sağlar. Şu konuyu inceleyin: [Azure IoT Hub ile Azure Event Hubs Karşılaştırması](../iot-hub/iot-hub-compare-event-hubs.md).
* [Azure Time Series Insights](../time-series-insights/index.yml). Çözüm hızlandırıcıları, cihazlarınızın telemetri verilerini analiz etmek ve görüntülemek için bu hizmeti kullanır.
* [Azure Container Service](../container-service/index.yml). Bu hizmet, çözüm hızlandırıcılarında mikro hizmetleri barındırır ve yönetir.
* Veri depolama için [Azure Cosmos DB](../cosmos-db/index.yml) ve [Azure Depolama](../storage/index.yml).
* [Azure Stream Analytics](../stream-analytics/index.yml). Tahmin dayalı bakım önceden yapılandırılmış çözümü, gelen telemetri işlemek, toplama gerçekleştirmek ve olayları algılamak için bu hizmeti kullanır. Önceden yapılandırılmış bu çözüm, meta veriler ya da cihazlardan alınan komut yanıtları gibi verileri içeren bilgi iletilerini işlemek için de akış analizini kullanır.
* [Azure Web Apps](../app-service/index.yml) önceden yapılandırılmış çözümlerde özel uygulama kodunu barındırır.

Tipik bir IoT çözüm mimarisine genel bakış için [Microsoft Azure ve Nesnelerin İnterneti (IoT)](iot-suite-what-is-azure-iot.md) konusunu inceleyin.

## <a name="whats-new-in-solution-accelerators"></a>Çözüm hızlandırıcılarındaki yenilikler nedir?

Microsoft, çözüm hızlandırıcıları yeni bir mikro hizmet tabanlı mimariye güncelleştirmektedir. Aşağıdaki tabloda, çözüm hızlandırıcılarının geçerli durumu gösterilmektedir:

| Çözüm hızlandırıcısı | Mimari  | Diller     |
| ---------------------- | ------------- | ------------- |
| Uzaktan İzleme      | Mikro hizmetler | Java ve .NET |
| Tahmine Dayalı Bakım | MVC           | .NET          |
| Bağlı Fabrika      | MVC           | .NET          |

Aşağıdaki bölümlerde, mikro hizmet tabanlı çözüm hızlandırıcılarındaki yenilikler açıklanmaktadır:

### <a name="microservices"></a>Mikro hizmetler

Uzaktan izleme çözüm hızlandırıcısının yeni sürümü bir mikro hizmet mimarisi kullanır. Bu çözüm hızlandırıcısı, *IoT Hub yöneticisi* ve *Depolama yöneticisi* gibi birden fazla mikro hizmetten oluşur. İlgili geliştirici belgeleriyle birlikte her bir mikro hizmetin Java ve .NET sürümleri indirilebilir. Mikro hizmetler hakkında daha fazla bilgi için [Uzaktan İzleme mimarisi](iot-suite-remote-monitoring-sample-walkthrough.md) bölümüne bakın.

Bu mikro hizmet mimarisi, bulut çözümleri için aşağıdaki özelliklere sahip olduğu kanıtlanmış bir düzendir:

* Ölçeklenebilir.
* Genişletilebilirlik sağlar.
* Anlaşılması kolaydır.
* Alternatifleri için hizmetlerin tek tek takas edilmesini sağlar.

> [!TIP]
> Mikro hizmet mimarisi hakkında daha fazla bilgi için [.NET Uygulama Mimarisi](https://www.microsoft.com/net/learn/architecture) ve [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) konusunu inceleyin.

Uzaktan izlemenin yeni sürümünü dağıttığınızda, aşağıdaki dağıtım seçeneklerinden birini belirlemeniz gerekir:

* **Temel:** Tanıtım için veya bir dağıtımı test etmek için daha düşük maliyetli sürüm. Tüm mikro hizmetler tek bir Azure sanal makinesine dağıtılır.
* **Standart:** Bir üretim dağıtımı geliştirmek için genişletilmiş altyapı dağıtımı. Azure Container Service, mikro hizmetleri birden fazla Azure sanal makinesine dağıtır. Kubernetes mikro hizmetleri tek tek barındıran Docker kapsayıcılarını düzenler.

### <a name="language-choices-java-and-net"></a>Dil seçimleri: Java ve .NET

Mikro hizmetlerin her birinin uygulamaları hem Java hem de .NET ile kullanılabilir. .NET kodu gibi Java kaynak kodu da açık bir kaynaktır ve belirli gereksinimlerinize göre özelleştirmek için kullanılabilir:

* [Uzaktan İzleme .NET GitHub deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Uzaktan İzleme Java GitHub deposu](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

Diğer dil uygulamalarını görmek isterseniz, [Azure IoT user voice](https://feedback.azure.com/forums/321918-azure-iot)’a bir istek ekleyin.

### <a name="react-user-interface-framework"></a>React kullanıcı arabirimi çerçevesi

Kullanıcı arabirimi, [React](https://facebook.github.io/react/) javascript kitaplığı kullanılarak derlenir. Kaynak kodu açık kaynaktır ve indirilip özelleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının genel bakışını gördüğünüze göre, her bir çözüm hızlandırıcısı için önerilen sonraki adımlara bakabilirsiniz:

* [Uzaktan İzleme çözümünü keşfetme](iot-suite-remote-monitoring-explore.md).
* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](iot-suite-predictive-overview.md).
* [Bağlı Fabrika çözüm hızlandırıcısını kullanmaya başlama](iot-suite-connected-factory-overview.md).

IoT çözüm mimarileri hakkında daha fazla bilgi için [Microsoft Azure IoT hizmetleri: Başvuru Mimarisi](http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf) konusunu inceleyin.
