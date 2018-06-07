---
title: Uzaktan izleme çözümü mimari seçenekleri - Azure | Microsoft Docs
description: Bu makalede Uzaktan izleme mimari ve teknik seçimlerinizi
author: timlaverty
manager: camerons
ms.author: timlav
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 04/30/2018
ms.topic: conceptual
ms.openlocfilehash: 6c4bf0e4bf0a6c1a791cf762ec9bb44ed5c0b1bd
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34627697"
---
# <a name="remote-monitoring-architectural-choices"></a>Uzaktan izleme mimari seçenekleri

Azure IOT Uzaktan izleme Çözüm Hızlandırıcısı açık kaynaklı, MIT lisansı, müşterilerin kendi geliştirme hızlandırabilir böylece cihaz bağlantısı, cihaz yönetim ve akış işleme, gibi yaygın IOT senaryolarını tanıtır Çözüm Hızlandırıcısı işlem.  Uzaktan izleme çözümü yayımlanan önerilen Azure IOT başvuru mimarisi izleyen [burada](https://aka.ms/iotrefarchitecture).  

Bu makalede, her alt sistemlerini uzaktan izleme çözümü için Mimari ve teknik seçimlerinizi açıklar ve kabul alternatifleri anlatılmaktadır.  Uzaktan izleme çözümünde teknik seçimlerinizi Uzaktan izleme IOT çözümü uygulamak için tek yolu olmayan dikkate almak önemlidir.  Teknik uygulamadan başarılı bir uygulama oluşturmak için bir taban çizgisi ve yetenekleri, deneyimi ve müşteri çözüm uygulaması için dikey uygulamanızın gereksinimlerine uyacak şekilde değiştirilmesi gerekir.

## <a name="architectural-choices"></a>Mimari seçenekleri

### <a name="microservices-serverless-and-cloud-native"></a>Mikro sunucusuz ve yerel bulut

Mimari bulut yerel, mikro hizmet, IOT uygulamalardır ve sunucusuz dayalı öneririz.  Bir IOT uygulamanın farklı alt sistemleri bağımsız olarak dağıtılabilir ve bağımsız olarak ölçeklendirmek için ayrık Hizmetleri olarak oluşturulmalıdır.  Bu öznitelikler büyük ölçekli, tek tek alt sistemleri, güncelleştirme daha fazla esneklik etkinleştirin ve her alt sistemi temelinde uygun teknoloji seçim yapma esnekliği sağlar.  Mikro birden çok teknolojileriyle uygulanabilir. Örneğin, Azure işlevleri gibi sunucusuz teknolojisi gibi Docker kapsayıcısı teknolojisi kullanarak ya da Azure App Services gibi PaaS Hizmetleri'ndeki mikro barındırma.

## <a name="core-subsystem-technology-choices"></a>Çekirdek alt sistemi teknoloji seçimleri

Bu bölümde Uzaktan izleme çözümünde her çekirdek alt sistemleri için teknoloji seçimlerinizi ayrıntılarını verir.

![Çekirdek diyagramı](./media/iot-accelerators-remote-monitoring-architectural-choices/subsystem.png) 

### <a name="cloud-gateway"></a>Bulut ağ geçidi
Azure IOT hub'ı Uzaktan izleme çözümü bulut ağ geçidi olarak kullanılır.  IOT hub'ı cihazlarla güvenli, çift yönlü iletişimi sağlar. IOT Hub hakkında daha fazla bilgiyi [burada](https://azure.microsoft.com/services/iot-hub/). IOT cihaz bağlantısı için .NET Core ve Java IOT Hub SDK'ları kullanılır.  SDK'ları sarmalayıcıları IOT hub'ı REST API geçici sunar ve yeniden deneme gibi senaryoları tanıtıcı.

### <a name="stream-processing"></a>Akış işleme
Akış için Uzaktan izleme çözümü işleme Azure akış analizi karmaşık kural işlenmek üzere kullanır.  Daha basit kuralları isteyen müşteriler için de basit kurallarının işlenmesini desteği ile özel bir mikro hizmet bu ayarı rağmen kutusunu dağıtım dışı parçası olmayan sahibiz. Başvuru mimarisine Azure işlevleri için kullanımını basit kural işleme ve Azure akış analizi (ASA) karmaşık kural işleme önerir.  

### <a name="storage"></a>Depolama
Depolama için Azure Cosmos DB tüm depolama gereksinimleriniz için kullanılır: soğuk depolama, yarı depolama, kuralları depolama ve uyarılar. Şu anda Azure blob depolama birimine taşıma işleminde referans mimarisi tarafından önerildiği şekilde duyuyoruz.  Çözümleri Azure zaman serisi Öngörüler ve Azure Data Lake gibi birçok kullanım durumları için uygun olmakla birlikte azure Cosmos DB IOT uygulamaları için önerilen genel amaçlı sıcak depolama çözümüdür.

### <a name="business-integration"></a>İş tümleştirmesi
İş tümleştirmesi Uzaktan izleme çözümünde sıcak depolamaya yerleştirilen alarmlar nesil sınırlıdır. Daha fazla iş tümleştirmeler çözümünü Azure Logic Apps ile tümleştirerek gerçekleştirilebilir.

### <a name="user-interface"></a>Kullanıcı arabirimi
Web kullanıcı Arabirimi JavaScript tepki ile yerleşik olarak bulunur.  Tepki yaygın olarak kullanılan endüstri web kullanıcı Arabirimi framework sunar ve diğer popüler uygulamayı Angular gibi benzer.  

### <a name="runtime-and-orchestration"></a>Çalışma zamanı ve düzenleme
Uzaktan izleme çözümünde alt sistemi uygulaması için seçilen uygulama çalışma zamanı için yatay ölçek orchestrator Kubernetes ile Docker kapsayıcıları gibidir.  Alt sistemi başına tek tek ölçek tanımı ancak VM'ler ve kapsayıcıları güvenlik açısından güncel tutma içinde DevOps maliyetler doğurur için bu mimarisi sağlar.  Docker ve Kubernetes alternatifleri (örneğin, Azure App Service) PaaS Hizmetleri'ndeki mikro barındırma ya da bir orchestrator Service Fabric, DCOS, Swarm, vb. kullanarak içerir.

## <a name="next-steps"></a>Sonraki adımlar
* Uzaktan izleme çözümü dağıtmak [burada](https://www.azureiotsolutions.com/).
* GitHub kodda keşfedin [C#](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) ve [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java/).  
* IOT başvuru mimarisi hakkında daha fazla bilgi [burada](https://aka.ms/iotrefarchitecture).
