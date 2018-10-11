---
title: Azure Blueprints’e genel bakış
description: Azure Blueprints, Azure ortamınızda yapıt oluşturmak, tanımlamak ve dağıtmak için kullanılan bir Azure hizmetidir.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: c7ffbe86407bc776870890e5b4151556f572832e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957525"
---
# <a name="what-is-azure-blueprints"></a>Azure Blueprints nedir?

Mühendislerin veya mimarların projenin ana hatlarını oluşturmak için kullandıkları şemalar gibi Azure Blueprints de bulut mimarlarının ve merkezi BT ekibinin bir kuruluşun standartlarına, desenlerine ve gereksinimlerine uygun Azure kaynaklarından oluşan tekrarlanabilir bir küme tanımlamasını sağlar. Azure Blueprints geliştirme ekiplerinin yeni ortamları hızlı bir şekilde sağlayıp kullanıma almalarını ve bunu yaparken kurumsal uyumluluk çerçevesinde olduklarından ve ağ iletişimi gibi yerleşik bileşenlere sahip olduklarından emin olmalarını sağlar.

Blueprints, aşağıdakiler gibi birden fazla kaynak şablonunu ve diğer yapıtları dağıtma sürecini yönetmenin bildirim temelli bir yoludur:

- Rol Atamaları
- İlke Atamaları
- Azure Resource Manager şablonları
- Kaynak Grupları

## <a name="how-it-is-different-from-resource-manager-templates"></a>Resource Manager şablonlarından farkı

Blueprints, _ortam kurulumu_ konusunda yardımcı olmak üzere tasarlanmıştır ve buna genellikle Resource Manager şablonu dağıtımlarına ek olarak kaynak grubu kümesi, ilkeler ve rol atamaları dahildir. Şema, tüm bu _yapıt_ türlerini bir araya getirerek CI/CD işlem hattı dahil olmak üzere oluşturmanızı ve sürüm belirlemenizi sağlayan bir pakettir. Sonuç olarak her biri tek bir işlem içindeki bir aboneliğe atanır ve denetlenip izlenebilir.

Blueprints ile dağıtmak için kullanmak istediğiniz hemen her şey Resource Manager şablonu ile gerçekleştirilebilir. Ancak Resource Manager şablonu, Azure'da yerel olarak bulunan bir belge değildir, her birinin yerel ortamda veya kaynak denetiminde depolanması gerekir. Şablon bir veya daha fazla Azure kaynağının dağıtılması için kullanılır ancak bu kaynaklar dağıtıldıktan sonra kullanılan şablonla olan bağlantı ve ilişki kaybolur.

Blueprints ile şema tanımı (dağıtılması _gereken şey_) ile şema ataması (dağıtılan _şey_) arasındaki ilişki korunur. Bu bağlantı dağıtımların daha iyi izlenmesini ve denetlenmesini, aynı şema tarafından yönetilen birden fazla aboneliğin aynı anda yükseltilmesini ve daha fazlasını mümkün kılar.

Resource Manager şablonu ile şema arasında seçim yapmanıza gerek yoktur. Her şema sıfır veya daha fazla Resource Manager şablonu _yapıtı_ içerebilir. Bu da Resource Manager şablonu kitaplığı geliştirme ve koruma çabalarının Blueprints ile sürdürülebileceği anlamına gelir.

## <a name="how-it-is-different-from-azure-policy"></a>Azure İlkesi'nden farkı

Şema tutarlılık ve uyumluluk sağlamak için yeniden kullanılabilen Azure bulut hizmetleri, güvenlik ve tasarım uygulamasıyla ilgili belirli bir alana yönelik standart, desen ve gereksinim kümesi oluşturmak için kullanılan bir paket veya kapsayıcıdır.

[İlke](../policy/overview.md) ise yeni dağıtılan ve var olan kaynakların özelliklerine odaklanan varsayılan olarak izin verme ve açıkça reddetme sistemidir. Bir abonelik içindeki kaynakların gereksinimlere ve standartlara uygun olmasını sağlayarak BT yönetimini destekler.

Şema içine bir ilkenin dahil edilmesi, şema ataması sırasında doğru desenin veya tasarımın oluşturulmasına ek olarak şemanın amacıyla uyumluluk sağlamak için ortamda yalnızca onaylanan veya beklenen değişikliklerin yapılmasını sağlar.

İlke, bir şema tanımına dahil edilen _yapıtlardan_ biri olabilir. Şemalar ayrıca ilkeler ve girişimlerle parametrelerin kullanılmasını da destekler.

## <a name="blueprint-definition"></a>Şema tanımı

Şemalar, _yapıtlardan_ meydana gelir. Şu an için aşağıdaki kaynaklar şemalarda yapıtlar olarak kullanılabilir:

|Kaynak  | Hiyerarşi seçenekleri| Açıklama  |
|---------|---------|---------|
|Kaynak Grupları     | Abonelik | Şema içindeki diğer yapıtlar tarafından kullanılacak yeni bir kaynak grubu oluşturur.  Bu yer tutucu kaynak grupları, kaynakları tam olarak istediğiniz yapıda düzenlemenizi sağlar ve dahil olan ilke ve rol ataması yapıtlarına ek olarak Azure Resource Manager şablonları için kapsam sınırlayıcı olarak görev yapar.         |
|Azure Resource Manager şablonu      | Kaynak Grubu | Bu şablonlar SharePoint grubu, Azure Otomasyonu Durum Yapılandırması veya Log Analytics çalışma alanı gibi karmaşık ortamlar oluşturmak için kullanılabilir. |
|İlke Ataması     | Abonelik, Kaynak Grubu | Bir ilkenin veya girişimin, şemanın atanmış olduğu yönetim grubuna veya aboneliğe atanmasını sağlar. İlke veya girişimin şema kapsamında olması gerekir (şema yönetim grubunda veya altında). İlke veya girişimde parametre varsa bu parametreler şema oluşturma veya şema ataması sırasında atanabilir.       |
|Rol Ataması   | Abonelik, Kaynak Grubu | Kaynaklarınıza her zaman doğru kişilerin doğru şekilde erişmesini sağlamak için var olan bir kullanıcıyı veya grubu yerleşik role ekleyin. Rol atamaları aboneliğin tamamı için tanımlanabilir veya şemada bulunan belirli bir kaynak grubuna yerleştirilebilir. |

### <a name="blueprints-and-management-groups"></a>Şemalar ve yönetim grupları

Şema tanımı oluştururken şemanın kaydedileceği yeri de tanımlarsınız. Şemalar şu an için yalnızca **Katkıda bulunan** erişimine sahip olduğunuz bir [yönetim grubuna](../management-groups/overview.md) kaydedilebilir. Kaydedilen şema ilgili yönetim grubunun tüm alt öğelerine (yönetim grubu veya abonelik) atanabilir.

> [!IMPORTANT]
> Herhangi bir yönetim grubuna erişiminiz yoksa veya yönetim grubu yapılandırılmamışsa şema tanımlarının listesi yüklendiğinde herhangi bir giriş gösterilmez ve **Kapsam**'a tıkladığınızda yönetim gruplarını alma uyarısının görüntülendiği bir pencere açılır. Bu sorunu çözmek için uygun erişime sahip olduğunuz bir aboneliğin [yönetim grubunun](../management-groups/overview.md) bir parçası olduğundan emin olun.

### <a name="blueprint-parameters"></a>Şema parametreleri

Şemalar bir ilkeye/girişime veya bir Azure Resource Manager şablonuna parametre iletebilir.
Bir şemaya yeni bir _yapıt_ eklendiğinde yazar her bir şema ataması için tanımlı bir değer sunabilir veya her şema atamasının atama sırasında değer sağlamasına izin verebilir. Bu esneklik şemanın tüm kullanımları için önceden tanımlanmış bir değer tanımlama veya bu kararın atama sırasında verilmesi seçeneğini sunar.

> [!NOTE]
> Şema kendi parametrelerine sahip olabilir ancak bu özellik şu an için yalnızca şemanın portal yerine REST API ile oluşturulması durumunda kullanılabilir.

Daha fazla bilgi için bkz. [şema parametreleri](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Şema yayımlama

Şema ilk oluşturulduğunda **Taslak** modunda olur. Atanmaya hazır olduğunda **Yayımlandı** durumuna alınması gerekir. Yayımlamak için bir **Sürüm** dizesi (harfler, sayılar ve kısa çizgiler, maksimum 20 karakter) ve isteğe bağlı **Değişiklik notları** tanımlanması gerekir.
**Sürüm** değeri şemada daha sonra yapılacak değişikliklerin tanımlanmasını sağlar ve her sürümün ayrıca atanmasını mümkün hale getirir. Bu aynı zamanda aynı şemanın farklı **Sürümlerinin** aynı aboneliğe atanabileceği anlamına da gelir. Şemada ek değişiklikler yapıldığında **Yayımlanmış** **Sürüm** var olmaya devam eder ve **Yayımlanmamış değişiklikler** de gösterilir. Değişiklikler tamamlandıktan sonra güncelleştirilen şema yeni ve benzersiz bir **Sürüm** değeriyle **Yayımlanır** ve atamaya hazır hale gelir.

## <a name="blueprint-assignment"></a>Şema ataması

Bir şemanın **Yayımlanmış** tüm **Sürümleri** var olan bir aboneliğe atanabilir. Portalda varsayılan olarak en son **Yayımlanmış** olan şema **Sürümü** kullanılır. Yapıt parametreleri (veya şema parametreleri) varsa atama işlemi sırasında ilgili parametreler tanımlanır.

## <a name="permissions-in-azure-blueprints"></a>Azure Blueprints'te izinler

Şemaları kullanmak için [Rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ile yetkilendirilmiş olmanız gerekir. Şema oluşturmak için hesabınız şu izinlere sahip olmalıdır:

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

Bu izinler **Sahip** rolüne dahil edilmiştir ve şema atama izinleri hariç olmak üzere **Katkıda bulunan** rollerinde de bulunur. Bu yerleşik roller güvenlik gereksinimlerinize uygun değilse [özel rol](../../role-based-access-control/custom-roles.md) oluşturabilirsiniz.

> [!NOTE]
> Azure Blueprint hizmet sorumlusu, dağıtımı etkinleştirmek için atanan abonelikte **Sahip** rolüne ihtiyaç duyar. Portalı kullanıyorsanız bu rol dağıtım için otomatik olarak verilir ve iptal edilir. REST API kullanıyorsanız bu rolün el ile verilmesi gerekir ancak dağıtım tamamlandıktan sonra iptal işlemi otomatik olarak gerçekleştirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Şema oluşturma - Portal](create-blueprint-portal.md)
- [Şema oluşturma - REST API](create-blueprint-rest-api.md)