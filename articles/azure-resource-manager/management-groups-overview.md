---
title: Azure Yönetim grupları ile kaynaklarınızı düzenleme | Microsoft Docs
description: Yönetim grupları ve bunları nasıl kullanacağınızı öğrenin.
author: rthorn17
manager: rithorn
editor: ''
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 6/28/2018
ms.author: rithorn
ms.openlocfilehash: 611faef7e4b94b1734896fb64ca29540b12bc057
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102714"
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Azure Yönetim grupları ile kaynaklarınızı düzenleme

Kuruluşunuzun fazla aboneliğiniz varsa, verimli bir şekilde erişim, ilkeleri ve bu abonelik için uyumluluğu yönetmek için bir yöntem gerekebilir. Azure Yönetim grupları abonelikleri yukarıda kapsam düzeyi sağlar. "Yönetim grupları" olarak adlandırılan kapsayıcılar içine abonelikleri düzenlemenize ve idare koşullarınızı yönetim gruplarına uygulayın. Bir yönetim grubundaki tüm abonelikleri yönetim grubuna uygulanan koşulları otomatik olarak devralır. Yönetim grupları abonelikleri türüne sahip olabileceğiniz olsun büyük ölçekli Kurumsal düzeyde yönetim verin.

Yönetim grubu özelliğini genel önizleme olarak kullanılabilir. Yönetim grupları'nı kullanmaya başlamak için oturum açın [Azure portal](https://portal.azure.com) arayın ve **Yönetim grupları** içinde **tüm hizmetleri** bölümü.

Örneğin, sanal makine (VM) oluşturmak için kullanılabilir bölgeleri sınırlar bir yönetim grubuna ilkelerini uygulayabilirsiniz. Bu ilke tüm Yönetim grupları, abonelikleri ve bu yönetim grubu altında kaynaklar yalnızca bu bölgede oluşturulan VM'ler vererek uygulanması.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikleri hiyerarşisi

Esnek yapısını Yönetim grupları ve Abonelikleri birleşik İlkesi ve erişim yönetimi için bir hiyerarşiye kaynaklarınızı düzenleme oluşturabilirsiniz.
Aşağıdaki diyagramda Yönetim grupları ve abonelikleri departmanlara göre düzenlenmiş oluşan bir örnek hiyerarşisini gösterir.

![Ağaç](media/management-groups/MG_overview.png)

Departmanlara göre gruplandırılmış bir hiyerarşi oluşturarak atayabilirsiniz [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) rolleri, *devral* departmanlara bu yönetim grubu altında. Yönetim gruplarını kullanarak, iş yükünü azaltabilir ve rol kez atamak yalnızca sağlayarak hata riskini azaltır.

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler

- 10.000 Yönetim grupları tek bir dizin desteklenebilir.
- Bir yönetim grubu ağaç derinliğini altı düzeylerini destekler.
  - Bu sınır, kök düzeyinde veya abonelik düzeyinde içermez.
- Her yönetim grubu ve abonelik yalnızca bir üst destekler.
- Her bir yönetim grubunda birden çok alt bulunabilir.
- Tüm abonelikleri ve Yönetim grupları her dizinde tek bir hiyerarşi içinde yer alır. Bkz: [kök yönetim grubu hakkında önemli bilgiler](#important-facts-about-the-root-management-group) Önizleme sırasında özel durumlar için.

### <a name="preview-subscription-visibility-limitation"></a>Önizleme abonelik görünürlüğü kısıtlama

Şu anda nerede erişimi devralmış abonelikleri görmeye kullanmadığınız önizleme içinde bir sınırlama yoktur. Erişim için abonelik devralınan ancak Azure Resource Manager devralma erişim henüz dikkate mümkün değil.  

Abonelik hakkında bilgi almak için REST API kullanarak erişebilirsiniz, ancak Azure portalı ve Azure Powershell içinde abonelikleri gösterme ayrıntılarını döndürür.

Bu öğe üzerinde çalışılan ve Yönetim grupları "Genel kullanılabilirlik" duyurulur önce çözülmüş  

### <a name="cloud-solution-provider-csp-limitation-during-preview"></a>Bulut çözümü sağlayıcısı (CSP) sınırlaması Önizleme sırasında

Burada oluşturmak veya, müşterinin Yönetim grupları, müşterinin dizini içindeki yönetmek mümkün olmayan bulut çözümü sağlayıcısı (CSP) iş ortakları için geçerli bir sınırlama yoktur.  
Bu öğe üzerinde çalışılan ve Yönetim grupları "Genel kullanılabilirlik" duyurulur önce çözülmüş

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizin "Root" Yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu verilir. Bu kök yönetim grubunun tüm Yönetim gruplarını sağlamak için hiyerarşiye oluşturulur ve abonelikleri Katlama. Bu kök yönetim grubu genel ilkeler ve RBAC atamaları için dizin düzeyinde uygulanmasını sağlar. [Directory yöneticisinin gereken kendilerini yükseltmesine](../role-based-access-control/elevate-access-global-admin.md) başlangıçta bu kök grubunun sahibi olmalıdır. Yönetici grubun sahibi olduktan sonra bunların herhangi bir RBAC rolü hiyerarşi yönetmek için diğer dizin kullanıcılara ve gruplara atayabilirsiniz.  

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubu hakkında önemli bilgiler

- Varsayılan olarak, kök yönetim grubunun adı ve kimliği verilir. Görünen ad, Azure Portalı'ndan farklı göstermek için herhangi bir zamanda güncelleştirilebilir.
  - Adı "Kiracı kök grubu" olacaktır.
  - Kimliği Azure Active Directory kimliği olacaktır
- Kök yönetim grubu taşınmış veya silinmiş, diğer yönetim gruplarına olamaz.  
- Tüm abonelikleri ve Yönetim grupları dizininde bir kök yönetim grubu Katlama.
  - Kök yönetim grubu için genel yönetim kadar dizin Katlama tüm kaynakları.
  - Yeni Abonelik oluşturduğunuzda kök yönetim grubuna otomatik olarak ayarlanır.
- Tüm Azure müşterilerine kök yönetim grubunu görebilirsiniz, ancak tüm müşteriler, bu kök yönetim grubu yönetmek için erişimi.
  - Bir abonelik erişimi olan herkes bu abonelik hiyerarşi içinde olduğu bağlamı görebilirsiniz.  
  - Kök yönetim grubuna hiç kimse varsayılan erişim verilir. Dizin genel Yöneticiler erişim kazanmak için kendilerini yükseltebilir yalnızca kullanıcılardır.  Dizin yöneticileri erişimleri sonra herhangi bir RBAC rolü yönetmek için diğer kullanıcılara atayabilirsiniz.  

>[!NOTE]
>Dizininizi 25/6/2018 önce yönetim grupları hizmeti kullanmaya başladıysanız, dizininize hiyerarşideki tüm abonelikleri ile kurulmamış. Yönetim grubu ekibi firmalarda geriye dönük Temmuz 2018 içinde bu tarihten önce genel önizlemede Yönetim grupları kullanılarak başlatılan her dizin güncelleştiriliyor. Tüm abonelikleri dizinlerde kök yönetim grubu altında alt önceki yapılacaktır.  
>
>Geriye dönük bu işlem hakkında sorularınız varsa, başvurun: managementgroups@microsoft.com  
  
## <a name="initial-setup-of-management-groups"></a>İlk kurulum, Yönetim grupları

Herhangi bir kullanıcı yönetim gruplarını kullanarak başlatıldığında gerçekleşir bir ilk kurulum işlemi yoktur. Kök yönetim grubu dizinde oluşturulur ilk adımdır. Bu grup oluşturulduktan sonra dizinde kayıtlı tüm mevcut abonelikleri kök yönetim grubunun alt yapılır.  Bu işlem için yalnızca bir yönetim grubu hiyerarşisi bir dizin içinde olduğundan emin olmak için nedenidir.  Tek bir hiyerarşi dizini içindeki genel erişim ve dizin içindeki diğer müşteriler atlayamazsınız ilkeleri uygulamak yönetim müşterilerin olanak tanır. Herhangi bir şey kök atanan tüm Yönetim grupları, abonelikler, kaynak grupları ve dizin içindeki kaynaklar arasında bir hiyerarşi dizini içindeki sağlayarak uygulanır.  

> [!IMPORTANT]
> Kullanıcı erişim veya ilke atama kök yönetim grubundaki herhangi bir atama **dizini içindeki tüm kaynaklara uygulandığı**. Bu nedenle, bu kapsamda tanımlanan öğeleriniz için gereken tüm müşteriler değerlendirmelisiniz.  Kullanıcı erişim ve ilke atamaları "Olması gerekir" yalnızca bu kapsamda olmalıdır.  
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure Yönetim grupları Destek [Azure rol tabanlı erişim denetimi (RBAC)](../role-based-access-control/overview.md) tüm kaynak erişir ve rol tanımları için. Bu izinler, hiyerarşi içinde mevcut alt kaynaklara devralınır. Herhangi bir yerleşik RBAC rolü kaynaklara hiyerarşide aşağı devralır bir yönetim grubuna atanabilir.  Örneğin, RBAC rolü VM katkıda bulunan bir yönetim grubuna atanabilir. Bu rol yönetim grubu üzerinde herhangi bir eylemi var, ancak bu yönetim grubundaki tüm sanal makineleri için devralır.  

Aşağıdaki grafikte yönetim gruplarında rolleri ve desteklenen eylemlerin listesini gösterir.

| RBAC rolü adı             | Oluştur | Yeniden adlandır | Taşı | Sil | Erişimi atayın | İlke Ata | Okuma  |
|:-------------------------- |:------:|:------:|:----:|:------:|:-------------:| :------------:|:-----:|
|Sahip                       | X      | X      | X    | X      | X             |               | X     |
|Katılımcı                 | X      | X      | X    | X      |               |               | X     |
|Okuyucu                      |        |        |      |        |               |               | X     |
|Kaynak İlkesine Katkıda Bulunan |        |        |      |        |               | X             |       |
|Kullanıcı Erişimi Yöneticisi   |        |        |      |        | X             |               |       |

### <a name="custom-rbac-role-definition-and-assignment"></a>Özel RBAC rol tanımı ve atama

Özel RBAC rolleri yönetim gruplarında şu anda desteklenmez.  Bkz: [yönetim grubu geri bildirim Forumunda](https://aka.ms/mgfeedback) bu öğenin durumunu görüntülemek için.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim grupları hakkında daha fazla bilgi için bkz:

- [Azure kaynaklarını düzenlemek için yönetim grubu oluşturma](management-groups-create.md)
- [Değiştirme, silme veya Yönetim gruplarını yönet](management-groups-manage.md)
- [Azure PowerShell modülünü yükleyin](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Spec gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Azure CLI uzantısını yükleyin](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)
