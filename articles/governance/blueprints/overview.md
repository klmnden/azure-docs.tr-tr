---
title: Azure Blueprints’e genel bakış
description: Azure Blueprints, Azure ortamınızda yapıt oluşturmak, tanımlamak ve dağıtmak için kullanılan bir Azure hizmetidir.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 12/05/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: f1ebbc10109563b771c5417a0449efec12138526
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52967700"
---
# <a name="what-is-azure-blueprints"></a>Azure Blueprints nedir?

Mühendislerin veya mimarların projenin ana hatlarını oluşturmak için kullandıkları şemalar gibi Azure Blueprints de bulut mimarlarının ve merkezi bilgi teknolojisi gruplarının bir kuruluşun standartlarına, desenlerine ve gereksinimlerine uygun Azure kaynaklarından oluşan tekrarlanabilir bir küme tanımlamasını sağlar. Azure Blueprints geliştirme ekiplerinin yeni ortamları hızlı bir şekilde oluşturup kullanıma almalarını ve bunu yaparken kurumsal uyumluluk çerçevesinde olduklarından ve ağ iletişimi gibi yerleşik bileşenlere sahip olduklarından emin olmalarını mümkün hale getirir.

Blueprints, aşağıdakiler gibi çeşitli kaynak şablonlarını ve diğer yapıtları dağıtma sürecini yönetmenin bildirim temelli bir yoludur:

- Rol Atamaları
- İlke Atamaları
- Azure Resource Manager şablonları
- Kaynak Grupları

Azure Blueprints hizmeti, genel olarak dağıtılmış [Azure Cosmos DB](../../cosmos-db/introduction.md) tarafından desteklenir.
Şema nesneleri birden çok Azure bölgesinde çoğaltılır. Bu çoğaltma Blueprints’in kaynaklarınızı dağıttığı bölgeden bağımsız olarak tüm şema nesnelerinizde düşük gecikme süresi, yüksek kullanılabilirlik ve tutarlılık sağlar.

## <a name="how-its-different-from-resource-manager-templates"></a>Resource Manager şablonlarından farkı

Bu hizmet, _ortam kurulumu_ aşamasında yardımcı olmak üzere tasarlanmıştır. Bu kurulum genellikle bir dizi kaynak grubu, ilke, rol ataması ve Resource Manager şablon dağıtımını içerir. Şema, tüm bu _yapıt_ türlerini bir araya getirerek CI/CD işlem hattı dahil olmak üzere oluşturmanızı ve sürüm belirlemenizi sağlayan bir pakettir. Sonuç olarak her biri tek bir işlem içindeki bir aboneliğe atanır ve denetlenip izlenebilir.

Blueprints ile dağıtmak için kullanmak istediğiniz hemen her şey Resource Manager şablonu ile gerçekleştirilebilir. Ancak Resource Manager şablonu, Azure'da yerel olarak bulunan bir belge değildir, her birinin yerel ortamda veya kaynak denetiminde depolanması gerekir. Şablon bir veya daha fazla Azure kaynağının dağıtılması için kullanılır ancak bu kaynaklar dağıtıldıktan sonra kullanılan şablonla etkin bir bağlantı veya ilişki kalmaz.

Blueprints ile şema tanımı (dağıtılması _gereken şey_) ile şema ataması (dağıtılan _şey_) arasındaki ilişki korunur. Bu bağlantı dağıtımların daha iyi izlenmesini ve denetlenmesini destekler. Blueprints ile aynı şema tarafından yönetilen birden fazla aboneliğin aynı anda yükseltilmesi de mümkündür.

Resource Manager şablonu ile şema arasında seçim yapmanıza gerek yoktur. Her şema sıfır veya daha fazla Resource Manager şablonu _yapıtı_ içerebilir. Bu destek, Resource Manager şablonu kitaplığı geliştirme ve koruma çabalarının Blueprints ile yeniden kullanılabileceği anlamına gelir.

## <a name="how-its-different-from-azure-policy"></a>Azure İlkesi'nden farkı

Şema tutarlılık ve uyumluluk sağlamak için yeniden kullanılabilen Azure bulut hizmetleri, güvenlik ve tasarım uygulamasıyla ilgili belirli bir alana yönelik standart, desen ve gereksinim kümesi oluşturmak için kullanılan bir paket veya kapsayıcıdır.

[İlke](../policy/overview.md) ise yeni dağıtılan ve var olan kaynakların özelliklerine odaklanan varsayılan olarak izin verme ve açıkça reddetme sistemidir. Bir abonelik içindeki kaynakların gereksinimlere ve standartlara uygun olduğunu doğrulayarak bulut yönetimini destekler.

Şema içine bir ilkenin dahil edilmesi, şema ataması sırasında doğru desenin veya tasarımın oluşturulmasını da sağlar. Bu ilke şemanın amacıyla uyumluluk sağlamak için ortamda yalnızca onaylanan veya beklenen değişikliklerin yapılmasını garanti eder.

İlke, bir şema tanımına dahil edilen _yapıtlardan_ biri olabilir. Şemalar ayrıca ilkeler ve girişimlerle parametrelerin kullanılmasını da destekler.

## <a name="blueprint-definition"></a>Şema tanımı

Şemalar, _yapıtlardan_ meydana gelir. Şu an için aşağıdaki kaynaklar şemalarda yapıtlar olarak kullanılabilir:

|Kaynak  | Hiyerarşi seçenekleri| Açıklama  |
|---------|---------|---------|
|Kaynak Grupları     | Abonelik | Şema içindeki diğer yapıtlar tarafından kullanılacak yeni bir kaynak grubu oluşturur.  Bu yer tutucu kaynak grupları, kaynakları tam olarak istediğiniz yapıda düzenlemenizi sağlar ve dahil olan ilke ve rol ataması yapıtlarına ek olarak Azure Resource Manager şablonları için kapsam sınırlayıcı olarak görev yapar.         |
|Azure Resource Manager şablonu      | Abonelik, Kaynak Grubu | Şablonlar, karmaşık ortamlar oluşturmak için kullanılır. Örnek ortamlar: SharePoint grubu, Azure Otomasyonu Durum Yapılandırması veya Log Analytics çalışma alanı. |
|İlke Ataması     | Abonelik, Kaynak Grubu | Bir ilkenin veya girişimin, şemanın atanmış olduğu aboneliğe atanmasını sağlar. İlke veya girişimin şema kapsamında olması gerekir (şema yönetim grubunda veya altında). İlke veya girişimde parametre varsa bu parametreler şema oluşturma veya şema ataması sırasında atanır.       |
|Rol Ataması   | Abonelik, Kaynak Grubu | Kaynaklarınıza her zaman doğru kişilerin doğru şekilde erişmesini sağlamak için var olan bir kullanıcıyı veya grubu yerleşik role ekleyin. Rol atamaları aboneliğin tamamı için tanımlanabilir veya şemada bulunan belirli bir kaynak grubuna yerleştirilebilir. |

### <a name="blueprints-and-management-groups"></a>Şemalar ve yönetim grupları

Şema tanımı oluştururken şemanın kaydedileceği yeri de tanımlarsınız. Şemalar şu an için yalnızca **Katkıda bulunan** erişimine sahip olduğunuz bir [yönetim grubuna](../management-groups/overview.md) kaydedilebilir. Kaydedilen şema ilgili yönetim grubunun tüm alt öğelerine (abonelik) atanabilir.

> [!IMPORTANT]
> Herhangi bir yönetim grubuna erişiminiz yoksa veya yönetim grubu yapılandırılmamışsa şema tanımlarının listesi yüklendiğinde herhangi bir giriş gösterilmez ve **Kapsam**'a tıkladığınızda yönetim gruplarını alma uyarısının görüntülendiği bir pencere açılır. Bu sorunu çözmek için uygun erişime sahip olduğunuz bir aboneliğin [yönetim grubunun](../management-groups/overview.md) bir parçası olduğundan emin olun.

### <a name="blueprint-parameters"></a>Şema parametreleri

Şemalar bir ilkeye/girişime veya bir Azure Resource Manager şablonuna parametre iletebilir.
Bir şemaya yeni bir _yapıt_ eklendiğinde yazar her bir şema ataması için tanımlı bir değer sunar veya her şema atamasının atama sırasında değer sağlamasına izin verir. Bu esneklik şemanın tüm kullanımları için önceden tanımlanmış bir değer tanımlama veya bu kararın atama sırasında verilmesi seçeneğini sunar.

> [!NOTE]
> Şema kendi parametrelerine sahip olabilir ancak bu özellik şu an için yalnızca şemanın portal yerine REST API ile oluşturulması durumunda kullanılabilir.

Daha fazla bilgi için bkz. [şema parametreleri](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Şema yayımlama

Şema ilk oluşturulduğunda **Taslak** modunda olur. Atanmaya hazır olduğunda **Yayımlandı** durumuna alınması gerekir. Yayımlamak için bir **Sürüm** dizesi (harfler, sayılar ve kısa çizgiler, maksimum 20 karakter) ve isteğe bağlı **Değişiklik notları** tanımlanması gerekir. **Sürüm** değeri şemada daha sonra yapılacak değişikliklerin tanımlanmasını sağlar ve her sürümün ayrıca atanmasını mümkün hale getirir. Sürüm oluşturma aynı zamanda aynı şemanın farklı **Sürümlerinin** aynı aboneliğe atanabileceği anlamına da gelir. Şemada ek değişiklikler yapıldığında **Yayımlanmış** **Sürüm** ile birlikte **Yayımlanmamış değişiklikler** de var olmaya devam eder. Değişiklikler tamamlandıktan sonra güncelleştirilen şema yeni ve benzersiz bir **Sürüm** değeriyle **Yayımlanır** ve atamaya hazır hale gelir.

## <a name="blueprint-assignment"></a>Şema ataması

Bir şemanın **Yayımlanmış** tüm **Sürümleri** var olan bir aboneliğe atanabilir. Portalda varsayılan olarak en son **Yayımlanmış** olan şema **Sürümü** kullanılır. Yapıt parametreleri (veya şema parametreleri) varsa atama işlemi sırasında ilgili parametreler tanımlanır.

## <a name="permissions-in-azure-blueprints"></a>Azure Blueprints'te izinler

Şemaları kullanmak için [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ile gerekli izinlerin verilmiş olması gerekir. Şema oluşturmak için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprints/write` - Şema tanımı oluştur
- `Microsoft.Blueprint/blueprints/artifacts/write` - Şema tanımında yapıt oluştur
- `Microsoft.Blueprint/blueprints/versions/write` - Şema yayımla

Şemaları silmek için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Şema tanımları yönetim grubunda oluşturulduğundan şema tanımı izinlerinin yönetim grubu kapsamında verilmesi veya yönetim grubu kapsamında devralınması gerekir.

Bir şemayı atamak veya atamasını kaldırmak için hesabınız şu izinlere sahip olmalıdır:

- `Microsoft.Blueprint/blueprintAssignments/write` - Şema ata
- `Microsoft.Blueprint/blueprintAssignments/delete` - Şemanın atamasını kaldır

> [!NOTE]
> Şema atamaları abonelikte oluşturulduğundan şema atama ve atamasını kaldırma izinlerinin abonelik kapsamında verilmesi veya abonelik kapsamında devralınması gerekir.

Yukarıdaki izinlerin hepsine yer **sahibi** rol. **Katkıda bulunan** rol oluşturma şema ve delete şema izinleri vardır, ancak blueprint ataması izinlere sahip değil. Bu yerleşik roller güvenlik gereksinimlerinize uygun değilse [özel rol](../../role-based-access-control/custom-roles.md) oluşturabilirsiniz.

> [!NOTE]
> Azure Blueprint hizmet sorumlusu, dağıtımı etkinleştirmek için atanan abonelikte **Sahip** rolüne ihtiyaç duyar. Portalı kullanıyorsanız bu rol dağıtım için otomatik olarak verilir ve iptal edilir. REST API kullanıyorsanız bu rolün el ile verilmesi gerekir ancak dağıtım tamamlandıktan sonra iptal işlemi otomatik olarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema oluşturma - Portal](create-blueprint-portal.md)
- [Şema oluşturma - REST API](create-blueprint-rest-api.md)
