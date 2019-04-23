---
title: Azure IOT hub'ı terimler sözlüğü | Microsoft Docs
description: Geliştirici Kılavuzu - Azure IOT Hub'ına ilişkin sık kullanılan terimleri.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/15/2019
ms.openlocfilehash: 6f89e27b06179c33857d581c0c6e3fc78c683d48
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59793464"
---
# <a name="glossary-of-iot-hub-terms"></a>IOT hub'ı terimler sözlüğü
Bu makalede IOT hub'ı makalelerinde kullanılan yaygın terimlerin bazıları listelenmektedir.

## <a name="advanced-message-queueing-protocol"></a>Gelişmiş ileti sıraya alma Protokolü
[Gelişmiş ileti sıraya alma Protokolü (AMQP)](https://www.amqp.org/) olduğu bir Mesajlaşma protokolleri [IOT hub'ı](#iot-hub) cihazlarla iletişim için destekler. IOT hub'ın desteklediği Mesajlaşma protokolleri hakkında daha fazla bilgi için bkz. [göndermek ve IOT Hub ile ileti alma](iot-hub-devguide-messaging.md).

## <a name="automatic-device-management"></a>Otomatik Cihaz Yönetimi
Azure IOT hub otomatik cihaz Yönetimi döngülerini tamamına büyük cihaz filolarına yönetme yinelenen ve karmaşık görevlerinin birçoğunu otomatik hale getirir. İle otomatik cihaz yönetimi, bir dizi cihazda özelliklerine göre hedef, istenen yapılandırmasını tanımlamak ve IOT Hub'ın kapsama geldikleri her cihazları güncelleştirmek olanak tanır.  Oluşan [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md) ve [IOT Edge otomatik dağıtımlar](../iot-edge/how-to-deploy-monitor.md).

## <a name="automatic-device-configuration"></a>Otomatik cihaz yapılandırması
Çözüm arka ucunuz kullanabilirsiniz [otomatik cihaz yapılandırmaları](iot-hub-auto-device-config.md) bir dizi için istenen özellikler atamak [cihaz ikizlerini](#device-twin) ve sistem ölçümlerini ve özel ölçüm kullanarak rapor durumu. 

## <a name="azure-classic-cli"></a>Azure klasik CLI
[Azure Klasik CLI](../cli-install-nodejs.md) oluşturmak ve Microsoft Azure kaynaklarını yönetmek için bir platformlar arası, açık kaynaklı, kabuk tabanlı komut aracı. CLI'ın bu sürümü yalnızca klasik dağıtımlar için kullanılmalıdır.

## <a name="azure-cli"></a>Azure CLI
[Azure CLI](https://docs.microsoft.com/cli/azure/install-az-cli2) oluşturmak ve Microsoft Azure kaynaklarını yönetmek için bir platformlar arası, açık kaynaklı, kabuk tabanlı komut aracı.


## <a name="azure-iot-device-sdks"></a>Azure IOT cihaz SDK'ları
Vardır _cihaz SDK'ları_ oluşturmanıza olanak sağlayan birden çok dil için kullanılabilir [cihaz uygulamaları](#device-app) bir IOT hub'ı ile etkileşim. IOT hub'ı öğreticiler bu cihaz SDK'ları kullanmayı gösterir. Kaynak kodu ve cihaz SDK'ları hakkında daha fazla bilgi bu Github'da bulabilirsiniz [depo](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-service-sdks"></a>Azure IOT hizmeti SDK'ları
Vardır _hizmet SDK'ları_ oluşturmanıza olanak sağlayan birden çok dil için kullanılabilir [arka uç uygulamaları](#back-end-app) bir IOT hub'ı ile etkileşim. IOT hub'ı öğreticiler bu hizmet SDK'ları kullanmayı gösterir. Kaynak kodu ve hizmet SDK'ları hakkında daha fazla bilgi bu Github'da bulabilirsiniz [depo](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-tools"></a>Azure IoT Araçları
[Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) yardımcı olan bir platformlar arası, açık kaynak Visual Studio Code uzantısı, Azure IOT Hub ve VS code'da cihazları yönetmek olan. Azure IOT araçları ile birlikte IOT geliştiriciler kolaylıkla VS Code'da projeyi IOT geliştirebilir.

## <a name="azure-portal"></a>Azure portal
[Microsoft Azure Portal'da](https://portal.azure.com) , sağlamak ve Azure kaynaklarınızı yönetmek merkezi bir yerdir. İçerik kullanarak düzenler _dikey pencereleri_.

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) cmdlet'lerini Windows PowerShell ile Azure'ı yönetmek için kullanabileceğiniz bir koleksiyonudur. Cmdlet'lerini kullanmak için oluşturma, test edin, dağıtın ve çözümleri ve Hizmetleri Azure platformu aracılığıyla teslim edildi yönetin.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kaynaklarla çözümünüzdeki bir grup olarak çalışmanıza olanak tanır. Dağıtma, güncelleştirme veya kaynakları tek ve eşgüdümlü bir işlemle çözümünüzdeki silin.

## <a name="azure-service-bus"></a>Azure Service Bus
[Service Bus](../service-bus/index.md) Kurumsal Mesajlaşma ile bulut özellikli iletişim ve yardımcı olan geçişli iletişim bağlantı şirket içi çözümler ile bulut sağlar. Service Bus hizmetini kullanan bazı IOT hub'ı öğreticiler olun [kuyrukları](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Azure depolama](../storage/common/storage-introduction.md) bir bulut depolama çözümüdür. Bu, yapılandırılmamış nesne verilerini depolamak için kullanabileceğiniz Blob Depolama hizmetinin içerir. Blob depolama bazı IOT hub'ı öğreticileri kullanın.

## <a name="back-end-app"></a>Arka uç uygulaması
Bağlamında [IOT hub'ı](#iot-hub), bir arka uç uygulaması, bir IOT hub'ında hizmet dönük uç noktalardan biri bağlandığı bir uygulamadır. Örneğin, bir arka uç uygulaması alacağı [CİHAZDAN buluta](#device-to-cloud) yönetmek veya iletileri [kimlik kayıt defteri](#identity-registry). Genellikle, bir arka uç uygulaması bulutta çalışır, ancak birçok öğreticileri arka uca yerel geliştirme makinenizde çalışan konsol uygulamaları uygulamalardır.

## <a name="built-in-endpoints"></a>Yerleşik uç noktaları
Her IOT hub'ı içeren yerleşik bir [uç nokta](iot-hub-devguide-endpoints.md) Event Hub ile uyumlu diğer bir deyişle. Bu uç noktadan CİHAZDAN buluta iletileri okumak için Event Hubs ile çalışan herhangi bir mekanizma kullanabilirsiniz.

## <a name="cloud-gateway"></a>Bulut ağ geçidi
Bir bulut ağ geçidi bağlantısını doğrudan bağlanamıyor cihazlar için etkinleştirir [IOT hub'ı](#iot-hub). Bir bulut ağ geçidi aksine bulutta barındırılan bir [alan ağ geçidi](#field-gateway) çalıştıran yerel cihazlarınıza. Bir bulut ağ geçidi için tipik kullanım örneği, cihazlarınız için protokol çevirisi uygulamaktır.

## <a name="cloud-to-device"></a>Buluttan cihaza
Bağlı bir cihaz IOT hub'ından gönderilen iletileri gösterir. Genellikle, bu iletiler, cihazın eyleme izin isteyin komutlardır. Daha fazla bilgi için [göndermek ve IOT Hub ile ileti alma](iot-hub-devguide-messaging.md).

## <a name="configuration"></a>Yapılandırma
Bağlamında [otomatik cihaz yapılandırma](iot-hub-auto-device-config.md), bir dizi cihazda çiftleri ve ölçümler için rapor durumunu ve ilerlemesini sağlar. istenen yapılandırma IOT hub'ının içinden bir yapılandırma tanımlar.

## <a name="connection-string"></a>Bağlantı dizesi
Bir uç noktaya bağlanmak için gereken bilgileri kapsüllemek için uygulama kodunuzdaki bağlantı dizelerini kullanın. Bir bağlantı dizesi genellikle uç noktası ve güvenlik bilgileri, ancak bağlantı dizesi biçimleri hizmetler arasında farklılık adresini içerir. Bağlantı dizesine IOT Hub hizmetiyle ilişkili iki tür vardır:
- *Cihaz bağlantı dizeleri* cihazların bir IOT hub'ındaki cihaz'e yönelik uç bağlanmasını etkinleştir.
- *IOT hub'ı bağlantı dizeleri* bir IOT hub'ındaki yönelik hizmet uç noktalarına bağlanmak arka uç uygulamaları olanak tanır.

## <a name="custom-endpoints"></a>Özel uç noktalar
Oluşturabileceğiniz özel [uç noktaları](iot-hub-devguide-endpoints.md) tarafından gönderilen iletileri sunmak için bir IOT hub'ındaki bir [yönlendirme kuralı](#routing-rules). Özel uç noktalar, doğrudan bir olay hub'ı, Service Bus kuyruğu veya Service Bus konu bağlanın.

## <a name="custom-gateway"></a>Özel bir ağ geçidi
Bir ağ geçidi bağlantısını doğrudan bağlanamıyor cihazlar için etkinleştirir [IOT hub'ı](#iot-hub). Azure IOT Edge, iletileri ve özel dönüştürmeler edge üzerinde başka bir işleme işlemek için özel mantığı uygulamasına özel bir ağ geçidi oluşturmak için kullanabilirsiniz.

## <a name="data-point-message"></a>Veri noktası iletisi
Bir veri noktasının ileti bir [CİHAZDAN buluta](#device-to-cloud) içeren ileti [telemetri](#telemetry) Rüzgar hızı veya sıcaklık gibi verileri.

## <a name="desired-configuration"></a>İstenen yapılandırma
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), istenen yapılandırma özellikleri ve meta verileri cihazla eşitlenmesi gereken cihaz ikizinde tam kümesini ifade eder.

## <a name="desired-properties"></a>İstenen özellikleri
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), istenen özellikleri ile birlikte cihaz çiftinin alt [bildirilen özellikler](#reported-properties) cihaz yapılandırması veya koşul eşitlenecek. İstenen özellikleri belirtilebilmesi bir [arka uç uygulaması](#back-end-app) ve tarafından gözlemlenen [cihaz uygulaması](#device-app).

## <a name="device-to-cloud"></a>Cihazdan buluta
Bağlı bir CİHAZDAN gönderilen iletileri başvurduğu [IOT hub'ı](#iot-hub). Bu iletiler olabilir [veri noktası](#data-point-message) veya [etkileşimli](#interactive-message) iletileri. Daha fazla bilgi için [göndermek ve IOT Hub ile ileti alma](iot-hub-devguide-messaging.md).

## <a name="device"></a>Cihaz
IOT bağlamında, bir cihaz genellikle veri toplayabilir veya diğer cihazları denetlemek, bir küçük ölçekli, tek başına bilgi işlem cihazıdır. Örneğin, çevre bir izleme cihaz ya da denetleyiciye bir greenhouse kullanmak ve havalandırma sistemleri için bir cihaz olabilir. [Cihaz Kataloğu](https://catalog.azureiotsolutions.com/) ile çalışmak üzere onaylandığını donanım cihazlarının bir listesini sağlar [IOT hub'ı](#iot-hub).

## <a name="device-app"></a>Cihaz uygulaması
Bir cihaz uygulamanın çalıştığı, [cihaz](#device) ve ile iletişimi gerçekleştirir, [IOT hub'ı](#iot-hub). Genellikle, birini [Azure IOT cihaz SDK'ları](#azure-iot-device-sdks) bir cihaz uygulaması uyguladığınızda. Çoğu IOT öğreticiler kullandığınız bir [sanal cihazı](#simulated-device) kolaylık sağlamak için.

## <a name="device-condition"></a>Cihaz durumu
Şu anda kullanımda bağlantı yöntemi gibi cihaz durumu bilgilerini tarafından raporlandığı şekilde gösterir bir [cihaz uygulaması](#device-app). [Cihaz uygulamaları](#device-app) yeteneklerini de bildirebilirsiniz. Cihaz ikizlerini kullanarak koşulu ve özellik bilgilerini sorgulayabilirsiniz.

## <a name="device-data"></a>Cihaz verileri
Cihaz verileri, IOT Hub'ında depolanan cihaz başına veri başvurduğu [kimlik kayıt defteri](#identity-registry). İçeri aktarma ve bu verileri dışarı aktarmak mümkündür.

## <a name="device-explorer"></a>Device Explorer
[Cihaz Gezgini](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) , Windows üzerinde çalışır ve cihazlarınızı yönetmenize olanak sağlayan bir araçtır [kimlik kayıt defteri](#identity-registry). Araç ayrıca gönderin ve cihazlarınıza iletileri alacak.

## <a name="device-identity"></a>Cihaz kimliği
Cihaz kimliği, kayıtlı her cihaza atanan benzersiz tanımlayıcıdır [kimlik kayıt defteri](#identity-registry).

## <a name="device-management"></a>Cihaz yönetimi
Cihaz yönetimi planlama, sağlama, yapılandırma, izleme ve devre dışı bırakma dahil olmak üzere, IOT çözümünüzün cihazların yönetimiyle ilgili tam yaşam döngüsünü kapsar.

## <a name="device-management-patterns"></a>Cihaz yönetimi modelleri
[IOT hub'ı](#iot-hub) yeniden başlatma, fabrika ayarlarına gerçekleştirme ve üretici yazılımı güncelleştirmelerini cihazlarınızda gerçekleştirme dahil olmak üzere ortak cihaz yönetim modellerini sağlar.

## <a name="device-rest-api"></a>Cihaz REST API'si
Kullanabileceğiniz [cihaz REST API'si](https://docs.microsoft.com/rest/api/iothub/device) bir IOT hub'ına CİHAZDAN buluta iletiler göndermek ve almak için bir CİHAZDAN [bulut-cihaz](#cloud-to-device) bir IOT hub'ından iletiler. Genellikle, üst düzey birini kullanmalıdır [cihaz SDK'ları](#azure-iot-device-sdks) IOT hub'ı öğreticilerde gösterildiği gibi.

## <a name="device-provisioning"></a>Cihaz sağlama
Cihaz sağlama, ilk ekleme [cihaz verilerini](#device-data) çözümünüzdeki depolarına. Hub'ınıza bağlanmak yeni bir cihaz etkinleştirmek için bir cihaz kimliği ve anahtarlar IOT Hub'ına eklemelisiniz [kimlik kayıt defteri](#identity-registry). Hazırlama işleminin bir parçası, cihaza özgü diğer çözüm depoları verilerde başlatmak gerekebilir.

## <a name="device-twin"></a>Cihaz çifti
A [cihaz ikizi](iot-hub-devguide-device-twins.md) meta veriler, yapılandırmalar ve koşullar gibi cihaz durumu bilgilerini depolayan JSON belgesidir. [IOT hub'ı](#iot-hub) IOT hub'ına sağlama her cihaz için bir cihaz çifti sürdürür. Cihaz ikizlerini eşitlemek üzere etkinleştirme [cihaz koşullar](#device-condition) ve yapılandırmalar arasında cihaz ve çözüm arka ucu. Belirli cihazlara bulun ve uzun süre çalışan işlemlerinin durumunu sorgulamak için cihaz ikizlerini sorgulayabilirsiniz.

## <a name="direct-method"></a>Doğrudan yöntem
A [doğrudan yöntemini](iot-hub-devguide-direct-methods.md) IOT hub'ınızdaki bir API çağırarak bir cihazda çalıştırılacak bir yöntemi tetiklemek yoludur.

## <a name="endpoint"></a>Uç Nokta
Birden çok IOT hub'ı sunan [uç noktaları](iot-hub-devguide-endpoints.md) uygulamalarınızı, IOT hub'ına bağlanmak etkinleştirin. Gönderme gibi işlemleri gerçekleştirmek cihazların cihaz'e yönelik uç vardır [CİHAZDAN buluta](#device-to-cloud) iletileri ve alma [bulut-cihaz](#cloud-to-device) iletileri. Etkinleştirme Hizmeti'e yönelik yönetim uç vardır [arka uç uygulamaları](#back-end-app) gibi işlemleri gerçekleştirmek için [cihaz kimliği](#device-identity) ve cihaz ikizi yönetimi. Hizmet kullanıma yönelik vardır [yerleşik uç noktaları](#built-in-endpoints) CİHAZDAN buluta iletileri okumak için. Oluşturabileceğiniz [özel uç noktalar](#custom-endpoints) tarafından gönderilen CİHAZDAN buluta iletileri almak için bir [yönlendirme kuralı](#routing-rules).

## <a name="event-hubs-service"></a>Event Hubs hizmeti
[Olay hub'ları](../event-hubs/event-hubs-what-is-event-hubs.md) milyonlarca alabilen bir yüksek düzeyde ölçeklenebilir bir veri alım sistemidir saniyede. Hizmet işleyebilir ve veri uygulamanızın bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki olanak sağlar. IOT Hub hizmeti ile bir karşılaştırması için bkz. [karşılaştırma Azure IOT Hub ve Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Olay Hub'ı ile uyumlu uç nokta
Okunacak [CİHAZDAN buluta](#device-to-cloud) iletileri, IOT hub'ına gönderilen, hub'ınızın bir uç noktaya bağlanmak ve bu iletileri okumak için Event Hub ile uyumlu herhangi bir yöntemi kullanabilirsiniz. Event Hub ile uyumlu yöntemleri içerir kullanarak [Event Hubs SDK'ları](../event-hubs/event-hubs-programming-guide.md) ve [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Alan ağ geçidi
Bir alan ağ geçidi bağlantısını doğrudan bağlanamıyor cihazlar için etkinleştirir [IOT hub'ı](#iot-hub) ve genellikle cihazlarınızla yerel olarak dağıtılır. Daha fazla bilgi için [Azure IOT Hub nedir?](about-iot-hub.md)

## <a name="free-account"></a>Ücretsiz hesap
Oluşturabileceğiniz bir [ücretsiz Azure hesabı](https://azure.microsoft.com/pricing/free-trial/) IOT hub'ı öğreticileri ve IOT Hub hizmeti (ve diğer Azure Hizmetleri) ile denemeler yapın.

## <a name="gateway"></a>Ağ geçidi
Bir ağ geçidi bağlantısını doğrudan bağlanamıyor cihazlar için etkinleştirir [IOT hub'ı](#iot-hub). Ayrıca bkz: [alan ağ geçidi](#field-gateway), [bulut ağ geçidi](#cloud-gateway), ve [özel ağ geçidi](#custom-gateway).

## <a name="identity-registry"></a>Kimlik kayıt defteri
[Kimlik kayıt defteri](iot-hub-devguide-identity-registry.md) ayrı aygıtlar hakkındaki bilgileri depolayan bir IOT hub'ın yerleşik bileşeni bir IOT hub'ına bağlanabilir.

## <a name="interactive-message"></a>Etkileşimli iletisi
Etkileşimli bir ileti bir [bulut-cihaz](#cloud-to-device) çözüm arka ucu hemen bir eylemi tetikleyen ileti. Örneğin, bir cihaz otomatik olarak bir CRM sistemine oturum açması bir hata hakkında bir uyarı gönderebilir.

[!INCLUDE [azure-iot-hub-edge-glossary-includes](../../includes/azure-iot-hub-edge-glossary-includes.md)]

## <a name="iot-hub"></a>IoT Hub
IOT Hub, milyonlarca cihaz arasında güvenilir ve güvenli çift yönlü iletişimler sağlayan tam olarak yönetilen bir Azure hizmeti olduğundan ve bir çözüm arka ucu. Daha fazla bilgi için [Azure IOT Hub nedir?](about-iot-hub.md) Kullanarak, [Azure aboneliği](#subscription), Mesajlaşma iş yükleri IOT işlemek için IOT hub'ları oluşturabilirsiniz.

## <a name="iot-hub-metrics"></a>IOT hub'ı ölçümleri
[IOT hub'ı ölçümleri](iot-hub-metrics.md) IOT hub'ı durumuyla ilgili size, [Azure aboneliği](#subscription). IOT hub'ı ölçümleri, hizmet ve ona bağlı cihazların genel durumunu değerlendirmek etkinleştirin. IOT hub'ı ölçümleri, IOT hub'ınıza neler olduğunu görebilir ve Azure desteğine başvurun gerek kalmadan kök neden sorunları araştırmanıza yardımcı olabilir.

## <a name="iot-hub-query-language"></a>IOT Hub sorgu dili
[IOT Hub sorgu dili](iot-hub-devguide-query-language.md) sorguya sağlayan bir SQL benzeri dili, [](#job) ve cihaz ikizleri.

## <a name="iot-hub-resource-rest-api"></a>IOT hub'ı kaynak REST API
Kullanabileceğiniz [IOT hub'ı kaynak REST API'si](https://docs.microsoft.com/rest/api/iothub/iothubresource) IOT hub'ı yönetmek için [Azure aboneliği](#subscription) oluşturma, güncelleştirme ve hub'ları silme gibi işlemleri gerçekleştirme.

## <a name="iot-solution-accelerators"></a>IoT çözüm hızlandırıcıları
Azure IOT Çözüm Hızlandırıcıları, birden çok Azure hizmeti çözümleriyle birlikte paketleyin. Bu çözümler yaygın IOT senaryolarının uçtan uca uygulamaları ile hızla çalışmaya başlamanızı sağlar. Daha fazla bilgi için [Azure IOT Çözüm Hızlandırıcısı nedir?](../iot-accelerators/about-iot-accelerators.md)

## <a name="the-iot-extension-for-azure-cli"></a>Azure CLI için IOT uzantısı 
[Azure CLI için IOT uzantısı](https://github.com/Azure/azure-iot-cli-extension) bir platformlar arası komut satırı aracıdır. Aracı, cihazlarınızı yönetmenize olanak tanır [kimlik kayıt defteri](#identity-registry), göndermek ve iletileri ve dosyaları cihazlarınızdan alabilir ve, IOT hub işlemlerini izleme.

## <a name="job"></a>İş
Çözüm arka ucunuz kullanabilirsiniz [işleri](iot-hub-devguide-jobs.md) zamanlayın ve IOT hub'ınıza kayıtlı cihazlar üzerinde etkinlikleri izlemek için. Etkinlikler içeren cihaz ikizi güncelleştirme [istenen özellikleri](#desired-properties), güncelleştirme cihaz ikizi [etiketleri](#tags)ve bunları çağırırken [doğrudan yöntemler](#direct-method). [IOT hub'ı](#iot-hub) için de kullanır [için içeri ve dışarı aktarma](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) gelen [kimlik kayıt defteri](#identity-registry).

## <a name="modules"></a>Modüller
Cihaz tarafında, IOT Hub cihazı SDK'ları etkinleştirmek oluşturmanızı [modülleri](iot-hub-devguide-module-twins.md) bağımsız bir IOT Hub bağlantısı her biri açılır burada. Bu işlev Cihazınızda farklı bileşenleri için ayrı ad alanları kullanmanıza olanak sağlar.

Modül kimliği ve modül ikizi aynı özellikleri sağlayan [cihaz kimliği](#device-identity) ve [cihaz ikizi](#device-twin) ancak daha iyi tanecikli. Bu daha iyi tanecikli, işletim sistemi tabanlı cihazları gibi özellikli cihazlarda veya üretici yazılımı cihazları yapılandırma ve koşullar için bu bileşenlerin her birini yalıtmak için birden çok bileşen yönetimi sağlar.

## <a name="module-identity"></a>Modül kimliği
Modül kimliği, bir cihaza ait her bir modüle atanan benzersiz tanımlayıcısıdır. Modül kimliği kayıtlı ayrıca [kimlik kayıt defteri](#identity-registry).

## <a name="module-twin"></a>Modül ikizi
Cihaz ikizi, modül ikizi meta veriler, yapılandırmalar ve koşullar gibi modülü durum bilgilerini depolayan JSON belgesini benzer. IOT Hub, IOT hub'ınızda bir cihaz kimliği altında sağlamanız her bir modül kimliği için bir modül ikizi'ni kalıcıdır. Modül ikizlerini modülü koşullar ve yapılandırmaları modülü ve çözüm arka ucu arasında eşitleme sağlar. Modül ikizlerini belirli modüller bulun ve uzun süre çalışan işlemlerinin durumunu sorgulamak için sorgu oluşturabilirsiniz.

## <a name="mqtt"></a>MQTT
[MQTT](https://mqtt.org/) olduğu bir Mesajlaşma protokolleri [IOT hub'ı](#iot-hub) cihazlarla iletişim için destekler. IOT hub'ın desteklediği Mesajlaşma protokolleri hakkında daha fazla bilgi için bkz. [göndermek ve IOT Hub ile ileti alma](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>İşlemleri izleme
IOT hub'ı [işlem izleme](iot-hub-operations-monitoring.md) gerçek zamanlı IOT hub'ınızdaki işlemlerin durumunu izlemenizi sağlar. [IOT hub'ı](#iot-hub) birkaç işlem kategorisi olayları izler. Olay işleme için bir IOT Hub uç noktası için bir veya daha fazla kategorilerden göndermeye içine seçebilirsiniz. Hatalar için verileri izlemek veya veri modellerini daha karmaşık bir işlem ayarlayın.

## <a name="physical-device"></a>Fiziksel cihaz
Gerçek bir cihaz IOT hub'a bağlanan bir Raspberry Pi gibi bir fiziksel cihazdır. Kolaylık sağlamak için IOT hub'ı öğreticiler çoğunu kullanır [sanal cihazlar](#simulated-device) sağlamak örneklerini yerel makinenizde çalıştırma.

## <a name="primary-and-secondary-keys"></a>Birincil ve ikincil anahtarları
Bir IOT hub'ındaki cihaz dönük ya da hizmet'e yönelik uç noktasına bağlandığında, [bağlantı dizesi](#connection-string) size erişim vermesini isteyin anahtarı içerir. Bir cihaza eklediğinizde [kimlik kayıt defteri](#identity-registry) veya bir [paylaşılan erişim ilkesi](#shared-access-policy) hub'ınıza, hizmet, birincil ve ikincil anahtar oluşturur. İki anahtarın kullanılması, IOT hub'ına erişimi kaybetmeden bir anahtarı güncelleştirme, bir anahtardan diğerine Atla olanak tanır.

## <a name="protocol-gateway"></a>Protokol ağ geçidi
Bir protokol ağ geçidi genellikle buluta dağıtılan ve protokolü, bağlanan cihazlar için çeviri hizmetleri sağlar. [IOT hub'ı](#iot-hub). Daha fazla bilgi için [Azure IOT Hub nedir?](about-iot-hub.md)

## <a name="quotas-and-throttling"></a>Kotalar ve azaltma
Vardır çeşitli [kotalar](iot-hub-devguide-quotas-throttling.md) kullanımınız için geçerli [IOT hub'ı](#iot-hub), kotaların birçoğu IOT hub'ının katmanına göre değişir. [IOT hub'ı](#iot-hub) de geçerlidir [kısıtlar](iot-hub-devguide-quotas-throttling.md) çalışma zamanında hizmetini kullanımınız için.

## <a name="reported-configuration"></a>Bildirilen yapılandırma
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), yapılandırma özellikleri ve meta verileri için çözüm arka ucu bildirilmesi cihaz ikizinde kümesinin tamamını başvurduğu bildirdi.

## <a name="reported-properties"></a>Bildirilen özellikler
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), özellikleri ile birlikte cihaz çiftinin alt bildirilir [istenen özellikleri](#desired-properties) cihaz yapılandırması veya koşul eşitlenecek. Bildirilen özellikler belirtilebilmesi [cihaz uygulaması](#device-app) ve okunabilir ve tarafından sorgulanan bir [arka uç uygulaması](#back-end-app).

## <a name="resource-group"></a>Kaynak grubu
[Azure Resource Manager](#azure-resource-manager) ilgili kaynakları bir araya gruplamak için kaynak gruplarını kullanır. Bir kaynak grubu, tüm kaynak grubunda aynı anda işlemleri için kullanabilirsiniz.

## <a name="retry-policy"></a>Yeniden deneme ilkesi
İşlemek için bir yeniden deneme ilkesini kullanacak [geçici hatalar](/azure/architecture/best-practices/transient-faults) bir bulut hizmetine bağladığınızda.

## <a name="routing-rules"></a>Yönlendirme kuralları
Yapılandırdığınız [yönlendirme kuralları](iot-hub-devguide-messages-read-custom.md) CİHAZDAN buluta iletileri yönlendirmek için IOT hub'ınızda bir [yerleşik uç nokta](#built-in-endpoints) veya [özel uç noktalar](#custom-endpoints) çözüm arka ucunuz tarafından işlenmek .

## <a name="sasl-plain"></a>SASL DÜZ
SASL DÜZ güvenlik belirteçleri aktarmak için AMQP protokolünü kullanan bir protokoldür.

## <a name="service-rest-api"></a>Hizmet REST API'si
Kullanabileceğiniz [hizmeti REST API'si](https://docs.microsoft.com/rest/api/iothub/service) çözüm arka ucu, cihazlarınızı yönetmek için. API almak ve güncelleştirmenize olanak tanır [cihaz ikizi](#device-twin) özelliklerini çağırmak [doğrudan yöntemler](#direct-method)ve zamanlama [işleri](#job). Genellikle, üst düzey birini kullanmalıdır [hizmet SDK'ları](#azure-iot-service-sdks) IOT hub'ı öğreticilerde gösterildiği gibi.

## <a name="shared-access-signature"></a>Paylaşılan erişim imzası
Paylaşılan erişim imzaları (SAS), SHA-256'yı güvenli karmaları veya URI dayalı bir kimlik doğrulama mekanizmasıdır. SAS kimlik doğrulaması iki bileşenden oluşur: bir _paylaşılan erişim ilkesi_ ve _paylaşılan erişim imzası_ (genellikle bir belirteç olarak da adlandırılır). Bir cihaz IOT hub'ı ile kimlik doğrulaması yapmak için SAS kullanır. [Arka uç uygulamaları](#back-end-app) ayrıca bir IOT hub'ında hizmet'e yönelik uç kimlik doğrulaması yapmak için SAS kullanın. Genellikle, SAS belirteci dahil [bağlantı dizesi](#connection-string) bir uygulama bir IOT hub'ına bağlantı kurmak için kullanır.

## <a name="shared-access-policy"></a>Paylaşılan erişim ilkesi
Paylaşılan erişim ilkesinin geçerli olan herkes için verilen izinleri tanımlar [birincil veya ikincil anahtarı](#primary-and-secondary-keys) Bu ilkeyle ilişkilendirilmiş. Hub'ına yönelik paylaşılan erişim ilkeleri ve anahtarları yönetebilir [portalı](#azure-portal).

## <a name="simulated-device"></a>Sanal cihaz
Kolaylık olması için IOT hub'ı öğreticiler birçoğu sağlamak örneklerini yerel makinenizde çalıştırma sanal cihazlar kullanın. Buna karşılık, bir [fiziksel cihaz](#physical-device) gerçek bir cihaz IOT hub'a bağlanan bir Raspberry Pi gibi.

## <a name="solution"></a>Çözüm
A _çözüm_ bir veya daha fazla proje içeren bir Visual Studio çözümü için başvurabilirsiniz. A _çözüm_ cihazları gibi öğeleri içeren bir IOT çözümü için başvurabilir [cihaz uygulamaları](#device-app), IOT hub'ı, diğer Azure Hizmetleri ve [arka uç uygulamaları](#back-end-app).

## <a name="subscription"></a>Abonelik
Azure aboneliğinin faturalandırma gerçekleştiği ' dir. Her Azure kaynağı oluşturduğunuzda veya kullandığınız Azure hizmet tek bir abonelik ile ilişkilidir. Birçok kotaları abonelik düzeyinde de geçerlidir.

## <a name="system-properties"></a>Sistem özellikleri
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), Sistem özellikleri salt okunurdur ve son etkinlik süresi ve bağlantı durumu gibi cihaz kullanımı ile ilgili bilgiler içerir.

## <a name="tags"></a>Etiketler
Bağlamında bir [cihaz ikizi](iot-hub-devguide-device-twins.md), etiketleri, cihaz meta verilerini depolanır ve bir JSON Belgesi biçiminde çözüm arka ucu tarafından alınır. Etiketler, bir cihazdaki uygulamalar için görünür değildir.

## <a name="telemetry"></a>Telemetri
Cihazlar gibi Rüzgar hızı veya sıcaklık, telemetri verilerini toplamak ve bir IOT hub'ına telemetri göndermek için veri noktası iletileri kullanın.

## <a name="token-service"></a>Belirteç Hizmeti
Cihazlarınız için bir kimlik doğrulama mekanizması uygulamak için bir belirteç hizmeti kullanabilirsiniz. IOT hub'ı kullanan [paylaşılan erişim ilkesi](#shared-access-policy) ile **DeviceConnect** oluşturma izni *cihaz kapsamlı* belirteçleri. Bu belirteçler, bir cihaz IOT hub'ınıza bağlanmak etkinleştirin. Bir cihaz, belirteç hizmeti ile kimlik doğrulaması için bir özel kimlik doğrulama mekanizması kullanır. Belirteç Hizmeti, cihazın kimliğini başarıyla doğrulayan, IOT hub'ınıza erişmek için kullanılacak cihaz için bir SAS belirteci verir.

## <a name="twin-queries"></a>Çifti sorguları
[Cihaz ve modül ikizi sorgularını](iot-hub-devguide-query-language.md) cihaz ikizlerini ya da modül ikizlerini bilgileri almak için SQL benzeri IOT Hub sorgu dili kullanın. Aynı IOT Hub sorgu dili hakkında bilgi almak için kullanabileceğiniz [](#job) IOT hub'ına çalışıyor.

## <a name="twin-synchronization"></a>İkiz eşitleme
İkiz eşitleme kullandığı [istenen özellikleri](#desired-properties) cihazlar veya modülleri yapılandırmak ve almak için cihaz ikizlerini veya modül ikizlerini [bildirilen özellikler](#reported-properties) onlardan çiftine depolamak için.

## <a name="x509-client-certificate"></a>X.509 istemci sertifikası
Bir cihaz ile kimlik doğrulaması için bir X.509 sertifikası kullanabilirsiniz [IOT hub'ı](#iot-hub). X.509 sertifikası kullanmaktır kullanmaya alternatif bir [SAS belirteci](#shared-access-signature).
