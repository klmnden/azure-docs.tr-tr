---
title: "Erişim ve izinleri ile RBAC - Azure RBAC yönetme | Microsoft Docs"
description: "Erişim Yönetimi'nde, Azure Portal'da Azure rol tabanlı erişim denetimi ile başlayın. Dizininizde izinler atamak için rol atamalarını kullanın."
services: active-directory
documentationcenter: 
author: andredm7
manager: mtillman
ms.assetid: 8f8aadeb-45c9-4d0e-af87-f1f79373e039
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/19/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 0eaa54252885cee8f90e65f299869216ca1b2144
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="get-started-with-role-based-access-control-in-the-azure-portal"></a>Azure portalında rol tabanlı erişim denetimi ile çalışmaya başlama
Güvenlik odaklı şirketler çalışanlar gereksinim duydukları izinleri tam vermiş odaklanmanız gerekir. Çok fazla izinler saldırganlar bir hesaba getirebilir. Çok az izinleri anlamına gelir çalışanlar verimli bir şekilde işlerini alınamıyor. Azure rol tabanlı erişim denetimi (RBAC), Azure için ayrıntılı erişim yönetimi sunarak bu sorunu gidermeye yardımcı olur.

RBAC kullanarak, ekibiniz içinde görevleri kurabilmeleri ve işlerini yapmak için gereksinim duydukları kullanıcılara sadece erişim miktarını verebilirsiniz. Kısıtlanmamış izinlerini herkes vermek yerine, Azure aboneliği veya kaynakları yalnızca belirli eylemleri izin verebilirsiniz. Örneğin, bir çalışan başka bir SQL veritabanları aynı abonelik içindeki yönetebilirsiniz sırada bir Abonelikteki sanal makineleri yönetme izin vermek için RBAC kullanın.

## <a name="basics-of-access-management-in-azure"></a>Azure erişim yönetimi temelleri
Her Azure aboneliği bir Azure Active Directory (AD) dizin ile ilişkilendirilir. Kullanıcılar, gruplar ve bu dizine uygulamalardan Azure aboneliğindeki kaynaklar yönetebilirsiniz. Azure portalı, Azure komut satırı araçları ve Azure Management API'leri kullanılarak bu erişim hakları atayın.

Kullanıcılar, gruplar ve belirli bir kapsamda uygulamalara uygun RBAC rolü atayarak erişim sağla. Rol atamasının kapsamı, bir abonelik, bir kaynak grubu veya tek bir kaynak olabilir. Bir üst kapsamda atanan bir rolü de içerdiği alt öğelerine erişim verir. Örneğin, bir kaynak grubu erişimi olan bir kullanıcı, Web siteleri, sanal makineler ve alt ağlar gibi içerdiği tüm kaynaklar yönetebilirsiniz.

![İlişki Azure Active Directory öğeleri arasında - diyagram](./media/role-based-access-control-what-is/rbac_aad.png)

Kullanıcı, Grup veya uygulama bu kapsamda yönetebilmeniz için hangi kaynakların atadığınız RBAC rolü belirler.

## <a name="built-in-roles"></a>Yerleşik roller
Azure RBAC tüm kaynak türleri için geçerli üç temel rol vardır:

* **Sahibi** temsilci başkalarına erişimi hakkı dahil olmak üzere tüm kaynaklara tam erişimi vardır.
* **Katkıda bulunan** olabilir oluşturun ve tüm türlerini Azure kaynaklarını yönetmek ancak başkalarına erişim izni veremiyor.
* **Okuyucu** mevcut Azure kaynaklarını görüntüleyebilirsiniz.

Azure RBAC rollerin geri kalanı belirli Azure kaynaklarının yönetimini sağlar. Örneğin, sanal makine katılımcı rolü oluşturmak ve sanal makineleri yönetmek kullanıcının sağlar. Bunları erişimi sanal ağ veya sanal makine bağlandığı alt sağlamaz. 

[RBAC yerleşik rolleri](role-based-access-built-in-roles.md) mevcut rolleri listeler. Her yerleşik rol kullanıcılara veren kapsam ve işlemleri belirtir. Daha fazla denetim için kendi rolleri tanımlamak için arıyorsanız, nasıl oluşturacağınızı öğrenin [Azure rbac'de özel roller](role-based-access-control-custom-roles.md).

## <a name="resource-hierarchy-and-access-inheritance"></a>Kaynak hiyerarşisi ve erişim devralma
* Her **abonelik** Azure içinde yalnızca bir dizine ait. (Ancak her dizin birden fazla aboneliğiniz olabilir.)
* Her **kaynak grubu** yalnızca bir aboneliğe ait.
* Her **kaynak** yalnızca bir kaynak grubuna ait.

Üst kapsamların vermek erişim alt kapsamların devralınır. Örneğin:

* Abonelik kapsamında bir Azure AD grubundaki okuyucu rolüne atayın. Bu grubun üyeleri, her kaynak grubu ve kaynak abonelikte görüntüleyebilirsiniz.
* Kaynak grubu kapsamındaki bir uygulama katılımcı rolü atar. Bu kaynak grubu, ancak diğer kaynak gruplarının değil Abonelikteki tüm türlerinin kaynakları yönetebilir.

## <a name="azure-rbac-vs-classic-subscription-administrators"></a>Azure RBAC Klasik abonelik yöneticileri karşılaştırması
[Klasik abonelik yöneticileri ve ortak yöneticileri](../billing/billing-add-change-azure-subscription-administrator.md) Azure aboneliği tam erişimi vardır. Kullanarak kaynakları yönetebilir [Azure portal](https://portal.azure.com), Azure Resource Manager API'leri ve API'ler Klasik dağıtım modeli. RBAC modelinde, Klasik Yöneticiler abonelik kapsamda sahip rolü atanır.

Yalnızca Azure portalı ve yeni Azure Resource Manager API'leri Azure RBAC destekler. Kullanıcılar ve RBAC rolleri atanmış uygulamalar kullanamaz Azure Klasik dağıtım modeli API'leri.

## <a name="authorization-for-management-vs-data-operations"></a>Veri işlemleri ve yönetimi için yetkilendirme
Azure RBAC Azure portalı ve Azure Resource Manager API'leri yalnızca Azure kaynaklarını yönetim işlemlerini destekler. Azure kaynakları için tüm veri düzeyi işlemleri yetkilendirilemiyor. Birisi depolama hesaplarını yönetmek için yetkilendirmek Örneğin, ancak BLOB veya bir depolama hesabı içindeki tabloları için. Benzer şekilde, bir SQL veritabanı, içindeki tabloları ancak yönetilebilir.

## <a name="next-steps"></a>Sonraki Adımlar
* Kullanmaya başlama [Azure portalında rol tabanlı erişim denetimi](role-based-access-control-configure.md).
* Bkz. [RBAC yerleşik rolleri](role-based-access-built-in-roles.md)
* Kendiniz için [Azure RBAC'de özel roller](role-based-access-control-custom-roles.md) tanımlama
