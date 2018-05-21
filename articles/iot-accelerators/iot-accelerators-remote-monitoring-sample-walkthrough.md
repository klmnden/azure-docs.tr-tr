---
title: Uzaktan izleme çözümü - Azure Mimarisi | Microsoft Docs
description: Uzaktan izleme Çözüm Hızlandırıcısı mimarisini bir kılavuz.
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 3effde81dfa48e9544d89153d40c160ff972d047
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="remote-monitoring-solution-accelerator-architecture"></a>Uzaktan izleme Çözüm Hızlandırıcısı mimarisi

Uzaktan izleme [Çözüm Hızlandırıcısı](../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md) uzak konumlarda birden fazla makine için uçtan uca bir izleme çözümü uygular. Bu çözüm, iş senaryosunun genel uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir. Çözümü kendi uygulamanız için bir başlangıç noktası olarak kullanabilirsiniz ve [özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) kendi belirli iş gereksinimlerinizi karşılamak üzere onu.

Bu makalede uzaktan izleme çözümünün nasıl çalıştığını anlamanız için çözümün temel öğelerinden bazıları açıklanmaktadır. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama.
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagram yayılan Uzaktan izleme Çözüm Hızlandırıcısı mantıksal bileşenlerinin ana hatların vermektedir [IOT mimarisi](../iot-accelerators/iot-accelerators-what-is-azure-iot.md):

![Mantıksal mimari](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Neden mikro?

Microsoft ilk Çözüm Hızlandırıcıları yayımlanan bu yana bulut mimarisi gelişmiştir. [Mikro](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) ölçek ve esneklik geliştirme hızını ödün vermeden elde etmek için kanıtlanmış yöntem olarak ortaya çıkmıştır. Birkaç Microsoft Hizmetleri bu tasarım örüntüsü mükemmel güvenilirlik ve ölçeklendirilebilirlik sonuçları ile dahili olarak kullanın. Ayrıca bunları yararlanabilir şekilde güncelleştirilmiş Çözüm Hızlandırıcıları bu learnings yöntem içine yerleştirin.

> [!TIP]
> Mikro hizmet mimarisi hakkında daha fazla bilgi için [.NET Uygulama Mimarisi](https://www.microsoft.com/net/learn/architecture) ve [Mikro hizmetler: Bulut tarafından desteklenen bir uygulama devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) konusunu inceleyin.

## <a name="device-connectivity"></a>Cihaz bağlantısı

Çözüm mantıksal mimarisi cihaz bağlantısı parçası aşağıdaki bileşenleri içerir:

### <a name="simulated-devices"></a>Sanal cihazlar

Çözüm, çözümdeki uçtan uca akış sınamak için sanal cihazları havuzu yönetmenize olanak sağlayan bir mikro hizmet içerir. Sanal cihazlar:

* Cihaz bulut telemetri oluşturmak.
* Bulut cihaz yöntem çağrıları için IOT Hub'ından yanıt.

Mikro hizmet RESTful bir uç noktası oluşturma, başlatma ve durdurma benzetimleri olanak sağlar. Her benzetimi telemetri göndermesine ve yöntem çağrıları için yanıt farklı türdeki sanal cihazlar oluşur.

Sanal cihazlar çözüm Portalı'nda panodan sağlayabilirsiniz.

### <a name="physical-devices"></a>Fiziksel cihazlar

Fiziksel aygıtların çözümüne bağlayabilirsiniz. Azure IOT cihaz SDK'ları kullanarak sanal cihazlarınızın davranışını uygulayabilirsiniz.

Çözüm Portalı'nda panodan fiziksel aygıtların sağlayabilirsiniz.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IOT hub'ı ve IOT Yöneticisi mikro hizmet

[IOT hub'ı](../iot-hub/index.yml) cihazlardan buluta gönderilen verileri alır ve için kullanılabilir hale getirir `telemetry-agent` mikro hizmet.

IoT hub çözümde aynı zamanda şunları yapar:

* Kimlikleri ve Portalı'na bağlanmasına izin verilen tüm cihazların kimlik doğrulama anahtarlarını depolayan bir kimlik kayıt defteri tutar. Cihazları kimlik kayıt defterinden etkinleştirebilir ve devre dışı bırakabilirsiniz.
* Çözüm portalı adına cihazlarınızda komut çağırır.
* Tüm kayıtlı cihazlar için cihaz ikizlerini tutar. Cihaz ikizi bir cihaz tarafından bildirilen özellik değerlerini depolar. Cihaz ikizi ayrıca cihaz bir kez daha bağlandığında alabilmesi için çözüm portalında ayarlanmış istenen özellikleri depolar.
* Birden fazla cihaza ait özellikleri ayarlamak veya birden fazla cihaz üzerinde yöntem çağırmak için işleri zamanlar.

Çözüm içeren `iot-manager` mikro IOT hub'ınızı ile etkileşim gibi işlemek için:

* Oluşturma ve IOT cihazları yönetme.
* Cihaz çiftlerini yönetme.
* Cihazlarda yöntemlerini çağırma.
* IOT kimlik bilgilerini yönetme.

Bu hizmet, kullanıcı tanımlı gruplarına ait cihazları almak için sorguları IOT hub'ı da çalışır.

Mikro hizmet cihazları ve cihaz çiftlerini yönetme yöntemleri çağırma ve IOT hub'ı sorguları çalıştırmak için bir RESTful uç noktası sağlar.

## <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Çözüm veri işleme ve analizi mantıksal mimarisi parçası aşağıdaki bileşenleri içerir:

### <a name="device-telemetry"></a>Cihaz telemetrisi

Çözüm cihaz telemetri işlemek için iki mikro içerir.

[Telemetri aracısını](https://github.com/Azure/telemetry-agent-dotnet) mikro hizmet:

* Telemetri Cosmos DB içinde depolar.
* Cihazlara ait telemetri akışına analiz eder.
* Tanımlı kurallara göre uyarılar oluşturur.

Alarmlar Cosmos DB içinde depolanır.

`telemetry-agent` Mikro hizmet aygıtlardan gönderilen telemetriyi okumak çözüm portalı sağlar. Çözüm portalı da bu hizmete kullanır:

* Alarmlar tetiklemek eşikleri gibi izleme kurallarını tanımlayın
* Son uyarıları listesini alır.

Telemetri, kuralları ve Uyarıları yönetmek için bu mikro hizmet tarafından sağlanan RESTful uç noktası kullan.

### <a name="storage"></a>Depolama

[Depolama bağdaştırıcısı](https://github.com/Azure/pcs-storage-adapter-dotnet) mikro hizmet Çözüm Hızlandırıcısı için kullanılan ana depolama hizmeti önünde bir bağdaştırıcı olduğundan. Basit bir koleksiyonun ve anahtar-değer depolama sağlar.

Çözüm Hızlandırıcısı standart dağıtımını Cosmos DB kendi ana depolama hizmeti kullanır.

Cosmos DB veritabanı Çözüm Hızlandırıcısı verileri depolar. **Depolama bağdaştırıcısı** mikro hizmet çözümü depolama hizmetlerine erişmek için diğer mikro hizmetler için bir bağdaştırıcı görür.

## <a name="presentation"></a>Sunum

Çözüm mantıksal mimarisi sunu parçası aşağıdaki bileşenleri içerir:

[Web kullanıcı arabirimi olan React Javascript uygulama](https://github.com/Azure/pcs-remote-monitoring-webui). Uygulama:

* JavaScript tepki yalnızca kullanır ve tamamen tarayıcıda çalıştırır.
* CSS stili.
* Genel kullanıma yönelik mikro AJAX çağrıları aracılığıyla ile etkileşim kurar.

Kullanıcı arabirimi tüm Çözüm Hızlandırıcısı işlevselliği sunar ve diğer hizmetlerle gibi etkileşim kurar:

* [Kimlik doğrulaması](https://github.com/Azure/pcs-auth-dotnet) kullanıcı verilerini korumak için mikro hizmet.
* [İothub-manager](https://github.com/Azure/iothub-manager-dotnet) listelemek ve IOT cihazları yönetmek için mikro hizmet.

[UI-config](https://github.com/Azure/pcs-config-dotnet) mikro hizmet depolamak ve yapılandırma ayarlarını almak kullanıcı arabirimi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak kodu ve geliştirici belgeleri araştırmak istiyorsanız, biriyle iki ana GitHub depolarının başlatın:

* [Azure IOT (.NET) ile Uzaktan izleme Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Azure IOT (Java) uzaktan izleme Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).
* [Uzaktan izleme mimarisi için Çözüm Hızlandırıcısı)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Uzaktan izleme Çözüm Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz: [Çözüm Hızlandırıcısı özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
