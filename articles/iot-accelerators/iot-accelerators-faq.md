---
title: Azure IOT Çözüm Hızlandırıcıları SSS | Microsoft Docs
description: IOT Çözüm Hızlandırıcıları için sık sorulan sorular
services: iot-suite
suite: iot-suite
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''
ms.assetid: cb537749-a8a1-4e53-b3bf-f1b64a38188a
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2018
ms.author: dobett
ms.openlocfilehash: 6c8c87dc4f7bb91ce227bbfbaf1cabf0c44d2e0b
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="frequently-asked-questions-for-iot-solution-accelerators"></a>IOT Çözüm Hızlandırıcıları için sık sorulan sorular

Ayrıca bkz [bağlı Fabrika özgü SSS](iot-accelerators-faq-cf.md) ve [Uzaktan izleme özgü SSS](iot-accelerators-faq-rm-v2.md) .

### <a name="where-can-i-find-the-source-code-for-the-solution-accelerators"></a>Çözüm Hızlandırıcıları için kaynak kodunu nereden bulabilirim?

Kaynak kodu aşağıdaki GitHub depolarının depolanır:

* [Uzaktan izleme Çözüm Hızlandırıcısı (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Uzaktan izleme Çözüm Hızlandırıcısı (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)
* [Tahmine dayalı bakım Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-predictive-maintenance)
* [Bağlı Fabrika Çözüm Hızlandırıcısı](https://github.com/Azure/azure-iot-connected-factory)

### <a name="what-sdks-can-i-use-to-develop-device-clients-for-the-solution-accelerators"></a>Aygıt istemcileri için Çözüm Hızlandırıcıları geliştirmek için hangi SDK'ları kullanabilir miyim?

Farklı dil (C, .NET, Java, Node.js, Python) IOT cihaz SDK'ları bağlantılar bulabilirsiniz içinde [Microsoft Azure IOT SDK'ları](https://github.com/Azure/azure-iot-sdks) GitHub depolarının.

DevKit cihaz kullanıyorsanız, kaynakları ve örnekleri bulabilirsiniz [IOT DevKit SDK'sı](https://github.com/Microsoft/devkit-sdk) GitHub depo.

### <a name="is-the-new-microservices-architecture-available-for-all-the-three-solution-accelerators"></a>Yeni mikro mimarisi için tüm üç Çözüm Hızlandırıcıları var mı?

Şu anda, en geniş senaryo kapsar gibi yalnızca Uzaktan izleme çözümü mikro mimarisi kullanır.

### <a name="what-advantages-does-the-new-open-sourced-microservices-based-architecture-provide-in-the-new-update"></a>Avantajları yeni açık kaynaklıdır mikro tabanlı mimari yeni güncelleştirme sağlar?

Son iki yıl içinde bulut mimarisi büyük ölçüde gelişmiştir. Mikro ölçek ve esneklik, geliştirme hızını ödün vermeden elde etmek için harika bir düzen olarak çıkmıştır. Bu tasarım örüntüsü mükemmel güvenilirlik ve ölçeklendirilebilirlik sonuçları birkaç Microsoft hizmetlerinde dahili olarak kullanılır. Biz bu uygulamada müşterilerimizin onlardan fayda böylece öğrenme koyarsınız.

### <a name="is-the-new-solution-accelerator-available-in-the-same-geographic-region-as-the-existing-solution"></a>Yeni Çözüm Hızlandırıcısı varolan bir çözümü ile aynı coğrafi bölgede var mı?

Evet, yeni Uzaktan izleme aynı coğrafi bölgelerde kullanılabilir.

### <a name="whats-the-difference-between-deleting-a-resource-group-in-the-azure-portal-and-clicking-delete-on-a-solution-accelerator-in-azureiotsuitecom"></a>Azure portalında bir kaynak grubunu silme ve Çözüm Hızlandırıcısı azureiotsuite.com içinde silin tıklayarak arasındaki fark nedir?

* Çözüm Hızlandırıcısı silerseniz [azureiotsuite.com](https://www.azureiotsuite.com/), Çözüm Hızlandırıcısı oluşturduğunuzda sağlanan tüm kaynakları silin. Kaynak grubuna ek kaynaklar eklediyseniz, bu kaynakları da silinir.
* Kaynak grubunu silerseniz [Azure portal](https://portal.azure.com), yalnızca bu kaynak grubundaki kaynakları silersiniz. Ayrıca, Çözüm Hızlandırıcısı ile ilişkili Azure Active Directory Uygulama silmeniz gerekir.

### <a name="can-i-continue-to-leverage-my-existing-investments-in-azure-iot-solution-accelerators"></a>Azure IOT Çözüm Hızlandırıcıları varolan my yatırım yüklemenizden yararlanmaya devam edebilirsiniz?

Evet. Bugün mevcut herhangi bir çözümü Azure aboneliğinizde çalışmaya devam eder ve kaynak kodu Github'da kullanılabilir kalır.

### <a name="how-many-iot-hub-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç IoT Hub örneği sağlayabilirim?

Varsayılan olarak, sağlayabilirsiniz [10 IOT hub'ları abonelik başına](../azure-subscription-service-limits.md#iot-hub-limits). Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bu sınırı artırmak için. Sonuç olarak, her Çözüm Hızlandırıcısı hükümleri bu yana yeni bir IOT Hub yalnızca herhangi bir abonelikte en fazla 10 Çözüm Hızlandırıcıları sağlayabilirsiniz.

### <a name="how-many-azure-cosmos-db-instances-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç Azure Cosmos DB örneği sağlayabilirim?

Elli. Oluşturabileceğiniz bir [Azure destek bileti](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bu sınırı artırmak için ancak varsayılan olarak, abonelik başına 50 Cosmos DB örnekleri yalnızca sağlayabilirsiniz.

### <a name="how-many-free-bing-maps-apis-can-i-provision-in-a-subscription"></a>Bir abonelikte kaç tane Ücretsiz Bing Haritaları API'si sağlayabilirim?

İki. Bir Azure aboneliğinde Enterprise planları için yalnızca iki adet İç İşlemler Düzey 1 Bing Haritası oluşturabilirsiniz. Uzaktan izleme çözümü, varsayılan olarak bir İç İşlemler Düzey 1 planı ile hazırlanır. Sonuç olarak, herhangi bir değişiklik yapılmadıysa bir abonelikte yalnızca en fazla iki tane uzaktan izleme çözümü sağlayabilirsiniz.

### <a name="can-i-create-a-solution-accelerator-if-i-have-microsoft-azure-for-dreamspark"></a>Çözüm Hızlandırıcısı, Microsoft Azure için DreamSpark varsa oluşturabilir miyim?

> [!NOTE]
> DreamSpark için Microsoft Azure artık Öğrenciler için Microsoft Imagine bilinir.

Çözüm Hızlandırıcısı ile şu anda oluşturulamıyor bir [DreamSpark için Microsoft Azure](https://azure.microsoft.com/pricing/member-offers/imagine/) hesabı. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure](https://azure.microsoft.com/free/) sağlayan yalnızca birkaç dakika içinde bir çözüm Hızlandırıcısı oluşturun.

### <a name="can-i-create-a-solution-accelerator-if-i-have-cloud-solution-provider-csp-subscription"></a>Çözüm Hızlandırıcısı, bulut çözümü sağlayıcısı (CSP) aboneliğiniz varsa oluşturabilir miyim?

Şu anda bir bulut çözümü sağlayıcısı (CSP) aboneliğiyle Çözüm Hızlandırıcısı oluşturulamıyor. Ancak, oluşturabileceğiniz bir [ücretsiz deneme hesabı için Azure](https://azure.microsoft.com/free/) sağlayan yalnızca birkaç dakika içinde bir çözüm Hızlandırıcısı oluşturun.

### <a name="how-do-i-delete-an-aad-tenant"></a>Bir AAD kiracısını nasıl silerim?

Bkz. Eric Golpe'un blog gönderisi [bir Azure AD Kiracısını silme Kılavuzu](http://blogs.msdn.com/b/ericgolpe/archive/2015/04/30/walkthrough-of-deleting-an-azure-ad-tenant.aspx).

### <a name="next-steps"></a>Sonraki adımlar

IoT çözüm hızlandırıcılarının diğer özellik ve yeteneklerinden bazılarını da keşfedebilirsiniz:

* [Uzaktan izleme Çözüm Hızlandırıcısı özelliklerini keşfedin](iot-accelerators-remote-monitoring-explore.md)
* [Tahmine Dayalı Bakım çözüm hızlandırıcısına genel bakış](../iot-suite/iot-suite-predictive-overview.md)
* [Bağlı Fabrika Çözüm Hızlandırıcısı genel bakış](iot-accelerators-connected-factory-overview.md)
* [IOT güvenlik sıfırdan](../iot-suite/securing-iot-ground-up.md)
