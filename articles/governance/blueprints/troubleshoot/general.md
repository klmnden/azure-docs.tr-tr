---
title: Kullanarak Azure şemaları hatalarıyla ilgili sorunları giderme
description: Oluşturma ve atama şemaları sorunları gidermeyi öğrenin
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dd1163ece225c2e9a9b082f5e8364f34b06a10ae
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982299"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Kullanarak Azure şemaları hatalarıyla ilgili sorunları giderme

Oluştururken ya da bir blueprint'i atama hatalarla karşılaşabilirsiniz. Bu makalede, oluşabilecek çeşitli hataları ve bunların nasıl çözüleceğine açıklanır.

## <a name="finding-error-details"></a>Bulma hatası ayrıntıları

Birçok hata, bir şema bir kapsama atama sonucunu olacaktır. Atama başarısız olduğunda, şema dağıtımı başarısız hakkında ayrıntılar sağlar. Bu bilgiler sonraki dağıtım başarılı olur ve düzeltilebilir böylece sorunu gösterir.

1. Tıklayarak Azure portalında Azure şemaları hizmet başlatma **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. Üzerinde **ilke** sayfasında, tıklayarak **şemaları**.

1. Seçin **atanan şemalar** blueprint ataması başarısız atamanın bulmak için filtre uygulamak için arama kutusunu kullanın ve sol sayfasında. Tablosu atamaları göre de sıralayabilirsiniz **sağlama durumu** tüm başarısız atamalar görmek için sütun gruplandırıldığı.

1. Sol tıklatma ile şema üzerinde _başarısız_ durumu veya sütuna sağ tıklayıp **atama ayrıntıları**.

1. Blueprint üst kısmında atama uyarı atama başarısız oldu, kırmızı bir başlık sayfasıdır. Daha fazla bilgi almak için başlık herhangi bir yere tıklayın.

Şema ve şema değil bir bütün olarak bulunan bir yapıt tarafından neden olduğu hata yaygındır. Örneğin, blueprint'e yapıt içeriyorsa bir Key Vault, ancak anahtar kasası oluşturma oluşturmak için Azure İlkesi tarafından engellendiğinde, tüm atama başarısız olur.

## <a name="general-errors"></a>Genel hatalar

### <a name="policy-violation"></a>Senaryo: İlke ihlali

#### <a name="issue"></a>Sorun

Şablon dağıtımı ilke ihlali nedeniyle başarısız oldu.

#### <a name="cause"></a>Nedeni

Bir ilke için pek çok dağıtımı ile çakışıyor:

- Oluşturulan kaynak (genellikle SKU veya konum kısıtlamaları) ilkesi tarafından kısıtlanıyor
- Dağıtım, ilke (etiketlerle yaygındır) tarafından yapılandırılan alanlar ayarlama

#### <a name="resolution"></a>Çözüm

Hata bilgileri listelenen ilkeleri çakışıyor olmaması için plan yapın. Bu mümkün değilse, alternatif bir seçenekleri var. şema çakışıyor İlkesi artık, bu nedenle değiştirilen bir ilke ataması kapsamı için

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görülmez veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

- [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
- [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.