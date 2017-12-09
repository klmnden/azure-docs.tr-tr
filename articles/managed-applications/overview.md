---
title: "Azure genel bakış yönetilen uygulamalar | Microsoft Docs"
description: "Azure için kavramlar açıklanmaktadır yönetilen uygulamalar"
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 10/26/2017
ms.author: tomfitz
ms.openlocfilehash: 7f0f18062bc426508ec98b190fe0b73e41e88aa2
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="azure-managed-applications-overview"></a>Azure yönetilen uygulamalara genel bakış

Azure yönetilen uygulamaları dağıtmak ve çalıştırmak Tüketiciler için kolay bulut çözümleri sunmak etkinleştirir. Altyapısını uygulayın ve devam eden desteği sağlar. Yönetilen bir uygulamanın tüm müşterilerin kullanımına için Azure Marketi'nde yayımlama. Yalnızca kuruluşunuzdaki kullanıcılar için kullanılabilmesi için bir iç kataloğa yayımlayın. 

Yönetilen bir uygulama, bir çözüm şablonu Market'te temel farklardan biri ile benzerdir. Yönetilen bir uygulamada uygulama yayımcı tarafından yönetilen bir kaynak grubu için kaynaklar sağlanır. Kaynak grubu tüketicinin abonelikte var, ancak kaynak grubuna erişim publisher'ın Kiracı içinde bir kimliğe sahip. Yayımcı olarak devam eden desteği çözümün maliyetini belirtin.

## <a name="advantages-of-managed-applications"></a>Yönetilen uygulamaların avantajları

Yönetilen uygulamaların çözümlerinizi kullanarak tüketicilere engelleri azaltın. Bulut altyapısı çözümünüzü kullanmaya uzmanlık gerek yoktur. Tüketiciler, kritik kaynaklara erişimi sınırlı. Bunu yönetirken hata yapma hakkında endişelenmeniz gerekmez. 

Yönetilen uygulamaların tüketicileriniz ile devam eden bir ilişki kurmak etkinleştirin. Tüm ücretleri aracılığıyla Azure faturalama işlenir ve uygulama yönetmek için koşulları tanımlayın.

Müşteriler kendi Aboneliklerde yönetilen bu uygulamaları dağıtmak karşın, bunların korumak, güncelleştirmeniz ya da bunları hizmet gerekmez. Tüm müşteriler onaylanan sürümleri kullanıyorsanız emin olabilirsiniz. Müşteriler, bu uygulamaları yönetmek için uygulamaya özgü etki alanı bilgi geliştirmek gerekmez. Müşteriler, otomatik olarak sorun giderme ve uygulamalar ile ilgili sorunları tanılama hakkında endişelenmeye gerek kalmadan uygulama güncelleştirmeleri alın. 

BT ekipleri için yönetilen uygulamalar, kullanıcılara kuruluştaki önceden onaylanmış çözümler sunmak etkinleştirin. Bu çözümlerin kuruluş standartlarıyla uyumlu olduğundan emin olun.

## <a name="types-of-managed-applications"></a>Yönetilen uygulama türleri

Yönetilen uygulamanızın harici veya dahili olarak yayımlayabilirsiniz.

![Dahili olarak veya harici olarak Yayımla](./media/overview/manage_app_options.png)

### <a name="service-catalog"></a>Hizmet Kataloğu

Hizmet Kataloğu, bir kuruluştaki kullanıcılar için onaylanan çözümlerinin iç bir kataloğudur. Katalog bunların çözümleri kuruluşlar için sağlama sırasında belirli kuruluş standartlarıyla uyumluluğu sağlamak için kullanın. Çalışanlar katalog zengin önerilen ve kendi BT departmanları tarafından onaylanan uygulamaların kolayca bulmak için kullanır. Kendi kuruluşunuzdaki diğer kişilerin paylaşın yönetilen uygulamaların görürler.

Hizmet Kataloğu yönetilen uygulama yayımlama hakkında daha fazla bilgi için bkz: [hizmet Kataloğu uygulaması oluştur](publish-service-catalog-app.md).

### <a name="marketplace"></a>Market

Satıcılar hizmetlerini faturalandırmak isteyen bir yönetilen uygulamayı Azure Market üzerinden sunabilirsiniz. Uygulamanın satıcısına yayımlar sonra kuruluş dışındaki kullanıcılar için kullanılabilir. Bu yaklaşım, yönetilen hizmet sağlayıcıları (MSP'ler), bağımsız yazılım satıcılarının (ISV'ler) ve sistem tümleştiricileri (SIS) çözümleri, tüm Azure müşterilerine sunabilir.

## <a name="resource-groups-for-managed-applications"></a>Yönetilen uygulamalar için kaynak grupları

Genellikle, bir yönetilen uygulama için Kaynaklar iki kaynak grubu içinde bulunur. Bir kaynak grubu tüketici yönetir ve bir kaynak grubu yayımcı yönetir. Yönetilen uygulama tanımlarken, yayımcı erişim düzeylerini belirler. Aşağıdaki resimde, burada yayımcı yönetilen kaynak grubu için sahip rolünü istekleri bir senaryo gösterilmektedir. Yayımcı salt okunur kilit tüketici için bu kaynak grubunda yerleştirilir.

![Kaynak Grup erişimi](./media/overview/access.png)

### <a name="application-resource-group"></a>Uygulama kaynak grubu

Bu kaynak grubu yönetilen uygulama örneği bulundurur. Bu kaynak grubu yalnızca bir kaynak içeriyor olabilir. Yönetilen uygulama kaynak türü **Microsoft.Solutions/applications**.

Tüketici kaynak grubuna tam erişimi vardır ve yönetilen uygulama yaşam döngüsü yönetmek için kullanır.

### <a name="managed-resource-group"></a>Yönetilen kaynak grubu

Bu kaynak grubu tarafından yönetilen uygulama gerekli tüm kaynakları tutar. Örneğin, bu kaynak grubu, sanal makineler, depolama hesapları ve çözüm için sanal ağları içeriyor. Tüketici yönetilen uygulama için ayrı ayrı kaynaklar yönetmediğinden tüketici bu kaynak grubuna erişim sınırlıdır. Bu kaynak grubuna erişim publisher'ın yönetilen uygulamayı tanımında belirtilen rol karşılık gelir. Örneğin, yayımcı, bu kaynak grubunun sahibi veya katkıda bulunan rolü isteyebilir.

Tüketici yönetilen uygulamayı sildiğinde, yönetilen kaynak grubunu da silinir.

## <a name="next-steps"></a>Sonraki adımlar

* Giriş için tanımlama ve yönetilen bir uygulamayı dağıtmak için bkz: [oluşturma ve Azure dağıtma yönetilen Azure CLI ile uygulama](managed-apps-quickstart-cli.md)
* Bir iç uygulama yayımlama hakkında daha fazla bilgi için bkz: [hizmet Kataloğu uygulaması oluştur](publish-service-catalog-app.md).

