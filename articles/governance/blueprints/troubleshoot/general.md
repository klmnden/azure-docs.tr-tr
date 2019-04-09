---
title: Sık karşılaşılan hataları giderme
description: Oluşturma, atama ve şemaları kaldırma sorunlarını giderme hakkında bilgi edinin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/11/2018
ms.topic: troubleshooting
ms.service: blueprints
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 42fdd6645a7a0e7cd9a2f0a7bc969e8eee62758c
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59267200"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Kullanarak Azure şemaları hatalarıyla ilgili sorunları giderme

Oluştururken ya da bir blueprint'i atama hatalarla karşılaşırsanız çalıştırabilirsiniz. Bu makalede, oluşabilecek çeşitli hataları ve bunların nasıl çözüleceğine açıklanır.

## <a name="finding-error-details"></a>Bulma hatası ayrıntıları

Birçok hata, bir şema bir kapsama atama sonucunu olacaktır. Atama başarısız olduğunda, şema dağıtımı başarısız hakkında ayrıntılar sağlar. Bu bilgiler, böylece sonraki dağıtım başarılı olduktan ve düzeltilebilir sorunu gösterir.

1. Seçin **tüm hizmetleri** sol bölmesinde. Arayın ve seçin **şemaları**.

1. Seçin **şemaları atanan** blueprint ataması başarısız atamanın bulmak için filtre uygulamak için arama kutusunu kullanın ve sol sayfasında. Tablosu atamaları göre de sıralayabilirsiniz **sağlama durumu** tüm başarısız atamalar görmek için sütun gruplandırıldığı.

1. Sol tıklatma ile şema üzerinde _başarısız_ durumu veya sütuna sağ tıklayıp **atama ayrıntıları görüntüle**.

1. Atama başarısız oldu uyarı kırmızı bir başlık blueprint ataması sayfanın en üstündeki ' dir. Daha fazla bilgi almak için başlık herhangi bir yere tıklayın.

Blueprint bir bütün olarak değil ve bir yapıt tarafından neden olduğu hata yaygındır. Bir yapının bir Key Vault oluşturur ve Azure İlkesi, anahtar kasası oluşturmayı engeller, tüm atama başarısız olur.

## <a name="general-errors"></a>Genel hata

### <a name="policy-violation"></a>Senaryo: İlke ihlali

#### <a name="issue"></a>Sorun

Şablon dağıtımı ilke ihlali nedeniyle başarısız oldu.

#### <a name="cause"></a>Nedeni

Bir ilke için pek çok dağıtımı ile çakışıyor:

- Oluşturulan kaynak (genellikle SKU veya konum kısıtlamaları) ilkesi tarafından kısıtlanıyor
- Dağıtım, ilke (etiketlerle yaygındır) tarafından yapılandırılan alanlar ayarlama

#### <a name="resolution"></a>Çözüm

Hata ayrıntılarında ilkelerle çakışmadığından biçimde şemayı değiştirin. Bu değişiklik mümkün değilse, alternatif bir seçenek şema çakışıyor İlkesi artık, bu nedenle değiştirilen bir ilke ataması kapsamı sağlamaktır.

### <a name="escape-function-parameter"></a>Senaryo: Blueprint parametresi bir işlev değil

#### <a name="issue"></a>Sorun

İşlevleri şema parametreleri yapıtları iletilmeden önce işlenir.

#### <a name="cause"></a>Nedeni

Blueprint parametresi geçirmeden kullanan bir işlev gibi `[resourceGroup().tags.myTag]`, dinamik işlevi yerine yapıt üzerinde ayarlanan işlevi gören sonucunu bir yapıt sonuçlanır.

#### <a name="resolution"></a>Çözüm

Bir işlev aracılığıyla bir parametre olarak geçirmek için tüm dize ile kaçış `[` blueprint parametresi şuna benzer şekilde `[[resourceGroup().tags.myTag]`. Kaçış karakteri değeri bir dize olarak şema işleme sırasında değerlendirilecek şemaları neden olur. Blueprint beklendiği gibi dinamik olarak izin veren yapıt üzerinde ardından işlev yerleştirir. Daha fazla bilgi için [şablon dosya yapısı - söz dizimi](../../../azure-resource-manager/resource-group-authoring-templates.md#syntax).

## <a name="next-steps"></a>Sonraki adımlar

Sorununuzu görmediniz veya sorununuzu çözmenize yüklenemiyor, daha fazla destek için aşağıdaki kanalları birini ziyaret edin:

- Aracılığıyla Azure uzmanlarından cevaplar [Azure forumları](https://azure.microsoft.com/support/forums/).
- [@AzureSupport](https://twitter.com/azuresupport) hesabı ile bağlantı kurun. Bu resmi Microsoft Azure hesabı, müşteri deneyimini geliştirmek için Azure topluluğunu doğru kaynaklara ulaştırır: yanıtlar, destek ve uzmanlar.
- Daha fazla yardıma ihtiyacınız varsa, bir Azure destek olayına dosya. Git [Azure Destek sitesi](https://azure.microsoft.com/support/options/) seçip **alma desteği**.