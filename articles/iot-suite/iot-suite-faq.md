---
title: Azure IOT paketi ile ilgili SSS | Microsoft Docs
description: "IoT Paketi için sık sorulan sorular"
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: aa8a053e6b3b6fcf6e026f2fd65942a943e12a08
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="frequently-asked-questions-for-iot-suite"></a>IoT Paketi için sık sorulan sorular

Ayrıca bkz.: bağlı Fabrika özgü [SSS](iot-suite-faq-cf.md).

### <a name="where-can-i-find-the-source-code-for-the-preconfigured-solutions"></a>Önceden yapılandırılmış çözümleri için kaynak kodunu nereden bulabilirim?

Kaynak kodu aşağıdaki GitHub depolarının depolanır:

* [Önceden yapılandırılmış Uzaktan izleme çözümü (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Önceden yapılandırılmış Uzaktan izleme çözümü (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Önceden yapılandırılmış Tahmine dayalı bakım çözümü](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Bağlı Fabrika önceden yapılandırılmış çözümü](https://github.com/Azure/azure-iot-connected-factory)

### <a name="how-much-does-it-cost-to-provision-the-new-remote-monitoring-solution"></a>Nasıl, yeni Uzaktan izleme çözümü sağlamak için maliyeti nedir?

Yeni önceden yapılandırılmış çözüm iki dağıtım seçenekleri sunar:

* A *temel* alt geliştirme maliyet veya bir tanıtım veya kavram kanıtı oluşturmak için arayan müşteriler arayan geliştiricileri için tasarlanmış seçeneği.
* A *standart* seçeneği üretime hazır bir altyapı dağıtmayı isteyen kuruluşlar için tasarlanmıştır.

### <a name="how-can-i-ensure-i-keep-my-costs-down-while-i-develop-my-solution"></a>My çözümü geliştirirken ı my maliyetleri düşük tutun nasıl sağlayabilirsiniz?

İki farklı dağıtım sağlamaya ek olarak, yeni Uzaktan izleme çözümü etkinleştirmek veya isteğe bağlı tüm sanal cihazların devre dışı bırakmak için bir ayar vardır. Benzetim devre dışı bırakma, çözüm ve genel maliyeti, bu nedenle, alınan veri azaltır.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-preconfigured-solutions"></a>Yeni mikro mimarisi, tüm üç önceden yapılandırılmış çözümler için kullanılabilir mi?

Şu anda, en geniş senaryo kapsar gibi yalnızca Uzaktan izleme çözümü mikro mimarisi kullanır.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Avantajları yeni açık kaynaklıdır mikro tabanlı mimari yeni güncelleştirme sağlar?

Son iki yıl içinde bulut mimarisi büyük ölçüde gelişmiştir. Mikro hizmetler, ölçek ve esneklik, geliştirme hızını ödün vermeden elde etmek için harika bir düzen olarak çıkmıştır. Bu tasarım örüntüsü mükemmel güvenilirlik ve ölçeklendirilebilirlik sonuçları birkaç Microsoft hizmetlerinde dahili olarak kullanılır. Biz bu uygulamada müşterilerimizin onlardan fayda böylece öğrenme koyarsınız.

### <a name="is-the-new-preconfigured-solution-available-in-the-same-geographic-region-as-the-existing-solution"></a>Yeni önceden yapılandırılmış çözümü varolan bir çözümü ile aynı coğrafi bölgede var mı?

Evet, yeni Uzaktan izleme aynı coğrafi bölgelerde kullanılabilir.

### <a name="what-is-the-difference-between-the-basic-and-standard-deployment-options-how-do-i-decide-between-the-two-deployment-options"></a>Temel ve standart dağıtım seçenekleri arasındaki fark nedir? İki dağıtım seçenekleri arasında nasıl karar?

Her dağıtım seçeneği için farklı gereksinimleri yanıt verir. Temel dağıtımı başlamak ve PoC ve küçük pilotlar geliştirmek üzere tasarlanmıştır. Minimum gerekli kaynakları ve daha düşük maliyetli bir kolaylaştırılmış bir mimari sağlar. Standart dağıtım oluşturmak ve bir üretime hazır çözümü özelleştirmek için tasarlanmıştır ve bir dağıtım, hayata geçirmek için gerekli öğeleri sağlar. Güvenilirlik ve ölçek, uygulama mikro Docker kapsayıcılar olarak oluşturulur ve bir orchestrator (varsayılan olarak Kubernetes) kullanılarak dağıtılabilir. Orchestrator, dağıtım, ölçeklendirme ve uygulama yönetimi için sorumludur. Geçerli gereksinimlerinize göre bir seçenek seçmeniz gerekir. Biri, diğer veya proje aşaması bağlı olarak her ikisinin birleşimini kullanabilirsiniz.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-suite"></a>Azure IOT Paketi varolan my yatırım yüklemenizden yararlanmaya devam edebilirsiniz?

Evet. Bugün mevcut herhangi bir çözümü Azure aboneliğinizde çalışmaya devam eder ve kaynak kodu Github'da kullanılabilir kalır.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-preconfigured-solution-in-azureiotsuitecom"></a>Azure portalında bir kaynak grubunu silmek ile azureiotsuite.com'da önceden yapılandırılmış bir çözüm için silmeye tıklama arasındaki fark nedir?

* Önceden yapılandırılmış çözümü silerseniz [azureiotsuite.com](https://www.azureiotsuite.com/), önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları silin. Kaynak grubuna ek kaynaklar eklediyseniz, bu kaynakları da silinir.
* Kaynak grubunu silerseniz [Azure portal](https://portal.azure.com), yalnızca bu kaynak grubundaki kaynakları silersiniz. Ayrıca önceden yapılandırılmış Çözümle ilişkili Azure Active Directory Uygulama silmeniz gerekir.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak, sağlayabilirsiniz [10 IOT hub'ları abonelik başına](../azure-subscription-service-limits.md#iot-hub-limits). Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bu sınırı artırmak için. Sonuç olarak, önceden yapılandırılmış her çözüm yeni bir IOT hub'ı hazırlar olduğundan, yalnızca belirli bir aboneliğe en fazla 10 önceden yapılandırılmış çözümü sağlayabilirsiniz.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç Azure Cosmos DB örneği sağlayabilirim?

Elli. Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bu sınırı artırmak için ancak varsayılan olarak, abonelik başına 50 Cosmos DB örnekleri yalnızca sağlayabilirsiniz.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir İç İşlemler Düzey 1 planı ile hazırlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-microsoft-azure-for-dreamspark"></a>DreamSpark için Microsoft Azure'a sahipsem önceden yapılandırılmış bir çözüm oluşturabilir miyim?

Şu anda önceden yapılandırılmış bir çözüm oluşturamazsınız bir [DreamSpark için Microsoft Azure](https://www.dreamspark.com/Product/Product.aspx?productid=99) hesabı. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure](https://azure.microsoft.com/free/) sağlayan yalnızca birkaç dakika içinde önceden yapılandırılmış bir çözüm oluşturun.

### <a name="can-i-create-a-preconfigured-solution-if-i-have-cloud-solution-provider-csp-subscription"></a>Önceden yapılandırılmış bir çözüm, bulut çözümü sağlayıcısı (CSP) aboneliğiniz varsa oluşturabilir miyim?

Şu anda bir bulut çözümü sağlayıcısı (CSP) aboneliğiyle önceden yapılandırılmış bir çözüm oluşturamazsınız. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure](https://azure.microsoft.com/free/) sağlayan yalnızca birkaç dakika içinde önceden yapılandırılmış bir çözüm oluşturun.

### <a name="how-do-i-delete-an-aad-tenant"></a>Bir AAD kiracısını nasıl silerim?

Bkz. Eric Golpe'un blog gönderisi [bir Azure AD Kiracısını silme Kılavuzu](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Sonraki adımlar

Önceden yapılandırılmış IoT Suite çözümlerinin diğer özelliklerinden bazılarını da keşfedebilirsiniz:

* [Önceden yapılandırılmış Tahmine dayalı bakım çözümüne genel bakış](iot-suite-predictive-overview.md)
* [Önceden yapılandırılmış bağlı Fabrika çözümüne genel bakış](iot-suite-connected-factory-overview.md)
* [IOT güvenlik sıfırdan](securing-iot-ground-up.md)