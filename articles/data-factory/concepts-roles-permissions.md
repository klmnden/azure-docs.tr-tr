---
title: Rolleri ve izinleri için Azure Data Factory | Microsoft Docs
description: Veri fabrikaları oluşturmak ve alt kaynak ile çalışmak için gerekli izinleri ve rolleri açıklanır.
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.date: 10/08/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.openlocfilehash: 10f325f3b7c93b91180b6a170c8b7accb75eb03b
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48883780"
---
# <a name="roles-and-permissions-for-azure-data-factory"></a>Rolleri ve izinleri için Azure Data Factory

Bu makalede, Azure Data Factory kaynaklarını oluşturmak ve yönetmek için gereken rolleri ve bu rolleri tarafından verilen izinleri açıklar.

## <a name="roles-and-requirements"></a>Rolleri ve gereksinimleri

Data Factory örnekleri oluşturmak için, Azure’da oturum açarken kullandığınız kullanıcı hesabı, *katkıda bulunan*, *sahip* veya *yönetici* rollerinin üyesi ya da bir Azure aboneliğinin yöneticisi olmalıdır. Azure portalında, bir abonelikte sahip olduğunuz izinleri görüntülemek için sağ üst köşeden kullanıcı adınızı seçin ve ardından **izinleri**. Birden çok aboneliğe erişiminiz varsa uygun aboneliği seçin. 

Oluşturmak ve Data Factory'deki veri kümelerini dahil olmak üzere - alt kaynakları yönetmek için bağlı hizmetler, işlem hatlarını, tetikleyiciler ve tümleştirme çalışma zamanları - aşağıdaki gereksinimler geçerlidir:
- Azure portalında alt kaynakları oluşturup yönetmek için ait olmalısınız **Data Factory Katılımcısı** kaynak grubu düzeyinde veya üzerinde rol.
- PowerShell veya SDK'sı ile alt kaynakları oluşturup yönetmek için **katkıda bulunan** kaynak düzeyinde veya üzerinde rol yeterli.

Örnek bir role kullanıcı ekleme hakkında yönergeler için [rolleri ekleme](../billing/billing-add-change-azure-subscription-administrator.md) makalesi.

## <a name="set-up-permissions"></a>İzinleri ayarlama

Bir veri fabrikasını oluşturduktan sonra data factory ile çalışmak için diğer kullanıcılara bildirmek isteyebilirsiniz. Diğer kullanıcılara erişim vermek için yerleşik olarak eklemek zorunda **Data Factory Katılımcısı** data factory içeren kaynak grubunu rolü.

### <a name="scope-of-the-data-factory-contributor-role"></a>Data Factory Katılımcısı rol kapsamı

Üyelik **Data Factory Katılımcısı** rol kullanıcıların yapmalarına şunları sağlar:
- Oluşturma, düzenleme ve veri fabrikaları ve veri kümeleri, bağlı hizmetler, işlem hatlarını, tetikleyiciler ve tümleştirme çalışma zamanları dahil olmak üzere alt kaynakları silin.
- Resource Manager şablonları dağıtın. Resource Manager dağıtımı, Azure portalında veri fabrikası tarafından kullanılan dağıtım yöntemidir.
- Veri Fabrikası için App Insights uyarıları yönetin.
- Destek bileti oluşturun.

Bu rol hakkında daha fazla bilgi için bkz. [Data Factory Katılımcısı rolü](../role-based-access-control/built-in-roles.md#data-factory-contributor).

### <a name="resource-manager-template-deployment"></a>Resource Manager şablon dağıtımı

**Data Factory Katılımcısı** rol, kaynak grubu düzeyinde veya bu düzeyin üstünde, Resource Manager şablonlarını dağıtma kullanıcılar olanak tanır. Sonuç olarak, rolünün üyeleri, hem veri fabrikaları ve veri kümeleri, bağlı hizmetler, işlem hatlarını, tetikleyiciler ve tümleştirme çalışma zamanları dahil olmak üzere, kendi alt kaynakları dağıtmak için Resource Manager şablonlarını kullanabilirsiniz. Bu rolün üyeliğini diğer kaynaklar, ancak oluşturmalarına izin vermez.

> [!IMPORTANT]
> Resource Manager şablon dağıtımı ile **Data Factory Katılımcısı** rol izinlerinizi yükseltmesine değil. Örneğin, bir Azure sanal makine oluşturan bir şablonu dağıtmak ve sanal makineler oluşturmak için izniniz yoksa, dağıtım bir Yetkilendirme hatası ile başarısız olur.

### <a name="custom-scenarios-and-custom-roles"></a>Özel senaryoları ve özel roller

Bazı durumlarda farklı bir veri fabrikası kullanıcılar için farklı erişim düzeyleri vermeniz gerekebilir. Örneğin:
- Kullanıcılar yalnızca izinlerine belirli data factory'ye sahip olduğu bir grubun gerekebilir.
- Veya bir grup olduğu kullanıcılar yalnızca veri fabrikası (veya fabrikaları) izleyebilir ancak değiştiremez gerekebilir.

Özel roller oluşturma ve bu rollere kullanıcılar atayarak bu özel senaryoları elde edebilirsiniz. Özel roller hakkında daha fazla bilgi için bkz. [azure'da özel roller](..//role-based-access-control/custom-roles.md).

Özel rollerle elde edebileceğiniz gösteren bazı örnekler şunlardır:

- Bir kullanıcı oluşturmak, düzenlemek veya herhangi bir kaynak grubu data factory'de Azure portalından silme olanak tanır.

  Yerleşik atama **Data Factory Katılımcısı** kullanıcı için kaynak grubu düzeyinde rolü. Bir Abonelikteki herhangi bir veri fabrikası erişmesine izin vermek istiyorsanız, abonelik düzeyinde rolü atayın.

- Bir kullanıcı görünümü (okuma) sağlar ve veri fabrikası, izleme ancak düzenleyemez ya da değiştirin.

  Yerleşik atama **okuyucu** kullanıcı için veri fabrikası kaynağında rol.

- Azure portalında tek bir veri fabrikası düzenleme kullanıcı sağlar.

  Bu senaryo, iki rol atamalarını gerektirir.

  1. Yerleşik atama **katkıda bulunan** data factory düzeyde rolü.
  2. Özel bir rol izniyle oluşturun * Microsoft.Resources/deployments/**. Kaynak grubu düzeyinde kullanıcı bu özel bir rol atayın.

- Bir kullanıcı ya da Powershell'den SDK'sı, ancak Azure Portalı'nda bir veri fabrikası güncelleştirmesi olanak tanır.

  Yerleşik atama **katkıda bulunan** kullanıcı için veri fabrikası kaynağında rol. Bu rol, Azure portalında kaynakları görmek kullanıcı izin verir, ancak kullanıcı erişemez **Yayımla** ve **tümünü Yayımla** düğmeleri.

## <a name="next-steps"></a>Sonraki adımlar

- Azure - roller hakkında daha fazla bilgi [rol tanımları anlama](../role-based-access-control/role-definitions.md)

- Daha fazla bilgi edinin **Data Factory Katılımcısı** rol - [Data Factory Katılımcısı rolü](../role-based-access-control/built-in-roles.md#data-factory-contributor).