---
title: Kaynaklarınızı Azure yönetim gruplarıyla düzenleme - Azure Governance
description: Yönetim grupları, izinlerinin nasıl çalıştığı ve bu grupların nasıl kullanıldığı hakkında bilgi edinin.
author: rthorn17
manager: rithorn
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2018
ms.author: rithorn
ms.topic: overview
ms.openlocfilehash: 9d606a46bd08ce3e999806bed2357968e5ffd914
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2019
ms.locfileid: "56339296"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure yönetim gruplarıyla düzenleme

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure yönetim grupları, aboneliklerin üzerinde bir kapsam düzeyi sunar. Abonelikleri "yönetim grupları" adlı kapsayıcılarla düzenler ve idare koşullarınızı bu yönetim gruplarına uygularsınız. Bir yönetim grubu içindeki aboneliklerin tümü otomatik olarak yönetim grubuna uygulanmış olan koşulları devralır. Yönetim grupları, sahip olabileceğiniz abonelik türüne bakılmaksızın kurumsal düzeyde yönetimi büyük ölçekte sunar.

Örneğin, sanal makine (VM) oluşturma işlemi için kullanılabilir olan bölgeleri sınırlayan bir yönetim grubuna ilkeleri uygulayabilirsiniz. Bu ilke, yalnızca o bölgede VM oluşturulmasına izin vererek söz konusu yönetim grubu altındaki tüm yönetim gruplarına, aboneliklere ve kaynaklara uygulanır.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Birleşik ilke ve erişim yönetimi için kaynaklarınızı bir hiyerarşi altında düzenlemek amacıyla yönetim grupları ve aboneliklerden oluşan esnek bir yapı inşa edebilirsiniz. Aşağıdaki diyagramda, yönetim grupları kullanılarak idare amaçlı bir hiyerarşi oluşturma örneği gösterilmektedir.

![ağaç](./media/tree.png)

Bir ilke uygulayabilmek, örneğin "Altyapı Ekibi yönetim grubu" grubunda VM konumlarını ABD Batı Bölgesiyle sınırlayabilmek için hiyerarşi oluşturun. Bu ilke, o yönetim grubu altındaki her iki EA aboneliğine devredilir ve o abonelikler altındaki tüm VM’ler için geçerli olur. Bu güvenlik ilkesi kaynak veya abonelik sahibi tarafından değiştirilemez ve bu da idarenin geliştirilmesine olanak tanır.

Yönetim gruplarını kullanacağınız başka bir senaryo ise birden fazla aboneliğe kullanıcı erişimi sağlamaktır. Birçok aboneliği söz konusu yönetim grubu altına taşıyarak, yönetim grubu üzerinde tüm aboneliklere erişimi devralacak bir [rol tabanlı erişim denetimi](../../role-based-access-control/overview.md) (RBAC) ataması oluşturabilirsiniz.
Yönetim grubunda bir atama olması, farklı abonelikler üzerinde RBAC komut dosyası kullanmak yerine kullanıcıların ihtiyaç duydukları her şeye erişmesini sağlayabilir.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- Tek bir dizinde 10.000 yönetim grubu desteklenebilir.
- Bir yönetim grubu en fazla altı derinlik düzeyini destekleyebilir.
  - Bu sınır, Kök düzeyini veya abonelik düzeyini içermez.
- Her yönetim grubu ve abonelik yalnızca bir üst öğeyi destekler.
- Her yönetim grubunda birçok alt öğe olabilir.
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

> [!IMPORTANT]
> Kök yönetim grubu grubunda yapılan herhangi bir kullanıcı erişimi ataması veya ilke ataması, **dizin içindeki tüm kaynaklara uygulanır**.
> Bu nedenle, tüm müşterilerin bu kapsamda tanımlanmış öğelere sahip olma gereksinimini değerlendirmesi gerekir.
> Kullanıcı erişimi ve ilke atamaları yalnızca bu kapsamda "Sahip Olunması Zorunlu" olmalıdır.  

## <a name="initial-setup-of-management-groups"></a>Yönetim gruplarının ilk ayarı

Herhangi bir kullanıcı yönetim gruplarını kullanmaya başladığında gerçekleştirilen bir ilk ayar işlemi vardır. İlk adım, dizinde kök yönetim grubunun oluşturulmasıdır. Bu grup oluşturulduktan sonra dizinde mevcut olan tüm abonelikler, kök yönetim grubunun alt öğesi yapılır. Bu işlemin yapılmasının nedeni, bir dizin içinde yalnızca bir yönetim grubu hiyerarşisi olmasını sağlamaktır. Dizin içinde tek hiyerarşi olması, yönetim müşterilerinin dizindeki diğer müşteriler tarafından atlanamayacak genel erişim ve ilkeler uygulamasına olanak tanır. Kökte atanan herhangi bir şey, dizinde tek hiyerarşi olduğunda dizin içindeki tüm yönetim gruplarına, aboneliklere, kaynak gruplarına ve kaynaklara uygulanır.

## <a name="trouble-seeing-all-subscriptions"></a>Tüm abonelikler görüntülenirken oluşan sorun

25 Haziran 2018 tarihinden önce, önizlemenin ilk aşamasında yönetim gruplarını kullanmaya başlayan birkaç dizin, tüm aboneliklerin hiyerarşiye katılmaya zorlanmamasıyla ilgili bir hatayla karşılaşabiliyordu.  Abonelikleri hiyerarşiye zorlamaya yönelik işlemler dizindeki kök yönetim grubunda bir rol veya ilke ataması yapıldıktan sonra gerçekleştiriliyordu.

### <a name="how-to-resolve-the-issue"></a>Sorunu çözme

Bu sorunu çözmek için kullanabileceğiniz iki seçenek vardır.

1. Kök yönetim grubundan tüm Rol ve İlke atamalarını kaldırma
    1. Kök yönetim grubundan bir ilke ve rol atamasını kaldırdığınızda, bir sonraki gece döngüsünde hizmet tüm abonelikleri hiyerarşiye geriye dönük olarak doldurur.  Bu işlemde, tüm kiracı aboneliklerine yanlışlıkla erişim verilmediğinden ve ilke ataması yapılmadığından emin olunur.
    1. Hizmetlerinizi etkilemeden bu işlemi yapmanın en iyi yolu, rolü veya ilke atamasını Kök yönetim grubunun bir alt düzeyine uygulamaktır. Daha sonra tüm atamaları kök kapsamından çıkarabilirsiniz.
1. Geriye dönük doldurma işlemini başlatmak için doğrudan API’yi çağırın
    1. Dizindeki herhangi bir müşteri *TenantBackfillStatusRequest* veya *StartTenantBackfillRequest* API’lerini çağırabilir. StartTenantBackfillRequest API’si çağrıldığında tüm abonelikleri hiyerarşiye taşımanın ilk kurulum işlemini başlatır. Ayrıca bu işlem, tüm yeni abonelikleri kök yönetim grubunun alt öğesi olmaya zorlama işlemini de başlatır. Bu işlem, kök düzeyde hiçbir atama değiştirilmeden yapılabilir. API'yi çağırarak, kökteki her ilke veya erişim atamasının tüm aboneliklere uygulanmasının sorun olmadığını belirtmiş olursunuz.

Bu geriye dönük doldurma işlemi hakkında sorularınız varsa şu adresten bize ulaşın: managementgroups@microsoft.com  
  
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

## <a name="audit-management-groups-using-activity-logs"></a>Etkinlik günlüklerini kullanarak yönetim gruplarını denetleme

Bu API yoluyla yönetim gruplarını izlemek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) kullanın. Şu anda PowerShell, CLI veya Azure portalını kullanarak yönetim grupları etkinliğini izlemek mümkün değildir.

1. Azure AD kiracısının kiracı yöneticisi olarak, [erişimi yükseltin](../../role-based-access-control/elevate-access-global-admin.md) ve sonra da `/providers/microsoft.insights/eventtypes/management` kapsamı üzerinden denetleyen kullanıcıya Okuyucu rolü atayın.
1. Denetleyen kullanıcı olarak, yönetim grubu etkinliklerini görmek için [Kiracı Etkinlik Günlüğü API'sini](/rest/api/monitor/tenantactivitylogs) çağırın. Tüm yönetim grubu etkinliği için **Microsoft.Management** Kaynak Sağlayıcısına göre filtrelemek istersiniz.  Örnek:

```http
GET "/providers/Microsoft.Insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '{greaterThanTimeStamp}' and eventTimestamp le '{lessThanTimestamp}' and eventChannels eq 'Operation' and resourceProvider eq 'Microsoft.Management'"
```

> [!NOTE]
> Bu API'yi komut satırından rahatça çağırmak için [ARMClient](https://github.com/projectkudu/ARMClient)'ı deneyin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi almak için bkz.:

- [Azure kaynaklarını düzenlemek için yönetim grupları oluşturma](create.md)
- [Yönetim gruplarınızı değiştirme, silme veya yönetme](manage.md)
- [Azure PowerShell Kaynak Modülünde yönetim gruplarını gözden geçirme](https://aka.ms/mgPSdocs)
- [REST API'de yönetim gruplarını gözden geçirme](https://aka.ms/mgAPIdocs)
- [Azure CLI'de yönetim gruplarını gözden geçirme](https://aka.ms/mgclidoc)
