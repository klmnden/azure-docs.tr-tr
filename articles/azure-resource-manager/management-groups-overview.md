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
ms.date: 7/09/2018
ms.author: rithorn
ms.openlocfilehash: c8152a6c12c776806d9a17c5e434d825d6c91165
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38466652"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure Yönetim grupları ile düzenleme

Kuruluşunuzda birden fazla abonelik varsa bu abonelikler için verimli bir şekilde erişim, ilke ve uyumluluk yönetimi gerçekleştirmek isteyebilirsiniz. Azure Yönetim grupları abonelikleri yukarıda kapsam düzeyini sağlar. "Yönetim grupları" adlı kapsayıcılarına abonelikleri düzenlemenize ve yönetim gruplarına, idare koşullar geçerlidir. Bir yönetim grubundaki tüm abonelikler otomatik olarak yönetim grubu için geçerli olan koşullar devralır. Yönetim grupları, size abonelik türüne sahip olabileceğiniz ne olursa olsun büyük ölçekli Kurumsal düzeyde Yönetimi sunar.

Yönetim grubu özellik genel önizlemede kullanılabilir. Yönetim gruplarını kullanmaya başlamak için oturum açın [Azure portalında](https://portal.azure.com) araması **Yönetim grupları** içinde **tüm hizmetleri** bölümü.

Örneğin, sanal makine (VM) oluşturma işlemi için kullanılabilir bölgeleri sınırlayan bir yönetim grubu için ilkeler uygulayabilirsiniz. Bu ilke tüm Yönetim gruplarını, abonelikleri ve bu yönetim grubu kapsamındaki kaynaklar için yalnızca bu bölgede oluşturulacak Vm'leri vererek uygulanacak.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikler hiyerarşisi

Yönetim gruplarında ve Aboneliklerde birleşik İlkesi ve erişim yönetimi için bir hiyerarşiye kaynaklarınızı düzenlemek için esnek bir yapısını oluşturabilirsiniz.
Aşağıdaki diyagramda yönetim gruplarında ve Aboneliklerde bölümlere göre düzenlenmiştir oluşan bir örnek hiyerarşisini gösterir.

![Ağaç](media/management-groups/MG_overview.png)

Departmanlara göre gruplandırılmış bir hiyerarşisini oluşturarak atamak için [Azure rol tabanlı Access Control (RBAC)](../role-based-access-control/overview.md) rolleri, *devral* departmanlara bu yönetim grubu altında. Yönetim grupları kullanarak iş yükünüzü azaltmak ve rol atamak yalnızca sağlayarak hata riskini azaltır.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- 10.000 Yönetim grupları, tek bir dizinde desteklenebilir.
- Bir yönetim grubu Ağaç derinliği en fazla altı düzeyde destekleyebilir.
  - Bu sınır, kök düzeyinde veya abonelik düzeyinde içermez.
- Her bir yönetim grubu ve abonelik yalnızca bir üst destekler.
- Her bir yönetim grubunda birden fazla alt olabilir.
- Tüm abonelikler ve Yönetim grupları, her dizinde tek bir hiyerarşisi içinde yer alır. Bkz: [kök yönetim grubu hakkında önemli bilgiler](#important-facts-about-the-root-management-group) Önizleme sırasında özel durumlar için.

### <a name="preview-subscription-visibility-limitation"></a>Önizleme abonelik görünürlüğü kısıtlama

Şu anda burada erişim devralmış abonelikleri görmek mümkün olmayan önizleme içinde ilgili bir sınırlama yoktur. Aboneliğe erişim devralınır ancak Azure Resource Manager devralma erişim henüz dikkate mümkün değil.  

Abonelik hakkında bilgi almak için REST API kullanarak erişiminiz, ancak aboneliklerin Azure Powershell ve Azure portalı içinde gösterme ayrıntılarını döndürür.

Bu öğe üzerinde çalışılan ve Yönetim grupları "Genel kullanılabilirlik" duyurulur önce çözümlenir  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>Bulut çözümü sağlayıcısı (CSP) sınırlama Önizleme süresince

Burada oluşturmak veya yönetmek, müşterinin Yönetim grupları, müşterinin dizininden mümkün olmayan bir bulut çözümü sağlayıcısı (CSP) iş ortakları için geçerli bir sınırlama yoktur.  
Bu öğe üzerinde çalışılan ve Yönetim grupları "Genel kullanılabilirlik" duyurulur önce çözümlenir

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
>6/25/2018'den önce yönetim grupları hizmetini kullanarak dizininize başlattıysanız, dizininize hiyerarşideki tüm abonelikleri ile ayarlanmamış. Yönetim grubu ekibi, Temmuz 2018 içinde bu tarihten önce genel önizlemede Yönetim grupları kullanılarak başlatılan her dizini geriye dönük olarak güncelleştiriliyor. Tüm abonelikleri dizinlerde kök yönetim grubu altında alt önceki sunulacaktır.  
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
|Sahip                       | X      | X      | X    | X      | X             |               | X     |
|Katılımcı                 | X      | X      | X    | X      |               |               | X     |
|Okuyucu                      |        |        |      |        |               |               | X     |
|Kaynak İlkesine Katkıda Bulunan |        |        |      |        |               | X             |       |
|Kullanıcı Erişimi Yöneticisi   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Özel bir RBAC rolü tanımı ve atama

Özel bir RBAC rollerini yönetim gruplarında şu anda desteklenmiyor.  Bkz: [yönetim grubu geri bildirim Forumu](https://aka.ms/mgfeedback) bu öğenin durumunu görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi için bkz:

- [Azure kaynaklarını düzenlemek için Yönetim grupları oluşturma](management-groups-create.md)
- [Değiştirme, silme veya yönetim gruplarınızı yönetme](management-groups-manage.md)
- [Azure PowerShell modülünü yükleme](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Belirtimi gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview)
- [Azure CLI uzantısını yükleme](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
