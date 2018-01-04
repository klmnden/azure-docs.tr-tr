---
title: "Azure IOT hub'ı terimler sözlüğü | Microsoft Docs"
description: "Geliştirici Kılavuzu - Azure IOT Hub'ına ilgili ortak terimleri içeren sözlük."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
<<<<<<< HEAD
ms.openlocfilehash: 87ab620444df4588cc43a3691cb215006561090d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
=======
ms.openlocfilehash: 406fd095896e2c00920555d3dfce1b5c2ae7fca7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="glossary-of-iot-hub-terms"></a>IOT hub'ı terimler sözlüğü
Bu makalede IOT hub'ı makalelerinde kullanılan ortak terimleri bazıları listelenmektedir.

## <a name="advanced-message-queueing-protocol"></a>Gelişmiş Message Queuing protokolü
[Message Queuing Protokolü (AMQP) Gelişmiş](https://www.amqp.org/) olan bir Mesajlaşma protokolleri [IOT hub'ı](#iot-hub) aygıtlarıyla iletişim kurmak için destekler. IOT hub'ı destekleyen Mesajlaşma protokolleri hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Azure CLI
[Azure CLI](../cli-install-nodejs.md) oluşturmak ve Microsoft Azure kaynakları yönetmek için bir platformlar arası, açık kaynaklı, kabuk tabanlı komut bir araçtır. Bu sürümü CLI, Node.js kullanarak uygulanır.

## <a name="azure-cli-20"></a>Azure CLI 2.0
[Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) oluşturmak ve Microsoft Azure kaynakları yönetmek için bir platformlar arası, açık kaynaklı, kabuk tabanlı komut bir araçtır. Bu önizleme sürümü CLI, Python kullanılarak uygulanır.


## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları
Vardır _cihaz SDK'ları_ oluşturmanıza olanak sağlayan birden çok dil için kullanılabilir [cihaz uygulamaları](#device-app) bir IOT hub ile etkileşim. IOT hub'ı öğreticiler bu cihaz SDK'ları kullanmayı gösterir. Bu Github'da kaynak kodu ve cihaz SDK'ları hakkında daha fazla bilgi bulabilirsiniz [depo](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları
Vardır _SDK hizmeti_ oluşturmanıza olanak sağlayan birden çok dil için kullanılabilir [arka uç uygulamaları](#back-end-app) bir IOT hub ile etkileşim. IOT hub'ı öğreticiler bu hizmeti SDK'ları kullanmayı gösterir. Bu Github'da kaynak kodu ve hizmet SDK'ları hakkında daha fazla bilgi bulabilirsiniz [depo](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-portal"></a>Azure portalına
[Microsoft Azure portal](https://portal.azure.com) burada sağlamak ve Azure kaynaklarınızı yönetmek merkezi bir yerdir. İçerik kullanarak düzenler _dikey_.

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) Azure Windows PowerShell ile yönetmek için kullanabileceğiniz cmdlet'leri koleksiyonudur. Oluşturma, test, dağıtmak için cmdlet'leri kullanın ve çözümleri ve Azure platformu aracılığıyla teslim edildi hizmetlerini yönetebilir.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) bir grup olarak çözümünüzdeki kaynaklar ile çalışmanıza olanak tanır. Dağıtma, güncelleştirme veya kaynakları tek ve eşgüdümlü bir işlemle çözümünüzdeki silin.

## <a name="azure-service-bus"></a>Azure Service Bus
[Hizmet veri yolu](../service-bus/index.md) Kurumsal Mesajlaşma ile iletişimi bulut etkin ve yardımcı olan geçirilen iletişimi bağlanmak şirket içi çözümler ile bulut sağlar. Service Bus hizmetini kullanmak bazı IOT hub'ı öğreticileri olun [sıraları](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Azure depolama](../storage/common/storage-introduction.md) bulut depolama çözümüdür. Yapılandırılmamış nesne verilerini depolamak için kullanabileceğiniz Blob Depolama Birimi hizmeti içerir. Bazı IOT hub'ı öğreticileri blob storage'ı kullanın.

## <a name="back-end-app"></a>Arka uç uygulama
Bağlamında [IOT hub'ı](#iot-hub), bir arka uç uygulaması bir IOT hub'ındaki service'e yönelik uç noktalardan biri bağlanan bir uygulamadır. Örneğin, bir arka uç uygulaması alacağı [cihaz bulut](#device-to-cloud)yönetmek veya iletileri [kimlik kayıt defteri](#identity-registry). Genellikle, bir arka uç uygulama bulutta çalışır, ancak çoğu öğreticileri arka uç uygulamalar, yerel geliştirme makinenizde çalışan konsol uygulamalardır.

## <a name="built-in-endpoints"></a>Yerleşik uç noktaları
Her IOT hub'ı içeren yerleşik bir [endpoint](iot-hub-devguide-endpoints.md) Event Hub ile uyumlu olan. Bu uç noktasından cihaz-bulut iletileri okumak için Event Hubs ile çalışan herhangi bir mekanizma kullanabilirsiniz.

## <a name="cloud-gateway"></a>Bulut ağ geçidi
Bağlantıyı doğrudan bağlanamıyor cihazlar için bir bulut ağ geçidi etkinleştirir [IOT hub'ı](#iot-hub). Bulut ağ geçidi tersine için bulutta barındırılan bir [alan ağ geçidi](#field-gateway) çalıştıran yerel aygıtlarınıza. Bulut ağ geçidi için tipik kullanım örneği, cihazlarınız için protokol çevirisi uygulamaktır.

## <a name="cloud-to-device"></a>Buluttan cihaza
Bağlı bir aygıt bir IOT hub'ından gönderilen iletileri gösterir. Genellikle, bu iletiler, bir eylem aygıta yönlendiren komutlardır. Daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Bağlantı dizesi
Bir bitiş noktasına bağlanmak için gereken bilgileri yalıtan uygulama kodunuzda bağlantı dizelerini kullanın. Bir bağlantı dizesi genellikle endpoint ve güvenlik bilgileri, ancak bağlantı dizesi biçimleri hizmetleri arasında farklılık adresini içerir. IOT Hub hizmeti ile ilişkili bağlantı dizesi iki tür vardır:
- *Cihaz bağlantı dizelerini* bir IOT hub'ında cihaz'te Uç noktalara bağlanmak aygıtları etkinleştirin.
- *IOT Hub bağlantı dizelerini* bir IOT hub'ındaki service'te Uç noktalara bağlanmak arka uç uygulamaları etkinleştirme.

## <a name="custom-endpoints"></a>Özel uç noktaları
Oluşturabileceğiniz özel [uç noktaları](iot-hub-devguide-endpoints.md) tarafından gönderilen iletileri sunmak için bir IOT hub'ında bir [yönlendirme kuralı](#routing-rules). Özel uç noktaları, doğrudan bir olay hub'ı, Service Bus kuyruğuna veya Service Bus konu bağlanın.

## <a name="custom-gateway"></a>Özel ağ geçidi
Bağlantıyı doğrudan bağlanamıyor cihazlar için bir ağ geçidi etkinleştirir [IOT hub'ı](#iot-hub). Kullanabileceğiniz [Azure IOT kenar](#azure-iot-edge) iletileri, özel protokol dönüşümler ve başka bir işlem olarak kenarında işlemek için özel mantığı uygulamanız özel ağ geçitleri oluşturmak için.

## <a name="data-point-message"></a>Veri noktası iletisi
Veri noktası iletisi bir [cihaz bulut](#device-to-cloud) içeren ileti [telemetri](#telemetry) Rüzgar hızı veya sıcaklık gibi verileri.

## <a name="desired-configuration"></a>İstenen yapılandırma
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), yapılandırma özelliklerinin ve meta verileri cihazla eşitlenmesi gereken cihaz çiftine eksiksiz başvurduğu istenen.

## <a name="desired-properties"></a>İstenen özellikleri
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), istenen özellikleri ile birlikte kullanılan cihaz çifti alt [özellikleri bildirilen](#reported-properties) aygıt yapılandırması veya koşul eşitlenecek. İstenen özellikleri yalnızca ayarlanabilir bir [arka uç uygulama](#back-end-app) ve tarafından uyulması gereken [cihaz uygulaması](#device-app).

## <a name="device-to-cloud"></a>Cihazdan buluta
Bağlı bir CİHAZDAN gönderilen iletileri başvurduğu [IOT hub'ı](#iot-hub). Bu iletiler olabilir [veri noktası](#data-point-message) veya [etkileşimli](#interactive-message) iletileri. Daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine](iot-hub-devguide-messaging.md).

## <a name="device"></a>Cihaz
IOT bağlamında, bir aygıt genellikle, veri toplayabilir veya diğer cihazları denetlemek bir küçük ölçekli, tek başına işlem cihazıdır. Örneğin, bir aygıt bir ortam izleme aygıtı veya bir greenhouse su kaynağı ve havalandırma sistemlerinde denetleyicisi olabilir. [Aygıt katalog](https://catalog.azureiotsuite.com/) çalışmak için sertifikalı donanım cihazlarının bir listesini sağlar [IOT hub'ı](#iot-hub).

## <a name="device-app"></a>Cihaz uygulaması
Bir cihaz uygulamasının çalışır, [aygıt](#device) ve iletişim işler, [IOT hub'ı](#iot-hub). Genellikle, birini kullanmanız [Azure IOT cihaz SDK'ları](#azure-iot-device-sdks) uygulamak zaman cihaz uygulaması. Birçok IOT öğreticileri kullandığınız bir [sanal cihaz](#simulated-device) kolaylık sağlamak için.

## <a name="device-condition"></a>Cihaz durumu
Şu anda kullanımda bağlantı yöntemi gibi cihaz durumu bilgilerini tarafından bildirilen başvurduğu bir [cihaz uygulaması](#device-app). [Cihaz uygulamaları](#device-app) yeteneklerini de bildirebilirsiniz. Cihaz çiftlerini kullanarak koşul ve yetenek bilgi sorgulayabilirsiniz.

## <a name="device-data"></a>Cihaz verileri
IOT hub'ı depolanan aygıt başına veri başvurduğu cihaz verileri [kimlik kayıt defteri](#identity-registry). İçeri ve bu verileri dışarı aktarma mümkündür.

## <a name="device-explorer"></a>Cihaz Gezgini
[Aygıt explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) Windows üzerinde çalışan ve cihazlarınızı yönetmenize olanak sağlayan bir araçtır [kimlik kayıt defteri](#identity-registry). Aracı da gönderebilir ve cihazlarınıza iletileri alacak.

## <a name="device-identities-rest-api"></a>Cihaz Kimlikleri REST API’si
[Aygıt kimlikleri REST API](https://docs.microsoft.com/rest/api/iothub/iothubresource) kayıtlı cihazlarınızı yönetmenize olanak veren [kimlik kayıt defteri](#identity-registry) bir REST API kullanarak. Genellikle, üst düzey birini kullanmalıdır [SDK hizmeti](#azure-iot-service-sdks) IOT hub'ı eğitimlerine gösterildiği gibi.

## <a name="device-identity"></a>Cihaz kimliği
Cihaz kimliği, kayıtlı her bir aygıtı atanan benzersiz tanımlayıcıdır [kimlik kayıt defteri](#identity-registry).

## <a name="device-management"></a>Cihaz yönetimi
Cihaz Yönetimi çözümünüzde planlama, sağlama, yapılandırma, izleme ve devre dışı dahil olmak üzere IOT cihazları yönetmeyle ilgili tam yaşam kapsar.

## <a name="device-management-patterns"></a>Cihaz yönetimi modelleri
[IOT hub'ı](#iot-hub) yeniden başlatma, fabrika ayarlarına sıfırlama gerçekleştirme ve bellenim güncelleştirmeleri aygıtlarınızda gerçekleştirme gibi sık kullanılan aygıt yönetimi desenlerini sağlar.

## <a name="device-messaging-rest-api"></a>Cihaz Mesajlaşma REST API’si
Kullanabileceğiniz [cihaz Mesajlaşma REST API](https://docs.microsoft.com/rest/api/iothub/httpruntime) bir IOT hub'ına cihaz bulut iletilerini göndermek ve almak için bir aygıttan [bulut cihaz](#cloud-to-device) IOT hub'ı gelen iletileri. Genellikle, üst düzey birini kullanmalıdır [cihaz SDK'ları](#azure-iot-device-sdks) IOT hub'ı eğitimlerine gösterildiği gibi.

## <a name="device-provisioning"></a>Cihaz sağlama
Cihaz sağlama olan ilk ekleme işlemini [aygıt verilerini](#device-data) çözümünüzdeki depolarına. Hub'ınıza bağlanmak yeni bir cihaz etkinleştirmek için bir cihaz kimliği ve anahtarları IOT Hub'ına eklemelisiniz [kimlik kayıt defteri](#identity-registry). Hazırlama işleminin bir parçası olarak, diğer çözüm depolarında aygıta özgü verileri başlatma gerekebilir.

## <a name="device-twin"></a>Cihaz çifti
A [cihaz çifti](iot-hub-devguide-device-twins.md) meta verileri, yapılandırmaları ve koşulları gibi cihaz durumu bilgilerini depolar JSON belgesi. [IOT hub'ı](#iot-hub) IOT hub'ınıza sağlamak her cihaz için cihaz çifti devam ettirir. Cihaz çiftlerini eşitlemek etkinleştirme [aygıt koşullar](#device-condition) yapılandırmaları arasında cihaz ve çözüm arka uç ve. Belirli aygıtları bulmak ve uzun süre çalışan işlemleri durumunu sorgulamak için cihaz çiftlerini sorgulayabilirsiniz.

## <a name="device-twin-queries"></a>Cihaz çifti sorguları
[Cihaz çifti sorguları](iot-hub-devguide-query-language.md) SQL benzeri IOT hub'ı sorgu dili, cihaz çiftlerini bilgileri almak için kullanın. Aynı IOT hub'ı sorgu dili hakkında bilgi almak için kullanabileceğiniz [işleri](#job) IOT hub'ınıza çalışıyor.

## <a name="device-twin-rest-api"></a>Cihaz çifti REST API'si
Kullanabileceğiniz [cihaz çifti REST API](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) çözümden arka uç cihaz çiftlerini yönetmek için. API almak ve güncelleştirmek sağlar [cihaz çifti](#device-twin) özellikleri ve çağırma [doğrudan yöntemleri](#direct-method). Genellikle, üst düzey birini kullanmalıdır [SDK hizmeti](#azure-iot-service-sdks) IOT hub'ı eğitimlerine gösterildiği gibi.

## <a name="device-twin-synchronization"></a>Cihaz çifti eşitleme
Cihaz çifti eşitlemenin kullandığı [özellikleri istenen](#desired-properties) cihazlarınızı yapılandırmak ve almak için cihaz çiftlerini içinde [özellikleri bildirilen](#reported-properties) cihaz çiftine depolamak için aygıtlardan.

## <a name="direct-method"></a>Doğrudan yöntemi
A [doğrudan yöntemi](iot-hub-devguide-direct-methods.md) bir IOT hub'ınızı API'sini çağırarak bir aygıtta yürütmek için bir yöntemi tetiklemek yoldur.

## <a name="endpoint"></a>Uç Nokta
IOT hub'ı birden çok sunan [uç noktaları](iot-hub-devguide-endpoints.md) IOT hub'ına bağlanmak, uygulamalarınızı etkinleştirin. Gönderme gibi işlemler gerçekleştirmek aygıtları etkinleştirin aygıt'e yönelik uç noktalar vardır [cihaz bulut](#device-to-cloud) iletileri ve alma [bulut cihaz](#cloud-to-device) iletileri. Etkinleştirme hizmeti kullanıma yönelik yönetim uç noktalar vardır [arka uç uygulamaları](#back-end-app) gibi işlemleri gerçekleştirmek için [cihaz kimliği](#device-identity) ve cihaz çifti yönetimi. Hizmet dönük vardır [yerleşik uç noktaları](#built-in-endpoints) cihaz-bulut iletileri okumak için. Oluşturabileceğiniz [özel uç noktaları](#custom-endpoints) tarafından gönderilen cihaz-bulut iletileri almak için bir [yönlendirme kuralı](#routing-rules).

## <a name="event-hubs-service"></a>Olay hub'ları hizmeti
[Olay hub'ları](../event-hubs/event-hubs-what-is-event-hubs.md) milyonlarca işleyebilen bir yüksek düzeyde ölçeklenebilir veri alım sistemidir saniye başına olayların. Hizmet, işleme ve veri bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki çözümlemek sağlar. IOT Hub hizmeti ile bir karşılaştırması için bkz: [karşılaştırma Azure IOT Hub ve Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Olay Hub'ı ile uyumlu uç nokta
Okunacak [cihaz bulut](#device-to-cloud) , hub'ına bir bitiş noktasına bağlanmak ve bu iletileri okumak için herhangi bir Event Hub ile uyumlu yöntemini kullanmak, IOT hub'ına gönderilen iletileri. Event Hub ile uyumlu yöntemleri dahil kullanarak [olay hub'ları SDK'ları](../event-hubs/event-hubs-programming-guide.md) ve [Azure akış analizi](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Alan ağ geçidi
Bağlantıyı doğrudan bağlanamıyor cihazlar için bir alan ağ geçidi etkinleştirir [IOT hub'ı](#iot-hub) ve genellikle cihazlarınızı yerel olarak dağıtılır. Daha fazla bilgi için bkz: [Azure IOT Hub nedir?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Ücretsiz hesap
Oluşturabileceğiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) IOT hub'ı öğreticiler ve IOT Hub hizmeti (ve diğer Azure Hizmetleri) ile deneyin.

## <a name="gateway"></a>Ağ geçidi
Bağlantıyı doğrudan bağlanamıyor cihazlar için bir ağ geçidi etkinleştirir [IOT hub'ı](#iot-hub). Ayrıca bkz. [alan ağ geçidi](#field-gateway), [bulut ağ geçidi](#cloud-gateway), ve [özel ağ geçidi](#custom-gateway).

## <a name="identity-registry"></a>Kimlik kayıt defteri
[Kimlik kayıt defteri](iot-hub-devguide-identity-registry.md) ayrı aygıtlar hakkındaki bilgileri depolayan bir IOT hub'ın yerleşik bileşeni bir IOT hub'ına bağlanmak için izin verilir.

## <a name="interactive-message"></a>Etkileşimli iletisi
Etkileşimli bir ileti bir [bulut cihaz](#cloud-to-device) çözüm arka ucu anlık bir eylemi tetikleyen ileti. Örneğin, bir cihaz otomatik olarak bir CRM sistemine oturum açması bir hata hakkında bir uyarı gönderebilir.

[!INCLUDE [azure-iot-hub-edge-glossary-includes](../../includes/azure-iot-hub-edge-glossary-includes.md)]

## <a name="iot-hub"></a>IoT Hub’ı
IOT hub'ı milyonlarca cihaza arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir Azure hizmeti olduğundan ve bir çözüm arka ucu. Daha fazla bilgi için bkz: [Azure IOT Hub nedir?](iot-hub-what-is-iot-hub.md) Kullanarak, [Azure aboneliği](#subscription), iş yüklerini Mesajlaşma, IOT işlemek için IOT hub'ları oluşturabilirsiniz.

## <a name="iot-hub-metrics"></a>IOT hub'ı ölçümleri
[IOT hub'ı ölçümleri](iot-hub-metrics.md) IOT hub'ları durumuyla ilgili size, [Azure aboneliği](#subscription). IOT hub'ı ölçümleri, hizmet ve ona bağlı aygıtlar genel durumunu değerlendirmek etkinleştirin. IOT hub'ı ölçümleri IOT hub'ınıza neler olup bittiğini görmek ve Azure desteğine başvurun gerek kalmadan kök neden sorunları araştırmanıza yardımcı olabilir.

## <a name="iot-hub-query-language"></a>IOT hub'ı sorgulama dili
[IOT hub'ı sorgu dili](iot-hub-devguide-query-language.md) sorgusu tanır SQL benzeri bir dil olan, [işleri](#job) ve cihaz çiftlerini.

## <a name="iot-hub-resource-provider-rest-api"></a>IOT hub'ı kaynak sağlayıcısı REST API'si
Kullanabileceğiniz [IOT Hub kaynak sağlayıcısı REST API](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) , IOT hub'ları yönetmek için [Azure aboneliği](#subscription) oluşturma, güncelleştirme ve hub'ları silme gibi işlemleri gerçekleştirme.

## <a name="iot-suite"></a>IoT Paketi
Azure IOT paketi önceden yapılandırılmış çözümleri birden çok Azure hizmetleriyle birlikte paketler. Bu önceden yapılandırılmış çözümler, ortak IOT senaryolarını uçtan uca uygulamaları ile hızlı bir şekilde başlamak etkinleştirin. Daha fazla bilgi için bkz: [Azure IOT paketi nedir?](../iot-suite/iot-suite-overview.md)

## <a name="iothub-explorer"></a>ıothub Gezgini
[İothub-explorer](https://github.com/azure/iothub-explorer) platformlar arası, komut satırı aracıdır. Aracı, cihazlarınızı yönetmenize olanak tanır [kimlik kayıt defteri](#identity-registry)göndermek ve iletileri ve dosyaları, almasını ve IOT hub işlemlerini izleyebilirsiniz.

## <a name="job"></a>İş
Çözüm arka ucunuz kullanabilirsiniz [işleri](iot-hub-devguide-jobs.md) zamanlamak ve IOT hub'ınıza kayıtlı cihazlar üzerinde etkinliklerini izlemek için. Etkinlikler dahil cihaz çifti güncelleştirme [özelliklerini istenen](#desired-properties), güncelleştirme cihaz çifti [etiketleri](#tags)ve çağırma [doğrudan yöntemleri](#direct-method). [IOT hub'ı](#iot-hub) işleri için de kullanır [için içeri ve dışarı aktarma](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) gelen [kimlik kayıt defteri](#identity-registry).

## <a name="jobs-rest-api"></a>İşlerini REST API'si
[İşleri REST API](https://docs.microsoft.com/rest/api/iothub/jobapi) yönetmenize olanak veren [işleri](#job) IOT hub'ınıza çalıştıran.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) olan bir Mesajlaşma protokolleri [IOT hub'ı](#iot-hub) aygıtlarıyla iletişim kurmak için destekler. IOT hub'ı destekleyen Mesajlaşma protokolleri hakkında daha fazla bilgi için bkz: [IOT Hub ile iletileri almasına ve göndermesine](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>İşlemleri izleme
IOT hub'ı [izleme işlemleri](iot-hub-operations-monitoring.md) gerçek zamanlı IOT hub'ınızı işlemlerinin durumunu izlemenizi sağlar. [IOT hub'ı](#iot-hub) işlemlerinin birkaç kategoriler arasında olayları izler. Bir IOT hub'ı uç işleme için bir veya daha fazla kategorilerden olayları göndermeyi seçebilirsiniz. Hatalar için verileri izlemek veya veri düzenlerini esas alarak daha karmaşık işleme ayarlayın.

## <a name="physical-device"></a>Fiziksel cihaz
Gerçek bir cihaz IOT hub'a bağlanan Raspberry Pi'yi gibi bir fiziksel aygıttır. Kolaylık olması için IOT hub'ı öğreticileri çoğunu kullanmak [benzetimli aygıtları](#simulated-device) örneklerini yerel makinenizde çalıştırma sağlamak için.

## <a name="primary-and-secondary-keys"></a>Birincil ve ikincil anahtarları
Bir IOT hub cihaz dönük veya hizmet dönük bir noktadaki bağlandığınızda, [bağlantı dizesi](#connection-string) size erişim vermek için anahtar içerir. Bir cihaza eklediğinizde [kimlik kayıt defteri](#identity-registry) veya ekleme bir [paylaşılan erişim ilkesi](#shared-access-policy) hub'ınıza, hizmeti birincil ve ikincil bir anahtar oluşturur. İki anahtarın kullanılması, IOT hub'ına erişimi kaybetmeden bir anahtar güncelleştirdiğinizde bir anahtardan diğerine geçir olanak sağlar.

## <a name="protocol-gateway"></a>Protokol ağ geçidi
Bir protokol ağ geçidi genellikle buluta dağıtılan ve protokolü bağlanan cihazlar için çeviri hizmetleri sağlayan [IOT hub'ı](#iot-hub). Daha fazla bilgi için bkz: [Azure IOT Hub nedir?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma
Vardır çeşitli [kotaları](iot-hub-devguide-quotas-throttling.md) kullanımınız için geçerli [IOT hub'ı](#iot-hub), kotalar IOT hub katman göre değişebilir. [IOT hub'ı](#iot-hub) de geçerlidir [kısıtlar](iot-hub-devguide-quotas-throttling.md) çalışma zamanında hizmet kullanımınız için.

## <a name="reported-configuration"></a>Bildirilen yapılandırma
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), yapılandırma özellikleri ve çözüm arka ucuna bildirilen cihaz çiftine meta veriler tamamını başvurduğu bildirdi.

## <a name="reported-properties"></a>Bildirilen özellikleri
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), özellikleri ile kullanılan cihaz çifti alt bildirilir [özelliklerini istenen](#desired-properties) aygıt yapılandırması veya koşul eşitlenecek. Bildirilen özellikleri yalnızca ayarlanabilir [cihaz uygulaması](#device-app) ve okunabilir ve tarafından sorgulanan bir [arka uç uygulama](#back-end-app).

## <a name="resource-group"></a>Kaynak grubu
[Azure Resource Manager](#azure-resource-manager) ilgili kaynaklar gruplamak için kaynak gruplarını kullanır. Bir kaynak grubu, aynı anda grubunun tüm kaynaklar üzerinde işlem gerçekleştirmek için kullanabilirsiniz.

## <a name="retry-policy"></a>Yeniden deneme ilkesi
İşlemek için bir yeniden deneme ilkesi kullanır [geçici hataları](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) bir bulut hizmetine bağlandığınızda.

## <a name="routing-rules"></a>Yönlendirme kuralları
Yapılandırdığınız [yönlendirme kuralları](iot-hub-devguide-messages-read-custom.md) cihaz-bulut iletileri yönlendirmek için IOT hub'bir [yerleşik uç nokta](#built-in-endpoints) veya [özel uç noktaları](#custom-endpoints) çözüm arka ucu tarafından işlenecek.

## <a name="sasl-plain"></a>SASL DÜZ
SASL DÜZ bir protokolüdür, [AMQP](#advanced-message-queue-protocol) Protokolü güvenlik belirteçleri aktarımı için kullanır.

## <a name="shared-access-signature"></a>Paylaşılan erişim imzası
Paylaşılan erişim imzaları (SAS), SHA-256 güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. SAS kimlik doğrulaması iki bileşeni vardır: bir _paylaşılan erişim ilkesi_ ve _paylaşılan erişim imzası_ (genellikle bir belirteç olarak da adlandırılır). Bir cihaz IOT hub ile kimlik doğrulaması yapmak için SAS kullanır. [Arka uç uygulamaları](#back-end-app) ayrıca service'e yönelik uç noktalar bir IOT hub'ındaki kimlik doğrulaması yapmak için SAS kullanın. Genellikle, SAS belirteci içerir [bağlantı dizesi](#connection-string) bir uygulama bir IOT hub bağlantı kurmak için kullanır.

## <a name="shared-access-policy"></a>Paylaşılan Erişim İlkesi
Bir paylaşılan erişim ilkesi geçerli olan herkes için verilen izinleri tanımlar [birincil veya ikincil anahtarı](#primary-and-secondary-keys) Bu ilkeyle ilişkilendirilmiş. Hub'ınıza için paylaşılan erişim ilkeleri ve anahtarları yönetebilir [portal](#azure-portal).

## <a name="simulated-device"></a>Sanal cihaz
Kolaylık sağlamak için IOT hub'ı öğreticileri çoğunu örneklerini yerel makinenizde çalıştırma sağlamak için sanal cihazlar kullanın. Buna karşılık, bir [fiziksel aygıt](#physical-device) gerçek bir cihaz IOT hub'a bağlanan Raspberry Pi'yi gibi.

## <a name="solution"></a>Çözüm
A _çözüm_ bir veya daha fazla projeleri içeren bir Visual Studio çözümü başvurabilir. A _çözüm_ cihazları gibi öğeleri içeren bir IOT çözüm için de başvurabilir [cihaz uygulamaları](#device-app), IOT hub'ı, diğer Azure hizmetleriyle ve [arka uç uygulamaları](#back-end-app).

## <a name="subscription"></a>Abonelik
Bir Azure aboneliği faturalama gerçekleştiği ' dir. Her Azure kaynak oluşturduğunuz veya Azure hizmeti kullandığınız tek bir abonelik ile ilişkilendirilir. Birçok kotaları, ayrıca bir abonelik düzeyinde uygulanır.

## <a name="system-properties"></a>Sistem özellikleri
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), Sistem özellikleri salt okunurdur ve son etkinlik süresi ve bağlantı durumu gibi cihaz kullanımı ile ilgili bilgiler içerir.

## <a name="tags"></a>Etiketler
Bağlamında bir [cihaz çifti](iot-hub-devguide-device-twins.md), etiketleri, cihaz meta verilerini depolanır ve bir JSON belgesinin biçiminde çözüm arka ucu tarafından alınır. Etiketler bir cihazdaki uygulamalar için görünür değildir.

## <a name="telemetry"></a>Telemetri
Cihazları Rüzgar hızı veya sıcaklık, gibi telemetri verileri toplama ve kullanma [veri noktası iletileri](#data-point-messages) bir IOT hub'ına telemetri göndermeyi.

## <a name="token-service"></a>Belirteç Hizmeti
Cihazlar için bir kimlik doğrulama mekanizması uygulamak için bir belirteci hizmeti kullanabilirsiniz. IOT hub'ı kullanan [paylaşılan erişim ilkesi](#shared-access-policy) ile **DeviceConnect** oluşturma izni *aygıt kapsamlı* belirteçleri. Bu belirteçler IOT hub'ınıza bağlanmak bir aygıt etkinleştirin. Bir aygıt, belirteç hizmeti ile kimlik doğrulaması için bir özel kimlik doğrulama mekanizması kullanır. Aygıt başarıyla GERÇEKLEŞTİRİYORSA, belirteç hizmetine cihazın IOT hub'ınızı erişmek için kullandığınız bir SAS belirteci verir.

## <a name="x509-client-certificate"></a>X.509 istemci sertifikası
Bir aygıt ile kimlik doğrulaması için bir X.509 sertifikası kullanabilirsiniz [IOT hub'ı](#iot-hub). Bir X.509 sertifikası kullanmaktır kullanmaya alternatif bir [SAS belirteci](#shared-access-signature).
