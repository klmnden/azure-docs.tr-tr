---
title: Kaynaklarınızı Azure yönetim gruplarıyla düzenleme | Microsoft Docs
description: Yönetim gruplarını ve bunların nasıl kullanıldığı hakkında bilgi edinin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 7/31/2018
ms.author: rithorn
ms.openlocfilehash: edc57d146ccb034ac3fd627386000a1953b0e558
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480331"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure Yönetim grupları ile düzenleme

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure Yönetim grupları abonelikleri yukarıda kapsam düzeyini sağlar. "Yönetim grupları" adlı kapsayıcılarına abonelikleri düzenlemenize ve yönetim gruplarına, idare koşullar geçerlidir. Bir yönetim grubundaki tüm abonelikler otomatik olarak yönetim grubu için geçerli olan koşullar devralır. Yönetim grupları, size abonelik türüne sahip olabileceğiniz ne olursa olsun büyük ölçekli Kurumsal düzeyde Yönetimi sunar.

Örneğin, sanal makine (VM) oluşturma işlemi için kullanılabilir bölgeleri sınırlayan bir yönetim grubu için ilkeler uygulayabilirsiniz. Bu ilke tüm Yönetim gruplarını, abonelikleri ve bu yönetim grubu kapsamındaki kaynaklar için yalnızca bu bölgede oluşturulacak Vm'leri vererek uygulanacak.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Yönetim gruplarında ve Aboneliklerde birleşik İlkesi ve erişim yönetimi için bir hiyerarşiye kaynaklarınızı düzenlemek için esnek bir yapısını oluşturabilirsiniz.
Aşağıdaki diyagramda, idare, Yönetim gruplarını kullanarak bir hiyerarşi oluşturma örneği gösterilmektedir.

![Ağaç](media/management-groups/MG_overview.png)

Şu örnekteki gibi bir hiyerarşisini oluşturarak bir ilke, örneğin, ABD Batı bölgesi için sınırlı grubu "altyapı takım yönetim grubunda" VM konumları etkinleştir dahili uyumluluk ve güvenlik ilkeleri uygulayabilirsiniz. Bu ilke, bu yönetim grubundaki her iki EA aboneliklerinde oturum devralır ve bu Aboneliklerdeki altındaki tüm Vm'lere uygulanır. Bu ilke için abonelikler yönetim grubundan devralır gibi bu güvenlik ilkesi izin vermek için geliştirilmiş İdaresi kaynak veya abonelik sahibi tarafından değiştirilemez.

Burada Yönetim grupları kullanmak başka bir senaryo, birden fazla aboneliğiniz için kullanıcı erişimini sağlamaktır.  Bu yönetim grubundaki birden çok abonelik taşıyarak tüm abonelikleri erişimin devralır yönetim grubunda bir RBAC ataması oluşturma özelliğine sahiptir.  Betik RBAC atamaları birden fazla aboneliğiniz üzerinden gereksinimi, yönetim grubundaki bir atama kullanıcıların ihtiyaç duydukları her şeye erişimi olmasını etkinleştirebilirsiniz.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- 10.000 Yönetim grupları, tek bir dizinde desteklenebilir.
- Bir yönetim grubu Ağaç derinliği en fazla altı düzeyde destekleyebilir.
  - Bu sınır, kök düzeyinde veya abonelik düzeyinde içermez.
- Her bir yönetim grubu ve abonelik yalnızca bir üst destekler.
- Her bir yönetim grubunda birden fazla alt olabilir.
- Tüm abonelikler ve Yönetim grupları, her dizinde tek bir hiyerarşisi içinde yer alır. Bkz: [kök yönetim grubu hakkında önemli bilgiler](#important-facts-about-the-root-management-group) Önizleme sırasında özel durumlar için.

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizin bir tek bir üst düzey yönetim grubu "Kök" Yönetim grubu adı verilir. Diğer tüm yönetim grupları ve abonelikler hiyerarşide en üstte yer alan bu kök yönetim grubunun altındadır. Bu kök yönetim grubunun genel ilkeleri ve RBAC atamaları için dizin düzeyinde uygulanmasına olanak sağlar. [Dizin Yöneticisi gereken kendilerini yükseltmesine](../role-based-access-control/elevate-access-global-admin.md) başlangıçta bu kök grubun sahibi olması. Yönetici grubun sahibi olduktan sonra bunlar herhangi bir RBAC rolü hiyerarşi yönetmek için diğer dizin kullanıcılara ve gruplara atayabilirsiniz.  

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubu hakkında önemli bilgiler

- Varsayılan olarak, kök yönetim grubunun adı ve kimliği verilir. Görünen ad, Azure portalının içinde farklı göstermek için dilediğiniz zaman güncelleştirilebilir.
  - Adı "Kiracı kök grubu" olur.
  - Kimliği Azure Active Directory kimliği olacaktır
- Kök yönetim grubu taşınmış veya silinmiş, diğer yönetim gruplarına olamaz.  
- Tüm abonelikler ve Yönetim grupları dizininden bir kök yönetim grubuna katlayın.
  - Genel Yönetim için kök yönetim grubuna dizindeki tüm kaynaklara katlayın.
  - Yeni abonelikler otomatik olarak oluşturduğunuzda kök yönetim grubuna varsayılan olarak ayarlanır.
- Tüm Azure müşterileri, kök yönetim grubu görebilirsiniz, ancak tüm müşterileri, bu kök yönetim grubunu yönetmek için erişebilir.
  - Bir aboneliğe erişimi olan herkes bu abonelik hiyerarşide olduğu bağlamı görebilirsiniz.  
  - Kök yönetim grubu için hiç varsayılan erişim verilir. Directory küresel yöneticileri kendilerini erişim elde etmek için yükseltebilir yalnızca kullanıcılardır.  Dizin yöneticileri erişime sahip oldukları sonra herhangi bir RBAC rolü yönetmek için diğer kullanıcılara atayabilirsiniz.  

>[!NOTE]
>6/25/2018'den önce yönetim grupları hizmetini kullanarak dizininize başlattıysanız, dizininize hiyerarşideki tüm abonelikleri ile ayarlanmamış. Yönetim grubu ekibi, Temmuz/Ağustos 2018 içinde bu tarihten önce genel önizlemede Yönetim grupları kullanılarak başlatılan her dizini geriye dönük olarak güncelleştiriliyor. Tüm abonelikleri dizinlerde kök yönetim grubu altında alt önceki sunulacaktır.  
>
>Geriye dönük bu işlemle ilgili sorularınız varsa şuraya başvurun: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>Yönetim grubunun ilk kurulum

Herhangi bir kullanıcı yönetim grupları kullanılarak başlatıldığında gerçekleşen bir ilk kurulum işlemi yoktur. Kök yönetim grubu dizinde oluşturulan ilk adımdır. Bu grup oluşturulduktan sonra kök yönetim grubunun alt dizinde tüm mevcut abonelikleri yapılır.  Nedeni bu işlem için yalnızca bir yönetim grubu hiyerarşisi içinde bir dizin olduğundan emin olmaktır.  Dizin içindeki tek hiyerarşi küresel erişim ve diğer müşterilerin dizininden atlayamazsınız ilkeleri uygulamak yönetim müşterilerin olanak tanır. Herhangi bir şey kökte atanan tüm Yönetim gruplarını, Abonelikleri, kaynak grupları ve dizin içindeki kaynaklar arasında bir hiyerarşi dizini içindeki sağlayarak uygulanır.  

> [!IMPORTANT]
> Kullanıcı erişim veya ilke atama kök yönetim grubundaki herhangi bir atama **dizinin içindeki tüm kaynaklar için geçerlidir**. Bu nedenle, tüm müşteriler bu kapsamda tanımlanan öğeler gerek değerlendirmelidir.  Kullanıcı erişim ve ilke atamaları "Olmalıdır" yalnızca bu kapsamda olmalıdır.  
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure Yönetim grupları desteği [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md) tüm kaynak erişir ve rol tanımları. Bu izinler, hiyerarşide alt kaynaklara devralınır. Herhangi bir yerleşik RBAC rolü kaynaklara hiyerarşide aşağı devralacak bir yönetim grubuna atanabilir.  Örneğin, bir yönetim grubuna RBAC rolü sanal makine Katılımcısı atanabilir. Bu rol, yönetim grubu üzerinde herhangi bir eylemi sahiptir, ancak bu yönetim grubundaki tüm vm'lere devralır.  

Aşağıdaki grafik yönetim gruplarında rollerin ve desteklenen eylemlerin listesini gösterir.

| RBAC rolü adı             | Oluştur | Yeniden adlandır | Taşı | Sil | Erişim atama | İlke Ata | Okuma  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Sahip                       | X      | X      | X    | X      | X             | X             | X     |
|Katılımcı                 | X      | X      | X    | X      |               |               | X     |
|Yönetim grubu katkıda bulunan *             | X      | X      | X    | X      |               |               | X     |
|Okuyucu                      |        |        |      |        |               |               | X     |
|MG okuyucu *                  |        |        |      |        |               |               | X     |
|Kaynak İlkesine Katkıda Bulunan |        |        |      |        |               | X             |       |
|Kullanıcı Erişimi Yöneticisi   |        |        |      |        | X             |               |       |

*: MG katkıda bulunan ve MG okuyucu yalnızca yönetim grubu kapsamında bu eylemleri gerçekleştirme olanağı sağlar.  

### <a name="custom-rbac-role-definition-and-assignment"></a>Özel bir RBAC rolü tanımı ve atama

Özel bir RBAC rollerini yönetim gruplarında şu anda desteklenmiyor.  Bkz: [yönetim grubu geri bildirim Forumu](https://aka.ms/mgfeedback) bu öğenin durumunu görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi için bkz:

- [Azure kaynaklarını düzenlemek için Yönetim grupları oluşturma](management-groups-create.md)
- [Değiştirme, silme veya yönetim gruplarınızı yönetme](management-groups-manage.md)
- [Azure PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Belirtimi gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI uzantısını yükleme](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-list-available)
