---
title: Kullanarak Azure şemaları hatalarıyla ilgili sorunları giderme
description: Oluşturma ve atama şemaları sorunları gidermeyi öğrenin
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b910f90e70af4ce6d4243c06bfe5bd03d25d74d6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50092943"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Kullanarak Azure şemaları hatalarıyla ilgili sorunları giderme

Oluştururken ya da bir blueprint'i atama hatalarla karşılaşırsanız çalıştırabilirsiniz. Bu makalede, oluşabilecek çeşitli hataları ve bunların nasıl çözüleceğine açıklanır.

## <a name="finding-error-details"></a>Bulma hatası ayrıntıları

Birçok hata, bir şema bir kapsama atama sonucunu olacaktır. Atama başarısız olduğunda, şema dağıtımı başarısız hakkında ayrıntılar sağlar. Bu bilgiler, böylece sonraki dağıtım başarılı olduktan ve düzeltilebilir sorunu gösterir.

1. Tıklayarak **tüm hizmetleri** arama ve seçme **ilke** sol bölmesinde. **İlke** sayfasında **Şemalar**’a tıklayın.

1. Seçin **atanan şemalar** blueprint ataması başarısız atamanın bulmak için filtre uygulamak için arama kutusunu kullanın ve sol sayfasında. Tablosu atamaları göre de sıralayabilirsiniz **sağlama durumu** tüm başarısız atamalar görmek için sütun gruplandırıldığı.

1. Sol tıklatma ile şema üzerinde _başarısız_ durumu veya sütuna sağ tıklayıp **atama ayrıntıları**.

1. Atama başarısız oldu uyarı kırmızı bir başlık blueprint ataması sayfanın en üstündeki ' dir. Daha fazla bilgi almak için başlık herhangi bir yere tıklayın.

Blueprint bir bütün olarak değil ve bir yapıt tarafından neden olduğu hata yaygındır. Bir yapının bir Key Vault oluşturur ve Azure İlkesi, anahtar kasası oluşturmayı engeller, tüm atama başarısız olur.

## <a name="general-errors"></a>Genel hatalar

### <a name="policy-violation"></a>Senaryo: İlke ihlali

#### <a name="issue"></a>Sorun

Şablon dağıtımı ilke ihlali nedeniyle başarısız oldu.

#### <a name="cause"></a>Nedeni

Bir ilke için pek çok dağıtımı ile çakışıyor:

- Oluşturulan kaynak (genellikle SKU veya konum kısıtlamaları) ilkesi tarafından kısıtlanıyor
- Dağıtım, ilke (etiketlerle yaygındır) tarafından yapılandırılan alanlar ayarlama

#### <a name="resolution"></a>Çözüm

Hata ayrıntılarında ilkelerle çakışmadığından biçimde şemayı değiştirin. Bu değişiklik mümkün değilse, alternatif bir seçenek şema çakışıyor İlkesi artık, bu nedenle değiştirilen bir ilke ataması kapsamı sağlamaktır.

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

- [Azure Forumları](https://azure.microsoft.com/support/forums/) aracılığıyla Azure uzmanlarından yanıtlar alın
- [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.