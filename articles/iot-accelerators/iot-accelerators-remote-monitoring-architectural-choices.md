---
title: Uzaktan izleme çözümü mimari seçenekleri - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme mimari ve teknik seçimlerinizi
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 09/12/2018
ms.topic: conceptual
ms.openlocfilehash: 09c5981701ffdee5f2e5dba47cc98c91d5df7526
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603915"
---
# <a name="remote-monitoring-architectural-choices"></a>Uzaktan izleme mimari seçenekleri

Azure IOT Uzaktan izleme çözüm Hızlandırıcısını açık kaynaklı, MIT lisansı, müşterilerin kendi geliştirme sürecini hızlandırabilir, cihaz bağlantısı, cihaz yönetimi ve akış işlemede, gibi yaygın IOT senaryolarını tanıtır Çözüm Hızlandırıcısı işlem.  Uzaktan izleme çözümü yayımlanan önerilen Azure IOT başvuru mimarisi aşağıdaki [burada](https://aka.ms/iotrefarchitecture).  

Bu makale, her alt sistemlerin Uzaktan izleme çözümü mimari ve teknik seçimlerinizi açıklar ve dikkate alınan alternatifler açıklanır.  Uzaktan izleme çözümünde teknik seçimlerinizi tek yolu bir uzaktan izleme IOT çözümü uygulama olmadığına dikkat edin önemlidir.  Teknik uygulama, başarılı bir uygulama oluşturmak için bir taban çizgisi ve becerileri, deneyimi ve müşteri çözüm uygulaması için dikey uygulama gereksinimlerinize uyacak şekilde değiştirilmelidir.

## <a name="architectural-choices"></a>Mimari seçenekleri

### <a name="microservices-serverless-and-cloud-native"></a>Mikro hizmetler, sunucusuz, yerel bulut

Mimari bulut yerel, mikro hizmet, IOT uygulamaları olan ve sunucusuz tabanlı öneririz.  Bir IOT uygulamanın farklı alt sistemleri bağımsız bir şekilde dağıtılabilen ve bağımsız olarak ölçeklendirme yapabilir, ayrı hizmetler derlenmelidir.  Bu öznitelikler, daha yüksek ölçek, ayrı ayrı alt sistemler, güncelleştirme daha fazla esneklik etkinleştirin ve her alt sistemi temelinde uygun teknolojiyi seçmenize izin verin.  Mikro hizmetler ile birden çok teknoloji uygulanabilir. Örneğin, Azure işlevleri gibi sunucusuz teknolojilerini gibi Docker kapsayıcı teknolojisini kullanarak veya Azure uygulama hizmetleri gibi PaaS hizmetlerine mikro hizmet barındırma.

## <a name="core-subsystem-technology-choices"></a>Çekirdek alt teknoloji seçimleri

Bu bölümde, Uzaktan izleme çözümünde her çekirdek alt sistemler için yapılan teknoloji seçimleri açıklanmaktadır.

![Çekirdek diyagramı](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Bulut ağ geçidi
Azure IOT hub'ı Uzaktan izleme çözümü bulut ağ geçidi olarak kullanılır.  IOT hub'ı cihazlarla güvenli, çift yönlü iletişimi sağlar. IOT Hub hakkında daha fazla bilgi [burada](https://azure.microsoft.com/services/iot-hub/). .NET Core ve Java IOT Hub SDK'ları, IOT cihaz bağlantısı için kullanılır.  SDK'ları, IOT Hub REST API çevresinde sarmalayıcılar teklif ve deneme gibi senaryoları ele.

### <a name="stream-processing"></a>Akış işleme
Akış için Uzaktan izleme çözümü işleme Azure Stream Analytics karmaşık kural işleme için kullanır.  Daha basit kural isteyen müşteriler için de basit kural işleme desteği ile özel bir mikro hizmet bu ayarı rağmen kutusu dağıtım dışı parçası olmayan sahibiz. Başvuru mimarisi, Azure işlevleri karmaşık kural işleme için kullanımı basit kural işleme ve Azure Stream Analytics (ASA) önerir.  

### <a name="storage"></a>Depolama
Azure Time Series Insights hem Azure Cosmos DB, depolama için Uzaktan izleme çözüm Hızlandırıcısını kullanır. Azure zaman serisi görüşleri, IOT hub'ı aracılığıyla bağlı cihazlarınızdan gelen iletileri depolar. Çözüm Hızlandırıcısını soğuk depolama, kuralları tanımlar, uyarılar ve yapılandırma ayarları gibi diğer tüm depolama için Azure Cosmos DB kullanır. Azure Time Series Insights ve Azure Data Lake gibi çözümler birçok kullanım durumları için uygun olsa azure Cosmos DB IOT uygulamaları için önerilen genel amaçlı sıcak depolama çözümüdür. Azure Time Series Insights ile eğilimleri ve anormallikleri, kök neden analizleri gerçekleştirebilir ve masraflı sistem kapatma sürelerini önlemenize olanak tanıyan kapsamlı olarak zaman serisi sensör verileriniz daha ayrıntılı Öngörüler elde edebilirsiniz. 

> [!NOTE]
> Zaman serisi görüşleri Azure Çin Bulutu şu anda kullanılabilir değil. Yeni Uzaktan izleme çözüm Hızlandırıcı dağıtımlarda Azure Çin Bulutu, Cosmos DB için tüm depolama kullanın.

### <a name="business-integration"></a>İş tümleştirmesi
Uzaktan izleme çözümünde iş tümleştirmesi sıcak depolamaya yerleştirilen alarmlar, nesil sınırlıdır. Daha fazla çözüm Azure Logic Apps ile tümleştirerek iş tümleştirmeler gerçekleştirilebilir.

### <a name="user-interface"></a>Kullanıcı arabirimi
Web kullanıcı Arabirimi, React JavaScript ile oluşturulmuştur.  React, yaygın olarak kullanılan endüstri web kullanıcı Arabirimi çerçevesi sunar ve Angular gibi diğer popüler çerçeveleri benzer.  

### <a name="runtime-and-orchestration"></a>Çalışma zamanı ve düzenleme
Uzaktan izleme çözümünde alt sistemi uygulama için seçilen uygulama çalışma zamanı için yatay ölçek orchestrator Kubernetes ile Docker kapsayıcıları gibidir.  Bu mimari alt sistem başına bağımsız ölçek tanımı ancak sanal makine ve kapsayıcı güvenlik açısından güncel tutarak, DevOps maliyetler doğurur izin verir.  Docker ve Kubernetes alternatifleri mikro hizmet PaaS hizmetlerine (örneğin, Azure App Service) barındırma ya da bir orchestrator kullanarak, Service Fabric, DCOS, Swarm vb. içerir.

## <a name="next-steps"></a>Sonraki adımlar
* Uzaktan izleme çözümünüzü dağıtmak [burada](https://www.azureiotsolutions.com/).
* GitHub kod keşfedin [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) ve [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* IOT başvuru mimarisi hakkında daha fazla bilgi [burada](https://aka.ms/iotrefarchitecture).
