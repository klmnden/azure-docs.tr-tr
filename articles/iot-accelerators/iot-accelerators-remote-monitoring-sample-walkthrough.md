---
title: Uzaktan izleme çözüm hızlandırıcısına genel bakış - Azure | Microsoft Docs
description: Uzaktan izleme çözüm Hızlandırıcısını genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: dfe584532efeab1dbc0d2928b7afb0a6695a21ee
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39184954"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Uzaktan izleme çözüm hızlandırıcısına genel bakış

Uzaktan izleme [Çözüm Hızlandırıcısı](../iot-accelerators/about-iot-accelerators.md) birden çok makine için uçtan uca bir izleme çözümü, uzak konumlarda uygular. Bu çözüm, iş senaryosunun genel uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir. Çözümü kendi uygulamanız için başlangıç noktası olarak kullanabilirsiniz ve [özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) kendi özel iş gereksinimlerinizi karşılayacak şekilde.

Bu makalede Uzaktan izleme çözümünün nasıl çalıştığını anlamanız için temel öğelerinden bazıları açıklanmaktadır. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama.
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagramda yayılan Uzaktan izleme çözüm Hızlandırıcısını mantıksal bileşenlerinin özetler [IOT mimarisi](../iot-fundamentals/iot-introduction.md):

![Mantıksal mimari](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Neden mikro hizmetler?

Microsoft ilk Çözüm Hızlandırıcıları yayımlanan bu yana bulut mimarisi gelişmiştir. [Mikro Hizmetler](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) geliştirme hızdan ödün vermeden ölçek ve esneklik elde etmek için kendini kanıtlamış bir yöntem olarak ortaya çıkmıştır. Çeşitli Microsoft hizmetleriyle bu mimari deseni harika güvenilirlik ve ölçeklenebilirlik sonuçları ile dahili olarak kullanır. Bunları ayrıca yararlanabilir, böylece güncelleştirilmiş Çözüm Hızlandırıcıları bu dersleri uygulama içine yerleştirin.

> [!TIP]
> Mikro hizmet mimarisi hakkında daha fazla bilgi için [.NET Uygulama Mimarisi](https://www.microsoft.com/net/learn/architecture) ve [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) konusunu inceleyin.

## <a name="device-connectivity"></a>Cihaz bağlantısı

Çözüm, mantıksal mimarisini cihaz bağlantısı parçası aşağıdaki bileşenleri içerir:

### <a name="simulated-devices"></a>Sanal cihazlar

Çözüm, çözümde uçtan uca akışı test etmek için sanal cihazlarla bir havuz yönetmenize olanak veren bir mikro hizmet içerir. Sanal cihazlar:

* CİHAZDAN buluta telemetri oluşturun.
* Bulut-cihaz yöntem çağrıları için IOT Hub'ından yanıt.

Mikro hizmet, oluşturmak, başlatmak ve benzetimleri durdurmak bir RESTful uç noktası sağlar. Telemetri gönderme ve yöntem çağrıları için yanıt farklı türlerde sanal cihazlar her benzetimi oluşur.

Çözüm portalı Pano sanal cihazlardan sağlayabilirsiniz.

### <a name="physical-devices"></a>Fiziksel cihazlar

Çözüme fiziksel cihazlar bağlanabilirsiniz. Azure IOT cihaz SDK'larını kullanarak sanal cihazlarınızı davranışını uygulayabilirsiniz.

Fiziksel cihazlar çözüm portalında panodan sağlayabilir.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IOT Hub ve IOT Yöneticisi mikro hizmet

[IOT hub'ı](../iot-hub/index.yml) cihazlardan buluta gönderilen verileri alır ve kullanılabilir hale getirir `telemetry-agent` mikro hizmet.

IoT hub çözümde aynı zamanda şunları yapar:

* Kimlikleri ve portala bağlanmasına izin verilen tüm cihazların kimlik doğrulama anahtarlarını depolayan bir kimlik kayıt defteri tutar. Cihazları kimlik kayıt defterinden etkinleştirebilir ve devre dışı bırakabilirsiniz.
* Çözüm portalı adına cihazlarınızda komut çağırır.
* Tüm kayıtlı cihazlar için cihaz ikizlerini tutar. Cihaz ikizi bir cihaz tarafından bildirilen özellik değerlerini depolar. Cihaz ikizi ayrıca cihaz bir kez daha bağlandığında alabilmesi için çözüm portalında ayarlanmış istenen özellikleri depolar.
* Birden fazla cihaza ait özellikleri ayarlamak veya birden fazla cihaz üzerinde yöntem çağırmak için işleri zamanlar.

Çözümde `iot-manager` mikro hizmet, IOT hub'ınıza etkileşim gibi işlemek için:

* Oluşturma ve IOT cihazları yönetme.
* Cihaz ikizlerini yönetme.
* Cihazlarda yöntemleri çağırma.
* IOT kimlik bilgilerini yönetme.

Bu hizmet, kullanıcı tanımlı gruba ait olan cihazları almak için sorguları da IOT hub'ı çalışır.

Mikro hizmet, cihazları ve cihaz ikizlerini yönetme, çağırma yöntemlerinin ve IOT hub'ı sorguları çalıştırmak için bir RESTful uç noktası sağlar.

## <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Çözüm, veri işleme ve analizi mantıksal mimarisini parçası aşağıdaki bileşenleri içerir:

### <a name="device-telemetry"></a>Cihaz telemetrisi

Çözüm, cihaz telemetrisi işlemek için iki mikro hizmetleri içerir.

[Telemetri aracısını](https://github.com/Azure/telemetry-agent-dotnet) mikro hizmet:

* Telemetri, Azure Cosmos DB içinde depolar.
* Cihazlara ait telemetri akışına analiz eder.
* Tanımlanmış kurallara göre uyarılar oluşturur.

Alarmlar, Azure Cosmos DB'de depolanır.

[Telemetri aracısını](https://github.com/Azure/telemetry-agent-dotnet) mikro hizmet cihazlardan gönderilen telemetri okumak çözüm portalı sağlar. Çözüm portalı, bu hizmet için de kullanır:

* Alarmlar tetikleyen eşikleri gibi izleme kuralları tanımlar
* Son uyarılar listesini alın.

Bu mikro hizmet tarafından sağlanan RESTful uç noktası, telemetri, kurallar ve alarmlar yönetmek için kullanın.

### <a name="storage"></a>Depolama

[Depolama bağdaştırıcısı](https://github.com/Azure/pcs-storage-adapter-dotnet) çözüm Hızlandırıcı için kullanılan ana depolama hizmetini önünde bir bağdaştırıcı mikro hizmetidir. Bu basit toplama ve anahtar-değer deposu sağlar.

Çözüm Hızlandırıcısını standart dağıtımı, Azure Cosmos DB, ana Depolama hizmetinden olarak kullanır.

Azure Cosmos DB veritabanı içinde çözüm Hızlandırıcısını verilerini depolar. **Depolama bağdaştırıcısı** mikro hizmet çözümü depolama hizmetlerine erişim diğer mikro hizmetler için bir bağdaştırıcı olarak görev yapar.

## <a name="presentation"></a>Sunum

Çözüm mantıksal mimarisini sunu bölümünde aşağıdaki bileşenleri içerir:

[Web kullanıcı arabirimi, React Javascript bir uygulamadır](https://github.com/Azure/pcs-remote-monitoring-webui). Uygulama:

* Tamamen tarayıcıda çalışan ve React JavaScript'ı yalnızca kullanır.
* CSS stil uygulanmış.
* AJAX çağrıları aracılığıyla genel kullanıma yönelik mikro hizmetler ile etkileşim kurar.

Kullanıcı arabirimi, tüm çözüm Hızlandırıcı işlevselliği sunar ve gibi diğer hizmetlerle etkileşime geçer:

* [Kimlik doğrulaması](https://github.com/Azure/pcs-auth-dotnet) mikro hizmet, kullanıcı verilerini korumak için.
* [İothub-manager](https://github.com/Azure/iothub-manager-dotnet) listelemek ve IOT cihazları yönetmek için mikro hizmet.

[UI-config](https://github.com/Azure/pcs-config-dotnet) mikro hizmet depolamak ve yapılandırma ayarlarını almak kullanıcı arabirimi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak kodu ve geliştirici belgeleri araştırmak istiyorsanız, bir iki ana GitHub depoları başlatın:

* [Azure IOT (.NET) ile Uzaktan izleme çözüm Hızlandırıcısını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [(Java) Azure IOT ile Uzaktan izleme çözüm Hızlandırıcısını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Mimari diyagramları ayrıntılı çözümü:
* [Uzaktan izleme mimarisi için çözüm hızlandırıcı](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [çözüm Hızlandırıcısını özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
