---
title: Uzaktan izleme çözümü mimari seçenekleri - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme mimari ve teknik seçimlerinizi
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/20/2018
ms.topic: conceptual
ms.openlocfilehash: 1bd08596a30db7322a72b4269fddfe0b9df19119
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61447185"
---
# <a name="remote-monitoring-architectural-choices"></a>Uzaktan izleme mimari seçenekleri

Azure IOT Uzaktan izleme çözüm Hızlandırıcısını açık kaynak, MIT lisanslı Çözüm Hızlandırıcısı ' dir. Yardımcı olmak için IOT geliştirme sürecinizi hızlandırmak için aşağıdakiler gibi yaygın IOT senaryolarını gösterir:

- Cihaz bağlantısı
- Cihaz yönetimi
- Akış işleme

Uzaktan izleme çözümü önerilen izleyen [Azure IOT başvuru mimarisi](https://aka.ms/iotrefarchitecture).

Bu makalede, her uzaktan izleme alt anahtar mimari ve teknik seçimlerinizi açıklanır. Ancak, Uzaktan izleme çözümünde yapılan Microsoft Teknik seçimleri Uzaktan izleme bir IOT çözümü uygulamak için tek yolu değil. Başarılı bir uygulama oluşturmaya yönelik bir temel olarak teknik uygulama Algıla ve bunu değiştirmeniz gerekir:

- Kullanılabilir yetenekler uygun ve kuruluşunuzda deneyimi.
- Dikey uygulama gereksinimlerinizi karşılayın.

## <a name="architectural-choices"></a>Mimari seçenekleri

Microsoft tarafından bir IOT uygulaması için önerilen mimari bulut yerel, mikro hizmet, sunucusuz tabanlı ve. Farklı alt sistemleri bir IOT uygulaması olarak dağıtabileceğiniz ayrı hizmetler ve ölçek bağımsız olarak oluşturması gerekir. Bu öznitelikler, daha yüksek ölçek, ayrı ayrı alt sistemler, güncelleştirme daha fazla esneklik etkinleştirin ve her alt sistemi için uygun bir teknoloji seçme esnekliği sağlar.

Birden fazla teknolojisini kullanarak mikro hizmetler uygulayabilirsiniz. Örneğin, bir mikro hizmet uygulamak için aşağıdaki seçeneklerden birini seçebilir:

- Docker gibi bir kapsayıcı teknolojisi ile Azure işlevleri gibi sunucusuz teknolojilerini kullanın.
- Azure uygulama hizmetleri gibi PaaS hizmetlerine, mikro hizmetleri barındırır.

## <a name="technology-choices"></a>Teknoloji seçimleri

Bu bölümde, Uzaktan izleme çözümünde her çekirdek alt sistemler için yapılan teknoloji seçimleri açıklanmaktadır.

![Çekirdek diyagramı](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png)

### <a name="cloud-gateway"></a>Bulut ağ geçidi

Azure IOT hub'ı Uzaktan izleme çözümü bulut ağ geçidi olarak kullanılır. [IOT hub'ı](https://azure.microsoft.com/services/iot-hub/) cihazlarla güvenli, çift yönlü iletişimi sağlar.

IOT cihaz bağlantısı için kullanabilirsiniz:

- [IOT Hub cihazı SDK'ları](../iot-hub/iot-hub-devguide-sdks.md#azure-iot-hub-device-sdks) cihazınız için bir yerel istemci uygulamasını gerçekleştirme. SDK'ları, IOT Hub REST API çevresinde sarmalayıcılar teklif ve deneme gibi senaryoları ele.
- Dağıtıp cihazlarınızda kapsayıcılarda çalıştırılan özel modüller yönetmek için Azure IOT Edge ile tümleştirme.
- IOT hub'ı toplu bağlı cihazları yönetmek için otomatik cihaz yönetimi ile tümleştirme.

### <a name="stream-processing"></a>Akış işleme

Akış işleme için Uzaktan izleme çözümü, karmaşık kural işleme için Azure Stream Analytics kullanır. Daha basit kuralları kullanmak istiyorsanız, yoktur basit kural işleme desteği ile özel bir mikro hizmet bu ayarı rağmen kullanıma hazır dağıtımının bir parçası değildir. Başvuru mimarisi, karmaşık kural işleme için Azure işlevleri basit kural işleme ve Azure Stream Analytics için önerir.

### <a name="storage"></a>Depolama

Azure Time Series Insights hem Azure Cosmos DB, depolama için Uzaktan izleme çözüm Hızlandırıcısını kullanır. Azure zaman serisi görüşleri, IOT hub'ı aracılığıyla bağlı cihazlarınızdan gelen iletileri depolar. Çözüm Hızlandırıcısını soğuk depolama, kuralları tanımlar, uyarılar ve yapılandırma ayarları gibi diğer tüm depolama için Azure Cosmos DB kullanır.

Azure Cosmos DB, IOT uygulamaları için önerilen genel amaçlı sıcak depolama çözümüdür. Ancak, çözümleri Azure Time Series Insights ve Azure Data Lake gibi birçok kullanım için uygundur. Azure Time Series Insights ile sayede eğilimleri ve anormallikleri tarafından zaman serisi sensör verileriniz daha ayrıntılı Öngörüler elde edebilirsiniz. Bu özellik, kök neden analizleri gerçekleştirebilir ve masraflı sistem kapatma sürelerini önlemenize olanak sağlar.

> [!NOTE]
> Zaman serisi görüşleri Azure Çin Bulutu şu anda kullanılabilir değil. Yeni Uzaktan izleme çözüm Hızlandırıcı dağıtımlarda Azure Çin Bulutu, Cosmos DB için tüm depolama kullanın.

### <a name="business-integration"></a>İş tümleştirmesi

Uzaktan izleme çözümünde iş tümleştirmesi sıcak depolamaya yerleştirilen uyarıların oluşturulmasını sınırlıdır. Çözüm, daha kapsamlı iş tümleştirme senaryolarını uygulamak için Azure Logic Apps ile bağlanın.

### <a name="user-interface"></a>Kullanıcı arabirimi

Web kullanıcı Arabirimi, React JavaScript ile oluşturulmuştur. React, yaygın olarak kullanılan endüstri web kullanıcı Arabirimi çerçevesi sunar ve Angular gibi diğer popüler çerçeveleri benzer.

### <a name="runtime-and-orchestration"></a>Çalışma zamanı ve düzenleme

Uzaktan izleme çözümü, alt sistemler ile Kubernetes orchestrator yatay ölçek çalıştırmak için Docker kapsayıcıları kullanır. Bu mimari, tek tek ölçek tanımları için her alt sistem sağlar. Ancak, bu mimari, sanal makineler ve kapsayıcılar, güncel ve güvenli tutmak için DevOps ücret yansıtılmaz.

Azure App Service gibi PaaS hizmetlerine mikro hizmetleri barındıran Docker alternatifleri içerir. Alternatifleri Kubernetes, DC/OS, Service Fabric gibi düzenleyicilerle ekleyebilir ya da Swarm.

## <a name="next-steps"></a>Sonraki adımlar

* Uzaktan izleme çözümünüzü dağıtmak [burada](https://www.azureiotsolutions.com/).
* GitHub kod keşfedin [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) ve [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* IOT başvuru mimarisi hakkında daha fazla bilgi [burada](https://aka.ms/iotrefarchitecture).
