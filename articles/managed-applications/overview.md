---
title: Azure yönetilen uygulamalarına genel bakış | Microsoft Docs
description: Azure yönetilen uygulamaları kavramlarını açıklar
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.date: 07/11/2018
ms.author: tomfitz
ms.openlocfilehash: 628a936d85eb94a1ee332205047527b0f9795d50
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38990523"
---
# <a name="azure-managed-applications-overview"></a>Azure yönetilen uygulamalarına genel bakış

Azure yönetilen uygulamaları, tüketicilerin kolayca dağıtıp çalıştırabileceği bulut çözümleri sunmanızı sağlar. Altyapıyı uygular ve sürekli destek sağlarsınız. Yönetilen bir uygulamayı tüm müşterilerin kullanımına sunmak için Azure markette yayımlayın. Uygulamayı yalnızca kuruluşunuzdaki kullanıcıların kullanımına sunmak için bir iç katalogda yayımlayın. 

Yönetilen bir uygulama, tek bir önemli farkla, Market’teki bir çözüm şablonuna benzer. Yönetilen bir uygulamada kaynaklar, uygulamanın yayımcısı tarafından yönetilen bir kaynak grubuna sağlanır. Kaynak grubu, tüketicinin aboneliğinde mevcuttur ancak yayımcının kiracısındaki bir kimlik, kaynak grubuna erişime sahiptir. Yayımcı olarak, çözümün sürekli desteği için maliyeti belirtirsiniz.

## <a name="advantages-of-managed-applications"></a>Yönetilen uygulamaların avantajları

Yönetilen uygulamalar, çözümlerinizi kullanan tüketicilerin önündeki engelleri azaltır. Çözümünüzü kullanmak için bulut altyapısında uzmanlığa gerek yoktur. Tüketiciler, kritik kaynaklara sınırlı erişime sahiptir. Yönetirken hata yapmaktan endişelenmeleri gerekmez. 

Yönetilen uygulamalar, tüketicilerinizle sürekli bir ilişki kurmanızı sağlar. Uygulamayı yönetme koşullarını siz tanımlarsınız ve tüm ücretler Azure faturalama üzerinden işleme alınır.

Müşteriler bu yönetilen uygulamaları aboneliklerinde dağıtsa da, uygulamaların bakım, güncelleştirme ve servisinden sorumlu olmazlar. Tüm müşterilerin onaylanan sürümleri kullandığından emin olabilirsiniz. Müşteriler, bu uygulamaları yönetmek için uygulamaya özgü bir etki alanı geliştirmek zorunda değildir. Müşteriler, uygulamalarla ilgili sorun giderme ve sorunları tanılama konusunda endişelenmeye gerek olmadan uygulama güncelleştirmelerini otomatik olarak alır. 

BT ekipleri için yönetilen uygulamalar, kuruluştaki kullanıcılara önceden onaylanmış çözümler sunmanızı sağlar. Bu çözümlerin kuruluş standartlarıyla uyumlu olduğundan emin olun.

## <a name="types-of-managed-applications"></a>Yönetilen uygulamaların türleri

Yönetilen uygulamanızı harici veya dahili olarak yayımlayabilirsiniz.

![Dahili veya harici olarak yayımlama](./media/overview/manage_app_options.png)

### <a name="service-catalog"></a>Hizmet kataloğu

Hizmet kataloğu, bir kuruluştaki kullanıcılar için onaylı çözümleri içeren bir dahili katalogdur. Bazı kuruluş standartlarına uyum gösterildiğinden emin olmak için, kuruluşlar için çözümler sağlayan kataloğu kullanabilirsiniz. Çalışanlar, kataloğu kullanarak BT bölümleri tarafından önerilen ve onaylanan uygulamaları kolayca keşfedebilir. Kuruluşlarındaki diğer kişilerin kendileriyle paylaştıkları yönetilen uygulamaları görebilirler.

Bir Hizmet Kataloğu yönetilen uygulaması yayımlama hakkında bilgi için bkz. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md).

### <a name="marketplace"></a>Market

Hizmetlerini faturalandırmak isteyen satıcılar, Azure market aracılığıyla bir yönetilen uygulamayı kullanılabilir hale getirebilir. Satıcı bir uygulamayı yayımladıktan sonra, uygulama kuruluşun dışındaki kullanıcıların kullanımına sunulur. Bu yaklaşımla, yönetilen hizmet sağlayıcıları (MSP), bağımsız yazılım satıcıları (ISV) ve sistem tümleştiricileri (SI) tüm Azure müşterilerine çözümlerini sunabilir.

Market'te yönetilen uygulama yayımlama hakkında daha fazla bilgi için [Market uygulaması oluşturma](publish-marketplace-app.md) konusunu inceleyin.

## <a name="resource-groups-for-managed-applications"></a>Yönetilen uygulamalar için kaynak grupları

Genellikle, yönetilen bir uygulamanın kaynakları iki kaynak grubunda yer alır. Tüketici, bir kaynak grubunu yönetirken yayımcı diğer kaynak grubunu yönetir. Yayımcı, yönetilen uygulamayı tanımlarken erişim düzeylerini belirtir. [Veri işlemlerine](../role-based-access-control/role-definitions.md) yönelik erişim kısıtlama şu anda Azure’daki tüm veri sağlayıcıları için desteklenmiyor.

Aşağıdaki görüntüde, yayımcının yönetilen kaynak grubu için sahip rolünü istediği bir senaryo gösterilmektedir. Yayımcı, tüketici için bu kaynak grubuna salt okunur bir kilit yerleştirmiştir. Yönetilen kaynak grubuna erişim izni verilen yayımcı kimlikleri kilitli olmaz.

![Kaynak grubu erişimi](./media/overview/access.png)

### <a name="application-resource-group"></a>Uygulama kaynak grubu

Bu kaynak grubu, yönetilen uygulama örneğini tutar. Bu kaynak grubu yalnızca bir kaynak içerebilir. Yönetilen uygulamanın kaynak türü **Microsoft.Solutions/applications** şeklindedir.

Tüketici, kaynak grubuna tam erişime sahiptir ve yönetilen uygulamanın yaşam döngüsünü yönetmek için bu erişimi kullanır.

### <a name="managed-resource-group"></a>Yönetilen kaynak grubu

Bu kaynak grubu, yönetilen uygulamanın gerektirdiği tüm kaynakları tutar. Örneğin, bu kaynak grubu, çözümün sanal makinelerini, depolama hesaplarını ve sanal ağlarını içerir. Tüketici, yönetilen uygulamanın kaynaklarını tek tek yönetmediği için bu kaynak grubuna sınırlı erişime sahiptir. Yayımcının bu kaynak grubuna erişimi, yönetilen uygulama tanımında belirtilen role karşılık gelir. Örneğin, yayımcı bu kaynak grubu için Sahip veya Katkıda Bulunan rolünü isteyebilir.

Tüketici yönetilen uygulamayı sildiğinde, yönetilen kaynak grubu da silinir.

## <a name="azure-policy"></a>Azure İlkesi

Yönetilen uygulamanıza [Azure İlkesi](../azure-policy/azure-policy-introduction.md) uygulayabilirsiniz. Yönetilen uygulamanızın dağıtılan örneklerinin veri ve güvenlik gereksinimlerine uyduğundan emin olmak için ilkeleri uygularsınız. Uygulamanız hassas verilerle etkileşime geçiyorsa, bu etkileşimin nasıl koruma altına alınacağını değerlendirdiğinizden emin olun. Örneğin uygulamanız Office 365’teki verilerle etkileşimli çalışıyorsa veri şifrelemenin etkin olduğunu doğrulayan bir ilke uygulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulama tanımlama ve dağıtmaya giriş için bkz. [Azure CLI ile Azure yönetilen uygulaması oluşturma ve dağıtma](managed-apps-quickstart-cli.md)
* Bir dahili uygulama yayımlama hakkında bilgi için bkz. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md).
* Markette yönetilen uygulamaları yayımlama hakkında daha fazla bilgi için [Market uygulaması oluşturma](publish-marketplace-app.md) konusunu inceleyin.
