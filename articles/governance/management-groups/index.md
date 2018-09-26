---
title: Kaynaklarınızı Azure yönetim gruplarıyla düzenleme
description: Yönetim grupları ve nasıl kullanıldığı hakkında bilgi edinin.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 9/18/2018
ms.author: rithorn
ms.openlocfilehash: d031059f9811cedb703fec4920e00fd1b2e3f877
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "47045366"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure yönetim gruplarıyla düzenleme

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve idare koşullarınızı bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır. Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.

Örneğin, sanal makine (VM) oluşturma işlemi için kullanılabilir olan bölgeleri sınırlayan bir yönetim grubuna ilkeleri uygulayabilirsiniz. Bu ilke, yalnızca o bölgede VM oluşturulmasına izin vererek söz konusu yönetim grubu altındaki tüm yönetim gruplarına, aboneliklere ve kaynaklara uygulanır.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Birleşik ilke ve erişim yönetimi için kaynaklarınızı bir hiyerarşi altında düzenlemek amacıyla yönetim grupları ve aboneliklerden oluşan esnek bir yapı inşa edebilirsiniz. Aşağıdaki diyagramda, yönetim grupları kullanılarak idare amaçlı bir hiyerarşi oluşturma örneği gösterilmektedir.

![ağaç](./media/MG_overview.png)

Bu örnekteki bir hiyerarşi oluşturarak, örneğin şirket içi uyumluluk ve güvenlik ilkelerini etkinleştirmek amacıyla "Altyapı Takımı yönetim grubu" üzerinde Batı ABD Bölgesi ile sınırlı VM konumlarına bir ilke uygulayabilirsiniz. Bu ilke, o yönetim grubu altındaki her iki EA aboneliğine devredilir ve o abonelikler altındaki tüm VM’ler için geçerli olur. Bu ilke yönetim grubundan aboneliklere devredildiği için, bu güvenlik ilkesi kaynak ya da abonelik sahibi tarafından değiştirilemez ve daha iyi idareye olanak tanır.

Yönetim gruplarını kullanacağınız başka bir senaryo ise birden fazla aboneliğe kullanıcı erişimi sağlamaktır. Birden fazla aboneliği söz konusu yönetim grubu altına taşıyarak, yönetim grubu üzerinde tüm aboneliklere erişimi devralacak bir [rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ataması oluşturabilirsiniz.
Birden fazla abonelik üzerinde RBAC atamaları oluşturma gereksinimi olmadan, yönetim grubunda bir atama olması kullanıcıların ihtiyaç duydukları her şeye erişmesini sağlayabilir.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- Tek bir dizinde 10.000 yönetim grubu desteklenebilir.
- Bir yönetim grubu en fazla altı derinlik düzeyini destekleyebilir.
  - Bu sınır, Kök düzeyini veya abonelik düzeyini içermez.
- Her yönetim grubu ve abonelik yalnızca bir üst öğeyi destekler.
- Her yönetim grubunda birden fazla alt öğe olabilir.
- Tüm abonelikler ve yönetim grupları, her bir dizindeki tek bir hiyerarşi içinde yer alır. Önizleme sırasında özel durumlar için [Kök yönetim grubu hakkında önemli bilgiler](#important-facts-about-the-root-management-group) bölümüne bakın.

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizinde "Kök" yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu bulunur.
Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu Kök yönetim grubu, genel ilkelerin ve RBAC atamalarının dizin düzeyinde uygulanmasını sağlar. Başlangıçta bu kök grubunun sahibi olmak için [Dizin Yöneticisinin kendisini yükseltmesi gerekir](../../role-based-access-control/elevate-access-global-admin.md). Yönetici, grubun sahibi olduktan sonra hiyerarşiyi yönetmek için diğer dizin kullanıcılarına veya gruplara herhangi bir RBAC rolü atayabilir.

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubu hakkında önemli bilgiler

- Kök yönetim grubunun adı ve kimliği varsayılan olarak verilir. Görünen ad, Azure portalında farklı görünecek şekilde herhangi bir zamanda güncelleştirilebilir.
  - Ad "Kiracı kök grubu" olacaktır.
  - Kimlik Azure Active Directory Kimliği olacaktır.
- Diğer yönetim gruplarının aksine kök yönetim grubu taşınamaz veya silinemez.  
- Tüm abonelikler ve yönetim grupları, dizinin içindeki bir kök yönetim grubu altında birleşir.
  - Dizindeki tüm kaynaklar, genel yönetim için kök yönetim grubu altında birleşir.
  - Yeni abonelikler, oluşturulduğunda kök yönetim grubuna otomatik olarak eklenir.
- Tüm Azure müşterileri kök yönetim grubunu görebilir ancak tüm müşteriler o kök yönetim grubunu yönetmek için erişime sahip değildir.
  - Bir aboneliğe erişimi olan herkes bu aboneliğin hiyerarşide bulunduğu bağlamı görebilir.  
  - Kök yönetim grubuna hiç kimsenin varsayılan erişimi yoktur. Erişim kazanmak için yalnızca Dizin Genel Yöneticileri kendi rollerini yükseltebilir.  Erişim kazandıktan sonra, dizin yöneticileri yönetecek kullanıcılara herhangi bir RBAC rolü atayabilir.  

> [!NOTE]
> Dizininiz 25/06/2018'den önce yönetim gruplarını kullanmaya başladıysa, dizininiz hiyerarşideki tüm aboneliklerle ayarlanmamış olabilir. Yönetim grubu takımı, Temmuz/Ağustos 2018 içinde o tarihten önce Genel Önizlemede olan yönetim gruplarını kullanmaya başlamış her bir dizini geriye dönük olarak güncelleştirmektedir. Dizinlerdeki tüm abonelikler, önceki kök yönetim grubu altında alt öğe yapılacaktır.
>
> Geriye dönük süreç hakkında sorularınız olursa iletişim: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Yönetim gruplarının ilk ayarı

Herhangi bir kullanıcı yönetim gruplarını kullanmaya başladığında gerçekleştirilen bir ilk ayar işlemi vardır. İlk adım, dizinde kök yönetim grubunun oluşturulmasıdır. Bu grup oluşturulduktan sonra dizinde mevcut olan tüm abonelikler, kök yönetim grubunun alt öğesi yapılır. Bu işlemin yapılmasının nedeni, bir dizin içinde yalnızca bir yönetim grubu hiyerarşisi olmasını sağlamaktır. Dizin içinde tek hiyerarşi olması, yönetim müşterilerinin dizindeki diğer müşteriler tarafından atlanamayacak genel erişim ve ilkeler uygulamasına olanak tanır. Kökte atanan herhangi bir şey, dizinde tek hiyerarşi olduğunda dizin içindeki tüm yönetim gruplarına, aboneliklere, kaynak gruplarına ve kaynaklara uygulanır.

> [!IMPORTANT]
> Kök yönetim grubu grubunda yapılan herhangi bir kullanıcı erişimi ataması veya ilke ataması, **dizin içindeki tüm kaynaklara uygulanır**.
> Bu nedenle, tüm müşterilerin bu kapsamda tanımlanmış öğelere sahip olma gereksinimini değerlendirmesi gerekir.
> Kullanıcı erişimi ve ilke atamaları yalnızca bu kapsamda "Sahip Olunması Zorunlu" olmalıdır.  
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure yönetim grupları tüm kaynak erişimleri ve rol tanımları için [Azure Rol Tabanlı Erişim Denetimi’ni (RBAC)](../../role-based-access-control/overview.md) destekler.
Bu izinler, hiyerarşide mevcut olan alt kaynaklara devredilir. Herhangi bir yerleşik RBAC rolü, hiyerarşiyi kaynaklara devredecek bir yönetim grubuna atanabilir.
Örneğin, VM katılımcısı adlı RBAC rolü bir yönetim grubuna atanabilir. Bu rolün yönetim grubu üzerinde herhangi bir eylemi yoktur ancak o yönetim grubu altındaki tüm VM’lere devredilir.

Aşağıdaki grafikte rollerin listesi ve yönetim gruplarında desteklenen eylemler gösterilmektedir.

| RBAC Rol Adı             | Oluştur | Yeniden Adlandır | Taşı | Sil | Erişim Ata | İlke Ata | Okuma  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Sahip                       | X      | X      | X    | X      | X             | X             | X     |
|Katılımcı                 | X      | X      | X    | X      |               |               | X     |
|MG Katılımcısı*             | X      | X      | X    | X      |               |               | X     |
|Okuyucu                      |        |        |      |        |               |               | X     |
|MG Okuyucusu*                  |        |        |      |        |               |               | X     |
|Kaynak İlkesine Katkıda Bulunan |        |        |      |        |               | X             |       |
|Kullanıcı Erişimi Yöneticisi   |        |        |      |        | X             |               |       |

*: MG Katılımcısı ve MG Okuyucusu, kullanıcıların bu eylemleri yalnızca yönetim grubu kapsamında gerçekleştirmesine izin verir.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Özel RBAC Rol Tanımı ve Ataması

Özel RBAC rolleri şu anda yönetim gruplarında desteklenmemektedir. Bu maddenin durumunu görüntülemek için [yönetim grubu geri bildirim forumuna](https://aka.ms/mgfeedback) bakın.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups)
- [REST API Belirtimini gözden geçirme](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI uzantısını yükleme](/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)