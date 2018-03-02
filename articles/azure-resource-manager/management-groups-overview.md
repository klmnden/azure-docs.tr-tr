---
title: "Azure Yönetim grupları ile kaynaklarınızı düzenleme | Microsoft Docs"
description: "Yönetim grupları ve bunları nasıl kullanacağınızı öğrenin."
author: rthorn17
manager: rithorn
editor: 
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/22/2018
ms.author: rithorn
ms.openlocfilehash: d1974ff9716d6b84a5f4b4f1795a84483d277a05
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/28/2018
---
# <a name="organize-your-resources-with-azure-management-groups"></a>Kaynaklarınızı Azure Yönetim grupları ile düzenleme 

Birden çok aboneliğiniz varsa, bunları "Yönetim grupları" olarak adlandırılan kapsayıcılarına düzenleyebilirsiniz erişim, ilke ve uyumluluk aboneliklerinizi yönetmenize yardımcı olmak için. Bu kapsayıcılar abonelikleri türüne sahip olabileceğiniz olsun büyük ölçekli Kurumsal düzeyde yönetim verin.  

Yönetim grubu özelliğini genel önizleme olarak kullanılabilir. Management'ı kullanmaya başlamak için gruplar, oturum açma [Azure portal](https://portal.azure.com) arayın ve **Yönetim grupları** içinde **tüm hizmetleri** bölümü. 

Yönetim grupları Azure İlkesi desteğini genel Önizleme ve bunu aşağıdaki hafta içinde geliyor henüz kullanılabilir değil.  

Örnek olarak, sanal makine (VM) oluşturmak için kullanılabilir bölgeleri sınırlar bir yönetim grubuna ilkelerini uygulayabilirsiniz. Bu ilke tüm Yönetim grupları, abonelikleri ve bu yönetim grubu altında kaynaklar yalnızca bu bölgede oluşturulan VM'ler vererek uygulanması.

## <a name="hierarchy-of-management-groups-and-subscriptions"></a>Yönetim grupları ve abonelikleri hiyerarşisi 

Esnek yapısını Yönetim grupları ve Abonelikleri birleşik İlkesi ve erişim yönetimi için bir hiyerarşiye kaynaklarınızı düzenleme oluşturabilirsiniz. Aşağıdaki diyagramda Yönetim grupları ve abonelikleri departmanlara göre düzenlenmiş oluşan bir örnek hiyerarşisini gösterir.    

![hiyerarşi](media/management-groups/MG_overview.png)

Departmanlara göre gruplandırılmış bir hiyerarşi oluşturarak, atayabilirsiniz [Azure rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-what-is.md) rolleri, *devral* departmanlara bu yönetim grubu altında. Yönetim gruplarını kullanarak, iş yükünü azaltabilir ve rol kez atamak yalnızca sağlayarak hata riskini azaltır. 

### <a name="important-facts-about-management-groups"></a>Yönetim grupları hakkında önemli bilgiler
- 10.000 Yönetim grupları tek bir dizin desteklenebilir. 
- Bir yönetim grubu ağaç derinliğini altı düzeylerini destekler.
    - Bu sınır, kök düzeyinde veya abonelik düzeyinde içermez.
- Her yönetim grubu yalnızca bir üst destekleyebilir.
- Her bir yönetim grubunda birden çok alt bulunabilir. 

## <a name="root-management-group-for-each-directory"></a>Her dizin için kök yönetim grubu

Her dizin "Root" Yönetim grubu olarak adlandırılan tek bir üst düzey yönetim grubu verilir. Bu kök yönetim grubunun tüm Yönetim gruplarını sağlamak için hiyerarşiye oluşturulur ve abonelikleri Katlama. Bu kök yönetim grubu genel ilkeler ve RBAC atamaları için dizin düzeyinde uygulanmasını sağlar. [Directory yöneticisinin gereken kendilerini yükseltmesine](../active-directory/role-based-access-control-tenant-admin-access.md) başlangıçta bu kök grubunun sahibi olmalıdır. Yönetici grubun sahibi olduktan sonra bunların herhangi bir RBAC rolü hiyerarşi yönetmek için diğer dizin kullanıcılara ve gruplara atayabilirsiniz.  

### <a name="important-facts-about-the-root-management-group"></a>Kök yönetim grubu hakkında önemli bilgiler
- Kök yönetim grubunun adı ve kimliği Azure Active Directory kimliği varsayılan olarak verilir. Görünen ad, Azure Portalı'ndan farklı göstermek için herhangi bir zamanda güncelleştirilebilir. 
- Tüm abonelikleri ve Yönetim grupları dizininde bir kök yönetim grubuna Katlama.  
    - Tüm öğeleri kök yönetim grubu için genel yönetim kadar dizin Katlama sağlamak için önerilir.  
    - Genel Önizleme sırasında dizini içindeki tüm abonelikleri otomatik olarak kök alt duruma getirilmez.   
    - Genel Önizleme sırasında yeni abonelikler otomatik olarak kök yönetim grubuna olarak ayarlanır değil. 
- Kök yönetim grubu taşınmış veya silinmiş, diğer yönetim gruplarına olamaz. 
  
## <a name="management-group-access"></a>Yönetim grubu erişimi

Azure Yönetim gruplarını destekleyen [Azure rol tabanlı erişim denetimi (RBAC)](../active-directory/role-based-access-control-what-is.md) tüm kaynak erişir ve rol tanımları için. Bu izinler, hiyerarşi içinde mevcut alt kaynaklara devralınır.   

While herhangi [yerleşik RBAC rolü](../active-directory/role-based-access-control-what-is.md#built-in-roles) atanabilir bir yönetim grubu için sık kullanılan dört rolleri vardır: 
- **Sahibi** temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır. 
- **Katkıda bulunan** olabilir oluşturun ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor.
- **Kaynak İlkesi katkıda bulunan** oluşturabilir ve kaynakları dizininde ilkelerini yönetebilirsiniz.     
- **Okuyucu** mevcut Azure kaynaklarını görüntüleyebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar 
Yönetim grupları hakkında daha fazla bilgi için bkz: 
- [Azure kaynaklarını düzenlemek için yönetim grubu oluşturma](management-groups-create.md)
- [Değiştirme, silme veya Yönetim gruplarını yönet](management-groups-manage.md)
- [Azure PowerShell modülünü yükleyin](https://www.powershellgallery.com/packages/AzureRM.ManagementGroups/0.0.1-preview)
- [REST API Spec gözden geçirin](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/managementgroups/resource-manager/Microsoft.Management/preview/2018-01-01-preview)
- [Azure CLI uzantısını yükleyin](https://docs.microsoft.com/en-us/cli/azure/extension?view=azure-cli-latest#az_extension_list_available)

