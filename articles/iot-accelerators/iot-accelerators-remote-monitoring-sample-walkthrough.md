---
title: Uzaktan izleme çözüm hızlandırıcısına genel bakış - Azure | Microsoft Docs
description: Uzaktan izleme çözüm Hızlandırıcısını genel bakış.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 03/08/2019
ms.author: dobett
ms.openlocfilehash: af09ea39f373d518d5600e3fa46adc378fd9236d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61442548"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Uzaktan İzleme çözümü hızlandırıcısına genel bakış

Uzaktan izleme [Çözüm Hızlandırıcısı](../iot-accelerators/about-iot-accelerators.md) birden çok makine için uçtan uca bir izleme çözümü, uzak konumlarda uygular. Bu çözüm, iş senaryosunun genel uygulamasını sağlamak üzere temel Azure hizmetlerini bir araya getirir. Çözümü kendi uygulamanız için başlangıç noktası olarak kullanabilirsiniz ve [özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) kendi özel iş gereksinimlerinizi karşılayacak şekilde.

Bu makalede Uzaktan izleme çözümünün nasıl çalıştığını anlamanız için temel öğelerinden bazıları açıklanmaktadır. Bu bilgiler şunları yapmanıza yardımcı olur:

* Çözümdeki sorunları giderme.
* Çözümü kendinize özel gereksinimleri karşılayacak şekilde nasıl özelleştireceğinizi planlama.
* Azure hizmetlerini kullanan kendi IoT çözümünüzü tasarlama.

Uzaktan izleme çözüm Hızlandırıcı kodunu Github'da kullanılabilir:

* [.NET](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

## <a name="logical-architecture"></a>Mantıksal mimari

Aşağıdaki diyagramda yayılan Uzaktan izleme çözüm Hızlandırıcısını mantıksal bileşenlerinin özetler [IOT mimarisi](../iot-fundamentals/iot-introduction.md):

![Mantıksal mimari](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Neden mikro hizmetler?

Microsoft ilk Çözüm Hızlandırıcıları yayımlanan bu yana bulut mimarisi gelişmiştir. [Mikro Hizmetler](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) geliştirme hızdan ödün vermeden ölçek ve esneklik elde etmek için kendini kanıtlamış bir yöntem olarak ortaya çıkmıştır. Çeşitli Microsoft hizmetleriyle bu mimari deseni harika güvenilirlik ve ölçeklenebilirlik sonuçları ile dahili olarak kullanır. Bunları ayrıca yararlanabilir, böylece güncelleştirilmiş Çözüm Hızlandırıcıları bu dersleri uygulama içine yerleştirin.

> [!TIP]
> Mikro hizmet mimarileri hakkında daha fazla bilgi için bkz: [.NET uygulama mimarisi](https://www.microsoft.com/net/learn/architecture) ve [mikro hizmetler: Bulut tarafından desteklenen bir uygulama Devrimi](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="device-connectivity"></a>Cihaz bağlantısı

Çözüm, mantıksal mimarisini cihaz bağlantısı parçası aşağıdaki bileşenleri içerir:

### <a name="real-devices"></a>Gerçek cihaz

Çözüme gerçek cihazda bağlanabilirsiniz. Azure IOT cihaz SDK'larını kullanarak sanal cihazlarınızı davranışını uygulayabilirsiniz.

Çözüm portalı Pano gerçek cihazlardan sağlayabilirsiniz.

### <a name="device-simulation-microservice"></a>Cihaz benzetimi mikro hizmet

Çözümde [cihaz benzetimi mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-simulation) sanal cihazlarla bir havuz çözümde uçtan uca akışı test etmek için çözüm portalından yönetmenize imkan sağlar. Sanal cihazlar:

* CİHAZDAN buluta telemetri oluşturun.
* Bulut-cihaz yöntem çağrıları için IOT Hub'ından yanıt.

Mikro hizmet, oluşturmak, başlatmak ve benzetimleri durdurmak bir RESTful uç noktası sağlar. Telemetri gönderme ve yöntem çağrıları için yanıt farklı türlerde sanal cihazlar her benzetimi oluşur.

Çözüm portalı Pano sanal cihazlardan sağlayabilirsiniz.

### <a name="iot-hub"></a>IoT Hub

[IOT hub'ı](../iot-hub/index.yml) hem gerçek ve sanal cihazlardan buluta gönderilen telemetri alır. IOT hub'ı telemetriyi kullanılabilir hizmetlerine IOT çözümü arka uç işleme için yapar.

IoT hub çözümde aynı zamanda şunları yapar:

* Kimlikleri ve portala bağlanmasına izin verilen tüm cihazların kimlik doğrulama anahtarlarını depolayan bir kimlik kayıt defteri tutar.
* Çözüm Hızlandırıcısını adına cihazlarınızda yöntem çağırır.
* Tüm kayıtlı cihazlar için cihaz ikizlerini tutar. Cihaz ikizi bir cihaz tarafından bildirilen özellik değerlerini depolar. Cihaz ikizi ayrıca cihaz bir kez daha bağlandığında alabilmesi için çözüm portalında ayarlanmış istenen özellikleri depolar.
* Birden fazla cihaza ait özellikleri ayarlamak veya birden fazla cihaz üzerinde yöntem çağırmak için işleri zamanlar.

## <a name="data-processing-and-analytics"></a>Veri işleme ve analizi

Çözüm, veri işleme ve analizi mantıksal mimarisini parçası aşağıdaki bileşenleri içerir:

### <a name="iot-hub-manager-microservice"></a>IOT Hub Yöneticisi mikro hizmet

Çözümde [IOT Hub Yöneticisi mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) IOT hub'ınıza etkileşim gibi işlemek için:

* Oluşturma ve IOT cihazları yönetme.
* Cihaz ikizlerini yönetme.
* Cihazlarda yöntemleri çağırma.
* IOT kimlik bilgilerini yönetme.

Bu hizmet, kullanıcı tanımlı gruba ait olan cihazları almak için sorguları da IOT hub'ı çalışır.

Mikro hizmet, cihazları ve cihaz ikizlerini yönetme, çağırma yöntemlerinin ve IOT hub'ı sorguları çalıştırmak için bir RESTful uç noktası sağlar.

### <a name="device-telemetry-microservice"></a>Cihazın telemetri mikro hizmet

[Cihaz telemetrisi mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-telemetry) yönelik okuma erişimi cihaz telemetrisi Time Series Insights içinde depolanan bir RESTful uç noktası sağlar. RESTful uç noktası, kuralları ve okuma/yazma erişimi için depolama uyarısı tanımlarından CRUD işlemleri de sağlar.

### <a name="storage-adapter-microservice"></a>Depolama bağdaştırıcısı mikro hizmet

[Depolama bağdaştırıcısı mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/storage-adapter) Azure Cosmos DB kullanarak herhangi bir biçimdeki verileri depolamak için depolama hizmeti semantiği özetleyen ve basit bir arabirim sunan anahtar-değer çiftleri yönetir.

Değerler, koleksiyonlar düzenlenir. Tüm koleksiyonlar getirme ya da tek tek değerler üzerinde çalışır. Karmaşık veri yapılarını istemciler tarafından seri hale getirilmiş ve basit metin yükü olarak yönetilebilir.

Hizmet, anahtar-değer çiftleri CRUD işlemleri için bir RESTful uç noktası sağlar. Değerleri

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Çözüm Hızlandırıcı dağıtımları kullanın [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) kuralları, uyarılar, yapılandırma ayarlarını ve diğer tüm soğuk depolama depolamak için.

### <a name="azure-stream-analytics-manager-microservice"></a>Azure Stream Analytics Yöneticisi mikro hizmet

[Azure Stream Analytics Yöneticisi mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/asa-manager) başlatma ve bunları durduruluyor ve durumlarını İzleme yapılandırmalarını ayarlama dahil olmak üzere, Azure Stream Analytics (ASA) işleri yönetir.

ASA işi iki başvuru veri kümesi tarafından desteklenir. Kuralları tek bir veri kümesini tanımlar ve bir cihaz gruplarını tanımlar. Kuralları başvuru verilerini cihaz telemetrisi mikro hizmet tarafından yönetilen bilgileri oluşturulur. Azure Stream Analytics Yöneticisi mikro hizmet telemetri kuralları işleme mantığı bir akışa dönüştürür.

Cihaz grupları başvuru verilerini kuralları için gelen bir telemetri iletisi uygulamak grubunu tanımlamak için kullanılır. Cihaz gruplarını yapılandırma mikro hizmet tarafından yönetilen ve Azure IOT Hub cihaz çifti sorguları kullanın.

ASA işleri telemetri, bağlı cihazlardan depolama ve analiz için zaman serisi öngörüleri sunun.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) , yüksek hacimli verileri cihazlardan akışı incelemenize olanak sağlayan bir olay işleme altyapısıdır.

### <a name="azure-time-series-insights"></a>Azure Zaman Serisi Görüşleri

[Azure Time Series Insights](https://docs.microsoft.com/azure/time-series-insights/) cihazlardaki telemetri bağlı çözüm hızlandırıcısına depolar. Ayrıca cihaz telemetrisi çözüm Web kullanıcı Arabiriminde sorgulama ve görselleştirme sağlar.

> [!NOTE]
> Zaman serisi görüşleri Azure Çin Bulutu şu anda kullanılabilir değil. Yeni Uzaktan izleme çözüm Hızlandırıcı dağıtımlarda Azure Çin Bulutu, Cosmos DB için tüm depolama kullanın.

### <a name="configuration-microservice"></a>Yapılandırma mikro hizmet

[Yapılandırma mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config) cihaz grupları, çözüm ayarları ve kullanıcı-çözüm Hızlandırıcısını ayarlarında CRUD işlemleri için bir RESTful uç noktası sağlar. Yapılandırma verileri kalıcı hale getirmek için depolama bağdaştırıcısı mikro hizmet ile çalışır.

### <a name="authentication-and-authorization-microservice"></a>Kimlik doğrulama ve yetkilendirme mikro hizmet

[Kimlik doğrulama ve yetkilendirme mikro hizmet](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) çözüm Hızlandırıcısını erişmeye yetkili kullanıcıları yönetir. Kullanıcı Yönetimi gerçekleştirilebilir destekleyen herhangi bir kimlik hizmeti sağlayıcısı kullanarak [Openıd Connect](https://openid.net/connect/).

### <a name="azure-active-directory"></a>Azure Active Directory

Çözüm Hızlandırıcı dağıtımları kullanın [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Openıd Connect sağlayıcısı olarak. Azure Active Directory, kullanıcı bilgilerini depolar ve JWT doğrulamak için sertifikaları imzaları belirteci sağlar.

## <a name="presentation"></a>Sunum

Çözüm mantıksal mimarisini sunu bölümünde aşağıdaki bileşenleri içerir:

[Web kullanıcı arabirimi, React Javascript bir uygulamadır](https://github.com/Azure/pcs-remote-monitoring-webui). Uygulama:

* Tamamen tarayıcıda çalışan ve React JavaScript'ı yalnızca kullanır.
* CSS stil uygulanmış.
* AJAX çağrıları aracılığıyla genel kullanıma yönelik mikro hizmetler ile etkileşim kurar.

Kullanıcı arabirimi tüm çözüm Hızlandırıcı işlevselliği sunar ve ile diğer mikro hizmetler gibi etkileşim kurar:

* Kullanıcı verilerini korumak için kimlik doğrulama ve yetkilendirme mikro hizmet.
* Listelemek ve IOT cihazları yönetmek için IOT Hub Yöneticisi mikro hizmet.

Kullanıcı arabirimi, sorgulama ve cihaz telemetrisi analizini etkinleştirmek için Azure Time Series Insights gezgininin birleştirir.

Yapılandırma mikro hizmet depolamak ve yapılandırma ayarlarını almak kullanıcı arabirimi sağlar.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak kodu ve geliştirici belgeleri araştırmak istiyorsanız, iki GitHub depoları biriyle başlayın:

* [Azure IOT (.NET) ile Uzaktan izleme çözüm Hızlandırıcısını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet).
* [(Java) Azure IOT ile Uzaktan izleme çözüm Hızlandırıcısını](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Mimari diyagramları ayrıntılı çözümü:
* [Uzaktan izleme mimarisi için çözüm hızlandırıcı](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Uzaktan izleme çözüm Hızlandırıcısını hakkında daha fazla kavramsal bilgi için bkz. [çözüm Hızlandırıcısını özelleştirme](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
