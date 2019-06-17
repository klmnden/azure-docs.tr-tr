---
title: Azure Digital Twins'e genel bakış | Microsoft Docs
description: Uzamsal zekaya yönelik bir Azuer IoT çözümü olan Azure Digital Twins hakkında daha fazla bilgi edinin.
author: julieseto
ms.author: jseto
ms.date: 12/14/2018
ms.topic: overview
ms.service: digital-twins
services: digital-twins
manager: bertvanhoof
ms.custom: mvc
ms.openlocfilehash: 41a6b040c04c3a212a7ee89897b29f5ec96048d7
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072187"
---
# <a name="overview-of-azure-digital-twins"></a>Azure Digital Twins'e genel bakış

Azure Digital Twins, fiziksel ortamların kapsamlı modellerini oluşturan bir Azure IoT hizmetidir. Bu kişiler, boşluk ve cihazlar arasındaki etkileşimler ve ilişkileri modellemek için uzamsal zeka grafikler oluşturabilirsiniz.

Azure ile dijital İkizlerini, fiziksel bir boşluk yerine birçok farklı sensörlerden alınan verileri sorgulayabilirsiniz. Bu hizmet dijital ve fiziksel dünya genelindeki akış veri bağlantısı yeniden kullanılabilir, ölçeklenebilir, mekan kullanan deneyimleri oluşturmanıza yardımcı olur. Uygulamalarınızı bu benzersiz olarak ilgili bağlamsal özellikler tarafından geliştirilmiştir. Azure dijital çiftleri için aşağıdaki örnek görevler için kullanılabilir:

- İçin bir Fabrika bakım ihtiyaçlarını tahmin edin.
- Bir Elektrik şebekesi için gerçek zamanlı enerji gereksinimleri analiz edin.
- Office için kullanılabilir alan kullanımı iyileştirin.

Azure dijital İkizlerini ortamları tüm türleri için geçerlidir. Ambar, ofisleri, okullar, hastaneler ve bankalar bunun yalnızca birkaç örnektir. Hatta kurucular, fabrikaları, park, parka, Akıllı Kılavuzlar ve şehir için kullanılabilir. Azure dijital çiftleri için aşağıdaki örnek senaryolarda kullanılabilir:

- Günlük sıcaklık arasında çeşitli durumları izler.
- Meşgul bir insansız hava aracı yolları izleyin.
- Otonom taşıtlardan belirleyin.
- Bir yapı için sahiplik düzeyleri analiz edin.
- En yoğun saatinde Yazar Kasa deponuzda bulun.

Gerçek iş senaryonuza olarak ne olursa olsun, büyük olasılıkla Azure ile dijital İkizlerini karşılık gelen bir dijital örneği sağlanabilir.

Aşağıdaki video Azure dijital İkizlerini ayrıntılı olarak ele alır.

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Temel işlevler

Azure dijital İkizlerini aşağıdaki temel özellikleri vardır.

### <a name="spatial-intelligence-graph"></a>Uzamsal zeka grafı

[ *Uzamsal zeka grafik*](./concepts-objectmodel-spatialgraph.md), veya *uzamsal graf*, fiziksel ortam sanal bir gösterimidir. Kişiler, yerler ve cihazlar arasındaki ilişkileri modellemek için kullanabilirsiniz.

Bir Komşuları bağlı birkaç elektrik kullanım ölçümleri içeren bir akıllı yardımcı uygulama göz önünde bulundurun. Akıllı yardımcı şirket gerekir doğru bir şekilde izleyin ve elektrik kullanımını tahmin edin ve faturalandırma. Her bir cihaz ve algılayıcıyı konumu ve faturalandırılmaya müşteri hakkında bağlam ile modellenmesi gerekir. Bu tür karmaşık ilişkileri modellemek için uzamsal zeka Grafiği'ni kullanabilirsiniz.

### <a name="digital-twin-object-models"></a>Dijital ikiz nesne modelleri

[Dijital ikizini nesne modellerini](./concepts-objectmodel-spatialgraph.md) önceden tanımlanmış cihaz protokolleri ve veri şeması. Bunlar, geliştirme işlemini basitleştirin ve hızlandırın, çözümünüzün gereksinimlerini karşılayacak etki alanına özgü hizalayın.

Bir oda doluluk uygulama kampüs, bina, kat ve yer gibi önceden tanımlanmış alan türlerini kullanabilir bir örnektir.

### <a name="multiple-and-nested-tenants"></a>Birden çok ve iç içe yerleştirilmiş kiracılar

Birden çok Kiracı için yeniden kullanılabilir ve güvenli bir şekilde ölçeklendirme çözümleri oluşturabilirsiniz. Erişilen ve yalıtılmış ve güvenli bir şekilde kullanılan birden fazla subtenants de oluşturabilirsiniz.

Örnek, bir kiracının verileri tek bir yapı içinde diğer kiracıların verilerinden yalıtmak için yapılandırılmış bir alanı kullanımı uygulamadır. Veya uygulama verileri için tek bir kiracı ile çok sayıda binalar birleştirmek için kullanılır.

### <a name="advanced-compute-capabilities"></a>Gelişmiş işlem özellikleri

İle [kullanıcı tanımlı işlevleri](./concepts-user-defined-functions.md), tanımlamak ve gelen özel işlevleri çalıştırma [cihaz verilerini](./concepts-device-ingress.md) önceden tanımlı uç nokta için sinyaller göndermek için. Bu özellik Gelişmiş özelleştirme ve cihaz görevlerin Otomasyonu artırır.

Örneği Toprak nem sensör okumaları ve hava durumu tahminini değerlendirmek için bir kullanıcı tanımlı işlev içeren bir akıllı Tarım uygulamasıdır. Uygulama, ardından sinyalleri Sulama ihtiyaçları hakkında gönderir.

### <a name="built-in-access-control"></a>Yerleşik erişim denetimi

Erişim ve kimlik yönetimi özellikleri gibi kullanarak [rol tabanlı erişim denetimi](./security-role-based-access-control.md) ve [Azure Active Directory](./security-authenticating-apis.md), güvenli bir şekilde kişiler ve cihazlar için erişimi denetleyebilirsiniz.

Örnek belirtilen bir aralıktaki sıcaklık ayarlamak bir yer occupants izin verecek şekilde yapılandırılmış bir tesis Yönetimi uygulamasıdır. Tesis yöneticileri sıcaklık her yerde herhangi bir değere ayarlamak için izin verilir.

### <a name="ecosystem"></a>Ekosistem

Azure dijital İkizlerini örneği çok sayıda güçlü Azure hizmetlerine bağlanabilirsiniz. Bu hizmetler, Azure Stream Analytics, Azure yapay ZEKA ve Azure Storage içerir. Ayrıca Azure haritalar, Microsoft karma gerçeklik, Dynamics 365 veya Office 365 içerirler.

Bir akıllı office takımlar ve birçok Katlar üzerinde bulunan aygıtları göstermek için dijital İkizlerini Azure kullanan uygulama oluşturmaya bir örnektir. Stream Analytics, cihazları sağlanan dijital ikizini örneğine canlı veri akışı olarak anahtar eyleme dönüştürülebilir Öngörüler sağlamak için bu verileri işler. Veriler Azure Depolama'da depolanan ve paylaşılabilir dosya biçimine dönüştürülür. Dosya, Office 365'i kullanarak tüm kuruluş genelinde dağıtılır.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Azure Digital Twins'den yararlanan çözümler

Azure dijital İkizlerini Fiziksel dünyadaki ve birçok ilişkilerini temsil etmek için kullanışlıdır. IOT modelleme, veri işleme, olay işleme ve izleme aygıtın kolaylaştırır. Aşağıdaki senaryolardan birkaçı çeşitli sektörlerde göz önünde bulundurun. Bunlar hizmetin kullanımı için yararlanın:

* Emlak yönetim şirketi, ofis binasının yapılandırmak için en iyi yöntemleri hakkında Öngörüler elde zamanla boşlukla doluluk düzeylerini gösterir.
* Bir mobil uygulama için tetikleyici iş sırası bilet. Güvenlik koruyucuları ve zamanlama temizlik ve diğer hizmetlerde perakende boşluk veya spor mekan gönderileceği kullanın.
* Bir yapı occupant hangi odaları gerçek zamanlı bir binada kapladığı gösterir. Ardından, kendi ihtiyaçlarına uygun çalışma alanları rezerve occupant yardımcı olur.
* Varlıklar bir alanı içinde bulunduğu yere izleyin.
* Kullanıcı tercihleri ve enerji şebekesi kısıtlamaları modelleme tarafından ücretlendirme elektrik vehicle iyileştirin.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Azure dijital İkizlerini bağlamında, diğer IOT Hizmetleri

Azure Digital Twins, verileri fiziksel dünyayla eşitlemek için Azure IoT Hub'ı kullanarak IoT cihazlarına ve sensörlerine bağlanır. Aşağıdaki diyagramda Azure dijital çiftleri için diğer Azure IOT Hizmetleri ilişkisini gösterir.

![Azure Digital Twins, Azure IoT Hub üzerinde geliştirilmiş olan bir hizmettir][1]

IOT hakkında daha fazla bilgi için bkz: [Azure IOT teknolojilerini ve çözümlerini](https://docs.microsoft.com/azure/iot-fundamentals/iot-services-and-technologies).

## <a name="next-steps"></a>Sonraki adımlar

Azure dijital İkizleri hakkında kısa bir tanıtım gidin:

>[!div class="nextstepaction"]
>[Hızlı Başlangıç: Azure dijital İkizlerini kullanarak kullanılabilir odaları Bul](./quickstart-view-occupancy-dotnet.md)

Azure dijital İkizlerini kullanarak yakından tesis yönetimi uygulaması bakın:

>[!div class="nextstepaction"]
>[Öğretici: Azure dijital İkizlerini dağıtma ve uzamsal graph'ı yapılandırma](./tutorial-facilities-setup.md)

Temel Azure Digital Twins kavramları hakkında bilgi edinin:

>[!div class="nextstepaction"]
>[Dijital İkizlerini nesne modeli ve uzamsal zeka graf anlama](./concepts-objectmodel-spatialgraph.md)

<!-- Images -->
[1]: media/overview/azure-digital-twins-in-iot-ecosystem.png